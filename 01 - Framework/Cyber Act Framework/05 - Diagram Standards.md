# Diagram Standards

> **Specification:** Cyber Act Mermaid Visual Diagram Standard  
> **Version:** 2.0  
> **Status:** Active Standard

---

## Architecture Diagram
Used to represent system components, OS kernel layers, and technology stack boundaries.

```mermaid
graph TD
    UserSpace[User Space Applications - Ring 3] -->|System Calls| KernelSpace[Kernel Space Core - Ring 0]
    KernelSpace --> VFS[Virtual Filesystem VFS]
    KernelSpace --> Memory[Virtual Memory MMU]
    KernelSpace --> Hardware[Physical Hardware Devices]
```

---

## Workflow Diagram
Used to illustrate operational sequences and step-by-step processing logic.

```mermaid
graph LR
    Input[Input Request] --> Validation[Parameter Validation]
    Validation --> Processing[Execution Engine]
    Processing --> Output[Formatted Output Payload]
```

---

## Sequence Diagram
Used to illustrate time-ordered interactions between system components, network protocols, or security actors.

```mermaid
sequenceDiagram
    autonumber
    actor Client
    participant Proxy as Reverse Proxy
    participant Server as Backend API

    Client->>Proxy: HTTPS Request (TLS 1.3)
    Proxy->>Server: Forward Request
    Server-->>Proxy: Return JSON Response
    Proxy-->>Client: Stream Payload
```

---

## State Machine
Used to map lifecycle states and state transition triggers.

```mermaid
stateDiagram-v2
    [*] --> Created
    Created --> Running : execve()
    Running --> Sleeping : I/O Wait
    Sleeping --> Running : Wake Interrupt
    Running --> Zombie : exit()
    Zombie --> [*] : waitpid()
```

---

## Component Diagram
Used to decompose internal subsystems and modules.

```mermaid
graph TD
    Engine[Core Engine] --> Parser[Data Parser]
    Engine --> Storage[Storage Driver]
    Engine --> Logger[Telemetry Logger]
```

---

## Data Flow Diagram
Used to trace data transformation pipelines from input to storage.

```mermaid
graph LR
    RawData[Raw Sensor Data] --> Forwarder[Log Forwarder]
    Forwarder --> Indexer[SIEM Indexer]
    Indexer --> Dashboard[SOC Analytics Dashboard]
```

---

## Knowledge Graph
Used to map conceptual topic hierarchies and skill dependencies.

```mermaid
graph TD
    LinuxBasics[Linux Foundations P-02] --> Processes[Process Architecture P-07]
    LinuxBasics --> Memory[Virtual Memory P-09]
    Processes --> Kernel[Linux Kernel P-14]
    Memory --> Kernel
```

---

## Timeline
Used to represent historical evolution or chronological event logs.

```text
1991: Linux Kernel v0.01 ──► 2003: Linux Kernel v2.6 (NPTL) ──► Modern: Linux Kernel v6.x (eBPF)
```

---

## Decision Tree
Used to model troubleshooting workflows and technology selection.

```mermaid
graph TD
    Start{Is Service Running?} -->|No| Fix1[Start Service via systemctl]
    Start -->|Yes| Check2{Is Port Open?}
    Check2 -->|No| Fix2[Update Firewall Rules]
    Check2 -->|Yes| OK[Service Healthy]
```

---

## Comparison Diagram
Used to contrast competing architectural paradigms.

```mermaid
graph TD
    subgraph Monolithic Kernel
        M1[VFS + Drivers + Scheduler in Ring 0]
    end
    subgraph Microkernel
        UK[Minimal IPC in Ring 0] --> US[Drivers in User Space Ring 3]
    end
```
