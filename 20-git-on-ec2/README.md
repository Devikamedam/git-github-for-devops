# Git on AWS EC2 — Day 6

## 1. What Is Git on EC2?

Git is a distributed version control system used to track source code changes.

AWS EC2 provides virtual servers where developers and DevOps engineers can install Git, clone repositories, and work with application or infrastructure code.

Git on EC2 is commonly used for:

- Development and testing environments
- Retrieving application source code
- Infrastructure automation
- CI/CD runners and build servers
- Troubleshooting deployments

**Important:** Production deployments should generally use controlled CI/CD pipelines rather than manually editing application code on EC2.

## 2. Real-World DevOps Architecture

```text
Developer Laptop
       |
       v
GitHub Repository
       |
       v
CI/CD Pipeline
       |
       v
Build and Test
       |
       v
Deployment Artifact
       |
       v
AWS EC2 Instance
       |
       v
Running Application
```

In a simple lab, an EC2 instance may clone a GitHub repository directly.

In production, a pipeline commonly builds and tests the application before deploying an approved artifact.

## 3. Prerequisites

Before starting, ensure you have:

- An AWS account
- An EC2 instance
- SSH access to the instance
- Appropriate network connectivity
- Permission to install packages
- A GitHub repository

For private repositories, you also need an approved authentication method.

## 4. Connect to EC2 Using SSH

Example:

```bash
ssh -i "my-key.pem" ec2-user@EC2_PUBLIC_IP
```

For Ubuntu:

```bash
ssh -i "my-key.pem" ubuntu@EC2_PUBLIC_IP
```

Replace the example values with your actual key file and EC2 public IP address.

The default username depends on the AMI.

**Security:** Verify the instance's SSH host-key fingerprint before accepting it. Keep your private key protected.

## 5. Install Git on Amazon Linux 2023

Update packages:

```bash
sudo dnf update -y
```

Install Git:

```bash
sudo dnf install git -y
```

Verify:

```bash
git --version
```

Expected output will resemble:

```text
git version 2.x.x
```

The installed version depends on the operating system repositories.

## 6. Install Git on Ubuntu

Update package information:

```bash
sudo apt update
```

Install Git:

```bash
sudo apt install git -y
```

Verify:

```bash
git --version
```

## 7. Configure Git Username and Email

Set your Git identity:

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

Verify:

```bash
git config --global --list
```

These values identify the author of new commits.

They do not authenticate you to GitHub.

## 8. HTTPS vs SSH Authentication

GitHub supports different methods of connecting to repositories.

| HTTPS                                                    | SSH                                                     |
| -------------------------------------------------------- | ------------------------------------------------------- |
| Uses HTTPS transport                                     | Uses SSH transport                                      |
| Public repositories can be cloned without authentication | SSH cloning normally requires configured authentication |
| Private operations may use tokens or credential managers | Uses SSH key authentication                             |
| Useful for developer and automation workflows            | Useful for key-based workflows                          |

Example HTTPS URL:

```text
https://github.com/USERNAME/REPOSITORY.git
```

Example SSH URL:

```text
git@github.com:USERNAME/REPOSITORY.git
```

Both are encrypted transport methods.

## 9. Clone a Public Repository Using HTTPS

Example:

```bash
git clone https://github.com/USERNAME/REPOSITORY.git
```

Enter the directory:

```bash
cd REPOSITORY
```

Check:

```bash
git status
git remote -v
```

Public repositories can normally be cloned without GitHub authentication.

## 10. Configure SSH Authentication for GitHub

For this practice, use a dedicated SSH key rather than copying your laptop's personal private key to EC2.

### Step 1: Check Existing SSH Keys

```bash
ls -la ~/.ssh
```

### Step 2: Generate an SSH Key

```bash
ssh-keygen -t ed25519 -C "ec2-github-practice"
```

For a dedicated practice key, choose a descriptive filename, such as:

