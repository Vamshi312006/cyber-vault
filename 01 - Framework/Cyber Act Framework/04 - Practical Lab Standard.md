# Practical Lab Standard

> **Specification:** Cyber Act Hands-On Lab Standard  
> **Version:** 2.0  
> **Status:** Active Standard

---

## Objective
State clearly what skill, command, trace, or security concept the learner will build, observe, debug, or mitigate in this lab.

---

## Prerequisites
List required software tools, user privileges (e.g. `sudo`), and background knowledge needed before executing the lab.

---

## Environment
Define the required runtime environment (e.g., Ubuntu 22.04 LTS, Windows 11 with Sysmon, Docker Container, or Isolated VM).

---

## Steps

Provide executable, copy-pasteable terminal commands accompanied by clear explanatory comments.

```bash
# 1. Initialize lab environment directory
mkdir -p ~/cyber-lab && cd ~/cyber-lab

# 2. Execute target lab command
{{command_execution}}

# 3. Inspect telemetry or generated output
{{inspection_command}}
```

---

## Expected Output
Provide explicit textual or code snippets demonstrating what successful execution looks like on terminal standard output (stdout).

```text
[+] Operation completed successfully!
[+] Output verified against expected schema.
```

---

## Verification
Detail exact validation commands to verify that state changes occurred correctly (e.g., checking return codes `$?`, inspecting `/proc`, or querying log files).

---

## Cleanup
Provide exact teardown commands to return the system to its original clean state.

```bash
# Clean up temporary test artifacts
cd ~ && rm -rf ~/cyber-lab
```

---

## Discussion
Explain *why* the commands produced the observed output, linking the practical results back to low-level kernel mechanics, memory structures, or security models.

---

## Further Exploration
Provide 2–3 advanced extension exercises for deep-dive investigation.
