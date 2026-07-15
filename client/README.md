# Client

CLI client for the distributed file system.

## Files

- `main.c` -- entry point, connects to name server, runs interactive command loop
- `src/commands.c` -- implements all user commands (CREATE, READ, WRITE, DELETE, UNDO, INFO, VIEW, LIST, EXEC, ADDACCESS, REMACCESS, CHECKPOINT, etc.)
- `include/commands.h` -- command handler declarations

## How It Works

1. Client connects to the Name Server on startup and registers a username
2. For metadata operations (CREATE, DELETE, LIST, VIEW, ADDACCESS), the client talks directly to the Name Server
3. For data operations (READ, WRITE, STREAM), the client asks the Name Server for the file's storage server location, then connects directly to that SS for the I/O
