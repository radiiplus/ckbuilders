# Orbital — Project Reference

## Purpose
Orbital is a local-first development environment for Nervos CKB smart contracts. It replaces fragmented CLI tooling, manual node management, and error-prone deployment scripts with a unified orchestration layer. The system automates multi-network setup, contract compilation, on-chain deployment, test funding, and real-time observability—so developers can focus on contract logic instead of infrastructure coordination.

It is not a wallet, a cloud deployment service, or a general-purpose IDE. It is a focused workspace built for secure, repeatable, and observable smart contract iteration.

## Architecture Overview
Orbital is structured into three isolated components that communicate through a well-defined API boundary:

| Component | Responsibility | Trust Boundary |
|-----------|----------------|----------------|
| **Server (Control Plane)** | GraphQL/REST API handling session management, wallet linking, deployment orchestration, contract metadata persistence, and event routing. | Server-side, authenticated |
| **Public (Dashboard)** | React-based frontend focused on deployment observability, contract inspection, funding controls, and real-time progress visualization. Zero trust; never handles secrets. | Browser-based, stateless |
| **Helper (Daemon)** | Isolated local process that executes privileged operations (node interaction, signing, funding), streams progress via Server-Sent Events (SSE), and maintains the security boundary between UI and chain. | Local-only, least-privilege |

## Core Workflow
The system follows a deterministic, observable cycle:
1. **Configure** → Define target network, contract paths, and deployment parameters in a single manifest.
2. **Build** → Compile Rust contracts, validate dependencies, and generate deployment artifacts.
3. **Deploy** → Orchestrate script upload, capture code hashes, cell dependencies, and witness templates. Persist metadata locally.
4. **Fund** → Execute devnet/testnet transfers via the helper service. Stream confirmation status in real time.
5. **Observe** → Inspect contract structure, deployment history, funding balances, and pipeline status through the dashboard.
6. **Iterate** → Adjust logic, redeploy, and repeat without environment reset or manual state tracking.

## Technical Principles
- **Local-First Execution**: Sensitive operations (signing, funding, node communication) run in the isolated helper runtime. No cloud dependency for critical paths.
- **Multi-Network Ready**: Devnet is the default testing environment, but the configuration model, artifact structure, and deployment pipeline explicitly support testnet and mainnet targets.
- **Observability by Design**: Every pipeline step emits structured events. SSE streams deliver live progress, funding status, and deployment confirmations without polling.
- **Structured Artifacts**: Deployments generate versioned manifests containing code hashes, cell dependencies, network targets, and transaction metadata for reliable inspection and replay.
- **Security Through Isolation**: Passkey-authenticated sessions, ephemeral credentials, and a strict UI-to-helper boundary prevent secret exposure. Mainnet actions require explicit confirmation and secondary verification.

## Current Development Scope
- **Operational**: Local devnet provisioning, Rust contract compilation, deployment orchestration, helper-driven funding, SSE progress streaming, and contract metadata inspection.
- **In Progress**: Live dashboard integration with backend pipeline responses, testnet configuration validation, session security hardening, and automated end-to-end testing.
- **Planned**: Mainnet guardrails, advanced contract analysis (execution cost estimation, dependency graphing), and multi-environment CI validation.

## Context for This Repository
Orbital is the primary project I am building and iterating on for the CKB Academy Builder Track. All weekly reports in this repository document progress, technical decisions, blockers, and learning outcomes directly tied to this codebase. 

When reviewing reports, note that:
- Progress percentages and status tags reflect the state of Orbital’s architecture, not external coursework.
- Technical learnings are derived from implementing CKB-specific patterns (UTXO management, script deployment, cell dependencies, VM execution constraints).
- Screenshots, transaction hashes, and CLI outputs reference Orbital’s internal tooling or the target CKB network being interacted with.

This document serves as the baseline reference for understanding the system being tracked across weekly logs.