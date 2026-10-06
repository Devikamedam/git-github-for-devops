# Git Cherry-Pick

## Overview

`git cherry-pick` is used to apply the changes introduced by a **specific commit** onto the current branch.

It is useful when we do not want to merge an entire branch and need only one selected commit.

```bash
git cherry-pick <commit-id>
```

---

## Merge vs Cherry-Pick

Suppose the `dev` branch contains:

```text
Base → A → B → C
```

If we run:

```bash
git merge dev
```

the changes reachable from `dev` are integrated into the current branch.

But if we need only commit `B`:

```bash
git cherry-pick <commit-id-of-B>
```

Git applies the change introduced by `B` to the current branch.

---

# Hands-On Practice

## Requirement

I created three separate commits on the `dev` branch:

| Commit | Change               |
| ------ | -------------------- |
| A      | Added Subtraction    |
| B      | Added Multiplication |
| C      | Added Division       |

The original `main` branch contained only:

```python
a = 10
b = 20

result = a + b

print("Addition:", result)
```

The requirement was:

> Bring only **Commit B - Multiplication** into `main` without bringing A or C.

---

## Step 1 - Dev Branch History

The `dev` branch contained:

```text
Base → A → B → C
```

In my practice:

```text
A = Subtraction
B = Multiplication
C = Division
```

I checked the commits using:

```bash
git log --oneline
```

Example from the practice:

```text
4e99dee C
4f0efa0 B
a0702dc A
f96d46f added app.py file
```

---

## Step 2 - Start from Main

I switched back to `main`:

```bash
git checkout main
```

Then created a new branch from `main`:

```bash
git checkout -b test
```

Why create `test`?

Because I wanted to:

1. Cherry-pick the selected commit.
2. Push the change to GitHub.
3. Raise a Pull Request.
4. Review the change before merging into `main`.

---

## Step 3 - Cherry-Pick Commit B

Commit B was:

```text
4f0efa0 B
```

I ran:

```bash
git cherry-pick 4f0efa0
```

The goal was:

```text
dev

Base → A → B → C
            |
            | cherry-pick B
            v

test

Base → B'
```

`B'` represents the cherry-picked copy of commit B.

The original B remains on `dev`.

---

# Cherry-Pick Conflict

During the cherry-pick, Git reported:

```text
CONFLICT (content): Merge conflict in app.py
error: could not apply 4f0efa0... B
```

The conflict appeared similar to:

```text
<<<<<<< HEAD
=======
sub = a - b
print("Subtraction:", sub)

mul = a * b
print("Multiplication:", mul)
>>>>>>> 4f0efa0 (B)
```

This happened because commit B had originally been created after commit A, while the new `test` branch was created from `main`.

Git could not automatically apply the selected patch to the different file state.

---

## Resolving the Conflict

The requirement was to bring the intended multiplication change into `test`.

The resolved `app.py` contained:

```python
a = 10
b = 20

result = a + b
print("Addition:", result)

mul = a * b
print("Multiplication:", mul)
```

Then I staged the resolved file:

```bash
git add app.py
```

And continued the cherry-pick:

```bash
git cherry-pick --continue
```

---

## Verify the Cherry-Pick

I checked the history:

```bash
git log --oneline
```

The `test` branch now contained a new commit for B.

Conceptually:

```text
dev:
Base → A → B → C

test:
Base → B'
```

The cherry-picked commit normally has a **different commit ID** because Git creates a new commit on the target history.

---

# Push the Test Branch

I pushed the new branch to GitHub:

```bash
git push -u origin test
```

Here:

```text
-u
```

sets the upstream relationship:

```text
local test <-> origin/test
```

After this, future commands can normally be:

```bash
git push
git pull
```

---

# Pull Request

After pushing `test`, I created a GitHub Pull Request.

```text
base: main
compare: test
```

Meaning:

```text
test
  |
  | Pull Request
  v
main
```

