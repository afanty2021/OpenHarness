# Voice 模块 - 语音模式支持

[根目录](../../../CLAUDE.md) > [src](../../) > [openharness](../) > **voice**

---

## 模块职责

Voice 模块提供语音模式支持，负责：

- **语音识别**: 流式语音转文本 (STT)
- **关键词提取**: 从语音输入中提取关键词
- **模式切换**: 在语音模式和文本模式之间切换
- **能力检测**: 检测系统语音能力

---

## 入口与启动

### 切换语音模式

**主要入口** (`voice_mode.py`):
```python
from openharness.voice import toggle_voice_mode, inspect_voice_capabilities

# 检测语音能力
from openharness.api import ProviderInfo
provider = ProviderInfo(name="anthropic", voice_supported=True)
diagnostics = inspect_voice_capabilities(provider)

if diagnostics.available:
    print(f"语音可用：{diagnostics.reason}, 录音设备：{diagnostics.recorder}")
    
# 切换语音模式
new_state = toggle_voice_mode(current_state)
```

### 流式语音识别

```python
from openharness.voice import transcribe_stream

async for text in transcribe_stream(audio_source):
    print(f"识别文本：{text}")
```

### 关键词提取

```python
from openharness.voice import extract_keyterms

keyterms = extract_keyterms("Use the read tool to open config.py")
# 提取关键动作和参数
```

---

## 对外接口

### VoiceDiagnostics 数据类

```python
@dataclass(frozen=True)
class VoiceDiagnostics:
    available: bool       # 语音是否可用
    reason: str           # 状态原因
    recorder: str | None  # 录音设备 (sox/ffmpeg/arecord)
```

### API 函数

| 函数 | 描述 |
|------|------|
| `toggle_voice_mode(enabled: bool) -> bool` | 切换语音模式状态 |
| `inspect_voice_capabilities(provider) -> VoiceDiagnostics` | 检测语音能力 |
| `transcribe_stream(audio_source)` | 流式语音识别 |
| `extract_keyterms(text)` | 提取关键词 |

### 与 AppState 集成

语音模式状态存储在 `AppState` 的多个字段中：

```python
from openharness.state import AppState

state = AppState(
    voice_enabled=False,      # 语音模式是否启用
    voice_available=False,    # 语音功能是否可用
    voice_reason="",          # 不可用时的原因
)
```

---

## 关键依赖与配置

### 依赖库

- `shutil`: Python 标准库（用于检测录音设备）
- `dataclasses`: Python 标准库

### 系统依赖

语音模式需要以下录音工具之一：
- `sox` (推荐)
- `ffmpeg`
- `arecord` (Linux ALSA)

### 配置项

| 配置 | 描述 |
|------|------|
| `voice_enabled` | AppState 中的布尔标志 |
| `ctrl+v` | 默认切换快捷键（见 keybindings 模块） |
| `provider.voice_supported` | API 提供商语音支持标志 |

---

## 数据模型

### 语音模式状态机

```
文本模式 --(toggle_voice_mode)--> 语音模式 --(toggle_voice_mode)--> 文本模式
```

### 能力检测流程

```
ProviderInfo → 检查 voice_supported → 检测录音设备 → 返回 VoiceDiagnostics
```

### 与 TUI 集成

```
键盘输入 (ctrl+v) → Keybindings 解析 → toggle_voice_mode() → AppState 更新 → TUI 重新渲染
```

---

## 测试与质量

### 测试覆盖

- **单元测试**: `tests/voice/` 目录（待补充）
- **集成测试**: TUI 语音模式切换测试
- **E2E 测试**: 真实语音识别测试

### 质量工具

- **类型检查**: mypy 严格模式
- **代码检查**: ruff

---

## 相关文件清单

| 文件 | 描述 |
|------|------|
| `__init__.py` | 模块导出 |
| `voice_mode.py` | 语音模式切换和能力检测 |
| `stream_stt.py` | 流式语音转文本 |
| `keyterms.py` | 关键词提取 |

---

## 常见问题 (FAQ)

**Q: 语音模式如何工作？**
A: 语音模式使用系统录音工具（sox/ffmpeg/arecord）捕获音频，通过 STT 服务转换为文本。

**Q: 如何启用语音模式？**
A: 按 `ctrl+v` 快捷键或在 AppState 中设置 `voice_enabled=True`。

**Q: 为什么语音不可用？**
A: 可能原因：1) API 提供商不支持语音；2) 系统未安装录音工具。运行 `inspect_voice_capabilities()` 查看具体原因。

**Q: 支持哪些语音识别服务？**
A: 取决于 API 提供商。Anthropic、OpenAI 等主流提供商均支持语音输入。

---

## 变更记录 (Changelog)

### 2026-04-15 - 模块文档初始化
- ✅ 创建 voice 模块完整文档
- 📝 记录 VoiceDiagnostics 和 API

---

*最后更新: 2026-04-15 14:23:59*
