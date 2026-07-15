# Network File Storage (Docs++)

Distributed file system written in C. Central Name Server handles metadata and routing, multiple Storage Servers provide persistent file storage, and CLI clients expose the user interface. Communication uses a compact binary header with JSON payloads.

## Architecture

```
Client(s) --> Name Server (metadata, routing, ACL) --> Storage Server(s) (file I/O, checkpoints)
```

- **Name Server** -- central coordinator: file index (trie-based), access control, request routing, heartbeat monitoring
- **Storage Servers** -- persistent file storage, sentence-level locking for concurrent writes, checkpoint/revert, undo support
- **Client** -- CLI for file operations (CREATE, READ, WRITE, DELETE, LIST, INFO, UNDO, EXEC, CHECKPOINT, etc.)

## Key Features

- Sentence-level locking -- multiple clients can write different sentences in the same file concurrently
- Access control -- owner-managed permissions (read/write), persisted to disk
- Checkpoints -- tag-based snapshots with list, view, and revert
- Undo -- revert last write per file
- Heartbeat monitoring -- name server detects storage server failures
- Persistence -- ACL and file index survive restarts

## Build and Run

```
# Build all components
cd common && make && cd ..
cd name_server && make && cd ..
cd storage_server && make && cd ..
cd client && make && cd ..

# Start (separate terminals)
./name_server/name_server 8000
./storage_server/storage_server localhost 8000 SS1 9002
./storage_server/storage_server localhost 8000 SS2 9003
./client/client localhost 8000
```

## Project Structure

```
client/          -- CLI client (commands.c, main.c)
name_server/     -- metadata coordinator (file_index.c with trie + LRU cache, main.c)
storage_server/  -- file store (storage_engine.c, main.c)
common/          -- shared code (protocol encoding, networking, persistence)
scripts/         -- build, start, stop helpers
docs/            -- architecture notes
tests/           -- test scripts
```
