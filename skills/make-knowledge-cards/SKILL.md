---
name: make-knowledge-cards
description: Convert pasted articles or local Markdown/TXT files into 5-8 source-grounded knowledge cards with one concept per card. Use for knowledge extraction, study cards, and self-test card requests; do not use for web scraping, PDF handling, Anki export, or GUI work.
---

# Make Knowledge Cards

Create Markdown knowledge cards from text the user supplies. Optimize for accurate recall and understanding, not for reaching a fixed card count.

## Scope

Supported inputs:

- Article text pasted into the conversation.
- A local `.md` or `.txt` file. Read the complete file and use only its contents.

Unsupported inputs and outputs:

- URL-only requests, web scraping, or fetching linked pages.
- PDFs, images, or OCR.
- Anki decks, Anki exports, or CSV files for Anki.
- Browser or desktop GUI automation.

For an unsupported input, briefly state the limitation and ask the user to paste the article text or provide a local `.md` or `.txt` file. Do not silently switch to web search, PDF extraction, OCR, or another tool.

## Workflow

1. Read the entire source, ignoring navigation text, boilerplate, ads, repeated calls to action, and non-content metadata.
2. Build a private inventory of candidate knowledge units. A knowledge unit is a distinct idea that can be recalled or tested independently, such as a definition, mechanism, cause, distinction, condition, process, rule, formula, or source-supported conclusion.
3. Remove repetitions, synonymous restatements, decorative examples, and points already covered by a more general idea. Merge related fragments when they express one testable idea.
4. Select the 5-8 most useful distinct units when the source contains that many. Prioritize foundational ideas, causal explanations, decision rules, and conditions that change how the idea applies.
5. If fewer than 5 distinct units are supported, output only the supported number. If none are supported, say the source is insufficient and ask for more substantive text. Never pad with generic advice, outside context, repeated explanations, or invented facts.
6. Order the cards logically, typically from definition or principle to mechanism, conditions, and application.

One card must explain one idea. Split a card if it combines independently testable ideas; merge two cards if doing so would not remove a distinct recall target. The 5-8 range is not a quota: never split an idea or promote a supporting detail to a separate card merely to reach the lower or upper bound.

Apply these merge tests:

- If a card only introduces an umbrella idea whose components are covered by separate cards, keep either the integrated umbrella card or the component cards, not both.
- If a card only says that two other cards differ, coexist, or cannot be substituted, fold that relationship into the relevant concept card.
- If a card only repeats a scope caveat such as "may", "depends on context", or "not universal", keep the caveat with the claim it limits unless the source treats that caveat as a separate decision rule.
- Before drafting, compare every pair of candidates. If one card's answer is already contained in another card's explanation, merge or drop one.

## Source Fidelity

- Treat the supplied text as the only factual source. Do not add background knowledge, definitions, examples, numbers, dates, names, causes, comparisons, recommendations, or quotations that are absent from it.
- Preserve important qualifiers such as "may", "usually", "only", "except", and stated conditions. Do not turn a possibility into a certainty or a suggestion into a rule.
- Do not silently correct, complete, or adjudicate the source. For a contestable or explicitly attributed claim, make the attribution clear, for example "原文认为" or "The source argues".
- If a statement is too ambiguous or contradictory to explain faithfully, omit it from the cards rather than guessing.
- A title or core marker may be newly phrased as a recall cue, but it must not introduce a new factual claim or imply that the source coined that term.

## Card Content

Use the following fields. Localize the labels into the output language; when the request is in Chinese, use the Chinese labels shown here.

```markdown
## 1. 标题
- **核心标识**：可直接回忆的短词、规则、公式、阈值或对比。
- **简明解释**：用 1-3 句话说明它是什么、为什么或如何成立。保留条件与限定词。
- **例子或自测**：原文中能说明该点的例子；若没有，则写一个可由卡片或原文作答的自测题。可附一句原文可支持的参考要点。
```

Field requirements:

- **Title:** Specific, self-contained, and focused on the card's single idea. Avoid generic labels such as "知识点 1" or clickbait.
- **Core marker:** A concise recall cue, preferably a short noun phrase, rule, threshold, or contrast. Use the source's term when it has one; otherwise use a faithful descriptive label that is not presented as the author's terminology. If the marker reads like a full explanatory sentence, shorten it.
- **Explanation:** Paraphrase rather than copying long passages. Define unfamiliar shorthand, preserve scope and exceptions, and explain the relationship between facts rather than merely restating the title.
- **Example or self-test:** Prefer a relevant example from the source. If none exists, ask one question that tests recall, explanation, comparison, or application using only supplied facts. Do not invent a hypothetical scenario that adds external details. An answer key is optional and must also stay grounded.

## Output

Return cards directly in Markdown:

```markdown
## 知识卡片（N 张）

## 1. ...
- **核心标识**：...
- **简明解释**：...
- **例子或自测**：...

## 2. ...
...
```

If the source supports fewer than 5 cards, the shortfall notice is mandatory. Insert one brief sentence immediately after the card heading and before the first card: "原素材仅支持 N 个独立知识点，因此未凑足 5 张。" Use an equivalent sentence in the output language. Do not add a long analysis of what was excluded unless the user asks.

## Quality Check

Before responding, verify:

- Every card passes the one-idea test.
- No two cards teach the same idea.
- No umbrella card is duplicated by its component cards, and no card exists only for a relation or scope caveat.
- Every claim, number, example, and answer key is traceable to the supplied text.
- No qualifying words or conditions have been weakened, strengthened, or dropped.
- Every card has all four requested components.
- The total is 5-8 only when that many distinct knowledge units are actually available.
- When the total is below 5, the mandatory shortfall notice appears before the first card.
- The cards contain no analysis of the skill, no fabricated gaps filled from outside knowledge, and no unsupported embellishment.
