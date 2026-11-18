# CodeArts 使用记录

以下记录来自本次实验的 CodeArts Agent 会话时间线。记录只保留开发动作和验证结果，不包含 API Key、认证信息或完整聊天内容。

## 任务一：配置 MCP

- CodeArts 读取 `.codeartsdoer/mcp/mcp_settings.json` 并写入 `amap-maps` 配置。
- MCP Server 启动参数：`npx -y @amap/amap-maps-mcp-server`。
- 真实 Key 通过环境变量注入，未写入其他提交文件。
- 为防止密钥泄漏，根目录 `.gitignore` 同时忽略 MCP 配置和本地运行配置。

## 任务二：创建 food-recommender Skill

- 在 CodeArts 项目级技能目录创建 `.codeartsdoer/skills/food-recommender/SKILL.md`。
- 固化距离优先级、价格区间、口味分类、评分优先级和输出格式。
- 提交版本另存于 `skills/food-recommender/SKILL.md`。

## 任务三：三轮 Vibe Coding

### 第一轮

- CodeArts 生成 `index.html`、`styles.css`、`app.js`、`mock-data.js`、`config.example.js` 和本地 `config.local.js`。
- 完成 IP 定位、逆地理编码、POI 搜索、排序、地图联动和 Mock 降级。
- 证据：`evidence/screenshots/04b-round1-app.png`。

### 第二轮

- CodeArts 修改 `index.html`、`styles.css`、`app.js`，共 3 个文件。
- 增加价格/口味组合筛选、推荐理由、步行路线和估算路线。
- 证据：
  - `evidence/screenshots/05-round2-desktop.png`
  - `evidence/screenshots/06-round2-route-filter.png`
  - `evidence/screenshots/07-round2-combined-filter.png`
  - `evidence/screenshots/08-round2-mobile.png`

### 第三轮

- CodeArts 修改 `app.js`，共 1 个文件。
- 增加 `distance` / `_distance` 兼容、球面距离回退、地图性能参数和分批标记渲染。
- 证据：
  - `evidence/screenshots/09-round3-final-desktop.png`
  - `evidence/screenshots/10-round3-final-mobile.png`

## MCP 联调

CodeArts 实际调用：

```text
Tool: amap-maps_maps_text_search
Arguments:
  keywords: 中关村 川菜馆
  city: 北京市
  types: 050000
```

工具返回 20 条中关村周边餐饮 POI。本次 `maps_text_search` 返回字段包含 `id`、`name`、`address`、`typecode` 和 `photos`，未返回评分字段，因此联调记录中如实标注“接口未返回”。

证据：

- `evidence/screenshots/11-mcp-call-summary.png`
- `evidence/screenshots/11-mcp-call.png`
- `evidence/mcp-call.md`

## 关于 CodeArts 卡顿

第一次生成时，CodeArts 内置沙箱命令曾在文件校验阶段长时间没有返回，聊天界面停留在“命令运行中”。日志显示该命令被包装为 Git Bash 沙箱进程并触发 120 秒超时；更新并重启 CodeArts 后恢复。后续提示词已改为禁止执行 shell/沙箱命令，只直接读取和写入文件。

