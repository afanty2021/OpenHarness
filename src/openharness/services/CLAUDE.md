# Services 模块 - 辅助服务

[根目录](../../../CLAUDE.md) > [src](../../) > [openharness](../) > **services**

---

## 模块职责

Services 模块提供辅助服务，负责：

- **压缩服务**: 上下文压缩
- **Cron 服务**: 定时任务调度
- **OAuth 服务**: OAuth 认证
- **会话存储**: 会话持久化

---

## 入口与启动

### Cron 调度器

**主要入口** (`cron_scheduler.py`):
```bash
oh cron start
oh cron list
oh cron toggle <name> true|false
```

---

## 对外接口

### Cron 工具

**CronCreate**: 创建定时任务
**CronList**: 列出定时任务
**CronDelete**: 删除定时任务
**RemoteTrigger**: 远程触发任务

---

## 关键依赖与配置

### 依赖库

- `croniter>=2.0.0`: Cron 表达式解析
- `watchfiles>=0.20.0`: 文件监控

---

## 测试与质量

### 测试覆盖

- **单元测试**: `tests/services/` 目录

---

## 变更记录 (Changelog)

### 2026-04-04 22:29:01 - 模块文档初始化

---

*最后更新: 2026-04-04 22:29:01*
