# 00 Local LLM Benchmarks

> Document ID: DOC-ULTRON-L7-RESEARCH-BENCHMARKS
> Document Name: 00 Local LLM Benchmarks
> Version: 1.0.0
> Status: Frozen
> Owner: Ultron.AIEngineer
> Architecture Version: 1.0.0
> Abstraction Level: L7 Research & Benchmarks
> Last Updated: 2026-07-27

---

# Purpose

This document records the evaluation benchmarks, token throughput rates, context window limits, and JSON parsing error rates for local open-source LLM models tested with Project Ultron.

---

# Evaluated Local Models

| Model Name | Quantization | Context Window | Inference Latency | JSON Parsing Pass Rate | Recommended Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `llama3:8b-instruct` | Q4_K_M | 8192 tokens | ~45 tokens/sec | `98.4%` | **Primary Production Model** |
| `mistral:7b-instruct` | Q4_K_M | 4096 tokens | ~52 tokens/sec | `96.1%` | Secondary Fallback |
| `codellama:7b` | Q4_K_M | 4096 tokens | ~48 tokens/sec | `95.2%` | Code Generation Specialist |
