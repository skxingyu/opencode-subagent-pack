# opencode-subagent-pack

一组开箱即用的 OpenCode 子代理（Subagent），包含自动委托策略。

## 📦 包含内容

| 文件 | 用途 |
|------|------|
| `agents/researcher.md` | **Researcher** — 信息搜集与调研子代理 |
| `agents/reviewer.md` | **Reviewer** — 代码审查子代理 |
| `instructions/delegation.md` | 委托策略指令（合并到 AGENTS.md） |

## 🚀 安装步骤

### 1. 放置子代理文件

将 `agents/` 下的两个 `.md` 文件复制到你的 opencode agent 目录：

```bash
# 全局安装（推荐）
cp agents/*.md ~/.config/opencode/agents/

# 或项目级安装
cp agents/*.md .opencode/agents/
```

### 2. 合并委托策略

将 `instructions/delegation.md` 的内容追加到你的 `AGENTS.md` 中：

```bash
# 全局
cat instructions/delegation.md >> ~/.config/opencode/AGENTS.md

# 或项目级
cat instructions/delegation.md >> .opencode/AGENTS.md
```

> 如果不存在 AGENTS.md，直接复制文件过去即可。

### 3. 自定义模型（重要）

子代理的 frontmatter 中指定了模型 ID，你需要根据自己的提供商修改：

**researcher.md** — 信息搜集用廉价模型：
```yaml
model: opencode/deepseek-v4-flash-free  # ← 替换为你自己的廉价模型
```

**reviewer.md** — 审查用高性能模型：
```yaml
model: mimo/mimo-v2.5-pro  # ← 替换为你自己的高性能模型
```

常用模型 ID 示例：
- `anthropic/claude-sonnet-4-20250514`
- `openai/gpt-5.5`
- `opencode/deepseek-v4-flash-free`
- `google/gemini-2.5-flash`

如果删除 `model` 行，子代理将继承主 agent 的模型。

### 4. 重启生效

```bash
# 重启 opencode
opencode
```

## 🧠 使用方式

### 手动调用

在对话中直接用 `@` 提及：

```
@researcher 对比一下 React 和 Vue 的架构差异
@reviewer 审查一下 src/auth.ts
```

### 自动委托

主 agent 会根据 AGENTS.md 中的委托策略，**自动判断**是否派发任务：

| 场景 | 行为 |
|------|------|
| 需要 10 条以上搜索的调研 | → 自动委托 researcher |
| 明显不确定、一波搜索不够 | → 自动委托 researcher |
| 简单问答（<10 条搜索） | → 主 agent 自己处理 |
| 写代码超过 200 行后 | → 自动触发 reviewer 审查 |
| 用户说"用researcher查一下" | → 委托 researcher |

## 🛠 自定义提示词

你可以任意修改 `agents/*.md` 文件中 `---` 之后的正文部分，每个子代理的系统提示词都可以按需调整。

## 📄 文件结构参考

安装完成后，你的 opencode 配置目录应包含：

```
~/.config/opencode/
├── AGENTS.md              # 含委托策略（已合并）
├── agents/
│   ├── researcher.md      # 信息搜集子代理
│   └── reviewer.md        # 代码审查子代理
└── opencode.jsonc         # 你的主配置文件
```

无需修改 `opencode.jsonc`，子代理通过 Markdown 文件自动注册。
