# Builder Track Weekly Report - Month 2 Week 1
> **Tracking progress in the CKB Academy Builder Program**

---

Name: Positive Vibes  
Week Ending: May 10, 2026  
Track: CKB Developer Builder  
Status: Month 2 Week 1 - Complete

---

## Builder Progress

```txt
Progress: 100%
Multi-Network Contract Workflow -> Backend Coordination -> Dashboard Integration
```

| Status | Focus Area | Topics |
|--------|------------|--------|
| Complete | Runtime Setup | Environment boot flow and network-aware deployment structure |
| Complete | Contract Workspace | Rust contract structure, deployment config, artifact generation |
| Complete | Backend Control Plane | GraphQL, session storage, wallet linking, helper coordination |
| Complete | Helper Service | Live balance polling, SSE sync, devnet funding execution |
| In Progress | Frontend Integration | Dashboard wiring for real build/deploy actions |

---

## Key Learnings

### Network-Aware Tooling Matters
> A large part of developer friction on CKB is not contract logic itself but repeatedly managing environment setup, funding state, deployment metadata, and RPC coordination across devnet, testnet, and eventually mainnet. Orbital reduces that friction by treating deployment workflow and network configuration as first-class parts of the product.

### Helper-to-Backend Streaming Simplifies State Sync
> Using SSE between the backend and helper process creates a clean pattern for watched wallets, funding queues, and live progress updates. This keeps the frontend lightweight while the helper stays responsible for talking to the local node.

### Contract Tooling Needs Visibility, Not Just Execution
> It is not enough to deploy a contract successfully. Developers also need to inspect structure, deployment history, code hashes, and file-level context quickly. That insight layer is what turns a script repo into a usable development workspace.

---

## Practical Progress

### Project: **Orbital**
> **A CKB development workspace for smart contract deployment across devnet, testnet, and mainnet, with wallet/session management, funding support, and contract inspection**

```txt
Configure Network -> Build/Deploy Contract -> Fund Wallets -> Inspect State -> Iterate
```

### Month 2 Week 1 Achievements

#### Network + Automation
- [x] Confirmed Orbital project configuration through `orbital.config.js`
- [x] Set up local devnet boot flow through `build/setup.js`
- [x] Added automated deployment/funding entrypoints in `auto/`
- [x] Structured deployment outputs for `devnet`, `testnet`, and `mainnet`
- [x] Persisted deployment script metadata in `deployment/scripts.json`
- [x] Generated contract deployment analysis artifacts under `deployment/devnet/`

#### Contract + Deployment Workspace
- [x] Established active Rust contract workspace in `contract/rior`
- [x] Implemented policy-based validation flow for the `rior` script
- [x] Kept a sample contract alongside the active contract for reference/testing
- [x] Connected contract discovery to runtime config loading from the backend
- [x] Preserved a configuration model that can target networks beyond devnet even though the current active setup is devnet

#### Backend + Helper Flow
- [x] Built GraphQL server for wallet creation, wallet linking, username checks, and session registration
- [x] Added SQLite persistence for generated wallets, user-wallet links, sessions, helper API keys, and contract history
- [x] Implemented helper SSE stream for watched wallets and queued devnet funding requests
- [x] Implemented helper-side balance reporting and funding progress updates
- [x] Added contract history and contract structure endpoints for frontend inspection

#### Frontend Direction
- [x] Built passkey-oriented auth flow with username checks and wallet creation/import UX
- [x] Built dashboard sections for wallets, contracts, file structure, pipeline, and activity logs
- [x] Added UI flow for helper API key handling and devnet wallet funding requests
- [x] Wired frontend reads for runtime config, wallet lists, balances, contract history, and funding progress

---

## What's Left

```txt
Remaining:
1. Replace simulated frontend build/deploy states with live backend execution
2. Connect dashboard actions directly to deployment pipeline responses
3. Expand live network handling beyond the current devnet-first flow
4. Tighten auth/session security and reduce exposure of wallet secrets
5. Add end-to-end tests across frontend, backend, helper, and deployment flow
```

| # | Goal | Priority | Status |
|---|------|----------|--------|
| 1 | Live build/deploy execution from dashboard | High | In Progress |
| 2 | End-to-end contract deployment feedback in UI | High | In Progress |
| 3 | Broaden runtime handling for testnet and mainnet workflows | High | Pending |
| 4 | Harden session/auth and wallet secret handling | High | Pending |
| 5 | Automated integration tests for helper + funding flow | Medium | Pending |
| 6 | Deeper contract metadata and analysis in dashboard | Medium | Pending |

---

## Progress Summary

| Category | Completion |
|----------|------------|
| Devnet Setup | 100% |
| Contract Workspace | 90% |
| Deployment Automation | 90% |
| Backend Control Plane | 90% |
| Helper Service | 95% |
| Frontend Dashboard | 75% |
| End-to-End Integration | 65% |

**Overall Month 2 Week 1: Orbital now has a working CKB development foundation with a network-aware deployment structure, active devnet setup, deployment automation, wallet/session management, helper-driven funding, and a dashboard for contract and wallet visibility. The current implementation is still devnet-first in practice, but the project is intended to support testnet and mainnet workflows as well.**

## Repository References

- CKBuilders Repository: https://github.com/Radiiplus/ckbuilders
- Orbital Repository: https://github.com/Radiiplus/Orbital

---
*Report generated for CKB Academy Builder Track*

