# Commands 模块 - 命令注册表

[根目录](../../../CLAUDE.md) > [src](../../) > [openharness](../) > **commands**

---

## 模块职责

Commands 模块提供命令注册表，负责：

- **命令注册**: 维护 54+ 命令的注册表
- **命令解析**: 解析用户输入的命令
- **命令执行**: 执行命令并返回结果
- **插件集成**: 支持插件提供的命令

---

## 入口与启动

### CommandRegistry

**主要入口** (`registry.py`):
```python
from openharness.commands import CommandRegistry

registry = CommandRegistry()
registry.register_all_default_commands()
```

---

## 对外接口

### 内置命令

**会话管理**:
- `/help`: 显示帮助信息
- `/resume`: 恢复之前的会话
- `/continue`: 继续最近的会话

**权限管理**:
- `/permissions`: 切换权限模式
- `/allow`: 允许工具调用
- `/deny`: 拒绝工具调用

**开发工具**:
- `/commit`: 创建 git 提交
- `/plan`: 创建实现计划
- `/test`: 运行测试
- `/review`: 代码审查

**系统管理**:
- `/config`: 管理配置
- `/memory`: 管理内存
- `/skill`: 加载技能

---

## 关键依赖与配置

### 依赖模块

- `openharness.plugins`: 插件系统

---

## 测试与质量

### 测试覆盖

- **单元测试**: `tests/commands/` 目录

---

## 变更记录 (Changelog)

### 2026-04-04 22:29:01 - 模块文档初始化

---

*最后更新: 2026-04-04 22:29:01*
