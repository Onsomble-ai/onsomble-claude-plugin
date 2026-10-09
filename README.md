# Onsomble for Claude

Track how AI assistants talk about your brand and market, and learn how to show up better.

Onsomble is an AI marketing research and analytics platform. It asks AI assistants such as ChatGPT, Claude, Gemini and Perplexity the questions your customers ask, then measures how often your brand is mentioned or recommended, how it is described, which competitors appear instead, and which sources the answers cite.

This plugin connects Claude to your Onsomble account. Once it is installed, you can ask Claude to set up research for a brand or topic, run a Scan, check its progress, and explain the results, all in conversation.

## What you need

An Onsomble account. You sign in to Onsomble the first time Claude connects. A new account can start from scratch in Claude: create a brand site or research topic, set up its questions, and run a Scan. Results appear once the first Scan completes.

## What you can ask

- "Set up research for my brand."
- "How visible is our brand in AI answers, and how do we compare with our competitors?"
- "What are the main narratives in AI answers about our brand? Show me the original answer behind one."
- "Find ways to get my brand recommended more often by AI."

## What the plugin contains

- **The Onsomble connector.** A connection to Onsomble's server at `https://mcp.onsomble.ai/mcp`. Claude uses it to read your Onsomble research and, when you ask, to change your saved setup, manage recommendations, or start a Scan.
- **The Onsomble skill.** Instructions that teach Claude how Onsomble's data fits together, how to read the metrics accurately, and which steps to follow for common research tasks.

The plugin contains no programs or scripts. It runs nothing on your computer.

## How your data is handled

Claude sends requests only to Onsomble's server at `mcp.onsomble.ai`, and only after you sign in to Onsomble and approve access. Each request carries the inputs for one action, such as which site to read or which question to add. Your Claude conversation itself is not sent to Onsomble.

Onsomble stores your research setup and results in your Onsomble account. Starting a Scan uses your account's Scan allowance, so Claude asks you to confirm first. The SEO tools return search and backlink data that Onsomble licenses from DataForSEO; Claude still talks only to Onsomble.

Read Onsomble's [privacy policy](https://www.onsomble.ai/privacy) and [terms of service](https://www.onsomble.ai/terms).

## Help

- Setup guide: [Use Onsomble with Claude](https://docs.onsomble.ai/developers/connect/claude)
- Support: [Contact Onsomble](https://www.onsomble.ai/company/contact)

## License

MIT. See [LICENSE](LICENSE).
