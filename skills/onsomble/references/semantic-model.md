# Semantic model

The MCP resource `onsomble://model` exposes the domain reference as JSON. Use tools to obtain actual account data and IDs; the reference is static.

| Concept               | Meaning and relationship                                                                                                                                                                                        |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Account / Client      | Account owns membership, billing and access. An agency Client groups brand Sites and can optionally be associated with topics; Client association is independent of topic focus.                                |
| Subject / `siteId`    | A brand Site or research Topic, distinguished by `subjectKind`. Topics have a title and scope and do not require a website or business profile.                                                                 |
| Archived subject / Client | A Site, Topic or Client with `archivedAt` set. `archive_site`, `archive_topic` and `archive_client` retire items while preserving saved detail and reports. Client archive includes all linked Sites and Topics. Active lists exclude archived items, later writes are refused, and there is no MCP restore. |
| Working configuration | Mutable setup read by `get_scan_config` without `scanId`: questions, strategic goal, reusable classification definitions, tracked entities, models, regions and schedule. Saving it does not start a Scan.      |
| Strategic goal        | Optional `{objective: string}` context. `create_site`/`create_topic` save the initial note; creation recovery preserves a saved note. `update_site`/`update_topic` save/replace it; null clears on update. `get_site`/`get_topic`/`get_scan_config` read it. `get_scan_goal_history` shows saved edits. Saving it does not generate questions or change scoring.                  |
| Persona               | Reusable audience segment. Prompts and personas have a many-to-many relationship. Defining it does not assign it; assignment does not instruct the scanned AI to impersonate it.                                |
| Product               | Strategy product/service reference, separate from profile product text and topic tracked entities whose type happens to be product.                                                                             |
| Journey stage         | Customer journey position; separate from question category. First supplied stage is primary.                                                                                                                    |
| Prompt                | Stored question with ID, text, category and optional classification references. Distinct from an MCP protocol prompt template.                                                                                  |
| Category / tags       | Category is `comparison`, `recommendation`, `specific` or `general`. Tags are free-form labels. Neither automatically assigns an audience, offering or stage.                                                   |
| Tracked entity        | Competitor for a brand Site, or person/organisation/group/brand/product in a topic. Stable `entityId` identifies it; it is separate from a persona.                                                             |
| Scan / result         | A Scan freezes the measurement setup; later working edits do not rewrite it. A result measures a question on a platform in a region. Multiple personas group the same evidence, without multiplying executions. |

## Define, assign, verify

1. Read `get_scan_config` without `scanId` for current `personas`, `products` and `journeyStages` definitions.
2. Reuse matching definitions. If the user-requested audience is missing, call `create_persona` with its name within authorized setup work; use returned `item.id`. Products/stages follow the same pattern with `create_product` and `create_journey_stage`.
3. Include the persona in `create_prompt.personas` in the same call that creates the audience-specific question. Also include relevant configured offerings/stages where applicable.
4. Check the returned `prompt` assignments (IDs and names), or read `list_prompts`.

For example, suppose a current configuration or persona creation result actually returned `{id: "mid-market-finance-teams", name: "Mid-market finance teams"}`. With the real Site ID supplied by `list_sites_and_topics`, call:

```json
{
  "siteId": "dddddddd-0000-4000-8000-000000000001",
  "operation": "add",
  "text": "Which expense platform suits a mid-market finance team?",
  "category": "recommendation",
  "personas": ["mid-market-finance-teams"]
}
```

The UUID and persona ID above are illustrative fixtures; replace them with actual returned values. Question text expresses the audience perspective; assignment supports coverage and reporting. Check that the response's `prompt.personas` includes the saved ID/name.

Keep audience-neutral questions unassigned where appropriate. If several audiences plausibly match, clarify the mapping. Topics do not require products or purchase stages.

## Field and time semantics

- Write arrays `personas`, `products` and `journeyStages` accept configured IDs or names; prefer IDs. Omission on creation leaves that dimension unassigned. On update or reactivation, omission preserves current valid assignments (reactivation prunes deleted references and reports cleanup); supplied arrays replace that dimension, `[]` or null clears it. Creation never restores a retired question automatically. A match returns retiredMatches with no write: choose reactivate_prompt for its existing identity/history, or repeat create_prompt with all returned IDs in acknowledgedRetiredPromptIds to explicitly create a fresh identity. Reactivation prunes obsolete assignment references and reports cleanup. Matching ignores case, repeated whitespace and trailing .?!; no fuzzy matching.
- Missing assignments prevent matching filters for the missing dimension and placement in its corresponding matrix cells. The question remains accessible in the catalogue and other assignments remain valid.
- Report filters `persona`, `product` and `stage` select evidence. Obtain their tokens from `get_filter_options`; write IDs are not assumed to be report tokens. `onsomble://filters` supplies shared platform and metric vocabulary.
- Model selection IDs come from `get_scan_config`; execution region refs come from `list_regions`. Report filters select already collected evidence and do not configure future runs.
- `get_scan_config` with `scanId` reads that Scan's saved setup and attribution. Use the working configuration for new edits and the saved setup to explain historical measurement.

- Definition edits return complete saved `scanConfig` and public `item` (null after removal). Product/persona aliases and journey-stage descriptions are included in current and historical reads. Retired prompt rows and saved snapshots are not rewritten by definition edits; reactivation validates current assignments.
- Regions are optional. Use `list_regions` with search/countryCode/kind and fixed pages of 50. `add_scan_region`/`remove_scan_region` edit defaults, while `update_prompt.regions` edits one active prompt's override. Omission preserves; null/[] clears the override, after which it inherits defaults and then Global if defaults are empty. Removing a default leaves explicit overrides intact.

Narrative and claim evidence reads (`get_narratives`, `get_narrative_claims`) use fixed twenty-record pages, while collected answers (`get_prompt_results`) use five by default and at most ten. All retain the selected Scan across continuation, with whole-scope total and nextCursor. Current saved classifications include retired prompts; restart after membership edits or evidence repair. scanDate is the stored UTC calendar day or null and scanCompletedAt is the finish time. subjectContext is read from the Scan's saved snapshot, with null for missing legacy context. Claim sourceResultId identifies its source answer; claimId identifies evidence itself. Complete quoted text remains evidence to analyse.
