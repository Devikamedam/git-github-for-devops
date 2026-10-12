# Git & GitHub for DevOps — Complete Practical Guide

A structured Git, GitHub, and GitLab learning repository focused on version control, branching strategies, team collaboration, troubleshooting, repository security, and real-world Cloud/DevOps workflows.

This repository includes explanations, Git commands, practical examples, interview scenarios, revision notes, and hands-on exercises.

> **Learning status:** Day 1–6 documentation completed. Selected hands-on exercises are pending.

---

## 1. Overview

Git is a distributed version control system used to track changes to source code and infrastructure configurations.

GitHub and GitLab are platforms that host Git repositories and provide collaboration, code review, access control, and CI/CD capabilities.

This repository covers Git fundamentals through advanced team workflows, with examples relevant to:

- Application development
- Cloud infrastructure
- Infrastructure as Code (IaC)
- CI/CD pipelines
- DevOps collaboration
- Production troubleshooting
- Repository security and governance

---

## 2. Learning Objectives

By following this guide, the goal is to understand how to:

- Initialize, clone, and manage Git repositories.
- Stage, commit, and track changes.
- Work with local and remote branches.
- Collaborate using GitHub and GitLab.
- Understand fetch, pull, merge, and rebase.
- Cherry-pick commits between branches.
- Restore files and understand reset and revert.
- Temporarily save changes using Git stash.
- Resolve Git merge conflicts.
- Protect important branches.
- Create and review Pull Requests.
- Use Git on AWS EC2.
- Apply Git best practices in DevOps environments.

---

## 3. Repository Structure

```text
git-github-for-devops/
│
├── 01-init-clone/
│   └── README.md
│
├── 02-add-commit/
│   └── README.md
│
├── 03-status-log-diff/
│   └── README.md
│
├── 04-branches/
│   └── README.md
│
├── 05-merge/
│   └── README.md
│
├── 06-remote-repositories/
│   └── README.md
│
├── 07-push-pull/
│   └── README.md
│
├── 08-branching-workflows/
│   └── README.md
│
├── 09-git-tags/
│   └── README.md
│
├── 10-rebase/
│   └── README.md
│
├── 11-fetch-fast-forward/
│   └── README.md
│
├── 12-cherry-pick/
│   └── README.md
│
├── 13-restore-reset-revert/
│   └── README.md
│
├── 14-gitignore/
│   └── README.md
│
├── 15-git-stash/
│   └── README.md
│
├── 16-gitlab/
│   └── README.md
│
├── 17-branch-protection/
│   └── README.md
│
├── 18-pull-requests-code-review/
│   └── README.md
│
├── 19-merge-conflicts/
│   └── README.md
│
├── 20-git-on-ec2/
│   └── README.md
│
├── revision-notes/
│   └── README.md
│
└── README.md
```

---

## 4. Complete Git & GitHub Topics

### Git Fundamentals

| No. | Topic                    | Documentation                              |
| --- | ------------------------ | ------------------------------------------ |
| 01  | Git Init and Clone       | [View Notes](01-init-clone/README.md)      |
| 02  | Git Add and Commit       | [View Notes](02-add-commit/README.md)      |
| 03  | Git Status, Log and Diff | [View Notes](03-status-log-diff/README.md) |
| 04  | Git Branches             | [View Notes](04-branches/README.md)        |
| 05  | Git Merge                | [View Notes](05-merge/README.md)           |

### Remote Repositories and Collaboration

| No. | Topic               | Documentation                                  |
| --- | ------------------- | ---------------------------------------------- |
| 06  | Remote Repositories | [View Notes](06-remote-repositories/README.md) |
| 07  | Git Push and Pull   | [View Notes](07-push-pull/README.md)           |
| 08  | Branching Workflows | [View Notes](08-branching-workflows/README.md) |
| 09  | Git Tags            | [View Notes](09-git-tags/README.md)            |
| 10  | Git Rebase          | [View Notes](10-rebase/README.md)              |

### Advanced Git Operations — Day 5

| No. | Topic                        | Documentation                                   |
| --- | ---------------------------- | ----------------------------------------------- |
| 11  | Fetch and Fast-Forward Merge | [View Notes](11-fetch-fast-forward/README.md)   |
| 12  | Git Cherry-Pick              | [View Notes](12-cherry-pick/README.md)          |
| 13  | Restore, Reset and Revert    | [View Notes](13-restore-reset-revert/README.md) |