```text
~/.ssh/github_ec2_practice
```

Protect the private key and use a passphrase where practical.

### Step 3: Display the Public Key

```bash
cat ~/.ssh/github_ec2_practice.pub
```

Copy only the public key.

Never share the private key.

### Step 4: Add the Key to GitHub

For a single repository, a read-only deploy key is often more appropriate than granting an EC2 instance broad personal account access.

In the GitHub repository:

Settings → Deploy keys → Add deploy key

Paste the public key.

Leave write access disabled unless there is a justified need.

For personal account authentication, GitHub also supports adding an SSH key under account settings.

### Step 5: Configure SSH to Use the Key

Edit:

```bash
nano ~/.ssh/config
```

Add:

```sshconfig
Host github-ec2-practice
    HostName github.com
    User git
    IdentityFile ~/.ssh/github_ec2_practice
    IdentitiesOnly yes
```

Save the file.

Set appropriate permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/github_ec2_practice
chmod 600 ~/.ssh/config
```

### Step 6: Test SSH Authentication

```bash
ssh -T github-ec2-practice
```

On the first connection, verify GitHub's SSH host-key fingerprint using GitHub's official documentation before accepting it.

A successful authentication test may display a message explaining that GitHub does not provide shell access.

## 11. Clone a Repository Using the SSH Alias

Use the configured SSH alias:

```bash
git clone git@github-ec2-practice:USERNAME/REPOSITORY.git
```

Replace USERNAME and REPOSITORY.

The SSH alias tells Git to use the dedicated key.

If you configured the key as a repository deploy key, it can access only the repository for which it was authorized.

## 12. Basic Git Commands on EC2

Check repository status:

```bash
git status
```

Display branches:

```bash
git branch -a
```

View commit history:

```bash
git log --oneline --graph -5
```

Retrieve remote updates:

```bash
git fetch origin
```

Update the current branch using a fast-forward pull when possible:

```bash
git pull --ff-only origin main
```

Inspect the remote:

```bash
git remote -v
```

**Note:** Replace `main` if your repository uses another branch.

## 13. Troubleshooting: Permission Denied (publickey)

Example error:

```text
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
```

Possible causes:

- Incorrect private key
- Public key not registered in GitHub
- SSH client selecting another key
- Incorrect repository permissions
- Wrong SSH URL or alias

Troubleshooting:

```bash
ssh -vT github-ec2-practice
```

Check the key configuration and GitHub permissions.

Do not expose private key contents in troubleshooting logs or screenshots.

## 14. Troubleshooting: Repository Not Found

Example:

```text
ERROR: Repository not found.
```

Possible causes:

- Incorrect repository name
- Incorrect owner or organization
- Private repository access not granted
- Deploy key registered for another repository

Verify the repository URL and permissions.

## 15. Troubleshooting: Git Command Not Found

Example:

```text
git: command not found
```

Check:

```bash
which git
git --version
```

Install Git using the appropriate package manager for your operating system.

## 16. Troubleshooting: SSH Connection Timeout

Example:

```text
ssh: connect to host EC2_PUBLIC_IP port 22: Connection timed out
```

Possible causes:

- Security group does not allow SSH from your authorized IP.
- Instance has no reachable public IP.
- Network ACL or route configuration blocks traffic.
- Instance is stopped or unreachable.
- Local network blocks outbound SSH.

Check:

1. EC2 instance state
2. Security group rules
3. Route tables and internet connectivity
4. Public IP or approved private connectivity
5. Network ACLs
6. Operating system SSH service

Avoid opening SSH port 22 to the entire internet unnecessarily.

For production environments, consider AWS Systems Manager Session Manager or an approved bastion/VPN architecture.

## 17. Real-World DevOps Scenario

### Scenario: Application Deployment on EC2

A company stores its application code in GitHub.

A CI/CD pipeline runs when changes are merged into the protected `main` branch.

Workflow:

```text
Developer
    |
    v
