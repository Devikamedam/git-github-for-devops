# GitHub Pull Requests and Code Reviews — Day 6

## 1. What Is a Pull Request?

A Pull Request (PR) is a request to merge changes from one Git branch into another.

Developers use Pull Requests to:

- Share code changes.
- Request code reviews.
- Run automated CI/CD checks.
- Discuss improvements.
- Resolve issues before merging.
- Maintain code quality.

In GitHub, this process is called a Pull Request.

In GitLab, it is called a Merge Request (MR).

## 2. Why Are Pull Requests Important in DevOps?

In a real project, multiple developers work on the same application.

For example, an e-commerce application may have:

- Frontend developers
- Backend developers
- DevOps engineers
- QA engineers
- Security engineers

If everyone pushes directly to `main`, changes can interfere with one another.

Pull Requests provide a controlled way to integrate code.

They also create an audit trail showing what changed, why it changed, and who reviewed it.

## 3. Real-World DevOps Workflow

```text
Developer
    |
    v
Create Feature Branch
    |
    v
Modify Application Code
    |
    v
Git Add + Commit
    |
    v
Push Feature Branch
    |
    v
Create Pull Request
    |
    +------------------------+
    |                        |
    v                        v
Code Review              CI/CD Pipeline
    |                        |
    v                        v
Approval                 Tests Pass
    |                        |
    +------------+-----------+
                 |
                 v
          Merge into Main
                 |
                 v
       Deployment Workflow
```

A production deployment should follow the team's environment protection and release policies.

## 4. Creating a Feature Branch

First, switch to `main`:

```bash
git switch main
```

Get the latest changes:

```bash
git pull --ff-only origin main
```

Create a feature branch:

```bash
git switch -c feature-login
```

Now make changes to your application.

For example, update `app.py` with a login-related feature.

Check your changes:

```bash
git status
git diff
```

## 5. Stage and Commit Changes

Stage the required files:

```bash
git add app.py
```

Commit:

```bash
git commit -m "Add login validation"
```

Push the feature branch:

```bash
git push -u origin feature-login
```

**Note:** Use specific files with `git add` when possible. This helps avoid accidentally staging unrelated or sensitive files.

## 6. How to Create a Pull Request in GitHub

### Step 1: Open the Repository

Navigate to your GitHub repository.

### Step 2: Open Pull Requests

Select:

Pull requests → New pull request

### Step 3: Select Branches

```text
Base branch: main
Compare branch: feature-login
```

The base branch is where the changes will be merged.

The compare branch contains the proposed changes.

### Step 4: Add PR Details

Example title:

```text
Add login validation
```

Example description:

```text
Summary:
- Added login input validation.
- Improved error handling.

Testing:
- Tested valid and invalid login inputs.
- Verified application behavior.

Impact:
- Backend login functionality.
```

### Step 5: Create the Pull Request

Select:

Create pull request

The PR is now available for review.

## 7. What Is Code Review?

Code review is the process of examining proposed code changes before merging them.

Reviewers check:

- Correctness
- Security
- Readability
- Performance
- Error handling
- Test coverage
- Infrastructure impact
- Coding standards

For DevOps projects, reviewers may also inspect:

- Terraform changes
- Kubernetes manifests
- Dockerfiles
- CI/CD workflows
- IAM permissions
- Secrets management
- Deployment configuration

## 8. Pull Request Review Options

GitHub reviewers can generally choose:

### Comment

Provide feedback without explicitly approving or requesting changes.

### Approve

Indicate that the reviewer approves the proposed changes.

### Request Changes

Indicate that changes are required before approval.

**Important:** Whether a review blocks merging depends on repository rules, review requirements, and permissions.

## 9. How to Request a Reviewer

Open the Pull Request.

In the reviewer section:

1. Select Reviewers.
2. Choose an eligible collaborator or team.
3. Request a review.

A reviewer can inspect the changed files and submit feedback.

For required approvals, an eligible reviewer must approve according to the repository's rules.

## 10. What Happens When Changes Are Requested?

Suppose a reviewer finds a problem in the login validation code.

The reviewer requests changes.

The developer updates the same feature branch.

```bash
git switch feature-login
```

Make the corrections.

Then:

```bash
git add app.py
git commit -m "Address login validation review feedback"
git push origin feature-login
```

The existing Pull Request automatically reflects the new commit.

You usually do not need to create another PR.

Depending on repository rules, new commits may dismiss earlier approvals and require another review.

## 11. Pull Request Merge Strategies

GitHub commonly supports three merge strategies.

### A. Create a Merge Commit

```text
main:     A --- B -------- M
               \         /
feature:        C --- D
```

The merge commit combines the histories of the two branches.

Advantages:

- Preserves the original feature commits.
- Shows the branch integration point.

Disadvantages:

- Can produce a more complex commit graph.

### B. Squash and Merge

```text
Before:

main:     A --- B
               \
feature:        C --- D

After:

main:     A --- B --- S
```

Commits C and D are combined into a single new commit S on `main`.

Advantages:

- Keeps the main branch history concise.
- Useful for features with many small commits.

Disadvantages:

- Individual feature commit history is not preserved on `main`.

