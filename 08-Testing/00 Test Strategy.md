# 00 Test Strategy

## Overview
This document outlines the testing strategy for Ultron, defining what gets tested, when tests are executed, how they run, and the distinction between unit and integration testing.

## Strategy Summary

| Level | Scope | Frequency | Tools / Frameworks |
| :--- | :--- | :--- | :--- |
| **Unit Tests** | Fast, isolated function & module logic testing | Every commit / PR | pytest, unittest |
| **Integration Tests** | Subsystem interaction, API contracts, tool execution | On merge / Nightly | pytest, Docker testbed |
| **Security Tests** | Command validation, sandbox containment, secret scanning | CI Pipeline & Pre-release | Custom security suites |
| **End-to-End (E2E)** | Complete request-to-execution user flows | Pre-release | System test scripts |

## What Gets Tested

### 1. Core Logic & Utilities (Unit)
- Input parsing, schema validation, string manipulation
- Context window calculations and prompt formatting
- Security rule matchers and sanitizer logic

### 2. Subsystems & Integration (Integration)
- **Planner & Dispatcher**: Tool selection accuracy and command dispatch
- **Security Guardrails**: Command validation and privilege escalation checks
- **Tool Suite**: Execution of filesystem, process, and CLI tools inside sandbox environments
- **API Endpoints**: Request/response validation and error status codes

## Testing Cadence (When)
- **Pre-commit**: Fast unit tests and linting
- **Pull Request**: Full unit test suite + security static analysis
- **Main Branch Merge**: Integration test suite in isolated container environment
- **Release Candidate**: Full end-to-end regression and manual test protocols

## Unit vs. Integration Testing Guidance
- **Unit Tests**: Must mock external OS calls, network requests, and LLM API calls. They run in milliseconds.
- **Integration Tests**: Test interactions between real modules (e.g., Dispatcher calling actual Tool wrappers in a containerized environment).
