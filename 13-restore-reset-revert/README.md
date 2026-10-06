# Git Restore, Reset & Revert

## Overview

Git provides different commands for undoing changes depending on where the change currently exists.

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
```

The main undo commands covered in Day 5 are:

- `git restore`
- `git reset`
- `git revert`

---

# 1. Git Restore

`git restore` is mainly used to restore files in the working directory or remove files from the staging area.

## Discard Working Directory Changes

Suppose `app.py` was modified but not staged.

Check:

```bash
git status
```

Discard the modification:

```bash
git restore app.py
```

This restores the file from the index/staging area.

---

## Unstage a File

Suppose:

```bash
git add app.py
```

Now `app.py` is staged.

To remove it from staging:

```bash
git restore --staged app.py
```

The file changes are still present in the working directory.

```text
Before:

Working → Staging

After:

git restore --staged app.py

Working Directory
      ↑
      |
changes remain here
```

---

## Completely Discard a Staged Change

If a file is staged and I want to discard the change completely:

```bash
git restore --staged app.py
git restore app.py
```

First:

```text
Staging → Working Directory
```

Then:

```text
Working change → discarded
```

---

# 2. Git Reset

`git reset` can move `HEAD` and the current branch pointer to another commit.

The three important reset modes are:

```text
--soft
--mixed
--hard
```

---

# HEAD~1

Before understanding reset, understand:

```bash
HEAD~1
```

`HEAD` represents the currently checked-out commit.

```text
A → B → C
        ↑
       HEAD
```

Then:

```text
HEAD~1 = B
HEAD~2 = A
```

So:

```bash
git reset --soft HEAD~1
```

means:

> Move the current branch back by one commit.

---

# 3. Soft Reset

Command:

```bash
git reset --soft HEAD~1
```

Suppose:

```text
A → B → C
        ↑
       HEAD
```

After:

```bash
git reset --soft HEAD~1
```

the branch moves back to:

```text
A → B
    ↑
   HEAD
```

But the changes from C remain **staged**.

```text
Commit C
   |
   | reset --soft
   v
Staging Area
```

### Easy Memory

```text
SOFT = remove commit
       keep changes staged
```

Useful when:

> I committed too early and want to modify/recommit the changes.

---

# 4. Mixed Reset

Command:

```bash
git reset --mixed HEAD~1
```

or simply:

```bash
git reset HEAD~1
```

`--mixed` is the default reset mode.

Suppose:

```text
A → B → C
        ↑
       HEAD
```

After mixed reset:

```text
A → B
    ↑
   HEAD
```

The changes from C remain in the **working directory**, but they are no longer staged.

```text
Commit C
   |
   | reset --mixed
   v
Working Directory
```

### Easy Memory

```text
MIXED = remove commit
        unstage changes
        keep changes in working directory
```

---

# 5. Hard Reset

Command:

```bash
git reset --hard HEAD~1
```

This moves the branch backward and also discards tracked working-directory and staged changes associated with the reset.

```text
A → B → C
        ↑
       HEAD
```

After:

```bash
git reset --hard HEAD~1
```

```text
A → B
    ↑
   HEAD
```

The changes from C are discarded from the working tree.

### Easy Memory

```text
HARD = remove commit
       remove staged changes
       remove tracked working changes
```

> ⚠️ Use `git reset --hard` carefully because work can be lost.

---

# Soft vs Mixed vs Hard

| Reset Mode | Commit Removed | Changes Staged | Changes in Working Directory |
| ---------- | -------------- | -------------- | ---------------------------- |
| `--soft`   | Yes            | Yes            | Yes                          |
| `--mixed`  | Yes            | No             | Yes                          |
| `--hard`   | Yes            | No             | No                           |

Easy memory:

```text
SOFT
Commit → Staging

MIXED
Commit → Working Directory

HARD
Commit → Discard
```

---

# 6. Git Revert

`git revert` is different from `git reset`.

Command:

```bash
git revert <commit-id>
```

Instead of removing existing history, Git creates a **new commit** that reverses the selected commit's changes.

Suppose:

```text
A → B → C
```

If B introduced an unwanted change:

```bash
git revert <commit-id-of-B>
```

Git creates another commit:

```text
A → B → C → R
            ↑
       revert of B
