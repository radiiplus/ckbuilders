---

# Builder Track Weekly Report — Month 3 Week 2
> Tracking progress in the CKB Academy Builder Program

---

**Name:** Positive Vibes  
**Week Ending:** June 14, 2026  
**Track:** CKB Developer Builder  
**Status:** Month 3 Week 2 — Complete

---

## Executive Summary

This week focused on finalizing the decoupled architecture and achieving a fully functional end-to-end workflow. The primary deliverables were completing the unsigned deployment flow, implementing real-time project structure synchronization, and fully integrating the Orbital Frontend with the new backend. Orbkit now reliably executes builds, funding, and deployment preparation, while the frontend handles passkey signing and visualizes NDJSON streams. The system is now stable, secure, and ready for iterative CKB smart contract development.

---

## Builder Progress

```
Focus: Deployment Flow → Project Sync → Frontend Integration
```

| Status | Focus Area | Outcome |
|--------|------------|---------|
| Complete | Unsigned Deployment Flow | Orbkit prepares transactions, frontend signs via passkey, backend broadcasts to network |
| Complete | Project Structure Sync | Manual and live file watching implemented; structure streamed via NDJSON and GraphQL |
| Complete | Frontend Integration | https://orbital10.web.app/ fully connected to backend; UI wired to all NDJSON streams |
| Complete | Build & Funding Execution | Orbkit reliably compiles RISC-V contracts and executes devnet funding with live logs |
| Complete | End-to-End Validation | Full cycle (Init → Start Orbkit → Build → Deploy → Fund) verified on local devnet |

---

## Key Learnings

### Split-Signing Workflows Improve Security
> By separating transaction preparation (Orbkit) from signing (Frontend Passkey) and broadcasting (Backend), we maintain a strict security boundary. Orbkit handles the heavy lifting of cell dependency resolution and binary sizing without needing access to the user's private keys. This ensures that even if the backend is compromised, user assets remain secure.

### Live Project Structure Sync Requires Debouncing
> Watching local file changes and streaming the project structure to the UI caused performance spikes during rapid saves. Implementing debouncing and differential snapshots (sending only changed file stats and entrypoints) reduced backend load and prevented UI lag. The `liveSyncEnabled` toggle gives developers control over this resource usage.

### Frontend State Management with NDJSON Streams
> Consuming continuous NDJSON streams in the React frontend required a robust state machine. Buffering events and updating the UI in batches prevents rendering thrashing during verbose Rust compilation logs. Mapping NDJSON phases (`queued` → `building` → `completed`) to visual progress indicators provides immediate, actionable feedback to the developer.

---

## Practical Progress

### Project: Orbital & Orbkit
> A decoupled CKB development environment where Orbkit acts as the local runtime bridge, and the Orbital Frontend/Backend handles orchestration and state.

```
Workflow: Init Workspace → Start Orbkit → Build → Deploy → Fund → Observe
```

#### Deployment & Execution Flow
- [x] Implemented `/contracts/deploy` endpoint: Orbkit prepares unsigned deploy transaction
- [x] Implemented `/contracts/deploy/broadcast`: Backend broadcasts frontend-signed transaction
- [x] Implemented `/contracts/deploy/simulate`: Calculates binary size and estimated deploy fees
- [x] Finalized Orbkit build execution with RISC-V target validation and artifact caching
- [x] Finalized Orbkit devnet funding with retry logic and balance synchronization

#### Project Structure Synchronization
- [x] Implemented `/projects/structure/sync` for manual on-demand analysis
- [x] Implemented `/projects/structure/live` to toggle file watching per contract path
- [x] Created `/projects/structure/stream` NDJSON endpoint for real-time structure updates
- [x] Integrated GraphQL `projectStructureEvents` subscription for frontend reactivity
- [x] Analyzed project structure includes manifest, entrypoints, per-file stats, and shared functions

#### Frontend Integration (https://orbital10.web.app/)
- [x] Connected frontend to the new backend API and GraphQL subscriptions
- [x] Wired UI build/deploy/fund actions to trigger backend NDJSON streams
- [x] Implemented real-time log viewers for compilation and funding progress
- [x] Integrated passkey signing flow for deployment transactions
- [x] Built project structure explorer UI with live-sync toggle and file metrics

#### Backend & Orbkit Stability
- [x] Added graceful shutdown handling to Orbkit to prevent orphaned devnet processes
- [x] Implemented automatic reconnection logic if Orbkit loses backend connectivity
- [x] Hardened session refresh flow to handle expired tokens transparently
- [x] Validated end-to-end workflow across fresh devnet instances

---

## Blockers & Resolutions

| Blocker | Impact | Resolution Path |
|---------|--------|-----------------|
| Passkey signing payload mismatch for CKB | Frontend signatures were rejected by the CKB node during broadcast | Aligned the unsigned transaction hashing in Orbkit with Lumos SDK expectations; passkey now signs the correct message digest |
| Live sync performance degradation | Rapid file changes caused backend CPU spikes and UI freezing | Implemented 500ms debouncing on file watchers and switched to differential state updates |
| NDJSON stream disconnection on frontend refresh | UI lost progress context when reloading during a build | Backend now caches the latest state event per stream; frontend requests the last known state on reconnect |

---

## Next Week's Focus

| Priority | Goal | Success Criteria |
|----------|------|-----------------|
| High | Testnet Workflow Validation | Successfully deploy and fund a contract on CKB testnet using the Orbital platform |
| High | Contract Deployment Tracking | Persist deployed script metadata (code hash, cell deps) and display in frontend |
| Medium | Multi-Contract Workspace Support | Allow Orbkit to manage and build multiple contracts within a single project structure |
| Medium | Error Boundary Improvements | Map backend/orbkit errors to user-friendly frontend notifications with recovery steps |
| Low | Documentation & Onboarding | Finalize README, Orbkit CLI docs, and record a walkthrough video |

---

## Progress Summary

| Category | Completion | Notes |
|----------|------------|-------|
| Orbkit CLI Architecture | 100% | Stable, handles build/deploy/fund/structure commands |
| Backend API & Supabase | 100% | All endpoints, streaming, and GraphQL subscriptions functional |
| NDJSON Streaming Layer | 100% | Reliable, buffered, and integrated with frontend state |
| Deployment Flow | 100% | Unsigned preparation, passkey signing, and broadcast complete |
| Project Structure Sync | 100% | Manual and live sync operational with differential updates |
| Frontend Integration | 100% | https://orbital10.web.app/ fully wired and operational |

**Overall Month 3 Week 2:** The architectural reset is complete. Orbital now operates as a robust, decoupled system where Orbkit manages local execution and the hosted platform handles orchestration, state, and security. The end-to-end workflow—from initializing a workspace to deploying a contract via passkey signing—is fully functional and stable. Next week will focus on extending this stability to the CKB testnet and implementing post-deployment contract tracking.

---

*Report generated for CKB Academy Builder Track — Published to personal CKBuilder repository*
