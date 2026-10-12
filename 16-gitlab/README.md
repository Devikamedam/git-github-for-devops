# GitLab — Day 6

## 1. What Is GitLab?

GitLab is a Git-based DevSecOps platform used to manage source code, collaborate with developers, automate CI/CD pipelines, and support software delivery.

GitLab provides:

- Git repositories
- Branch management
- Merge Requests (MRs)
- Code reviews
- CI/CD pipelines
- Issue tracking
- Access control and security features

Git is the version control system. GitLab is a platform that hosts Git repositories and provides collaboration and automation tools.

## 2. Git vs GitHub vs GitLab

| Feature             | Git                    | GitHub                   | GitLab              |
| ------------------- | ---------------------- | ------------------------ | ------------------- |
| Type                | Version control system | Git hosting platform     | DevSecOps platform  |
| Repository hosting  | Local repositories     | Yes                      | Yes                 |
| Code review         | Not built in           | Pull Requests            | Merge Requests      |
| CI/CD               | External integration   | GitHub Actions           | GitLab CI/CD        |
| Issue tracking      | Not built in           | Yes                      | Yes                 |
| Self-managed option | Git can run locally    | GitHub Enterprise Server | GitLab Self-Managed |

**Important:** Both GitHub and GitLab can support enterprise-level DevOps workflows.

## 3. Real-World DevOps Scenario

Suppose a company has three teams:

- Developers write application code.
- DevOps engineers manage CI/CD pipelines.
- QA engineers test application changes.

The team uses GitLab to collaborate.

Workflow:

```text
Developer
    |
    v
Create Feature Branch
    |
    v
Write Code and Commit
    |
    v
Push to GitLab
    |
    v
Create Merge Request
    |
    v
CI/CD Pipeline Runs
    |
    v
Code Review + Approval
    |
    v
Merge into Main
    |
    v
Deploy Application
```

Deployment should happen only after the required tests, approvals, and environment protections succeed.

## 4. Creating a GitLab Repository

### Step 1: Sign In

Visit:

https://gitlab.com

### Step 2: Create a Project

Select:

Create new → New project/repository → Create blank project

Example project name:

```text
gitlab-devops-practice
```

Choose the appropriate project visibility.

### Step 3: Initialize the Repository

You may initialize the project with a README file.

After creation, GitLab displays the repository URL.

Example HTTPS URL:

```text
https://gitlab.com/USERNAME/gitlab-devops-practice.git
```

Example SSH URL:

```text
git@gitlab.com:USERNAME/gitlab-devops-practice.git
```

Replace USERNAME with your actual GitLab namespace.

## 5. Clone a GitLab Repository

### HTTPS Method

```bash
git clone https://gitlab.com/USERNAME/gitlab-devops-practice.git
```

### SSH Method

```bash
git clone git@gitlab.com:USERNAME/gitlab-devops-practice.git
```

Both methods use encrypted connections.

Authentication is required for private repositories and authenticated Git operations.

## 6. GitLab SSH Authentication

SSH uses a public/private key pair for authentication.

### Step 1: Check Existing SSH Keys

```bash
ls -la ~/.ssh
```

Look for existing public keys such as:

```text
id_ed25519.pub
```

### Step 2: Generate a New SSH Key

If you do not already have a suitable key:

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

Follow the prompts and protect your private key with a passphrase.

### Step 3: Display the Public Key

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy only the public key.

Never share your private key.

### Step 4: Add the Public Key to GitLab

In GitLab:

Profile → Preferences or Edit profile → SSH Keys

Paste your public key and save it.

Menu wording may vary with GitLab versions.

### Step 5: Test SSH Authentication

```bash
ssh -T git@gitlab.com
```

On the first connection, verify the server's SSH host-key fingerprint against GitLab's official published fingerprints before accepting it.

A successful test should indicate that GitLab recognizes your account.

## 7. GitLab HTTPS Authentication

GitLab supports Personal Access Tokens (PATs) for Git operations over HTTPS.

For example:

```bash
git clone https://gitlab.com/USERNAME/gitlab-devops-practice.git
```

When authentication is requested, use the supported credential method.

For a token-based workflow, GitLab may require a personal access token instead of your account password.

Choose only the permissions needed.

Examples of token scopes:

- `read_repository` — Read repository contents.
- `write_repository` — Read and write repository contents.

Do not place access tokens directly inside repository URLs, scripts, or committed configuration files.

Use a credential manager or secure secret storage.

## 8. Basic GitLab Workflow

### Step 1: Clone the Repository

```bash
git clone https://gitlab.com/USERNAME/gitlab-devops-practice.git
```

### Step 2: Enter the Repository

```bash
cd gitlab-devops-practice
```

### Step 3: Create a Feature Branch

```bash
git switch -c feature-login
```

### Step 4: Make Changes

Create or update an application file.

### Step 5: Stage and Commit

```bash
git add .
git commit -m "Add login feature"
```

### Step 6: Push the Branch

```bash
git push -u origin feature-login
```

### Step 7: Create a Merge Request

Open GitLab and create a Merge Request from:

```text
Source branch: feature-login
Target branch: main
```

