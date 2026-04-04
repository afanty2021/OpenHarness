# Permissions 模块 - 权限与安全系统

[根目录](../../../CLAUDE.md) > [src](../../) > [openharness](../) > **permissions**

---

## 模块职责

Permissions 模块提供多级权限系统，负责：

- **权限检查**: 在工具执行前检查权限
- **权限模式**: 支持多种权限模式
- **路径规则**: 基于路径的访问控制
- **命令黑名单**: 危险命令拦截

---

## 入口与启动

### PermissionChecker

**主要入口** (`checker.py`):
```python
from openharness.permissions import PermissionChecker

checker = PermissionChecker(mode="default")
allowed = await checker.check_tool_use(
    tool_name="write",
    arguments={"file_path": "/etc/passwd"},
)
```

---

## 对外接口

### 权限模式

**default**: 默认模式，写操作和执行需要用户确认
**plan**: 计划模式，阻止所有写操作
**full_auto**: 全自动模式，允许所有操作（仅用于沙箱环境）

### 路径规则

**settings.json 配置**:
```json
{
  "permission": {
    "mode": "default",
    "path_rules": [
      {"pattern": "/etc/*", "allow": false},
      {"pattern": "*/.git/*", "allow": true}
    ],
    "denied_commands": ["rm -rf /", "DROP TABLE *"]
  }
}
```

---

## 关键依赖与配置

### 依赖模块

- `pydantic`: 权限配置验证
- `pathlib`: 路径匹配
- `fnmatch`: 模式匹配

### 配置参数

**PermissionChecker**:
```python
class PermissionChecker:
    mode: str  # "default" | "plan" | "full_auto"
    path_rules: list[PathRule]
    denied_commands: list[str]
```

---

## 数据模型

### PathRule

**路径规则**:
```python
class PathRule(BaseModel):
    pattern: str
    allow: bool
```

### PermissionResult

**权限检查结果**:
```python
class PermissionResult:
    allowed: bool
    reason: str | None
```

---

## 测试与质量

### 测试覆盖

- **单元测试**: `tests/permissions/` 目录
- **集成测试**: 权限系统集成测试
- **安全测试**: 危险命令拦截测试

### 质量指标

- 类型注解覆盖率: 100%
- 文档字符串覆盖率: 90%+
- 测试通过率: 100%

---

## 常见问题 (FAQ)

### Q1: 如何配置权限模式？

A: 在 `settings.json` 中设置 `permission.mode`，或使用 CLI `--permission-mode` 参数。

### Q2: 路径规则如何工作？

A: 使用 `fnmatch` 模式匹配，按顺序检查，第一个匹配的规则决定结果。

### Q3: 如何临时禁用权限？

A: 使用 `--dangerously-skip-permissions` 标志（仅用于沙箱环境）。

---

## 相关文件清单

### 核心文件

- `checker.py`: PermissionChecker 权限检查器
- `modes.py`: 权限模式定义

### 测试文件

- `tests/permissions/test_checker.py`: 权限检查器测试
- `tests/permissions/test_modes.py`: 权限模式测试

---

## 变更记录 (Changelog)

### 2026-04-04 22:29:01 - 模块文档初始化
- 创建 permissions 模块文档
- 说明三种权限模式
- 提供配置示例

---

*最后更新: 2026-04-04 22:29:01*
*文档版本: 1.0.0*
