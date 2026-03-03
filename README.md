# Token-snatcher-contracts
Token Snatcher (Whack-to-Earn)

A decentralized arcade game where players “snatch” tokens before they disappear and earn real on-chain rewards.

This project demonstrates:

Skill-based Web3 gaming

Secure score validation

On-chain reward distribution

Transparent leaderboard

NFT-powered multipliers

📁 Project Structure
token-snatcher/
│
├── frontend/   → Next.js game client
├── backend/    → NestJS API server
├── contract/   → Soroban smart contract
│
└── README.md
🖥 FRONTEND — Next.js
📌 Overview

The frontend is responsible for:

Rendering the game UI

Handling player interactions

Connecting wallet

Starting game sessions

Submitting scores

Displaying leaderboard

It does NOT control rewards.
All rewards are validated on-chain.

🏗 Tech Stack

Next.js

TypeScript

Phaser.js (game engine)

Stellar Wallet integration (Freighter compatible)

Tailwind (optional styling)

📁 Folder Structure
frontend/
│
├── app/
│   ├── page.tsx
│   ├── leaderboard/
│   ├── ranked/
│   └── free/
│
├── components/
│   ├── GameCanvas.tsx
│   ├── WalletButton.tsx
│   ├── ScoreBoard.tsx
│   └── Leaderboard.tsx
│
├── game/
│   ├── scenes/
│   │   ├── GameScene.ts
│   │   └── ResultScene.ts
│   │
│   ├── spawnLogic.ts
│   └── scoring.ts
│
├── services/
│   ├── contract.service.ts
│   ├── wallet.service.ts
│   └── api.service.ts
│
├── utils/
│   └── constants.ts
│
└── package.json
🎮 Game Flow (Ranked Mode)

Player connects wallet

Player clicks “Start Ranked Game”

Frontend calls contract → start_session()

Contract returns session ID + randomness seed

Game starts using that seed

Player finishes round

Frontend calls → submit_score()

Smart contract validates score

Reward distributed automatically

▶ Installation
cd frontend
npm install
npm run dev

App runs at:

http://localhost:3000
🖥 BACKEND — NestJS
📌 Overview

The backend is optional but recommended.

It handles:

Leaderboard caching

Tournament scheduling

Analytics

Bot detection

Listening to blockchain events

The backend does NOT control rewards.

🏗 Tech Stack

NestJS

TypeScript

PostgreSQL

Stellar SDK

📁 Folder Structure
backend/
│
├── src/
│   ├── modules/
│   │   ├── game/
│   │   │   ├── game.controller.ts
│   │   │   └── game.service.ts
│   │   │
│   │   ├── leaderboard/
│   │   │   ├── leaderboard.controller.ts
│   │   │   └── leaderboard.service.ts
│   │   │
│   │   └── tournaments/
│   │
│   ├── blockchain/
│   │   ├── stellar.service.ts
│   │   └── contract.listener.ts
│   │
│   ├── database/
│   │   └── entities/
│   │
│   └── main.ts
│
└── package.json
🔁 Backend Responsibilities
1️⃣ Listen to Smart Contract Events

New session started

Score submitted

Reward paid

2️⃣ Cache Leaderboard

Instead of querying blockchain every time.

3️⃣ Anti-Bot Monitoring

Detect abnormal play frequency.

▶ Installation
cd backend
npm install
npm run start:dev

Runs at:

http://localhost:4000
🦀 SMART CONTRACT — Stellar (Soroban)
📌 Overview

The smart contract is the core of the system.

It handles:

Game sessions

Random seed generation

Score validation

Reward distribution

Leaderboard updates

NFT multiplier logic

All financial logic lives here.

📁 Folder Structure
contract/
│
├── src/
│   ├── lib.rs
│   ├── game.rs
│   ├── rewards.rs
│   ├── leaderboard.rs
│   ├── storage.rs
│   └── errors.rs
│
├── tests/
│   ├── test_game.rs
│   ├── test_rewards.rs
│   └── test_leaderboard.rs
│
├── Cargo.toml
└── README.md
🔑 Core Contract Functions
start_session(player: Address) -> u64

submit_score(session_id: u64, score: u32)

claim_reward(session_id: u64)

get_leaderboard() -> Vec<PlayerScore>
🔐 Anti-Cheat Mechanism

The contract:

Generates randomness seed

Stores start timestamp

Enforces time limit

Calculates max theoretical score

Rejects impossible values

Prevents duplicate submissions

Frontend scores are verified mathematically.

▶ Build Contract
cd contract
cargo build --target wasm32-unknown-unknown --release
🚀 Deploy to Testnet
soroban contract deploy \
  --wasm target/wasm32-unknown-unknown/release/token_snatcher.wasm \
  --source your-identity
🔄 How Everything Works Together
Player clicks token
        ↓
Frontend updates score
        ↓
Player submits score
        ↓
Contract verifies logic
        ↓
Reward distributed
        ↓
Backend caches event
        ↓
Leaderboard updates
