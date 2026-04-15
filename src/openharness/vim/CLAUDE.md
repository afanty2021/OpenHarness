# Vim 模块 - Vim 模式支持

[根目录](../../../CLAUDE.md) > [src](../../) > [openharness](../) > **vim**

---

## 模块职责

Vim 模块提供 Vim 模式支持，负责：

- **模式切换**: 在 Vim 模式和默认模式之间切换
- **状态管理**: 跟踪 Vim 模式启用状态
- **TUI 集成**: 与 React TUI 前端集成

---

## 入口与启动

### 切换 Vim 模式

**主要入口** (`transitions.py`):
```python
from openharness.vim import toggle_vim_mode

# 切换 Vim 模式
new_state = toggle_vim_mode(current_state)

# 在 TUI 中，这通常由键盘快捷键触发 (ctrl+k)
```

---

## 对外接口

### API 函数

| 函数 | 描述 |
|------|------|
| `toggle_vim_mode(enabled: bool) -> bool` | 切换 Vim 模式状态 |

### 与 AppState 集成

Vim 模式状态存储在 `AppState.vim_enabled` 字段中：

```python
from openharness.state import AppState

state = AppState(
    model="claude-sonnet-4-20250514",
    vim_enabled=True,  # Vim 模式已启用
)
```

---

## 关键依赖与配置

### 依赖库

- 无外部依赖（纯 Python 标准库）

### 配置项

| 配置 | 描述 |
|------|------|
| `vim_enabled` | AppState 中的布尔标志 |
| `ctrl+k` | 默认切换快捷键（见 keybindings 模块） |

---

## 数据模型

### Vim 模式状态机

```
默认模式 --(toggle_vim_mode)--> Vim 模式 --(toggle_vim_mode)--> 默认模式
```

### 与 TUI 集成

```
键盘输入 (ctrl+k) → Keybindings 解析 → toggle_vim_mode() → AppState 更新 → TUI 重新渲染
```

---

## 测试与质量

### 测试覆盖

- **单元测试**: `tests/vim/` 目录（待补充）
- **集成测试**: TUI Vim 模式切换测试

### 质量工具

- **类型检查**: mypy 严格模式
- **代码检查**: ruff

---

## 相关文件清单

| 文件 | 描述 |
|------|------|
| `__init__.py` | 模块导出 |
| `transitions.py` | Vim 模式切换函数 |

---

## 常见问题 (FAQ)

**Q: Vim 模式提供什么功能？**
A: Vim 模式在 TUI 中提供 Vim 风格的键盘导航和编辑功能。

**Q: 如何启用 Vim 模式？**
A: 按 `ctrl+k` 快捷键或在 AppState 中设置 `vim_enabled=True`。

**Q: Vim 模式支持哪些 Vim 命令？**
A: 当前实现较为基础，主要提供模式切换。完整的 Vim 命令支持在 TUI 组件中实现。

---

## 变更记录 (Changelog)

### 2026-04-15 - 模块文档初始化
- ✅ 创建 vim 模块完整文档
- 📝 记录 toggle_vim_mode API

---

*最后更新: 2026-04-15 14:23:59*
