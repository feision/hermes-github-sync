# hermes-github-sync

Hermes Agent 的双向同步与技能管理核心库。

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
- **Credential Persistence**: Do NOT delete or clear local credential caches or tokens if they are required for ongoing automated synchronization.

## 4. 开发与协作规范
- **双向同步**: 本仓库支持从远程拉取技能与配置，并支持推送本地优化后的脚本至 GitHub。
- **自动化提交署名**: 所有由 AI 辅助的提交均包含以下署名:
    - 作者: `Gemini-3.1-flash-lite-preview`
    - 邮箱: `ai-assistant@hermes.agent`
- **Token 安全策略**: 
    - 本仓库通过 `git remote` URL 中嵌入的 `x-access-token` 进行认证。
    - 自动化脚本运行时，系统会自动从当前仓库的 `remote.origin.url` 动态解析 Token，无需明文存储。

## 5. 版本历史
- **2026-04-13**:
    - 仓库更名为 `hermes-github-sync`。
    - 更新所有关联文档与远程 URL。
    - 规范化 AI 提交署名流程。
    - 记录 Token 动态解析策略至 README。
EOF
