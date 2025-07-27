# VoidRiders: Dogfight Arena

VoidRiders is a 3D browser‑based multiplayer game built on the Monad blockchain.  Players pilot NFT‑backed spaceships in real time dogfights against one another.  This repository contains both the Solidity smart contracts and the React/Three.js frontend needed to deploy your own instance of the game.

## Repository Structure

```
voidriders/
│
├── contracts/           # Solidity smart contracts used by the game
│   ├── utils/           # Reusable utility contracts (Ownable, etc.)
│   ├── PlayerRegistry.sol
│   ├── MatchManager.sol
│   ├── VoidNFT.sol
│   └── ScoreToken.sol
│
├── frontend/            # React + Three.js client
│   ├── package.json     # Frontend dependencies and scripts
│   ├── tailwind.config.js
│   ├── postcss.config.js
│   ├── vite.config.js
│   ├── index.html       # Entry point for Vite
│   └── src/
│       ├── main.jsx     # React entry point
│       ├── App.jsx      # Top‑level component orchestrating the game
│       ├── components/  # React components (Lobby, Leaderboard, NFT viewer)
│       ├── hooks/       # Custom hooks for Wagmi, viem and game state
│       └── assets/      # Static assets (textures, UI art)
│
├── public/
│   └── models/
│       └── Spaceships/  # 3D GLTF spaceship models supplied with this project
│
└── scripts/             # Deployment scripts for contracts
    └── deploy.ts        # Example script using Viem to deploy contracts
```

## Getting Started

The project is split into two parts: smart contracts and the web client.  To work with these parts you will need Node.js (≥ 20) and Yarn or npm installed.  Foundry is used to compile and test contracts but is optional if you prefer another Solidity tool.

1. **Install dependencies**

   From the `voidriders/frontend` directory run:

   ```bash
   yarn install
   # or: npm install
   ```

2. **Configure environment**

   Create a `.env` file in the root of the repository and provide your Monad testnet RPC endpoint and deployer private key:

   ```env
   PRIVATE_KEY=
   RPC_URL=https://testnet.monadexplorer.com/
   ```

   These variables are used by the deployment scripts and the frontend wallet connectors.

3. **Deploy contracts**

   The contracts reside under `voidriders/contracts`.  You can compile and deploy them with Foundry or Hardhat.  A simple deploy script is provided in `scripts/deploy.ts` which uses `viem` to deploy contracts to your configured network.

   ```bash
   cd voidriders
   # compile and deploy using foundry (optional)
   # forge build
   # forge script scripts/Deploy.s.sol --rpc-url $RPC_URL --private-key $PRIVATE_KEY

   # deploy using our viem script
   node scripts/deploy.ts
   ```

4. **Run the frontend**

   From within `voidriders/frontend` start the development server:

   ```bash
   yarn dev
   # or: npm run dev
   ```

   Open `http://localhost:5173` (the default Vite port) in your browser to view the lobby.  Connect your wallet via RainbowKit, enter a match and start dogfighting!

## 3D Models

The spaceship GLTF models are included in `public/models/Spaceships`.  They are loaded at runtime using `@react-three/drei`’s `useGLTF` hook.  The original ZIP archive supplied with this task has been extracted into this directory; there is no need to download anything else.

## Contracts Overview

### PlayerRegistry

Tracks which addresses have registered to play the game.  Players must call `registerPlayer` before joining a match.

### MatchManager

Creates and tracks individual matches.  Records kills, determines winners, and emits events that the frontend can listen to via Wagmi.

### VoidNFT

An ERC‑721 contract that mints spaceship NFTs.  Each token represents a unique ship that can be selected in the game.  Metadata URIs can be customized per token.

### ScoreToken (optional)

An ERC‑20 token used for leaderboards or rewards.  Minting is restricted to the contract owner; integration with the game is optional.

## Disclaimer

This is a work in progress.  The smart contracts provided here are intentionally simplified for demonstration purposes and have not been audited.  Use them at your own risk in production environments.
