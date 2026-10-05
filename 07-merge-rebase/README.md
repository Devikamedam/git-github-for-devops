# Git Merge & Rebase

## Overview

When multiple developers work on the same repository, the local branch and remote branch can have different commits.

Example:

```text
          C --- D   Local
         /
A --- B
         \
          E --- F   Remote
```

Now Git needs to integrate both histories.

Two important approaches are:

```text
MERGE
or
REBASE
```

---

# 1. Why Do Histories Diverge?

Suppose Developer 1 and Developer 2 both start from:

```text
A --- B
```

Developer 1 creates and pushes a new commit:

```text
A --- B --- C
```

At the same time, Developer 2 creates a local commit:

```text
A --- B --- D
```

Now the histories have diverged:

```text
        C   Remote
       /
A --- B
       \
        D   Local
```

Before Developer 2 can push normally, the remote work needs to be integrated.

---

# 2. Merge

A merge combines histories.

One way to pull remote changes using merge behavior is:

```bash
git pull --no-rebase
```

Conceptually:

```text
        C
       / \
A --- B   M
       \ /
        D
```

`M` represents a merge commit when a merge commit is required.

The history of both lines of development is preserved.

---

# 3. Merge Workflow

Example:

```bash
git pull --no-rebase
```

Git retrieves the remote changes and integrates them with the local branch using merge behavior.

After successful integration:

```bash
git push
```

Flow:

```text
Local Commit
      \
       \
        Merge
       /
Remote Commit
```

---

# 4. Rebase

Rebase takes local commits and replays them on top of the updated upstream history.

Example before rebase:

```text
        C   Remote
       /
A --- B
       \
        D   Local
```

After rebase:

```text
A --- B --- C --- D'
```

The original local commit:

```text
D
```

is replayed as a new commit:

```text
D'
```

Because the commit is recreated, its commit ID changes.

---

# 5. Pull Using Rebase

Use:

```bash
git pull --rebase
```

Conceptually:

```text
Before

        C
       /
A --- B
       \
        D

After

A --- B --- C --- D'
```

This produces a more linear history.

---

# 6. Merge vs Rebase

| Merge                                 | Rebase                                        |
| ------------------------------------- | --------------------------------------------- |
| Combines histories                    | Replays commits                               |
| Preserves the existing branch history | Creates new versions of replayed commits      |
| May create a merge commit             | Avoids that merge commit for the rebased work |
| History can show branch integration   | History can appear more linear                |
| Existing commit IDs are preserved     | Replayed commit IDs change                    |

---

# 7. Why Does Rebase Change Commit IDs?

A Git commit is identified by its hash.

When rebase moves and replays a commit onto a different parent, Git creates a new commit.

Example:

```text
Before:

A --- B
      \
       D
```

After rebasing onto `C`:

```text
A --- B --- C --- D'
```

`D'` is not exactly the same Git commit as `D`.

Therefore:

```text
Old commit ID != New commit ID
```

---

# 8. Merge History

Example:

```text
        C --------
       /           \
A --- B             M
       \           /
        D --------
```

This history shows that separate lines of development existed and were merged.

---

# 9. Rebase History

Example:

```text
A --- B --- C --- D'
```

This creates a linear-looking history.

---

# 10. Important Rebase Consideration

Rebase rewrites the history of the commits being replayed.

Because of this, avoid casually rebasing commits that other developers are already depending on in shared history.

For your own local commits that have not been shared, rebase can be useful for keeping history linear.

---

# 11. Push Rejected Scenario

Suppose:

```text
GitHub:
A --- B --- C

Local:
A --- B --- D
```

Trying:

```bash
git push
```

may be rejected because the remote contains work that the local branch does not contain.

One possible approach is merge:

```bash
git pull --no-rebase
```

or rebase:

```bash
git pull --rebase
```

After successful integration:

```bash
git push
```

---

# 12. Important: Divergence Does Not Always Mean Conflict

This is an important distinction.

```text
Diverged History
       ≠
Merge Conflict
```

Git may be able to integrate the changes automatically.

A conflict occurs when Git cannot automatically reconcile certain changes.

We cover conflicts in the next section.

---

# DevOps Example

Suppose two DevOps engineers modify a Terraform repository.

Engineer 1 pushes:

```text
Update security group
```

Engineer 2 has a local commit:

```text
Update EC2 configuration
```

The histories may look like:

```text
                 Security Group
                /
A --- B --------
                \
                 EC2 Configuration
```

Engineer 2 could integrate the remote history using:

```bash
git pull --rebase
```

Result:

```text
A --- B --- Security-Group --- EC2-Configuration'
```

Then:

```bash
git push
```

---

# Quick Revision

| Command / Concept      | Purpose                                                  |
| ---------------------- | -------------------------------------------------------- |
| `git pull --no-rebase` | Pulls remote changes and integrates using merge behavior |
| `git pull --rebase`    | Pulls remote changes and rebases local commits           |
| Merge                  | Combines separate histories                              |
| Rebase                 | Replays commits on top of another history                |
| Merge commit           | Commit created when Git records a non-fast-forward merge |
| Linear history         | Commit history without that merge commit                 |
| Divergence             | Local and remote histories contain different commits     |

---

# Interview Scenario

## What is the main difference between merge and rebase?

### Merge

```text
        C
       / \
A --- B   M
       \ /
        D
```

Merge combines the histories and may create a merge commit.

### Rebase

```text
A --- B --- C --- D'
```

Rebase replays commits on top of the updated history.

---

## Does rebase change commit IDs?

Yes, for the commits that are replayed.

```text
D → D'
```

The recreated commit has a new commit ID.

---

## Does a diverged branch always create a conflict?

No.

Git may integrate the changes automatically.

A conflict occurs only when Git cannot automatically reconcile the changes.

---

# Key Takeaway

```text
MERGE
↓
Combine histories
↓
May create merge commit
↓
Preserve existing commit history


REBASE
↓
Replay local commits
↓
New commit IDs
↓
Linear-looking history
```

Both solve history-integration problems differently.
