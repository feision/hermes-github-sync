# hermes-github-sync

Hermes Agent 的双向同步与技能管理核心库。

## 开发与协作规范
- **双向同步**: 本仓库支持从远程拉取技能与配置，并支持推送本地优化后的脚本至 GitHub。
- **自动化提交署名**: 所有由 AI 辅助的提交均包含以下署名，以记录贡献：
    - 作者: `Gemini-3.1-flash-lite-preview`
    - 邮箱: `ai-assistant@hermes.agent`
- **Token 安全策略**: 
    - 本仓库通过 `git remote` URL 中嵌入的 `x-access-token` 进行认证。
    - 自动化脚本运行时，系统会自动从当前仓库的 `remote.origin.url` 动态解析 Token，无需明文存储。

## 版本历史
- **2026-04-13**:
    - 仓库更名为 `hermes-github-sync`。
    - 更新所有关联文档与远程 URL。
    - 规范化 AI 提交署名流程。
    - 记录 Token 动态解析策略至 README。
EOF
