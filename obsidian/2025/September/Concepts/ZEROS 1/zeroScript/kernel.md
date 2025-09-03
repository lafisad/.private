# kernel integrity & loopback verification

[[!index.md]]

- [[0krnl.md]] is the root of trust. it checks signatures, manages secure context switches, and logs kernel events for audit.
- everything not from the system (user modules, user protocols) is loopback verified: it always passes through the kernel's integrity checks (see [[0krnl.md]]).
- you are root, but the kernel is always watching.
