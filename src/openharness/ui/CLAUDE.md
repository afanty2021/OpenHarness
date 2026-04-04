# UI 模块 - React TUI 前端

[根目录](../../../CLAUDE.md) > [src](../../) > [openharness](../) > **ui**

---

## 模块职责

UI 模块提供 React/Ink 终端用户界面，负责：

- **交互式 REPL**: 交互式命令行界面
- **命令选择器**: 可视化命令选择
- **权限对话框**: 交互式权限确认
- **会话管理**: 会话恢复和历史

---

## 入口与启动

### REPL 启动

**主要入口** (`app.py`):
```python
from openharness.ui.app import run_repl

await run_repl(
    prompt=None,
    cwd="/workspace",
    model="claude-sonnet-4-20250514",
)
```

### 前端启动

**前端入口** (`frontend/terminal/src/index.tsx`):
```bash
cd frontend/terminal
npm run start
```

---

## 对外接口

### React 组件

**App**: 主应用组件
**ConversationView**: 对话视图
**CommandPicker**: 命令选择器
**PromptInput**: 提示输入
**StatusBar**: 状态栏
**SidePanel**: 侧边栏

---

## 关键依赖与配置

### 技术栈

- **React 18.3.1**: UI 框架
- **Ink 5.1.0**: React TUI 渲染器
- **TypeScript 5.7.3**: 类型安全

### 配置参数

**后端协议**: 结构化 JSON 通信
**前端渲染**: Ink 组件树

---

## 测试与质量

### 测试覆盖

- **E2E 测试**: `scripts/react_tui_e2e.py`
- **交互测试**: `scripts/test_tui_interactions.py`

---

## 变更记录 (Changelog)

### 2026-04-04 22:29:01 - 模块文档初始化

---

*最后更新: 2026-04-04 22:29:01*
