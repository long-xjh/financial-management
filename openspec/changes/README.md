# 变更提案（Changes）

进行中的变更提案放在这里，每个提案一个目录：

```text
openspec/changes/<change-id>/
├── proposal.md   # 为什么改、改什么（必填，基于 ../template/proposal.md）
├── tasks.md      # 实施任务清单（必填）
└── design.md     # 技术设计（复杂变更才需要）
```

- `change-id` 命名：`add-<能力>` / `update-<能力>` / `remove-<能力>` 加短横线简述，如 `add-quote-fetcher`、`add-scheduled-analysis`。
- 流程：创建提案 → 用户确认 → 按 tasks.md 实施 → 将规格变更合入 `../specs/` → 删除或归档本提案目录。
