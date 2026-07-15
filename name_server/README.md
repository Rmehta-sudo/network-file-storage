# Name Server

Central coordinator for the distributed file system.

## Files

- `src/main.c` -- server main loop, accepts connections, dispatches opcodes (LOOKUP, COMMAND_FORWARD, REGISTER, etc.)
- `src/file_index.c` -- trie-based file index for O(k) filename lookups, LRU cache (128 entries) for O(1) hot-path, and per-file ACL enforcement
- `include/file_index.h` -- data structures and API for trie, cache, and ACL

## Responsibilities

- Maintains the file index (which files exist on which storage servers)
- Enforces access control before routing clients to storage servers
- Handles CREATE, DELETE, ADDACCESS, REMACCESS, LIST, VIEW, EXEC commands directly
- Selects the least-loaded storage server for read/write requests
- Monitors storage server health via heartbeats
- Persists ACL data to disk (`nm_data/acl/`)
