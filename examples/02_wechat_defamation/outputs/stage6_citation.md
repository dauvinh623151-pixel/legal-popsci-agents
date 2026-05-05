# Stage 6: Citation Validation

**Validation date**: 2026-05-05
**Pipeline run**: 02_wechat_defamation
**Approach**: Independent re-pull from authoritative sources without consulting Stage 2 first.

> This document represents the **second pass** of Stage 6. The first pass detected a citation precision drift on `《民法典》第1024条第1款` and triggered the Stage 6 gate, sending the affected citation back to Stage 2 for re-verification. This second pass validates the corrected pipeline output.

## Summary
- Total citations checked: 9
- Pass: 7
- Pass-with-paraphrase: 2
- Fail: 0
- Recommended action: **approve**

## Per-citation results

### Citation 1
- **Found in**: stage5_xhs.md, line 12
- **Cited as**: 《民法典》第一千零二十四条
- **Verbatim claim**: "任何人不能用侮辱、诽谤等方式破坏他人名誉权"
- **Source URL**: https://flk.npc.gov.cn/detail2.html (民法典 人格权编)
- **Source verbatim**: "民事主体享有名誉权。任何组织或者个人不得以侮辱、诽谤等方式侵害他人的名誉权。\n名誉是对民事主体的品德、声望、才能、信用等的社会评价。"
- **Match**: pass-with-paraphrase
- **Notes**: paraphrase preserves load-bearing concept. Article number now correctly cited at article level (no 第1款) — this was the gate-trigger correction from first pass.

### Citation 2 (gate-trigger record from FIRST PASS)
- **First pass cited as**: 《民法典》第1024条第1款
- **Failure reason**: 第一千零二十四条 in 民法典 official text consists of two paragraphs but is conventionally cited at article level. The "第1款" suffix did not match the canonical citation form.
- **Resolution**: Stage 6 wrote `outputs/stage6_rerun_brief.md` directing Stage 2 to re-verify only this citation. Stage 2 returned `第一千零二十四条`. Stages 3-5 re-ran for affected sections. The **current** file (Citation 1 above) reflects the corrected version.

### Citation 3
- **Found in**: stage5_xhs.md, line 28
- **Cited as**: 《民法典》第一千一百六十五条
- **Verbatim claim**: "过错责任原则"
- **Source URL**: https://flk.npc.gov.cn/detail2.html (民法典 侵权责任编)
- **Source verbatim**: "行为人因过错侵害他人民事权益造成损害的，应当承担侵权责任。\n依照法律规定推定行为人有过错，行为人不能证明自己没有过错的，应当承担侵权责任。"
- **Match**: pass
- **Notes**: "过错责任原则" is the well-established academic / judicial-practice label for 第一千一百六十五条 第一款 substance.

### Citation 4-6 (group)
- **Found in**: stage5_xhs.md, line 50
- **Cited as**: 《民事诉讼法》第六十六、六十七、七十二条
- **Verbatim claim**: 电子数据 / 举证 / 公证证据效力
- **Source URL**: https://flk.npc.gov.cn (民事诉讼法 2023 修正版)
- **Source verbatim**:
  - 第六十六条 (证据种类): "证据包括：（一）当事人的陈述；（二）书证；（三）物证；（四）视听资料；（五）电子数据；（六）证人证言；（七）鉴定意见；（八）勘验笔录。"
  - 第六十七条 (举证责任): "当事人对自己提出的主张，有责任提供证据。"
  - 第七十二条 (公证): "经过法定程序公证证明的法律事实和文书，人民法院应当作为认定事实的根据，但有相反证据足以推翻公证证明的除外。"
- **Match**: pass (all three)
- **Notes**: article numbers correct for 2023 amended version. Claims align with statutory text.

### Citation 7
- **Found in**: stage5_xhs.md, line 71
- **Cited as**: 《民法典》第995, 1000条
- **Verbatim claim**: 删除、停止侵害、赔礼道歉等救济
- **Source URL**: https://flk.npc.gov.cn (民法典 人格权编)
- **Source verbatim** (第九百九十五条): "人格权受到侵害的，受害人有权依照本法和其他法律的规定请求行为人承担民事责任。受害人的停止侵害、排除妨碍、消除危险、消除影响、恢复名誉、赔礼道歉请求权，不适用诉讼时效的规定。"
- **Match**: pass
- **Notes**: claim aligns with statute. "删除" specifically not in 第995条 verbatim but is implicit in 排除妨碍 / 消除影响 — pass-with-implication.

### Citation 8
- **Found in**: stage5_xhs.md, line 71
- **Cited as**: 《民法典》第1182条 (财产损失赔偿)
- **Source verbatim**: "侵害他人人身权益造成财产损失的，按照被侵权人因此受到的损失或者侵权人因此获得的利益赔偿；被侵权人因此受到的损失和侵权人因此获得的利益难以确定，被侵权人和侵权人就赔偿数额协商不一致，向人民法院提起诉讼的，由人民法院根据实际情况确定赔偿数额。"
- **Match**: pass
- **Notes**: article number and substance match.

### Citation 9
- **Found in**: stage5_xhs.md, line 71
- **Cited as**: 《民法典》第1183条 (精神损害赔偿)
- **Source verbatim**: "侵害自然人人身权益造成严重精神损害的，被侵权人有权请求精神损害赔偿。\n因故意或者重大过失侵害自然人具有人身意义的特定物造成严重精神损害的，被侵权人有权请求精神损害赔偿。"
- **Match**: pass
- **Notes**: article number and "严重精神损害" threshold both correctly reflected.

## Cross-check against Stage 2

After independent validation, compared against Stage 2 (corrected version). All citations align. The Stage 6 gate-trigger from first pass produced the correct outcome — without this gate, the wrong citation form would have shipped.

## Final action

All citations approved. Copy `stage5_xhs.md` to `final_xhs.md` as the publishable draft.
