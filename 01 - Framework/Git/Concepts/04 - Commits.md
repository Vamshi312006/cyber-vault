# Commits

## What is a Commit?

A commit is a Git object that records a specific state of the repository.

A commit does **not** directly store your files.

Instead, it stores:

- A pointer to a Tree object
- A pointer to its Parent Commit
- Author information
- Date & Time
- Commit Message

---

## Why do Commits exist?

Commits create the history of a repository.

Every commit represents one checkpoint in the project's evolution.

Example:

    Commit A
        │
        ▼
    Commit B
        │
        ▼
    Commit C

Each commit knows its parent, allowing Git to reconstruct the entire history.

---

## Commit Structure

    Commit
    │
    ├── Parent Commit
    ├── Tree
    ├── Author
    ├── Date
    └── Commit Message

A commit never stores file contents directly.

---

## Git Objects

Git stores information using objects.

The three primary objects are:

### Blob

Stores the contents of a single file.

Example:

    marks.txt

    Maths = 90
    English = 80

Blob contains only the file contents.

A Blob does NOT know:

- Filename
- Folder
- Project structure

---

### Tree

Represents a directory.

A Tree maps filenames to Blob objects.

Example:

    student/

    ├── marks.txt ───► Blob M
    ├── info.txt  ───► Blob I
    └── notes.txt ───► Blob N

The Tree stores:

- Filenames
- Folder hierarchy
- References to Blob objects

---

### Commit

Represents a version of the repository.

A Commit points to:

- Tree
- Parent Commit

and stores metadata.

---

## Commit Creation Process

Suppose the repository contains:

    student/

    ├── marks.txt
    ├── info.txt
    └── notes.txt

### Step 1

Modify:

    marks.txt

### Step 2

Run:

    git add marks.txt

Git creates a new Blob object containing the updated contents of marks.txt.

No commit exists yet.

### Step 3

Run:

    git commit -m "Updated Marks"

Git creates:

- A new Tree object
- A new Commit object

The new Tree points to:

    marks.txt ───► New Blob

    info.txt  ───► Existing Blob

    notes.txt ───► Existing Blob

Only the changed file receives a new Blob.

Unchanged files reuse existing Blob objects.

---

## Internal Representation

    Commit C2
        │
        ▼
      Tree T2
      │
      ├── marks.txt ───► Blob M2
      ├── info.txt  ───► Blob I1
      └── notes.txt ───► Blob N1

Parent:

    Commit C2
        │
        ▼
    Commit C1

---

## Retrieval

When Git checks out a commit:

1. Read the Commit object.
2. Follow the Tree pointer.
3. Read each Blob referenced by the Tree.
4. Reconstruct the Working Directory.

Example:

    Commit
        │
        ▼
      Tree
        │
        ▼
    Blob Objects
        │
        ▼
    Working Directory

---

## Important Notes

- Commits are immutable.
- Blob objects are immutable.
- Tree objects are immutable.
- New commits never overwrite previous commits.
- Only changed files receive new Blob objects.
- Unchanged files reuse existing Blob objects.

---

## Mental Model

    Working Directory
            │
            ▼
        git add
            │
            ▼
      Blob Object
            │
            ▼
      Tree Object
            │
            ▼
     Commit Object
            │
            ▼
     Repository History
