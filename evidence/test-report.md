# 验收报告

## 环境

- 操作系统：Windows
- Node.js：24.18.0
- 浏览器：Google Chrome，Playwright 自动化
- 启动方式：`python -m http.server 8765`
- 页面地址：`http://127.0.0.1:8765/`

## 功能验收

| 项目 | 结果 | 证据 |
|---|---|---|
| 页面加载 | 通过，12 家示例餐厅正常展示 | `04b-round1-app.png`、`09-round3-final-desktop.png` |
| IP 定位/逆地理编码 | 已接入；本机联调时回退到示例位置，页面有明确提示 | `09-round3-final-desktop.png` |
| POI 搜索 | 已接入 `/v3/place/text`，失败时使用 Mock | `05-round2-desktop.png` |
| 距离排序 | 通过，默认按距离升序 | `05-round2-desktop.png` |
| 评分排序 | 通过，评分从 4.7 到 3.9 降序 | 自动化断言 |
| 地图标记 | 通过，12 家餐厅对应 12 个本地标记 | 自动化断言 |
| 列表/标记联动 | 通过，点击卡片后显示选中状态和路线按钮 | `06-round2-route-filter.png` |
| 价格筛选 | 通过，平价 `<30` 显示 5 家 | 自动化断言 |
| 口味筛选 | 通过，奶茶筛选正常工作 | `07-round2-combined-filter.png` |
| 组合筛选 | 通过，平价 + 奶茶显示 2 家，地图同步为 2 个标记 | `07-round2-combined-filter.png` |
| 推荐理由 | 通过，Mock 与真实数据均生成非空理由 | `05-round2-desktop.png` |
| 路线规划 | 通过，真实接口失败时使用估算路线 | `06-round2-route-filter.png` |
| 距离异常修复 | 通过，页面未出现 `0 米` 或 `0m` | 自动化断言 |
| 地图性能参数 | 代码中已加入 `enableHighResolution: false`、`zooms: [12, 18]`、`fadeEffect: false` | `app.js` |
| 分批标记渲染 | 代码中按每批 6 个标记分批加入地图 | `app.js` |
| 移动端 | 通过，390px 宽度无横向溢出 | `08-round2-mobile.png`、`10-round3-final-mobile.png` |
| 页面异常 | 通过，无 `pageerror` | 自动化断言 |

## 测试结果摘要

```text
JS syntax: PASS
default cards: 12
default markers: 12
low-price cards: 5
low-price + milk-tea cards: 2
filtered markers: 2
rating order: 4.7 -> 3.9
route: 约 534 米 / 步行约 7 分钟
zero-distance text: absent
mobile horizontal overflow: absent
page errors: 0
```

## 说明

本机提供的高德 Key 是 Web 服务 Key。高德 JS API 在当前 Key 权限下无法初始化，因此页面按设计切换到可交互的本地示意地图；REST 定位、POI 和步行路线能力仍保留，Mock 数据保证实验现场离线演示不空白。

