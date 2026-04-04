# API 模块 - API 客户端

[根目录](../../../CLAUDE.md) > [src](../../) > [openharness](../) > **api**

---

## 模块职责

API 模块提供 API 客户端实现，负责：

- **Anthropic 客户端**: Anthropic API 客户端
- **OpenAI 客户端**: OpenAI 兼容 API 客户端
- **提供商检测**: 自动检测 API 提供商
- **流式支持**: 支持流式响应

---

## 入口与启动

### AnthropicClient

**Anthropic 格式** (`client.py`):
```python
from openharness.api import AnthropicClient

client = AnthropicClient(
    api_key="sk-xxx",
    base_url="https://api.anthropic.com",
)
async for event in client.stream_messages(messages, tools):
    # 处理流式事件
    pass
```

### OpenAIClient

**OpenAI 格式** (`openai_client.py`):
```python
from openharness.api import OpenAIClient

client = OpenAIClient(
    api_key="sk-xxx",
    base_url="https://api.openai.com/v1",
)
async for event in client.stream_messages(messages, tools):
    # 处理流式事件
    pass
```

---

## 对外接口

### 提供商支持

**Anthropic 格式**:
- Anthropic Claude
- Moonshot / Kimi
- Vertex AI
- Bedrock

**OpenAI 格式**:
- Alibaba DashScope
- DeepSeek
- OpenAI
- GitHub Models
- Ollama

---

## 关键依赖与配置

### 依赖库

- `anthropic>=0.40.0`: Anthropic SDK
- `openai>=1.0.0`: OpenAI SDK
- `httpx>=0.27.0`: HTTP 客户端

---

## 测试与质量

### 测试覆盖

- **单元测试**: `tests/api/` 目录
- **E2E 测试**: 真实 API 调用测试

---

## 变更记录 (Changelog)

### 2026-04-04 22:29:01 - 模块文档初始化

---

*最后更新: 2026-04-04 22:29:01*
