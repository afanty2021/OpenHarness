# Tasks 模块 - 后台任务管理

[根目录](../../../CLAUDE.md) > [src](../../) > [openharness](../) > **tasks**

---

## 模块职责

Tasks 模块提供后台任务管理，负责：

- **任务创建**: 创建后台任务
- **任务状态**: 跟踪任务状态
- **任务输出**: 获取任务输出
- **任务停止**: 停止运行中的任务

---

## 入口与启动

### TaskManager

**主要入口** (`manager.py`):
```python
from openharness.tasks import TaskManager

manager = TaskManager()
task = await manager.create_task(
    name="my_task",
    func=async_function,
    args=[],
)
```

---

## 对外接口

### Task 工具

**TaskCreate**: 创建后台任务
**TaskGet**: 获取任务状态
**TaskList**: 列出任务
**TaskUpdate**: 更新任务
**TaskStop**: 停止任务
**TaskOutput**: 获取任务输出

---

## 关键依赖与配置

### 依赖模块

- `asyncio`: 异步任务执行

---

## 测试与质量

### 测试覆盖

- **单元测试**: `tests/tasks/` 目录

---

## 变更记录 (Changelog)

### 2026-04-04 22:29:01 - 模块文档初始化

---

*最后更新: 2026-04-04 22:29:01*
