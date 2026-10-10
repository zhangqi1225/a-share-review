# A股复盘档案 · 少妇战法版

本地静态复盘网站，呈现每日收盘复盘数据。**双击 `index.html` 即可在浏览器打开**，无需安装、无需服务器。

## 文件说明
| 文件 | 作用 |
|------|------|
| `index.html` | 网站主文件（页面结构 + 样式 + 逻辑，单文件） |
| `data.js` | 复盘数据（内嵌，`window.REPORTS = {...}`），每日由脚本自动更新 |
| （可选）`README.md` | 本说明 |

## 打开方式
- **电脑**：双击 `index.html`，浏览器直接打开。
- **手机 / 云盘**：把两个文件放一起传到手机 / 华为云空间，用浏览器打开 `index.html`。

## 数据更新（三种方式）
1. **自动（推荐）**：每日 22:00 定盘脚本 `daily_report.py` 运行后会自动同步生成 `data.js`，网站自动更新最新复盘。
2. **手动重新生成**：改动 `data/gen_site_data.py` 后运行
   `python3 data/gen_site_data.py`，会从 `daily_reports.json` 重新生成 `data.js`。
3. **手工编辑**：直接改 `data.js` 里对应日期的字段（适合补板块/持仓/预案/正文）。

## data.js 数据结构
```js
window.REPORTS = {
  meta: { siteTitle, subtitle, updated },
  reports: [{
    date: "2026-10-09",
    title: "震荡修复 · 多数指数收涨 · 活跃市值回升",   // 复盘标题，可改
    confidence: "中等",                                // 判断置信度
    signal: "RANGE",                                   // FULL_POSITION / REDUCE / RANGE
    signalText: "震荡区间 · 半仓 · 只做B1",
    amv_yi: 154262.3,                                 // 活跃市值(亿)
    amv_pct: 1.25,
    rzrq_yi: 25741.6,                                 // 两融余额(亿)
    rzrq_pct: 0.17,
    rzrq_date: "2026-10-08",                          // 两融披露日
    amount_yi: 19001,                                 // 两市成交额(亿)
    amount_note: "",                                  // 成交额备注
    width: { up, down, flat, ratio },                 // 市场宽度
    index: {                                           // 指数：close/ pct / open/high/low(可选)
      "上证指数": { close:3813.79, pct:0.05, open:null, high:null, low:null },
      ...
    },
    sectors: [],                                       // 板块主线: 字符串 或 {name,rank,note}
    positions: [{ name, code, change, perf, conclusion }],  // 持仓回顾
    plan: { conclusion, direction, hold, watch, stop },     // 次日预案
    notes: "",                                         // 完整复盘正文(支持简单Markdown)
    source: "新浪行情 · 东财数据中心",
    dataQuality: ""
  }]
};
```

## 已有功能
- 顶部导航：复盘总览 / 完整报告 / 历史档案 / 数据来源
- 交易日历（圆点标记有复盘的日期）、历史档案列表、全局日期切换
- 收盘快照：活跃市值 / 两融 / 成交额 / 市场宽度（全部自动）
- 主要指数热力卡（红涨绿跌，底色深浅=涨跌幅）
- 择时研判（少妇战法：满仓 / 减仓 / 震荡半仓B1）
- **板块主线**（新浪行业板块涨幅榜 Top8 + 领涨股，自动）
- **涨停梯队**（东财涨停池：涨停家数/最高连板/连板分布/情绪主线行业聚合/高标股 Top10，自动）
- **龙虎榜**（东财个股榜去重：上榜数/净买 TOP5/净卖 TOP5，自动）
- 持仓回顾、次日预案（人工填写）
- 主题切换（浅色 / 深色 / 科学蓝）
- 导出 / 打印、复制 Markdown（含全部模块）

## 数据自动更新链路（每日 22:00 定盘）
```
daily_report.py（活跃市值/两融/指数/择时）
  → fetch_extra.py（板块/涨停池/龙虎榜/宽度/成交额）
  → gen_site_data.py（生成 data.js）
  → 网站 index.html 自动展示最新复盘
```

## 规划中（完整版 12 模块）
市场结构 · 多空对照 · 外围资产 · 三日资金 · 龙虎榜 · 连板梯队 ·
核心与波浪趋势 · 持仓条件 · 做T与调仓 · 次日预案 · 数据质量 · 交付归档
（页面已预留框架，按需逐步开放）

## 免责
本页面为个人复盘归档工具，数据来自公开接口，不构成投资建议。
