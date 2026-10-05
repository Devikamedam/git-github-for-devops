# Git Workflow

## Overview

Git uses different stages to track changes before they are finally pushed to GitHub.

The basic workflow is:

```text
Working Directory
       |
       | git add
       v
Staging Area
       |
       | git commit
       v
Local Repository
       |
       | git push
       v
Remote Repository
      GitHub
```

This workflow is one of the most important Git concepts to understand.

---

## 1. Working Directory

The working directory is where we create or modify project files.

Example:

```text
Project
├── app.py
├── README.md
└── config.txt
```

When we change `app.py`, the change initially exists only in the working directory.

Check the current status using:

```bash
git status
```

`git status` shows which files are modified, untracked, or staged.

---

## 2. Staging Area

Before creating a commit, we select which changes should be included.

To stage one file:

```bash
git add app.py
```

To stage all current changes:

```bash
git add .
```

Flow:

```text
Working Directory
       |
       | git add
       v
Staging Area
```

The staging area allows us to prepare changes before committing them.

---

## 3. Local Repository

After staging the changes, create a commit:

```bash
git commit -m "Update application"
```

Flow:

```text
Staging Area
     |
     | git commit
     v
Local Repository
```

The commit records the staged changes in the local Git history.

---

## 4. Remote Repository

The local commit is not automatically available on GitHub.

To send the committed changes to the remote repository:

```bash
git push
```

Flow:

```text
Local Repository
       |
       | git push
       v
Remote Repository
      GitHub
```

---

# Complete Git Workflow

Suppose we modify `app.py`.

First check the repository:

```bash
git status
```

Stage the file:

```bash
git add app.py
```

Check again:

```bash
git status
```

Commit the staged change:

```bash
git commit -m "Update app.py"
```

Push the commit:

```bash
git push
```

Complete flow:

```text
app.py changed
      |
      v
Working Directory
      |
      | git add app.py
      v
Staging Area
      |
      | git commit -m "Update app.py"
      v
Local Repository
      |
      | git push
      v
GitHub
Remote Repository
```

---

## `git status`

Command:

```bash
git status
```

Purpose:

Checks the current state of the working directory and staging area.

It helps identify:

- Untracked files
- Modified files
- Staged files
- Current branch
- Whether the local branch is ahead or behind its remote tracking branch

This is one of the most frequently used Git commands.

---

## `git add`

Stage one specific file:

```bash
git add app.py
```

Stage all current changes:

```bash
git add .
```

Purpose:

```text
Working Directory
        ↓
     git add
        ↓
Staging Area
```

`git add` does **not** create a commit and does **not** send anything to GitHub.

---

## `git commit`

Example:

```bash
git commit -m "Add login feature"
```

Purpose:

Records the staged changes in the local repository.

```text
Staging Area
     ↓
 git commit
     ↓
Local Repository
```

Each commit receives a unique commit ID.

---

## `git push`

Example:

```bash
git push
```

Purpose:

Sends local commits to the configured remote repository.

```text
Local Repository
       ↓
    git push
       ↓
GitHub
```

---

# Important Difference

These commands perform different jobs:

```text
git add
   ↓
Prepare changes

git commit
   ↓
Record changes locally

git push
   ↓
Send commits to remote repository
```

A common misunderstanding is:

```text
git add → GitHub
```

That is incorrect.

The actual flow is:

```text
git add
   ↓
Staging Area

git commit
   ↓
Local Repository

git push
   ↓
GitHub
```

---

# Example DevOps Workflow

Suppose a DevOps engineer modifies:

```text
terraform/main.tf
```

The workflow could be:

```bash
git status
git add terraform/main.tf
git commit -m "Update EC2 configuration"
git push
```

Architecture:

```text
Terraform Code
      |
      | modify
      v
Working Directory
      |
      | git add
      v
Staging Area
      |
      | git commit
      v
Local Git Repository
      |
      | git push
      v
GitHub Repository
```

The same workflow can be used for:

- Terraform files
- Dockerfiles
- Kubernetes manifests
- Shell scripts
- Application code
- CI/CD configuration

---

# Quick Revision

| Command                   | One-line Purpose                                        |
| ------------------------- | ------------------------------------------------------- |
| `git status`              | Shows the current state of files and repository changes |
| `git add <file>`          | Stages a specific file                                  |
| `git add .`               | Stages current changes under the working directory      |
| `git commit -m "message"` | Records staged changes in the local repository          |
| `git push`                | Sends local commits to the remote repository            |

---

# Interview Scenario

### You changed a file. Does GitHub immediately receive the change?

No.

The change first exists in the working directory.

```text
Change File
    ↓
git add
    ↓
Staging Area
    ↓
git commit
    ↓
Local Repository
    ↓
git push
    ↓
GitHub
```

---

## Key Takeaway

Remember:

```text
WORK
 ↓
ADD
 ↓
COMMIT
 ↓
PUSH
```

Or:

```text
Working Directory
      ↓
Staging Area
      ↓
Local Repository
      ↓
GitHub
```
