# 🌍 世界监控室 World Monitor

> 客观、宏观、直观地监测世界现状与变化。  
> A global situation awareness dashboard — 28 indicators, real-time data, Big History perspective.

---

## 🚀 Quick Start

```bash
git clone https://github.com/yuanwenchen2/world-monitor.git
cd world-monitor
pip install -r requirements.txt
python run.py --port 8001
```

打开浏览器访问 **http://localhost:8001**

---

## 📡 功能一览

### 仪表盘主页面 (`/`)

| 板块 | 内容 |
|------|------|
| 🔴 高能信号条 | 自动滚动 L3-L5 重大事件，悬停暂停 |
| 🗺 冲突态势地图 | ECharts 世界地图，12个热点，L4-5 波纹动画 |
| 📊 28个指标卡片 | 6大分类，SVG迷你趋势图，实时值+变化率 |
| ⏳ 重大事件时间线 | 8个高能信号事件，悬停展开详情 |
| 📈 趋势数据图表 | 28个全尺寸 ECharts 折线图，含完整历史数据 |
| 🕰 **大历史视角** | DTW 曲线匹配 · 预测走廊 · 文明脉搏 · 历史韵脚 |

### 大历史视角 — Big History Perspective

基于 **DTW（动态时间规整）算法**，在全部 28 个指标的完整历史中寻找与当前趋势形态最相似的历史片段，据此生成未来预测。

| 功能 | 说明 |
|------|------|
| 🔍 曲线匹配 | 选择任意指标，自动在全部 28 个指标中寻找 Top-5 最相似的历史曲线段 |
| 📡 预测走廊 | 基于"历史相似段之后发生了什么"，生成乐观/中枢/悲观三条预测路径 |
| 💓 文明脉搏 | 聚合全部 28 个指标的归一化趋势（1960-2026），展示人类文明整体状态 |
| 🎭 历史韵脚 | 10 组精心策划的跨时代结构性类比（1929→2008、1918→2020、1970s 滞胀→现在等） |

> *"History doesn't repeat itself, but it often rhymes."* — Mark Twain

### 后端 API

| Method | Path | 说明 |
|--------|------|------|
| GET | `/api/indicators` | 全部指标 JSON |
| GET | `/api/chart-data` | 趋势图表数据 |
| GET | `/api/summary` | 仪表盘摘要 + 采集器状态 |
| GET | `/api/conflict-hotspots` | 冲突热点坐标 |
| GET | `/api/events` | 信号事件列表 |
| GET | `/api/big-history/analogs?indicator_id=X` | DTW 历史相似曲线匹配 |
| GET | `/api/big-history/predictions?indicator_id=X` | 预测走廊数据 |
| GET | `/api/big-history/pulse` | 文明脉搏聚合数据 |
| GET/POST | `/admin/` | 数据管理面板 |
| POST | `/api/collect-now` | 手动触发数据采集 |
| GET | `/api/export/csv` `/api/export/json` | 数据导出 |
| GET | `/docs` | Swagger API 文档 |

---

## 🏗 架构

```
world-monitor/
├── run.py                          # 启动入口 (argparse: --host, --port)
├── requirements.txt                # 纯 Python，零 Node.js 依赖
├── app/
│   ├── main.py                     # FastAPI 应用工厂 + lifespan
│   ├── config.py                   # 分类/能量级/采集器配置
│   ├── models/
│   │   ├── schema.py               # SQLAlchemy ORM (Indicator, IndicatorValue, SignalEvent)
│   │   └── database.py             # SQLite 引擎与会话管理
│   ├── services/
│   │   ├── indicator_service.py    # 指标业务逻辑
│   │   └── big_history_service.py  # DTW 引擎 · 模拟发现 · 预测 · 文明脉搏
│   ├── routes/
│   │   ├── dashboard.py            # 仪表盘页面路由
│   │   ├── api.py                  # REST JSON API
│   │   ├── big_history.py          # 大历史视角路由
│   │   └── admin.py                # 数据管理路由
│   ├── collectors/
│   │   ├── scheduler.py            # APScheduler 定时采集
│   │   ├── yahoo_finance.py        # 新浪财经 + yfinance
│   │   ├── world_bank.py           # World Bank API
│   │   └── climate.py              # NOAA CO2 + NASA GISTEMP
│   └── templates/                  # Jinja2 模板
├── static/
│   ├── css/world-monitor.css       # 暗色主题主样式
│   ├── css/big-history.css         # 大历史视角样式
│   ├── js/dashboard.js             # ECharts 图表初始化
│   ├── js/big-history.js           # 大历史视角前端逻辑
│   └── data/echarts-world.js       # 世界地图 GeoJSON
└── data/
    ├── seed_data.py                # 28个指标 + 357条历史数据 + 12个冲突热点
    └── world_monitor.db            # SQLite 数据库 (自动创建)
```

### 技术栈

- **后端**: Python 3.12+ · FastAPI · SQLAlchemy · SQLite · Jinja2 · APScheduler
- **前端**: ECharts 5 · HTMX · 纯手写 CSS 暗色指挥中心主题
- **算法**: DTW (Dynamic Time Warping) 纯 Python 实现 · Sakoe-Chiba 带约束
- **设计哲学**: 零 Node.js 构建步骤 · 零新依赖 · 纯 Python 全栈

---

## 📊 28 个监测指标

| 分类 | 指标 |
|------|------|
| 🏛 政治 | 全球活跃武装冲突 · 安理会决议数 · 民主指数 · 贸易限制措施 |
| 📈 经济 | 联邦基金利率 · CPI · 非农就业 · GDP增速 · WTI原油 · 黄金 · 标普500 · 沪深300 · 10Y国债 |
| ⚔ 军事 | 全球军费 · 核弹头数 · 冲突死亡人数 |
| 🔬 技术 | 航天发射次数 · 半导体市场 · AI训练算力 · 可再生能源装机 |
| 🌾 民生 | 大气CO2浓度 · 温度距平 · 极端贫困人口 · 粮食价格 · 难民人数 · 全球人口 |
| 📊 综合 | 人类发展指数 · 全球和平指数 |

---

## ⚡ 能量等级

| Level | 标签 | 含义 |
|-------|------|------|
| 5 🔴 | 文明级 | 改变人类文明走向（核战争、AGI、气候临界点） |
| 4 🟠 | 战略级 | 战略影响（政策转向、金融危机） |
| 3 🟡 | 重大级 | 显著事件（选举、重大数据异动） |
| 2 🔵 | 关注级 | 值得留意（常规政策调整） |
| 1 ⚪ | 背景级 | 背景信息（常规统计发布） |

---

## 🤝 贡献

欢迎提交 Issue 和 Pull Request。未来计划：

- [ ] GDELT 实时冲突事件检测
- [ ] 新闻情感分析
- [ ] 邮件/Webhook 预警（L5 事件）
- [ ] Docker 部署
- [ ] 多用户认证

---

## 📄 License

MIT
