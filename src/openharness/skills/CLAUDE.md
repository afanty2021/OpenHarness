# Skills 模块 - 技能系统

[根目录](../../../CLAUDE.md) > [src](../../) > [openharness](../) > **skills**

---

## 模块职责

Skills 模块提供按需知识加载系统，负责：

- **技能加载**: 从 `.md` 文件加载技能定义
- **技能注册**: 维护技能注册表
- **技能调用**: 通过 `Skill` 工具调用技能
- **兼容性**: 与 anthropics/skills 兼容

---

## 入口与启动

### SkillLoader

**主要入口** (`loader.py`):
```python
from openharness.skills import SkillLoader

loader = SkillLoader()
skills = loader.load_skills_from_directory("~/.openharness/skills/")
```

### SkillRegistry

**技能注册表** (`registry.py`):
```python
from openharness.skills import SkillRegistry

registry = SkillRegistry()
registry.register_all(skills)

# 查找技能
skill = registry.find_skill("commit")
```

---

## 对外接口

### 内置技能

**commit**: 创建清晰、结构化的 git 提交
**review**: 审查代码的 bug、安全问题和质量
**debug**: 系统性地诊断和修复 bug
**plan**: 编码前设计实现计划
**test**: 为代码编写和运行测试
**simplify**: 重构代码使其更简单和可维护
**diagnose**: 使用运行产物追踪智能体运行失败和回归

### 技能格式

**Markdown frontmatter**:
```markdown
---
name: my-skill
description: Expert guidance for my specific domain
---

# My Skill

## When to use
Use when the user asks about [your domain].

## Workflow
1. Step one
2. Step two
```

---

## 关键依赖与配置

### 依赖模块

- `openharness.tools`: Skill 工具调用
- `pydantic`: 技能元数据验证
- `pathlib`: 路径处理

### 配置参数

**技能目录**:
- `~/.openharness/skills/`: 用户技能目录
- `src/openharness/skills/bundled/content/`: 内置技能目录

**技能元数据**:
```python
class SkillMetadata:
    name: str
    description: str
    type: str  # "instruction" | "tool-use"
    content: str
```

---

## 数据模型

### Skill

**技能定义**:
```python
class Skill:
    name: str
    description: str
    content: str
    metadata: dict[str, Any]
```

### SkillInput

**技能工具输入**:
```python
class SkillInput(BaseModel):
    name: str = Field(description="Name of the skill to invoke")
    query: str | None = Field(description="Optional query to pass to the skill")
```

---

## 测试与质量

### 测试覆盖

- **单元测试**: `tests/skills/` 目录
- **集成测试**: 技能加载和调用测试
- **兼容性测试**: anthropics/skills 兼容性测试

### 质量指标

- 内置技能数量: 7+
- 类型注解覆盖率: 100%
- 文档字符串覆盖率: 90%+
- 测试通过率: 100%

---

## 常见问题 (FAQ)

### Q1: 如何添加自定义技能？

A: 在 `~/.openharness/skills/` 目录创建 `.md` 文件，使用 frontmatter 定义元数据。

### Q2: 技能如何被调用？

A: 使用 `Skill` 工具或通过 `/skill` 命令调用，技能内容会被注入到系统提示词。

### Q3: 技能与插件有什么区别？

A: 技能是知识文件（`.md`），插件是功能扩展（命令、钩子、智能体、MCP 服务器）。

---

## 相关文件清单

### 核心文件

- `loader.py`: SkillLoader 加载器
- `registry.py`: SkillRegistry 注册表
- `types.py`: 技能类型定义
- `bundled/__init__.py`: 内置技能包
- `bundled/content/`: 内置技能内容

### 内置技能

- `bundled/content/commit.md`: commit 技能
- `bundled/content/review.md`: review 技能
- `bundled/content/debug.md`: debug 技能
- `bundled/content/plan.md`: plan 技能
- `bundled/content/test.md`: test 技能
- `bundled/content/simplify.md`: simplify 技能
- `bundled/content/diagnose.md`: diagnose 技能

### 测试文件

- `tests/skills/test_loader.py`: 加载器测试
- `tests/skills/test_registry.py`: 注册表测试

---

## 变更记录 (Changelog)

### 2026-04-04 22:29:01 - 模块文档初始化
- 创建 skills 模块文档
- 列出 7+ 内置技能
- 提供自定义技能开发指南

---

*最后更新: 2026-04-04 22:29:01*
*文档版本: 1.0.0*
