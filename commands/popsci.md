---
description: Run the legal-popsci pipeline. Takes a raw legal topic, dispatches 6 specialist agents in sequence, produces platform-ready posts. Usage - /popsci <topic> [--platform xhs|wechat|script]
---

# /popsci

End-to-end command for the legal-popsci pipeline. Spawns the orchestrator, which dispatches the 6 specialist agents.

## Usage

```
/popsci <topic>
/popsci <topic> --platform xhs
/popsci <topic> --platform xhs --platform wechat
/popsci <topic> --platform xhs --run-id deposit-2026-05
```

If `--platform` is not specified, defaults to `xhs` (Xiaohongshu).

## What this command does

1. Parses the topic and platform flags
2. Creates output directory `examples/<run-id>/outputs/`
3. Spawns the [popsci-orchestrator](../agents/orchestrator.md) agent with the topic
4. The orchestrator runs the 6-stage pipeline:
   - Stage 1: [topic-researcher](../agents/topic-researcher.md)
   - Stage 2: [statute-verifier](../agents/statute-verifier.md)
   - Stage 3: [plain-language-rewriter](../agents/plain-language-rewriter.md)
   - Stage 4: [compliance-reviewer](../agents/compliance-reviewer.md)
   - Stage 5: [style-adapter](../agents/style-adapter.md)
   - Stage 6: [citation-validator](../agents/citation-validator.md)
5. Returns the final platform draft(s) and a summary

## Inputs

- `topic` (required): a legal question or topic in Chinese or English
- `--platform` (optional, repeatable): target platform — `xhs`, `wechat`, `script`
- `--run-id` (optional): override the default timestamp run-id

## Outputs

Files written to `examples/<run-id>/outputs/`:
- `stage1_research.md` through `stage6_citation.md` — every intermediate output
- `final_<platform>.md` — final platform-ready draft(s)
- `run.log` — single-line entries for each agent dispatch

## Examples

### Single platform (default)
```
/popsci 朋友圈骂人会构成名誉权侵权吗
```

### Multi-platform
```
/popsci 网购7天无理由退货被拒绝怎么办 --platform xhs --platform wechat
```

### Pinned run-id (for re-runs / regression)
```
/popsci 加班费怎么算 --run-id overtime-test
```

## Failure handling

The orchestrator handles intra-pipeline failures (citation drift, compliance gate triggers) by re-routing to the appropriate upstream stage with at most one full re-run. If failures persist, the orchestrator escalates with an `[ESCALATION]` message; the user must clarify before re-running.

## See also

- [docs/architecture.md](../docs/architecture.md) — full pipeline design
- [docs/workflow.md](../docs/workflow.md) — step-by-step walkthrough of a typical run
- [docs/design-principles.md](../docs/design-principles.md) — why this is structured as 6 agents instead of 1
