# Git Merge Conflicts and Conflict Resolution — Day 6

## 1. What Is a Git Merge Conflict?

A Git merge conflict occurs when Git cannot automatically combine changes from different branches.

Git requires a developer to decide which changes should be kept.

**Simple explanation:**

Two developers modify the same part of a file differently. Git cannot safely decide which version is correct.

## 2. Why Do Merge Conflicts Occur?

Common reasons include:

- Two developers modify the same lines.
- One branch deletes a file while another modifies it.
- Two branches rename or modify related files differently.
- A cherry-pick introduces conflicting changes.
- A rebase encounters changes that cannot be applied automatically.
- A stash is applied over incompatible working-tree changes.

**Important:** Git can often merge different changes in the same file automatically. Editing the same file does not always cause a conflict.

## 3. Real-World DevOps Example

Imagine two DevOps engineers working on the same Terraform configuration.

Developer A changes the EC2 instance type to:

```hcl
instance_type = "t3.medium"
```

Developer B changes the same line to:

```hcl
instance_type = "t3.large"
```

When their branches are merged, Git may report a conflict.

The team must decide which instance type satisfies the application's requirements.

## 4. Understanding the Branch History

Before merging:

```text
             C --- D  (feature)
            /
A --- B --- E         (main)
```

Both branches contain changes after their common ancestor.

When Git cannot combine those changes automatically, a conflict occurs.

## 5. What Does a Merge Conflict Look Like?

Suppose `app.py` contains:

```python
<<<<<<< HEAD
print("Application version 1")
=======
print("Application version 2")
>>>>>>> feature-update
```

### Conflict Markers Explained

| Marker                   | Meaning                                   |
| ------------------------ | ----------------------------------------- |
| `<<<<<<< HEAD`           | Beginning of the current branch's version |
| `=======`                | Separator between conflicting versions    |
| `>>>>>>> feature-update` | End of the incoming branch's version      |

During a regular merge, `HEAD` normally refers to the branch you currently have checked out.

**Important:** During a rebase, the meaning of ours and theirs can be different from what beginners expect. Always inspect the changes before resolving them.

## 6. How to Resolve a Merge Conflict

### Step 1: Check Git Status

```bash
git status
```

Example output:

```text
both modified: app.py
```

### Step 2: Open the Conflicted File

Open `app.py` in VS Code.

You may see options such as:

- Accept Current Change
- Accept Incoming Change
- Accept Both Changes
- Compare Changes

These options depend on the editor and Git operation.

### Step 3: Resolve the Conflict

Suppose both changes are required.

You might update the file to:

```python
print("Application version 1")
print("Application version 2")
```

Or choose one version if only one is correct.

Remove all conflict markers.

### Step 4: Stage the Resolved File

```bash
git add app.py
```

### Step 5: Complete the Merge

If a merge is in progress:

```bash
git merge --continue
```

Git may open an editor for the merge commit message.

### Step 6: Verify

```bash
git status
git log --oneline --graph --all
```

Confirm that the merge completed successfully.

## 7. Complete Hands-On Example

**Practice in a separate temporary repository. Do not use your main learning repository.**

### Step 1: Create a Repository

```bash
mkdir merge-conflict-practice
cd merge-conflict-practice
git init -b main
```

### Step 2: Create a File

Create `app.py`:

```python
print("Hello DevOps")
```

Commit it:

```bash
git add app.py
git commit -m "Initial application"
```

### Step 3: Create a Feature Branch

```bash
git switch -c feature
```

Change `app.py` to:

```python
print("Hello from feature branch")
```

Commit:

```bash
git add app.py
git commit -m "Update greeting in feature"
```

### Step 4: Return to Main

```bash
git switch main
```

Change `app.py` to:

```python
print("Hello from main branch")
```

Commit:

```bash
git add app.py
git commit -m "Update greeting in main"
```

### Step 5: Merge Feature into Main

```bash
git merge feature
```

Expected conflict message:

```text
CONFLICT (content): Merge conflict in app.py
Automatic merge failed; fix conflicts and then commit the result.
```

### Step 6: Resolve the Conflict

Open `app.py`.

Replace the conflicting content with:

```python
print("Hello from main branch")
print("Hello from feature branch")
```

Save the file.

### Step 7: Complete the Merge

```bash
git add app.py
git merge --continue
```

### Step 8: Verify the Result

```bash
git status
git log --oneline --graph --all
python app.py
```

Expected Python output:

```text
Hello from main branch
Hello from feature branch
```

## 8. How to Abort a Merge

If you want to cancel a merge that is currently in progress:

```bash
git merge --abort
```

This attempts to restore the state before the merge began.

**Important:** If you had uncommitted changes before starting the merge, restoration may not always be possible. Commit or stash unrelated work before attempting complex merges.

## 9. How to Resolve a Rebase Conflict

Suppose you run:

```bash
git switch feature
git rebase main
```

Git reports a conflict.

### Resolution

1. Open the conflicted file.
2. Resolve the conflicting changes.
3. Save the file.
4. Stage the resolved file.

```bash
git add app.py
```

Continue:

```bash
git rebase --continue
```

If additional conflicts occur, repeat the process.

To cancel the rebase:

```bash
git rebase --abort
```

## 10. How to Resolve a Cherry-Pick Conflict

Suppose you run:

