# DevSecOps for Git & GitHub Security Practices

## Why Git Security Matters?

Git is not just source code storage.

Repositories often contain:

* Application Code
* Infrastructure as Code (Terraform)
* Kubernetes Manifests
* CI/CD Pipelines
* Secrets & Credentials
* Configuration Files

### Golden Rule

> **Git Never Forgets.**
>
> Once a secret is committed, it remains in Git history even if deleted later.

---

# 1. .gitignore

## Purpose

Prevent sensitive files from being tracked by Git.

### Common Files

```gitignore
.env
*.pem
*.key
terraform.tfstate
id_rsa
```

### Example

Without `.gitignore`

```bash
git add .
git commit -m "added changes"
```

Accidentally commits:

```bash
.env
DB_PASSWORD=admin123
```

### Interview Answer

`.gitignore` prevents accidental commits of sensitive or unnecessary files.

---

# 2. Pre-Commit Hooks

## Purpose

Run security checks before commit.

### Flow

```text
Developer
    ↓
git commit
    ↓
Pre-Commit Hook
    ↓
Pass / Fail
```

### Example

```python
secret="AWS_KEY"
```

Hook detects:

```text
Secret Found
Commit Blocked
```

### Interview Answer

Pre-commit hooks shift security left by preventing insecure code from entering Git.

---

# 3. Pre-Commit Framework

## Problem

Custom scripts become difficult to maintain.

Need to detect:

* Passwords
* Tokens
* API Keys
* AWS Secrets

## Solution

```bash
pip install pre-commit
```

Uses tools like:

* GitLeaks
* Trivy
* Checkov
* Bandit

### Benefits

* Standardized
* Reusable
* Easier Maintenance

---

# 4. GitLeaks

## Purpose

Detect secrets inside files.

### Detects

* AWS Keys
* Azure Keys
* GCP Keys
* GitHub Tokens
* Passwords
* JWT Tokens

### Command

```bash
gitleaks detect
```

### Example

```bash
AWS_SECRET_ACCESS_KEY=xyz
```

Output:

```text
Leak Detected
```

### Interview Answer

GitLeaks scans repositories for exposed credentials and sensitive information.

---

# 5. Repository Scanning

## Purpose

Scan entire Git history.

### Why?

Developers may have committed secrets months ago.

### Command

```bash
gitleaks detect
```

### Workflow

```text
Git History
    ↓
GitLeaks Scan
    ↓
Report
```

### Best Practice

Run monthly security scans across all repositories.

---

# 6. GitLeaks in CI/CD

## Purpose

Enforce security centrally.

### Flow

```text
Developer Push
      ↓
GitHub Actions
      ↓
GitLeaks
      ↓
Pass / Fail
```

### Benefits

Even if developers skip local checks:

* Pipeline blocks secrets
* PR fails automatically

### Interview Answer

CI/CD scanning provides centralized security enforcement.

---

# 7. Branch Protection Rules

## Purpose

Protect critical branches.

### Rules

* No Direct Push
* Pull Request Required
* Review Required
* CI Checks Required

### Workflow

```text
Feature Branch
      ↓
Pull Request
      ↓
Review
      ↓
Merge
      ↓
Main Branch
```

### Interview Answer

Branch protection prevents unauthorized changes from reaching production branches.

---

# 8. RBAC (Role Based Access Control)

## Principle

Least Privilege Access

### Example

| Role      | Permission  |
| --------- | ----------- |
| Developer | Write       |
| QA        | Read        |
| Team Lead | Maintain    |
| Admin     | Full Access |

### Interview Answer

RBAC minimizes risk by granting only the permissions required for a user's role.

---

# 9. Mandatory Reviews & CODEOWNERS

## Mandatory Reviews

Require:

```text
Minimum 2 Approvals
```

before merge.

---

## CODEOWNERS

Example:

```text
/payment-service/ @payments-team
/k8s/ @platform-team
/terraform/ @devops-team
```

### Workflow

```text
PR Created
     ↓
CODEOWNERS Identified
     ↓
Review Requested
     ↓
Approval Required
```

### Benefits

* Accountability
* Better Code Quality
* Security Validation

---

# 10. Dependabot

## Purpose

Automatically update vulnerable dependencies.

### Supports

* Maven
* NPM
* Python
* Go
* Docker

### Workflow

```text
Dependency
      ↓
Vulnerability Found
      ↓
Dependabot
      ↓
Pull Request Created
```

### Example

```text
log4j 2.14
```

Updated To

```text
log4j 2.17
```

### Interview Answer

Dependabot continuously monitors dependencies and automatically creates PRs for vulnerable packages.

---

# Enterprise Git Security Pipeline

```text
Developer
    ↓
.gitignore
    ↓
Pre-Commit Hook
    ↓
GitLeaks
    ↓
Git Push
    ↓
GitHub Actions
    ↓
Repository Scan
    ↓
Pull Request
    ↓
CODEOWNERS
    ↓
Mandatory Reviews
    ↓
Branch Protection
    ↓
Merge
    ↓
Dependabot Monitoring
    ↓
Production
```

---

# Interview One-Liners

### .gitignore

Prevents sensitive files from being tracked.

### Pre-Commit Hook

Runs validation before commit.

### GitLeaks

Detects secrets in repositories.

### Repository Scanning

Checks current and historical commits.

### CI/CD Security

Enforces security centrally.

### Branch Protection

Protects production branches.

### RBAC

Implements least privilege.

### Mandatory Reviews

Requires human validation.

### CODEOWNERS

Ensures domain experts review changes.

### Dependabot

Automatically updates vulnerable dependencies.

---

# Memory Trick

```text
Ignore
   ↓
Validate
   ↓
Scan
   ↓
Protect
   ↓
Review
   ↓
Approve
   ↓
Update
```

**Ignore → Validate → Scan → Protect → Review → Approve → Update**

This flow covers the complete Git DevSecOps lifecycle.
