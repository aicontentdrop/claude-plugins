---
description: Hand a whole video job to Drop, the AI Content Drop video agent, with every paid step quoted and approved first
argument-hint: <goal in one sentence; optionally a public URL of a reference image or clip>
---

Run a drop for: $ARGUMENTS

Use the bundled `video-agent` skill and the AI Content Drop connector's tools `start_video_agent`, `message_video_agent`, `get_video_agent_run`, `approve_video_agent_spend`, `list_video_agent_runs` and, for a reference, `analyze_reference`. Call them as connector tools; do not reach for the REST API or a script instead.

Sign-in. The drop tools are listed only to a signed-in account. When a tool answers that it needs an account, sign in through the connector (in Claude Code, `/mcp`; on claude.ai and in Claude Desktop, the plugin's Connectors tab), then call the tool again. Never ask for or pass an API key.

Money. Reading, planning and judging cost nothing. A step that would charge credits parks the run with `status: waiting_approval`, and `pending_approval` carries `task_id`, `quote_id`, `credits` and `what`. Tell the person the step and the number read from `pending_approval.credits`, in words, and wait. Call `approve_video_agent_spend` with the `run_id`, `task_id` and `quote_id` exactly as given only after the person says yes to that number; an unclear answer is not a yes. `decision: "decline"` cancels that step. Credits are charged only when the step succeeds. Never set `budget_credits` unless the person gave the number.

1. With a reference image or clip: ask who holds its rights and pass the answer as `rights_basis` (`own_work`, `licensed` or `permission`) to `analyze_reference` with the public `https` `url`. Never choose a basis for the person and never infer one. A refusal names its reason in `details.reason` (`rights_required`: ask the person; `vision_unavailable`: run the drop from a written brief). Relay the summary, shots, cast and look as what the agent saw, with the `reference_class` and its confidence, and let the person correct the class. If `status` is `analyzing`, say the reading is still running and start the drop with the `reference_url`; the facts arrive on the run. Without a reference, skip this step.
2. `start_video_agent` with the goal in one sentence, plus `reference_url` exactly as the read returned it and `reference_class` when the person confirmed or corrected it. Starting charges nothing.
3. `message_video_agent` with the brief: subject, audience, purpose, length, format, and assets as public `https` URLs. Relay the drop's questions to the person as they are; do not answer for them.
4. Poll `get_video_agent_run` every 15 seconds. Report a change of phase, a finished piece or a pending decision, not every poll. While a turn is still running, do not send the message again. Stop after 15 minutes of no change and hand over the board link.
5. At every `pending_approval`, including a `retry_offer` on a scored piece, apply the money rule above. Read each piece's `score`, `verdict` and `reasons` and the run's `director_notes` to the person as the agent's opinion, never as a reason to approve on their behalf.
6. Hand back the board link, each finished video with its link, `credits_spent`, and any decision still waiting with its number. To pick up an earlier drop, `list_video_agent_runs` then `get_video_agent_run`. Plan questions go to https://aicontentdrop.com/plans and nowhere else.
