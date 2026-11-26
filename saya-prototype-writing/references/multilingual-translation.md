# Saya multilingual translation

Use this workflow only when the user explicitly requests Saya multilingual copy, localization, or a translation-item table.

## Direct-use workflow

The user can supply only the copy to translate. Translate it, generate missing item IDs, review the result, and deliver the backend-upload XLSX without repeatedly asking about language sets, English source text, IDs, headers, or country codes.

- Use the user's final copy and any supplied context. For Chinese-only input, draft the English baseline directly and reuse established Saya terminology; a routine English translation does not need a separate approval round.
- Preserve user-supplied item IDs and placeholders exactly. If the user supplies no ID, generate one using the rule below.
- Infer user-facing versus backend copy from the supplied context. Ask a focused question only if the intended meaning, a new product term, or an ambiguous placeholder materially changes the translation. Do not invent benefit rules or resolve a genuinely undecided product name without confirmation.
- If the user asks only to name, extract, compare, or preview copy, return the requested text first. Otherwise, translation requests default to an upload-template XLSX, not merely a chat table.

## Language modes

Use one of the following two sets in its exact order. An explicit custom list overrides both presets.

### Concise set: 11 codes (default)

For requests with no language selection, or requests for “11 种语言／精简版”, use:

`default, en, ar, ur, hi, bn, tr, id, fil, ru, uk`

This is 11 backend codes including the English fallback row, not 11 distinct languages. Do not add `cn` or other languages to this set without a request.

### Full set: 28 codes

For “全部语言／全语种／完整版”, use:

`default, en, cn, be, de, it, kk, fa, pl, ro, th, uk, uz, vi, tr, pt, fr, ur, hi, bn, ar, ru, fil, ka, ms, id, es, jv`

“全部语言” refers to these 28 supported backend codes, not every language worldwide.

### Shared language and country rules

- `default` and `en` contain exactly the same English text.
- When selected, `cn` contains Chinese. Saya uses `cn`, never `zh`; Chinese is included in the full set but not the concise set.
- Every other row contains its corresponding localized copy. `hi` is Hindi, `bn` Bengali, `id` Indonesian, and `fil` Filipino.
- Do not substitute country codes `IN`, `BD`, or `PH` for those language codes. Legacy `in` may mean Indonesian in some systems; do not interpret it as Hindi or silently mix it with `id`. If a future user supplies ambiguous internal codes, clarify their mapping.
- Set the country code to `default` on every row unless the user explicitly specifies another country rule.

## Translation-item IDs

- A translation-item ID identifies one source item. All language rows for that item share the same ID. Distinct items in a batch have distinct IDs.
- Preserve any supplied ID. Reuse the original ID for later revisions to the same item; do not regenerate it because its wording changes.
- For a new item with no supplied ID, use a descriptive English slug plus a numeric suffix: lowercase ASCII letters/digits, underscores between words, then `_001`, `_002`, etc. Remove punctuation; retain meaningful fixed numbers.
- Example: `成长等级勋章 Level 1` → `growth_level_badge_level_1_001`.
- Check against IDs in the supplied files, available prior outputs, and the current batch; increment the suffix for collisions with different items. Do not claim global backend uniqueness if that registry was not available, and do not access a live backend merely to generate IDs.
- Keep IDs as text. Do not request an ID from the user as a prerequisite to translating or exporting.

## Translation standard

- Preserve the confirmed meaning, entitlement, condition, and tone. Do not add claims, benefits, limitations, or actions.
- Prefer concise, natural in-app language over literal translation.
- Translate product terms for the live-room or voice-room context. Do not turn `Room ID` into a physical room number or imply that a premium identifier is a paid subscription.
- Keep the same product concept consistent across titles, bodies, and translation items.
- Translate validity-field values such as `永久` with the locally natural equivalent of permanent or indefinite, not a conversational equivalent of “forever.”
- Preserve placeholders such as `{room_id}`, `{item_name}`, and `{expiration_date}` exactly. Do not translate them, change their case, add spaces inside the braces, or replace the brace style. Local punctuation and word order may change around them.
- When the user specifies `NUM` for fillable content, keep the literal uppercase `NUM` in every translation, without braces, translation, or substitution. Preserve the occurrence count per item. Existing named placeholders keep their supplied format unless the user explicitly asks to convert them.
- Distinguish placeholders from fixed content: `Level 1` stays level 1; do not automatically replace its fixed number with `NUM`. Translate surrounding units as appropriate without turning fixed product rules into parameters.

## Independent review

After drafting the complete translation set, use an independent AI review when delegation is available and authorized. Ask it to check:

- semantic accuracy and missing conditions;
- natural local usage and brevity;
- grammar, gender agreement, and ambiguous pronouns;
- product-context accuracy;
- placeholder integrity and placement.

Review the proposed corrections before applying them. Do not accept a suggestion that changes confirmed product meaning or terminology. Do not add a review sheet to the user deliverable unless requested.

If independent review is unavailable or not authorized, perform a separate self-check and do not claim an independent AI review occurred. Obtain any required delegation authorization rather than bypassing it.

## Spreadsheet output

For a translation deliverable, create one XLSX worksheet with these exact four columns in this order, unless the user explicitly asks only for text or another format:

| 翻译项 | 语言码 | 翻译文字 | 国家码 |
| --- | --- | --- | --- |

- Group rows by translation-item ID.
- Within every item, use the exact order of the selected language mode or explicit custom list.
- Store translation-item IDs as text.
- Keep one row for every item-language pair and do not add explanation or review tabs.
- Apply a filterable header, freeze the first row, wrap long copy, and keep all four columns readable.
- Do not add Chinese reference rows to the concise set, extra metadata columns, explanatory title rows, review tabs, or merged cells to the upload worksheet.
- Deliver the actual downloadable XLSX with a concise note naming the selected mode. A chat preview does not replace the requested file. Generating an upload-ready file does not authorize uploading, publishing, or changing backend records.

If the user asks only to review wording or confirm terminology, return the requested text or comparison first and do not create the XLSX prematurely.

## Final validation

Before delivery, verify:

- data rows equal `translation-item count × selected code count` (11 for the concise set, 28 for the full set, or the explicit custom count), excluding the header;
- every item contains every requested language exactly once, with no blank or duplicate rows;
- each item uses one stable text ID, IDs are distinct across different items, and no supplied ID was changed;
- `default` and `en` exactly match the English baseline;
- when selected, `cn` exactly matches the Chinese source and no `zh` row exists; when not selected, no extra `cn` row is added;
- every country code is `default`, unless the user supplied another rule;
- each translated body contains the complete, unchanged placeholder set required by its source;
- placeholder occurrence counts, including literal `NUM`, are preserved and fixed numbers were not accidentally parameterized;
- the exported workbook can be reopened and the rendered sheet has no clipped or missing content.
