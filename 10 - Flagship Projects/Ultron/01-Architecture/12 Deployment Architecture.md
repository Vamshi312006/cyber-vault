# 12 Deployment Architecture

> Document ID: DOC-ULTRON-L5-DEPLOYMENT-ARCHITECTURE
> Document Name: 12 Deployment Architecture
> Version: 1.0.0
> Status: Frozen
> Owner: Ultron.ArchitectureLead
> Architecture Version: 1.0.0
> Abstraction Level: L5 Deployment Architecture
> Parent Document: DOC-ULTRON-L6-DATA-MODELS
> Child Documents: DOC-ULTRON-L3-SEQUENCE-DIAGRAMS
> Dependencies: A2A-ADP v1.0, AKM v1.0, ARM v1.0, ADS v1.0
> Last Updated: 2026-07-27

---

# Purpose

This document specifies the deployment topology, packaging model, host system prerequisites, and process supervision architecture for Project Ultron.

It defines how the application daemon is installed, configured, systemd-supervised, and run within local Linux operating system environments.

---

# Scope

### Included in Scope
- Host Linux deployment prerequisites (Python 3.11+, systemd, Ollama / llama.cpp).
- Systemd service configuration model (`ultron.service`).
- Environment variable configuration and deployment layout.

### Excluded from Scope
- Kubernetes multi-cluster Helm chart templates (out of scope for baseline single-node deployment).
- Cloud provider IAM role provisioning.

---

# Engineering Question

**How is the Ultron platform deployed, packaged, systemd-supervised, and run on host Linux operating environments?**

---

# Context

This document occupies abstraction level **L5 Deployment Architecture** in the Ultron architecture hierarchy.

It derives from `11 Data Models.md` and provides deployment specifications for `13 Sequence Diagrams.md`.

```
11 Data Models.md (L6 Data Models)
↓
12 Deployment Architecture.md (L5 Deployment Architecture - This Document)
↓
13 Sequence Diagrams.md (L3 Sequence Diagrams)
```

---

# Architecture

Ultron deploys as a systemd-managed background service daemon on Linux hosts:

```
+-----------------------------------------------------------------------+
|                       LINUX HOST OPERATING SYSTEM                     |
|                                                                       |
|  +-----------------------------------------------------------------+  |
|  | Systemd Service Manager (systemd)                               |  |
|  | - Service Unit: /etc/systemd/system/ultron.service               |  |
|  | - Restarts daemon automatically on failure                         |  |
|  +-----------------------------------------------------------------+  |
|                                 |                                     |
|                                 v (Supervises Daemon Process)         |
|  +-----------------------------------------------------------------+  |
|  | Ultron Application Daemon (`python3 backend/main.py`)            |  |
|  | - Listens on Local IPC / Loopback Port 127.0.0.1:8080            |  |
|  +-----------------------------------------------------------------+  |
|                                 |                                     |
|           +---------------------+---------------------+               |
|           |                                           |               |
|           v                                           v               |
|  +-------------------------+                 +---------------------+  |
|  | Local LLM Service       |                 | Local Filesystem    |  |
|  | (Ollama / llama.cpp)    |                 | (~/.ultron/storage) |  |
|  | Port 127.0.0.1:11434    |                 | (SQLite / Vector)   |  |
|  +-------------------------+                 +---------------------+  |
+-----------------------------------------------------------------------+
```

### Systemd Service Specification (`ultron.service`)
```ini
[Unit]
Description=Ultron AI Engineering Platform Daemon
After=network.target local-fs.target ollama.service

[Service]
Type=simple
User=ultron
WorkingDirectory=/opt/ultron
ExecStart=/opt/ultron/venv/bin/python3 backend/main.py
Restart=on-failure
RestartSec=5s
EnvironmentFile=/etc/ultron/ultron.env

[Install]
WantedBy=multi-user.target
```

---

# Responsibilities

### Primary Responsibilities
- Specify deployment architecture and systemd supervision model for Ultron.
- Ensure host installation dependencies and directory permissions are clearly defined.
- Maintain deployment simplicity for local Linux installations.

### Secondary Responsibilities
- Provide deployment reference for DevOps and Linux platform engineers.
- Guide troubleshooting for daemon startup failures.

### Out of Scope
- Writing Debian `.deb` or RPM `.rpm` package build scripts.
- Managing Linux kernel updates.

---

# Relationships

### Parent Of
- `DOC-ULTRON-L3-SEQUENCE-DIAGRAMS` (`13 Sequence Diagrams.md`).

### Child Of
- `DOC-ULTRON-L6-DATA-MODELS` (`11 Data Models.md`).

### References
- `AKM v1.0` (Architecture Knowledge Model Ontology).
- `ADS v1.0` (Architecture Document Schema).

---

# Dependencies

| Target | Type | Direction | Reason | Criticality |
| :--- | :--- | :--- | :--- | :--- |
| `11 Data Models.md` | Parent Document | Incoming | Provides entity schemas requiring persistent deployment storage | Mandatory |
| `ARM v1.0` | Reference Model | Incoming | Establishes deployment layer rules | Mandatory |
| `A2A-ADP v1.0` | Protocol | Operational | Enforces delegation and compilation rules | Mandatory |

---

# Constraints

- **Local Loopback Binding**: Default network listeners MUST bind strictly to local loopback (`127.0.0.1`) to prevent unauthorized external network access.
- **Systemd Supervision**: Production Linux deployments MUST run under systemd process supervision.
- **Offline Dependency Check**: Deployment scripts must verify that local inference endpoints (Ollama) are reachable without cloud network calls.

---

# References

- `10 - Flagship Projects/Ultron/01-Architecture/11 Data Models.md` (Data Models)
- `10 - Flagship Projects/Ultron/01-Architecture/08 Runtime Architecture.md` (Runtime Architecture)
- `00 - Meta/AI Collaboration/ADS.md` (Architecture Document Schema)

---

# Future Scope

- Specification of Containerfile / Dockerfile packaging for containerized deployments.
- Definition of multi-node systemd service mesh topologies.
