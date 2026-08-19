---
name: finalize-article-for-layout
description: "Run the final pre-layout check for a Chinese article after writing and structural editing are complete. Use when the user asks for proofreading, typo cleanup, proper-noun and English capitalization checks, Chinese punctuation normalization, restrained bold formatting, removal of AI-style em dashes, Feishu/Lark 文字排版助手 handoff, or a final article ready to @ the typesetter for WeChat publication."
---

# Finalize Article For Layout

Prepare a nearly final Chinese article for layout without rewriting it. Keep the workflow in this order:

`AI proofreading → 飞书「文字排版助手」一键优化 → @排版负责人`

## Establish the boundary

- Require a structurally final draft. If the article still needs substantive rewriting or section changes, finish those first and rerun this workflow afterward.
- Correct only clear errors. Preserve meaning, facts, paragraph order, heading hierarchy, lists, tables, links, images, and authorial voice.
- Do not introduce new facts, smooth over uncertainty, or silently change a proper noun whose official form is uncertain.

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
- Do not decide Chinese–English or Chinese–number spacing manually beyond fixing obvious duplicate spaces. Leave final spacing normalization to Feishu's built-in assistant.
- If an official spelling or intended correction cannot be verified, retain the source text and add it to `待确认项`.

Return the corrected full article first. Then return `待确认项`; write `无` when empty.

## Use the ready-to-copy prompt when requested

```text
请对下面这篇文章做一次发布前 proofreading：

1. 只修正明确错误，不改写文章，不调整结构、段落顺序、标题层级和原意，也不要增删信息。

2. 检查错别字、漏字、重复字、明显病句，以及专有名词、公司名、产品名和英文缩写的大小写。尤其注意统一官方名称，例如 OpenAI 不能写成 openai 或 Openai，OAI 不能写成 oai。无法确认的名称请保留原文并列为待确认项，不要自行猜测。

3. 中文正文统一使用全角标点，尤其不要出现英文半角双引号和逗号。网址、邮箱、代码、公式及文件名中的英文符号不要修改。

4. 尽量不要使用破折号（——）。请根据语义替换成句号或逗号，但不要改变原意。

5. 不要新增加粗。删除 bullet point、单个词或短语的加粗，只保留真正需要强调的完整句子。

6. 不要擅自删除中文与英文、中文与数字之间的空格，这部分会使用飞书「文字排版助手」统一处理。

请先输出修正后的完整文章，再单独列出待确认项；如果没有待确认项，请写“无”。

以下是需要 proofreading 的文章：

【粘贴全文】
```

## Run Feishu formatting normalization

After AI proofreading, run Feishu's built-in `文字排版助手` and select `一键优化` to normalize:

- spacing between Chinese and English or numbers;
- full-width and half-width punctuation covered by the assistant.

This is a Feishu UI capability, not a Lark OpenAPI operation. If a controllable Feishu UI is open and the user authorized end-to-end execution, use the applicable browser or computer-control workflow and verify the assistant reports completion. Otherwise, give the user this exact next step:

`请在飞书中打开「文字排版助手」，点击「一键优化」，统一处理中英文空格和全、半角标点。`

Never claim this step ran unless the UI visibly confirmed it.

## Hand off for layout

- Treat the text after Feishu optimization as the delivery version.
- Ask the author to resolve every `待确认项` before handoff.
- Do not make further piecemeal typo or structural edits after layout starts. If content changes materially, rerun the entire workflow.
- Finish with: `终检完成后，请 @排版负责人开始排版。`
