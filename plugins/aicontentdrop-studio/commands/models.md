---
description: List the AI video and image models with their credit costs and quote a job, without making anything
argument-hint: <optionally a brief, a model id, a duration and resolution, or a credit ceiling>
---

Models and quotes for: $ARGUMENTS

Use the bundled `ai-video-model-picker` and `credits-and-plans` skills and the AI Content Drop connector's tools `list_models` and `estimate_credit_cost`. This command is read-only: it makes nothing, charges nothing and signs nobody in. Both tools answer without an account, so it never needs a sign-in; should one ever answer that it needs an account, sign in through the connector (in Claude Code, `/mcp`; on claude.ai and in Claude Desktop, the plugin's Connectors tab) rather than passing a key. Do not call any other tool.

1. `list_models` for the live catalogue (filter by `type` video or image, and by a credit ceiling when the person gave one). Read ids, names and credits from the answer, never from memory. A model that prices by the second is listed at its default length.
2. When the person described a job, recommend one model and name the runner-up: native audio or dialogue, and whether a reference image must be preserved (image-to-video, where the entry has `needs_start_image`), decide it.
3. `estimate_credit_cost` with the `model_id`, the `duration` and `resolution` you recommend, and the `quantity`. Report `credits_each` and `credits_total` from the quote. If the person gave a balance, state balance, cost and shortfall as three numbers, and name the cheaper model that fits.
4. Say that credits are charged only when a generation succeeds, and that the quote's `quote_id` is what `/aicontentdrop-studio:make` will use if the person decides to go ahead. Do not go ahead here. Plan questions go to https://aicontentdrop.com/plans and nowhere else.
