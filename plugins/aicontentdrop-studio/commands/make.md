---
description: Quote, then make one video or image through the connector, and poll it to the finished file
argument-hint: <what to make; optionally the model id, duration, resolution, and a public image URL to animate>
---

Make: $ARGUMENTS

Use the bundled `ai-video-model-picker`, `video-prompt-writer` and `image-generation` skills and the AI Content Drop connector's tools `list_models`, `estimate_credit_cost`, `generate_video`, `generate_image` and `get_generation`.

Sign-in. `list_models` and `estimate_credit_cost` need no account. `generate_video`, `generate_image` and `get_generation` do: when one answers that it needs an account, sign in through the connector (in Claude Code, `/mcp`; on claude.ai and in Claude Desktop, the plugin's Connectors tab), then call it again. Never ask for or pass an API key.

1. Decide video or image, and text-to-video or image-to-video: a model whose `list_models` entry has `needs_start_image` takes an `image_url`. Pick the model from `list_models`, never from memory. For a draft, use the cheapest model that can express the idea and keep the best model for the one that ships.
2. Write the prompt in the skill's order: subject, setting, light, lens, treatment. One shot, one camera move, and what is there rather than what is absent.
3. `estimate_credit_cost` with the exact `model_id`, `duration` and `resolution` you will send. It returns `credits_total` and a `quote_id` valid for 15 minutes.
4. Money. Say the model and the number read from the quote, in words, and wait. Call `generate_video` or `generate_image` with the prompt and that `quote_id` only after the person says yes to that number; an unclear answer is not a yes. A quote that lapsed or does not match the job is refused (`QUOTE_EXPIRED`, `QUOTE_MISMATCH`): quote again, and ask again only if the number changed. Credits are charged only when the generation succeeds; a refusal at submit (`UNSAFE_CONTENT`) creates no job, and a `failed` or `timeout` job charges nothing.
5. Poll `get_generation` with the returned id (and `kind: "image"` for an image) every 5 seconds until the status is `completed`, `failed` or `timeout`. Never start a second job because the first is slow; stop after 15 minutes and hand back the id.
6. Hand back the file URL, the model, `credits_used`, and the prompt as sent. Plan questions go to https://aicontentdrop.com/plans and nowhere else.
