# Workflow Walkthrough

This document walks through a single end-to-end pipeline run, showing what each stage receives and what it produces. The example used here is the same as `examples/01_rental_deposit/`.

## Input

```
/popsci 房东不退我押金怎么办
```

(No `--platform` flag → defaults to `xhs`.)

## Stage 1: topic-researcher

**Receives**: the raw topic string.

**Does**:
1. Identifies the legal area: tenancy + civil contracts → 民法典 合同编 + 民法典 物权编 + 部分地方性租赁条例
2. Performs a brief web search to see what existing platform content looks like for the topic, to identify gaps
3. Generates 4 reader-perspective questions

**Produces** (`outputs/stage1_research.md`, abridged):

```markdown
## Reader-perspective questions
1. 房东扣押金的合理理由有哪些
2. 押金条款写在合同里就一定有效吗
3. 房东不退押金，我能怎么取证
4. 走法律程序追讨押金大概多久、多少钱
```

## Stage 2: statute-verifier

**Receives**: `stage1_research.md`.

**Does**:
1. For each of the 4 questions, identifies the operative statute
2. Pulls verbatim text from `flk.npc.gov.cn`
3. Spells out applicability conditions and common exceptions
4. Tags each citation with effective-date marker

**Produces** (`outputs/stage2_statute.md`, abridged):

```markdown
## Question 1: 房东扣押金的合理理由有哪些

### Primary statute
- **Name**: 《中华人民共和国民法典》
- **Article**: 第七百零三条 (定义), 第七百一十一条 (合同履行), 第七百三十一条 (终止后义务)
- **Effective from**: 2021-01-01
- ...
```

## Stage 3: plain-language-rewriter

**Receives**: `stage2_statute.md`.

**Does**:
1. For each question, writes a 1-sentence short answer + scenario + plain explanation + applicability bullets + actionable steps
2. Translates jargon on first use using `skills/plain-language-rules` substitution table

**Produces** (`outputs/stage3_plain.md`, abridged):

```markdown
### Q: 房东扣押金的合理理由有哪些

**Short answer**: 房东只能因为「损坏 / 欠款 / 提前退租违约」三类合理事由扣押金，不能随意扣留。

**The scenario**: 小李在合同期满后退房。房子整洁、家具完好。但房东说「窗帘有点旧，墙面有手印」要扣 2000 押金。这种情况下……

**What the law says, in plain words**: ……
```

## Stage 4: compliance-reviewer

**Receives**: `stage3_plain.md`.

**Does**:
1. Lexical scan against absolute-claim word list (`skills/plain-language-rules` Section A)
2. Semantic scan for outcome guarantees (Section B)
3. Structural scan on headings and CTAs

**Produces** (`outputs/stage4_compliance.md`, abridged):

```markdown
## Findings

### Finding 1
- **Severity**: medium
- **Rule**: 《广告法》第9条 — absolute claim
- **Original text**: "你一定可以拿回押金"
- **Suggested replacement**: "在合同明确、证据充分的情况下，押金通常可以追回"
- **Confidence**: 0.92

## Corrected draft
<full draft with replacements applied>
```

The Stage 4 gate evaluates: any high-confidence finding triggers a re-run of Stage 3. In this example, the finding is medium-confidence; the corrected draft is accepted and passed forward.

## Stage 5: style-adapter

**Receives**: `stage4_compliance.md` + `--platform xhs`.

**Does**:
1. Compresses to 600–800 字 budget
2. Designs a 20-字 title with pain-hook structure
3. Adds platform-safe emoji at paragraph starts
4. Selects 7 hashtags from the safe list

**Produces** (`outputs/stage5_xhs.md`, abridged):

```markdown
**Title**: 房东扣押金不退？教你3步合法追回

**Body**:
⚠️ 退房时被扣押金，是租房圈最常见的纠纷之一……

✅ 第一步：核对合同
……

📋 第二步：留好证据
……

💡 第三步：发书面催告
……

**Tags**: #法律科普 #民法典 #租房 #押金 #打工人 #普法 #维权指南
```

## Stage 6: citation-validator

**Receives**: `stage5_xhs.md`.

**Does**:
1. Extracts all `《...》第...条` citations from the styled draft
2. Independently re-pulls each cited article from `flk.npc.gov.cn`
3. Compares the styled draft's claim against the source verbatim

**Produces** (`outputs/stage6_citation.md`, abridged):

```markdown
## Per-citation results

### Citation 1
- **Found in**: stage5_xhs.md (line 22)
- **Cited as**: 《民法典》第七百三十一条
- **Source URL**: https://flk.npc.gov.cn/...
- **Match**: pass
- **Notes**: paraphrase preserves meaning

## Summary
- Total: 3
- Pass: 3
- Fail: 0
- Recommended action: approve
```

The orchestrator copies `stage5_xhs.md` to `outputs/final_xhs.md`.

## Final output

`outputs/final_xhs.md` contains the publishable Xiaohongshu draft.

## Total operator effort

In a successful run with no gate triggers:
- Operator types one slash command
- Pipeline executes ~6 agent dispatches over a few minutes
- Operator reads `outputs/final_xhs.md` and publishes (after a final human read-through)

When gates trigger (which they do on perhaps 20–30% of runs in early operation):
- Stage 4 gate adds one more Stage 3 dispatch
- Stage 6 gate adds re-runs of Stages 2–5 for the affected citations
- Maximum total: ~12 agent dispatches before escalation
