# Git Branching

## Overview

A Git branch allows development work to happen separately without directly changing the main branch.

Basic idea:

```text
                 main
                   |
          A -------B
                   |
                   |
                   C ------- D
                        feature branch
```

A developer can create a branch, make changes, commit them, and push the branch to GitHub.

---

# 1. Check Current Branch

```bash
git branch
```

Example:

```text
* main
```

The `*` shows the currently active branch.

---

# 2. Create a New Branch

```bash
git branch dev
```

This creates a branch named `dev`.

Check:

```bash
git branch
```

Example:

```text
  dev
* main
```

The branch exists, but we are still on `main`.

---

# 3. Switch to a Branch

```bash
git checkout dev
```

Now:

```bash
git branch
```

may show:

```text
* dev
  main
```

Conceptually:

```text
HEAD
 |
 v
dev
 |
 v
Current Commit
```

---

# 4. Create and Switch Together

Instead of two commands:

```bash
git branch dev
git checkout dev
```

we can use:

```bash
git checkout -b dev
```

This creates the branch and switches to it.

---

# 5. Work on the Development Branch

After switching:

```bash
git checkout dev
```

make the required changes.

Then:

```bash
git status
```

Stage:

```bash
git add .
```

Commit:

```bash
git commit -m "Add development changes"
```

Flow:

```text
main
 |
 A
 |
 B
  \
   \
    C
    |
    D
    ^
    |
   dev
   HEAD
```

The new commits belong to the `dev` branch.

---

# 6. Push a Branch to GitHub

Push the branch:

```bash
git push origin dev
```

Flow:

```text
Local dev
    |
    | git push origin dev
    v
GitHub
origin/dev
```

Now the development branch is also available remotely.

---

# 7. Local and Remote Branches

Conceptually:

```text
Local Repository                 GitHub

main --------------------------> main

dev  --------------------------> dev
```

Local branches are maintained in the local repository.

Remote branches are stored in the GitHub repository.

---

# 8. Why Use Branches?

Without branches:

```text
Developer
    |
    v
main
    |
Direct changes
```

With branches:

```text
                  main
                    |
        +-----------+-----------+
        |                       |
        v                       v
    feature-1               feature-2
        |                       |
   Developer 1              Developer 2
```

This allows developers to work independently.

---

# 9. Typical Development Workflow

```text
main
 |
 | create branch
 v
dev
 |
 | modify files
 v
git add
 |
 v
git commit
 |
 v
git push origin dev
 |
 v
GitHub dev branch
```

Commands:

```bash
git checkout -b dev
git status
git add .
git commit -m "Add new feature"
git push origin dev
```

---

# 10. Switch Back to Main

```bash
git checkout main
```

Check:

```bash
git branch
```

Example:

```text
  dev
* main
```

---

# 11. Branch and HEAD

When working on `main`:

```text
HEAD
 |
 v
main
 |
 v
Commit B
```

After switching to `dev`:

```text
HEAD
 |
 v
dev
 |
 v
Commit B
```

After creating a commit on `dev`:

```text
main
 |
 v
Commit B
    \
     \
      Commit C
         ^
         |
        dev
         ^
         |
        HEAD
```

`HEAD` follows the branch you currently have checked out.

---

# 12. Branch Collaboration

A team may use:

```text
                    main
                      |
          +-----------+-----------+
          |                       |
          v                       v
      developer-1             developer-2
          |                       |
       changes                  changes
          |                       |
       commit                   commit
          |                       |
        push                    push
```

The branch changes can later be reviewed and integrated into the main branch.

We cover that process in the Pull Request and Merge sections.

---

# Cloud & DevOps Example

Suppose a DevOps engineer needs to modify Terraform code.

Instead of directly changing `main`:

```bash
git checkout -b update-ec2
```

Modify:

```text
terraform/main.tf
```

Then:

```bash
git status
git add terraform/main.tf
git commit -m "Update EC2 configuration"
git push origin update-ec2
```

Architecture:

```text
main
 |
 +----------------------+
                        |
                        v
                   update-ec2
                        |
                  Terraform change
                        |
                     commit
                        |
                      push
                        |
                        v
                     GitHub
```

This keeps the change separate from `main` until it is ready to be integrated.

---

# Quick Revision

| Command                   | One-line Purpose                                     |
| ------------------------- | ---------------------------------------------------- |
| `git branch`              | Displays local branches and shows the current branch |
| `git branch dev`          | Creates a branch named `dev`                         |
| `git checkout dev`        | Switches to the `dev` branch                         |
| `git checkout -b dev`     | Creates and switches to `dev`                        |
| `git status`              | Shows current branch and repository status           |
| `git add .`               | Stages current changes                               |
| `git commit -m "message"` | Commits staged changes to the current branch         |
| `git push origin dev`     | Pushes the local `dev` branch to the remote          |

---

# Interview Scenario

## Why shouldn't every developer directly work on `main`?

Branches allow changes to be isolated.

```text
main
  |
  +---- feature-A
  |
  +---- feature-B
```

Developers can work independently and integrate their work after it is ready.

---

## Does creating a branch automatically push it to GitHub?

No.

```bash
git branch dev
```

creates a local branch.

To push it:

```bash
git push origin dev
```

---

## What happens to HEAD when you switch branches?

Before:

```text
HEAD → main
```

After:

```bash
git checkout dev
```

```text
HEAD → dev
```

---

# Key Takeaway

```text
CREATE BRANCH
      ↓
SWITCH BRANCH
      ↓
MAKE CHANGES
      ↓
git add
      ↓
git commit
      ↓
git push
      ↓
REMOTE BRANCH
```

Branches allow developers and DevOps engineers to work on changes separately before those changes are integrated into the main codebase.
