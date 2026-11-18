# 第一轮提示词

```text
请帮我创建一个“校园美食地图助手”HTML 应用。

背景：项目通过高德地图 MCP 接入真实地图能力，但 HTML 页面无法直接调用 MCP，因此浏览器端使用高德 REST API，MCP 配置用于 CodeArts 辅助开发与联调。

安全要求：
1. 不要把真实 API Key 写入会提交到 Git 的文件。
2. 从项目本地 mcp_settings.json 读取 AMAP_MAPS_API_KEY，写入被 .gitignore 忽略的 config.local.js。
3. 创建 config.example.js，Key 留空。
4. 真实接口或地图加载失败时必须使用 mock-data.js 降级，保证页面可演示。

功能要求：
1. 页面加载时调用 IP 定位和逆地理编码，顶部显示详细地址；失败时显示示例地址并标记“示例位置”。
2. 支持关键词搜索餐厅，列表展示名称、距离、人均价格、评分、口味。
3. 支持“距离最近”和“评分最高”排序。
4. 右侧显示地图，餐厅用红色标记显示；点击列表或标记可以联动查看详情。
5. 高德 JS API 不可用时必须有可拖动、缩放、点击的本地示意地图，不能出现空白地图。
6. 桌面端左右分栏，移动端上下布局。

工程要求：
- 创建 index.html、styles.css、app.js、mock-data.js、config.example.js、config.local.js。
- 不依赖构建工具，可用静态服务器运行。
- 完成后检查文件结构、运行方式和实现摘要。
```

