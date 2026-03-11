# RunLedger

RunLedger is the deterministic execution ledger for Covenant Systems.

Its purpose is to record **runs** as append-only, hash-chained events so execution can be verified, compared, witnessed, and audited without relying on narrative or mutable operator logs.

If a system says something ran, RunLedger is the layer that proves it.

## What this project is

RunLedger is a standalone deterministic execution instrument that:

- starts runs with a canonical `run.genesis.v1`
- appends run events in an append-only transcript
- closes runs with `run.closed.v1`
- verifies transcripts non-mutatingly
- provides the execution truth layer for future governed task management
- integrates with external trust and witness layers rather than replacing them

RunLedger is not a scheduler by itself.
RunLedger is not a policy engine by itself.
RunLedger is not a witness ledger by itself.

It is the **execution proof ledger**.

## External law dependencies

RunLedger operates inside the Covenant Systems law stack:

- **NeverLost v1** provides identity and signing trust roots
- **NFL** provides witness duplication only
- **WatchTower** provides verification / visibility plane
- **Packet Constitution v1** provides transport law for packetized artifacts

## Core model

A run is a deterministic chain of events.

Each event includes at minimum:

- `schema`
- `t_utc`
- `event_type`
- `run_id`
- `seq`
- `prev_event_hash`
- `event_hash`

The first event is `run.genesis.v1`.

The final event is typically `run.closed.v1`.

`event_hash` is computed over canonical event bytes **without** the `event_hash` field present.

The first event uses a `prev_event_hash` of 64 zero hex characters.

## Current implemented surface

Current implemented and proven pieces include:

- laws document writer
- event type registry lock
- run genesis model
- run close event
- transcript verification
- Packet Constitution minimal vector work
- NeverLost local integration repair
- selftests for registry, packet receipt path, and run genesis/close/verify flow

## Environment

Canonical environment:

- Windows PowerShell 5.1
- `Set-StrictMode -Version Latest`
- UTF-8 no BOM
- LF line endings only
- SHA-256 over canonical bytes
- deterministic write -> parse-gate -> run discipline

## Repo layout

```text
docs/
schemas/
scripts/
runs/
proofs/
test_vectors/