Feature Branch
    |
    v
Pull Request
    |
    v
Code Review
    |
    v
CI/CD Tests
    |
    v
Merge to Main
    |
    v
Build Artifact
    |
    v
Deploy to EC2
    |
    v
Health Checks
    |
    v
Monitoring and Alerts
```

For an AWS deployment, teams may use:

- GitHub Actions or Jenkins
- AWS CodeDeploy or Systems Manager
- Amazon S3 or an artifact repository
- EC2 instances
- CloudWatch monitoring
- IAM roles and least-privilege permissions

Git is responsible for version control, while the deployment tools deliver and manage application releases.

## 18. Security Best Practices

- Do not commit passwords, API keys, or private keys.
- Do not copy personal SSH private keys onto shared servers.
- Prefer narrowly scoped, read-only repository access.
- Rotate or revoke compromised credentials.
- Restrict SSH access to authorized networks.
- Use IAM roles for AWS service access where appropriate.
- Use approved CI/CD pipelines for production deployments.
- Avoid storing secrets in shell history or repository files.
- Keep Git and operating system packages updated.
- Use branch protection and code reviews.

## 19. Interview Questions

### Q1. Why do DevOps engineers install Git on EC2?

To retrieve and manage source code for development, testing, automation, and troubleshooting workflows.

### Q2. What is the difference between HTTPS and SSH cloning?

Both use encrypted transport. HTTPS commonly uses token-based or credential-manager authentication, while SSH uses key-based authentication.

### Q3. What is a GitHub deploy key?

An SSH key associated with a particular repository, often used to provide limited repository access to a server or automation system.

### Q4. Why should we avoid personal SSH keys on EC2?

Personal keys may grant broader access than the server requires. Dedicated, narrowly scoped credentials reduce risk.

### Q5. How do you troubleshoot Permission denied (publickey)?

Check SSH key selection, public key registration, repository permissions, and the remote URL.

### Q6. How do you update a cloned repository?

Use `git fetch` to retrieve updates or `git pull --ff-only` when a fast-forward update is appropriate.

### Q7. Should production EC2 servers always clone GitHub repositories directly?

No. Controlled CI/CD pipelines that deploy tested artifacts are often preferable.

### Q8. How can EC2 access a private GitHub repository securely?

Use an approved authentication method such as a narrowly scoped deploy key or supported automation credentials.

### Q9. What is the difference between Git authentication and AWS IAM authentication?

GitHub authentication controls repository access.

AWS IAM controls access to AWS resources and APIs.

### Q10. What is the recommended approach for production deployments?

Use a controlled CI/CD process with code review, automated testing, least-privilege access, deployment validation, and monitoring.

## 20. Weekend Hands-On Practice

**Status: Pending — Weekend Practice**

### Practice Checklist

- [ ] Launch or access an authorized EC2 instance.
- [ ] Connect using SSH or Session Manager.
- [ ] Install Git.
- [ ] Configure Git username and email.
- [ ] Clone a public repository using HTTPS.
- [ ] Check Git status and remotes.
- [ ] Generate a dedicated SSH key.
- [ ] Register the public key with GitHub.
- [ ] Test SSH authentication.
- [ ] Clone an authorized repository using SSH.
- [ ] Practice git fetch and pull.
- [ ] Review SSH and repository access security.

**Cost Reminder:** Stop or terminate unnecessary practice resources after finishing, and check for any remaining billable AWS resources.

## 21. Key Takeaways

- Git can be installed on Amazon Linux and Ubuntu EC2 instances.
- Git supports source code workflows on development and automation servers.
- HTTPS and SSH both provide encrypted transport.
- Dedicated, limited-scope credentials are preferable for server access.
- SSH keys must be handled securely.
- GitHub authentication and AWS IAM serve different purposes.
- Production deployments should use tested and controlled delivery workflows.
