---
name: finalize-article-for-layout
description: "Run the final pre-layout check for a Chinese article after writing and structural editing are complete. Use when the user asks for proofreading, typo cleanup, proper-noun and English capitalization checks, Chinese punctuation normalization, heading or list-depth checks, restrained bold formatting, removal of AI-style em dashes, conversion of embedded Feishu spreadsheet blocks into native document tables before Word export, Feishu/Lark 文字排版助手 handoff, or a final article ready to @ the typesetter for WeChat publication."
---

# Finalize Article For Layout

Prepare a nearly final Chinese article for layout without rewriting it. Keep the workflow in this order:

`AI proofreading → 嵌入式表格转为飞书原生表格 → 飞书「文字排版助手」一键优化 → @排版负责人`

## Establish the boundary

- Require a structurally final draft. If the article still needs substantive rewriting or section changes, finish those first and rerun this workflow afterward.
- Correct only clear errors. Preserve meaning, facts, paragraph order, heading hierarchy, lists, tables, links, images, and authorial voice.
- Do not introduce new facts, smooth over uncertainty, or silently change a proper noun whose official form is uncertain.
- Keep the article's complete heading system to no more than three levels.
- Keep bullet points and ordered lists to one level. Do not create nested sublists.

## Run AI proofreading

Check the complete article once for:

- typos, missing or duplicated characters, obvious grammar errors, repeated spaces, and misplaced spaces around punctuation;
- official capitalization and consistent spelling of companies, products, people, and abbreviations;
- Chinese full-width punctuation in Chinese prose, especially Chinese quotation marks and commas;
- AI-favored em dashes (`——`), replacing them with a full stop or comma only when the meaning is preserved;
- excessive bold formatting.

Apply these hard rules:

- Preserve official forms such as `OpenAI` and `OAI`; never normalize them to `openai`, `Openai`, or `oai`.
- Preserve half-width punctuation inside URLs, email addresses, code, formulas, file names, and other machine-readable strings.
- Do not add bold. Remove bold from bullet points, isolated words, and short phrases. Retain bold only for a complete sentence that is clearly intended as emphasis.
- Flatten a heading or list level only when this changes presentation without changing content, order, or parent-child meaning. Otherwise retain it and add the structural issue to `待确认项`.
- Do not decide Chinese–English or Chinese–number spacing manually beyond fixing obvious duplicate spaces. Leave final spacing normalization to Feishu's built-in assistant.
- If an official spelling or intended correction cannot be verified, retain the source text and add it to `待确认项`.

Return the corrected full article first. Then return `待确认项`; write `无` when empty.

## Use the ready-to-copy prompt when requested

```text
请对下面这篇文章做一次发布前 proofreading：

1. 只修正明确错误，不改写文章，不调整结构、段落顺序和原意，也不要增删信息。除为满足第 7 条进行无损的层级规范外，不要调整标题和列表层级。

2. 检查错别字、漏字、重复字、明显病句，以及专有名词、公司名、产品名和英文缩写的大小写。尤其注意统一官方名称，例如 OpenAI 不能写成 openai 或 Openai，OAI 不能写成 oai。无法确认的名称请保留原文并列为待确认项，不要自行猜测。

3. 中文正文统一使用全角标点，尤其不要出现英文半角双引号和逗号。网址、邮箱、代码、公式及文件名中的英文符号不要修改。

4. 尽量不要使用破折号（——）。请根据语义替换成句号或逗号，但不要改变原意。

5. 不要新增加粗。删除 bullet point、单个词或短语的加粗，只保留真正需要强调的完整句子。

6. 不要擅自删除中文与英文、中文与数字之间的空格，这部分会使用飞书「文字排版助手」统一处理。

7. 检查文章层级：全文标题体系最多使用三个层次；bullet point 和有序列表（编号条目）都只允许一层，不要嵌套。只有在不改变内容、顺序和从属关系时才可以直接扁平化；如果调整会影响文章逻辑，请保留原文并列为待确认项。

8. 如果可以访问原飞书文档，请检查是否存在嵌入式电子表格。此类表格通常会显示 A、B、C 等列标和 1、2、3 等行号，直接导出 Word 可能只保留部分内容。发现后请标记为“需转为飞书原生表格”，不要把不完整的 Word 导出结果当作最终表格。如果当前只能看到粘贴后的纯文本，请在待确认项中提醒人工检查飞书原文。

请先输出修正后的完整文章，再单独列出待确认项；如果没有待确认项，请写“无”。

以下是需要 proofreading 的文章：

【粘贴全文】
```

## Convert embedded tables before Word export

Inspect every tabular block in the source Feishu document before export.

- Treat a spreadsheet or sheet card embedded in the document as unsafe for Word export. Common signs include column letters such as `A/B/C`, numbered row headers, a sheet-style grid, scrolling inside the block, or opening the block in a spreadsheet view. Do not classify the block only by its visual borders because an embedded sheet can look like a normal table when its spreadsheet chrome is hidden.
- Confirm the replacement is a native Feishu document table by its native table block controls, such as the green table icon and block handle, and by direct document-cell editing rather than an embedded spreadsheet view.
- Replace each unsafe embedded block with a native Feishu document table before exporting Word. Do not replace it with a screenshot when the cells need to remain complete and editable.
- Preserve every visible row, column, cell value, header, unit, title, note, source, and reading order. Convert formulas to their displayed values when the native table cannot preserve formulas.
- Check hidden or scrollable rows and columns so the native table contains the full data set, not only the currently visible viewport. A partially recreated table is still a failure even if the visible cells are correct.
- Keep the original embedded block until the native table has been verified. Compare row count, column count, and cell contents, then remove or replace the embedded block.
- Export a test Word file when possible and confirm the entire native table is present. Do not mark the handoff ready if the exported table is clipped or incomplete.

This conversion requires access to the Feishu document UI and is not equivalent to editing the incomplete Word export. If a controllable Feishu UI is open and the user authorized end-to-end execution, use the applicable browser or computer-control workflow. Otherwise, give the user this exact next step:

`请先把带列标和行号的飞书嵌入式表格重建为飞书文档原生表格，逐行逐列核对完整后，再导出 Word。`

Never claim the table was converted or exported completely unless the source and resulting native table were checked.

## Run Feishu formatting normalization

After AI proofreading, run Feishu's built-in `文字排版助手` and select `一键优化` to normalize:

- spacing between Chinese and English or numbers;
- full-width and half-width punctuation covered by the assistant.

This is a Feishu UI capability, not a Lark OpenAPI operation. If a controllable Feishu UI is open and the user authorized end-to-end execution, use the applicable browser or computer-control workflow and verify the assistant reports completion. Otherwise, give the user this exact next step:

`请在飞书中打开「文字排版助手」，点击「一键优化」，统一处理中英文空格和全、半角标点。`

Never claim this step ran unless the UI visibly confirmed it.

## Hand off for layout

- Treat the document after embedded-table conversion and Feishu optimization as the delivery version.
- Ask the author to resolve every `待确认项` before handoff.
- Do not make further piecemeal typo or structural edits after layout starts. If content changes materially, rerun the entire workflow.
- Finish with: `终检完成后，请 @排版负责人开始排版。`
