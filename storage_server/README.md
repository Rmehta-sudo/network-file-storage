# Storage Server

Persistent file store for the distributed file system.

## Files

- `src/main.c` -- server main loop, handles client connections for READ/WRITE/STREAM/INFO/UNDO, manages sentence-level locks
- `src/storage_engine.c` -- file I/O, metadata management (word/sentence/char counts, timestamps), undo snapshots, checkpoint storage and retrieval
- `include/storage_engine.h` -- storage engine API and error codes

## How It Works

- On startup, registers with the Name Server and reports its file manifest
- Listens on a dedicated client port for direct file I/O from clients
- WRITE operations: acquires sentence-level lock, queues word updates, applies on ETIRW commit, saves undo snapshot
- Stores files, metadata, undo history, and checkpoints under `ss_data/<SS_ID>/`
- Sends periodic heartbeats to the Name Server
