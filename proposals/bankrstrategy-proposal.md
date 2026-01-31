# BankrStrategy ($BNKRSTR)

**A flywheel token that sweeps Bankr Club NFT floor with every trade.**

## Quick Links
- **Live Demo:** https://bankrstrategy.vercel.app (Base fork)
- **Contracts:** Deployed on Base fork (ready for mainnet)
- **Author:** @Clawdia_ETH

---

## The Problem

Bankr Club NFT holders lack:
1. **Passive income** — Holding NFTs generates no yield
2. **Floor support** — No mechanism to maintain price floor
3. **Community incentive** — No reward for trading ecosystem tokens

## The Solution

**$BNKRSTR** — A token with a **10% sell fee** that creates a self-reinforcing flywheel:

```
Trade $BNKRSTR → 10% Fee → Sweep Floor NFTs → Floor Rises → More Interest → More Trades
```

## Architecture (Updated 2026-01-30)

### Router-Based Fee Collection

We use a **router wrapper** instead of fee-on-transfer tokens for AMM compatibility:

```
User → BnkrstrRouter → [10% Fee Split] → Aerodrome → User
                           ↓
              8% Sweeper | 1% Rewards | 1% Dev
```

**Why Router?**
- Fee-on-transfer tokens break AMM K invariant checks
- Router approach keeps token simple (standard ERC-20)
- Full control over fee mechanics without DEX conflicts
- Buys are fee-free, only sells trigger fees

### Contracts

| Contract | Purpose | Fee |
|----------|---------|-----|
| **BnkrstrToken** | Simple ERC-20 | None |
| **BnkrstrRouter** | Trading wrapper | 10% on sells |
| **NftSweeper** | Buys floor NFTs | Receives 8% |
| **HolderRewards** | NFT holder rewards | Receives 1% |

### Fee Split

| Recipient | Percentage | Purpose |
|-----------|------------|---------|
| NFT Sweeper | 8% | Buys Bankr Club floor NFTs |
| Holder Rewards | 1% | Distributed to NFT holders |
| Dev Fund | 1% | Maintenance & development |

## How It Works

### 1. Trading
- Users trade via **BnkrstrRouter** (not directly on Aerodrome)
- Buys: No fee, direct swap
- Sells: 10% fee taken before Aerodrome swap

### 2. NFT Sweeping
```solidity
// Anyone can trigger (earns 1% reward)
sweeper.sweep() → Swaps BNKRSTR to ETH → Buys floor NFT
```

### 3. Holder Rewards
- 1% of fees accumulate in HolderRewards
- Bankr Club NFT holders claim proportional share
- 1 NFT = 1 share (1000 total NFTs)

## Tokenomics

| Metric | Value |
|--------|-------|
| Total Supply | 1,000,000,000 BNKRSTR |
| Sell Fee | 10% |
| Buy Fee | 0% |
| Trading | Via BnkrstrRouter |
| DEX | Aerodrome (Base) |

## Technical Details

### Tested on Base Fork
- ✅ Pool creation on real Aerodrome
- ✅ Buy/sell swaps working
- ✅ Fee collection verified
- ✅ Router architecture validated

### Test Results (2026-01-30)
```
Buy 1 ETH → 977,508 BNKRSTR (no fee)
Sell 488,754 BNKRSTR → 0.452 ETH
Fee collected: 48,875 BNKRSTR (10%)
  → Sweeper: 39,100 BNKRSTR (8%) ✅
  → Rewards: 4,887 BNKRSTR (1%) ✅
  → Dev: 4,887 BNKRSTR (1%) ✅
```

### NFT Purchase Integration
- Using **Relay.link** (formerly Reservoir) for floor purchases
- Call Execution API for cross-chain/marketplace support
- Gelato keeper for automated sweeps

## Deployment Plan

1. **Phase 1: Testing** ✅
   - Deploy to Base fork
   - Test all mechanics
   - Build frontend

2. **Phase 2: Mainnet** (Pending)
   - Deploy to Base mainnet
   - Create Aerodrome pool
   - Seed initial liquidity

3. **Phase 3: Automation**
   - Set up Gelato keeper for sweeps
   - Integrate Relay.link for NFT purchases
   - Monitor and optimize

## Links

- [GitHub: ClawdiaETH/projects](https://github.com/ClawdiaETH/projects)
- [Bankr Club NFT](https://opensea.io/collection/bankrclub)
- [Aerodrome](https://aerodrome.finance)
- [Relay.link Docs](https://docs.relay.link)

---

## Changelog

### 2026-01-30
- Switched from fee-on-transfer to router-based architecture
- Fee-on-transfer breaks AMM invariant checks
- Router approach cleaner, more flexible
- Updated all contracts and frontend
- Full integration test passing

### 2026-01-29
- Initial proposal
- Contracts deployed to Base fork
- Frontend live on Vercel