GitHub reported:

```text
No conflicts with base branch
```

The Pull Request was then merged successfully.

---

# Final Result

Before:

```text
main:
Base

dev:
Base → A → B → C
```

After cherry-pick and Pull Request:

```text
dev:
Base → A → B → C
            |
            | cherry-pick B
            v
test:
Base → B'
       |
       | Pull Request
       v
main:
Base → B'
```

The final `main` application contained:

```python
a = 10
b = 20

result = a + b
print("Addition:", result)

mul = a * b
print("Multiplication:", mul)
```

So:

```text
main
Addition
Multiplication
```

But `dev` still contained:

```text
Addition
Subtraction      ← A
Multiplication   ← B
Division         ← C
```

---

# Important Observation

Cherry-pick did **not** delete A, B, or C from `dev`.

When I switched back:

```bash
git checkout dev
```

and checked:

```bash
git log --oneline
```

the original commits were still present:

```text
C
B
A
Base
```

This demonstrates that cherry-pick **copies a selected commit's change** to another branch.

It does not move the original commit.

---

# Cherry-Pick vs Pull Request

These are different concepts.

### Cherry-Pick

```bash
git cherry-pick <commit-id>
```

Selects a particular commit and applies its change to the current branch.

### Pull Request

A Pull Request asks for changes from one branch to be reviewed and merged into another branch.

In this practice:

```text
dev
 |
 | select B
 v
test
 |
 | Pull Request
 v
main
```

---

# Why Not PR Directly from Dev?

A normal Pull Request:

```text
dev → main
```

would propose the changes on `dev` that are not already in `main`.

But our requirement was to deliver only the selected B change.

Therefore we used:

```text
main
 |
 | create test
 v
test
 |
 | cherry-pick B
 v
test with B'
 |
 | push
 v
origin/test
 |
 | Pull Request
 v
main
```

---

# Useful Cherry-Pick Commands

| Purpose                           | Command                       |
| --------------------------------- | ----------------------------- |
| Apply selected commit             | `git cherry-pick <commit-id>` |
| Continue after resolving conflict | `git cherry-pick --continue`  |
| Abort cherry-pick                 | `git cherry-pick --abort`     |
| Skip current commit               | `git cherry-pick --skip`      |
| View commit IDs                   | `git log --oneline`           |

---

# Troubleshooting

## Scenario 1 - Cherry-Pick Conflict

Error:

```text
CONFLICT (content): Merge conflict in app.py
```

Steps:

```bash
# Resolve the file manually

git add app.py

git cherry-pick --continue
```

---

## Scenario 2 - Cancel the Cherry-Pick

If I do not want to continue:

```bash
git cherry-pick --abort
```

This returns the branch to its state before the cherry-pick operation, when an active cherry-pick is in progress.

---

## Scenario 3 - Commit Depends on an Earlier Commit

Suppose:

```text
A → B
```

and B was developed based on code introduced by A.

Cherry-picking only B may cause:

- conflicts
- missing dependencies
- broken application behavior

So before cherry-picking, understand what changes the selected commit actually depends on.

---

# DevOps Use Case

Suppose a `dev` branch contains:

```text
Commit A → Terraform refactoring
Commit B → Critical security-group fix
Commit C → New monitoring configuration
```

Production urgently needs only the security fix.

Instead of merging all development work, we can select the required commit:

```bash
git cherry-pick <security-fix-commit-id>
```

Then test, review, and deliver only that change through the required branch/PR workflow.

---

# Key Takeaways

- Cherry-pick selects a specific commit.
- It does not merge the entire source branch.
- The original commit remains on the source branch.
- The target normally receives a new commit ID.
- Cherry-picking can cause conflicts.
- A selected commit may depend on earlier commits.
- Resolve conflicts, stage the file, and use `git cherry-pick --continue`.
- For reviewed workflows, cherry-pick onto a separate branch and raise a Pull Request to `main`.
