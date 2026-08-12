# Documentation Style Guide

> **Standard:** Cyber Act Presentation & Formatting Rules  
> **Version:** 2.0  
> **Status:** Active Standard

---

## Heading Hierarchy

All Cyber Act documentation strictly follows a 4-level Markdown heading hierarchy:

```markdown
# Module Name (H1 - Document Title Only)

## Major Section (H2 - Parts & Primary Framework Sections)

### Subsection (H3 - Detailed Categories & Components)

#### Item (H4 - Individual Commands, Terms, or Specific Sub-components)
```

---

## Writing Rules

- **One Idea Per Sentence:** Avoid long compound sentences. Keep statements concise and direct.
- **Maximum Paragraph Length:** Paragraphs must not exceed 3–4 lines. Break text into scannable blocks or bullet lists.
- **Active Voice:** Write in the active voice (e.g., "The kernel allocates memory," not "Memory is allocated by the kernel").
- **Consistent Terminology:** Use standardized technical terms throughout (e.g., always use `User Space` and `Kernel Space`, not `user-land` or `kernel mode` interchangeably).

---

## Spacing Rules

- **Section Breaks:** Place horizontal rules (`---`) between major H2 sections.
- **Line Spacing:** Use single line breaks between list items and double line breaks between paragraphs/subsections.
- **Whitespace:** Ensure adequate visual breathing room to prevent dense blocks of text.

---

## Tables

### When to Use
- **Comparative Analysis:** Comparing technologies, protocols, or operating systems across fixed attributes.
- **Status & Event Code Mapping:** Displaying Event IDs, HTTP Status Codes, or Signal mappings.
- **Quick Reference Checklists:** Highlighting feature matrices and trade-offs.

### When NOT to Use
- **Definition Lists:** Do NOT use tables for simple term definitions; use formatted definition lists instead.
- **Command References:** Do NOT compress complex CLI commands into table cells; use dedicated command blocks.

---

## Cards

Use blockquote cards for module headers, key definitions, and mental models:

```markdown
> **Module ID:** P-01  
> **Category:** Software Engineering & Version Control  
> **Prerequisites:** Command Line Basics  
> **Framework Standard:** Cyber Act Universal Engineering Framework
```

---

## Callouts

Use GitHub-flavored alert callouts strategically to highlight key operational insights:

```markdown
> [!NOTE]
> Background context, foundational details, or technical clarifications.

> [!TIP]
> Operational shortcuts, performance optimization tips, or efficiency hacks.

> [!IMPORTANT]
> Essential configuration rules, prerequisite steps, or critical concepts.

> [!WARNING]
> Breaking changes, common footguns, or operational risks.

> [!CAUTION]
> High-risk security vulnerabilities, data loss scenarios, or destructive commands.
```

---

## Diagrams

Diagrams must be rendered using standard **Mermaid** syntax:

- **Architecture Diagrams:** `graph TD` / `graph LR` for component relationships.
- **Workflow / Data Flow:** `graph TD` showing directional data movement.
- **Sequence Diagrams:** `sequenceDiagram` with numbered steps (`autonumber`) for execution flows.
- **State Machines:** `stateDiagram-v2` for process/connection lifecycles.
- **Knowledge Graphs:** `graph TD` showing conceptual dependencies.

---

## Code Blocks

- Always specify explicit language identifiers (`bash`, `powershell`, `python`, `sql`, `c`, `text`).
- Include explanatory comments for complex syntax lines.

---

## Command Formatting

Every command must be documented in an individual reference block:

```markdown
#### `command_name`
* **Purpose:** Concise description of what the command does.
* **Syntax:** `command [options] [arguments]`
* **Parameters:**
  * `-flag`: Description of parameter.
* **Input / Output:** Data input sources ➔ Output results.
* **Example Usage:**
  ```bash
  command -flag argument
  ```
* **Notes:**
  > [!TIP]
  > Operational tip or best practice.
```

---

## Lab Formatting

Every practical lab must follow the standard step-by-step code execution format:

```markdown
### Lab Title (Lab Type)
```bash
# 1. Step description
command_1

# 2. Verification step
command_2
```
```

---

## Interview Formatting

Format Q&A items using structured callouts:

```markdown
### Category Title
* **Question:** *State the question clearly in italics?*
  > [!NOTE]
  > Direct, engineering-grade answer highlighting key concepts and technical depth.
```

---

## Summary Formatting

Conclude every document with a concise executive summary and a 1-minute revision checklist:

```markdown
## Summary

### Executive Summary & Revision
* **Key Takeaways:** 2–3 sentence high-level summary of the entire module.
* **One-Minute Revision:** Quick chronological memory chain (e.g. `Step 1 ➔ Step 2 ➔ Step 3`).
```
