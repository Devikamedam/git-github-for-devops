# Git & GitHub — Quick Revision & Troubleshooting Guide

Quick revision notes for Git and GitHub concepts covered through **Day 4**, organized for Cloud/DevOps practice and interview preparation.

---

# 1. Git Core Concepts

| Concept           | Meaning                                | Remember                         |
| ----------------- | -------------------------------------- | -------------------------------- |
| Git               | Distributed version control system     | Tracks code changes and history  |
| GitHub            | Remote Git repository hosting platform | Collaboration and remote storage |
| Repository        | Project tracked by Git                 | Contains files + Git history     |
| Local Repository  | Repository on your machine             | Where commits are created        |
| Remote Repository | Repository hosted remotely             | Example: GitHub                  |
| Commit            | Saved point in Git history             | Has a unique commit ID           |
| Branch            | Independent line of development        | Used to isolate changes          |
| HEAD              | Current Git position                   | Usually points to current branch |
| `main`            | Local main branch                      | Local development reference      |
| `origin`          | Conventional remote name               | Usually points to GitHub repo    |
| `origin/main`     | Remote-tracking reference              | Local record of remote `main`    |

---

# 2. Git Workflow

| Stage                | Command                   | Purpose                          |
| -------------------- | ------------------------- | -------------------------------- |
| Working Directory    | `git status`              | Check current changes            |
| Working → Staging    | `git add <file>`          | Stage one file                   |
| Working → Staging    | `git add .`               | Stage current changes            |
| Staging → Local Repo | `git commit -m "message"` | Create local commit              |
| Local → Remote       | `git push`                | Push commits to GitHub           |
| Remote → Local       | `git pull`                | Fetch + integrate remote changes |

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
GitHub
```

### Easy Memory

```text
WORK → ADD → COMMIT → PUSH
```

---

# 3. Repository Commands

| Command                | Purpose                                    |
| ---------------------- | ------------------------------------------ |
| `git clone <url>`      | Clone remote repository locally            |
| `git status`           | Check repository status                    |
| `git log`              | View commit history                        |
| `git remote -v`        | Check configured remote URLs               |
| `git pull`             | Get and integrate remote changes           |
| `git fetch`            | Get remote information without integrating |
| `git push`             | Push local commits                         |
| `git push origin main` | Push local `main` to `origin`              |

---

# 4. Clone vs Pull vs Fetch

| Command           | When to Use                             | Result                             |
| ----------------- | --------------------------------------- | ---------------------------------- |
| `git clone <url>` | First time getting repository           | Creates local repository           |
| `git pull`        | Repo already exists locally             | Fetches + integrates changes       |
| `git fetch`       | Want remote updates without integration | Updates remote-tracking references |

```text
First time
↓
git clone

Already have repository
↓
git pull

Only inspect remote updates
↓
git fetch
```

---

# 5. Remote Commands

| Command                           | Purpose                           |
| --------------------------------- | --------------------------------- |
| `git remote -v`                   | Display configured remotes        |
| `git remote add origin <url>`     | Add remote called `origin`        |
| `git remote remove origin`        | Remove local remote configuration |
| `git remote set-url origin <url>` | Change remote URL                 |
| `git push origin main`            | Push `main` to `origin`           |
| `git push origin dev`             | Push `dev` to `origin`            |

> Removing `origin` does **not** delete the GitHub repository.

---

# 6. GitHub Authentication

| Method | How It Works              | Important                               |
| ------ | ------------------------- | --------------------------------------- |
| HTTPS  | Uses HTTPS repository URL | Requires appropriate GitHub credentials |
| SSH    | Uses SSH key pair         | Public key goes to GitHub               |
| PAT    | Personal Access Token     | Treat as a secret                       |

### SSH

```bash
ssh-keygen
```

```text
Local Machine                    GitHub
-------------                    ------
Private Key                      Public Key
     |                               |
     +------- Authentication --------+
```

### Never Commit

```text
Private SSH Keys
PATs
Passwords
AWS Access Keys
Application Secrets
```

---

# 7. Git History & HEAD

| Concept / Command          | Purpose                         |
| -------------------------- | ------------------------------- |
| `git log`                  | View commit history             |
| Commit ID                  | Unique identifier for a commit  |
| `HEAD`                     | Current Git position            |
| `main`                     | Local main branch               |
| `origin/main`              | Remote-tracking reference       |
| `origin/HEAD`              | Remote default-branch reference |
| `git checkout <commit-id>` | Inspect specific commit         |
| `git checkout main`        | Return to main                  |

### Normal HEAD

```text
HEAD
 ↓
main
 ↓
Latest Commit
```

### Detached HEAD

```bash
git checkout <commit-id>
```

```text
HEAD
 ↓
Specific Commit
```

Return:

```bash
git checkout main
```

---

# 8. Branch Commands

| Command               | Purpose                   |
| --------------------- | ------------------------- |
| `git branch`          | List local branches       |
| `git branch dev`      | Create `dev` branch       |
| `git checkout dev`    | Switch to `dev`           |
| `git checkout -b dev` | Create + switch to `dev`  |
| `git checkout main`   | Switch to main            |
| `git push origin dev` | Push dev branch to GitHub |

### Branch Workflow

```text
main
 ↓
