# BankrStrategy ($BNKRSTR)

**A flywheel token that sweeps Bankr Club NFT floor with every trade.**

## Quick Links
- **Live Demo:** https://bankrstrategy.vercel.app
- **Source Code:** https://github.com/ClawdiaETH/bankrstrategy
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

## Architecture

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
| **BnkrstrToken** | Simple ERC-20 (1B supply) | None |
| **BnkrstrRouter** | Trading wrapper + fee collection | 10% on sells |
| **NftSweeper** | Accumulates fees → buys floor NFTs | Receives 8% |
| **HolderRewards** | Distributes rewards to NFT holders | Receives 1% |

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

### Test Results
```
Buy 1 ETH → 977,508 BNKRSTR (no fee)
Sell 488,754 BNKRSTR → 0.452 ETH
Fee collected: 48,875 BNKRSTR (10%)
  → Sweeper: 39,100 BNKRSTR (8%) ✅
  → Rewards: 4,887 BNKRSTR (1%) ✅
  → Dev: 4,887 BNKRSTR (1%) ✅
```

### Integrations

| Service | Status | Purpose |
|---------|--------|---------|
| Aerodrome | ✅ Tested | DEX for BNKRSTR/WETH trading |
| Gelato | ✅ Account ready | Automated sweep triggers |
| Relay.link | ⏳ Pending | NFT floor purchases |

## Deployment Plan

1. **Phase 1: Testing** ✅
   - Deploy to Base fork
   - Test all mechanics
   - Build frontend
   - Frontend redesign with new branding

2. **Phase 2: Mainnet** (Pending funding)
   - Deploy contracts to Base mainnet
   - Create Aerodrome BNKRSTR/WETH pool
   - Seed initial liquidity (~$1,500 from bounty)

3. **Phase 3: Automation**
   - Configure Gelato Web3 Function for sweeps
   - Integrate Relay.link for NFT purchases
   - Monitor and optimize

## Links

- [Source Code](https://github.com/ClawdiaETH/bankrstrategy)
- [Live Demo](https://bankrstrategy.vercel.app)
- [Bankr Club NFT](https://opensea.io/collection/bankrclub)
- [Aerodrome](https://aerodrome.finance)
- [Relay.link Docs](https://docs.relay.link)
- [Gelato](https://app.gelato.network)

---

## Changelog

### 2026-01-30 (Evening)
- Frontend redesign with dark theme + new logo
- GitHub repo created: ClawdiaETH/bankrstrategy
- Gelato account configured
- Deployed to Vercel

### 2026-01-30 (Earlier)
- Switched from fee-on-transfer to router-based architecture
- Fee-on-transfer breaks AMM invariant checks
- Router approach cleaner, more flexible
- Updated all contracts and frontend
- Full integration test passing

### 2026-01-29
- Initial proposal
- Contracts deployed to Base fork
