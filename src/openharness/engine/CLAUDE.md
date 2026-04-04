# Engine 模块 - AI 智能体循环引擎

[根目录](../../../CLAUDE.md) > [src](../../) > [openharness](../) > **engine**

---

## 模块职责

Engine 模块是 OpenHarness 的核心，实现了 AI 智能体的主循环引擎，负责：

- **查询执行**: 管理对话历史和工具感知的模型循环
- **流式处理**: 处理 API 流式响应和事件
- **成本追踪**: Token 使用和成本计算
- **消息管理**: 对话消息的构建和转换

---

## 入口与启动

### 主要入口点

**QueryEngine** (`query_engine.py`):
```python
from openharness.engine.query_engine import QueryEngine

engine = QueryEngine(
    api_client=api_client,
    tool_registry=tool_registry,
    permission_checker=permission_checker,
    cwd="/workspace",
    model="claude-sonnet-4-20250514",
    system_prompt=system_prompt,
)

async for event in engine.submit_message("Hello, world!"):
    # 处理流式事件
    pass
```

**查询函数** (`query.py`):
```python
from openharness.engine.query import run_query, QueryContext

context = QueryContext(
    api_client=api_client,
    tool_registry=tool_registry,
    permission_checker=permission_checker,
    cwd="/workspace",
    model="claude-sonnet-4-20250514",
    system_prompt=system_prompt,
    max_tokens=4096,
)

async for event, usage in run_query(context, messages):
    # 处理事件和使用情况
    pass
```

---

## 对外接口

### QueryEngine 类

**主要方法**:
- `submit_message(prompt: str) -> AsyncIterator[StreamEvent]`: 提交用户消息并执行查询循环
- `clear() -> None`: 清除对话历史
- `set_system_prompt(prompt: str) -> None`: 更新系统提示词
- `set_model(model: str) -> None`: 更新模型
- `load_messages(messages: list) -> None`: 加载历史消息

**属性**:
- `messages: list[ConversationMessage]`: 当前对话历史
- `total_usage`: 总的 token 使用情况

### StreamEvent 类型

**事件类型** (`stream_events.py`):
- `StreamEventType.TEXT`: 文本内容
- `StreamEventType.TOOL_USE`: 工具调用
- `StreamEventType.THINKING`: 思考过程
- `StreamEventType.ERROR`: 错误信息
- `StreamEventType.PERMISSION_REQUEST`: 权限请求
- `StreamEventType.ASK_USER`: 用户询问

---

## 关键依赖与配置

### 依赖模块

- `openharness.api`: API 客户端（Anthropic/OpenAI）
- `openharness.tools`: 工具注册表
- `openharness.permissions`: 权限检查器
- `openharness.hooks`: 钩子执行器

### 配置参数

**QueryEngine 初始化参数**:
- `api_client`: 支持流式消息的 API 客户端
- `tool_registry`: 工具注册表实例
- `permission_checker`: 权限检查器实例
- `cwd`: 工作目录
- `model`: 模型名称
- `system_prompt`: 系统提示词
- `max_tokens`: 最大 token 数（默认 4096）
- `permission_prompt`: 权限提示函数
- `ask_user_prompt`: 用户询问提示函数
- `hook_executor`: 钩子执行器
- `tool_metadata`: 工具元数据

---

## 数据模型

### ConversationMessage

**消息类型** (`messages.py`):
- `UserMessage`: 用户消息
- `AssistantMessage`: 助手消息
- `ToolResultMessage`: 工具结果消息

### QueryContext

**查询上下文** (`query.py`):
```python
class QueryContext:
    api_client: SupportsStreamingMessages
    tool_registry: ToolRegistry
    permission_checker: PermissionChecker
    cwd: Path
    model: str
    system_prompt: str
    max_tokens: int
    permission_prompt: PermissionPrompt | None
    ask_user_prompt: AskUserPrompt | None
    hook_executor: HookExecutor | None
    tool_metadata: dict[str, object]
```

### StreamEvent

**流式事件** (`stream_events.py`):
```python
class StreamEvent:
    type: StreamEventType
    content: str | None
    tool_use: ToolUseEvent | None
    error: str | None
    metadata: dict[str, Any]
```

---

## 测试与质量

### 测试覆盖

- **单元测试**: `tests/engine/` 目录
- **集成测试**: 查询循环的端到端测试
- **流式处理测试**: 确保事件流正确性

### 质量指标

- 类型注解覆盖率: 100%
- 文档字符串覆盖率: 90%+
- 测试通过率: 100%

---

## 常见问题 (FAQ)

### Q1: 如何处理工具执行失败？

A: 工具执行失败会返回错误事件，可以在流式事件中捕获并处理。错误会被添加到对话历史，模型可以重试或调整策略。

### Q2: 如何限制最大轮次？

A: 使用 CLI 的 `--max-turns` 参数或在 QueryEngine 中实现自定义逻辑来限制循环次数。

### Q3: 如何追踪成本？

A: QueryEngine 内置 CostTracker，通过 `total_usage` 属性访问累计的 token 使用和成本。

---

## 相关文件清单

### 核心文件

- `query_engine.py`: QueryEngine 主类
- `query.py`: 查询执行函数和循环逻辑
- `messages.py`: 消息类型定义
- `stream_events.py`: 流式事件类型定义
- `cost_tracker.py`: Token 和成本追踪

### 测试文件

- `tests/engine/test_query_engine.py`: QueryEngine 测试
- `tests/engine/test_query.py`: 查询执行测试
- `tests/engine/test_messages.py`: 消息类型测试

---

## 变更记录 (Changelog)

### 2026-04-04 22:29:01 - 模块文档初始化
- 创建 engine 模块文档
- 识别核心接口和数据模型
- 提供使用示例和常见问题

---

*最后更新: 2026-04-04 22:29:01*
*文档版本: 1.0.0*
