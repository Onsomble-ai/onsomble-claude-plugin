# First setup

A user connected Onsomble to their AI tool and asks "I just connected — what now?". `list_sites_and_topics` returns no items across all pages, ending with `nextCursor: null`: no accessible subjects. Read `get_account` before concluding that setup is needed. In this example the account is empty. Establish business-or-agency first, set the Site up **with** the user, and confirm changes unless already authorized by their request.

---

**You:** Welcome. Onsomble tracks how AI assistants talk about a brand — each brand you track is a "Site". Before we set your first one up: are you a business tracking your own brand, or an agency tracking brands for clients?

**User:** Just us — one business.

**You:** Great. What's your website?

**User:** getcedar.com

**You:** _[researches the website using the assistant’s own research capabilities]_ Here's what I found, in plain terms:

- **Cedar** — expense and spend-management software for small finance teams.
- Main competitors it looks like you'd care about: Ramp, Brex, Divvy.
- Primary market: United States.

Does that look right, or is anything off?

**User:** Close, but we're mid-market now, not small teams.

**You:** Noted — I'll set the audience as mid-market finance teams. One thing before I create the Site: it takes one Site slot on your account. Want me to go ahead?

**User:** Yes.

**You:** _[calls `create_site` with the URL and corrected businessProfile, then `get_scan_config` to inspect the current definitions]_ Cedar is set up. It has your profile and the default AI models, but no prompts yet, so a scan can't run until we add some. What should this scan help you achieve?

**User:** Understand whether buyers discover Cedar when looking for mid-market expense software.

**You:** _[calls `update_site` with the saved `siteId` and `strategicGoal: {objective: "Understand whether buyers discover Cedar when looking for mid-market expense software."}`]_ I'll use that objective to guide these questions.

I'd start by tracking a few buyer questions your customers would actually ask an AI assistant. Based on your profile:

- "best expense management software for mid-market" — a category question where you'd want to appear.
- "Ramp vs Cedar" — a head-to-head you'll want to watch.
- "how to control company spend" — a problem-first question.

I'll group these under the audience you identified, "Mid-market finance teams", and use any relevant offerings and journey stages already configured. Want these three, or would you change them?

**User:** Those three are good.

**You:** _[reuses the matching persona from `get_scan_config`; if missing, calls `create_persona` with `name: "Mid-market finance teams"`. Takes the returned `item.id` and includes it in `personas` on each `create_prompt` call, alongside the question's `text` and `category`. Includes relevant existing `products`/`journeyStages` where applicable; checks returned prompt IDs/names.]_ Added the three questions for mid-market finance teams. Running a first scan measures across the AI platforms and uses one scan from your plan's allowance — shall I start it?

For one of these questions, the creation call has this shape (the IDs are illustrative; actual IDs must come from tool responses):

```json
{
  "siteId": "dddddddd-0000-4000-8000-000000000001",
  "operation": "add",
  "text": "What is the best expense management software for a mid-market finance team?",
  "category": "recommendation",
  "personas": ["mid-market-finance-teams"]
}
```

Assignment groups the question for audience reporting. Its text expresses the audience perspective; the association does not instruct the scanned AI to impersonate the persona.

**User:** Go for it.

**You:** _[before asking to run, has shown the complete saved setup: the goal, three questions and audience assignments, tracked competitors, regions and enabled models. Now calls `trigger_scan` after the user's explicit confirmation]_ Your first scan is running. It takes a little while — ask me for the results in a few minutes and I'll show you where Cedar stands and what to fix first.

---

Notes on the shape: read the account and existing configuration first. Let the user correct the researched profile. Define a missing audience before assigning it, and save assignments during question creation. Relevant definitions can be reused without inventing extra categories. Confirm configuration changes unless already authorized. Show the complete setup, including questions, audience assignments, tracked entities, regions and models, and explain Scan allowance use. You must explicitly confirm with the user before starting the scan. For an agency, use `update_account` to set the type while the account is empty, then resolve existing Clients with `list_clients`/`get_client`, use `create_client` only when a new Client is needed, and call `create_site` with the Client ID and the assistant-researched profile. See `../references/semantic-model.md` for preservation/clearing and neutral-question cases.
