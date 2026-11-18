# 高德 MCP 调用记录

## 调用

```text
server: amap-maps
tool: amap-maps_maps_text_search
keywords: 中关村 川菜馆
city: 北京市
types: 050000
```

## 结果摘要

调用成功，返回 20 条 POI。前 3 条为：

| 名称 | 地址 |
|---|---|
| 锦府盐帮（领展购物广场中关村店） | 丹棱街甲1号领展购物广场中关村6层605号 |
| 懒人盐府人家（中关村店） | 中关村南三街保福寺76号 |
| 椒爱水煮鱼川菜（新中关店） | 北京城区中关村大街19号新中关大厦地下一层B115 |

`maps_text_search` 的本次响应未返回评分字段，因此没有伪造评分；联调重点是验证 MCP 服务注册、工具选择和真实 POI 返回链路。

## 截图

![MCP 调用结果摘要](../screenshots/11-mcp-call-summary.png)

![MCP 原始工具结果](../screenshots/11-mcp-call.png)

