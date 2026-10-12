# Git & GitHub — Quick Revision & Troubleshooting Guide

Quick revision notes for Git and GitHub concepts covered through **Day 5**, organized for Cloud/DevOps practice and interview preparation.

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

---

# Day 5 — Git Fetch, Cherry-Pick, Restore, Reset & Revert

## 1. Fetch vs Pull

| Command                 | What it does                     | Changes current branch? |
| ----------------------- | -------------------------------- | ----------------------- |
| `git fetch origin`      | Downloads remote updates         | ❌ No                   |
| `git pull origin main`  | Fetches + integrates             | ✅ Yes                  |
| `git merge origin/main` | Integrates fetched `origin/main` | ✅ Yes                  |

### Safe inspection workflow

```bash
git fetch origin
git log --oneline main..origin/main
git diff main..origin/main
git merge origin/main
```

```text
GitHub
   ↓ fetch
origin/main
   ↓ merge
local main
```

---

## 2. Fast-Forward Merge

If local `main` has no separate commits and the remote is simply ahead:

```text
Local:
A → B

Remote:
A → B → C → D
```

After:

```bash
git fetch origin
git merge origin/main
```

Result:

```text
A → B → C → D
            ↑
           main
```

No separate merge commit is required.

---

## 3. Cherry-Pick

```bash
git cherry-pick <commit-id>
```

Use it when you need a **specific commit**, not the entire branch.

### Our Hands-On Scenario

```text
dev:
Base → A → B → C
       |    |    |
       |    |    └─ Division
       |    └────── Multiplication
       └─────────── Subtraction

             B
             │ cherry-pick
             ▼

test:
Base → B'
       │
       │ Pull Request
       ▼

main:
Base → B'
```

Our actual commits:

| Commit | Change         |
| ------ | -------------- |
| A      | Subtraction    |
| B      | Multiplication |
| C      | Division       |

Only **B — Multiplication** was required in `main`.

The original A, B and C remained on `dev`.

---

## 4. Cherry-Pick Conflict

If:

```bash
git cherry-pick <commit-id>
```

produces a conflict:

```text
Resolve file
    ↓
git add <file>
    ↓
git cherry-pick --continue
```

Commands:

```bash
git add app.py
git cherry-pick --continue
```

Cancel an active cherry-pick:

```bash
git cherry-pick --abort
```

### Important

Cherry-pick applies the **change introduced by a commit**.

If the selected commit depends on an earlier commit, cherry-picking it alone may cause a conflict or incomplete/broken code.

---

## 5. Restore

| Requirement                   | Command                       |
| ----------------------------- | ----------------------------- |
| Discard unstaged file changes | `git restore <file>`          |
| Remove file from staging      | `git restore --staged <file>` |

Example:

```bash
git restore app.py
```

Unstage:

```bash
git restore --staged app.py
```

---

## 6. HEAD~1

```text
A → B → C
        ↑
       HEAD
```

Therefore:

```text
HEAD~1 = B
HEAD~2 = A
```

---

## 7. Reset

| Command                    | Commit | Staging     | Working Changes |
| -------------------------- | ------ | ----------- | --------------- |
| `git reset --soft HEAD~1`  | Undo   | Keep staged | Keep            |
| `git reset --mixed HEAD~1` | Undo   | Unstage     | Keep            |
| `git reset --hard HEAD~1`  | Undo   | Remove      | Discard         |

### Easy Memory

```text
SOFT  → Commit → Staging
MIXED → Commit → Working Directory
HARD  → Commit → Discard
```

---

## 8. Revert

```bash
git revert <commit-id>
```

Revert creates a **new commit** that reverses an earlier commit.

```text
A → B → C
```

After reverting B:

```text
A → B → C → R
            ↑
       reverses B
```

The original commit remains in history.

---

## 9. Reset vs Revert

| Reset                                    | Revert                             |
| ---------------------------------------- | ---------------------------------- |
| Moves branch/HEAD backward               | Creates a new commit               |
| Can rewrite history                      | Preserves history                  |
| Useful for local/unpublished corrections | Safer for shared/published history |
| `git reset ...`                          | `git revert <commit-id>`           |

