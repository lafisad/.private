# modules

[[!index.md]]

modules = kernel/syscalls. they do one thing, are small, composable, patchable. user can override, extend, or replace any module. all modules (except core/system) are loopback verified by the kernel (see [[0krnl.md]]).

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