Assign reviewers, check pipeline results, and merge after the required approvals.

## 9. What Is a Merge Request?

A Merge Request (MR) proposes changes from one branch into another branch.

It allows team members to:

- Review code changes.
- Discuss implementation details.
- Run automated tests.
- Request modifications.
- Approve changes.
- Merge approved code.

GitHub calls this a Pull Request (PR).

GitLab calls this a Merge Request (MR).

Both support collaborative code review workflows.

## 10. GitLab CI/CD

GitLab CI/CD automates tasks such as:

- Code validation
- Unit testing
- Application builds
- Container image creation
- Security scanning
- Deployment

GitLab CI/CD pipelines are commonly defined in:

```text
.gitlab-ci.yml
```

### Example Pipeline

```yaml
stages:
  - test
  - build

test:
  stage: test
  image: python:3.12
  script:
    - python -m compileall -q .

build:
  stage: build
  image: python:3.12
  script:
    - echo "Build stage placeholder"
```

This example demonstrates pipeline stages and job execution.

It does not build or deploy a production application.

A GitLab Runner must be available to execute the jobs.

## 11. What Is GitLab Runner?

GitLab Runner is an application that executes CI/CD jobs.

For example:

```text
Developer Push
      |
      v
GitLab Repository
      |
      v
GitLab CI/CD Pipeline
      |
      v
GitLab Runner
      |
      v
Test / Build / Deploy Jobs
```

Runners can execute jobs using different executors, such as Docker, Shell, or Kubernetes.

Runner configuration and permissions are important for security.

## 12. GitLab Branch Protection

Protected branches help prevent unauthorized or accidental changes.

For example, a team may configure `main` so that:

- Only authorized roles can push.
- Only authorized roles can merge.
- Developers use Merge Requests.
- Required approvals are enforced.

Branch protection and approval capabilities depend on the GitLab edition, tier, and project settings.

## 13. GitLab vs GitHub in a Real Project

### GitHub Workflow

```text
Developer
   |
   v
GitHub Repository
   |
   v
Pull Request
   |
   v
GitHub Actions
   |
   v
Review and Merge
```

### GitLab Workflow

```text
Developer
   |
   v
GitLab Repository
   |
   v
Merge Request
   |
   v
GitLab CI/CD
   |
   v
Review and Merge
```

The core Git commands are largely the same on both platforms.

The main differences are in the platform interfaces, integrations, and administrative features.

## 14. Real-Time DevOps Troubleshooting Scenario

### Scenario: GitLab Pipeline Fails After a Code Push

A developer pushes a new branch.

The GitLab pipeline fails.

### Investigation Steps

1. Open the failed pipeline.
2. Identify the failed job.
3. Review the job logs.
4. Check whether a compatible GitLab Runner is available.
5. Verify dependencies and environment variables.
6. Check access to required container images or package repositories.
7. Fix the underlying issue.
8. Commit and push the correction.
9. Verify the new pipeline run.

Do not bypass required checks merely to merge failing code.

## 15. Interview Questions

### Q1. What is GitLab?

GitLab is a Git-based DevSecOps platform for repository hosting, collaboration, code reviews, and CI/CD automation.

### Q2. What is the difference between Git and GitLab?

Git is a distributed version control system.

GitLab is a platform built around Git repositories.

### Q3. What is the difference between GitHub PR and GitLab MR?

Both propose changes for review and merging.

GitHub uses the term Pull Request; GitLab uses Merge Request.

### Q4. What is a GitLab Runner?

A GitLab Runner executes CI/CD jobs defined in a pipeline.

### Q5. What is `.gitlab-ci.yml`?

It is the configuration file commonly used to define GitLab CI/CD pipelines.

### Q6. What happens when a GitLab pipeline fails?

Engineers inspect the failed job, analyze logs, identify the cause, fix the issue, and rerun the pipeline.

### Q7. Why are protected branches important?

They help enforce access controls and reduce unauthorized or accidental changes to important branches.

### Q8. Can we use the same Git commands with GitHub and GitLab?

Yes. Commands such as `git clone`, `git add`, `git commit`, `git fetch`, and `git push` work with both platforms.

## 16. Weekend Hands-On Practice

**Status: Pending — Weekend Practice**

Practice checklist:

- [ ] Create a GitLab account or sign in.
- [ ] Create a sample GitLab project.
- [ ] Clone using HTTPS.
- [ ] Generate or reuse an appropriate SSH key.
- [ ] Add the public key to GitLab.
- [ ] Test SSH authentication.
- [ ] Clone using SSH.
- [ ] Create a feature branch.
- [ ] Commit and push changes.
- [ ] Create a Merge Request.
- [ ] Explore branch protection settings.
- [ ] Review the GitLab CI/CD interface.

## 17. Key Takeaways

- GitLab provides Git repository hosting and DevSecOps features.
- GitHub uses Pull Requests; GitLab uses Merge Requests.
- GitLab supports HTTPS and SSH authentication.
- GitLab CI/CD automates testing, building, and deployment.
- GitLab Runners execute pipeline jobs.
- Protected branches and code reviews improve collaboration and change control.