```

The original B commit still exists in history.

---

# Reset vs Revert

## Reset

```text
A → B → C
```

Run:

```bash
git reset --hard HEAD~1
```

Result:

```text
A → B
```

The branch pointer moved backward.

---

## Revert

Starting history:

```text
A → B → C
```

Run:

```bash
git revert <commit-id>
```

Result:

```text
A → B → C → R
```

History is preserved.

---

# When Should I Use Reset or Revert?

### Local / Unpublished History

Reset can be useful when commits have not been shared and I want to rewrite my local history.

Example:

```bash
git reset --soft HEAD~1
```

---

### Shared / Published History

For a shared branch such as `main`, revert is generally safer because it preserves history.

```bash
git revert <commit-id>
```

Then:

```bash
git push
```

This allows other team members to see both:

```text
Original commit
+
Revert commit
```

---

# Restore vs Reset vs Revert

| Command                       | Main Purpose                                        |
| ----------------------------- | --------------------------------------------------- |
| `git restore app.py`          | Discard working-file changes                        |
| `git restore --staged app.py` | Unstage a file                                      |
| `git reset --soft HEAD~1`     | Undo commit, keep changes staged                    |
| `git reset --mixed HEAD~1`    | Undo commit, keep changes unstaged                  |
| `git reset --hard HEAD~1`     | Move back and discard tracked changes               |
| `git revert <commit-id>`      | Create a new commit that reverses an earlier commit |

---

# Troubleshooting Scenarios

## Scenario 1 - Accidentally Modified a File

I changed:

```text
app.py
```

but I do not want the changes.

Check:

```bash
git diff
```

Then:

```bash
git restore app.py
```

---

## Scenario 2 - Accidentally Staged a File

I ran:

```bash
git add app.py
```

but I do not want it staged.

Use:

```bash
git restore --staged app.py
```

The code change remains in the working directory.

---

## Scenario 3 - Committed Too Early

I created a commit but want to modify it again.

```bash
git reset --soft HEAD~1
```

The commit is undone, but the changes remain staged.

---

## Scenario 4 - Undo Commit and Rework Files

I want to remove the latest local commit and continue editing the files.

```bash
git reset --mixed HEAD~1
```

The changes return to the working directory.

---

## Scenario 5 - Completely Remove Latest Local Commit

If I intentionally want to discard the latest commit and its tracked changes:

```bash
git reset --hard HEAD~1
```

Use this carefully.

---

## Scenario 6 - Bad Commit Already Shared

Suppose a bad change has already been pushed to a shared `main` branch.

Instead of rewriting shared history with reset:

```bash
git revert <bad-commit-id>
```

Then:

```bash
git push
```

This preserves the project history.

---

# DevOps Example

Suppose a Terraform configuration change was committed:

```text
A → B → C
```

Commit C contains an incorrect infrastructure change.

### If C is still local

I could use:

```bash
git reset --soft HEAD~1
```

fix the Terraform file, then recommit.

### If C is already shared

I would generally prefer:

```bash
git revert <commit-id-of-C>
```

This creates a new commit reversing the bad infrastructure change while preserving the audit trail.

---

# Important Safety Note

Be especially careful with:

```bash
git reset --hard
```

and with rewriting commits that have already been pushed to shared branches.

For collaborative repositories, preserving history with `git revert` is often safer than rewriting shared history.

---

# Quick Revision

```text
RESTORE
File-level undo / unstage

SOFT RESET
Undo commit
Keep staged

MIXED RESET
Undo commit
Keep unstaged

HARD RESET
Undo commit
Discard tracked changes

REVERT
Undo through a new commit
Preserve history
```

---

# Key Takeaways

- `git restore` works mainly with working-directory and staged file changes.
- `git restore --staged` removes a file from staging without deleting its modification.
- `HEAD~1` means one commit before the current `HEAD`.
- `git reset --soft` keeps changes staged.
- `git reset --mixed` keeps changes unstaged.
- `git reset --hard` discards tracked changes and must be used carefully.
- `git revert` creates a new undo commit.
- Prefer `git revert` for shared/published history when history should be preserved.
