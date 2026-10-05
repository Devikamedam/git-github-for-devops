# Git & GitHub for Cloud & DevOps

Hands-on Git and GitHub learning focused on the concepts and workflows commonly used in Cloud and DevOps environments.

This repository documents my Git and GitHub learning through practical commands, workflows, branching, collaboration, authentication, merge/rebase concepts, conflict resolution, and troubleshooting.

---

## Git Workflow

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
Remote Repository (GitHub)

Remote Repository
       |
       | git pull
       v
Local Repository
```

---

## Topics Covered

### 1. [Git & GitHub Basics](./01-git-github-basics/README.md)

Git, GitHub, version control, local repository, remote repository, and commit concepts.

### 2. [Git Workflow](./02-git-workflow/README.md)

Working directory, staging area, local repository, remote repository, `git add`, `git commit`, `git status`, and `git push`.

### 3. [Clone, Pull & Remote](./03-clone-pull-remote/README.md)

Clone repositories, pull remote changes, inspect remotes, and understand the local-to-remote relationship.

### 4. [GitHub Authentication](./04-github-authentication/README.md)

HTTPS authentication, SSH authentication, SSH keys, and Personal Access Token concepts.

### 5. [Git History & HEAD](./05-git-history-head/README.md)

Commit history, commit IDs, `git log`, `HEAD`, `main`, `origin/main`, `origin/HEAD`, and checkout concepts.

### 6. [Git Branching](./06-branching/README.md)

Create, switch, inspect, and push branches for isolated development work.

### 7. [Merge & Rebase](./07-merge-rebase/README.md)

Understand merge, rebase, `git pull --rebase`, and their effects on commit history.

### 8. [Git Conflicts](./08-conflicts/README.md)

Understand why conflicts occur and how conflicting changes are identified and resolved.

### 9. [Pull Requests & Fork](./09-pull-requests-fork/README.md)

Team collaboration using branches, pull requests, code review, merge, and repository forks.

### 10. [Git Diff](./10-git-diff/README.md)

Compare working-directory changes, staged changes, commits, branches, and local/remote changes.

---

## Cloud & DevOps Use

Git and GitHub are commonly used in Cloud and DevOps workflows for:

- Infrastructure as Code
- CI/CD pipelines
- Application source code
- Configuration management
- Shell scripts and automation
- Terraform code
- Kubernetes manifests
- Deployment files
- Team collaboration
- Change tracking and rollback

---

## Revision Notes

Quick command revision and scenario-based notes will be maintained in:

[`revision-notes/`](./revision-notes/)

---

### Learning Progress

- [x] Git & GitHub Basics
- [x] Git Workflow
- [x] Clone, Pull & Remote
- [x] GitHub Authentication
- [x] Git History & HEAD
- [x] Branching
- [x] Merge & Rebase
- [x] Conflict Concepts
- [x] Pull Requests & Fork
- [x] Git Diff

---

## Current Scope

This repository currently documents the Git and GitHub topics covered through **Day 4** of my hands-on learning.

Additional topics will be added as learning progresses.
