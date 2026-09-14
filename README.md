# DWN — C++ Decentralized Web Node

A lightweight C++20 Decentralized Web Node (DWN) service for secure, user-owned data storage and Web5 applications.

## Architecture

HTTP Client -> JSON-RPC (Beast) -> DWN Router (`dwn.processMessage`) -> Records Handler / DID Resolver / Auth Verifier -> Storage Interface -> LevelDB

## Storage

The DWN core uses **LevelDB as its local persistent storage backend**.

Logical storage namespaces include:
- `message|<targetDid>|<recordId>` — record/message metadata
- `data|<targetDid>|<recordId>` — actual binary record payload bytes
- `tenant|<targetDid>` — tenant state
- `snapshot|<name>` — snapshots
- `event|<targetDid>|<eventId>` — event data

The storage path can be configured with `DWN_LEVELDB_PATH`.

## Quick Start

Docker:
```bash
docker-compose -f docker/docker-compose.yml up --build
```

Local:
```bash
cmake -S . -B build
cmake --build build
./build/DWN
```

## API

POST `/json-rpc`

GET `/health`, `/info`, `/metrics`, `/version`

WebSocket: `ws://host:port/`

### `dwn.processMessage` example

See `examples/milan_client.js`.

## Security

TLS, rate limiting, request size limits, Ed25519 signature verification via libsodium, owner-only MVP controls, and tenant isolation at the storage layer.

## Milan Integration

Milan integrates with the DWN JSON-RPC endpoint using `dwn.processMessage`.

## Testing

```bash
cmake -S . -B build -DBUILD_TESTS=ON
./build/dwn_tests
```

## Current Status

This repository is an actively developed DWN implementation. The current core is built around C++20, JSON-RPC processing, DID/signature verification, and LevelDB-backed local persistence.

Protocol and storage capabilities will continue to expand toward broader DWN/Web5 compatibility.
