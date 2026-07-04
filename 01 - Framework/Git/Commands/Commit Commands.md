# Commit Commands

## git add

### Purpose

Stage changes from the Working Directory to the Staging Area.

### Syntax

    git add <file>
    git add .

### What it does

- Stages selected changes.
- Does **NOT** create a commit.

### Example

    git add README.md

### Notes

- Only staged changes are committed.
- You can stage one or many files.

---

## git commit

### Purpose

Create a new commit (snapshot) from the staged changes.

### Syntax

    git commit -m "Commit Message"

### Parameters

**-m**

Specifies the commit message.

### What it does

- Reads the Staging Area.
- Creates a new snapshot.
- Stores:
  - Author
  - Email
  - Date & Time
  - Commit Message

### Example

    git commit -m "Initial commit"

### Notes

- Only staged changes are committed.
- Commits are local.
- Does **NOT** upload anything to GitHub.

### Workflow

    Working Directory
            │
            ▼
        git add
            │
            ▼
     Staging Area
            │
            ▼
      git commit
            │
            ▼
    Repository History