### C. Rebase and Merge

```text
Before:

main:     A --- B
               \
feature:        C --- D

After:

main:     A --- B --- C' --- D'
```

GitHub replays the feature changes onto the base branch, creating new commit IDs.

Advantages:

- Maintains a linear history.
- Avoids merge commits.

Disadvantages:

- Rewrites the commits being integrated.

**Note:** Available merge strategies depend on repository settings and applicable branch rules.

## 12. Pull Request vs Direct Push

| Pull Request                              | Direct Push                                       |
| ----------------------------------------- | ------------------------------------------------- |
| Supports code review                      | Does not require a review step                    |
| Can run PR-specific CI checks             | Can trigger push-based CI checks                  |
| Supports discussions                      | No PR discussion by default                       |
| Helps enforce approvals                   | May bypass review workflow if allowed             |
| Recommended for shared protected branches | Useful for permitted personal or feature branches |

Direct pushes are not inherently wrong. Their appropriateness depends on the branch and team policy.

## 13. Pull Request vs Merge Request

| GitHub                     | GitLab             |
| -------------------------- | ------------------ |
| Pull Request (PR)          | Merge Request (MR) |
| GitHub Actions             | GitLab CI/CD       |
| Reviewers                  | Reviewers          |
| Branch rulesets/protection | Protected branches |
| Merge changes              | Merge changes      |

Both support collaboration and automated validation.

## 14. Real-Time DevOps Scenario

### Scenario: Terraform Infrastructure Change

A DevOps engineer needs to update an AWS security group.

The change must be reviewed before reaching the main branch.

### Workflow

1. Create a feature branch.
2. Modify the Terraform configuration.
3. Run `terraform fmt`.
4. Run `terraform validate`.
5. Review the Terraform plan using an approved workflow.
6. Commit and push the changes.
7. Create a Pull Request.
8. Request infrastructure and security review.
9. Verify CI/CD checks.
10. Merge after approval.
11. Apply changes through the authorized deployment pipeline.

**Security Note:** Terraform plans and CI logs may contain sensitive information. Protect access and avoid exposing secrets in PR comments or artifacts.

## 15. Troubleshooting: Pull Request Cannot Be Merged

Possible reasons:

- Required approval is missing.
- CI/CD checks are failing.
- Merge conflicts exist.
- Review conversations are unresolved.
- The feature branch needs updating.
- The user lacks merge permission.
- A repository ruleset blocks the merge.

### Troubleshooting Process

1. Open the Pull Request.
2. Read the merge requirements.
3. Inspect failed CI/CD jobs.
4. Review comments and requested changes.
5. Resolve merge conflicts.
6. Request required approvals.
7. Verify the branch protection requirements.
8. Merge after all checks pass.

## 16. Interview Questions

### Q1. What is a Pull Request?

A Pull Request proposes changes from one branch to another and provides a place for review, discussion, and automated checks.

### Q2. What is the difference between PR and MR?

PR is GitHub terminology, while MR is GitLab terminology. Both support code review and merging.

### Q3. What is the difference between merge, squash, and rebase?

Merge preserves branch integration through a merge commit.

Squash combines multiple commits into one.

Rebase replays commits on top of another branch.

### Q4. What happens when a reviewer requests changes?

The developer updates the feature branch, commits the corrections, and pushes them to the same PR.

### Q5. Why are Pull Requests important for Terraform?

They support infrastructure code review, validation, security checks, and controlled changes.

### Q6. Can a Pull Request be merged without approval?

Yes, if repository rules permit it. Required approvals can be enforced through branch protection or rulesets.

### Q7. Can CI/CD pipelines run automatically when a PR is created?

Yes. GitHub Actions and other CI systems can be configured to run on PR events.

### Q8. What would you do if a PR has merge conflicts?

Update the feature branch, resolve the conflicting changes, test the result, and push the resolution.

### Q9. What is the purpose of CODEOWNERS?

A CODEOWNERS file identifies people or teams responsible for reviewing particular files or directories.

When combined with appropriate repository rules, code owner approval can be required.

### Q10. What is the difference between a Pull Request and a Git commit?

A commit records changes in Git history.

A Pull Request proposes integrating branch changes through a collaborative review workflow.

## 17. Weekend Hands-On Practice

**Status: Pending — Weekend Practice**

### Practice Checklist

- [ ] Create a feature branch.
- [ ] Modify a sample application file.
- [ ] Commit and push changes.
- [ ] Create a Pull Request.
- [ ] Add a meaningful PR description.
- [ ] Request a reviewer.
- [ ] Review changed files.
- [ ] Add review comments.
- [ ] Update the feature branch.
- [ ] Observe CI/CD checks.
- [ ] Compare merge, squash, and rebase strategies.
- [ ] Merge the PR when permitted.
- [ ] Delete the merged feature branch if no longer needed.

## 18. Key Takeaways

- Pull Requests support controlled code integration.
- Code reviews improve quality and collaboration.
- Automated checks help identify problems before merging.
- Required approvals depend on repository rules.
- Merge, squash, and rebase create different commit histories.
- DevOps teams use PRs for application code and infrastructure changes.
- Protected branches and PR workflows support safer releases.
