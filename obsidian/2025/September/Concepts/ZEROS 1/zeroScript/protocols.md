# protocols

[[!index.md]]

protocols = pipes, sockets, streams, debug, net, ipc. everything is a stream, everything is text, everything is open. protocols are for connection, debug, io, networking. all non-system protocols are loopback verified by the kernel (see [[0krnl.md]]).

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
