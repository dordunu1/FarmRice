# 🚀 RiceRise Deployment Guide

## Overview

This guide covers the deployment of RiceRise smart contracts on Somnia testnet and the setup of the frontend application.

## 📋 Prerequisites

- Node.js 18+ and npm
- Hardhat development environment
- MetaMask or compatible wallet
- STT tokens for gas fees

## 🔗 Network Configuration

### Somnia Testnet (Primary)
- **Chain ID**: 50312
- **RPC URL**: `https://dream-rpc.somnia.network`
- **WebSocket**: `wss://dream-rpc.somnia.network/ws`
- **Native Token**: STT
- **Explorer**: `https://shannon-explorer.somnia.network`

## 🏗️ Smart Contract Deployment

### 1. Environment Setup

Create environment file for Somnia network:

```bash
# .env.somnia
PRIVATE_KEY=your_private_key_here
SOMNIA_RPC_URL=https://dream-rpc.somnia.network
SOMNIA_CHAIN_ID=50312
SOMNIA_EXPLORER=https://shannon-explorer.somnia.network
```

### 2. Hardhat Configuration

```javascript
// hardhat.config.cjs
require('dotenv').config();

module.exports = {
  solidity: "0.8.19",
  networks: {
    somnia: {
      url: process.env.SOMNIA_RPC_URL,
      accounts: [process.env.PRIVATE_KEY],
      chainId: parseInt(process.env.SOMNIA_CHAIN_ID),
    },
  },
  etherscan: {
    apiKey: {
      somnia: process.env.SOMNIA_EXPLORER_API_KEY,
    },
  },
};
```

### 3. Deployment Commands

```bash
# Deploy to Somnia Testnet
npx hardhat run scripts/deploy.cjs --network somnia
```

### 4. Contract Verification

```bash
# Verify on Somnia Explorer
npx hardhat verify --network somnia DEPLOYED_CONTRACT_ADDRESS
```

## 🌐 Frontend Deployment

### 1. Environment Configuration

Update the frontend environment variables:

```bash
# .env
VITE_CURRENT_CHAIN=SOMNIA
VITE_FARMING_ADDRESS=0x65268AEbBA4ebfdFE1FcaE8f99D3A6181Ce49eFC
VITE_CHAIN_ID=50312
VITE_RPC_URL=https://dream-rpc.somnia.network
VITE_CURRENCY_SYMBOL=STT

# Somnia address
VITE_SOMNIA_FARMING_ADDRESS=0x65268AEbBA4ebfdFE1FcaE8f99D3A6181Ce49eFC
```

### 2. Build and Deploy

```bash
# Install dependencies
npm install

# Build for production
npm run build

# Deploy to hosting service (Netlify, Vercel, etc.)
npm run deploy
```

## 🔧 Backend Services Setup

### 1. Firebase Configuration

```typescript
// src/config/firebase.ts
import { initializeApp } from 'firebase/app';
import { getFirestore } from 'firebase/firestore';

const firebaseConfig = {
  apiKey: process.env.VITE_FIREBASE_API_KEY,
  authDomain: process.env.VITE_FIREBASE_AUTH_DOMAIN,
  projectId: process.env.VITE_FIREBASE_PROJECT_ID,
  storageBucket: process.env.VITE_FIREBASE_STORAGE_BUCKET,
  messagingSenderId: process.env.VITE_FIREBASE_MESSAGING_SENDER_ID,
  appId: process.env.VITE_FIREBASE_APP_ID,
};

const app = initializeApp(firebaseConfig);
export const db = getFirestore(app);
```

### 2. Supabase Setup

```typescript
// indexer/farming-marketplace-indexer.js
import { createClient } from '@supabase/supabase-js';

const supabase = createClient(
  process.env.SUPABASE_URL,
  process.env.SUPABASE_SERVICE_KEY
 );
```

### 3. Indexer Deployment

```bash
# Install dependencies
cd indexer
npm install

# Set environment variables
export SUPABASE_URL=your_supabase_url
export SUPABASE_SERVICE_KEY=your_service_key
export SOMNIA_FARMING_ADDRESS=0x65268AEbBA4ebfdFE1FcaE8f99D3A6181Ce49eFC

# Start the indexer
npm start
```

## 🚀 Railway Deployment

### 1. Railway Configuration

```toml
# railway.toml
[build]
builder = "nixpacks"

[deploy]
startCommand = "npm start"
healthcheckPath = "/health"
healthcheckTimeout = 300
restartPolicyType = "on_failure"
```

### 2. Environment Variables

Set these in Railway dashboard:

```bash
SUPABASE_URL=your_supabase_url
SUPABASE_SERVICE_KEY=your_service_key
SOMNIA_FARMING_ADDRESS=0x65268AEbBA4ebfdFE1FcaE8f99D3A6181Ce49eFC
```

## 🔍 Post-Deployment Verification

### 1. Contract Verification

- Verify contracts on Somnia explorer
- Test core functions (plant, water, harvest)
- Check event emissions

### 2. Frontend Testing

- Connect wallet to Somnia network
- Test farming mechanics
- Verify marketplace functionality
- Check strategic warning system

### 3. Backend Verification

- Confirm indexer is running
- Verify database connections
- Test real-time updates

## 🐛 Troubleshooting

### Common Issues

1. **RPC Connection Failed**
   - Check network configuration
   - Verify RPC URLs are accessible
   - Ensure chain IDs match

2. **Contract Verification Failed**
   - Verify constructor parameters
   - Check explorer API keys
   - Ensure contract bytecode matches

3. **Frontend Build Errors**
   - Check environment variables
   - Verify contract addresses
   - Clear build cache

### Debug Commands

```bash
# Check network connectivity
npx hardhat console --network somnia

# Verify contract deployment
npx hardhat verify --network somnia CONTRACT_ADDRESS

# Check contract state
npx hardhat run scripts/check-contract.cjs --network somnia
```

## 📊 Monitoring

### Health Checks

- Contract function calls
- Indexer event processing
- Database connection status
- Frontend performance metrics

### Logs

- Smart contract events
- Indexer processing logs
- Frontend error tracking
- User activity analytics

---

*This deployment guide covers the essential steps to deploy RiceRise on Somnia testnet.*