Create Branch
 ↓
Make Changes
 ↓
git add
 ↓
git commit
 ↓
git push
 ↓
Pull Request
```

---

# 9. Merge vs Rebase

| Feature         | Merge                    | Rebase                            |
| --------------- | ------------------------ | --------------------------------- |
| Purpose         | Combine histories        | Replay commits                    |
| History         | Preserves branch history | More linear-looking               |
| Commit IDs      | Existing commits remain  | Replayed commits get new IDs      |
| Merge commit    | May create one           | Does not create that merge commit |
| Command learned | `git pull --no-rebase`   | `git pull --rebase`               |

### Merge

```text
        C
       / \
A --- B   M
       \ /
        D
```

### Rebase

Before:

```text
        C
       /
A --- B
       \
        D
```

After:

```text
A --- B --- C --- D'
```

Remember:

```text
D ≠ D'
```

Rebase recreated the commit, so the commit ID changes.

---

# 10. Git Diff

| Command                     | Comparison                          |
| --------------------------- | ----------------------------------- |
| `git diff`                  | Working tree vs staging area        |
| `git diff --staged`         | Staging area vs HEAD                |
| `git diff main dev`         | `main` vs `dev`                     |
| `git diff main origin/main` | Local main vs remote-tracking main  |
| `git fetch`                 | Refresh remote-tracking information |

### Before Commit

```bash
git status
git diff
git add .
git diff --staged
git commit -m "Update configuration"
```

---

# 11. Pull Request & Fork

| Concept      | Purpose                                |
| ------------ | -------------------------------------- |
| Branch       | Separate development line              |
| Pull Request | Propose changes for review/integration |
| Review       | Team checks proposed changes           |
| Merge        | Integrate branch histories             |
| Fork         | GitHub-side copy of another repository |
| Clone        | Local copy of repository               |

### PR Workflow

```text
Branch
 ↓
Changes
 ↓
Commit
 ↓
Push
 ↓
Pull Request
 ↓
Review
 ↓
Merge
 ↓
main
```

### Fork vs Clone

```text
Fork
GitHub → GitHub

Clone
GitHub → Local Machine
```

---

# Git Troubleshooting

## Scenario 1 — `git push` Rejected

### Problem

```text
Remote:
A --- B --- C

Local:
A --- B --- D
```

You run:

```bash
git push
```

and Git rejects the push.

### Cause

The remote contains commits that your local branch does not have.

### Check

```bash
git status
git fetch
```

### Solution

Integrate the remote changes.

Merge approach:

```bash
git pull --no-rebase
```

or rebase approach:

```bash
git pull --rebase
```

After successful integration:

```bash
git push
```

### Remember

```text
Push Rejection
≠
Merge Conflict
```

---

# Scenario 2 — Merge Conflict

### Problem

Developer 1:

```text
server_port = 8080
```

Developer 2:

```text
server_port = 9000
```

Git cannot determine which change is correct.

### Conflict May Look Like

```text
<<<<<<< HEAD
server_port = 9000
=======
server_port = 8080
>>>>>>> incoming-change
```

### Troubleshooting

Check:

```bash
git status
```

Open the conflicted file.

Choose/edit the correct final value.

Remove:

```text
<<<<<<<
=======
>>>>>>>
```

Stage the resolved file:

```bash
git add <filename>
```

If merging:

```bash
git commit
git push
```

If rebasing:

```bash
git rebase --continue
```

### Flow

```text
Conflict
 ↓
git status
 ↓
Open File
 ↓
Resolve Content
 ↓
git add
 ↓
Complete Merge/Rebase
 ↓
git push
```

---

# Scenario 3 — Same File Changed by Two Developers

### Situation A

Developer 1 changes Line 1.

Developer 2 changes Line 10.

```text
Same File
+
Different Non-Overlapping Lines
↓
Usually Git merges automatically
```

### Situation B

Both developers modify the same/overlapping content differently.

```text
Same Content
+
Different Changes
↓
Possible Conflict
```

Important:

> Same file does not automatically mean conflict.

---

# Scenario 4 — Authentication Failure During Push

### Problem

```bash
git push
```

fails with an authentication/permission error.

### Step 1

Check remote:

```bash
git remote -v
```

### Step 2

Determine authentication type.

```text
https://github.com/...
↓
HTTPS

git@github.com:...
↓
SSH
```

### For SSH

Check:

```text
SSH key exists
↓
Public key added to GitHub
↓
Correct GitHub account/repository permission
```

### For HTTPS

Check:

```text
GitHub credentials
↓
Token/credential access
↓
Repository permission
```

Never solve authentication problems by committing credentials into the repository.

---

# Scenario 5 — Wrong Remote Repository

### Problem

You cloned or configured the wrong GitHub remote.

### Check

```bash
git remote -v
```

### Fix

```bash
git remote set-url origin <correct-url>
```

Verify:

```bash
git remote -v
```

---

# Scenario 6 — `origin` Already Exists

### Problem

You try:

```bash
git remote add origin <url>
```

but `origin` is already configured.

### Check

```bash
git remote -v
```

### Fix Existing URL

```bash
git remote set-url origin <correct-url>
```

Instead of adding another `origin`.

---

# Scenario 7 — Local Branch Ahead of Remote

Example:

```text
origin/main
     |
     v
