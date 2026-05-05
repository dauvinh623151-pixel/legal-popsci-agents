---
name: plain-language-rules
description: Use this skill in Stage 3 (plain-language-rewriter) and Stage 4 (compliance-reviewer) to apply or audit the plain-language and compliance rules for Chinese legal popsci. Defines forbidden absolute-claim word lists, jargon-to-plain substitution tables, and platform-specific tag-risk lists.
---

# Plain Language and Compliance Rule Pack

This skill is the canonical source of rules and word lists used by the rewriter (Stage 3) and the compliance reviewer (Stage 4). It is updated as platform rules and regulations change. **Updated: 2026-05-05**.

## Section A: Absolute-claim word list (《广告法》Article 9)

Hard-blocked tier (high-severity violation if found in published text):
```
最佳, 最权威, 最强, 最优, 最高级, 最先进, 最快, 最便宜, 最有效,
唯一, 独一, 第一, 首选, 顶级, 顶尖, 极致, 神器, 王炸, 必入,
国家级, 世界级, 全国第一, 全球领先, 行业第一, 业内第一,
百分百, 100%, 包赢, 包过, 必胜, 一定, 必然, 绝对, 永远,
彻底解决, 完美解决, 全方位解决
```

Soft-blocked tier (medium-severity, often acceptable with qualifier):
```
非常, 极度, 高度, 充分, 显著
(acceptable with measured qualifier: "非常有效" → "在大多数情况下有效")
```

## Section B: Outcome-guarantee patterns (《律师法》+ 《律师执业行为规范》)

These are **structural** patterns, not lexical. A keyword scan won't catch them; semantic review must.

| Pattern | Why blocked | Fix |
|---|---|---|
| "只要你按这样做，就一定能要到X" | Outcome guarantee | "如果同时满足以下条件，通常可以争取X" |
| "找律师就能解决" | Service-outcome guarantee | "律师可以帮你梳理思路并代为协商/诉讼" |
| "我办的XX案100%胜诉" | Case-result advertising | Drop entirely; cite generic legal principle instead |
| "其他律师做不到的，我们可以" | Disparaging peers | Drop comparison entirely |
| "我有内部关系" | Improper-influence claim | Drop entirely; severe risk |

## Section C: Jargon → Plain substitution table

Use in Stage 3 (plain-language-rewriter). Apply on first use of each jargon term.

| Legal jargon | Plain Chinese |
|---|---|
| 善意取得 | 你买的时候不知道这东西是别人的 |
| 不当得利 | 你拿了不该拿的东西/钱，没有正当理由 |
| 表见代理 | 看起来有代理权，实际上没有 |
| 追认 | 事后承认（让原本无效的行为变有效） |
| 撤销 | 把一个法律行为取消掉 |
| 解除 | 终止一个还在进行的合同 |
| 主张 | 提出（诉求） |
| 履行 | 做到 / 完成（合同里答应的事） |
| 救济 | 法律上能用的解决办法 |
| 胜诉 | 打赢官司 |
| 败诉 | 打输官司 |
| 诉讼时效 | 法律允许你告对方的最长期限 |
| 举证责任 | 谁说有，谁要拿出证据 |
| 民事行为能力 | 法律上能不能自己做决定 |
| 法定代表人 | 公司里能代表公司签字的那个人 |
| 连带责任 | 多个人一起承担，债主可以找任何一个要钱 |
| 抗辩权 | 你有权说"我不还" / "我不付"的理由 |
| 善意第三人 | 不知情的局外人 |
| 物权 | 你对一个东西的"所有权 / 使用权 / 抵押权"等权利 |
| 债权 | 别人欠你东西/钱，你可以要回的权利 |

When using a jargon term unavoidably (e.g., the statute uses it), translate in parentheses on first use, then continue using the term:

```
善意取得（你买的时候不知道东西是别人的），是民法典里的一个制度……
```

## Section D: Platform-specific tag-risk list

### Xiaohongshu

**Safe high-traffic tags**:
```
#法律科普 #民法典 #劳动法 #消费者权益 #普法 #打工人 #租房 #维权指南
```

**Limit-distribution risk tags** (avoid as primary, OK as one of many):
```
#维权 (overused, sometimes flagged)
#打官司
#律师
```

**Hard-block tags** (avoid entirely):
```
#胜诉率 #包赢 #必胜 #内部消息 #潜规则
#上海最好律师 等含「最/第一」的地域排序词
```

### WeChat Official Account

WeChat enforces less on tags; main risks are in title/body:
- Title 标题党: opening with "震惊" / "万万没想到" / "再不看就晚了"
- Body 焦虑营销: "如果你不知道这一点，将损失 X"
- Body 谣言: medical / 命理 / 投资建议 even tangentially mixed in

### 抖音 / 快手 (短视频)

- Avoid 焦虑营销 hooks ("90% 的人都被坑过", "再不知道就晚了")
- Avoid showing real document photos (合同照片、判决书) unless heavily redacted
- Avoid律所招牌、律师证书 visible in shot (compliance + privacy)

## Section E: Disclaimer templates (when required)

When Stage 4 determines a disclaimer is needed, use these canonical forms.

### General popsci disclaimer (default)
```
本文内容仅作普法参考，不构成对个案的法律意见。具体情况请咨询执业律师。
```

### Statute-time-sensitive disclaimer
```
本文法律依据截至 <YYYY-MM-DD> 现行有效，后续如有修订请以最新版本为准。
```

### Worked-example disclaimer
```
文中人物均为虚构，案例为说明法条适用而设。如与现实雷同，纯属巧合。
```

### Tax / 投资 / 医疗 cross-domain disclaimer (required when topic touches these)
```
本文不涉及税务/投资/医疗专业建议。涉及上述领域请咨询相应专业人士。
```

Place disclaimers at the **end** of the body, before tags/CTA. Do not bury in middle of text.

## Section F: Forbidden topics for popsci

These topics should not be answered as popsci; if encountered, the orchestrator should escalate:

- 任何刑事案件具体辩护策略 (criminal-defense strategy specifics)
- 上市公司证券违规具体操作 (listed-company securities specifics)
- 涉及国家安全、宗教、民族议题的法律分析
- 涉及未成年人、被害人具体身份的案件分析
- 用户提供了真实人名/身份证号/案号 (refuse and request redaction)

## Update protocol

When platform rules change or a new regulation lands:
1. Add the new rule to the appropriate section above with its effective date
2. Run all checked-in examples through the pipeline to confirm they still pass
3. Update this file's `Updated:` header
4. Note the change in CHANGELOG (when CHANGELOG exists; for now, in commit message)
