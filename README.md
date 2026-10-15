# Codex 工作流技能

面向 Codex 及兼容 Agent Skills 的中文工作流技能集合。

## 技能

- `bug-fix-guardrail`：先查历史 Bug 摘要和详细记录，再执行最小修复、验证并同步记录。
- `minimal-production-change-submit`：梳理未提交变更，生成中文 commit 信息，提交并推送当前分支。

## 安装

```bash
npx skills add ji8385186/codex-workflow-skills --skill bug-fix-guardrail
npx skills add ji8385186/codex-workflow-skills --skill minimal-production-change-submit
```

每个技能的完整说明及所需引用文件位于 `skills/<技能名>/`。