A --- B
      \
       C
       ↑
      main
      HEAD
```

### Meaning

You have a local commit that has not been pushed.

### Check

```bash
git status
```

### Action

If the commit is ready:

```bash
git push
```

---

# Scenario 8 — Remote Has Changes You Don't Have

### Check

```bash
git fetch
```

Then:

```bash
git diff main origin/main
```

This allows you to inspect differences before integrating them.

Then choose the appropriate integration workflow.

---

# Scenario 9 — Accidentally Checked Out a Commit

You ran:

```bash
git checkout <commit-id>
```

Now Git reports detached HEAD.

### Meaning

```text
HEAD
 ↓
Commit
```

instead of:

```text
HEAD
 ↓
Branch
```

### Return to Main

```bash
git checkout main
```

---

# Scenario 10 — Branch Exists Locally but Not on GitHub

### Problem

You created:

```bash
git checkout -b dev
```

but don't see `dev` on GitHub.

### Why?

Creating a branch locally does not automatically create it remotely.

### Solution

```bash
git push origin dev
```

---

# Scenario 11 — Changes Not Showing in `git diff`

You modified a file and then ran:

```bash
git add <file>
```

Now:

```bash
git diff
```

doesn't show that staged change.

### Why?

`git diff` normally shows unstaged tracked-file changes.

### Check Staged Changes

```bash
git diff --staged
```

Remember:

```text
git diff
↓
Unstaged Changes

git diff --staged
↓
Staged Changes
```

---

# Scenario 12 — Pull Request Has Conflict

```text
feature branch
      \
       \
        X ← Conflict
       /
      /
    main
```

### Cause

Both branches contain changes Git cannot automatically reconcile.

### Resolution

```text
Update branch
↓
Resolve conflicting files
↓
Stage resolved files
↓
Complete merge/rebase
↓
Push updated branch
↓
PR becomes mergeable
```

---

# DevOps Troubleshooting Scenario

Suppose two engineers modify:

```text
terraform/main.tf
```

Engineer 1:

```hcl
instance_type = "t3.small"
```

Engineer 2:

```hcl
instance_type = "t3.medium"
```

Git reports a conflict.

Do **not** randomly choose one.

First determine which infrastructure configuration is actually required.

Then:

```bash
git status
```

Edit:

```text
terraform/main.tf
```

Stage:

```bash
git add terraform/main.tf
```

Complete the merge/rebase and review:

```bash
git diff --staged
```

Then commit/push as appropriate.

This is important in DevOps because resolving the Git conflict incorrectly could result in the wrong infrastructure configuration being deployed.

---

# Fast Command Cheat Sheet

| I Want To...          | Command                           |
| --------------------- | --------------------------------- |
| Check status          | `git status`                      |
| Stage one file        | `git add <file>`                  |
| Stage current changes | `git add .`                       |
| Commit                | `git commit -m "message"`         |
| Push                  | `git push`                        |
| Clone                 | `git clone <url>`                 |
| Pull                  | `git pull`                        |
| Fetch only            | `git fetch`                       |
| View history          | `git log`                         |
| List branches         | `git branch`                      |
| Create branch         | `git branch dev`                  |
| Switch branch         | `git checkout dev`                |
| Create + switch       | `git checkout -b dev`             |
| Push branch           | `git push origin dev`             |
| Check remote          | `git remote -v`                   |
| Change remote         | `git remote set-url origin <url>` |
| Unstaged diff         | `git diff`                        |
| Staged diff           | `git diff --staged`               |
| Compare branches      | `git diff main dev`               |
| Merge-style pull      | `git pull --no-rebase`            |
| Rebase-style pull     | `git pull --rebase`               |
| Continue rebase       | `git rebase --continue`           |
| Return to main        | `git checkout main`               |

---

# Final Troubleshooting Memory Map

```text
                    GIT PROBLEM
                        |
          +-------------+-------------+
          |             |             |
          v             v             v
     Push Failed     Conflict      Auth Failed
          |             |             |
          v             v             v
   Check Remote     git status    git remote -v
          |             |             |
          v             v             v
 Fetch / Pull       Edit File     HTTPS or SSH?
          |             |             |
          v             v             v
 Merge/Rebase       git add       Fix Credentials/
          |             |          SSH Access
          v             v
        Push       Continue
                       |
                       v
                      Push
```

---

## Current Scope

This guide covers the Git and GitHub concepts learned through **Day 4**.

It is designed for:

- Quick revision
- Hands-on Git practice
- Cloud/DevOps troubleshooting
- Scenario-based interview preparation
