# Hooks 模块 - 生命周期钩子系统

[根目录](../../../CLAUDE.md) > [src](../../) > [openharness](../) > **hooks**

---

## 模块职责

Hooks 模块提供生命周期钩子系统，负责：

- **PreToolUse**: 工具执行前触发
- **PostToolUse**: 工具执行后触发
- **热重载**: 钩子配置动态更新
- **插件集成**: 支持插件提供的钩子

---

## 入口与启动

### HookExecutor

**主要入口** (`executor.py`):
```python
from openharness.hooks import HookExecutor

executor = HookExecutor()
await executor.pre_tool_use(tool_name, arguments, context)
await executor.post_tool_use(tool_name, result, context)
```

---

## 对外接口

### 钩子类型

**PreToolUse**: 工具执行前触发，可用于参数验证、日志记录
**PostToolUse**: 工具执行后触发，可用于结果处理、通知

### 钩子配置

**hooks.json**:
```json
{
  "pre_tool_use": [
    {
      "name": "log_tool_use",
      "enabled": true
    }
  ],
  "post_tool_use": [
    {
      "name": "notify_result",
      "enabled": true
    }
  ]
}
```

---

## 关键依赖与配置

### 依赖模块

- `openhertools.plugins`: 插件系统
- `pydantic`: 钩子模式验证

### 配置参数

**钩子目录**:
- `~/.openharness/hooks/`: 用户钩子目录
- `plugins/*/hooks/`: 插件钩子目录

---

## 数据模型

### HookEvent

**钩子事件**:
```python
class HookEvent:
    type: str  # "pre_tool_use" | "post_tool_use"
    tool_name: str
    arguments: dict[str, Any]
    result: ToolResult | None
```

---

## 测试与质量

### 测试覆盖

- **单元测试**: `tests/hooks/` 目录
- **集成测试**: 钩子执行测试

---

## 变更记录 (Changelog)

### 2026-04-04 22:29:01 - 模块文档初始化

---

*最后更新: 2026-04-04 22:29:01*
