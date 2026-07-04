# HEAD

## What is HEAD?

HEAD is a special Git reference that represents your current position in the repository.

Whenever Git needs to know:

- Which commit am I viewing?
- Which branch am I on?
- Where should the next commit go?

It first looks at HEAD.

---

## Why does HEAD exist?

A repository may contain many branches and thousands of commits.

Git needs one reference that says:

    "This is where you are right now."

That reference is HEAD.

---

## Normal State

Usually HEAD points to a branch.

Example:

    HEAD
      │
      ▼
    master
      │
      ▼
    Commit C

The branch points to the latest commit.

HEAD points to the branch.

---

## Detached HEAD

HEAD can also point directly to a commit.

Example:

    HEAD
      │
      ▼
    Commit B

No branch is involved.

This is called a Detached HEAD.

---

## Why Detached HEAD?

Suppose history is:

    Commit A
        │
        ▼
    Commit B
        │
        ▼
    Commit C
          ▲
          │
        master

You run:

    git checkout <Commit B>

Git changes:

    HEAD
      │
      ▼
    Commit B

Now you're viewing an older version of the project.

---

## What happens to the Working Directory?

Git reconstructs your files from the commit pointed to by HEAD.

Example:

HEAD points to:

    Commit A

Working Directory becomes:

    README.md

    Version from Commit A

Later

HEAD points to:

    Commit C

Working Directory becomes:

    README.md

    Version from Commit C

Git rewrites the Working Directory automatically.

---

## Making a Commit

Normal state:

    HEAD
      │
      ▼
    master
      │
      ▼
    Commit C

After

    git commit

Git creates:

    Commit D

Then moves

    master

to

    Commit D

HEAD still points to master.

Result:

    HEAD
      │
      ▼
    master
      │
      ▼
    Commit D

---

## Commiting in Detached HEAD

Suppose:

    HEAD
      │
      ▼
    Commit B

You commit.

Git creates:

    Commit X

Result:

    Commit A
        │
        ▼
    Commit B
        │
        ▼
    Commit X
      ▲
      │
    HEAD

No branch points to Commit X.

If you switch away without creating a branch,
Commit X may become unreachable.

Git warns you about this.

---

## Important Notes

- HEAD always represents your current location.
- Normally HEAD points to a branch.
- In Detached HEAD, it points directly to a commit.
- Git reconstructs the Working Directory from the commit HEAD ultimately refers to.
- New commits are attached wherever HEAD is currently attached.

---

## Mental Model

Normal:

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
     Blob

Detached:

    HEAD
      │
      ▼
    Commit
      │
      ▼
     Tree
      │
      ▼
     Blob

