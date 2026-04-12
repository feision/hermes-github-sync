# Hermes GitHub Pull Skills

Automated documentation for Hermes Agent deployment and Git synchronization workflows.

## 1. Core Workflow: Automated Push
Standardized process to bypass credential caching issues.

```bash
# 1. Initialize
git init
git branch -m main

# 2. Remote Configuration (Embedded Token)
git remote remove origin
git remote add origin https://USERNAME:TOKEN@github.com/OWNER/REPO.git

# 3. Execution
git add .
git commit -m "Automated update"
git push -u origin main
```

## 2. Troubleshooting & Error Resolution (Machine Readable)
Structured diagnostics for automated Git failures.

| Error | Cause | Resolution |
| :--- | :--- | :--- |
| **401 Bad credentials** | Invalid or expired PAT | Validate token via API: `curl -H "Authorization: token $TOKEN" https://api.github.com/user` |
| **Remote origin already exists** | Duplicate configuration | Run `git remote remove origin` before adding new remote |
| **Repository not found** | Incorrect URL or permissions | Verify repository visibility and URL spelling |
| **src refspec main does not match** | Branch name mismatch | Standardize branch: `git branch -m main` |
| **Updates were rejected** | Remote/Local divergence | Force sync: `git fetch origin && git reset --hard origin/main` |

## 3. Automation Best Practices
- **Credential Injection**: Always embed credentials in the URL (`https://USER:TOKEN@github.com/...`) to avoid system-level credential helper interference in non-interactive shells.
- **Idempotency**: Always cleanup local Git state (remove existing remotes, force branch renaming) before automated execution.
