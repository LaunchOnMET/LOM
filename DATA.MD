 Launch On MET - Server.js BreakdownA stylish, annotated walkthrough of server.js—the heart of our Solana token launchpad backend. Each section includes code snippets, explanations, icons for quick scanning, and links to docs/resources. Perfect for your GitHub README or docs site. (Full file: server.js) Imports & Global SetupPurpose: Load dependencies for Express server, file uploads, Solana integration, and caching. Sets up constants like cache duration.javascript

require('dotenv').config();  // 🌍 Loads .env vars (e.g., WALLET_SECRET)
const express = require('express');  // 🚀 Web framework
const http = require('http');  // 🔌 HTTP server
const multer = require('multer');  // 📁 File uploads (images/metadata)
const fs = require('fs');  // 💾 File system ops
const path = require('path');  // 📂 Path handling
const { PinataSDK } = require('pinata');  // ☁️ IPFS uploads
const low = require('lowdb');  // 📊 Lightweight JSON DB
const FileSync = require('lowdb/adapters/FileSync');  // 🔄 DB sync
const { Connection, Keypair, PublicKey, SystemProgram, Transaction, LAMPORTS_PER_SOL } = require('@solana/web3.js');  // 🪙 Solana core
const { DynamicBondingCurveClient } = require('@meteora-ag/dynamic-bonding-curve-sdk');  // 📈 Meteora DBC
const bs58 = require('bs58');  // 🔑 Base58 encoding (Solana keys)
const BN = require('bn.js');  // 🔢 Big numbers (SOL amounts)
const { WebSocketServer } = require('ws');  // 💬 Real-time chat
const chatRooms = new Map();  // 🗺️ In-mem chat storage
const clientRooms = new Map();  // 👥 Client sessions
const jupiterCache = new Map();  // ⚡ API caching
const CACHE_DURATION_MS = 2 * 60 * 1000;  // ⏱️ 2-min cache TTL
const crypto = require('crypto');  // 🔐 Hashing (UUIDs)

Why? Modular imports keep code clean; globals like caches avoid re-fetching Jupiter data.
Links: dotenv | Pinata SDK | Solana Web3.js

 Database InitializationPurpose: LowDB for JSON persistence (tokens, trades, wallets). Defaults empty arrays.javascript

const dbPath = process.env.RENDER ? '/data/db.json' : 'db.json';  // 📍 Render-friendly path
const adapter = new FileSync(dbPath);
const db = low(adapter);
db.defaults({
    tokens: [],      // 🪙 Token metadata
    trades: [],      // 📊 Trade history
    wallets: [],     // 👛 User profiles (points/quests)
    quests: [],      // 🎯 Quest list
    comments: []     // 💬 Token comments
}).write();  // 💾 Persist defaults

Why? Simple, file-based DB—no SQL setup. Scales to ~10K records; swap for Postgres later.
Links: LowDB Docs

 Master Quests LoadingPurpose: Load quests from JSON for gamification (e.g., "First Launch" rewards points).javascript

let masterQuests = [];  // 🎮 Quest array
try {
    const questsPath = process.env.RENDER ? '/data/quests.json' : 'quests.json';
    masterQuests = JSON.parse(fs.readFileSync(questsPath, 'utf-8'));
    if (db.get('quests').value().length === 0) {  // 🔄 Seed if empty
        db.set('quests', masterQuests).write();
    }
    console.log(`✅ Loaded ${masterQuests.length} quests.`);
} catch (error) {
    console.error("❌ Quests file missing!");  // 🚨 Graceful fail
}

Why? Decouples quests from code—edit JSON for new bounties (e.g., "Whale Trade: +50 pts").
Links: Example quests.json

 Solana & Meteora SetupPurpose: Connect to mainnet, load wallet, init DBC client for token pools.javascript

const walletSecretBase58 = process.env.WALLET_SECRET;  // 🔑 Platform wallet
if (!walletSecretBase58) throw new Error("WALLET_SECRET missing!");  // 🚨 Validate
const wallet = Keypair.fromSecretKey(bs58.decode(walletSecretBase58));  // 🛡️ Private key
const connection = new Connection('https://serene-icy-mound.solana-mainnet.quiknode.pro/...', 'confirmed');  // 🌐 RPC
const client = new DynamicBondingCurveClient(connection, 'confirmed');  // 📈 DBC
const PROGRAM_ID = new PublicKey('dbcij3LWUppWqq96dh6gJWwBifmcGfLSB5D4DuSMaqN');  // 🎯 Meteora program
const PLATFORM_FEE = 0.001 * LAMPORTS_PER_SOL;  // 💰 0.001 SOL fee

const quoteMints = {  // 💱 Quote assets
    USDC: new PublicKey('EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v'),
    USD1: new PublicKey('USD1ttGY1N17NEEHLmELoaybftRBUSErhqYiQzvEmuB'),
    SOL: new PublicKey('So11111111111111111111111111111111111111112'),
    MET: new PublicKey('METwwAQPKGTdSJ8W7wTbPG1QC16w8qocqh73YMwJiYE'),
};

