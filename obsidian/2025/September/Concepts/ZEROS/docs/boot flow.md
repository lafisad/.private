# ZEROS Boot Flow

This diagram illustrates the secure boot process of the ZEROS operating system, from kernel initialization to system running state. The secScore is calculated during initialization and determines if the system is secure enough to proceed. Each step includes security checks and module loading, ensuring only validated components are executed.

```mermaid
flowchart TD
    KRNL["0krnl: Root-of-Trust"] --> INIT["0init: Hardware Scan + Security Check"]
    INIT --> SECCALC["Calculate secScore"]
    SECCALC --> SEC["secScore ≥ 85%?"]
    SEC -- "No" --> STOP["System Halted"]
    SEC -- "Yes" --> BOOT["0boot: Load Core Modules (if secScore valid)"]

    %% Core Frameworks
    subgraph SYSFW["System Framework"]
        SCHD["0schd: Task Scheduler"]
        MEMR["0memr: Memory Manager"]
        IO["0io: I/O Manager"]
        FSYS["0fsys: Filesystem Manager"]
    end

    subgraph DEVFW["Driver + Device Framework"]
        DRV["0drv: Driver Framework"]
        DEV["0dev: Device Manager"]
        IPC["0ipc: Interprocess Comms"]
    end

    subgraph NETFW["Network + External Framework"]
        NETW["0netw: Network Stack"]
    end

    %% Connections
    BOOT --> SYSFW
    BOOT --> DEVFW
    BOOT --> NETFW

    SYSFW --> COREVAL["Core Modules Validated?"]
    DEVFW --> COREVAL
    NETFW --> COREVAL

    COREVAL -- "No" --> STOP
    COREVAL -- "Yes" --> ZS["zeroScript: Parse + Verify"]
    ZS --> ZSVAL["zeroScript Verified?"]
    ZSVAL -- "No" --> STOP
    ZSVAL -- "Yes" --> RUN["System Running"]

