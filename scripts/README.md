# 脚本目录

存放行情抓取与分析脚本（规划中）。涉及新脚本先走 `openspec/changes/` 提案。

规划中的脚本：

- `fetch_quotes.py`：读取 `../analysis/watchlist.md`，抓取各标的最新净值/行情，输出到 `../analysis/data/`（JSON），供报告生成使用。候选数据源：akshare、天天基金/东方财富公开接口。