Why? Handles tx signing, pool creation. Fees fund ops; quotes enable multi-asset pairs.
Links: Solana Connection | Meteora DBC

 Configs & MiddlewarePurpose: Load Meteora blueprints, setup Express/static files/uploads.javascript

let configs = {};  // 🔧 Quote configs
try {
    const configsRaw = fs.readFileSync(path.join(__dirname, 'configs.json'), 'utf-8');
    const configsJson = JSON.parse(configsRaw);
    for (const key in configsJson) {  // 📍 PublicKeys
        configs[key] = new PublicKey(configsJson[key]);
    }
    console.log("✅ Loaded configs:", configs);
} catch (error) {
    console.error("❌ configs.json missing!");  // 🚨 Exit if critical
    process.exit(1);
}

app.use(express.static('public'));  // 📂 Serve HTML/CSS/JS
app.use(express.json());  // 📄 Parse JSON bodies
app.use(express.urlencoded({ extended: true }));  // 📝 Form data
const upload = multer({ dest: process.env.RENDER ? '/tmp/uploads/' : 'uploads/' });  // 📁 Temp uploads
const pinata = new PinataSDK({ pinataJwt: process.env.PINATA_JWT });  // ☁️ IPFS

Why? Static for frontend; Multer for images; Pinata for decentralized storage.
Links: Express | Multer | Pinata

 Core EndpointsPurpose: Handle token creation, signing, fee claims, and API routes.Token Creation (POST /create)javascript

app.post('/create', upload.single('image'), async (req, res) => {  // 📸 Image upload
    const { name, symbol, description, ... } = req.body;  // 📝 Form data
    // ... Vanity keypair, Pinata upload for metadata/image
    const { createPoolTx, swapBuyTx } = await client.pool.createPoolWithFirstBuy({ ... });  // 🏗️ DBC pool
    // Build/sign tx, return base64 for frontend
});

Flow: Form → IPFS metadata → DBC pool creation + optional initial buy.
Links: Meteora SDK

Confirm Creation (POST /confirm-creation)javascript

app.post('/confirm-creation', async (req, res) => {  // ✅ Post-tx validation
    // Retry-fetch tx, extract pool addr
    db.get('tokens').push({ baseMint, description, ... }).write();  // 💾 Save to DB
    // Award quests (e.g., "First Launch")
});

Why? Ensures on-chain success; seeds DB for dashboard.
Links: Solana Tx Fetch

Fee Claim (POST /claim-fees)javascript

app.post('/claim-fees', async (req, res) => {  // 💰 35% deployer share
    // Jupiter API for quote/tx build
    const txData = await fetch('https://studio-api.jup.ag/dbc/fee/create-tx', { ... });
    res.json({ transaction: txData.transaction });
});

Links: Jupiter Fee API

API Routes (e.g., /all-tokens)javascript

app.get('/all-tokens', async (req, res) => {  // 📊 Enrich with Jupiter
    const tokens = db.get('tokens').value();
    let enriched = await Promise.all(tokens.map(enrichWithJupiterData));  // 🔄 Cache Jupiter
    res.json(enriched);
});

Why? Dashboard data with prices/MCAP from Jupiter.
Links: Jupiter Assets API

 Real-Time Chat (WebSockets)Purpose: Token-specific comments via WS.javascript

const wss = new WebSocketServer({ server });  // 🔌 WS server
wss.on('connection', ws => {
    ws.on('message', data => {  // 📨 Subscribe/post
        if (data.type === 'chatMessage') {
            const newMsg = { ... };  // 💾 To in-mem map
            broadcast(token, { type: 'newMessage', message: newMsg });  // 📡 Emit
        }
    });
});

Why? Live discussions per token; cleans old msgs every 30s.
Links: WS Library

 Background EnginePurpose: Poll Jupiter for trades, award quests automatically.javascript

async function updateDataEngine() {  // ⚙️ 30s interval
    const tokens = db.get('tokens').value();
    for (const token of tokens) {
        const trades = await getLatestTradesFromApi(token.baseMint);  // 📡 Jupiter txs
        const newTrades = trades.filter(...);  // 🆕 Unique
        db.get('trades').push(...newTrades).write();
        await processNewTradesForQuests(newTrades);  // 🎯 Check milestones
    }
}
setInterval(updateDataEngine, 30 * 1000);

Why? Keeps DB fresh; auto-unlocks quests like "Sniper" (buy <30s post-launch).
Links: Jupiter Txs API

 Styling & Deployment NotesTheme: Dark glassmorphism (--bg-1: rgba(18,14,16,0.5)), purple accents (--accent: #8b5cf6).
Deploy: Render (env vars), or Vercel. Add public/banner.png for GitHub.
Next? Postgres swap, Redis cache, auth (e.g., Helius webhooks).

Full Credits: Built with  using Solana, Meteora, Jupiter. Fork & star! Last Updated: October 18, 2025

