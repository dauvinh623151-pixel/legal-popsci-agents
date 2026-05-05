---
name: legal-citation-format
description: Use this skill whenever an agent in the pipeline needs to format, parse, or validate a Chinese statute citation. Defines the canonical format (statute name + article + subsection + item + effective date), provides parsers, and lists common drift patterns to detect.
---

# Legal Citation Format

The canonical format for citing a mainland-China statute in this pipeline:

```
《<statute-full-official-name>》第<article-number>条第<subsection-number>款第<item-number>项 (effective <YYYY-MM-DD>)
```

Examples:
- `《中华人民共和国民法典》第1024条 (effective 2021-01-01)`
- `《中华人民共和国劳动合同法》第38条第1款第3项 (effective 2013-07-01 amended)`
- `《中华人民共和国消费者权益保护法》第25条 (effective 2014-03-15)`

Short form is acceptable in body text after first full mention:
- First use: `《中华人民共和国民法典》第1024条 (effective 2021-01-01)`
- Subsequent: `《民法典》第1024条`

## When to use this skill

- **Always** when writing or validating a statute citation
- **Always** when an agent's output needs to interoperate with another agent that consumes citations (e.g., Stage 2 → Stage 6)
- **Optional** when the citation appears in a free-form note for the human reviewer (still encouraged)

## Format components

| Component | Required | Notes |
|---|---|---|
| 《》 brackets around statute name | Yes | These are the Chinese book-title brackets, U+300A and U+300B. Not regular angle brackets. |
| Full official name on first use | Yes | "中华人民共和国" prefix included on first use |
| 条 (article) number | Yes | Use Arabic digits (1024) or full Chinese (一千零二十四), be consistent within a document |
| 款 (subsection) number | If meaningful | Drop only if statute has no subsections OR all subsections are equally relevant |
| 项 (item) number | If meaningful | Same rule as 款 |
| Effective date | First use, yes | Format `effective YYYY-MM-DD` or `effective YYYY-MM-DD amended` |

## Authoritative sources for verification

In priority order:
1. **国家法律法规数据库** (flk.npc.gov.cn) — fastest, cleanest navigation
2. **全国人大** (npc.gov.cn) — authoritative, slower
3. **最高人民法院** (court.gov.cn) — for judicial interpretations only
4. **国务院各部委官网** — for ministry-issued regulations

**Never** use as primary source:
- Wikipedia / Baidu Baike
- 北大法宝 (cross-reference only — paywalled, may have outdated cached versions)
- Random law-firm websites
- Bar association blog posts

## Common drift patterns to detect

### Pattern 1: Article number transposition
- "第一千零二十四条" vs "第一千零四十二条" — easy slip when typing Chinese numerals
- "第1024条" vs "第1042条" — same risk in digit form
- **Detection**: re-pull from authoritative source character-by-character

### Pattern 2: Pre-Civil-Code statute citation post-2021
- 《合同法》, 《物权法》, 《侵权责任法》, 《婚姻法》, 《继承法》 — all repealed and merged into 《民法典》 effective 2021-01-01
- Article numbers from these old statutes do not survive into the Civil Code unchanged
- **Detection**: any draft citing these statutes should trigger a hard fail and a rewrite to 《民法典》 article numbers

### Pattern 3: Lost subsection precision during compression
- Stage 2 cites "第38条第2款" → Stage 5 compresses to "第38条" → meaning narrows incorrectly because 第38条第1款 has different scope
- **Detection**: compare 款/项 fields between stages

### Pattern 4: Dropped effective-date marker
- Citation otherwise correct but reader has no way to know if law has changed
- **Detection**: every citation in stage 5/6 output should have a date marker on first use

### Pattern 5: 总则 vs 分则 confusion
- 民法典 has 总则 + 7 分编 (物权 / 合同 / 人格权 / 婚姻家庭 / 继承 / 侵权责任 / 附则)
- Article numbers run continuously (1–1260), but the 编 boundary matters semantically
- **Detection**: when citing, note which 编 the article lives in if relevant to the substance

## Format violations: severity table

| Violation | Severity |
|---|---|
| Wrong article number | High — fail |
| Wrong statute name | High — fail |
| Citing repealed statute | High — fail (must rewrite to current law) |
| Subsection dropped where meaningful | Medium — fail |
| Effective-date marker dropped | Low — pass-with-note |
| Using 《》 vs `<>` brackets | Low — fix-on-write |
| Inconsistent Arabic / Chinese numeral mix | Low — fix-on-write |

## Quick parser regex (for tooling)

A pragmatic regex for extracting citations from Chinese text:

```
《[^《》]{2,40}》\s*第[\d一二三四五六七八九十百千零]+条(?:第[\d一二三四五六七八九十]+款)?(?:第[\d一二三四五六七八九十]+项)?
```

Note: this is a heuristic, not a parser. Edge cases (司法解释 with different numbering schemes) require manual review.

## Related skills

- [plain-language-rules](../plain-language-rules/SKILL.md) — for translating cited text into reader-readable language
