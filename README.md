# Hermes Deployment & Troubleshooting Tutorial

本教程记录了如何通过 Git CLI 自动化部署 Hermes Agent 及处理常见的自动化推送 Bug。

## 核心部署流程
1. **初始化**：`git init`
2. **规范化分支**：`git branch -m main`
3. **远程配置**：`git remote add origin https://USERNAME:TOKEN@github.com/OWNER/REPO.git`
4. **推送**：`git push -u origin main`

## 常见 Bug 及修复方案
- **Repository not found**: 检查 Token 权限，确认 URL 拼写。
- **Remote origin already exists**: 执行 `git remote remove origin`。
- **分支冲突**: 确保本地与远程分支名一致，执行 `git branch -m main`。

## 为什么自动化推送会失败？
自动化脚本容易受系统 `credential.helper` 缓存干扰。推荐在自动化环境中，使用 `https://USER:TOKEN@github.com/` 格式直接嵌入 Token，避免系统凭据冲突。
