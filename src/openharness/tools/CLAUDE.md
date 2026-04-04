# Tools 模块 - 工具注册表与实现

[根目录](../../../CLAUDE.md) > [src](../../) > [openharness](../) > **tools**

---

## 模块职责

Tools 模块提供 43+ 内置工具，负责：

- **工具注册**: 维护所有可用工具的注册表
- **工具执行**: 安全地执行工具调用
- **权限集成**: 与权限系统集成
- **钩子支持**: PreToolUse/PostToolUse 生命周期事件

---

## 入口与启动

### ToolRegistry

**主要入口** (`registry.py`):
```python
from openharness.tools import ToolRegistry

registry = ToolRegistry()
registry.register_all_default_tools()

# 执行工具
result = await registry.execute_tool(
    tool_name="read",
    arguments={"file_path": "/path/to/file"},
    context=execution_context,
)
```

### BaseTool

**自定义工具** (`base.py`):
```python
from openharness.tools.base import BaseTool, ToolExecutionContext, ToolResult
from pydantic import BaseModel, Field

class MyToolInput(BaseModel):
    query: str = Field(description="Search query")

class MyTool(BaseTool):
    name = "my_tool"
    description = "Does something useful"
    input_model = MyToolInput

    async def execute(self, arguments: MyToolInput, context: ToolExecutionContext) -> ToolResult:
        return ToolResult(output=f"Result for: {arguments.query}")
```

---

## 对外接口

### 工具分类

**文件 I/O (6 个工具)**:
- `Read`: 读取文件内容
- `Write`: 写入文件（覆盖）
- `Edit`: 编辑文件（基于 diff）
- `Glob`: 文件模式匹配
- `Grep`: 内容搜索（基于 ripgrep）
- `Bash`: 执行 Shell 命令

**搜索工具 (4 个工具)**:
- `WebFetch`: 获取网页内容
- `WebSearch`: 网页搜索
- `ToolSearch`: 工具搜索
- `LSP`: 语言服务器协议

**智能体工具 (3 个工具)**:
- `Agent`: 创建子智能体
- `SendMessage`: 发送消息给智能体
- `TeamCreate/Delete`: 团队管理

**任务工具 (6 个工具)**:
- `TaskCreate`: 创建后台任务
- `TaskGet`: 获取任务状态
- `TaskList`: 列出任务
- `TaskUpdate`: 更新任务
- `TaskStop`: 停止任务
- `TaskOutput`: 获取任务输出

**MCP 工具 (3 个工具)**:
- `MCPTool`: 调用 MCP 工具
- `ListMcpResources`: 列出 MCP 资源
- `ReadMcpResource`: 读取 MCP 资源

**其他工具 (21+ 个工具)**:
- `NotebookEdit`: Jupyter 笔记本编辑
- `EnterPlanMode`: 进入计划模式
- `ExitPlanMode`: 退出计划模式
- `Worktree`: 工作树操作
- `CronCreate/List/Delete`: Cron 任务管理
- `RemoteTrigger`: 远程触发
- `Skill`: 技能加载
- `Config`: 配置管理
- `Brief`: 项目概要
- `Sleep`: 休眠
- `AskUser`: 询问用户

---

## 关键依赖与配置

### 依赖模块

- `openharness.permissions`: 权限检查
- `openharness.hooks`: 钩子执行
- `pydantic`: 输入验证
- `aiofiles`: 异步文件操作
- `httpx`: HTTP 请求

### 配置参数

**ToolRegistry**:
- 无需特殊配置，自动注册所有默认工具

**工具执行上下文** (`ToolExecutionContext`):
```python
class ToolExecutionContext:
    cwd: Path
    permission_checker: PermissionChecker
    hook_executor: HookExecutor | None
    tool_metadata: dict[str, object]
```

---

## 数据模型

### ToolResult

**工具执行结果**:
```python
class ToolResult:
    output: str | None
    error: str | None
    system_message: str | None
    base64_image: str | None
```

### ToolInput

**工具输入** (基于 Pydantic):
```python
class ReadInput(BaseModel):
    file_path: str = Field(description="Absolute path to file")
    offset: int | None = None
    limit: int | None = None
```

---

## 测试与质量

### 测试覆盖

- **单元测试**: `tests/tools/` 目录
- **集成测试**: 工具执行的端到端测试
- **权限测试**: 工具权限集成测试

### 质量指标

- 工具数量: 43+
- 类型注解覆盖率: 100%
- 文档字符串覆盖率: 95%+
- 测试通过率: 100%

---

## 常见问题 (FAQ)

### Q1: 如何添加自定义工具？

A: 继承 `BaseTool`，实现 `execute` 方法，然后使用 `registry.register_tool()` 注册。

### Q2: 工具权限如何工作？

A: 每个工具执行前都会通过 `PermissionChecker` 检查权限，可以在 `settings.json` 中配置路径规则和命令黑名单。

### Q3: 如何处理工具执行错误？

A: 工具执行错误会返回 `ToolResult(error=...)`，错误会被添加到对话历史，模型可以重试或调整策略。

---

## 相关文件清单

### 核心文件

- `base.py`: BaseTool 基类和工具上下文
- `registry.py`: ToolRegistry 注册表
- `file_io.py`: 文件 I/O 工具（Read, Write, Edit, Glob, Grep）
- `bash.py`: Bash 工具
- `search.py`: 搜索工具（WebFetch, WebSearch, ToolSearch）
- `agent.py`: 智能体工具（Agent, SendMessage, TeamCreate/Delete）
- `task.py`: 任务工具（TaskCreate/Get/List/Update/Stop/Output）
- `mcp.py`: MCP 工具（MCPTool, ListMcpResources, ReadMcpResource）
- `notebook.py`: NotebookEdit 工具
- `mode.py`: 模式工具（EnterPlanMode, ExitPlanMode, Worktree）
- `schedule.py`: 调度工具（CronCreate/List/Delete, RemoteTrigger）
- `meta.py`: 元工具（Skill, Config, Brief, Sleep, AskUser）

### 测试文件

- `tests/tools/test_registry.py`: 注册表测试
- `tests/tools/test_file_io.py`: 文件 I/O 工具测试
- `tests/tools/test_bash.py`: Bash 工具测试
- `tests/tools/test_search.py`: 搜索工具测试

---

## 变更记录 (Changelog)

### 2026-04-04 22:29:01 - 模块文档初始化
- 创建 tools 模块文档
- 列出 43+ 工具及其分类
- 提供自定义工具开发指南

---

*最后更新: 2026-04-04 22:29:01*
*文档版本: 1.0.0*
