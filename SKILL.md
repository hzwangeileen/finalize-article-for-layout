---
name: finalize-article-for-layout
description: "Apply a fail-closed pre-layout quality gate to a final Chinese article. Use when an author wants to run a finished draft through proofreading before WeChat layout or Word export: fix safe typos, official-name casing, Chinese punctuation, AI-style em dashes, and excessive bold; inspect heading and list depth without restructuring; require embedded Feishu sheets to become complete native document tables; require Feishu 文字排版助手 normalization; collect special layout notes; and decide whether the article is ready to @ the typesetter."
---

# Finalize Article For Layout

Run a release gate, not a general rewrite. Use this sequence:

`AI proofreading → 作者处理结构问题 → 嵌入式表格转为飞书原生表格 → 飞书「文字排版助手」一键优化 → @排版负责人`

Do not report `可以交付排版` until every required check is either verified or explicitly resolved by the author.

## 1. Establish the source and verification boundary

- Accept a Feishu document, an open controllable Feishu page, or pasted article text.
- Treat the current Feishu document as the authority for headings, lists, tables, images, and rich-text formatting.
- When only pasted text is available, proofread the text but mark Feishu-only checks as `未验证`. Never infer that tables are native or that the formatting assistant ran.
- Require a near-final draft. If the article needs substantive changes, stop after the audit and return the required author actions.

## 2. Separate issues by ownership

### Automatically fix only mechanical issues

- Clear typos, missing or duplicated characters, and obvious grammatical slips.
- Confirm official capitalization and consistent spelling of companies, products, people, and abbreviations. Preserve `OpenAI` and `OAI`; never change them to `openai`, `Openai`, or `oai`.
- Use Chinese full-width punctuation in Chinese prose, especially Chinese quotation marks and commas.
- Preserve half-width punctuation inside URLs, email addresses, code, formulas, file names, and other machine-readable strings.
- Replace AI-favored em dashes (`——`) with a full stop or comma only when the meaning is unambiguous.
- Add no bold. Remove bold from bullet points, isolated words, and short phrases. Retain bold only for a complete sentence that is clearly intended as emphasis.
- Fix duplicate spaces and obvious spaces beside punctuation. Leave final Chinese–English and Chinese–number spacing to Feishu's built-in assistant.

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

## 3. Require native Feishu tables before Word export

Inspect every tabular block in the source Feishu document.

- Treat embedded spreadsheets and sheet cards as unsafe for Word export, even when their borders make them look like normal tables.
- Common embedded-sheet signals include `A/B/C` column labels, numbered row headers, an internal scrollbar, opening in spreadsheet view, or the absence of native document-table controls.
- Confirm a native Feishu document table through its native table block controls, such as the green table icon and block handle, and direct document-cell editing.
- Tell the author to rebuild each embedded sheet as a native Feishu document table. Do not perform this content-structural conversion silently.
- Require every header, row, column, cell value, unit, title, note, source, and reading order to survive. Convert formulas to displayed values when formulas cannot be preserved.
- Check hidden and scrollable rows or columns. A partially recreated table is a failure even when the visible cells are correct.
- Keep the embedded source until the native table has been compared against it. Verify row count, column count, and cell contents before removing the source block.
- Export a test Word file when possible and confirm the complete native table is present. A clipped or incomplete export blocks handoff.

If the Feishu UI is not controllable, output this author action exactly:

`请把嵌入式电子表格重建为飞书文档原生表格，逐行逐列核对完整，并确认导出的 Word 包含全部表格内容。`

Never claim a table passed when the original Feishu block and the resulting native table were not inspected.

## 4. Require Feishu formatting normalization

After the text and author-owned issues are resolved, run Feishu `文字排版助手` → `一键优化` to normalize:

- spacing between Chinese and English or numbers;
- full-width and half-width punctuation covered by the assistant.

This is a Feishu UI capability, not a Lark OpenAPI operation. If the UI is controllable and the user authorized execution, run it and verify the visible completion result. Otherwise, output:

`请在飞书中打开「文字排版助手」，点击「一键优化」，统一处理中英文空格和全、半角标点。`

Never mark this check as passed without visible confirmation from Feishu or explicit confirmation from the author.

## 5. Collect layout handoff notes

List any non-default requirement the typesetter needs before starting, including:

- no introduction;
- hand-drawn figures that need the 海外独角兽 template;
- special image, table, or layout treatment;
- a non-default publication time.

Do not invent requirements. Write `无` when the author confirms there are none.

## 6. Apply the fail-closed handoff gate

Mark `可以 @排版` only when all of the following are true:

- mechanical proofreading is complete;
- no uncertain spelling, typo, or wording remains;
- no author-owned structural issue remains;
- heading depth is at most three levels;
- bullet and ordered lists are flat;
- every table is either a verified native Feishu table or intentionally excluded;
- Feishu `文字排版助手` completion is confirmed;
- special layout requirements are recorded;
- the draft is final enough that no structural or piecemeal typo edits are expected after layout begins.

If any item is unresolved, use `暂不建议 @排版`. Do not soften a blocking issue into a generic reminder.

## 7. Return the fixed output contract

Always return these sections:

1. `终检状态` — `可以 @排版` or `暂不建议 @排版`.
2. `已自动修正` — concise list with counts or `无`.
3. `作者需处理` — each location, problem, rule, and action; write `无` only when empty.
4. `飞书操作` — native-table conversion and formatting-assistant status.
5. `排版备注` — confirmed special requirements or `无`.
6. `修正后全文` — include only when the input was pasted text or the user asks for a clean copy.

For a standalone prompt that authors can paste into another AI, read and return [references/author-prompt.md](references/author-prompt.md).
