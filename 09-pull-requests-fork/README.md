# GitHub Pull Requests & Fork

## Overview

Pull Requests and Forks are important GitHub collaboration concepts.

A common team workflow is:

```text
main
 ↓
Create Branch
 ↓
Make Changes
 ↓
Commit
 ↓
Push Branch
 ↓
Create Pull Request
 ↓
Review
 ↓
Resolve Conflicts if Required
 ↓
Merge
 ↓
main
```

---

# 1. What is a Pull Request?

A Pull Request (PR) is a request to review and merge changes from one branch into another branch.

Example:

```text
feature branch
      |
      | Pull Request
      v
     main
```

Instead of directly modifying `main`, a developer can work on a separate branch and submit the changes for review.

---

# 2. Pull Request Workflow

Suppose we start from `main`.

Create a branch:

```bash
git checkout -b feature-login
```

Make the required changes.

Check:

```bash
git status
```

Stage:

```bash
git add .
```

Commit:

```bash
git commit -m "Add login feature"
```

Push the branch:

```bash
git push origin feature-login
```

Now the branch exists on GitHub.

```text
Local feature-login
        |
        | git push
        v
GitHub feature-login
```

From GitHub, create a Pull Request to merge:

```text
feature-login
      ↓
Pull Request
      ↓
main
```

---

# 3. Pull Request Review

After the Pull Request is created, team members can review the proposed changes.

Conceptually:

```text
Developer
    |
    | Push branch
    v
GitHub
    |
    | Create PR
    v
Code Review
    |
    +---- Changes Accepted
    |
    +---- Changes Requested
```

If changes are required, the developer can update the branch and push additional commits.

The Pull Request will reflect the updated branch.

---

# 4. Merge the Pull Request

After review and approval, the changes can be merged.

```text
feature-login
      |
      | PR approved
      v
    Merge
      |
      v
     main
```

After the merge, `main` contains the integrated changes.

---

# 5. Pull Request Conflict

Suppose both `main` and the feature branch changed overlapping content differently.

```text
               feature
                  |
                  C
                 /
A ----- B ------+
                 \
                  D
                  |
                 main
```

When attempting to merge the Pull Request, GitHub may report a conflict.

The conflict needs to be resolved before the branches can be merged successfully.

Conceptually:

```text
Pull Request
     ↓
Conflict Detected
     ↓
Resolve Conflict
     ↓
Review Final Changes
     ↓
Merge
```

---

# 6. Why Pull Requests Are Important

Pull Requests provide a collaboration workflow before changes reach an important branch.

They allow teams to:

- Review code changes
- Discuss proposed changes
- Detect merge conflicts
- Request modifications
- Approve changes
- Merge branches

---

# 7. DevOps Pull Request Example

Suppose a DevOps engineer needs to modify Terraform infrastructure.

Create a branch:

```bash
git checkout -b update-security-group
```

Modify:

```text
terraform/security-group.tf
```

Then:

```bash
git add terraform/security-group.tf
git commit -m "Update security group rules"
git push origin update-security-group
```

Create a Pull Request:

```text
update-security-group
        ↓
   Pull Request
        ↓
      Review
        ↓
      Merge
        ↓
       main
```

This allows infrastructure changes to be reviewed before they are integrated into `main`.

---

# 8. What is a Fork?

A fork creates a copy of another repository under your GitHub account.

Conceptually:

```text
Original Repository
        |
        | Fork
        v
Repository under
Your GitHub Account
```

A fork is useful when you want your own GitHub-side copy of a repository, especially when contributing to a repository you do not directly maintain.

---

# 9. Fork Workflow

Example:

```text
Original GitHub Repository
           |
           | Fork
           v
Your GitHub Repository
           |
           | Clone
           v
Your Local Machine
```

After cloning your fork, you can:

```text
Create Branch
     ↓
Make Changes
     ↓
Commit
     ↓
Push
     ↓
Your Fork
     ↓
Pull Request
     ↓
Original Repository
```

---

# 10. Fork vs Clone

These concepts are different.

## Fork

```text
GitHub Repository
       |
       | Fork
       v
Your GitHub Account
```

A fork creates a GitHub-side copy.

## Clone

```text
GitHub Repository
       |
       | git clone
       v
Local Computer
```

A clone creates a local working copy.

Simple difference:

```text
Fork
=
GitHub → GitHub

Clone
=
GitHub → Local Machine
```

---

# 11. Branch vs Fork

A branch exists inside a repository.

```text
Repository
   |
   +---- main
   |
   +---- dev
   |
   +---- feature
```

A fork is a separate repository under another GitHub account.

```text
Original Repository
        |
        | fork
        v
Your Repository
```

Therefore:

```text
Branch
=
Separate line of development
inside the same repository

Fork
=
Separate GitHub repository
copied from another repository
```

---

# 12. Team Collaboration Architecture

A common branch-based team workflow:

```text
                  GitHub Repository
                         |
                        main
                         |
             +-----------+-----------+
             |                       |
             v                       v
        feature-A                feature-B
             |                       |
        Developer 1               Developer 2
             |                       |
           commit                  commit
             |                       |
            push                    push
             |                       |
             v                       v
            PR                      PR
             \                       /
              \                     /
               +------ Review -----+
                         |
                         v
                        main
```

---

# Quick Revision

| Concept / Command          | Purpose                                                |
| -------------------------- | ------------------------------------------------------ |
| Pull Request               | Requests review and integration of branch changes      |
| Code Review                | Team reviews proposed changes                          |
| Merge                      | Integrates branch changes                              |
| Fork                       | Creates a repository copy under another GitHub account |
| Clone                      | Creates a repository copy on the local machine         |
| Branch                     | Separate line of development within a repository       |
| `git checkout -b <branch>` | Creates and switches to a branch                       |
| `git push origin <branch>` | Pushes the branch to GitHub                            |

---

# Interview Scenario

## What is the difference between a Pull Request and a merge?

A Pull Request is the collaboration/review process proposing that changes be integrated.

A merge performs the actual integration of the branch histories.

```text
Pull Request
↓
Review / Discussion / Approval

Merge
↓
Integrate Changes
```

---

## What is the difference between fork and clone?

```text
Fork
↓
Creates a repository copy on GitHub

Clone
↓
Creates a repository copy on the local machine
```

---

## What is the difference between branch and fork?

```text
Branch
↓
Another development line
inside the same repository

Fork
↓
Separate repository
under another GitHub account
```

---

# Key Takeaway

```text
BRANCH
↓
Develop separately

PUSH
↓
Send branch to GitHub

PULL REQUEST
↓
Propose and review changes

MERGE
↓
Integrate approved changes

FORK
↓
Create your own GitHub-side
copy of another repository
```
