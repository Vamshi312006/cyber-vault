# Repository

## What is a Repository?

A repository is a project managed by Git.

A Git repository consists of:

- Working Directory
- .git Directory

Example:

    student/
    │
    ├── .git/
    ├── marks.txt
    ├── info.txt
    └── notes.txt

---

## Components

### Working Directory

The Working Directory contains the files you actively edit.

Example:

    marks.txt
    info.txt
    notes.txt

Changes made here are not automatically stored by Git.

---

### .git Directory

The .git directory is Git's internal database.

It stores:

- Git Objects
- Commit History
- Branch References
- Configuration
- Logs
- Repository Metadata

Deleting the .git directory removes the Git repository but leaves your project files untouched.

---

## Important Internal Directories

### objects/

Stores Git objects.

Objects include:

- Blob
- Tree
- Commit

Example:

    .git/
        └── objects/

---

### refs/

Stores references (pointers).

Example:

    refs/
        ├── heads/
        └── tags/

Branches are stored inside:

    refs/heads/

---

### HEAD

HEAD represents the current location in the repository.

Normally:

    HEAD
      │
      ▼
    master

Later we'll learn detached HEAD.

---

### config

Stores repository configuration.

Examples:

- username
- email
- remotes
- aliases

---

### logs/

Stores reference logs.

Used by commands like:

    git reflog

---

## Repository Initialization

Command:

    git init

Purpose:

Creates the hidden .git directory.

Before:

    student/

        marks.txt

After:

    student/

        .git/
        marks.txt

No commits are created.

No files are modified.

Only the repository database is initialized.

---

## Repository Workflow

    Working Directory
            │
            ▼
         git add
            │
            ▼
      Blob Objects
            │
            ▼
       Tree Object
            │
            ▼
      Commit Object
            │
            ▼
     Repository History

---

## Important Notes

- The Working Directory contains your current files.
- The .git directory contains Git's database.
- Git never modifies previous commits.
- Objects inside .git are immutable.
- The repository history is built from Commit objects linked by Parent references.

