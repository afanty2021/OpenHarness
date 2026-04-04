# Plugins 模块 - 插件系统

[根目录](../../../CLAUDE.md) > [src](../../) > [openharness](../) > **plugins**

---

## 模块职责

Plugins 模块提供插件扩展系统，负责：

- **插件加载**: 从插件目录加载插件
- **插件安装**: 安装和卸载插件
- **插件管理**: 启用/禁用插件
- **兼容性**: 与 claude-code/plugins 兼容

---

## 入口与启动

### PluginLoader

**主要入口** (`loader.py`):
```python
from openharness.plugins import load_plugins

plugins = load_plugins(settings, cwd)
for plugin in plugins:
    if plugin.enabled:
        print(f"Loaded plugin: {plugin.name}")
```

### PluginInstaller

**插件安装** (`installer.py`):
```python
from openharness.plugins.installer import install_plugin_from_path

result = install_plugin_from_path("/path/to/plugin")
print(f"Installed plugin: {result}")
```

---

## 对外接口

### 插件结构

**插件目录结构**:
```
my-plugin/
├── .claude-plugin/
│   └── plugin.json       # 插件元数据
├── commands/             # 命令定义 (.md)
├── hooks/                # 钩子定义 (hooks.json)
├── agents/               # 智能体定义 (.md)
└── skills/               # 技能定义 (.md)
```

### 插件元数据

**plugin.json**:
```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "My custom plugin"
}
```

---

## 关键依赖与配置

### 依赖模块

- `openharness.commands`: 命令注册表
- `openharness.hooks`: 钩子系统
- `openharness.coordinator`: 智能体协调
- `pydantic`: 插件元数据验证

### 配置参数

**插件目录**:
- `~/.openharness/plugins/`: 用户插件目录
- `src/openharness/plugins/bundled/`: 内置插件目录

**插件元数据**:
```python
class PluginMetadata:
    name: str
    version: str
    description: str
    enabled: bool
    path: Path
```

---

## 数据模型

### Plugin

**插件定义**:
```python
class Plugin:
    name: str
    version: str
    description: str
    enabled: bool
    path: Path
    commands: list[Command]
    hooks: dict[str, Any]
    agents: list[Agent]
    skills: list[Skill]
```

### PluginSchema

**插件模式** (`schemas.py`):
```python
class PluginSchema(BaseModel):
    name: str
    version: str
    description: str
```

---

## 测试与质量

### 测试覆盖

- **单元测试**: `tests/plugins/` 目录
- **集成测试**: 插件加载和安装测试
- **兼容性测试**: claude-code/plugins 兼容性测试

### 质量指标

- 内置插件数量: 12+ 已测试
- 类型注解覆盖率: 100%
- 文档字符串覆盖率: 90%+
- 测试通过率: 100%

---

## 常见问题 (FAQ)

### Q1: 如何创建自定义插件？

A: 创建插件目录结构，添加 `plugin.json` 元数据，然后在相应子目录添加命令、钩子、智能体或技能。

### Q2: 插件与技能有什么区别？

A: 插件是功能扩展（命令、钩子、智能体、MCP 服务器），技能是知识文件（`.md`）。

### Q3: 如何管理插件？

A: 使用 `oh plugin list/install/uninstall` 命令管理插件。

---

## 相关文件清单

### 核心文件

- `loader.py`: PluginLoader 加载器
- `installer.py`: PluginInstaller 安装器
- `schemas.py`: 插件模式定义
- `types.py`: 插件类型定义
- `bundled/__init__.py`: 内置插件包

### 测试文件

- `tests/plugins/test_loader.py`: 加载器测试
- `tests/plugins/test_installer.py`: 安装器测试
- `scripts/test_real_skills_plugins.py`: 真实插件 E2E 测试

---

## 变更记录 (Changelog)

### 2026-04-04 22:29:01 - 模块文档初始化
- 创建 plugins 模块文档
- 提供插件开发指南
- 说明与 claude-code/plugins 的兼容性

---

*最后更新: 2026-04-04 22:29:01*
*文档版本: 1.0.0*
