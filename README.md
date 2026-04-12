# Hermes 自动化部署与技能管理指南

本指南旨在帮助开发者快速配置 Hermes Agent 的自动化部署工作流，并提供针对常见环境挑战的解决方案。

## 1. 核心技能：GitHub 自动化部署 (github-deploy-custom)
本技能封装了标准 Git CLI 工作流，通过在 URL 中嵌入 Token 实现自动化推送，避免了凭据缓存带来的认证冲突。

### 部署步骤
1. **初始化仓库**：
   ```bash
   git init
   git branch -m main
   ```
2. **配置远程仓库并推送**：
   ```bash
   git remote remove origin
   git remote add origin https://USERNAME:TOKEN@github.com/OWNER/REPO.git
   git add .
   git commit -m "部署更新"
   git push -u origin main
   ```

## 2. 常见挑战与解决方案
在自动化部署过程中，我们总结了以下环境配置经验，以确保流程的鲁棒性。

### 身份认证优化
- **问题**：HTTPS 模式下使用 Token 认证失败。
- **优化**：在自动化脚本中，推荐使用 `https://USERNAME:TOKEN@github.com/...` 格式，直接嵌入 Token 绕过系统凭据缓存的干扰。

### 远程仓库管理
- **问题**：推送时出现 `Repository not found` 或 `remote origin already exists`。
- **优化**：在关联远程仓库前，先执行 `git remote remove origin` 清理旧配置。

### 分支同步规范
- **问题**：本地分支与远程分支不匹配（如 `master` vs `main`）。
- **优化**：统一执行 `git branch -m main` 确保分支名称规范化。

### 备份策略
- **策略**：使用 `rsync` 排除缓存（如 `audio_cache`、`cache`）以精简备份体积，仅保留关键的 `config.yaml`、`memories/` 和 `skills/`。

---