---

# Day 5 Troubleshooting Scenarios

| Problem                                           | Solution                                           |
| ------------------------------------------------- | -------------------------------------------------- |
| Want remote updates without changing local branch | `git fetch origin`                                 |
| Want to inspect remote commits                    | `git log --oneline main..origin/main`              |
| Need only one commit from another branch          | `git cherry-pick <commit-id>`                      |
| Cherry-pick conflict                              | Resolve → `git add` → `git cherry-pick --continue` |
| Want to cancel cherry-pick                        | `git cherry-pick --abort`                          |
| Accidentally modified file                        | `git restore <file>`                               |
| Accidentally staged file                          | `git restore --staged <file>`                      |
| Committed too early; keep staged                  | `git reset --soft HEAD~1`                          |
| Undo local commit; continue editing               | `git reset --mixed HEAD~1`                         |
| Discard latest local commit and tracked changes   | `git reset --hard HEAD~1`                          |
| Bad commit already shared                         | `git revert <commit-id>`                           |

---

# Day 5 Fast Revision

```text
FETCH
Download remote information
Does not automatically integrate

PULL
Fetch + integrate

FAST-FORWARD
Move branch pointer forward
No separate merge commit required

CHERRY-PICK
Apply selected commit

RESTORE
File-level undo / unstage

RESET --SOFT
Undo commit → keep staged

RESET --MIXED
Undo commit → keep unstaged

RESET --HARD
Undo commit → discard tracked changes

REVERT
Create new undo commit
Preserve history
```

---

## Current Scope

This guide covers the Git and GitHub concepts learned through **Day 5**.

It is designed for:

- Quick revision
- Hands-on Git practice
- Cloud/DevOps troubleshooting
- Scenario-based interview preparation

---

## Day 6 — Quick Revision

| Command / Concept                      | Purpose                                             |
| -------------------------------------- | --------------------------------------------------- |
| `.gitignore`                           | Ignore matching untracked files                     |
| `git check-ignore -v FILE`             | Check why a file is ignored                         |
| `git rm --cached FILE`                 | Stop tracking a file while keeping it locally       |
| `git stash`                            | Temporarily save tracked changes                    |
| `git stash -u`                         | Stash tracked changes and untracked files           |
| `git stash list`                       | List saved stashes                                  |
| `git stash apply`                      | Restore changes and keep stash entry                |
| `git stash pop`                        | Restore changes and remove stash on success         |
| `git stash drop`                       | Delete a selected stash                             |
| `git merge --abort`                    | Cancel an in-progress merge                         |
| `git merge --continue`                 | Continue after resolving merge conflicts            |
| `git rebase --continue`                | Continue a paused rebase                            |
| `git cherry-pick --continue`           | Continue a paused cherry-pick                       |
| `git diff --name-only --diff-filter=U` | List unresolved conflict files                      |
| `ssh-keygen -t ed25519`                | Generate an SSH key pair                            |
| `ssh -T git@github.com`                | Test GitHub SSH authentication                      |
| `git clone URL`                        | Clone a remote repository                           |
| Pull Request                           | Propose and review changes before merging           |
| Branch Ruleset                         | Enforce repository branch policies                  |
| GitLab Merge Request                   | GitLab's workflow for reviewing and merging changes |
| GitLab Runner                          | Execute GitLab CI/CD jobs                           |

### Important Interview Reminders

1. `.gitignore` does not automatically untrack committed files.
2. `git stash apply` keeps the stash; `git stash pop` removes it after successful application.
3. GitHub uses Pull Requests; GitLab uses Merge Requests.
4. Protect important branches using reviews and required checks.
5. Merge conflicts require resolving the actual code changes, not just removing markers.
6. SSH private keys must remain protected.
7. Production deployments should follow controlled CI/CD workflows.

**Hands-on status:** Pending weekend practice.
