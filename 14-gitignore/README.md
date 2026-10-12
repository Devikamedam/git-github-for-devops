# Git .gitignore — Day 6

## 1. What is .gitignore?

`.gitignore` is a file that tells Git which untracked files and folders should be ignored.

It helps keep unnecessary and sensitive files out of the repository.

## 2. Why Do We Use .gitignore in DevOps?

- Avoid accidentally committing passwords and API keys.
- Exclude temporary files and logs.
- Ignore Python virtual environments.
- Prevent unnecessary generated files from being committed.
- Keep GitHub repositories clean.

## 3. Real-World Example

Suppose we are developing a Python application.

Project structure:

```text
cloud-app/
├── app.py
├── requirements.txt
├── .env
├── debug.log
├── .venv/
└── .gitignore
```

We want Git to track:

- `app.py`
- `requirements.txt`
- `.gitignore`

We do not want Git to track:

- `.env`
- `debug.log`
- `.venv/`

### Create a .gitignore file

```gitignore
# Environment variables
.env
.env.*

# Log files
*.log

# Python virtual environment
.venv/
venv/

# Python cache
__pycache__/
*.pyc

# Terraform state and local variables
*.tfstate
*.tfstate.*
*.tfvars
*.tfvars.json
.terraform/

# OS files
.DS_Store
Thumbs.db
```

**Important:** Do not ignore `.terraform.lock.hcl` by default. It should normally be committed for consistent provider versions.

## 4. Common .gitignore Patterns

| Pattern        | Meaning                            |
| -------------- | ---------------------------------- |
| `.env`         | Ignore the .env file               |
| `*.log`        | Ignore all .log files              |
| `*.pyc`        | Ignore Python compiled files       |
| `venv/`        | Ignore virtual environment folders |
| `temp/`        | Ignore directories named temp      |
| `*.tfstate`    | Ignore Terraform state files       |
| `!example.log` | Exception for example.log          |

## 5. Useful Git Commands

### Check Git status

```bash
git status
```

### Check whether a file is ignored

```bash
git check-ignore -v .env
```

### Add .gitignore to Git

```bash
git add .gitignore
```

### Commit the changes

```bash
git commit -m "Add gitignore configuration"
```

### Push to GitHub

```bash
git push origin main
```

## 6. What If a File Is Already Tracked?

Adding a file to `.gitignore` does not automatically stop Git from tracking it.

Example:

```bash
git rm --cached .env
```

This removes `.env` from Git's index but keeps the local file.

Then:

```bash
git add .gitignore
git commit -m "Stop tracking environment file"
git push origin main
```

If the file contained real secrets, rotate or revoke them immediately. Removing the file from the latest commit does not erase secrets from Git history.

## 7. Real-Time DevOps Scenario

**Scenario:** A developer accidentally includes `.env` while preparing a Python Flask application for GitHub.

**Problem:** The file may contain database passwords or API keys.

**Solution:**

1. Add `.env` to `.gitignore`.
2. Stop tracking the file if it was previously added.
3. Rotate any exposed credentials.
4. Store production secrets securely, for example in AWS Secrets Manager.
5. Add secret scanning to the CI/CD workflow.

## 8. Interview Questions

**Q1. What is .gitignore?**

A file containing patterns that tell Git which untracked files to ignore.

**Q2. Does .gitignore remove files already committed?**

No. Already tracked files must be untracked separately.

**Q3. What is the difference between .gitignore and git rm --cached?**

`.gitignore` defines ignore rules, while `git rm --cached` removes files from the Git index without deleting local copies.

**Q4. Should we commit Terraform state files?**

Generally no. Terraform state may contain sensitive information. Use a secure remote backend with appropriate access controls.

**Q5. Can .gitignore guarantee that secrets never reach GitHub?**

No. It is only one safeguard. Secret scanning, code reviews, and secure secret management are also necessary.

## 9. Hands-On Practice

**Status:** Pending — Weekend practice.

### Practice Tasks

1. Create a sample Python project.
2. Create `.env`, `app.py`, and `debug.log`.
3. Create a `.gitignore` file.
4. Run `git status`.
5. Verify ignored files using `git check-ignore -v`.
6. Practice stopping tracking of a harmless test file.

## 10. Key Takeaways

- `.gitignore` prevents matching untracked files from being added accidentally.
- It does not remove already tracked files.
- Sensitive files should not be committed.
- Terraform state, logs, and temporary files should be handled carefully.
- Use secret management and scanning for additional protection.
