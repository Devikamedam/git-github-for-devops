# Git & GitHub Basics

## Overview

Git and GitHub are important tools used by developers and DevOps engineers to manage source code, track changes, collaborate with teams, and maintain different versions of applications and infrastructure code.

---

## What is Version Control?

Version control is a system used to track changes made to files over time.

For example:

```text
Application
    |
    | First change
    v
Version 1
    |
    | Second change
    v
Version 2
    |
    | Third change
    v
Version 3
```

Instead of manually keeping multiple copies of the same project, Git tracks the changes through commits.

Each commit receives a unique commit ID.

---

## What is Git?

Git is a distributed version control system.

Git tracks changes made to files and maintains the history of those changes.

Git mainly works on the developer's local machine.

```text
Developer
    |
    v
Local Git Repository
```

Git allows us to:

- Track file changes
- Maintain different versions
- Create commits
- View previous changes
- Create branches
- Merge changes
- Work with other developers

---

## What is GitHub?

GitHub is a cloud-based platform used to host Git repositories.

```text
Developer
    |
    | git push
    v
GitHub
Remote Repository
```

GitHub allows teams to store and collaborate on source code using remote repositories.

---

## Git vs GitHub

| Git                     | GitHub                                         |
| ----------------------- | ---------------------------------------------- |
| Version control system  | Cloud-based Git repository hosting platform    |
| Works locally           | Hosts repositories remotely                    |
| Tracks changes          | Stores and shares Git repositories             |
| Creates commits         | Supports collaboration around Git repositories |
| Does not require GitHub | Uses Git repositories                          |

Simple way to remember:

```text
Git
=
Track and manage code changes

GitHub
=
Store and collaborate on Git repositories remotely
```

---

## Local Repository

A local repository exists on the developer's machine.

```text
Developer
    |
    v
Local Repository
```

The developer makes changes and records those changes using Git.

---

## Remote Repository

A remote repository is a Git repository stored on a remote platform such as GitHub.

```text
Developer
    |
    | push
    v
GitHub
Remote Repository
```

The remote repository allows multiple developers to collaborate on the same project.

---

## Developer Collaboration

Example:

```text
             GitHub
         Remote Repository
              /   \
             /     \
            /       \
      Developer 1  Developer 2
```

Both developers can work locally and synchronize their changes with the remote repository.

Typical flow:

```text
Developer 1
    |
Local Repository
    |
    | push
    v
GitHub

Developer 2
    |
    | pull
    v
Local Repository
```

---

## What is a Commit?

A commit represents a saved point in Git history.

Example:

```text
Commit 1
   |
Commit 2
   |
Commit 3
```

Each commit has a unique commit ID.

This allows Git to track how the project changes over time.

---

## Commit ID

Git generates a unique identifier for each commit.

Example:

```text
Commit 1 → a1b2c3...
Commit 2 → d4e5f6...
Commit 3 → g7h8i9...
```

The commit ID helps identify a particular point in repository history.

You can view commit history using:

```bash
git log
```

---

## Git Repository

A Git repository contains the project files together with Git's version-control information.

Repositories can be:

```text
Local Repository
      |
      | push
      v
Remote Repository
```

For this repository, GitHub is being used as the remote repository platform.

---

## Why Git is Important in DevOps

DevOps environments use Git to version and manage many types of files, including:

```text
Application Code
Terraform
Shell Scripts
Dockerfiles
Kubernetes YAML
CI/CD Pipeline Files
Configuration Files
Automation Scripts
```

Example:

```text
Developer / DevOps Engineer
           |
           v
          Git
           |
           v
        GitHub
           |
           v
      CI/CD Pipeline
           |
           v
       Deployment
```

This makes Git a fundamental part of modern DevOps workflows.

---

## Key Takeaway

```text
Git
↓
Tracks versions and changes

Git Repository
↓
Stores Git history

Commit
↓
Represents a saved point in history

GitHub
↓
Hosts remote Git repositories

Developers
↓
Collaborate through Git and GitHub
```

---

## Quick Revision

| Concept           | Purpose                               |
| ----------------- | ------------------------------------- |
| Git               | Tracks and manages versions of files  |
| GitHub            | Hosts Git repositories remotely       |
| Local Repository  | Repository on the developer's machine |
| Remote Repository | Repository hosted remotely            |
| Commit            | Saved point in Git history            |
| Commit ID         | Unique identifier for a commit        |
| `git log`         | Displays commit history               |
