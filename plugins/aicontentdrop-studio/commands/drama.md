---
description: Write, review, price, render and cut a short vertical drama through Drama Studio, one approved step at a time
argument-hint: <premise; optionally episodes, seconds per episode, genre, style, language and romance ceiling>
---

Make a short drama from: $ARGUMENTS

Use the bundled `drama-studio` and `short-drama-writer` skills and the AI Content Drop connector's tools `list_dramas`, `start_drama`, `get_drama`, `revise_drama`, `review_drama_draft`, `quote_drama_episode`, `produce_drama_episode` and `assemble_drama_episode`. Do not assemble a drama out of `generate_video` calls; the characters would not stay the same between clips.

Sign-in. The drama tools are listed only to a signed-in account; when they are missing, or one answers that it needs an account, sign in through the connector (in Claude Code, `/mcp`; on claude.ai and in Claude Desktop, the plugin's Connectors tab), then call it again. Never ask for or pass an API key.

1. Settle the brief: the premise, episodes (1 to 12), seconds per episode (15, 30, 60 or 90), 9:16 unless asked for 16:9, genre, style, language and the romance ceiling. Say what you assumed. `list_dramas` first when the person may be picking up an earlier drama.
2. Money, the writing. `start_drama` has the whole script written and charges a few credits for it, only when the draft lands; `revise_drama` is a new draft and another writing charge. Say so in words and call either one only after the person says yes to the writing charge. Both return `credits_reserved`, the number that will be charged, and a `write_job_id`. Say `credits_reserved` to the person as soon as it comes back.
3. Poll `get_drama` every 30 seconds until `pending_draft` arrives; stop after 10 minutes and report the last status. Summarise the draft for the person (the hook, the turn, the ending question) before anything is spent on video. Happy: `review_drama_draft` with the `proposal_id` and `decision: "accept"`, which costs nothing. Changes wanted: `revise_drama` with the instruction in words, under the rule in step 2.
4. `quote_drama_episode` (optionally with an `episode_id`). It costs nothing and returns one quote per clip with `clip_id`, `quote_id`, `credits`, `seconds` and `model`, plus `total_credits`; quotes last 10 minutes. If `complete` is false, call it again to finish. Say which clips are in `unavailable` and why.
5. Money, the render. Say the clip count, the episode length and `total_credits` read from the quote, in words, and wait. Call `produce_drama_episode` with the `clips` array of `{clip_id, quote_id}` pairs exactly as the quote returned them only after the person says yes to that number; an unclear answer is not a yes. Each clip is charged only when it renders; a retry with the same quote returns the same render, never a second charge.
6. Poll `get_drama` every 30 seconds until the episode's `clips_ready` equals `clips_total`; stop after 15 minutes and report which clips are still rendering. Then `assemble_drama_episode`, which costs no credits and cuts the episode from the newest render of each clip; the finished video appears as `episodes[].cut.video_url` on `get_drama`.
7. Hand back the episode's `video_url`, the logline, the credits the writing and the clips used, and the drama's page on Drama Studio, <https://aicontentdrop.com/generate/drama>; the video link lasts about an hour, and `get_drama` gives a fresh one. On `ENTITLEMENT_REQUIRED`, give the `info_url` the refusal carries (https://aicontentdrop.com/plans) and nothing else about plans.
