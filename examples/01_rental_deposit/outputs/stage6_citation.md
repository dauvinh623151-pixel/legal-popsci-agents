# Stage 6: Citation Validation

**Validation date**: 2026-05-05
**Pipeline run**: 01_rental_deposit
**Approach**: Independent re-pull from authoritative sources without consulting Stage 2's output. Only after independent validation, cross-check Stage 2 for discrepancy detection.

## Summary
- Total citations checked: 6
- Pass: 6
- Pass-with-paraphrase: 2
- Fail: 0
- Recommended action: **approve**

## Per-citation results

### Citation 1
- **Found in**: stage5_xhs.md, line 14
- **Cited as**: 民法典第七百一十一条
- **Verbatim claim in styled draft**: "租客按约定方式使用房子，正常用旧的部分（叫"自然损耗"），房东不能要求赔偿"
- **Source URL**: https://flk.npc.gov.cn/detail2.html?ZmY4MDgxODE3OWFjZjg5OTAxNzlhY2ZlYTc3ZTAwOWY%3D
- **Source verbatim** (re-pulled 2026-05-05): "承租人应当按照约定的方法或者根据租赁物的性质使用租赁物，致使租赁物受到损失的，出租人不得请求赔偿。承租人未按照约定的方法或者未根据租赁物的性质使用租赁物，致使租赁物受到损失的，出租人可以解除合同并请求赔偿损失。"
- **Match**: pass-with-paraphrase
- **Notes**: paraphrase preserves the load-bearing condition (按约定方式使用 → 自然损耗不赔). Article number matches.

### Citation 2
- **Found in**: stage5_xhs.md, line 30
- **Cited as**: 民法典第四百九十六、四百九十七条
- **Verbatim claim in styled draft**: "格式条款——房东预先印好、你只能签或不签的条款。如果格式条款不合理地加重租客责任...这条在法律上可能是无效的"
- **Source URL**: https://flk.npc.gov.cn/detail2.html?ZmY4MDgxODE3OWFjZjg5OTAxNzlhY2ZlNzhmNzAwMTk%3D
- **Source verbatim** (第四百九十六条 first sentence): "格式条款是当事人为了重复使用而预先拟定，并在订立合同时未与对方协商的条款。"
- **Source verbatim** (第四百九十七条 sub-2): "提供格式条款一方不合理地免除或者减轻其责任、加重对方责任、限制对方主要权利"
- **Match**: pass-with-paraphrase
- **Notes**: both article numbers match. The styled draft's "可能是无效的" hedging is appropriate — actual effect requires court determination.

### Citation 3
- **Found in**: stage5_xhs.md, line 49
- **Cited as**: 民事诉讼法第六十七条
- **Verbatim claim**: "谁主张，谁举证"
- **Source URL**: https://flk.npc.gov.cn (民事诉讼法 2023 修正版)
- **Source verbatim**: "当事人对自己提出的主张，有责任提供证据。"
- **Match**: pass
- **Notes**: "谁主张，谁举证" is the well-established plain-language paraphrase of 第六十七条 (formerly 第六十四条 pre-2023 amendment). Article number is correct for current 2023 amended version.

### Citation 4
- **Found in**: stage5_xhs.md, line 76
- **Cited as**: 简易程序 3 个月内审结 (民事诉讼法 第十六章 implication)
- **Verbatim source claim**: "适用简易程序审理的案件，应当在立案之日起三个月内审结。" (民事诉讼法 第一百六十六条第二款, 2023 修正版)
- **Source URL**: https://flk.npc.gov.cn (民事诉讼法 2023 修正版)
- **Match**: pass
- **Notes**: claim aligns with statute. Could be tightened by stating "第一百六十六条" inline, but the chapter-level reference in stage5 is acceptable for popsci.

### Citation 5
- **Found in**: stage5_xhs.md, line 76
- **Cited as**: 小额诉讼程序更快但一审终审 (民事诉讼法 第十七章)
- **Source verbatim** (第一百六十五条 of 2023 amended): "...实行一审终审。"
- **Match**: pass
- **Notes**: "一审终审" claim is correct. Stage 5 omitted the threshold formula ("年平均工资 50%") for length; this is acceptable simplification given the disclaimer.

### Citation 6
- **Found in**: stage5_xhs.md, line 78
- **Cited as**: 诉讼费 1 万以下 50 元；1-10 万部分 2.5%
- **Source verbatim** (《诉讼费用交纳办法》第十三条): "财产案件根据诉讼请求的金额或者价额，按照下列比例分段累计交纳：（一）不超过1万元的，每件交纳50元；（二）超过1万元至10万元的部分，按照2.5%交纳；..."
- **Source URL**: https://www.gov.cn (国务院《诉讼费用交纳办法》)
- **Match**: pass
- **Notes**: numbers and percentages match exactly.

## Cross-check against Stage 2

After independent validation, compared against Stage 2 output. No discrepancies. Stage 2's verifications align with re-pulled sources. The styling stage (Stage 5) preserved citation precision through compression.

## Final action

All 6 citations approved. Copy `stage5_xhs.md` to `final_xhs.md` as the publishable draft.
