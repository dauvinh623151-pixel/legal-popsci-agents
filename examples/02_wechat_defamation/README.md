# Example 02: 朋友圈骂人是否构成名誉权侵权

End-to-end pipeline run. Topic: `朋友圈骂人会构成名誉权侵权吗`. Target platform: Xiaohongshu.

**All persons named in this example are fictional.** Statute references are real and were verified against `flk.npc.gov.cn` on the validation date noted in `outputs/stage6_citation.md`.

## Files in this example

| File | What it shows |
|---|---|
| [input.md](input.md) | The original topic the user typed |
| [outputs/stage1_research.md](outputs/stage1_research.md) | 4 reader-perspective questions + differentiation notes |
| [outputs/stage2_statute.md](outputs/stage2_statute.md) | Verified statute table for each question |
| [outputs/stage3_plain.md](outputs/stage3_plain.md) | Plain-language draft |
| [outputs/stage4_compliance.md](outputs/stage4_compliance.md) | 2 compliance findings + corrected draft |
| [outputs/stage5_xhs.md](outputs/stage5_xhs.md) | Xiaohongshu-formatted draft |
| [outputs/stage6_citation.md](outputs/stage6_citation.md) | Final independent citation re-verification |
| [outputs/final_xhs.md](outputs/final_xhs.md) | Publishable final draft |

## What this example demonstrates

- **Stage 6 gate triggering**: Stage 5's first xhs draft cited `《民法典》第1024条第1款` but Stage 6's independent re-pull showed 第1024条 doesn't have numbered subsections (款) — it has two paragraphs but the convention in 民法典 numbering for this article is to refer to either the article level or specific 项 within sub-articles. The orchestrator re-ran Stage 2 for this single citation, Stage 2 returned the corrected form `《民法典》第1024条`, and Stages 3–5 re-ran for the affected sections.
- **Topic-area boundary handling**: Stage 1 noted that 名誉权侵权 has both 民事 and 治安管理处罚 dimensions. Pipeline scoped to 民事 only; 治安管理处罚 边界 noted in stage1_research.md as an out-of-scope flag passed through to the disclaimer.
- **Multiple statute interaction**: 民法典 第1024条 (实体), 第995条 (请求权), 第1183条 (精神损害赔偿) — three articles cited together, each verified independently.
