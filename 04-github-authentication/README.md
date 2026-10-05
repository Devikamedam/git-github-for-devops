# GitHub Authentication

## Overview

When a local Git repository communicates with GitHub, authentication is required for operations that need permission, such as pushing code.

Common methods covered here are:

```text
1. HTTPS
2. SSH
3. Personal Access Token (PAT)
```

---

# 1. HTTPS

A GitHub repository can be cloned using an HTTPS URL.

Example:

```bash
git clone https://github.com/username/repository.git
```

Check the configured remote:

```bash
git remote -v
```

Example:

```text
origin  https://github.com/username/repository.git (fetch)
origin  https://github.com/username/repository.git (push)
```

---

# 2. SSH Authentication

SSH allows Git to communicate securely with GitHub using a key pair.

The key pair contains:

```text
SSH Key Pair
    |
    +---- Private Key
    |
    +---- Public Key
```

The basic idea is:

```text
Local Machine                         GitHub
     |                                   |
Private Key                        Public Key
     |                                   |
     +---------- Authentication ---------+
```

The **private key stays on your machine**.

The **public key is added to GitHub**.

---

# Generate an SSH Key

Use:

```bash
ssh-keygen
```

Follow the prompts to create the key pair.

SSH keys are normally stored inside the user's `.ssh` directory.

Example:

```text
~/.ssh/
```

The directory contains the private and public key files.

---

# Add the Public Key to GitHub

Copy the contents of the **public key**.

Then in GitHub:

```text
Settings
   ↓
SSH and GPG keys
   ↓
New SSH key
   ↓
Add the public key
```

Do not upload or share the private key.

---

# Clone Using SSH

Copy the SSH URL from the GitHub repository.

Example format:

```text
git@github.com:username/repository.git
```

Then:

```bash
git clone git@github.com:username/repository.git
```

Flow:

```text
Git Client
    |
    | SSH authentication
    v
GitHub
    |
    v
Repository
```

---

# HTTPS vs SSH

```text
HTTPS
  |
  +---- Uses an HTTPS remote URL

SSH
  |
  +---- Uses an SSH remote URL
  +---- Authentication uses SSH keys
```

Both can be used to work with GitHub repositories.

---

# 3. Personal Access Token

A Personal Access Token (PAT) can be used for GitHub authentication over HTTPS where applicable.

A token acts as a credential and should be protected like a password.

Conceptual process from the learning notes:

```text
GitHub Settings
      ↓
Developer Settings
      ↓
Personal Access Token
      ↓
Configure access/permissions
      ↓
Generate Token
```

The exact GitHub interface can change over time.

---

# Token Security

Never store a real token inside:

```text
README files
Git repositories
Application source code
Shell scripts committed to Git
Screenshots
```

Avoid examples such as:

```text
https://REAL_TOKEN@github.com/...
```

because credentials can leak through repository configuration, logs, shell history, or screenshots.

Use secure credential-management mechanisms instead.

---

# Check Current Remote

Use:

```bash
git remote -v
```

This helps determine whether the repository is currently using an HTTPS or SSH remote.

HTTPS example:

```text
https://github.com/username/repository.git
```

SSH example:

```text
git@github.com:username/repository.git
```

---

# Change Remote URL

A repository can switch between HTTPS and SSH without deleting and cloning it again.

Check the current remote:

```bash
git remote -v
```

Change `origin` to SSH:

```bash
git remote set-url origin git@github.com:username/repository.git
```

Verify:

```bash
git remote -v
```

The repository now uses the new URL for `origin`.

---

# Authentication Troubleshooting

If:

```bash
git push
```

fails because of authentication, check:

```bash
git remote -v
```

Then determine the authentication method:

```text
HTTPS URL?
    ↓
Check HTTPS credentials/token access

SSH URL?
    ↓
Check SSH key configuration
```

Also verify that the GitHub account has permission to access the repository.

---

# DevOps Security Practice

Credentials must not be committed to Git.

Never commit:

```text
Private SSH keys
Personal Access Tokens
Passwords
Cloud access keys
Application secrets
```

For DevOps repositories, secrets should be handled separately from source code.

---

# Quick Revision

| Item                              | One-line Purpose                                           |
| --------------------------------- | ---------------------------------------------------------- |
| HTTPS                             | Connects to GitHub using an HTTPS repository URL           |
| SSH                               | Provides key-based authentication to GitHub                |
| `ssh-keygen`                      | Generates an SSH key pair                                  |
| Public key                        | Added to GitHub for SSH authentication                     |
| Private key                       | Kept securely on the local machine                         |
| PAT                               | Credential that can authorize GitHub operations over HTTPS |
| `git remote -v`                   | Shows the currently configured remote URLs                 |
| `git remote set-url origin <url>` | Changes the URL associated with `origin`                   |

---

# Interview Scenario

## Git push suddenly fails with an authentication error. What would you check?

First:

```bash
git remote -v
```

Identify whether the remote uses:

```text
HTTPS
or
SSH
```

For SSH, check the SSH key configuration and repository access.

For HTTPS, check the configured credentials/token access and repository permissions.

---

# Key Takeaway

```text
HTTPS
↓
HTTPS remote connection

SSH
↓
Public/private key authentication

PAT
↓
Credential for HTTPS-based GitHub access

Most Important
↓
Never commit credentials or private keys to Git
```
