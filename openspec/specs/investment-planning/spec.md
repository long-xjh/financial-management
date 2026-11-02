# investment-planning（投资计划与调整）规格

## Purpose

以文档形式维护总体投资策略与单项投资计划，并基于行情分析报告迭代调整，调整过程可追溯。

## Requirements

### Requirement: 策略总纲

系统 SHALL 在 `plans/strategy.md` 维护投资目标、风险偏好、资产配置比例与再平衡规则；单项投资计划不得与策略总纲冲突。

### Requirement: 单项投资计划

系统 SHALL 以 `plans/TEMPLATE.md` 为模板登记单项计划，包含背景逻辑、买入计划、止盈止损、退出条件与复盘。

### Requirement: 计划调整

系统 SHALL 保证对 `plans/` 的任何修改都记录理由；涉及 `portfolio/` 或 `plans/strategy.md` 的修改必须先经用户确认（见 AGENTS.md 安全红线）。

#### Scenario: 行情触发调整建议

- **WHEN** 分析报告建议调整某定投计划或配置比例
- **THEN** AI 给出具体可执行的修改建议并标注「待确认」
- **AND** 经用户确认后更新 `plans/` 与 `portfolio/dca-plans.md`，并在 `journal/decisions.md` 追加决策记录
