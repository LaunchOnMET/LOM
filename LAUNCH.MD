 Launch On MET - Demo Preview (Nerfed Edition)Banner
(Simplified, mobile-optimized view of the token launchpad. Full app at localhost:3000.) Live Dashboard SnapshotNew Launches GridToken
Age
MCAP
Progress
Actions
 Nong
1h ago
$6.8K
█████ 40%
  
 METS
13h ago
$6.8K
████████ 80%
  
 Seeker (VISA)
7h ago
$5.4K
████ 30%
  
 gMET
1h ago
$0.09
██ 15%
  
 Glob (USDC)
2h ago
$0.09
█ 8%
  
 CHOLANA
2h ago
$0.09
███ 25%
  

(Nerfed: Shows top 6 tokens; full grid auto-loads 18+ with infinite scroll. Hover for insights!)Token Card Breakdown (Nerfed View)

┌─────────────────────────────┐
│ 🐘 Nong (NONG)  │ 1h ago    │
│ No desc provided...         │
│                             │
│ $6.8K 💵                    │
│ █████ 40% (Bonding Curve)   │
│                             │
│ 📱 [Twitter] [Site]         │
│ ⭐ 💫 ⚡                     │  ← Quick Actions
└─────────────────────────────┘

Name/Symbol: Bold, full-text (no ellipsis).  
Desc: 2-line clamp (expandable).  
MCAP: Mono-font, with  emoji.  
Curve: Animated gradient bar (purple → orange).  
Actions: Tiny icons (=Fav, =Copy, =Buy) in a tight row.

 Styling Highlights (Nerfed CSS Preview)(Extracted from index.html <style>—dark theme, glassmorphism vibes.)css

:root {
    --bg-1: rgba(18, 14, 16, 0.50);  /* Smoky panels */
    --accent: #8b5cf6;  /* Purple glow */
    --warning: #f59e0b;  /* Golden accents */
    --radius: 12px;  /* Soft corners */
}

.token-card-v2 {
    background: var(--bg-1);
    border: 1px solid var(--accent);
    border-radius: var(--radius);
    padding: 16px;
    transition: all 0.2s ease;
}

.token-card-v2:hover {
    transform: translateY(-3px);
    box-shadow: 0 10px 15px rgba(139, 92, 246, 0.3);  /* Holographic lift */
}

.action-btn {
    font-size: 12px;  /* Tiny icons */
    padding: 1px;
    background: transparent;
    border: none;
    color: var(--muted);
}

.action-btn:hover {
    color: var(--warning);  /* Golden pop */
}

(Nerfed: 50% of full styles—focus on cards/buttons. Full CSS: 800+ lines of gradients/animations.) Nerfed Features (Lightweight Mode)Launch: Form → Pinata IPFS → Meteora DBC deploy (SOL quote only).  
Buy: Jupiter modal (0.1 SOL swaps).  
Insights: Basic chart + tx list (no WebSockets).  
Quests: 3 badges (First Trade, Launch, Volume).  
No DB: In-memory (restart clears data).

Full Mode: Add LowDB, quests.json, vanity keys for persistence. Setup (Nerfed Script)Run in terminal:  bash

# Clone & Nerf
git clone <repo> && cd launch-on-met
npm i express multer dotenv @solana/web3.js

# .env (minimal)
echo "WALLET_SECRET=your_key_here" > .env
echo "PINATA_JWT=your_jwt_here" >> .env

# Start
node server.js

(Nerfed: Skips quests/master data. Full: Add configs.json + vanity/.)Built with  for Solana degens. MET your launch destiny! 
Fork & Star: GitHub

