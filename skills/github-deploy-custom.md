---
name: github-deploy-custom
description: 使用 Git CLI 将文件或目录推送到 GitHub。
---

# GitHub 自定义部署技能

本技能使用标准的 Git CLI 工作流，通过在远程 URL 中嵌入 Token 实现自动化推送，避免了复杂的凭据缓存冲突。

## 执行步骤
1. 初始化仓库（如果尚未初始化）：
   ```bash
   git init
   git branch -m main
   ```
2. 配置远程仓库并推送：
   ```bash
   # 移除可能存在的旧远程配置
   git remote remove origin
   # 嵌入 Token 推送
   git remote add origin https://USERNAME:TOKEN@github.com/OWNER/REPO.git
   git add .
   git commit -m "部署更新"
   git push -u origin main
   ```

## 故障排查 (Bug Fixes)
- **Error: remote origin already exists**: 执行 `git remote remove origin`。
- **Error: Repository not found**: 检查 Token 权限及 URL 中的仓库名。
- **Error: src refspec main does not match**: 执行 `git branch -m main` 统一分支名。
- **自动化建议**: 脚本中优先使用 `https://USER:TOKEN@github.com/...` 格式，避免受系统 `credential.helper` 缓存干扰。