```bash
git cherry-pick <commit-hash>
```

Git reports a conflict.

Resolve the file, then:

```bash
git add app.py
git cherry-pick --continue
```

To cancel:

```bash
git cherry-pick --abort
```

**Note:** We already practiced cherry-pick conflict resolution during Day 5.

## 11. Merge vs Rebase vs Cherry-Pick Conflict

| Operation   | Continue Command             | Abort Command             |
| ----------- | ---------------------------- | ------------------------- |
| Merge       | `git merge --continue`       | `git merge --abort`       |
| Rebase      | `git rebase --continue`      | `git rebase --abort`      |
| Cherry-pick | `git cherry-pick --continue` | `git cherry-pick --abort` |

Always resolve the conflicting files and stage them before continuing.

## 12. Useful Troubleshooting Commands

### Show Working-Tree Status

```bash
git status
```

### Show Conflicting Changes

```bash
git diff
```

### List Files with Unresolved Conflicts

```bash
git diff --name-only --diff-filter=U
```

### Check for Conflict Markers

```bash
git grep -n -e '<<<<<<<' -e '=======' -e '>>>>>>>'
```

Review the results carefully because these strings may also appear legitimately in documentation or test data.

### View Branch History

```bash
git log --oneline --graph --all
```

## 13. Real-Time DevOps Scenario: Terraform Conflict

### Scenario

Two DevOps engineers modify the same Terraform security group configuration.

Developer A allows HTTPS access on port 443.

Developer B modifies the same configuration to change the allowed source CIDR.

A conflict occurs during integration.

### Recommended Resolution

1. Review both proposed changes.
2. Understand the intended network access.
3. Confirm security requirements.
4. Resolve the Terraform configuration.
5. Run formatting checks.

```bash
terraform fmt -check
```

6. Validate the configuration.

```bash
terraform validate
```

7. Generate and review a Terraform plan using the approved environment and backend.

```bash
terraform plan
```

8. Obtain the required review.
9. Merge only after validation succeeds.

**Important:** A successful Git merge does not guarantee that Terraform configuration is secure or correct.

## 14. Real-Time DevOps Scenario: CI/CD Pipeline Conflict

Two developers update the same GitHub Actions workflow.

One adds a security scanning job.

Another changes the Docker build process.

The merge produces a conflict in the workflow YAML.

### Resolution Approach

- Preserve both required jobs.
- Verify YAML syntax.
- Check job dependencies.
- Review permissions and secrets usage.
- Run the workflow in a safe branch.
- Merge after the checks pass.

Never resolve CI/CD conflicts simply by choosing one version without understanding the impact.

## 15. How to Prevent Frequent Merge Conflicts

- Pull or fetch changes regularly.
- Keep feature branches focused and short-lived.
- Communicate before editing shared configuration files.
- Use small, meaningful commits.
- Review Pull Requests early.
- Avoid unnecessary formatting changes.
- Coordinate infrastructure changes across teams.
- Test after resolving conflicts.

Conflicts cannot always be prevented, but good collaboration reduces their frequency.

## 16. Interview Questions

### Q1. What is a merge conflict?

A merge conflict occurs when Git cannot automatically reconcile changes and requires manual resolution.

### Q2. Does modifying the same file always create a conflict?

No. Git can often combine changes made to different parts of a file.

### Q3. What does HEAD represent in a regular merge conflict?

It generally represents the currently checked-out branch.

### Q4. How do you resolve a merge conflict?

Inspect the conflicting changes, edit the file to the correct final content, stage the resolution, and complete the merge.

### Q5. What is the difference between git merge --abort and git reset --hard?

`git merge --abort` attempts to cancel an in-progress merge.

`git reset --hard` resets the index and working tree to a target commit and can permanently discard uncommitted changes.

### Q6. Can conflicts occur during rebase?

Yes. Resolve the conflicts and use `git rebase --continue`.

### Q7. Can conflicts occur during cherry-pick?

Yes. Resolve the conflicts and use `git cherry-pick --continue`.

### Q8. How do you identify unresolved files?

Use:

```bash
git diff --name-only --diff-filter=U
```

### Q9. What should you do after resolving a Terraform merge conflict?

Run formatting, validation, plan review, and the team's approval process before applying changes.

### Q10. How do you handle a production-related merge conflict?

Understand both changes, coordinate with the relevant owners, resolve safely, run appropriate tests, and follow the protected-branch release process.

## 17. Weekend Hands-On Practice

**Status: Pending — Weekend Practice**

### Practice Checklist

- [ ] Create a separate Git repository.
- [ ] Create main and feature branches.
- [ ] Modify the same line differently.
- [ ] Trigger a merge conflict.
- [ ] Identify conflict markers.
- [ ] Resolve the conflict manually.
- [ ] Stage the resolved file.
- [ ] Complete the merge.
- [ ] Verify Git history.
- [ ] Practice `git merge --abort`.
- [ ] Practice a rebase conflict.
- [ ] Review cherry-pick conflict resolution.

## 18. Key Takeaways

- Merge conflicts require human decisions when Git cannot combine changes.
- Always understand both sides before resolving.
- VS Code provides tools to assist with conflict resolution.
- Merge, rebase, and cherry-pick have different continue and abort commands.
- Test the final result after resolving conflicts.
- Infrastructure and CI/CD conflicts require additional validation.
