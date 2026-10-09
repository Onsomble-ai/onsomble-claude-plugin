# Sample analysis

A user asked: "How is Acme doing in AI search this month?" The agent ran the visibility-audit workflow (overview → timeline → prompt results → recommendations). This is the expected output shape — headline with delta first, every claim attributed to evidence, one prioritised action, sample sizes and caveats stated inline.

---

**Acme's AI visibility is 62% (+6 points since the 1 July Scan)** — AI assistants mentioned Acme in 62% of the 40 measured responses. Share of Voice is 41%, Sentiment 62 (mildly positive; 50 is neutral), Health 55.

**What's driving it**

- The gain is a trend, not a blip: visibility has risen across the last three Scans (48 → 56 → 62 on the timeline).
- The gain is concentrated on web platforms. ChatGPT on the web now mentions Acme in 8 of 10 measured responses, up from 5, while ChatGPT through its API is flat at 4 of 10 — worth knowing that the two collections answer differently.
- Sentiment held at 62 but rests on 11 classified statements, so treat single-point moves cautiously. The strongest quoted statement: _"Acme is a solid choice for small teams"_ (ChatGPT, "best tools for small teams" prompt).
- BetterCorp remains the strongest competitor at 71% visibility. On the two comparison prompts Acme loses, responses cite reviewsite.example — a domain that never mentions Acme (a silent source).

**Recommended next step** (top item from Onsomble's recommendations): publish a comparison page targeting the "Acme vs BetterCorp" prompt cluster, where Acme is currently absent from 4 of 5 responses.

---

Notes on the shape: the headline states the metric, the delta in points, and the sample size. Each finding names its evidence (a timeline, a platform split, a quoted statement, a cited domain). The close is Onsomble's prioritised recommendation, not a generic suggestion. Nothing claims market share, customer numbers, or causation.
