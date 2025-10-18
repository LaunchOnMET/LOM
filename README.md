# Launch On MET 🚀

![Banner](banner.jpeg)

A sleek, full-stack Solana token launchpad powered by Meteora's Dynamic Bonding Curve (DBC) SDK. Deploy meme coins, utility tokens, or experimental projects with zero platform fees—earn from trading volume shares! Built for the Solana ecosystem, integrated with Jupiter for real-time data, and featuring gamified quests, leaderboards, and token insights. Join the MET revolution: Launch fast, trade smart, earn big.

[![GitHub Repo stars](https://img.shields.io/github/stars/LaunchOnMET/LOM?style=social)](https://github.com/LaunchOnMET/LOM)
[![Solana](https://img.shields.io/badge/Solana-9945FF?style=for-the-badge&logo=solana&logoColor=white)](https://solana.com)
[![Meteora](https://img.shields.io/badge/Meteora-8B5CF6?style=for-the-badge&logo=meteora&logoColor=white)](https://meteora.ag)

---

## 🌟 Features

- **⚡ Instant Token Launches**  
  Create tokens with custom metadata (name, symbol, description, image), paired with SOL/USDC/USD1/MET. Upload to Pinata IPFS for permanent storage. Vanity keypairs for unique mints.

- **📊 Real-Time Dashboard**  
  Browse new launches, trending/graduated tokens, markets table with MCAP/Volume/Holders. Top tokens flyer, bonding curve progress bars, and quick buy via Jupiter swaps.

- **🎮 Gamified Quests & Bounties**  
  Earn points for trading, launching, and milestones (e.g., "First Launch", "Whale Trade"). Leaderboards track top traders/deployers.

- **🔍 Token Insights**  
  Detailed views with price charts (Lightweight Charts), transaction history, holder stats, and comments section. Share on X/Twitter or DexScreener.

- **💼 My Tokens & Portfolio**  
  Track deployed tokens, claim trading fees (35% deployer share), and view PnL/win rate across holdings.

- **📈 Tokenomics & Stats**  
  Platform-wide analytics: Volume, liquidity, new tokens. $LOM utility token explained with pie charts and reward scenarios.

- **🛡️ Secure & Efficient**  
  Uses Solana mainnet (QuickNode RPC), Jupiter for quotes/swaps, and LowDB for lightweight persistence. No gas wars—confirmed txs with retries.

---

## 🚀 Quick Start

### Prerequisites
- Node.js 18+  
- Solana CLI (optional, for local testing)  
- `.env` file with secrets (see below)

### Installation
1. **Clone the Repo**  
   ```bash
   git clone https://github.com/yourusername/launch-on-met.git
   cd launch-on-met
