# State 模块 - 应用状态管理

[根目录](../../../CLAUDE.md) > [src](../../) > [openharness](../) > **state**

---

## 模块职责

State 模块提供应用状态管理，负责：

- **状态模型**: 定义应用状态数据结构
- **状态存储**: 可观察的状态存储实现
- **状态订阅**: 支持状态变更监听
- **UI 状态同步**: 为 TUI 提供共享状态

---

## 入口与启动

### 状态存储

**主要入口** (`store.py`):
```python
from openharness.state import AppState, AppStateStore

# 创建初始状态
initial_state = AppState(
    model="claude-sonnet-4-20250514",
    permission_mode="default",
    theme="default",
)

# 创建状态存储
store = AppStateStore(initial_state)

# 订阅状态变更
def on_state_change(state):
    print(f"Model changed to: {state.model}")

unsubscribe = store.subscribe(on_state_change)

# 更新状态
store.set(model="claude-opus-4-20250514")

# 取消订阅
unsubscribe()
```

---

## 对外接口

### AppState

**状态数据类** (`app_state.py`):
```python
@dataclass
class AppState:
    model: str              # 当前模型
    permission_mode: str    # 权限模式
    theme: str              # UI 主题
    cwd: str = "."          # 工作目录
    provider: str = "unknown"  # API 提供商
    auth_status: str = "missing"  # 认证状态
    base_url: str = ""      # API 基础 URL
    vim_enabled: bool = False    # Vim 模式
    voice_enabled: bool = False  # 语音模式
    voice_available: bool = False  # 语音可用
    voice_reason: str = ""  # 语音状态原因
    fast_mode: bool = False      # 快速模式
    effort: str = "medium"       # 努力程度
    passes: int = 1              # 迭代次数
    mcp_connected: int = 0       # 已连接 MCP 数
    mcp_failed: int = 0          # 失败 MCP 数
    bridge_sessions: int = 0     # 桥接会话数
    output_style: str = "default"  # 输出样式
    keybindings: dict = field(default_factory=dict)  # 键盘绑定
```

### AppStateStore

**状态存储** (`store.py`):
| 方法 | 描述 |
|------|------|
| `get()` | 获取当前状态快照 |
| `set(**updates)` | 更新状态并通知监听器 |
| `subscribe(listener)` | 注册监听器，返回取消订阅函数 |

---

## 关键依赖与配置

### 依赖库

- `dataclasses`: Python 标准库
- `collections.abc.Callable`: 类型注解

### 配置项

状态字段均为运行时配置，通过 CLI 或 TUI 动态修改。

---

## 数据模型

### 状态流转

```
初始状态 → 用户配置 → 运行时更新 → 状态变更通知 → UI 刷新
```

### 状态持久化

当前状态为易失性，会话结束后不持久化。持久化会话数据由 `services/session_storage.py` 处理。

---

## 测试与质量

### 测试覆盖

- **单元测试**: `tests/state/` 目录（待补充）
- **集成测试**: TUI 状态同步测试

### 质量工具

- **类型检查**: mypy 严格模式
- **代码检查**: ruff

---

## 相关文件清单

| 文件 | 描述 |
|------|------|
| `__init__.py` | 模块导出 |
| `app_state.py` | 状态数据类定义 |
| `store.py` | 可观察状态存储实现 |

---

## 常见问题 (FAQ)

**Q: 状态变更如何通知 UI？**
A: 使用订阅者模式，`AppStateStore.set()` 会通知所有注册的监听器。

**Q: 状态如何持久化？**
A: 当前状态为运行时易失状态。会话持久化由 `services/session_storage.py` 处理。

**Q: 如何添加新状态字段？**
A: 在 `AppState` 数据类中添加字段，默认值在构造函数中指定。

---

## 变更记录 (Changelog)

### 2026-04-15 - 模块文档初始化
- ✅ 创建 state 模块完整文档
- 📝 记录 AppState 和 AppStateStore API

---

*最后更新: 2026-04-15 14:23:59*
