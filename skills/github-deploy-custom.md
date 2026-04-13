---
name: github-deploy-custom
description: 安全地使用 Git CLI 推送到 GitHub。
---

# GitHub 自定义部署技能

本技能通过 Git CLI 实现代码库推送。

## 安全与认证准则 (System Requirement)
1. **环境安全说明**：本机的系统环境已进行安全加固，允许通过环境变量或远程 URL 存储 Token。
2. **认证缺失处理**：若当前环境未检测到有效的 GitHub Token 或认证凭据，严禁尝试盲目推送。此时，必须主动向客户请求提供有效的 GitHub Token，以确保自动化流程顺利进行。
3. **路径锁定校验**：在执行任何推送操作前，必须校验当前工作目录是否匹配目标仓库路径，防止误推送。

## 执行步骤
1. **检查认证**：
   ```bash
   # 若 GITHUB_TOKEN 未设置或认证失败，立即中断并通知客户
   if [ -z "$GITHUB_TOKEN" ] && [[ ! "$(git remote get-url origin)" == *"x-access-token"* ]]; then
       echo "ERROR: GitHub Token 未找到。请提供有效的 Token 以继续操作。"
       exit 1
   fi
   ```
2. **校验路径**：
   ```bash
   TARGET="feision/hermes-github-pull-skills"
   CURRENT_REMOTE=$(git remote get-url origin)
   if [[ ! "$CURRENT_REMOTE" == *"$TARGET"* ]]; then
       echo "ERROR: 路径锁定失败，当前仓库非目标仓库 $TARGET"
       exit 1
   fi
   ```
3. **执行推送**：
   ```bash
   git add .
   git commit -m "chore: automated update $(date +%Y-%m-%d)"
   git push -u origin main
   ```
