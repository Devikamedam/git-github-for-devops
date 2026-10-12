# GitHub Branch Protection and Rulesets — Day 6

## 1. What Is Branch Protection?

Branch protection is a GitHub feature that helps prevent unauthorized or accidental changes to important branches.

In real-world projects, the `main` branch often contains stable, reviewed code.

Developers should not make uncontrolled changes directly to `main`.

Branch protection can enforce rules such as:

- Require Pull Requests before merging.
- Require code review approvals.
- Require successful CI/CD checks.
- Block force pushes.
- Prevent branch deletion.
- Restrict who can update protected branches.
- Require resolution of review conversations.

## 2. Why Is Branch Protection Important in DevOps?

Imagine a company running an e-commerce application on AWS.

The application uses:

- GitHub for source code.
- Jenkins or GitHub Actions for CI/CD.
- Docker for containerization.
- Kubernetes or EC2 for deployment.
- AWS RDS for the database.

If developers directly push untested code to `main`, a deployment pipeline could release faulty code.

Branch protection helps enforce review and validation before code is merged.

**Important:** Branch protection is one control. It does not replace testing, security scanning, or deployment approvals.

## 3. Real-World Architecture

```text
Developer
    |
    v
Feature Branch
    |
    v
Code Changes
    |
    v
Git Push
    |
    v
GitHub Pull Request
    |
    +-----------------------+
    |                       |
    v                       v
Code Review             CI/CD Checks
    |                       |
    v                       v
Approval                 Tests Pass
    |                       |
    +-----------+-----------+
                |
                v
        Protected Main
                |
                v
        Deployment Pipeline
                |
                v
       Staging / Production
```

The merge is permitted only when all configured requirements are satisfied.

## 4. What Are GitHub Rulesets?

GitHub rulesets define policies that control how branches and tags can be updated.

Rulesets can be configured at repository level. Some organizations can also apply organization-level rulesets.

Common rules include:

- Restrict deletions.
- Block force pushes.
- Require Pull Requests.
- Require approvals.
- Require status checks.
- Require signed commits.
- Require linear history.

Availability and enforcement depend on the GitHub plan, repository visibility, and organization settings.

## 5. How to Create a Branch Ruleset

### Step 1: Open Your Repository

Example:

https://github.com/USERNAME/REPOSITORY

### Step 2: Open Repository Settings

Navigate to:

Settings → Rules → Rulesets

Select:

New ruleset → New branch ruleset

The exact menu labels may change as GitHub updates its interface.

### Step 3: Give the Ruleset a Name

Example:

```text
protect-main
```

### Step 4: Select Enforcement Status

Choose:

```text
Active
```

An evaluation mode, if available, may allow testing without enforcing all restrictions.

### Step 5: Select Target Branch

Configure the branch target to match:

```text
main
```

Use the exact branch name or an appropriate branch pattern.

### Step 6: Configure Rules

For a typical team workflow:

- Restrict deletions.
- Block force pushes.
- Require a Pull Request before merging.
- Require at least one approval.
- Require conversation resolution.
- Require selected status checks after those checks exist.

Save the ruleset.

**Note:** On GitHub Free, ruleset enforcement for private repositories may not be available. Check your repository's visibility and plan before testing.

## 6. What Is a Protected Main Branch?

A protected `main` branch is subject to configured policies.

Example:

```text
main
 |
 +-- Direct push restricted
 |
 +-- Pull Request required
 |
 +-- Code review required
 |
 +-- CI/CD checks required
 |
 +-- Force push blocked
 |
 +-- Branch deletion blocked
```

The exact behavior depends on the rules and any bypass permissions configured.

## 7. Why Block Force Pushes?

A force push can overwrite the remote branch's history.

Example:

```bash
git push --force origin main
```

This may discard commits from the branch's visible history and disrupt other developers.

For shared production branches, force pushes are usually blocked.

If a force push is genuinely necessary on a personal feature branch, prefer:

```bash
git push --force-with-lease
```

This provides an additional safeguard against overwriting unexpected remote updates.

It is still a history-rewriting operation and must follow team policy.

## 8. What Is a Pull Request Approval?

A Pull Request approval means a reviewer has reviewed the proposed changes and approved them.

Typical workflow:

1. Developer creates a feature branch.
2. Developer commits and pushes changes.
3. Developer opens a Pull Request.
4. Reviewer examines the code.
5. Reviewer requests changes or approves.
6. Required checks complete successfully.
7. Authorized user merges the Pull Request.

If the repository requires one approval, an eligible reviewer must approve before merging.

An author generally cannot approve their own Pull Request to satisfy the approval requirement.

## 9. Require Status Checks Before Merging

Status checks are automated results reported to GitHub.

Examples:

- Unit tests
- Linting
- Build validation
- Container image checks
- Security scanning

A team can configure required checks such as:

```text
unit-tests
lint
build
```

