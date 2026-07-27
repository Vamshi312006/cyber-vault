# 10 Glossary

## System Terminology & Definitions

### Runtime
The execution environment responsible for hosting processes, managing resource lifecycles, and supervising system interaction safely.

### Tool
An executable module or CLI capability exposed to the AI agent to interact with external systems, files, or operating system interfaces.

### Dispatcher
The component that receives planned tool calls, validates authorization/parameters, routes execution to the appropriate handler, and captures outputs.

### Planner
The reasoning engine that breaks down high-level user tasks into structured, sequential, or graph-based execution plans and tool calls.

### Capability
A specific feature, permission, or functional action (e.g., file read, command execution) supported by a tool or component.

### Context
The aggregated state, conversation history, system prompt, and active environment metadata provided to the LLM during reasoning.

### Event
A structured state change or system notification emitted during processing (e.g., tool started, execution completed, error raised).

### Registry
The central repository tracking available tools, capabilities, system agents, and operational plugins along with their metadata and schemas.

### Agent
An autonomous or semi-autonomous AI entity operating with a defined persona, prompt, available tools, and decision-making loop.

### Memory
The persistent and short-term state storage mechanisms (e.g., key-value store, vector store, session logs) used to retain context across operations.
