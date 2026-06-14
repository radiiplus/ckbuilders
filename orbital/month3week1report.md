# Builder Track Weekly Report — Month 3 Week 1
> Tracking progress in the CKB Academy Builder Program

---

**Name:** Positive Vibes  
**Week Ending:** June 7, 2026  
**Track:** CKB Developer Builder  
**Status:** Month 3 Week 1 — Complete

---

## Executive Summary

This week marked a complete architectural reset for the Orbital project. The previous monolithic approach to local runtime management introduced too much friction and state drift. Development restarted from scratch with a new decoupled architecture, introducing **Orbkit** as a standalone local CLI that bridges the user's workspace with the hosted Orbital platform. The primary focus was establishing the Orbkit scaffolding, rebuilding the backend API with Supabase, and implementing the foundational NDJSON streaming layer for build and funding commands. The system is currently in a transitional state, with the core communication bridge functional and command execution actively being wired up.

---

## Builder Progress

```
Focus: Architectural Reset → Orbkit CLI → Backend Streaming
```

| Status | Focus Area | Outcome |
|--------|------------|---------|
| Complete | Orbkit CLI Scaffolding | Standalone local runtime created with `orbital init`, service registration, and config management |
| Complete | Backend API Foundation | Rebuilt Node.js server with Supabase integration, GraphQL schema, and REST endpoints |
| In Progress | NDJSON Command Streaming | Backend accepts build/fund requests and streams structured NDJSON logs from Orbkit |
| In Progress | Passkey Authentication | Session management and device-bound token refresh integrated with the new backend |
| Pending | Deployment Flow | Unsigned transaction preparation and passkey signing flow not yet started |

---

## Key Learnings

### Decoupling Local Execution from Cloud Backend
> Moving from an embedded helper to a standalone CLI (Orbkit) required rethinking how commands are dispatched. By implementing a service bridge where Orbkit registers with an `ORBKIT_API_KEY`, the backend can securely route commands to the correct local workspace without exposing the user's local network. This separation of concerns makes the platform scalable and the local runtime portable.

### NDJSON over SSE for Command Streaming
> Switched to Newline Delimited JSON (NDJSON) for streaming build and deploy logs instead of Server-Sent Events. NDJSON provides better compatibility with standard HTTP clients and allows structured event parsing without the overhead of maintaining persistent SSE connections through Supabase. It simplifies the backend routing logic significantly.

### Passkey-Backed CKB Transactions
> CKB requires specific signing payloads for deployment transactions. Preparing the unsigned transaction in Orbkit and passing it to the frontend for WebAuthn/Passkey signing ensures private keys never leave the user's device. This split-signing model is critical for maintaining a zero-trust security boundary between the local runtime and the hosted platform.

---

## Practical Progress

### Project: Orbital & Orbkit
> A decoupled CKB development environment where Orbkit acts as the local runtime bridge, and the Orbital Frontend/Backend handles orchestration and state.

```
Workflow: Init Workspace → Start Orbkit → Build → Deploy → Fund → Observe
```

#### Orbkit CLI & Local Runtime
- [x] Designed and implemented the `orbital init` command to scaffold Rust contracts and runtime config
- [x] Implemented service registration logic (generates unique service ID, connects to backend)
- [x] Configured RISC-V target validation and `@offckb/cli` dependency integration
- [x] Established local environment variable handling (`ORBKIT_API_KEY`)

#### Backend API & Service Bridge
- [x] Rebuilt Node.js server with Supabase backend integration
- [x] Implemented GraphQL schema for account management, session validation, and service state
- [x] Created REST endpoints with NDJSON streaming (`/contracts/build`, `/wallets/devnet/fund`)
- [x] Implemented session refresh logic with device-bound tokens and automatic rotation

#### Integration & Streaming
- [x] Orbkit successfully registers with the backend and listens for incoming commands
- [x] Backend routes build requests to the first connected Orbkit service
- [x] NDJSON event structure defined (`queued`, `accepted`, `building`, `completed`, `failed`)
- [ ] Wiring Orbkit build execution to stream logs back to the backend in real-time
- [ ] Implementing devnet funding execution and progress streaming

---

## Blockers & Resolutions

| Blocker | Impact | Resolution Path |
|---------|--------|-----------------|
| Supabase Realtime limits for high-frequency logs | Using Supabase channels for verbose Rust compilation logs caused rate limiting | Pivoted to direct NDJSON streaming over REST endpoints; Supabase only handles state persistence |
| RISC-V compilation paths failing in Orbkit | Local builds failed due to missing target or incorrect toolchain paths | Added explicit `rustup target add` checks in Orbkit startup and improved error messaging |
| Passkey payload formatting for CKB | WebAuthn signing requires specific byte formatting for CKB transactions | Researching Lumos SDK transaction hashing to ensure the unsigned payload matches passkey expectations |

---

## Next Week's Focus

| Priority | Goal | Success Criteria |
|----------|------|-----------------|
| High | Complete Build & Funding NDJSON Streams | Orbkit executes builds and funding, streaming structured logs to the backend successfully |
| High | Implement Unsigned Deployment Flow | Orbkit prepares deploy tx, backend exposes broadcast endpoint, frontend can sign |
| Medium | Project Structure Sync | Implement manual and live file watching for contract structure analysis |
| Medium | Frontend Integration | Connect https://orbital10.web.app/ to the new backend and wire up basic commands |
| Low | Orbkit Error Handling | Graceful shutdown, reconnection logic, and better CLI error outputs |

---

## Progress Summary

| Category | Completion | Notes |
|----------|------------|-------|
| Orbkit CLI Architecture | 80% | Scaffolding and registration complete; execution logic in progress |
| Backend API & Supabase | 85% | Core endpoints, GraphQL, and session management functional |
| NDJSON Streaming Layer | 60% | Structure defined; build/fund execution wiring in progress |
| Deployment Flow | 10% | Architecture designed; implementation pending |
| Project Structure Sync | 0% | Not started |
| Frontend Integration | 10% | URL hosted; backend connection not yet wired |

**Overall Month 3 Week 1:** The project has been successfully reset with a new decoupled architecture. Orbkit is now a standalone local runtime that registers with the hosted Orbital backend. The foundational API, session management, and NDJSON streaming structures are in place. Next week will focus on completing the command execution flows (build, fund, deploy) and connecting the frontend to validate the end-to-end bridge.

---

*Report generated for CKB Academy Builder Track — Published to personal CKBuilder repository*

