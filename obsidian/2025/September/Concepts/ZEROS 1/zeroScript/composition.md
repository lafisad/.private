# composition

[[!index.md]]

modules + protocols = everything. compose, patch, override, chain, fork, pipe, redirect, log, debug, repeat. all userland code is loopback verified by the kernel for integrity (see [[0krnl.md]]).

```zeroscript
// example: read file, send over tcp, log debug
data = CALL module('fs').READ('/etc/passwd')
sock = CALL protocol('tcp').CONNECT('10.0.0.1:22')
CALL protocol('tcp').SEND(sock, data)
CALL protocol('debug').SEND('sent passwd over tcp')

// example: patch scheduler to log every spawn
PATCH MODULE 'sched' WITH {
    SPAWN(fn) {
        LOG 'spawning new process'
        RETURN FORK(fn)
    }
}
```
