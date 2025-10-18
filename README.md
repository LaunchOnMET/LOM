Launch On MET BannerA sleek, full-stack Solana token launchpad powered by Meteora's Dynamic Bonding Curve (DBC) SDK. Deploy meme coins, utility tokens, or experimental projects with zero platform fees—earn from trading volume shares! Built for the Solana ecosystem, integrated with Jupiter for real-time data, and featuring gamified quests, leaderboards, and token insights. Join the MET revolution: Launch fast, trade smart, earn big.![GitHub Repo stars](https://img.shields.io/github/stars/yourusername/launch-on-met?style=social)
![Solana](https://img.shields.io/badge/Solana-9945FF?style=for-the-badge&logo=solana&logoColor=white)
![Meteora](https://img.shields.io/badge/Meteora-8B5CF6?style=for-the-badge&logo=meteora&logoColor=white) Features Instant Token Launches 
 Create tokens with custom metadata (name, symbol, description, image), paired with SOL/USDC/USD1/MET. Upload to Pinata IPFS for permanent storage. Vanity keypairs for unique mints.
 Real-Time Dashboard 
 Browse new launches, trending/graduated tokens, markets table with MCAP/Volume/Holders. Top tokens flyer, bonding curve progress bars, and quick buy via Jupiter swaps.
 Gamified Quests & Bounties 
 Earn points for trading, launching, and milestones (e.g., "First Launch", "Whale Trade"). Leaderboards track top traders/deployers.
 Token Insights 
 Detailed views with price charts (Lightweight Charts), transaction history, holder stats, and comments section. Share on X/T witter or DexScreener.
 My Tokens & Portfolio 
 Track deployed tokens, claim trading fees (35% deployer share), and view PnL/win rate across holdings.
 Tokenomics & Stats 
 Platform-wide analytics: Volume, liquidity, new tokens. $LOM utility token explained with pie charts and reward scenarios.
 Secure & Efficient 
 Uses Solana mainnet (QuickNode RPC), Jupiter for quotes/swaps, and LowDB for lightweight persistence. No gas wars—confirmed txs with retries.

 Quick StartPrerequisitesNode.js 18+ 
Solana CLI (optional, for local testing) 
.env file with secrets (see below)

InstallationClone the Repo

bash

git clone https://github.com/yourusername/launch-on-met.git
cd launch-on-met

Install Dependencies

bash

npm install

Setup Environment 
 Create .env in root:

WALLET_SECRET=your_base58_wallet_secret_here
PINATA_JWT=your_pinata_jwt_here
PORT=3000

WALLET_SECRET: Base58-encoded private key for platform fees (e.g., from Solana CLI: solana-keygen new).
PINATA_JWT: From Pinata for IPFS uploads.

Prepare Assets

Add configs.json (Meteora blueprints for quote pairs). 
Place vanity keypair JSONs in ./vanity/ (generate with Solana tools). 
Upload banner.png to repo root for GitHub banner.

Run the Server

bash

node server.js

 Open http://localhost:3000 Deployment (Render/Vercel)Push to GitHub. 
On Render: Connect repo, set env vars, deploy as Node.js. 
Custom domain? Add via Render dashboard.

 ScreenshotsDashboard - New LaunchesDashboard 
(Tokens with progress bars, filters, and quick actions.)Token InsightsInsights 
(Charts, tx history, comments—powered by Jupiter/Lightweight Charts.)Create Token FormCreate 
(Upload image, set metadata, choose quote asset—deploy in seconds!)Quests & LeaderboardQuests 
Leaderboard 
(Gamify your launches and trades for rewards.)(Add your own screenshots to /screenshots/ folder for auto-embedding!) Tech StackCategory
Tech
Backend
Express.js, LowDB (JSON DB), Solana Web3.js, Meteora DBC SDK
Frontend
Vanilla JS/HTML/CSS, Chart.js (prices), Lightweight Charts (TPS/mini)
Integrations
Jupiter API (quotes/swaps/enrichment), Pinata (IPFS), WebSockets (chat/comments)
Deployment
Render (free tier), GitHub Actions (CI/CD)
Tools
dotenv, Multer (uploads), bs58 (keys), BN.js (math)

Why This Stack? Lightweight: No heavy frameworks—runs on free tiers. 
Solana-Native: DBC for fair launches, Jupiter for liquidity. 
Scalable: LowDB for MVP, easy swap to Postgres.

 Usage ExamplesLaunch a TokenConnect Phantom/Solflare. 
Fill form: Name="MyMeme", Symbol="MEME", Description="Fun token!", Image=Upload PNG. 
Select Quote (SOL/USDC), optional initial buy. 
Sign tx—token deploys + pools instantly! 
View in Dashboard: Share fees, track volume.

Trade/InsightsClick any token → Insights: See chart, txs, comment. 
Buy: Quick modal with Jupiter quote (e.g., 0.1 SOL → tokens). 
Watchlist: Heart  to sidebar for favorites.

Admin/DevFees Claim: In "My Tokens"—35% auto-share. 
Quests: Auto-awards on milestones (e.g., 5 launches = points). 
Customize: Edit masterQuests in code for new bounties.

 ContributingWe love pull requests! Fork the repo & clone locally. 
Create branch: git checkout -b feat/cool-feature. 
Commit: git commit -m "Add cool feature". 
Push: git push origin feat/cool-feature. 
Open PR—describe changes!

Guidelines: Keep it Solana/Meteora-focused. 
Test on mainnet (use devnet for experiments ). 
Update README with new features.

Issues? Open one or ping on X @junknetoracle
. DisclaimerBeta Software: For education/testing. Not financial advice—DYOR! 
Solana Risks: Network congestion, fees, rugs—trade at own risk. 
No Liability: Use as-is. We can't guarantee uptime or funds.

 LicenseMIT License © 2025 [Your Name/Team]. See LICENSE for details.Made with  for the Solana degens. Launch on MET—MET your match!  Questions? X
 | GitHub Issues 

