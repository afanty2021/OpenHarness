# Coordinator 模块 - 多智能体协调

[根目录](../../../CLAUDE.md) > [src](../../) > [openharness](../) > **coordinator**

---

## 模块职责

Coordinator 模块提供多智能体协调系统，负责：

- **子智能体生成**: 创建和启动子智能体
- **团队管理**: 管理智能体团队
- **任务协调**: 协调多个智能体的任务执行
- **后台执行**: 支持后台智能体任务

---

## 入口与启动

### CoordinatorMode

**主要入口** (`coordinator_mode.py`):
```python
from openharness.coordinator import CoordinatorMode

coordinator = CoordinatorMode()
await coordinator.spawn_subagent(
    prompt="Task description",
    context=execution_context,
)
```

---

## 对外接口

### Agent 工具

**Agent**: 创建子智能体
**SendMessage**: 发送消息给智能体
**TeamCreate**: 创建智能体团队
**TeamDelete**: 删除智能体团队

---

## 关键依赖与配置

### 依赖模块

- `openharness.swarm`: 智能体群管理
- `openharness.tasks`: 后台任务管理

---

## 测试与质量

### 测试覆盖

- **单元测试**: `tests/coordinator/` 目录

---

## 变更记录 (Changelog)

### 2026-04-04 22:29:01 - 模块文档初始化

---

*最后更新: 2026-04-04 22:29:01*
