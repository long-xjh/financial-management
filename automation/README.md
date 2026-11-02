# 定时与自动化

## 目标

让 AI 在固定时间（如每个交易日收盘后）自动完成：读取 watchlist → 拉取最新行情 → 生成当日分析报告 → 给出计划调整建议（待确认）。

## 方式一：ZCode 定时任务（推荐）

在 ZCode 会话中让 AI 建立 Automation，示例指令：

> 每个交易日 18:00，读取 analysis/watchlist.md，获取所有标的的最新行情，按 analysis/reports/TEMPLATE.md 生成当日分析报告；如建议调整投资计划，列出调整项等我确认，不要直接修改 portfolio/ 和 plans/。

## 方式二：系统 cron + 脚本

待 `scripts/` 行情抓取脚本就绪后配置，crontab 示例：

```cron
# 每个交易日 18:00 抓取行情（脚本待实现）
0 18 * * 1-5 cd /path/to/financial-management && python3 scripts/fetch_quotes.py >> logs/fetch.log 2>&1
```

## 现有任务登记

| 任务 | 触发时间 | 动作 | 状态 | 建立日期 |
| --- | --- | --- | --- | --- |
| 持仓行情分析与调仓触发检查（ZCode Automation `automation-3542202b`） | 每个交易日 **08:55**（cron `55 8 * * 1-5`，节假日自动识别并跳过触发判断；2026-10-29 由 18:00 改期，只能在 ZCode 应用运行时触发） | ① 基于昨日收盘+隔夜行情获取 watchlist 全部标的与宏观数据；② 逐项检查 plans/strategy.md 过渡执行规则（黄金反弹 ≥5%/单日 +2%、159509 名义溢价 >20%、020874→588080 合并平稳日、513770/015790 反弹、比例偏离 ±5pct、20% 回撤硬线预警、暴跌日禁令、8 周兜底至 2026-12-22）；③ 生成/追加 analysis/reports/YYYY-MM-DD.md，末尾输出「**今日**调仓操作指引」（执行/不执行/观察，全部待确认） | 运行中 | 2026-10-27（10-29 改期） |
