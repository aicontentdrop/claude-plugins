---
description: Create or open a board, add text, media and generation cards, and render cards one quoted step at a time
argument-hint: <board name, and what to put on it>
---

Plan on a board: $ARGUMENTS

Use the bundled `canvas-board` skill and the AI Content Drop connector's tools `list_canvases`, `create_canvas`, `get_canvas`, `get_canvas_link`, `rename_canvas`, `add_text_card`, `add_media_card`, `add_frame`, `add_generation_card`, `connect_cards`, `group_cards`, `update_card`, `refresh_canvas`, `estimate_credit_cost` and `run_generation_card`.

Sign-in. Every board tool needs an account, because a board is account data. When a tool answers that it needs an account, sign in through the connector (in Claude Code, `/mcp`; on claude.ai and in Claude Desktop, the plugin's Connectors tab), then call it again. Never ask for or pass an API key.

1. `list_canvases` first. Reuse the board whose name matches the task; otherwise `create_canvas` with a short name the person would recognise. Never make a second board for a task that already has one.
2. Add cards without `position`; the server places them. `add_text_card` for an idea, a hook or a script line; `add_media_card` for a public `https` image or video (an image that must stay available on the account can go through `import_asset` first; a video is added by its public URL); `add_generation_card` for a full prompt and a model id from `list_models`; `add_frame` or `group_cards` to label a region. A step that builds on an image card takes `from_card_id`, and `connect_cards` links any other two. Leave `revision` out of every call, and read the next step from the board each tool returns.
3. Nothing renders until `run_generation_card`, the only board tool that spends. For each card, `estimate_credit_cost` with `model_id` set to the card's `model`, and the card's `duration` and `resolution`, `quantity: 1`; the quote is valid for 15 minutes.
4. Money. Before the first render, say the card and its number read from the quote, in words, and ask how the rest of the task should be approved. Confirming each card is the default and the reading of any unclear answer. Before asking, quote every card (step 3) and say each card's number and the total, in words. Only when the person has said clearly, to that total, that the whole task may proceed without further questions do you run those quoted cards without asking again, and you report every charge afterwards. A card added or re-quoted after that yes is asked about on its own. Call `run_generation_card` with the card id and its `quote_id` only after the person says yes to that number. Credits are charged only when the render succeeds.
5. `refresh_canvas` every 10 seconds while any card is `generating`. Report a card when its status changes, not every poll. Stop after 15 minutes and say which cards were still rendering; never run a card a second time because the first run is slow.
6. Hand back the link from `get_canvas_link`, the cards you added by title, the rendered cards with their `credits_used`, and any card still rendering with its job id. Plan questions go to https://aicontentdrop.com/plans and nowhere else.
