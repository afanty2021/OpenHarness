# Config 模块 - 多层配置系统

[根目录](../../../CLAUDE.md) > [src](../../) > [openharness](../) > **config**

---

## 模块职责

Config 模块提供多层配置系统，负责：

- **设置加载**: 加载用户设置
- **设置保存**: 保存用户设置
- **路径管理**: 管理配置和数据路径
- **默认值**: 提供合理的默认配置

---

## 入口与启动

### load_settings

**主要入口** (`settings.py`):
```python
from openharness.config import load_settings, save_settings

settings = load_settings()
settings.model = "claude-sonnet-4-20250514"
save_settings(settings)
```

---

## 对外接口

### 配置文件

**settings.json**:
```json
{
  "model": "claude-sonnet-4-20250514",
  "api_key": "sk-xxx",
  "permission": {
    "mode": "default"
  }
}
```

---

## 关键依赖与配置

### 配置路径

- `~/.openharness/settings.json`: 用户配置
- `.openharness/settings.json`: 项目配置

---

## 测试与质量

### 测试覆盖

- **单元测试**: `tests/config/` 目录

---

## 变更记录 (Changelog)

### 2026-04-04 22:29:01 - 模块文档初始化

---

*最后更新: 2026-04-04 22:29:01*
