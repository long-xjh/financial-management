# AGENTS.md — AI 协作规范

本文件约束所有 AI 助手（ZCode 等）在本仓库中的行为。项目背景见 [README.md](README.md) 与 [openspec/project.md](openspec/project.md)。

## 项目定位

个人理财投资管理仓库：登记真实持仓与定投、制定投资计划、AI 获取行情并定时分析、迭代调整计划。数据层为 Markdown 文档，辅以 `scripts/` 脚本与定时任务（见 `automation/`）。

## 语言与格式约定

- 与用户交流、撰写文档一律使用**简体中文**；标的代码、字段名保留英文/数字。
- 金额默认人民币 CNY，其他币种必须显式标注；净值保留 4 位小数，比例保留 1 位小数。
- 日期一律 `YYYY-MM-DD`。

## 目录与数据约定

| 目录 | 用途 | 写入规则 |
| --- | --- | --- |
| `portfolio/holdings/` | 每个标的一个文件，命名 `<代码>-<slug>.md` | 新持仓基于 TEMPLATE.md 创建 |
| `portfolio/dca-plans.md` | 定投计划总表 | 一行一个计划，与持仓文件的「定投设置」保持一致 |
| `portfolio/accounts.md` | 账户与资产总览 | 持仓变动或月度复盘时更新 |
| `analysis/watchlist.md` | AI 获取行情的输入清单 | **新增持仓必须同步登记** |
| `analysis/reports/` | 分析报告，命名 `YYYY-MM-DD.md` | 每次分析生成当日报告，已有则追加 |
| `plans/` | 投资策略与单项计划 | 任何修改必须写明理由 |
| `journal/decisions.md` | 投资决策日志 | 每次重大决策追加一条 |

> 初始化提示：新用户将各 `*.TEMPLATE.md` 复制为对应数据文件后再使用（`accounts.TEMPLATE.md → accounts.md`、`dca-plans.TEMPLATE.md → dca-plans.md`、`strategy.TEMPLATE.md → strategy.md`、`decisions.TEMPLATE.md → decisions.md`、`watchlist.TEMPLATE.md → watchlist.md`）。个人数据文件已被 `.gitignore` 忽略，不会推送；若发现这些文件缺失，先引导用户完成模板复制。

## 行情获取

- 优先级：`scripts/` 下的数据脚本（若已实现）→ 联网搜索/网页读取（天天基金、东方财富、雪球等公开页面）。
- 基金：最新净值、累计净值、近 1 周/1 月/3 月/1 年涨跌幅；股票/ETF：现价、涨跌幅、成交量、关键新闻。
- 数据必须附带**时间戳与来源**；无法确认的数据标注「待核实」，**严禁编造**。

## 分析报告规范

- 按 `analysis/reports/TEMPLATE.md` 结构输出；结论必须区分「事实（行情数据）」与「观点（AI 判断）」。
- 建议必须具体可执行（如「518880 定投从 500 元/周调整为 300 元/周」），说明触发理由与风险，并标注「待确认」。

## 安全红线

1. 本仓库任何 AI 分析都**不构成投资建议**，报告中必须保留免责声明。
2. AI **不得执行任何真实交易**，只能修改文档、生成建议。
3. 修改 `portfolio/`（真实资产数据）与 `plans/strategy.md` 前，必须先向用户确认。
4. 联网查行情时**只发送标的代码/名称**，不得发送持仓金额、份额等隐私数据到外部服务。
5. 新增依赖、定时任务或对外网络调用，需先说明用途并获得用户同意。

## 变更流程（OpenSpec）

涉及新能力（新增数据源、定时分析流程、报表格式变更等）时，遵循 `openspec/` 流程：在 `openspec/changes/<change-id>/` 下基于 `template/` 创建 `proposal.md` 与 `tasks.md`，经用户确认后再实施，完成后将规格变更合入 `openspec/specs/`。
