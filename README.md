# 校园美食地图助手

实验 7-1：Vibe Coding 实战：多轮对话打造校园美食推荐小助手。

项目使用华为云 CodeArts Agent 通过三轮 Vibe Coding 对话完成。页面优先接入高德地图 Web 服务，网络、Key 权限或地图 JS API 不可用时自动切换到本地示意地图，保证演示和验收仍可进行。

## 功能

- IP 定位与逆地理编码：显示当前城市和详细地址，失败时显示示例位置。
- 校园周边 POI 搜索：支持川菜、粤菜、日料、西餐、小吃、奶茶等关键词。
- 餐厅列表：显示餐厅名称、距离、人均价格、口味、评分和推荐理由。
- 排序：距离最近、评分最高。
- 组合筛选：价格档次与口味类型可与关键词、排序同时使用。
- 地图标记：列表和地图双向联动，点击标记或卡片均可查看详情和推荐理由。
- 路线规划：调用高德步行路线接口，展示距离、时间和路线描述；真实接口失败时使用估算路线。
- 地图降级：高德 JS API 不可用时使用可拖动、缩放和点击的本地示意地图。
- 移动端适配：桌面端左右分栏，窄屏时地图与列表上下排列。

## 运行

本项目是无构建依赖的静态页面。

```powershell
cd D:\xuexiziliao\ai\lab7-campus-food-map
python -m http.server 8765
```

浏览器打开 <http://127.0.0.1:8765/>。

## 高德 Key 配置

真实 Key 只保存在本地忽略文件中，不提交到 Git。

1. 复制 [config.example.js](config.example.js) 为 `config.local.js`。
2. 在 `config.local.js` 中填写高德 Web 服务 Key。
3. 页面会从 `window.APP_CONFIG.amapKey` 读取 Key。
4. 不要把真实 Key 写入 `README.md`、HTML、CSS、JS 示例或截图。

`config.local.js`、`.codeartsdoer/mcp/mcp_settings.json` 已在 `.gitignore` 中排除。

## MCP 配置

高德 MCP 的配置示例见 [mcp_settings.example.json](mcp_settings.example.json)。真实配置文件路径为：

```text
.codeartsdoer/mcp/mcp_settings.json
```

该文件包含本地 Key，已被忽略。CodeArts 中的 MCP 联调记录见 [evidence/codearts-usage.md](evidence/codearts-usage.md) 和 [evidence/mcp-call.md](evidence/mcp-call.md)。

## 三轮迭代

### 第一轮：基础地图、定位与搜索

- 提示词：[prompts/round1.md](prompts/round1.md)
- 实现：IP 定位、逆地理编码、POI 搜索、列表排序、地图标记和本地 Mock 降级。
- 截图：[第一轮运行效果](evidence/screenshots/04b-round1-app.png)

### 第二轮：路线规划、推荐理由与筛选

- 提示词：[prompts/round2.md](prompts/round2.md)
- 实现：步行路线、估算路线降级、推荐理由、价格筛选、口味筛选和组合过滤。
- 截图：
  - [第二轮桌面界面](evidence/screenshots/05-round2-desktop.png)
  - [路线与筛选](evidence/screenshots/06-round2-route-filter.png)
  - [价格 + 口味组合筛选](evidence/screenshots/07-round2-combined-filter.png)
  - [第二轮移动端](evidence/screenshots/08-round2-mobile.png)

### 第三轮：距离修复与地图性能优化

- 提示词：[prompts/round3.md](prompts/round3.md)
- 实现：
  - 兼容 `distance` 与 `_distance` 字段，距离为 `0` 或非法时使用经纬度重新计算。
  - `enableHighResolution: false`
  - `zooms: [12, 18]`
  - `fadeEffect: false`
  - 高德标记每批 6 个分批渲染，并用批次 token 清理旧标记。
- 截图：
  - [第三轮桌面界面](evidence/screenshots/09-round3-final-desktop.png)
  - [第三轮移动端](evidence/screenshots/10-round3-final-mobile.png)

## CodeArts 与 MCP 证据

- [美食推荐 Skill 创建成功](evidence/screenshots/04-food-recommender-skill.png)
- [CodeArts MCP 调用结果](evidence/screenshots/11-mcp-call-summary.png)
- [CodeArts MCP 原始工具结果](evidence/screenshots/11-mcp-call.png)
- [AI 使用记录摘要](evidence/codearts-usage.md)
- [MCP 调用记录](evidence/mcp-call.md)
- [测试报告](evidence/test-report.md)

项目级技能文件同时保留在 [skills/food-recommender/SKILL.md](skills/food-recommender/SKILL.md)，CodeArts 项目级副本位于 `.codeartsdoer/skills/food-recommender/SKILL.md`。

## 测试结果

- JavaScript 语法检查：`app.js`、`mock-data.js`、`config.local.js` 通过。
- 默认数据：12 家餐厅、12 个地图标记。
- 组合筛选：平价 + 奶茶返回 2 家，标记数量同步为 2。
- 排序：评分最高按 `4.7` 到 `3.9` 降序排列。
- 距离：页面文本中没有 `0 米` 或 `0m`。
- 路线：Mock 数据下展示“约 534 米 / 步行约 7 分钟”的估算路线。
- 移动端：390px 宽度下无横向溢出，地图与列表正常上下排列。
- 浏览器运行日志：无页面异常。

## 提交结构

```text
.
├─ index.html
├─ styles.css
├─ app.js
├─ mock-data.js
├─ config.example.js
├─ mcp_settings.example.json
├─ prompts/
├─ skills/
└─ evidence/
   ├─ screenshots/
   └─ logs/
```

