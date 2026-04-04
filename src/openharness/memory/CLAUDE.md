# Memory 模块 - 持久化内存系统

[根目录](../../../CLAUDE.md) > [src](../../) > [openharness](../) > **memory**

---

## 模块职责

Memory 模块提供持久化内存系统，负责：

- **CLAUDE.md 扫描**: 自动发现和注入项目上下文
- **MEMORY.md 管理**: 持久化跨会话知识
- **内存搜索**: 基于元数据和内容的搜索
- **会话恢复**: 保存和恢复会话状态

---

## 入口与启动

### MemoryManager

**主要入口** (`manager.py`):
```python
from openharness.memory import MemoryManager

manager = MemoryManager(cwd="/workspace")
context = manager.build_context()
```

### MemoryScanner

**内存扫描** (`scan.py`):
```python
from openharness.memory import MemoryScanner

scanner = MemoryScanner()
memories = scanner.scan_directory("/workspace")
```

---

## 对外接口

### CLAUDE.md 发现

**自动发现规则**:
- 从当前目录向上查找 `CLAUDE.md`
- 查找 `.claude/` 目录下的文档
- 支持多项目工作区

### MEMORY.md

**持久化内存格式**:
```markdown
# Project Memory

## Context
Project-specific context and notes.

## Decisions
Important decisions and rationale.

## Progress
Current work and progress tracking.
```

---

## 关键依赖与配置

### 依赖模块

- `pydantic`: 内存元数据验证
- `pathlib`: 路径处理
- `yaml`: YAML frontmatter 解析

### 配置参数

**内存目录**:
- `~/.openharness/memory/`: 用户内存目录
- `.claude/memory/`: 项目内存目录

**内存元数据**:
```python
class MemoryMetadata:
    name: str
    description: str
    type: str  # "context" | "decision" | "progress"
```

---

## 数据模型

### Memory

**内存定义**:
```python
class Memory:
    path: Path
    metadata: MemoryMetadata
    content: str
    mtime: float
```

### SearchResult

**搜索结果**:
```python
class SearchResult:
    memory: Memory
    score: float
    highlights: list[str]
```

---

## 测试与质量

### 测试覆盖

- **单元测试**: `tests/memory/` 目录
- **集成测试**: 内存系统端到端测试
- **搜索测试**: 内存搜索准确性测试

### 质量指标

- 类型注解覆盖率: 100%
- 文档字符串覆盖率: 90%+
- 测试通过率: 100%

---

## 常见问题 (FAQ)

### Q1: CLAUDE.md 如何被注入？

A: 自动发现并注入到系统提示词，优先级高于默认提示词。

### Q2: MEMORY.md 与 CLAUDE.md 有什么区别？

A: CLAUDE.md 是项目级上下文，MEMORY.md 是跨会话的持久化知识。

### Q3: 如何搜索内存？

A: 使用 `/memory` 命令或 `Brief` 工具搜索相关内存。

---

## 相关文件清单

### 核心文件

- `manager.py`: MemoryManager 内存管理器
- `memdir.py`: 内存目录操作
- `scan.py`: MemoryScanner 扫描器
- `search.py`: 内存搜索
- `paths.py`: 内存路径管理
- `types.py`: 内存类型定义

### 测试文件

- `tests/memory/test_manager.py`: 内存管理器测试
- `tests/memory/test_scan.py`: 扫描器测试
- `tests/memory/test_search.py`: 搜索测试

---

## 变更记录 (Changelog)

### 2026-04-04 22:29:01 - 模块文档初始化
- 创建 memory 模块文档
- 说明 CLAUDE.md 和 MEMORY.md 的区别
- 提供内存搜索指南

---

*最后更新: 2026-04-04 22:29:01*
*文档版本: 1.0.0*
