# Network File Storage (Docs++)

A distributed file system written in C, featuring a central Name Server for metadata/routing, multiple Storage Servers for persistent file storage, and CLI clients for user interaction. Communication uses a compact binary header with JSON payloads.

## Architecture

```
Client(s) ──► Name Server (metadata, routing, ACL) ──► Storage Server(s) (file I/O, checkpoints)
```

- **Name Server** — Central coordinator: file index, access control lists, request routing, health monitoring via heartbeats
- **Storage Servers** — Persistent file storage, sentence-level locking for concurrent writes, checkpoint/revert support
- **Client** — CLI interface for file operations (CREATE, READ, WRITE, DELETE, LIST, INFO, etc.)

## Key Features

- **Sentence-level locking** — Multiple clients can write to different sentences in the same file simultaneously
- **Access control** — Owner-managed permissions with request/approve/deny workflow, persisted to disk
- **Checkpoints** — Tag-based snapshots with list, view, and revert operations
- **Undo support** — Revert last write operation per file
- **Heartbeat monitoring** — Name Server detects storage server failures
- **Persistence** — ACL and file index survive restarts

## Build & Run

```bash
# Build all components
cd client && make && cd ..
cd name_server && make && cd ..
cd storage_server && make && cd ..

# Start (separate terminals)
./name_server/name_server 8000
./storage_server/storage_server localhost 8000 SS1 9002
./storage_server/storage_server localhost 8000 SS2 9003
./client/client localhost 8000
```

See [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) for detailed network diagrams and communication flows.
See [docs/HOW_TO_RUN.md](docs/HOW_TO_RUN.md) for full setup instructions.

## Project Structure

```
client/          — CLI client (commands.c, main.c)
name_server/     — Metadata coordinator (file_index.c, main.c)
storage_server/  — File store (storage_engine.c, main.c)
common/          — Shared code (protocol, networking, persistence)
scripts/         — Build, start, stop, and test scripts
docs/            — Architecture, testing guide, report
```
