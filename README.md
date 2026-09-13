# ashare · A 股市场看板

**线上地址：<https://ammikiv.github.io/ashare/>**

| 文件 | 说明 |
|---|---|
| `index.html` | 最新一期市场看板（GitHub Pages 首页） |
| `report.html` | 与 `index.html` 内容相同，供直链引用 |

## 更新方式

由本地 Astock 项目的 `script/publish_pages.py` 在每日定时任务中自动发布：

```
Astock/scan/output/report_{YYYYMMDD}.html  ->  index.html / report.html  ->  git push
```

**本仓库文件由脚本生成，手工修改会在下一次发布时被覆盖。**

## 免责声明

本站是个人量化研究的**过程产物**，仅供记录与复盘，**不构成任何投资建议**。

- 全部数字来自公开行情数据，按固定口径自动计算，可能存在数据源误差、口径偏差与滞后。
- 页面中标注为「研究参考、非买入信号」的档位（如早期启动、Eve 预兆等）尤其不可作为交易依据。
- 使用本站内容作出的任何决策及其后果，由使用者自行承担。
