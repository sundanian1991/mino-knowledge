---
pos: memory/tasks/system/tavily-mcp-guide.md
input: Tavily MCP 配置需求
output: 搜索工具使用指南
update: 配置变更时更新
---

# Tavily MCP 搜索配置指南

## 快速排查（其他AI用这个）

如果看到"MCP服务器未启用"错误，按顺序检查：

### 1. 检查配置文件位置和内容
```bash
# 配置文件在用户主目录
cat ~/.mcp.json
```

正确配置应该长这样：
```json
{
  "mcpServers": {
    "tavily": {
      "command": "npx",
      "args": ["-y", "tavily-mcp"],
      "env": {
        "TAVILY_API_KEY": "tvly-dev-xxx"
      }
    }
  }
}
```

### 2. 检查包名是否正确
- ❌ 错误：`@tavily/mcp-server`（这个包不存在）
- ✅ 正确：`tavily-mcp`

### 3. 检查API密钥
确保 `TAVILY_API_KEY` 环境变量已设置。

### 4. **重启桌面端**（最重要）
MCP配置修改后，必须**完全退出**客户端再重新打开（Cmd+Q），不是刷新窗口。

### 5. 手动验证服务
```bash
# 验证包存在
npm view tavily-mcp

# 验证服务可用
echo '{"jsonrpc":"2.0","id":1,"method":"tools/list"}' | npx -y tavily-mcp
```

如果返回了工具列表（tavily_search、tavily_extract等），说明服务本身没问题，是客户端没连上。

---

## 历史错误记录

### 错误包名
```json
{
  "command": "npx",
  "args": ["-y", "@tavily/mcp-server"]  // ❌ 这个包不存在
}
```

**现象**：MCP服务一直显示"未启用"，npm报404

**根因**：正确包名是 `tavily-mcp`，不是 `@tavily/mcp-server`

---

## 正确配置

### .mcp.json 配置
```json
{
  "mcpServers": {
    "tavily": {
      "command": "npx",
      "args": ["-y", "tavily-mcp"],
      "env": {
        "TAVILY_API_KEY": "你的API密钥"
      }
    }
  }
}
```

### 获取API密钥
访问 https://tavily.com 注册并获取免费API密钥。

---

## 统一搜索协议

```python
# 优先级顺序
1. tavily_search     # 实时网络搜索
2. memory_search     # 知识图谱记忆搜索
3. grep fallback     # 本地精确匹配
```

---

## 常见问题

| 问题 | 解决 |
|------|------|
| MCP服务未启用 | 检查包名是否为 `tavily-mcp`，重启桌面端 |
| 搜索配额用完 | 等待重置或检查API密钥类型 |
| 返回空结果 | 检查API密钥是否有效 |

---

*最后更新：2026-02-18*
