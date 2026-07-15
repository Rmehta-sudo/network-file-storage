# Tests

Test scripts for the distributed file system.

- `test_commands.sh` -- tests all commands (CREATE, WRITE, READ, UNDO, DELETE, ADDACCESS, EXEC, etc.)
- `test_bugs_demo.sh` -- demonstrates bug fixes and error handling (WRITE errors don't crash client, ACL enforcement)
- `test_view.sh` -- tests VIEW command variants (-a, -l, -al flags)
- `test_phase3.sh` -- low-level integration tests using Python sockets (registration, lookup, cache)

## Usage

Start the servers first, then run any script:
```
./start_servers.sh       # from project root
./tests/test_commands.sh
```
