#!/bin/bash
# Launch On MET - Full Setup Script (Copy-Paste to Terminal)
# Run: bash <(curl -s https://raw.githubusercontent.com/yourusername/launch-on-met/main/setup.sh)  # Or save as setup.sh and bash setup.sh

echo "🚀 Setting up Launch On MET..."

# 1. Clone Repo (replace with your GitHub URL)
git clone https://github.com/yourusername/launch-on-met.git
cd launch-on-met

# 2. Install Dependencies
npm install

# 3. Create .env (EDIT THESE VALUES!)
cat > .env << EOF
WALLET_SECRET=your_base58_wallet_secret_here  # e.g., from solana-keygen new --outfile keypair.json; bs58 encode
PINATA_JWT=your_pinata_jwt_here  # From pinata.cloud
PORT=3000
EOF
echo "✅ .env created! Edit it with your secrets: nano .env"

# 4. Prepare Assets (manual steps - run these if files exist)
echo "📁 Add configs.json (Meteora blueprints) and vanity keypairs to ./vanity/"
echo "📁 Upload banner.png to repo root for GitHub"

# 5. Start Server
npm start  # Or: node server.js
echo "✅ Server running on http://localhost:3000"
echo "Open in browser & connect wallet to launch tokens! 🎉"
echo "For production: Deploy to Render, set env vars in dashboard."
