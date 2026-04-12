# Hermes 部署与自动化推送故障排除指南

本教程详细记录了在 Hermes Agent 多实例环境中，自动化部署与 Git 远程推送时遇到的核心坑点及解决方案。

## 1. 认证机制的“坑”
### 问题：`401 Bad credentials` 与 `Password authentication not supported`
在 HTTPS 模式下，GitHub 已不再支持直接使用密码认证。如果使用 Token 且配置不当，Git 会报错“Password authentication is not supported”。

### 解决方案：
- **API 验证**：使用 `curl -H "Authorization: token $GITHUB_TOKEN" https://api.github.com/user` 验证 Token 权限是否包含 `repo` 作用域。
- **强制嵌入凭据**：为了避免系统 `credential.helper` 缓存旧凭据或配置错误，在自动化脚本中，直接将 Token 嵌入远程仓库 URL 中：
  `git remote add origin https://USERNAME:TOKEN@github.com/OWNER/REPO.git`

## 2. 远程推送的“坑”
### 问题：`remote: Repository not found` 或 `fatal: repository not found`
即使仓库存在，如果 URL 拼写错误或 Token 无权限，Git 也会返回此错误。

### 解决方案：
- **清理旧配置**：在添加新的远程地址前，务必执行 `git remote remove origin`，防止旧的（错误的）配置残留。
- **分支名标准化**：GitHub 现默认分支为 `main`，而 `git init` 默认可能创建 `master`。
  - 修复命令：`git branch -m main`。

## 3. 冲突与强制同步的“坑”
### 问题：`Updates were rejected because the remote contains work that you do not have locally`
当远程仓库已有提交（例如你在网页端创建了仓库，且仓库已包含初始化文件），本地无法直接推送。

### 解决方案：
- **强制拉取同步**：
  ```bash
  git fetch origin
  git reset --hard origin/main
  ```
  *注意：此操作会覆盖本地未提交的改动，请确保已备份重要文件。*

## 4. 自动化脚本的“坑”
### 问题：脚本执行超时或权限不足
在多用户/多实例环境下，备份脚本可能因为权限不足或数据量过大导致超时。

### 解决方案：
- **精简备份**：使用 `rsync` 排除 `audio_cache`、`cache`、`cron/output` 等非关键缓存目录。
- **权限管理**：涉及 `/root/.hermes` 的备份必须使用 `sudo`。
- **超时设置**：在脚本调用中增加 `timeout` 参数（例如 `timeout=300`）。

## 5. 总结：自动化推送最佳实践
1. **不要依赖系统凭据缓存**：在自动化环境中，使用 `https://USER:TOKEN@github.com/...` 格式。
2. **幂等性设计**：脚本中先清理再配置（如 `git remote remove origin`）。
3. **标准化分支**：统一使用 `main` 分支。
EOF

git add README.md
git commit -m "docs: update detailed deployment and troubleshooting guide"
git push -u origin main
