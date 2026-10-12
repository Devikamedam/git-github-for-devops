# Git Stash — Day 6

## 1. What Is Git Stash?

Git stash temporarily saves your uncommitted changes without creating a commit.

It allows you to switch branches or work on another task while keeping your unfinished work safe.

**Simple meaning:** Save your unfinished work temporarily and restore it later.

## 2. Why Do We Use Git Stash in DevOps?

Imagine you are developing a new feature on a feature branch.

Suddenly, your production team reports a critical issue.

You need to switch to the main branch to fix the problem, but your current changes are incomplete.

Instead of committing unfinished code, you can use Git stash.

### Real-World Scenario

You are working on:

```text
feature-login
```

Your manager asks you to fix a production bug immediately.

Workflow:

```text
Feature Branch
      |
      v
Uncommitted Changes
      |
      v
git stash
      |
      v
Switch to main
      |
      v
Create hotfix branch
      |
      v
Fix and Commit Bug
      |
      v
Return to Feature Branch
      |
      v
git stash pop
      |
      v
Continue Development
```

## 3. Important Git Stash Commands

| Command                       | Purpose                                        |
| ----------------------------- | ---------------------------------------------- |
| `git stash`                   | Temporarily save tracked changes               |
| `git stash push -m "message"` | Stash with a description                       |
| `git stash list`              | Display saved stashes                          |
| `git stash show`              | Show summary of latest stash                   |
| `git stash show -p`           | Show detailed changes                          |
| `git stash apply`             | Restore changes and keep stash                 |
| `git stash pop`               | Restore changes and remove stash if successful |
| `git stash drop`              | Delete a particular stash                      |
| `git stash clear`             | Delete all stash entries                       |
| `git stash -u`                | Include untracked files                        |
| `git stash -a`                | Include ignored and untracked files            |
| `git stash branch`            | Create a branch from a stash                   |

## 4. Git Stash Hands-On Example

### Step 1: Check the Current Branch

```bash
git branch --show-current
```

Expected output:

```text
feature-login
```

### Step 2: Modify an Existing File

Suppose `app.py` already exists and is tracked.

Add:

```python
print("Login feature development")
```

Check the status:

```bash
git status
```

Expected result:

```text
modified: app.py
```

### Step 3: Save Changes Temporarily

```bash
git stash push -m "Login feature work in progress"
```

Git saves your tracked modifications in a stash.

### Step 4: Verify the Working Directory

```bash
git status
```

Expected result, assuming no other changes:

```text
nothing to commit, working tree clean
```

### Step 5: View Saved Stashes

```bash
git stash list
```

Example:

```text
stash@{0}: On feature-login: Login feature work in progress
```

### Step 6: Switch to Another Branch

```bash
git switch main
```

Create a hotfix branch:

```bash
git switch -c hotfix-login
```

Fix the production issue, test it, commit it, and follow the team's pull request process.

### Step 7: Return to the Feature Branch

```bash
git switch feature-login
```

### Step 8: Restore Your Changes

```bash
git stash pop
```

Your unfinished changes are restored.

If applying the stash succeeds, Git removes that stash entry.

## 5. git stash apply vs git stash pop

### git stash apply

```bash
git stash apply
```

- Restores saved changes.
- Keeps the stash entry.
- Useful when you want to reuse the same stash.

### git stash pop

```bash
git stash pop
```

- Restores saved changes.
- Removes the stash entry after successful application.
- Useful when you no longer need the saved stash.

**Important:** Stash application can produce conflicts. If `pop` encounters conflicts, Git normally retains the stash entry.

## 6. How to Stash Untracked Files

By default, Git stash does not include untracked files.

To include them:

```bash
git stash push -u -m "Save tracked and untracked work"
```

To include ignored files as well:

```bash
git stash push -a -m "Save all local work"
```

Use `-a` carefully because it can include large ignored folders and local files containing secrets.

## 7. How to Restore a Specific Stash

First, check:

```bash
git stash list
```

Example:

```text
stash@{0}: On feature-login: Login feature
stash@{1}: On feature-cart: Cart feature
```

Restore a specific stash:

```bash
git stash apply 'stash@{1}'
```

The quotes are particularly useful in PowerShell.

## 8. How to Delete a Stash

Delete one stash:

```bash
git stash drop 'stash@{0}'
```

Delete all stashes:

```bash
git stash clear
```

**Warning:** `git stash clear` removes all stash entries. Do not run it unless you are certain you no longer need them.

## 9. Create a New Branch From a Stash

```bash
git stash branch recovered-feature 'stash@{0}'
```

Git creates a branch based on the original stash context and attempts to apply the saved changes.

This is useful when the original branch has changed significantly.

## 10. Git Stash vs Git Commit

| Git Stash                          | Git Commit                           |
| ---------------------------------- | ------------------------------------ |
| Temporarily saves changes          | Records changes in branch history    |
| Usually used for unfinished work   | Used for meaningful checkpoints      |
| Not automatically pushed to GitHub | Can be pushed to a remote repository |
| Local to the repository            | Part of Git history                  |
| Can be applied or dropped          | Can be shared and reviewed           |

## 11. Real-Time DevOps Troubleshooting Scenario

**Scenario:** You are modifying Terraform configuration files when an urgent application deployment issue occurs.

**Problem:** Your Terraform changes are incomplete, and you need to switch branches.

**Solution:**

1. Check `git status`.
2. Save unfinished changes using `git stash push`.
3. Switch to the appropriate branch.
4. Create a hotfix branch.
5. Fix, test, and commit the issue.
6. Return to your original branch.
7. Restore the stash.
8. Continue development.

**Security Note:** Do not treat Git stash as a secure storage location for credentials. Stashed changes are stored in the local Git repository.

## 12. Interview Questions

### Q1. What is Git stash?

Git stash temporarily saves uncommitted changes so developers can work on another task without creating a commit.

### Q2. What is the difference between stash apply and stash pop?

`apply` restores changes while retaining the stash entry.

`pop` restores changes and removes the entry if application succeeds.

### Q3. Does Git stash save untracked files?

Not by default. Use `git stash -u` to include untracked files.

### Q4. Can we have multiple stashes?

Yes. Git maintains a list of stash entries.

### Q5. Is Git stash shared with other developers?

No. Stashes are local and are not transferred by normal Git push or pull operations.

### Q6. Can Git stash create conflicts?

Yes. Conflicts may occur when the current files differ from those expected by the stashed changes.

### Q7. When should we use a commit instead of stash?

Use a commit when changes represent a meaningful checkpoint that should be recorded or shared.

## 13. Weekend Hands-On Practice

**Status: Pending — Weekend Practice**

Practice checklist:

- [ ] Create a sample Git repository.
- [ ] Create a feature branch.
- [ ] Modify an existing tracked file.
- [ ] Save changes using `git stash`.
- [ ] Verify the stash list.
- [ ] Switch branches.
- [ ] Restore changes using `git stash apply`.
- [ ] Practice `git stash pop`.
- [ ] Practice stashing untracked files.
- [ ] Practice restoring a specific stash.
- [ ] Explain stash vs commit in your own words.

## 14. Key Takeaways

- Git stash temporarily stores unfinished work.
- It helps developers switch tasks without committing incomplete changes.
- `git stash list` displays saved stashes.
- `git stash apply` keeps the stash entry.
- `git stash pop` removes it after successful application.
- Stashes are local and are not a replacement for commits or backups.