### Advanced Collaboration and Security — Day 6

| No. | Topic                          | Documentation                                        |
| --- | ------------------------------ | ---------------------------------------------------- |
| 14  | Git Ignore                     | [View Notes](14-gitignore/README.md)                 |
| 15  | Git Stash                      | [View Notes](15-git-stash/README.md)                 |
| 16  | GitLab                         | [View Notes](16-gitlab/README.md)                    |
| 17  | Branch Protection and Rulesets | [View Notes](17-branch-protection/README.md)         |
| 18  | Pull Requests and Code Reviews | [View Notes](18-pull-requests-code-review/README.md) |
| 19  | Merge Conflicts                | [View Notes](19-merge-conflicts/README.md)           |
| 20  | Git on AWS EC2                 | [View Notes](20-git-on-ec2/README.md)                |

---

## 5. Git Workflow Architecture

```text
Developer
    |
    v
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
Remote Repository (GitHub/GitLab)
    |
    v
Pull Request / Merge Request
    |
    v
Code Review + CI/CD Validation
    |
    v
Protected Main Branch
    |
    v
Build and Deployment Pipeline
    |
    v
Application Environment
```

This illustrates a typical collaborative DevOps workflow.

---

## 6. Essential Git Commands

| Command                    | Purpose                                    |
| -------------------------- | ------------------------------------------ |
| `git init`                 | Initialize a repository                    |
| `git clone URL`            | Clone a remote repository                  |
| `git status`               | Check repository status                    |
| `git add FILE`             | Stage changes                              |
| `git commit -m "message"`  | Create a commit                            |
| `git log --oneline`        | View commit history                        |
| `git diff`                 | Inspect unstaged changes                   |
| `git branch`               | List local branches                        |
| `git switch BRANCH`        | Switch branches                            |
| `git switch -c BRANCH`     | Create and switch to a branch              |
| `git merge BRANCH`         | Merge another branch                       |
| `git remote -v`            | Display remote repositories                |
| `git fetch origin`         | Download remote updates                    |
| `git pull --ff-only`       | Pull using fast-forward only               |
| `git push origin BRANCH`   | Push commits                               |
| `git rebase main`          | Rebase current branch onto main            |
| `git cherry-pick HASH`     | Apply a selected commit                    |
| `git restore FILE`         | Restore working-tree file contents         |
| `git reset --soft HEAD~1`  | Move HEAD back while keeping index changes |
| `git reset --mixed HEAD~1` | Move HEAD back and unstage changes         |
| `git reset --hard HEAD~1`  | Discard commits and working-tree changes   |
| `git revert HASH`          | Create a commit that reverses a change     |
| `git stash`                | Temporarily save changes                   |
| `git stash list`           | View saved stashes                         |
| `git stash apply`          | Apply stash without deleting it            |
| `git stash pop`            | Apply and remove stash on success          |
| `git merge --abort`        | Cancel an in-progress merge                |
| `git check-ignore -v FILE` | Check Git ignore rules                     |

**Warning:** Commands such as `git reset --hard` can permanently discard uncommitted work. Use them only after verifying the target and protecting important changes.

---

## 7. Branching Strategies

### Feature Branch Workflow

```text
main
 |
 A --- B --- C
       \
        D --- E   feature-login
```

A developer creates a feature branch, implements changes, pushes the branch, and opens a Pull Request.

### Gitflow

Gitflow commonly uses branches such as:

- `main`
- `develop`
- `feature/*`
- `release/*`
- `hotfix/*`

### Trunk-Based Development

Developers integrate small changes frequently into a shared mainline, often using short-lived branches and feature flags.

The appropriate strategy depends on team size, release practices, and deployment requirements.

---

## 8. Merge vs Rebase vs Cherry-Pick

| Operation   | Purpose                           | Common Use                    |
| ----------- | --------------------------------- | ----------------------------- |
| Merge       | Combine branch histories          | Integrate feature branches    |
| Rebase      | Replay commits onto another base  | Update local feature branches |
| Cherry-pick | Apply selected commits            | Transfer a specific fix       |
| Revert      | Reverse changes with a new commit | Safely undo shared changes    |
| Reset       | Move a branch reference           | Correct local history         |

### Merge

```bash
git switch main
git merge feature-login
```

