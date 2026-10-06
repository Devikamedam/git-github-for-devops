# Git Fetch & Fast-Forward Merge

## Overview

`git fetch` downloads the latest commits and branch information from a remote repository without automatically changing the current working branch.

This makes it useful when we want to inspect remote changes before integrating them into our local branch.

---

## Git Fetch

```bash
git fetch origin
```

### Flow

```text
GitHub / Remote Repository
          |
          | git fetch
          v
Remote-Tracking Branch
     origin/main

Local main is not automatically changed.
```

Example:

```bash
git fetch origin
```

After fetching, we can compare the remote-tracking branch with our local branch.

```bash
git log --oneline main..origin/main
```

To inspect code differences:

```bash
git diff main..origin/main
```

---

## Git Fetch vs Git Pull

| Command     | Downloads Changes | Integrates Changes | Working Branch Changes |
| ----------- | ----------------- | ------------------ | ---------------------- |
| `git fetch` | Yes               | No                 | No                     |
| `git pull`  | Yes               | Yes                | Yes                    |

### Fetch

```bash
git fetch origin
```

Downloads remote updates but does not automatically integrate them into the current branch.

### Pull

```bash
git pull origin main
```

A pull performs a fetch and then integrates the fetched changes into the current branch.

Conceptually:

```text
git pull
   |
   +--- git fetch
   |
   +--- integrate
        (commonly merge, depending on configuration/options)
```

---

## Fetch First, Then Merge

Instead of immediately running:

```bash
git pull origin main
```

we can perform the steps separately:

```bash
git fetch origin
git merge origin/main
```

This gives us an opportunity to inspect the fetched changes before merging.

### Workflow

```text
Remote main
     |
     | git fetch
     v
origin/main
     |
     | git merge origin/main
     v
Local current branch
```

---

# Fast-Forward Merge

A fast-forward merge can happen when the current branch has no new commits of its own and the other branch is simply ahead.

Example:

```text
Before

main
 A --- B

origin/main
 A --- B --- C --- D
```

After:

```bash
git fetch origin
git merge origin/main
```

Git can move `main` directly forward:

```text
A --- B --- C --- D
                  ^
                 main
```

No separate merge commit is required.

---

## Fast-Forward Example

Suppose GitHub contains a new commit that is not available locally.

First:

```bash
git fetch origin
```

Now:

```text
Local main:
A --- B

origin/main:
A --- B --- C
```

Then:

```bash
git merge origin/main
```

Result:

```text
A --- B --- C
          ^
         main
```

This is a fast-forward.

---

## Fetch Does Not Mean Merge

This is important:

```bash
git fetch origin
```

does **not** mean:

```bash
git merge origin/main
```

Fetch only updates our knowledge of the remote repository.

For example:

```text
Working Directory
      |
Staging Area
      |
Local Repository
      |
Local main
      |
      | comparison
      |
origin/main
      ^
      |
   git fetch
      |
GitHub
```

---

## Useful Commands

Check configured remotes:

```bash
git remote -v
```

Download remote information:

```bash
git fetch origin
```

Check branch status:

```bash
git status
```

View commits available remotely but not locally:

```bash
git log --oneline main..origin/main
```

Compare local and remote-tracking branches:

```bash
git diff main..origin/main
```

Merge the fetched remote branch:

```bash
git merge origin/main
```

---

## DevOps Scenario

Assume another engineer updates infrastructure code on GitHub.

Instead of immediately pulling the changes:

```bash
git fetch origin
```

Inspect the commits:

```bash
git log --oneline main..origin/main
```

Inspect the code changes:

```bash
git diff main..origin/main
```

Then integrate them:

```bash
git merge origin/main
```

This allows the engineer to understand remote changes before updating the local branch.

---

## Troubleshooting Scenario

### `git pull` says there is no tracking information

Example:

```text
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
```

You can explicitly specify the remote and branch:

```bash
git pull origin test
```

Or configure the upstream:

```bash
git branch --set-upstream-to=origin/test test
```

After the upstream is configured:

```bash
git pull
git push
```

Git knows that:

```text
local test <-> origin/test
```

---

## Key Takeaways

- `git fetch` downloads remote updates without automatically changing the current branch.
- `origin/main` is a remote-tracking branch in the local repository.
- `git pull` fetches and then integrates changes.
- `git fetch` followed by `git merge origin/main` separates downloading from integration.
- A fast-forward merge moves the branch pointer forward when there is no divergent local history.
- Fetching first is useful when we want to inspect remote commits before integrating them.
