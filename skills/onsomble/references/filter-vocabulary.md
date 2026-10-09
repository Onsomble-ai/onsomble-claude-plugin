# Filter vocabulary

## Platforms

The `platform` filter takes an array of stable IDs. Each ID names one AI product **and** how Onsomble collected the answer: `_app` means the public web product, `_api` means the provider's API. Omit the filter to include every platform. `get_filter_options` returns the platforms actually present in saved evidence; historical platforms remain valid read filters even when unavailable for a new Scan. Use the returned IDs rather than treating the examples below as the complete current list.

| ID                        | Product              | Collection |
| ------------------------- | -------------------- | ---------- |
| `chatgpt_app`             | ChatGPT              | web        |
| `chatgpt_api`             | ChatGPT              | api        |
| `claude_api`              | Claude               | api        |
| `gemini_app`              | Gemini               | web        |
| `gemini_api`              | Gemini               | api        |
| `google_ai_summaries_app` | Google AI Summaries  | web        |
| `google_ai_mode_app`      | Google AI Mode       | web        |
| `microsoft_copilot_app`   | Microsoft Copilot    | web        |
| `perplexity_app`          | Perplexity           | web        |
| `perplexity_sonar_pro`    | Perplexity Sonar Pro | api        |

Web and API collections of the same product can answer differently — that difference is itself a finding worth reporting.

## Regions

The `region` filter takes an array of region codes, e.g. `["NZ:auckland"]`. A Site only has data for the regions its Scans measured; an unmeasured region returns empty results, not zeros. Region is a filter on every report — there is no separate region tool. For a region breakdown, filter any report by `region`; for a region trend, use `get_visibility_timeline` with `groupBy` region. Discover a Site's region codes with `get_filter_options`.

## Prompt classification filters

Every report tool accepts the dashboard's prompt vocabulary, all optional and repeatable. Each resolves to the set of prompts it matches; the report is then computed over that slice:

- `promptId`: limit to specific prompts (their promptIds).
- `tag`: prompts carrying these tags.
- `category`: prompts in these categories.
- `product`: prompts about these products.
- `persona`: prompts targeting these personas.
- `stage`: prompts in these journey stages, including custom stages. Use the IDs returned by `get_filter_options`; stages are not limited to the built-in list.
- `brandMention`: `mentions_tracked_brand` or `does_not_mention_tracked_brand` — prompts that do, or do not, mention the tracked brand.

The metric reports (`get_visibility_overview`, `get_visibility_timeline`) additionally take `brand`: brand identity ids that limit which brands are returned. It never changes a Share of Voice denominator — filtering to one brand does not inflate its SoV to 100%.

Use `get_filter_options` for Site/Topic report vocabulary. Its nine `dimensions` groups are `tags`, `categories`, `products`, `personas` (customer groups), `journeyStages`, `regions`, `models` (AI platforms), `brands` (primary and competitor-linked identities), and `brandMentions` (primary-brand mentions in prompt text). Request one or more groups, e.g. `dimensions:["tags"]` or `["personas","journeyStages"]`; omission browses all nine. Results are fixed pages of up to 20 `{dimension,option}` records, with whole-scope `total`, `totalsByDimension` and `nextCursor`. Follow every cursor with the same `siteId`, `scanId` and dimensions; the order of the selected dimension names does not matter. There is no `limit` argument. Restart after classifications, roster or evidence repairs change the vocabulary; new Scans do not extend a continuation.

Map groups to the supported report parameters: tags → `tag`, categories → `category`, products → `product`, personas → `persona`, journeyStages → `stage`, regions → `region`, models → `platform`, brands → `brand`, brandMentions → `brandMention`. Inspect the target tool schema: each report supports its own subset. Brand identity IDs are not competitor row IDs.

Pass `scanId` to narrow measured values to one eligible completed Scan; omit it for all eligible completed history. Classifications use current metadata on measured prompts, including retired prompts. Brands reflect the current primary/competitor-linked roster (linked identities with tracking off are included; discovered-only identities are excluded). Brand-mention tokens are fixed grammar. Those two groups can exist without Scan evidence: check `scanCount`, and do not treat them as proof of measured results. Use `get_scan_config` for saved definitions/new-Scan model choices, and `list_prompts` for question IDs and assignments. Historical models returned here are accepted by read filters; execution eligibility is separate. Region vocabulary keeps the existing normalization behavior.

## Dates

`startDate` and `endDate` are inclusive `YYYY-MM-DD` bounds on Scan dates. They apply to the history-shaped tools (`get_visibility_overview`, `get_visibility_timeline`, `get_recommendations`). `get_prompt_results`, `get_references`, `get_reference_domain`, `get_narratives`, and `get_narrative_claims` scope by Scan instead: pass `scanId` (resolve Scan ids for a period from paginated `list_scans`). References accepts an array of Scan IDs; narratives and answers use one selected Scan. `list_competitors` and `list_discovered_competitors` are catalogues keyed by a stable `competitorId`; pass a tracked `competitorId` to the metric tools to read that competitor's numbers.

## Timeline parameters

`get_visibility_timeline` additionally takes:

- `metric`: `visibility` | `shareOfVoice` | `sentiment` | `gap` (default `visibility`)
- `groupBy`: `brand` | `platform` | `region` (default `brand`) — one series per group, returned as paginated flat observations. Repeated keys across pages belong to the same series.

`brandId` selects the single entity for platform/region timelines; Topics require it. `groupBy=brand` rejects it: use the `brand` or tracked `competitorId` arrays instead. Model breakdown uses an explicit `brandId` or the Site primary / Scan-saved Topic focal entity. Neutral or legacy Topics with no saved focal entity require explicit selection. Model breakdown has no date window or pagination; region and classification filters affect the measured values and an empty slice does not select an older Scan.

Overview competitors are fixed at 20 per page. Scorecard/timeline are fixed at 20 actual metric records per page, not 20 Scans or series with unlimited nested rows. `total` is the complete pinned matching scope; follow `nextCursor` with unchanged filters. Complete chronology uses exact Scan completion timestamps; date bounds use source metric days. A later completed Scan does not extend continuation; historical repairs and current label/metadata edits require restarting.

## The consistency rule

Two numbers are only comparable when they were computed over the same filter scope. Before comparing a brand against a competitor, a region against a region, or this month against last month, make the calls with identical `platform`, `region`, and date filters. Every report response echoes the applied filters — check that echo before presenting a comparison.

## One answer series across Scans

`get_prompt_result_trends` takes `siteId`, `resultId` from `get_prompt_results` and optional `cursor`. It follows the exact saved question/model/region series; public model aliases do not combine distinct stored answer series. It takes no extra platform/region/classification/date selectors or limit. Read twenty flat observations at a time, oldest exact Scan completion first. Each observation carries date, scanId, completedAt, saved nullable Topic context and one complete brand metric. A Scan can span pages: group repeated scanId. `total` counts observations, `totalScans` counts fact-backed included Scans, and metric=null preserves a fact-backed Scan without displayed brands. Share of Voice keeps the full denominator, including undisplayed discovered entities. Follow nextCursor with unchanged IDs; later Scans do not extend continuation. Restart after historical evidence or anchor edits. Full collected answer text is in `get_prompt_results`.
