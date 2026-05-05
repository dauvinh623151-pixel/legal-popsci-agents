# Design Principles

Five principles guide every decision in this repository. They are listed in priority order — when two principles conflict, the higher-numbered one yields.

## 1. First-principles framing over copying conventions

We did not start by asking "how do other legal-content tools work". We asked: what do readers actually need from a legal popsci post, and what failure modes most often produce useless or unsafe output? The 6-stage pipeline is the answer that fell out of that question, not a copy of any existing system.

This applies recursively. When extending the pipeline, do not add a stage because "every content pipeline has X". Add a stage only when there is a failure mode that no current stage owns, or a discipline that no current stage carries.

## 2. Constraints first, content second

Before drafting any new agent or skill, write down the explicit rules it will enforce. This repo's `skills/plain-language-rules/SKILL.md` is the canonical example: the rule pack precedes any rewriting work, and rule updates lead, not follow, content updates.

The reason: LLM-generated content drifts predictably toward the most rewarded objective. Without explicit constraints, the model rewards itself for fluency and engagement at the expense of legal safety. With explicit constraints, the model has a hard wall to push against.

## 3. Single-responsibility per agent

Each agent owns exactly one discipline. The plain-language rewriter is judged solely on clarity. The compliance reviewer is judged solely on legal safety. The citation validator is judged solely on source fidelity.

When writing an agent prompt, the test is: can you state the agent's job in one sentence? If you need an "and" — "rewrites for clarity AND checks compliance" — split it.

This principle is the load-bearing reason this is a 6-agent pipeline rather than one big prompt. (See [architecture.md § Why six agents](architecture.md#why-six-agents-not-one).)

## 4. Surgical changes, not optimisation drift

When editing an existing agent, skill, or example, change only what the change request asks for. Do not "improve" surrounding text, do not "modernise" formatting, do not "tighten" wording that wasn't in scope.

Two reasons:
- **Auditability**: every diff should map back to a specific intent. Diffs that bundle unrelated edits make it impossible to attribute later regressions.
- **Reviewer cost**: a 200-line diff for a 5-line change burns reviewer trust and attention.

When you spot a real issue outside scope, file it as an issue or note it in the commit message. Do not silently bundle it.

## 5. Verifiable success criteria, not vibes

Every agent's output specification is testable. "Rewrite for clarity" is not testable; "every sentence ≤ 25 字, every legal term translated on first use, every applicability condition preserved" is testable. Stage 6 in particular exists because "the draft cites the right statute" is testable in a way that "the draft is correct" is not.

When proposing changes:
- The change request should specify a verifiable outcome, not a feeling
- "Make Stage 5 better" is not actionable; "Stage 5's xhs output should keep title under 20 字 in 100% of test runs" is

## Anti-patterns we explicitly reject

### Anti-pattern A: "smart" prompts that bundle disciplines
> "You are a legal content writer who is accurate, clear, compliant, and engaging."

This produces drift across all four properties because the model has no signal for which to prioritise. Replace with single-discipline prompts.

### Anti-pattern B: hidden citations
A citation that appears inline in body text without an explicit `《...》第...条` form is unverifiable downstream. Always use the canonical citation format defined in [skills/legal-citation-format/SKILL.md](../skills/legal-citation-format/SKILL.md).

### Anti-pattern C: rule packs in prose
Rules expressed as paragraph text are nearly impossible to maintain. Rules expressed as tables and word-lists are diffable, scannable, and updatable. The compliance rule pack is structured this way for a reason — keep it that way when adding new rules.

### Anti-pattern D: per-platform forks of the same draft
If you find yourself maintaining separate xhs / wechat / script drafts of the same legal content with the same statutes cited differently, your styling is leaking into substance. Stage 5 changes form, not content.

### Anti-pattern E: trust-the-LLM for citation
"The model knows what 《民法典》第1024条 says" — do not trust this. Always re-pull from authoritative source. Stage 6 exists precisely to make this verification an institutional habit.

## How to apply these principles

When proposing a change to this repository:

1. State the **failure mode** the change addresses (not "improvement", a specific thing that goes wrong now)
2. State which **principle** justifies the change
3. State the **verifiable success criterion** — how will we know it worked?
4. Confine the diff to the scope from (1)

When reviewing a change:

1. Does the diff match the stated scope?
2. Do the principles support this direction?
3. Will the success criterion in (3) actually be measurable?

If any answer is no, request clarification before approving.
