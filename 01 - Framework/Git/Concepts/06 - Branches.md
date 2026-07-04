# Branches

## What is a Branch?

A branch is a named reference (pointer) to a commit.

A branch does NOT contain:

- Files
- Folders
- Commits

A branch stores only the hash of a commit.

---

## Why do Branches exist?

Branches allow multiple lines of development.

Instead of modifying the same history, developers can work independently.

---

## Branch Structure

Example:

    HEAD
      │
      ▼
    master
      │
      ▼
    Commit C

master simply points to Commit C.

---

## Creating a Branch

Command:

    git branch experiment

Git creates:

    experiment

inside:

    .git/refs/heads/

Contents:

    Commit Hash

No files are copied.

No commits are created.

---

## Before Creating a Branch

    HEAD
      │
      ▼
    master
      │
      ▼
    Commit C

---

## After Creating a Branch

    HEAD
      │
      ▼
    master
      │
      ▼
    Commit C
      ▲
      │
 experiment

Both branches point to the same commit.

---

## Switching Branches

Command:

    git switch experiment

Git changes:

Before:

    HEAD
      │
      ▼
    master

After:

    HEAD
      │
      ▼
    experiment

The branch itself does not move.

Only HEAD changes.

Git then reconstructs the Working Directory from the commit pointed to by experiment.

---

## Making a Commit

Current state:

    HEAD
      │
      ▼
 experiment
      │
      ▼
    Commit C

After:

    git commit

Git creates:

    Commit D

Graph becomes:

              master
                 │
                 ▼
Commit A → Commit B → Commit C → Commit D
                              ▲          ▲
                              │          │
                           (old)    experiment

More accurately:

              master
                 │
                 ▼
Commit A → Commit B → Commit C
                               │
                               ▼
                            Commit D
                               ▲
                               │
                          experiment

Only experiment moves.

master still points to Commit C.

---

## Switching Back

Command:

    git switch master

HEAD changes:

    HEAD
      │
      ▼
    master

Git reconstructs the Working Directory from Commit C.

Your files become exactly as they were in Commit C.

Nothing from Commit D is lost.

---

## Important Notes

- A branch is only a pointer.
- Creating a branch creates no commits.
- Creating a branch copies no files.
- Switching branches changes HEAD.
- Committing moves only the current branch.
- Branches make commits reachable.

---

## Mental Model

HEAD
 │
 ▼
Branch
 │
 ▼
Commit
 │
 ▼
Tree
 │
 ▼
Blobs
