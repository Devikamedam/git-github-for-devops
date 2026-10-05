# Git Conflicts

## Overview

A Git conflict occurs when Git cannot automatically decide how different changes should be combined.

A common scenario is:

```text
Developer 1
     |
     | changes same file
     v
   GitHub
     ^
     | changes same file
     |
Developer 2
```

However, changing the same file does **not automatically mean** there will be a conflict.

The important question is whether Git can automatically combine the changes.

---

# 1. Same File, Different Lines

Suppose both developers start with:

```text
Line 1
Line 2
Line 3
```

Developer 1 changes Line 1:

```text
Updated Line 1
Line 2
Line 3
```

Developer 2 changes Line 3:

```text
Line 1
Line 2
Updated Line 3
```

Git can usually combine these automatically:

```text
Updated Line 1
Line 2
Updated Line 3
```

So:

```text
Same File
+
Different Non-Overlapping Changes
↓
Usually Automatic Merge
```

---

# 2. Same File, Same Line

Now suppose both developers modify the same line differently.

Original:

```text
server_port = 80
```

Developer 1:

```text
server_port = 8080
```

Developer 2:

```text
server_port = 9000
```

Git cannot automatically determine which value is correct.

This can produce a merge conflict.

```text
Same File
+
Same/Overlapping Content
+
Different Changes
↓
Git Cannot Decide
↓
Conflict
```

---

# 3. Two-Developer Scenario

Both developers start from:

```text
A --- B
```

Developer 1 creates:

```text
A --- B --- C
```

and pushes it.

Developer 2 creates:

```text
A --- B --- D
```

Now:

```text
        C   Remote
       /
A --- B
       \
        D   Local
```

Developer 2 tries:

```bash
git push
```

The push may be rejected because the remote branch contains commits that Developer 2 does not have.

This is a **push rejection / diverged-history situation**.

It is not automatically a merge conflict.

---

# 4. Synchronize the Repository

Developer 2 needs to integrate the remote changes.

For example:

```bash
git pull
```

Depending on the configured pull strategy, Git may merge or rebase.

Explicit examples are:

```bash
git pull --no-rebase
```

or:

```bash
git pull --rebase
```

If Git can combine the changes automatically, there may be no conflict.

If Git cannot reconcile overlapping changes, Git reports a conflict.

---

# 5. Conflict Markers

During a merge conflict, a file may contain markers similar to:

```text
<<<<<<< HEAD
server_port = 9000
=======
server_port = 8080
>>>>>>> incoming-change
```

These markers show the conflicting versions.

Conceptually:

```text
<<<<<<<
Current version

=======
Separator

>>>>>>>
Other version
```

---

# 6. Resolve the Conflict

The developer must inspect the conflicting code and decide the correct final content.

For example, after discussion the team decides:

```text
server_port = 8080
```

Remove the conflict markers and keep the correct content.

Then check:

```bash
git status
```

---

# 7. Stage the Resolved File

After resolving the conflict:

```bash
git add <filename>
```

Example:

```bash
git add config.txt
```

This tells Git that the conflict in that file has been resolved.

---

# 8. Complete a Merge

If the conflict occurred during a merge, after resolving and staging the files, complete the merge as appropriate.

For example:

```bash
git commit
```

Then:

```bash
git push
```

Flow:

```text
Conflict
   ↓
Edit File
   ↓
Remove Conflict Markers
   ↓
git add
   ↓
git commit
   ↓
git push
```

---

# 9. Complete a Rebase

If the conflict occurred during a rebase:

```bash
git add <filename>
```

Then continue:

```bash
git rebase --continue
```

There may be additional conflicts if multiple commits are being replayed.

After the rebase completes, continue with the appropriate push workflow.

---

# 10. Check Conflict Status

Use:

```bash
git status
```

Git reports which files need attention.

Typical workflow:

```bash
git status
```

Then:

```text
Identify conflicted file
        ↓
Open file
        ↓
Review both changes
        ↓
Choose/edit correct content
        ↓
Remove conflict markers
        ↓
git add <file>
        ↓
Complete merge/rebase
```

---

# 11. Push Rejection vs Merge Conflict

These are related concepts, but they are not the same.

## Push Rejection

Example:

```text
Remote:
A --- B --- C

Local:
A --- B --- D
```

A push may be rejected because the remote contains commits missing locally.

```text
Push Rejected
≠
Merge Conflict
```

First, the histories need to be integrated.

---

## Merge Conflict

A merge conflict happens when Git cannot automatically reconcile changes.

Example:

```text
Developer 1:
port = 8080

Developer 2:
port = 9000
```

Git needs human input to decide the final result.

---

# 12. Branch Conflict Scenario

Conflicts can also occur when merging branches.

Example:

```text
             feature
                |
                C
               /
A --- B -------+
               \
                D
                |
               main
```

Suppose both branches changed the same line differently.

When the branches are merged:

```text
feature
   ↓
 merge
   ↓
main
```

Git may report a conflict.

The conflict must be resolved before the merge can be completed.

---

# DevOps Example

Suppose two DevOps engineers modify the same Terraform configuration.

Original:

```hcl
instance_type = "t3.micro"
```

Engineer 1 changes it to:

```hcl
instance_type = "t3.small"
```

Engineer 2 changes it to:

```hcl
instance_type = "t3.medium"
```

Git cannot determine which infrastructure configuration the team wants.

```text
t3.small
    VS
t3.medium
```

The engineers must determine the correct configuration and resolve the conflict before merging.

---

# Conflict Resolution Workflow

Remember this flow:

```text
1. Detect conflict
       ↓
2. git status
       ↓
3. Open conflicted file
       ↓
4. Review both versions
       ↓
5. Edit final content
       ↓
6. Remove conflict markers
       ↓
7. git add <file>
       ↓
8. Complete merge/rebase
       ↓
9. git push
```

---

# Quick Revision

| Command / Concept       | Purpose                                       |
| ----------------------- | --------------------------------------------- |
| `git status`            | Shows conflicted files and repository state   |
| `git pull`              | Fetches and integrates remote changes         |
| `git pull --no-rebase`  | Pulls using merge behavior                    |
| `git pull --rebase`     | Pulls and rebases local commits               |
| `git add <file>`        | Marks the resolved file as staged             |
| `git commit`            | Can complete a conflict-resolved merge        |
| `git rebase --continue` | Continues a rebase after resolving a conflict |
| `git push`              | Sends completed local commits to the remote   |

---

# Interview Scenario

## Two developers changed the same file. Will there always be a conflict?

No.

```text
Same file + different non-overlapping lines
↓
Git can usually merge automatically
```

But:

```text
Same/overlapping content
+
Different changes
↓
Possible merge conflict
```

---

## Your push is rejected. Does that mean you have a merge conflict?

No.

A push rejection can simply mean:

```text
Remote has commits
        +
Local does not have them
        ↓
Push rejected
```

A conflict occurs only if Git cannot automatically reconcile changes during integration.

---

## How do you resolve a Git conflict?

```text
git status
↓
Open conflicted file
↓
Review conflict markers
↓
Edit correct final content
↓
git add <file>
↓
Complete merge/rebase
↓
git push
```

---

# Key Takeaway

```text
PUSH REJECTION
=
Remote history needs to be synchronized

CONFLICT
=
Git cannot automatically combine changes

RESOLUTION
=
Human decides the correct final content
```
