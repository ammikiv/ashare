# 选股机 · A 股市场看板

**线上地址：<https://ammikiv.github.io/ashare/>**

| 路径 | 说明 |
|---|---|
| `index.html` | **最新一期**：内联契约 + 构建期预渲染 ⇒ **无 JS 也能读全** |
| `report.html` | 与 `index.html` 内容相同，供直链引用 |
| `history/` | 各期 **gzip 契约**（实测约 26 KB/期）+ `index.json` 索引 |
| `candidates/` | **候选列表静态 JSON API**（供 THS-ext 等外部应用消费） |

## 多期浏览

顶栏右侧的**期选择器**可切换历史期；也可用深链接 `?d=YYYYMMDD` 直接打开某期。

- **最新一期**永远是页面内联的那一份 —— 断网、禁用 JS、邮件客户端/微信里都能读。
- **历史期**按需 fetch 上面那份 gzip 契约再渲染 ⇒ **必须能执行 JS**。
  （不把每期都做成完整 HTML 的原因：那样每期约 478 KB，250 期就是 119 MB；
   而契约每期约 26 KB，250 期约 6.5 MB。）
- 历史期契约由本地 `script/export_history.py` 从各期 scan 快照**重算**，
  回测取「日期 ≤ 该期快照日」的最新非变体者（**不用未来数据**）。

## 更新方式

由本地 Astock 项目的 `script/publish_pages.py` 在每日定时任务中自动发布：

```
Astock/scan/output/report_{YYYYMMDD}.html  ->  index.html / report.html
Astock/scan/output/history/*.json.gz       ->  history/          ->  git push
Astock/scan/output/candidates/*            ->  candidates/       ->  git push
```

`history/` **只增不删**（站点上删掉一期就再也回不来了）。

## 候选列表 API（candidates/）

**固定 URL 永远指向最新一期**，数据日写在 `index.json` 的 `asof`。每个列表都是**纯 JSON 数组**，
成员为三元组 `{code, name, industry}`：

| 列表 | URL | 口径 |
|---|---|---|
| 强多头候选 | `candidates/list/qiangduotou.json` | `tier == "强多头"`（仅牛/震荡产出；熊市返回空） |
| 观察档候选 | `candidates/list/guancha.json` | `tier == "观察"` |
| 临界候选 | `candidates/list/linjie.json` | 门槛下 10 分内 ∧ conf=high ∧ 未入档 |
| 早期启动候选 | `candidates/list/zaoqi.json` | 熊市前 5% 分位 |
| 刚突破候选 | `candidates/list/tupo.json` | 刚突破 + 突破候选 |
| Spring候选 | `candidates/list/tanhuang.json` | 裸K校核通过 / 距失效位≥1% / 上限 20 |
| 昇腾产业链 | `candidates/list/shengteng.json` | 22 只，industry = 产业链环节 |

完整 URL 前缀：`https://ammikiv.github.io/ashare/candidates/`。
列表为空时返回 `[]`（应用应按空列表处理，勿读成"接口出错"）。

**本仓库文件由脚本生成，手工修改会在下一次发布时被覆盖。**

## 免责声明

本站是个人量化研究的**过程产物**，仅供记录与复盘，**不构成任何投资建议**。

- 全部数字来自公开行情数据，按固定口径自动计算，可能存在数据源误差、口径偏差与滞后。
- 页面中标注为「研究参考、非买入信号」的档位（如早期启动、Eve 预兆等）尤其不可作为交易依据。
- 使用本站内容作出的任何决策及其后果，由使用者自行承担。
