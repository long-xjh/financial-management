# market-analysis（行情获取与定时分析）规格

## Purpose

由 AI 定期获取 watchlist 内标的的最新行情，生成结构化分析报告，并给出投资计划调整建议。

## Requirements

### Requirement: 行情获取

系统 SHALL 按 `analysis/watchlist.md` 清单获取每个标的的最新行情数据；基金为最新净值/累计净值与阶段涨跌幅，股票/ETF 为现价、涨跌幅与关键新闻。数据必须附带时间戳与来源，无法确认的标注「待核实」。

#### Scenario: 获取基金净值

- **WHEN** 分析任务运行
- **THEN** 逐个获取 watchlist 标的的最新行情，标注数据时间与来源

### Requirement: 分析报告

系统 SHALL 按 `analysis/reports/TEMPLATE.md` 生成当日报告（`analysis/reports/YYYY-MM-DD.md`），区分「事实」与「观点」，包含组合层面评估（对照 `plans/strategy.md`）与可执行建议，并保留免责声明。

#### Scenario: 定时分析

- **WHEN** 定时任务触发（见 `automation/`）
- **THEN** 生成或追加当日分析报告
- **AND** 当建议与当前 `plans/` 不一致时，在报告中明确列出差异，标注「待确认」

### Requirement: 子代理

系统 SHALL 通过 `.zcode/agents/market-analyst.md` 定义行情分析师子代理，供手动与定时分析复用。
