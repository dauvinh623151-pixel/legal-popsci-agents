# Example 01: 租房押金房东不退

End-to-end pipeline run. Topic: `房东不退我押金怎么办`. Target platform: Xiaohongshu.

**All persons named in this example are fictional.** Statute references are real and were verified against `flk.npc.gov.cn` on the validation date noted in `outputs/stage6_citation.md`.

## Files in this example

| File | What it shows |
|---|---|
| [input.md](input.md) | The original topic the user typed |
| [outputs/stage1_research.md](outputs/stage1_research.md) | 4 reader-perspective questions + differentiation notes |
| [outputs/stage2_statute.md](outputs/stage2_statute.md) | Verified statute table for each question |
| [outputs/stage3_plain.md](outputs/stage3_plain.md) | Plain-language draft, no compliance hedging yet |
| [outputs/stage4_compliance.md](outputs/stage4_compliance.md) | 3 compliance findings + corrected draft |
| [outputs/stage5_xhs.md](outputs/stage5_xhs.md) | Xiaohongshu-formatted draft (title, body, hashtags) |
| [outputs/stage6_citation.md](outputs/stage6_citation.md) | Final independent citation re-verification |
| [outputs/final_xhs.md](outputs/final_xhs.md) | Publishable final draft |

## What this example demonstrates

- **Stage 4 gate triggering**: Stage 3's first draft contained "你一定可以拿回押金" (absolute claim). Stage 4 flagged this with confidence 0.92 and the orchestrator re-prompted Stage 3 with the violation as a constraint. The corrected version uses conditional framing.
- **Citation precision survival**: All three statute citations passed Stage 6's independent re-pull from `flk.npc.gov.cn` without drift through the styling stage.
- **Plain-language compression**: Stage 3's draft is ~1400 字; Stage 5's xhs version is ~720 字 — substantial compression while preserving every applicability condition from Stage 2.
