# opencode-subagent-pack —— 轻量化子 agent 包

一组开箱即用的 OpenCode 子代理（Subagent），超级轻量，只有两个。

## 📦 包含内容

| 文件 | 用途 |
|------|------|
| `agents/researcher.md` | **Researcher** — 信息搜集与调研子代理（纯手动调用） |
| `agents/reviewer.md` | **Reviewer** — 代码审查子代理（支持 200 行自动触发） |
| `instructions/delegation.md` | 委托策略指令（合并到 AGENTS.md） |

## 🚀 安装步骤

让 AI agent 看这个，它会帮你装好。

### 1. 放置子代理文件

将 `agents/` 下的两个 `.md` 文件复制到你的 opencode agent 目录：

```bash
# 全局安装
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

### 3. 自定义模型

子代理的 frontmatter 中指定了模型 ID，你需要根据自己的提供商修改：

**researcher.md** — 信息搜集用廉价模型：
```yaml
model: opencode/deepseek-v4-flash-free  # ← 替换为你自己的廉价模型
```

**reviewer.md** — 审查用高性能模型：
```yaml
model: mimo/mimo-v2.5-pro  # ← 替换为你自己的高性能模型
```

如果删除 `model` 行，子代理将继承主 agent 的模型。

### 4. 重启生效

```bash
opencode
```

## 🧠 使用方式

### Reviewer — 自动触发 + 手动调用

```bash
# 自动触发：代码写完/改完超过 200 行时自动审查
# 手动调用：
@reviewer 审查一下 src/auth.ts
```

### Researcher — 仅手动调用

> 信息搜集类任务由 opencode 内置的 Explore（代码库探索）和 Scout（外部资料调研）自动处理。
> Researcher 仅在**需要特定研究方法论**时手动使用。

```bash
@researcher 用第一性原理分析一下这个模块的设计
```

### Researcher 与内置子代理的差异

| | Explore / Scout（内置） | Researcher（手动） |
|--|------------------------|-------------------|
| 搜索 | 搜到信息即可 | 多重假设 + 反例搜索 |
| 结论 | 直接回答 | 必须验证 + 标确信度 |
| 分析 | 默认单解释 | 至少 2-3 种解释 |
| 输出 | 随意 | 结构化 Understanding/Key Insights/Hypotheses/Risks |

## 🛠 自定义提示词

直接修改 `agents/*.md` 文件中 `---` 之后的正文即可。

## 📄 文件结构

安装完成后：

```
~/.config/opencode/
├── AGENTS.md              # 含委托策略（已合并）
├── agents/
│   ├── researcher.md      # 信息搜集（手动调用）
│   └── reviewer.md        # 代码审查（自动+手动）
└── opencode.jsonc         # 无需修改
```