### Rebase

```bash
git switch feature-login
git rebase main
```

### Cherry-Pick

```bash
git cherry-pick <commit-hash>
```

### Revert

```bash
git revert <commit-hash>
```

**Best practice:** Avoid rewriting shared branch history without coordinating with the team.

---

## 9. GitHub Collaboration Workflow

A common workflow in an enterprise DevOps project:

1. Create a feature branch.
2. Implement application or infrastructure changes.
3. Review the local changes.
4. Commit and push the feature branch.
5. Open a Pull Request.
6. Run automated validation.
7. Request code review.
8. Address review feedback.
9. Obtain required approvals.
10. Merge into the protected main branch.
11. Deploy through the approved CI/CD process.

Example:

```bash
git switch main
git pull --ff-only origin main

git switch -c feature-monitoring

# Make and test changes

git add .
git commit -m "Add monitoring configuration"

git push -u origin feature-monitoring
```

Then create a Pull Request in GitHub.

Review staged changes before committing to avoid accidentally including credentials or unrelated files.

---

## 10. GitHub Branch Protection

Important repository protection controls include:

- Requiring Pull Requests before merging.
- Requiring code review approvals.
- Requiring successful status checks.
- Restricting force pushes.
- Preventing branch deletion.
- Restricting direct updates to protected branches.
- Requiring conversation resolution.

Example workflow:

```text
Feature Branch
      |
      v
Pull Request
      |
      +-------------------+
      |                   |
      v                   v
Code Review          CI/CD Checks
      |                   |
      +---------+---------+
                |
                v
         Protected Main
                |
                v
          Deployment
```

Ruleset availability and enforcement depend on repository visibility, GitHub plan, and organization settings.

See [Branch Protection and Rulesets](17-branch-protection/README.md).

---

## 11. Git in Cloud and DevOps

Git is used throughout the software delivery lifecycle.

### Application Development

Store application source code and track changes.

### Infrastructure as Code

Manage Terraform configuration, Kubernetes manifests, and infrastructure definitions.

### CI/CD

Trigger automated pipelines when changes are pushed or merged.

### Configuration Management

Version-control Ansible playbooks and non-sensitive configuration files.

### Monitoring

Track changes to Prometheus configuration, alert rules, and dashboard definitions.

### Cloud Deployments

Use approved pipelines to build, test, and deploy application artifacts to cloud infrastructure.

---

## 12. Real-World DevOps Scenarios

### Scenario 1: Production Hotfix

**Problem:** A critical application issue requires an urgent fix.

**Approach:**

1. Preserve unfinished feature work.
2. Create a hotfix branch from the approved release baseline.
3. Implement and test the fix.
4. Open a Pull Request.
5. Obtain required approvals.
6. Deploy through the authorized pipeline.
7. Monitor the application after deployment.

### Scenario 2: Terraform Merge Conflict

**Problem:** Two engineers modify the same Terraform configuration.

**Approach:**

1. Inspect both sets of changes.
2. Resolve the conflict based on infrastructure requirements.
3. Run `terraform fmt`.
4. Run `terraform validate`.
5. Review the Terraform plan.
6. Obtain approval before applying changes.

### Scenario 3: Accidental Commit

**Problem:** A developer commits an incorrect change.

**Approach:**

- Use `git restore` for suitable uncommitted file changes.
- Use `git reset` carefully for local history corrections.
- Prefer `git revert` to undo a commit already shared with others.

### Scenario 4: Protected Branch Rejects a Push

**Problem:** A developer attempts to push directly to `main`.

**Approach:**

1. Check the repository rules.
2. Create a feature branch.
3. Push the changes to that branch.
4. Open a Pull Request.
5. Complete required reviews and checks.

---

## 13. Git Troubleshooting Guide

| Problem                        | Possible Cause                           | Investigation                         |
| ------------------------------ | ---------------------------------------- | ------------------------------------- |
| Permission denied (publickey)  | SSH authentication failure               | Check key and repository access       |
| Repository not found           | Incorrect URL or permissions             | Verify owner, name, and access        |
| Non-fast-forward push rejected | Remote branch has new commits            | Fetch and inspect remote changes      |
| Merge conflict                 | Incompatible branch changes              | Inspect and resolve conflicting files |
| Detached HEAD                  | Checkout of commit rather than branch    | Inspect HEAD and create/switch branch |
| Untracked files                | Files not staged                         | Review with `git status`              |
| Changes not ignored            | File already tracked                     | Check index and ignore patterns       |
| PR cannot merge                | Failed checks or missing approval        | Inspect PR requirements               |
| Git command not found          | Git not installed or unavailable in PATH | Verify Git installation               |

