# Output Styles 模块 - 输出样式系统

[根目录](../../../CLAUDE.md) > [src](../../) > [openharness](../) > **output_styles**

---

## 模块职责

Output Styles 模块提供输出样式系统，负责：

- **内置样式**: 提供默认、最小化、Codex 等内置样式
- **样式加载**: 从配置文件加载用户自定义样式
- **样式管理**: 支持样式的注册和切换

---

## 入口与启动

### 加载输出样式

**主要入口** (`loader.py`):
```python
from openharness.output_styles import load_output_styles, OutputStyle

# 加载所有样式（内置 + 用户自定义）
styles = load_output_styles()

for style in styles:
    print(f"{style.name} ({style.source}): {style.content}")
```

### 使用样式

```bash
# CLI 指定样式
oh --output-style codex -p "Explain this code"

# 或在 TUI 中切换
# 使用命令 /style codex
```

---

## 对外接口

### OutputStyle 数据类

```python
@dataclass(frozen=True)
class OutputStyle:
    name: str       # 样式名称
    content: str    # 样式内容/描述
    source: str     # 来源：builtin 或 user
```

### 内置样式

| 样式名 | 描述 | 来源 |
|--------|------|------|
| `default` | 标准 Rich 控制台输出 | builtin |
| `minimal` | 极简纯文本输出 | builtin |
| `codex` | 类 Codex 紧凑转录和工具输出 | builtin |

### API 函数

| 函数 | 描述 |
|------|------|
| `get_output_styles_dir()` | 获取用户样式目录 |
| `load_output_styles()` | 加载所有样式 |

---

## 关键依赖与配置

### 依赖库

- `pathlib`: Python 标准库
- `dataclasses`: Python 标准库
- `openharness.config.paths`: 配置路径工具

### 配置文件

**位置**: `~/.openharness/output_styles/*.md`

**格式**:
```markdown
---
name: my-style
---

# My Custom Output Style

Custom output formatting rules here.
```

---

## 数据模型

### 样式加载流程

```
内置样式 → 扫描用户目录 → 解析 .md 文件 → 合并样式列表 → 返回样式列表
```

---

## 测试与质量

### 测试覆盖

- **单元测试**: `tests/output_styles/` 目录（待补充）
- **集成测试**: TUI 样式切换测试

### 质量工具

- **类型检查**: mypy 严格模式
- **代码检查**: ruff

---

## 相关文件清单

| 文件 | 描述 |
|------|------|
| `__init__.py` | 模块导出 |
| `loader.py` | 样式加载器 |

---

## 常见问题 (FAQ)

**Q: 如何创建自定义输出样式？**
A: 在 `~/.openharness/output_styles/` 目录中创建 `.md` 文件，使用 YAML front matter 定义样式名称。

**Q: 内置样式有哪些？**
A: `default`（标准）、`minimal`（极简）、`codex`（紧凑转录）。

**Q: 如何在运行时切换样式？**
A: 使用 CLI 标志 `--output-style <name>` 或在 TUI 中使用 `/style <name>` 命令。

---

## 变更记录 (Changelog)

### 2026-04-15 - 模块文档初始化
- ✅ 创建 output_styles 模块完整文档
- 📝 记录内置样式和 API

---

*最后更新: 2026-04-15 14:23:59*
