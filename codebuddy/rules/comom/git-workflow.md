# Git Workflow

## Commit Message Format
```
<type>: <description>

<optional body>
```

Types: feat, fix, refactor, docs, test, chore, perf, ci

## 可配置项（settings.json）
以下规则可由 `~/.codebuddy/settings.json` 控制：
- `gitWorkflow.disableAttribution`: 是否关闭 Attribution（默认 `true`）。
- `gitWorkflow.commitTypes`: 允许的提交类型列表。
- `gitWorkflow.requireFullHistory`: PR 时是否必须分析完整提交历史。
- `gitWorkflow.requireDiffAgainstBase`: PR 时是否必须使用 `git diff [base-branch]...HEAD`。
- `gitWorkflow.requirePrSummary`: 是否必须提供 PR 总结。
- `gitWorkflow.requireTestPlan`: 是否必须提供测试计划（包含 TODOs）。
- `gitWorkflow.requirePushWithUpstream`: 新分支推送时是否必须使用 `-u`。

## Pull Request Workflow

When creating PRs:
1. Analyze full commit history (not just latest commit)
2. Use `git diff [base-branch]...HEAD` to see all changes
3. Draft comprehensive PR summary
4. Include test plan with TODOs
5. Push with `-u` flag if new branch

> For the full development process (planning, TDD, code review) before git operations,
> see [development-workflow.md](~/.codebuddy/rules/common/development-workflow.md).