Example GitHub Actions workflow:

```yaml
name: CI Validation

on:
  pull_request:
    branches:
      - main

jobs:
  unit-tests:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Validate Python syntax
        run: python -m compileall -q .
```

This example performs Python syntax compilation. It is not a full unit test suite.

For a real project, add actual automated tests and configure the appropriate job checks as required in the ruleset.

## 10. Require Linear History

Linear history avoids merge commits on the protected branch.

Example:

```text
A --- B --- C --- D --- E
```

Teams may use squash merging or rebase merging to maintain a linear history.

If linear history is required, standard merge commits are not allowed.

This can make commit history easier to inspect.

## 11. Branch Protection vs Rulesets

| Branch Protection Rules                 | Rulesets                               |
| --------------------------------------- | -------------------------------------- |
| Traditional GitHub protection mechanism | Newer policy framework                 |
| Configured for branch patterns          | Can target multiple branches or tags   |
| Controls PRs, reviews, and checks       | Supports broader policy configurations |
| Existing repositories may use it        | Useful for centralized governance      |
| Rules can overlap                       | Multiple applicable rulesets can apply |

Both mechanisms help protect important branches.

## 12. Real-Time DevOps Scenario

### Scenario: A Developer Tries to Push Directly to Main

The developer runs:

```bash
git push origin main
```

GitHub rejects the push because the branch requires changes through a Pull Request.

### Correct Workflow

Create a feature branch:

```bash
git switch main
git pull --ff-only origin main
git switch -c feature-payment
```

Make the required changes.

Stage and commit:

```bash
git add .
git commit -m "Implement payment validation"
```

Push:

```bash
git push -u origin feature-payment
```

Open a Pull Request targeting `main`.

Request review and wait for required checks.

Merge only after repository policies are satisfied.

## 13. Troubleshooting: Pull Request Cannot Be Merged

### Possible Causes

1. Required reviewer approval is missing.
2. Required status checks are failing.
3. Review conversations remain unresolved.
4. Branch is out of date and must be updated.
5. Merge conflicts exist.
6. The user lacks permission to merge.
7. Another applicable ruleset blocks the merge.

### Troubleshooting Steps

1. Open the Pull Request.
2. Review the merge requirements.
3. Check failed GitHub Actions jobs.
4. Review branch rulesets.
5. Resolve conflicts if present.
6. Request an eligible reviewer.
7. Verify all required checks.
8. Merge after requirements are satisfied.

Do not disable branch protection simply to bypass a failed test.

## 14. Interview Questions

### Q1. What is branch protection in GitHub?

Branch protection enforces rules that help prevent unauthorized or accidental changes to important branches.

### Q2. Why should we protect the main branch?

To reduce the risk of unreviewed, untested, or unauthorized changes reaching stable code.

### Q3. What is a GitHub ruleset?

A configurable set of policies governing how selected branches or tags can be updated.

### Q4. How do you require code review before merging?

Configure a branch protection rule or ruleset requiring a Pull Request and the desired number of eligible approvals.

### Q5. What happens if a required CI/CD check fails?

GitHub blocks merging until the configured requirements are satisfied, unless an authorized bypass applies.

### Q6. Why should force pushes be restricted on main?

Force pushes can rewrite shared branch history and disrupt collaboration.

### Q7. What is the difference between branch protection and deployment protection?

Branch protection controls changes to repository branches.

Deployment protection controls whether deployments to particular environments can proceed.

### Q8. Can repository administrators bypass branch rules?

It depends on the configured rules, bypass permissions, and applicable GitHub policies.

### Q9. What is linear Git history?

A commit history without merge commits on the protected branch.

### Q10. What would you do if a developer cannot merge a Pull Request?

Inspect the Pull Request requirements, reviewer approvals, CI/CD results, branch status, conflicts, permissions, and applicable rulesets.

## 15. Weekend Hands-On Practice

**Status: Pending — Weekend Practice**

### Practice Checklist

- [ ] Open a practice GitHub repository.
- [ ] Explore Settings → Rules → Rulesets.
- [ ] Create a branch ruleset targeting `main`.
- [ ] Enable deletion restrictions.
- [ ] Enable force-push restrictions.
- [ ] Configure Pull Request requirements where supported.
- [ ] Create a feature branch.
- [ ] Push changes.
- [ ] Create a Pull Request.
- [ ] Verify whether rules are enforced.
- [ ] Test code review using a second collaborator if available.
- [ ] Observe what happens when requirements are not satisfied.

**Important:** Use a disposable practice repository rather than modifying production repository policies.

## 16. Key Takeaways

- Protect important branches such as `main`.
- Use Pull Requests for controlled code integration.
- Require appropriate code reviews.
- Use CI/CD checks to validate changes.
- Block unwanted force pushes and branch deletions.
- Understand GitHub rulesets and bypass permissions.
- Combine repository protection with deployment security controls.