### Useful Troubleshooting Commands

```bash
git status
git remote -v
git branch -a
git log --oneline --graph --all
git diff
git diff --name-only --diff-filter=U
git stash list
git check-ignore -v FILE
```

---

## 14. Git Security Best Practices

- Never commit passwords, API keys, or private SSH keys.
- Use `.gitignore` for appropriate local files.
- Use secret scanning and secure secret management.
- Protect the `main` branch.
- Require code review for sensitive changes.
- Use least-privilege repository access.
- Prefer dedicated deploy credentials for automation.
- Avoid unnecessary force pushes.
- Review infrastructure changes before deployment.
- Rotate credentials immediately if exposed.

---

## 15. Hands-On Practice Progress

| Topic                           | Progress                                            |
| ------------------------------- | --------------------------------------------------- |
| Git fundamentals and branching  | Practiced                                           |
| Git merge                       | Practiced                                           |
| Cherry-pick                     | Practiced                                           |
| Cherry-pick conflict resolution | Practiced                                           |
| Fetch                           | Practiced                                           |
| Fast-forward merge              | Practiced                                           |
| Restore, Reset, Revert          | Documentation completed; practice pending           |
| Git Ignore                      | Documentation completed; practice pending           |
| Git Stash                       | Documentation completed; practice pending           |
| GitLab                          | Documentation completed; practice pending           |
| Branch Protection               | Documentation completed; practice pending           |
| Pull Requests and Reviews       | Further practice pending                            |
| Merge Conflicts                 | Additional practice pending                         |
| Git on EC2                      | Documentation completed; dedicated practice pending |

Practice progress will be updated as exercises are completed.

---

## 16. Interview Preparation

This repository supports interview preparation for Git and Git-based DevOps workflows.

Important areas include:

- Git architecture and the three working areas
- Git branching and merging
- Merge vs rebase
- Fetch vs pull
- Fast-forward vs three-way merge
- Cherry-pick and conflict resolution
- Reset vs revert vs restore
- Git stash
- Pull Requests and code reviews
- Protected branches and repository rulesets
- Git authentication
- GitLab CI/CD basics
- Git troubleshooting
- Git integration with Terraform, Docker, and CI/CD

For quick review, see:

[Git & GitHub Revision Notes](revision-notes/README.md)

---

## 17. Learning Outcomes

Through the documented topics and completed exercises, this repository demonstrates a structured approach to learning Git and Git-based collaboration.

The intended outcomes are:

- Strong understanding of version control fundamentals
- Ability to work with branches and commits
- Familiarity with remote repository collaboration
- Understanding of code review and protected branch workflows
- Ability to diagnose common Git errors
- Awareness of secure repository practices
- Readiness to apply Git concepts in Cloud/DevOps projects

Hands-on proficiency will continue to improve as the remaining exercises are completed.

---

## 18. Future Enhancements

- Complete remaining Day 5 and Day 6 hands-on labs.
- Add screenshots of verified Git operations.
- Practice GitLab Merge Requests and CI/CD.
- Configure branch protection in a practice repository.
- Integrate GitHub Actions with a sample application.
- Add Git troubleshooting case studies.
- Document real-world infrastructure Pull Request examples.

---

## 19. Repository Maintainer

**GitHub:** [Devikamedam](https://github.com/Devikamedam)

**Repository:** [git-github-for-devops](https://github.com/Devikamedam/git-github-for-devops)

**Focus Areas:**

AWS | Linux | Git | GitHub | GitLab | Terraform | Docker | Kubernetes | Jenkins | CI/CD | Monitoring | Cloud & DevOps

---

## 20. Conclusion

Git is a foundational skill for Cloud and DevOps engineering.

Understanding Git commands is important, but real-world proficiency also requires knowledge of collaboration, branching strategies, code reviews, conflict resolution, repository security, and CI/CD integration.

This repository brings together Git fundamentals, advanced workflows, troubleshooting scenarios, and practical learning resources in one place.

**Documentation:** Day 1–6 completed.
