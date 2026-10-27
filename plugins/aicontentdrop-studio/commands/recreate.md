---
description: Make a video in the manner of a reference clip or image through Drop, after the rights question and a free read of the reference into words
argument-hint: <public https URL of the reference clip or image, and the goal in one sentence>
---

Recreate a reference for: $ARGUMENTS

Use the bundled `recreate-reference` skill and the AI Content Drop connector's tools `analyze_reference`, `start_video_agent`, `message_video_agent`, `get_video_agent_run` and `approve_video_agent_spend`.

Sign-in. These tools are listed only to a signed-in account. When a tool answers that it needs an account, sign in through the connector (in Claude Code, `/mcp`; on claude.ai and in Claude Desktop, the plugin's Connectors tab), then call the tool again. Never ask for or pass an API key.

1. The rights question comes first. Ask who holds the rights to the reference and pass the answer exactly as `rights_basis`: `own_work`, `licensed` or `permission`. There is no default. Never choose a basis, never infer it from a filename, and never retry a refused read with another basis. A `rights_required` refusal (read from the refusal's `details.reason`) means ask the person; if they cannot answer, make the video from a written brief instead.
2. `analyze_reference` with the `url` and the `rights_basis`. It costs no credits and is limited per day, so call it once per reference. Relay the `reference_class` with its confidence and the facts (summary, shots, cast, look, lines) as what the agent saw, and let the person correct the class. If `status` is `analyzing`, say the reading is still running and start the drop with the `reference_url`; the facts arrive on the run. If it answers `vision_unavailable` (in `details.reason`), say so and run a plain drop from a written brief; do not describe the reference yourself and pass that off as a read.
3. `start_video_agent` with the goal, `reference_url` exactly as the read returned it, and `reference_class` when the person confirmed or corrected it. Starting charges nothing. Relay the drop's questions and send the answers with `message_video_agent`; do not answer for the person.
4. Poll `get_video_agent_run` every 15 seconds. Report a change of phase, a finished piece or a pending decision, not every poll. Stop after 15 minutes of no change and hand over the board link.
5. Money. Every render, and every second take the director offers (a `retry_offer` on a piece, which is also the run's `pending_approval` while it waits), is a priced step. Say the step and the number read from the offer, in words, and wait. Call `approve_video_agent_spend` with the `run_id`, `task_id` and `quote_id` exactly as given only after the person says yes to that number. A declined offer leaves the piece as it was, with its score. Credits are charged only when the step succeeds.
6. Hand back what the agent saw in plain words, the board link, each piece with its link, `score`, `verdict` and `reasons`, `credits_spent`, and any retry still offered with its number. The result recreates the look with an original cast; never claim the reference's people, lines or brand as the person's own. Plan questions go to https://aicontentdrop.com/plans and nowhere else.
