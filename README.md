# Legal PopSci Agents

> A multi-agent pipeline that turns legal questions into compliant, plain-language popular-science content for Chinese social platforms (Xiaohongshu, WeChat Official Account, short-video scripts).

[中文版本](#中文说明) · [English](#english)

---

## English

### Why this exists

Lawyers in China face an asymmetric problem: clients ask the same questions thousands of times, but every public answer must navigate three independent constraints simultaneously:

1. **Legal accuracy** — citing real, currently-effective statutes, not LLM hallucinations
2. **Plain-language usability** — readers without legal training must be able to act on the answer
3. **Regulatory compliance** — China's Advertising Law, Lawyers Law, and platform-specific rules forbid absolute claims, success-rate guarantees, and unauthorised case references

A single LLM call can do any one of these well. It cannot do all three reliably in one shot. Hence: a 6-stage agent pipeline, each agent specialised, each output verified before flowing downstream.

### Architecture

```
            ┌──────────────────┐
   topic ──▶│ topic-researcher │── reader-perspective questions
            └──────────────────┘
                     │
                     ▼
            ┌──────────────────┐
            │ statute-verifier │── verified statutes + applicability
            └──────────────────┘
                     │
                     ▼
        ┌──────────────────────────┐
        │ plain-language-rewriter  │── plain-language explanation
        └──────────────────────────┘
                     │
                     ▼
        ┌──────────────────────────┐
        │   compliance-reviewer    │── compliance-cleared draft
        └──────────────────────────┘
                     │
                     ▼
            ┌──────────────────┐
            │  style-adapter   │── platform-specific draft
            └──────────────────┘
                     │
                     ▼
        ┌──────────────────────────┐
        │   citation-validator     │── final, citation-checked output
        └──────────────────────────┘
```

Each agent has a single responsibility, a clear input/output contract, and explicit failure modes. The orchestrator handles routing, retries, and degradation. See [docs/architecture.md](docs/architecture.md) for the full design.

### Why multi-agent (not one big prompt)

A monolithic prompt that says "please write a legally-accurate, plain, compliant, platform-styled, citation-verified article" suffers from three failure modes documented in our [design principles](docs/design-principles.md):

- **Constraint collision** — the LLM trades off compliance against readability silently, with no way to inspect which got sacrificed
- **Citation drift** — without an isolated verification step, fabricated statute numbers slip through because they look plausible
- **Style overshoot** — Xiaohongshu's emoji-heavy register tempts the model toward absolute claims that violate the Advertising Law

Splitting the work into 6 specialised agents lets each one carry its own discipline. The pipeline gains:
- **Inspectability** — every intermediate output is human-readable
- **Composability** — swap any single agent without rewriting the rest
- **Long-chain reasoning** — the citation-validator can reject the entire pipeline output and force a re-run from stage 2

### Quick start

This project is built for [Claude Code](https://docs.claude.com/en/docs/claude-code/overview). After cloning:

```bash
# In Claude Code, from the repo root:
/popsci 朋友圈骂人会构成名誉权侵权吗
```

The orchestrator will dispatch the 6 agents in sequence. Intermediate outputs land in `examples/<run-id>/outputs/`. Final platform-ready drafts (Xiaohongshu / WeChat / short-video script) appear at the top of that folder.

### Examples

Two end-to-end runs are checked in. They use **fictional individuals and scenarios**; statute references are real and currently effective as of 2026-05.

- [examples/01_rental_deposit](examples/01_rental_deposit/) — "Landlord refuses to refund my deposit"
- [examples/02_wechat_defamation](examples/02_wechat_defamation/) — "Can a WeChat post be defamation?"

Each example folder contains:
- `input.md` — the original question
- `outputs/stage1_research.md` ... `outputs/stage6_citation.md` — every intermediate output
- `outputs/final_xhs.md` — final Xiaohongshu-formatted post

### Repository layout

```
legal-popsci-agents/
├── agents/           # 6 specialist agent configs + 1 orchestrator
├── skills/           # Reusable skills (citation formatting, plain-language rules)
├── commands/         # Claude Code slash commands (/popsci entry point)
├── docs/             # Architecture, workflow, design principles
├── workflows/        # Mermaid pipeline diagrams
└── examples/         # End-to-end runs with intermediate outputs
```

### License

MIT. See [LICENSE](LICENSE).

### Status

Active. Maintained by a Chinese law firm founder using Claude Code daily for legal-popsci content production. Pull requests welcome — particularly for new compliance-rule packs (different platforms have different forbidden-word lists) and for skill files that codify other domain conventions.

---

## 中文说明

### 为什么做这个

中国执业律师做公开普法时，每条内容必须同时满足三个独立约束：

1. **法律准确** —— 引用真实、现行有效的法条，不能依赖 LLM 凭印象生成
2. **通俗可用** —— 没有法律背景的读者要能据此行动
3. **合规** —— 《广告法》《律师法》《律师执业行为规范》以及平台规则禁止绝对化用语、胜诉率承诺、未授权案例引用

单次 LLM 调用任何一项都能做好。但要在一次输出里同时稳定满足三项，几乎不可能。因此：6 段 Agent 流水线，每个 Agent 各司其职，每段输出经下游验证后才往下流。

### 架构

见上文英文部分的流程图，或 [workflows/pipeline.mmd](workflows/pipeline.mmd) 中的 mermaid 源码。

每个 Agent 单一职责、明确的输入/输出契约、显式的失败模式。Orchestrator 负责路由、重试、降级。完整设计见 [docs/architecture.md](docs/architecture.md)。

### 为什么用多 Agent 而不是一个大 prompt

一个写"请生成法律准确、通俗、合规、平台风格化、引用核实的文章"的大 prompt，会出现三类失败：

- **约束相互挤压** —— LLM 默默在合规与可读性之间做取舍，不告诉你哪个被牺牲了
- **法条漂移** —— 没有独立的验证环节，编造的法条编号因为"看起来对"就溜过去了
- **风格越界** —— 小红书风格的 emoji + 短句诱导模型写出违反《广告法》的绝对化表述

把工作拆成 6 个专职 Agent，每个 Agent 各自背一个纪律。带来三个收益：
- **可检视** —— 每段中间产物都是人类可读的
- **可组合** —— 任意单个 Agent 都能换掉，不用重写其他
- **长链推理** —— citation-validator 可以否决整条流水线，强制从第 2 段重跑

### 使用方式

本项目基于 [Claude Code](https://docs.claude.com/en/docs/claude-code/overview)。clone 之后：

```bash
# 在 Claude Code 中，从仓库根目录执行：
/popsci 朋友圈骂人会构成名誉权侵权吗
```

Orchestrator 会按顺序调度 6 个 Agent。中间产物落到 `examples/<run-id>/outputs/`，最终平台稿（小红书 / 公众号 / 短视频脚本）出现在该文件夹顶部。

### 示例

仓库内附两条端到端示例。**人物与场景均为虚构**；法条引用真实且截至 2026-05 现行有效。

- [examples/01_rental_deposit](examples/01_rental_deposit/) ——「房东不退押金」
- [examples/02_wechat_defamation](examples/02_wechat_defamation/) ——「朋友圈骂人会构成名誉权侵权吗」

### 协议

MIT。详见 [LICENSE](LICENSE)。

### 维护状态

活跃维护中。由一位中国律所创始人作为日常普法内容生产工具持续使用与迭代。欢迎 PR——特别是新的合规规则包（不同平台禁用词清单不同）和编码其他领域规范的 skill 文件。
