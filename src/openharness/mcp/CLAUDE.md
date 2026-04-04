# MCP 模块 - Model Context Protocol 客户端

[根目录](../../../CLAUDE.md) > [src](../../) > [openharness](../) > **mcp**

---

## 模块职责

MCP 模块提供 Model Context Protocol 客户端，负责：

- **MCP 客户端**: 连接和管理 MCP 服务器
- **配置管理**: 加载和管理 MCP 服务器配置
- **工具集成**: 将 MCP 工具集成到工具注册表

---

## 入口与启动

### MCPClient

**主要入口** (`client.py`):
```python
from openharness.mcp import MCPClient

client = MCPClient(config)
await client.connect()
tools = await client.list_tools()
```

---

## 对外接口

### MCP 工具

**MCPTool**: 调用 MCP 服务器提供的工具
**ListMcpResources**: 列出 MCP 资源
**ReadMcpResource**: 读取 MCP 资源

### CLI 命令

```bash
oh mcp list
oh mcp add <name> <config_json>
oh mcp remove <name>
```

---

## 关键依赖与配置

### 配置参数

**MCP 服务器配置**:
```json
{
  "mcp_servers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/allowed/files"]
    }
  }
}
```

---

## 测试与质量

### 测试覆盖

- **单元测试**: `tests/mcp/` 目录

---

## 变更记录 (Changelog)

### 2026-04-04 22:29:01 - 模块文档初始化

---

*最后更新: 2026-04-04 22:29:01*
