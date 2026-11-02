# 个人理财投资管理系统（Financial Management）

这是一个以 AI 为核心助手的个人理财投资管理仓库：登记我的真实持仓与定投计划（黄金基金、其他基金、股票等），以文档形式制定投资计划，借助 AI 联网获取所持标的的最新行情，并通过定时任务自动分析行情、给出投资计划调整建议。

> ⚠️ **免责声明**：本仓库全部内容（含 AI 生成的分析与建议）仅为个人投资记录与决策辅助，不构成任何投资建议，所有最终交易决策由本人做出。

## 项目目标

1. **持仓登记** —— 集中登记基金（含黄金基金）、股票、ETF 等标的的持仓份额、成本与买卖记录。
2. **定投管理** —— 维护各标的的定投计划（频率、金额、暂停/恢复状态）。
3. **投资计划** —— 以文档形式制定总体策略与单项投资计划，并随行情迭代调整。
4. **行情跟踪** —— 利用 AI（联网搜索 / 数据接口）获取所持标的的最新净值与行情。
5. **自动化分析** —— 通过定时任务周期性拉取行情、生成分析报告、提出计划调整建议。

## 目录结构

```text
.
├── README.md               # 本文件
├── AGENTS.md               # AI 协作规范（AI 助手在本仓库的工作指南与安全红线）
├── .zcode/
│   └── agents/             # ZCode 自定义子代理（market-analyst 行情分析师）
├── openspec/               # OpenSpec 规格与变更管理
│   ├── project.md          # 项目上下文（给 AI 看的项目说明）
│   ├── specs/              # 现行能力规格（持仓登记 / 行情分析 / 投资计划）
│   ├── changes/            # 进行中的变更提案
│   └── template/           # 变更提案模板（proposal / tasks / design）
├── portfolio/              # 持仓与定投
│   ├── accounts.md         # 账户与资产总览（个人数据，不入库）
│   ├── accounts.TEMPLATE.md
│   ├── holdings/           # 持仓明细（每个标的一个文件，TEMPLATE.md 为模板）
│   ├── dca-plans.md        # 定投计划总表（个人数据，不入库）
│   └── dca-plans.TEMPLATE.md
├── plans/                  # 投资计划
│   ├── strategy.md         # 总体投资策略（个人数据，不入库）
│   ├── strategy.TEMPLATE.md
│   └── TEMPLATE.md         # 单项投资计划模板
├── analysis/               # 行情与分析
│   ├── watchlist.md        # 关注清单（个人数据，不入库）
│   ├── watchlist.TEMPLATE.md
│   └── reports/            # AI 生成的分析报告（个人报告不入库；TEMPLATE.md 为模板）
├── automation/             # 定时与自动化任务说明
├── journal/                # 投资决策日志
│   └── decisions.TEMPLATE.md
└── scripts/                # 行情抓取/分析脚本（预留）
```

## 快速开始

### 0. 新用户初始化（从模板创建你的数据文件）

本仓库推送时不包含任何个人财务数据；所有个人数据文件均从同名 `*.TEMPLATE.md` 模板复制创建：

| 复制模板 | 为 | 用途 |
| --- | --- | --- |
| `portfolio/accounts.TEMPLATE.md` | `portfolio/accounts.md` | 账户与资产总览 |
| `portfolio/dca-plans.TEMPLATE.md` | `portfolio/dca-plans.md` | 定投计划总表 |
| `plans/strategy.TEMPLATE.md` | `plans/strategy.md` | 总体投资策略（建议与 AI 对话共同填写并确认） |
| `journal/decisions.TEMPLATE.md` | `journal/decisions.md` | 投资决策日志 |
| `analysis/watchlist.TEMPLATE.md` | `analysis/watchlist.md` | 行情关注清单 |

个人持仓文件则复制 `portfolio/holdings/TEMPLATE.md`（见第 1 步）。这些数据文件已在 `.gitignore` 中忽略，你的后续使用数据不会被推送。

### 1. 登记一笔持仓

复制 `portfolio/holdings/TEMPLATE.md` 为 `<代码>-<slug>.md`（如 `518880-gold-etf.md`）填写持仓；同时在 `analysis/watchlist.md` 登记该标的，AI 才会跟踪它。

### 2. 建立定投计划

在 `portfolio/dca-plans.md` 总表加一行，并在对应持仓文件的「定投设置」补充细节。

### 3. 让 AI 做一次行情分析

直接对 AI 说：

> 读取 analysis/watchlist.md，获取其中所有标的的最新行情，按 analysis/reports/TEMPLATE.md 生成今天的分析报告，并结合 plans/strategy.md 给出计划调整建议。

也可以直接指名使用行情分析师子代理（`.zcode/agents/market-analyst.md`）。

### 4. 建立定时自动分析

参考 `automation/README.md`，用 ZCode 定时任务（Automations）或系统 cron，在交易日收盘后自动执行第 3 步的分析流程。

## AI 协作

- 本仓库面向 AI 优先：AI 助手的行为规范与安全红线见 [AGENTS.md](AGENTS.md)。
- 涉及新能力（数据源、定时流程、报表格式等）的变更，走 [openspec/](openspec/) 的提案流程：先写 proposal，确认后实施。

## 数据与隐私

个人财务数据（`accounts.md`、`dca-plans.md`、`holdings/` 明细、`strategy.md`、`decisions.md`、`watchlist.md`、`reports/` 报告）已在 `.gitignore` 中忽略，仓库仅推送框架、规范与模板，其他人可从模板初始化自己的数据。即便如此，**仍建议只使用私有远端**；AI 联网查行情时只发送标的代码/名称，不发送持仓金额（详见 AGENTS.md）。
