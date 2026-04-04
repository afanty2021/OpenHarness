# Prompts 模块 - 系统提示词与上下文

[根目录](../../../CLAUDE.md) > [src](../../) > [openharness](../) > **prompts**

---

## 模块职责

Prompts 模块提供系统提示词和上下文管理，负责：

- **系统提示词构建**: 组装系统提示词
- **CLAUDE.md 注入**: 发现并注入项目上下文
- **技能注入**: 按需注入技能内容
- **环境信息**: 收集环境和项目信息

---

## 入口与启动

### build_system_prompt

**主要入口** (`system_prompt.py`):
```python
from openharness.prompts import build_system_prompt

prompt = build_system_prompt(
    settings=settings,
    cwd="/workspace",
    skills=[],
    environment={},
)
```

---

## 对外接口

### 上下文注入

**CLAUDE.md**: 自动发现并注入项目上下文
**Skills**: 按需注入技能内容
**Environment**: 注入环境和项目信息

---

## 关键依赖与配置

### 依赖模块

- `openharness.memory`: 内存管理
- `openharness.skills`: 技能加载
- `openharness.config`: 配置管理

---

## 测试与质量

### 测试覆盖

- **单元测试**: `tests/prompts/` 目录

---

## 变更记录 (Changelog)

### 2026-04-04 22:29:01 - 模块文档初始化

---

*最后更新: 2026-04-04 22:29:01*
