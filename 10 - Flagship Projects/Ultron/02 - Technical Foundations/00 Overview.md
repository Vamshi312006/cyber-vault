# 00 Technical Foundations Overview

> Document ID: DOC-ULTRON-L2-TECHNICAL-FOUNDATIONS
> Document Name: 00 Technical Foundations Overview
> Version: 1.0.0
> Status: Frozen
> Owner: Ultron.ArchitectureLead
> Architecture Version: 1.0.0
> Abstraction Level: L2 Technical Prerequisites
> Last Updated: 2026-07-27

---

# Purpose

This document specifies the technical prerequisites, runtime environments, hardware dependencies, and host operating system constraints for Project Ultron.

---

# Prerequisites

- **Operating System**: Linux (Ubuntu 22.04 LTS / Debian 12 / Arch Linux).
- **Runtime Language**: Python 3.11+.
- **Local AI Inference Engine**: Ollama daemon running on `http://127.0.0.1:11434` with `llama3:8b-instruct`.
- **Vector Database**: Local Chroma / SQLite-vss instance.
- **Process Supervisor**: Linux `subprocess` API with `asyncio`.
