# Common

Shared library used by all components (client, name server, storage server).

## Files

- `src/protocol.c` -- binary protocol encoding/decoding (opcode + length header, JSON payload serialization)
- `src/net.c` -- socket helpers (send_all, recv_all, connection setup)
- `src/persistence.c` -- JSON file persistence for registry and ACL data
- `include/` -- corresponding header files with struct definitions and constants
