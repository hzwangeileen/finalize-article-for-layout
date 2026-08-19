---
name: finalize-article-for-layout
description: "Apply a fail-closed pre-layout quality gate to a final Chinese article. Use when an author wants to run a finished draft through proofreading before WeChat layout or Word export: fix safe typos, official-name casing, Chinese punctuation, Chinese-English spacing, AI-style em dashes, and excessive bold; inspect heading and list depth without restructuring; automatically replace embedded Feishu sheets with complete native document tables; collect special layout notes; and decide whether the article is ready to @ the typesetter."
---

# Finalize Article For Layout

Run a release gate, not a general rewrite. Use this sequence:

`AI proofreading 与中英文空格规范 → 作者处理结构问题 → Skill 自动把嵌入式表格转为飞书原生表格 → @排版负责人`

Do not report `可以交付排版` until every required check is either verified or explicitly resolved by the author.

## 1. Establish the source and verification boundary

- Accept a Feishu document, an open controllable Feishu page, or pasted article text.
- Treat the current Feishu document as the authority for headings, lists, tables, images, and rich-text formatting.
- When only pasted text is available, proofread and normalize the text. If it contains or may have originated from a table, mark the conversion `受阻：需要飞书原文链接或可控页面`; never infer that a table is native or complete.
- Require a near-final draft. If the article needs substantive changes, stop after the audit and return the required author actions.

## 2. Separate issues by ownership

### Automatically fix only mechanical issues

- Clear typos, missing or duplicated characters, and obvious grammatical slips.
- Confirm official capitalization and consistent spelling of companies, products, people, and abbreviations. Preserve `OpenAI` and `OAI`; never change them to `openai`, `Openai`, or `oai`.
- Use Chinese full-width punctuation in Chinese prose, especially Chinese quotation marks and commas.
- Preserve half-width punctuation inside URLs, email addresses, code, formulas, file names, and other machine-readable strings.
- Replace AI-favored em dashes (`——`) with a full stop or comma only when the meaning is unambiguous.
- Add no bold. Remove bold from bullet points, isolated words, and short phrases. Retain bold only for a complete sentence that is clearly intended as emphasis.
- In ordinary prose, insert exactly one half-width space at every direct boundary between a Chinese character and an English letter sequence, in either direction. For example, change `AI写作` to `AI 写作` and `使用OpenAI模型` to `使用 OpenAI 模型`.
- Collapse multiple spaces at those Chinese-English boundaries to one. Do not leave a space immediately before Chinese punctuation.
- Do not alter spacing inside URLs, email addresses, code, formulas, file names, or other machine-readable strings.

Do not silently change an uncertain proper noun or ambiguous sentence. Preserve it and send it to `作者需处理`.

### Audit structure; never rewrite it for the author

Check and locate each violation:

- The complete heading system uses no more than three levels.
- Bullet points and ordered lists use one level only; nested sublists are not allowed.
- Headings appear in a coherent order without an unexplained depth jump.
- Sections, paragraphs, conclusions, and examples do not obviously require splitting, merging, moving, or rewriting before publication.

Any fix that changes hierarchy, parent-child meaning, section order, paragraph scope, or argumentative logic belongs to the author. Do not flatten, merge, split, reorder, rename, or rewrite it. Report:

1. the exact heading, paragraph, or list item;
2. the problem;
3. the rule it violates;
4. the concrete action the author should take.

After the author edits the document, rerun the entire gate.

## 3. Automatically convert embedded Feishu sheets before Word export

Inspect every tabular block in the source Feishu document.

- Treat embedded spreadsheets and sheet cards as unsafe for Word export, even when their borders make them look like normal tables.
- Common embedded-sheet signals include `A/B/C` column labels, numbered row headers, an internal scrollbar, opening in spreadsheet view, or the absence of native document-table controls.
- Confirm a native Feishu document table through its native table block controls, such as the green table icon and block handle, and direct document-cell editing.
- Automatically replace every embedded sheet with a complete native Feishu document table. This conversion is Skill-owned; do not send it back to the author as routine manual work.
- Before doing any conversion, read [references/feishu-embedded-table-conversion.md](references/feishu-embedded-table-conversion.md) and follow it exactly.
- Preserve every header, row, column, displayed cell value, unit, title, note, source, merge, and reading order that is present in the source. Formulas become their displayed values because a document table is a publication snapshot.
- Include hidden and scrollable rows or columns when they contain source content. A partially recreated table is a failure even when the visible viewport is correct.
- Keep the embedded source block until the new native table has passed dimension and cell-by-cell comparison. Delete the source only after verification succeeds.
- If direct document and sheet APIs cannot access a specific block, use an authorized controllable Feishu UI. If neither route can read and replace it, preserve the original and return a precise blocker; never claim success or silently fall back to asking the author to rebuild it.
- Export a test Word file when the publication workflow makes export available, and confirm the complete native table is present. A clipped or incomplete export blocks handoff.

Never claim a table passed when the original embedded block, its complete source range, and the resulting native table were not all inspected.

## 4. Collect layout handoff notes

List any non-default requirement the typesetter needs before starting, including:

- no introduction;
- hand-drawn figures that need the 海外独角兽 template;
- special image, table, or layout treatment;
- a non-default publication time.

Do not invent requirements. Write `无` when the author confirms there are none.

## 5. Apply the fail-closed handoff gate

Mark `可以 @排版` only when all of the following are true:

- mechanical proofreading is complete;
- no uncertain spelling, typo, or wording remains;
- no author-owned structural issue remains;
- every Chinese-English boundary in ordinary prose contains exactly one half-width space;
- heading depth is at most three levels;
- bullet and ordered lists are flat;
- every table is either a verified native Feishu table or intentionally excluded by the author;
- special layout requirements are recorded;
- the draft is final enough that no structural or piecemeal typo edits are expected after layout begins.

If any item is unresolved, use `暂不建议 @排版`. Do not soften a blocking issue into a generic reminder.

## 6. Return the fixed output contract

Always return these sections:

1. `终检状态` — `可以 @排版` or `暂不建议 @排版`.
2. `已自动修正` — concise list or `无`; include counts only when they can be determined reliably.
3. `作者需处理` — each location, problem, rule, and action; write `无` only when empty.
4. `飞书表格` — number found, number converted, dimension and cell comparison, source-block removal, and Word-export status.
5. `排版备注` — confirmed special requirements or `无`.
6. `修正后全文` — include only when the input was pasted text or the user asks for a clean copy.

For a standalone prompt that authors can paste into another AI, read and return [references/author-prompt.md](references/author-prompt.md).
