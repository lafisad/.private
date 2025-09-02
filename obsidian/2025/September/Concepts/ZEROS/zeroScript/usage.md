

**if it works, change it.**


---

## modules
modules = kernel/syscalls. they do one thing, are small, composable, patchable. user can override, extend, or replace any module. all modules (except core/system) are loopback verified by the kernel (see @0krnl.md).

```zeroscript
// memory module: minimal, patchable
MODULE 'mem'
    ALLOC(size) { RETURN ALLOCATE(size) }
    FREE(ptr)   { DEALLOCATE(ptr) }
END MODULE

// scheduler module: simple, composable
MODULE 'sched'
    SPAWN(fn)   { RETURN FORK(fn) }
    WAIT(pid)   { RETURN JOIN(pid) }
END MODULE

// filesystem module: everything is a file
MODULE 'fs'
    READ(path)  { RETURN READ_FILE(path) }
    WRITE(path, data) { RETURN WRITE_FILE(path, data) }
END MODULE

// user can patch any module
PATCH MODULE 'fs' WITH {
    WRITE(path, data) {
        LOG 'patched write: ' + path
        RETURN WRITE_FILE(path, data)
    }
}
```

---

## protocols
protocols = pipes, sockets, streams, debug, net, ipc. everything is a stream, everything is text, everything is open. protocols are for connection, debug, io, networking. all non-system protocols are loopback verified by the kernel.

```zeroscript
// debug protocol: text stream for logs
PROTOCOL 'debug'
    SEND(msg) { LOG '[debug] ' + msg }
END PROTOCOL

// net protocol: minimal tcp
PROTOCOL 'tcp'
    CONNECT(addr) { RETURN OPEN_SOCKET(addr) }
    SEND(sock, data) { RETURN SOCKET_SEND(sock, data) }
    RECV(sock) { RETURN SOCKET_RECV(sock) }
END PROTOCOL

// shell protocol: everything is a command, everything is a stream
PROTOCOL 'shell'
    EXEC(cmd) { RETURN SYSTEM(cmd) }
    PIPE(cmd1, cmd2) { RETURN PIPE(cmd1, cmd2) }
END PROTOCOL

// user can patch any protocol
PATCH PROTOCOL 'debug' WITH {
    SEND(msg) { LOG '[patched debug] ' + msg }
}
```

---

## composition
modules + protocols = everything. compose, patch, override, chain, fork, pipe, redirect, log, debug, repeat. all userland code is loopback verified by the kernel for integrity (see @0krnl.md).

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

---

## kernel integrity & loopback verification
- [[0krnl]] is the root of trust. it checks signatures, manages secure context switches, and logs kernel events for audit.
- everything not from the system (user modules, user protocols) is loopback verified: it always passes through the kernel's integrity checks (see @0krnl.md).
- you are root, but the kernel is always watching.

---

## philosophy
- if it works, change it.
- do one thing well.
- everything is a file, everything is a stream.
- patch, don’t break.
- user is root, but trust is always verified.
- defaults are just a suggestion.

---

zeroScript = unix for the next era. user = coder, architect, hacker, admin. every idea = scriptable. every structure = patchable. every limit = overrideable. everything = userland, but the kernel always verifies.
