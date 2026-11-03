# Security Policy

Takt Protocol takes the security of its decentralized infrastructure, TypeScript SDK, keeper node daemons, and frontend web applications seriously.

## Supported Versions

| Package / Component   | Version | Supported          |
| --------------------- | ------- | ------------------ |
| `@takt-protocol/sdk`  | 0.1.x   | :white_check_mark: |
| `services/keeper`     | 0.1.x   | :white_check_mark: |
| `apps/web`            | 0.1.x   | :white_check_mark: |

## Threat Model & Operational Security

### 1. Keeper Secret Key Isolation (`KEEPER_SECRET_KEY`)
- The Go keeper node daemon (`services/keeper`) requires a funded Stellar secret key (`KEEPER_SECRET_KEY` / `TAKT_KEEPER_SECRET`) to sign and broadcast `execute_job` transactions.
- Keepers should never run daemons using admin or cold-wallet keys. Secret keys must be supplied via secure environment variables or vault secret managers, never committed to revision control, and isolated to dedicated runner processes with minimum necessary XLM fee balances.

### 2. Pre-Simulation & Revert Protection
- Arbitrary target contract executions triggered via `job-engine::execute_job` may fail, run out of gas, or trap due to target contract logic errors or malicious contract designs.
- Keepers must always pre-simulate `execute_job` invocations via Soroban RPC (`simulateTransaction`) before broadcasting signed transactions on-chain. Broadcasting failing transactions incurs unnecessary fee and gas consumption without capturing bounty payouts.

### 3. RPC Endpoint Trust & State Verifiability
- The keeper daemon and frontend web client query Soroban RPC endpoints (e.g., `https://soroban-testnet.stellar.org`).
- An untrusted, compromised, or desynchronized RPC provider could return stale ledger sequence numbers, manipulate simulated footprint resources, or withhold transactions. In production environments, node operators should connect to high-availability, authenticated, or self-hosted RPC endpoints.

## Reporting a Vulnerability

If you discover a potential security vulnerability within this repository (`takt-app`) or the on-chain smart contracts (`takt-contract`), please do NOT disclose it via public GitHub issues or discussions.

Instead, please report security issues privately using GitHub Security Advisories:
[Submit Security Advisory](https://github.com/takt-protocol/takt-app/security/advisories/new)

Please include:
- A description of the vulnerability and its potential impact.
- Step-by-step instructions or proof-of-concept scripts to reproduce the issue.
- Any relevant logs, transaction hashes, or environment details.

We commit to acknowledging reports within 48 hours and providing coordinated remediation timelines.
