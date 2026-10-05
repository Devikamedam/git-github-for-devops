# Git Diff

## Overview

`git diff` is used to compare changes in a Git repository.

It helps us understand:

```text
What changed?
Which lines changed?
What was added?
What was removed?
How are two branches different?
How is local main different from origin/main?
```

This is especially useful before staging, committing, merging, or reviewing changes.

---

# 1. Basic `git diff`

Command:

```bash
git diff
```

By default, this shows changes in tracked files that are in the **working directory but not yet staged**.

Conceptually:

```text
Working Directory
       |
       | git diff
       v
Staging Area
```

More precisely:

```text
Working Tree
     VS
Staging Area / Index
```

---

# 2. Example

Suppose a tracked file originally contains:

```text
server_port = 80
```

You change it to:

```text
server_port = 8080
```

Before staging:

```bash
git diff
```

Git may show something similar to:

```diff
-server_port = 80
+server_port = 8080
```

Meaning:

```text
-  Removed/old line
+  Added/new line
```

---

# 3. After `git add`

Suppose you run:

```bash
git add config.txt
```

Now the change is staged.

If you run:

```bash
git diff
```

the staged change will normally no longer appear in the default diff.

Why?

Because:

```text
git diff
```

compares:

```text
Working Directory
       VS
Staging Area
```

and those versions now match for that staged change.

---

# 4. View Staged Changes

To inspect changes that have already been staged:

```bash
git diff --staged
```

Conceptually:

```text
Local Repository / HEAD
          |
          | git diff --staged
          v
     Staging Area
```

This answers:

```text
What changes am I about to commit?
```

This is very useful before:

```bash
git commit
```

---

# 5. Working vs Staged vs Committed

Remember the Git workflow:

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
```

Different comparisons can be made at different points.

```text
git diff
↓
Working Directory vs Staging Area

git diff --staged
↓
Staging Area vs HEAD
```

---

# 6. Compare Branches

Git can also compare branches.

Example:

```bash
git diff main dev
```

This shows differences between the two specified branch tips.

Conceptually:

```text
main
 |
 A --- B
      \
       C --- D
            |
           dev
```

Run:

```bash
git diff main dev
```

to inspect differences between `main` and `dev`.

---

# 7. Compare Current Work with Another Branch

You may also see:

```bash
git diff <branch-name>
```

Example:

```bash
git diff main
```

This compares the current working-tree state against the specified branch according to Git's diff rules.

For clear branch-to-branch comparison, it is often easier to specify both references:

```bash
git diff main dev
```

---

# 8. Compare Local Main and `origin/main`

Use:

```bash
git diff main origin/main
```

This compares:

```text
Local main
    VS
origin/main
```

Remember:

```text
main
=
Local branch

origin/main
=
Local remote-tracking reference
for the remote main branch
```

Example:

```text
origin/main
     |
     v
A --- B --- C
             \
              D
              ^
              |
             main
```

The diff can help inspect how the two references differ.

---

# 9. Why `origin/main` May Be Outdated

`origin/main` is Git's **local remote-tracking reference**.

It reflects remote information from the last time Git fetched remote updates.

Therefore, if someone recently pushed to GitHub and you have not fetched those updates yet, your local `origin/main` may not reflect the newest remote state.

Conceptually:

```text
GitHub main
     |
     v
Newest Remote Commit

        git fetch
           ↓

Local origin/main
     |
     v
Updated Remote-Tracking Reference
```

---

# 10. `git fetch`

To retrieve updated remote information without immediately integrating it into your current branch:

```bash
git fetch
```

After fetching, comparisons such as:

```bash
git diff main origin/main
```

can use the updated remote-tracking reference.

---

# 11. `git diff` Before Commit

A useful workflow is:

```bash
git status
```

Then:

```bash
git diff
```

Review unstaged changes.

Stage them:

```bash
git add .
```

Then inspect what is staged:

```bash
git diff --staged
```

If everything looks correct:

```bash
git commit -m "Update configuration"
```

Flow:

```text
Modify File
    ↓
git status
    ↓
git diff
    ↓
Review Changes
    ↓
git add
    ↓
git diff --staged
    ↓
Review Staged Changes
    ↓
git commit
```

---

# 12. DevOps Example

Suppose a DevOps engineer modifies:

```text
terraform/main.tf
```

Before staging:

```bash
git diff
```

The engineer can review exactly what changed in the Terraform configuration.

Then:

```bash
git add terraform/main.tf
```

Review staged changes:

```bash
git diff --staged
```

Commit:

```bash
git commit -m "Update EC2 configuration"
```

This can help prevent accidental infrastructure changes from being committed.

---

# 13. Branch Comparison Example

Suppose:

```text
main
 |
 +-------------------+
                     |
                     v
               update-ec2
```

The engineer wants to review differences before integration.

Use:

```bash
git diff main update-ec2
```

This helps identify what changed between the branch tips.

---

# 14. Git Diff and Pull Requests

`git diff` and Pull Requests are related to code review.

Locally:

```bash
git diff
```

helps developers inspect changes.

On GitHub:

```text
Pull Request
     ↓
Changed Files
     ↓
Code Review
```

Both help answer:

```text
What changed before this code is integrated?
```

---

# Quick Revision

| Command                     | Purpose                                                     |
| --------------------------- | ----------------------------------------------------------- |
| `git diff`                  | Shows unstaged tracked-file changes                         |
| `git diff --staged`         | Shows staged changes compared with `HEAD`                   |
| `git diff main dev`         | Compares the specified branch tips                          |
| `git diff main origin/main` | Compares local `main` with its remote-tracking reference    |
| `git fetch`                 | Retrieves updated remote information without integrating it |

---

# Interview Scenario

## You modified a file but have not run `git add`. How do you inspect the change?

```bash
git diff
```

---

## You already ran `git add`. How do you inspect what will be committed?

```bash
git diff --staged
```

---

## How do you compare two branches?

For example:

```bash
git diff main dev
```

---

## How do you compare local `main` with `origin/main`?

```bash
git diff main origin/main
```

If you need the latest remote information first:

```bash
git fetch
git diff main origin/main
```

---

# Key Takeaway

```text
git diff
↓
Unstaged Changes

git diff --staged
↓
Staged Changes

git diff main dev
↓
Branch Comparison

git fetch
↓
Update Remote-Tracking Information

git diff main origin/main
↓
Local vs Remote-Tracking Comparison
```

`git diff` is an important command for reviewing changes before committing, merging, or deploying code.
