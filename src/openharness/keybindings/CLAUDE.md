# Keybindings 模块 - 键盘绑定系统

[根目录](../../../CLAUDE.md) > [src](../../) > [openharness](../) > **keybindings**

---

## 模块职责

Keybindings 模块提供键盘绑定系统，负责：

- **默认绑定**: 内置键盘快捷键映射
- **绑定加载**: 从配置文件加载用户自定义绑定
- **绑定解析**: 解析键盘绑定语法
- **绑定冲突解决**: 处理快捷键冲突

---

## 入口与启动

### 默认键盘绑定

**主要入口** (`default_bindings.py`):
```python
from openharness.keybindings import DEFAULT_KEYBINDINGS

# 默认绑定映射
print(DEFAULT_KEYBINDINGS)
# {
#     "ctrl+l": "clear",
#     "ctrl+k": "toggle_vim",
#     "ctrl+v": "toggle_voice",
#     "ctrl+t": "tasks",
# }
```

### 加载用户绑定

```python
from openharness.keybindings import load_keybindings, parse_keybindings

# 从配置文件加载
bindings = load_keybindings()

# 解析绑定字符串
parsed = parse_keybindings("ctrl+q: quit")
```

---

## 对外接口

### 默认绑定表

| 快捷键 | 动作 | 描述 |
|--------|------|------|
| `ctrl+l` | `clear` | 清除屏幕 |
| `ctrl+k` | `toggle_vim` | 切换 Vim 模式 |
| `ctrl+v` | `toggle_voice` | 切换语音模式 |
| `ctrl+t` | `tasks` | 打开任务面板 |

### API 函数

| 函数 | 描述 |
|------|------|
| `get_keybindings_path()` | 获取配置文件路径 |
| `load_keybindings()` | 加载用户绑定 |
| `parse_keybindings(text)` | 解析绑定语法 |
| `resolve_keybindings(bindings)` | 解决冲突 |

---

## 关键依赖与配置

### 依赖库

- `pathlib`: Python 标准库
- `openharness.config.paths`: 配置路径工具

### 配置文件

**位置**: `~/.openharness/keybindings.md`

**格式**:
```markdown
---
name: my-keybindings
---

# My Custom Keybindings

- `ctrl+q`: quit
- `ctrl+s`: save
- `ctrl+o`: open
```

---

## 数据模型

### 绑定数据结构

```python
dict[str, str]  # 快捷键 -> 动作
```

### 绑定解析流程

```
配置文件 → 解析器 → 验证 → 冲突解决 → 合并默认绑定 → 最终绑定表
```

---

## 测试与质量

### 测试覆盖

- **单元测试**: `tests/keybindings/` 目录（待补充）
- **集成测试**: TUI 键盘输入处理测试

### 质量工具

- **类型检查**: mypy 严格模式
- **代码检查**: ruff

---

## 相关文件清单

| 文件 | 描述 |
|------|------|
| `__init__.py` | 模块导出 |
| `default_bindings.py` | 默认键盘绑定映射 |
| `loader.py` | 绑定文件加载器 |
| `parser.py` | 绑定语法解析器 |
| `resolver.py` | 绑定冲突解决器 |

---

## 常见问题 (FAQ)

**Q: 如何自定义键盘绑定？**
A: 在 `~/.openharness/keybindings.md` 中创建配置文件，使用 Markdown 列表语法定义绑定。

**Q: 自定义绑定会覆盖默认绑定吗？**
A: 是的，用户自定义绑定会覆盖同名的默认绑定。

**Q: 支持哪些修饰键？**
A: 支持 `ctrl`, `alt`, `shift` 及其组合。

---

## 变更记录 (Changelog)

### 2026-04-15 - 模块文档初始化
- ✅ 创建 keybindings 模块完整文档
- 📝 记录默认绑定表和 API

---

*最后更新: 2026-04-15 14:23:59*
