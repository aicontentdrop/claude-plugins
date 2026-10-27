---
description: Search the ads competitors run, keep the ones worth studying, and decode a winner into a brief after a quote
argument-hint: <product, category or competitor; optionally the platform (meta or tiktok)>
---

Research competitor ads for: $ARGUMENTS

Use the bundled `competitor-ad-research` skill and the AI Content Drop connector's tools `search_competitor_ads`, `save_competitor_ad`, `list_saved_ads`, `get_saved_ad`, `estimate_ad_cost`, `decode_ad` and `ad_to_canvas_card`.

Sign-in. Every research tool except `estimate_ad_cost` needs an account, because saved ads are account data. When a tool answers that it needs an account, sign in through the connector (in Claude Code, `/mcp`; on claude.ai and in Claude Desktop, the plugin's Connectors tab), then call it again. Never ask for or pass an API key.

1. `search_competitor_ads` with the `keyword`, the `platform` (`meta` by default, or `tiktok`), `sort` (`impressions` for reach, `recent` for start date) and a `limit`. Rank the rows by how long each ad has run and say why. When the result says `demo: true`, tell the person the rows are samples; do not rank them, save them, decode them or build a brief on them.
2. `save_competitor_ad` with one row copied as returned, for each ad worth studying. `list_saved_ads` shows what the account has kept and which ads already carry a decode; `get_saved_ad` reads one in full with its stored decode and never runs a new one. Check it before decoding, because a decoded ad costs nothing to read again.
3. Money. A decode turns an ad into its formula: hook, script, scenes, audio, call to action, style and a recreation prompt. `estimate_ad_cost` with `kind: "decode"` returns a flat price and a `quote_id` valid for 15 minutes. Say the number read from the quote, in words, and wait. Call `decode_ad` with the `ad_id` and that `quote_id` only after the person says yes to that number; an unclear answer is not a yes. Credits are charged only when the analysis succeeds; a retry with the same quote returns the first result, never a second charge.
4. To remake it, `ad_to_canvas_card` with the `ad_id` and a `canvas_id` from `list_canvases` or `create_canvas` places the ad and its decoded brief on a board. From there: one finished ad through `/aicontentdrop-studio:ad`, variations through `/aicontentdrop-studio:board`, or the whole job through `/aicontentdrop-studio:drop`. Remake the formula with the person's own product and brand: no competitor footage, logo, product, script lines or on-screen people.
5. Hand back the ads ranked with advertiser, hook and run time, each decoded formula in a few lines with the credits used, and the board link if you placed them on one. Plan questions go to https://aicontentdrop.com/plans and nowhere else.
