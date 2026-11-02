# portfolio-tracking（持仓与定投登记）规格

## Purpose

以 Markdown 文档登记基金/股票/ETF 的持仓明细与定投计划，作为 AI 行情分析与计划调整的数据基础。

## Requirements

### Requirement: 持仓登记

系统 SHALL 以单文件形式登记每个标的的持仓信息，包括代码、名称、类型、持仓份额、成本与买卖记录。

#### Scenario: 登记新基金

- **WHEN** 用户购入新基金（如某黄金基金）
- **THEN** 基于 `portfolio/holdings/TEMPLATE.md` 创建 `<代码>-<slug>.md` 并填写持仓
- **AND** 在 `analysis/watchlist.md` 同步登记该标的

#### Scenario: 更新持仓

- **WHEN** 用户追加买入或卖出
- **THEN** 在对应持仓文件的「交易记录」表追加一行，并更新持仓份额与成本

### Requirement: 定投计划管理

系统 SHALL 在 `portfolio/dca-plans.md` 维护全部定投计划总表（标的、频率、金额、状态），并与持仓文件中的「定投设置」保持一致。

#### Scenario: 暂停定投

- **WHEN** 用户决定暂停某标的的定投
- **THEN** 总表中该行状态改为「暂停」，注明日期与原因，持仓文件同步更新
