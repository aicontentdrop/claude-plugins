# AI Content Drop Studio for Claude

Make AI video, images, finished ads, boards, competitor research and short drama from Claude, through the AI Content Drop connector, with every paid step quoted first and approved by you. The plugin bundles three things: the connector (the AI Content Drop MCP server at `https://aicontentdrop.com/mcp`), eight slash commands that drive its tools in the right order, and the agent skills that teach Claude how each family of tools works.

Two facts to know before installing. This plugin is distributed through our own marketplace, not through Anthropic's plugin directory, so nobody but us has reviewed it. It generates media through your own AI Content Drop account: you sign in through the connector, and credits are charged only when a step succeeds.

## What it does

The connector's tools come in six families. Each family has a command, and each command names the tools it calls and stops at every spend.

| Family | What the tools do | Command |
| --- | --- | --- |
| Generation | List the models and their credit costs, quote a job, make one video or image, poll it to the file. | `/aicontentdrop-studio:models`, `/aicontentdrop-studio:make` |
| Studio ads | Brief, storyboard, quote, render and poll a finished, captioned video ad, with presenters and brand profiles. | `/aicontentdrop-studio:ad` |
| Boards | Create or open a board, add text, media and generation cards, render cards one quoted step at a time. | `/aicontentdrop-studio:board` |
| Research | Search the ads competitors run, keep the ones worth studying, decode a winner into a brief, place it on a board. | `/aicontentdrop-studio:research` |
| Video agent | Hand a whole job to Drop, the video agent: research, concepts, plan, renders and a judge's score, with a reference read into words first when you have one. | `/aicontentdrop-studio:drop`, `/aicontentdrop-studio:recreate` |
| Drama Studio | Write, review, price, render and cut a short vertical drama with a cast that stays the same between clips. | `/aicontentdrop-studio:drama` |

The commands:

| Command | What it does |
| --- | --- |
| `/aicontentdrop-studio:drop <goal>` | Starts a drop from a goal, relays its questions, polls the run, and approves each quoted step only after you say yes. With a reference URL it asks who holds the rights, reads the reference into words, and starts from those words. |
| `/aicontentdrop-studio:recreate <reference URL and goal>` | The reference lane: the rights question, the free read, the run from the brief, each piece's score and verdict with the Director's notes, and a priced retry you approve by its exact quote. |
| `/aicontentdrop-studio:make <what to make>` | Quotes one video or image, waits for your yes, makes it, and polls it to the finished file. |
| `/aicontentdrop-studio:ad <brief>` | Storyboard first, then the quote with its lines, your yes, the render and the poll, to a captioned ad. |
| `/aicontentdrop-studio:board <name and contents>` | Opens or creates a board, lays out cards, and renders them one quoted card at a time. |
| `/aicontentdrop-studio:drama <premise>` | Has the script written, reads it back to you, prices the episode, renders the clips after your yes, and cuts the episode. |
| `/aicontentdrop-studio:research <keyword>` | Searches competitor ads, saves the ones worth studying, and decodes a winner into a brief after a quote. |
| `/aicontentdrop-studio:models [brief]` | Lists models and credit costs and quotes a job. Read-only: nothing is made and nobody is signed in. |

The skills load on their own when a conversation matches their description:

| Skill | Use it for |
| --- | --- |
| `ai-video-model-picker` | Choose a video model for a brief and quote the whole job first. |
| `video-prompt-writer` | Turn a brief, script or still into a shot-first prompt. |
| `image-generation` | Pick an image model, iterate cheaply, finish on the best one, and make a frame to animate. |
| `batch-generation-runner` | Quote, submit, poll, resume and report a storyboard or shot list. |
| `api-error-handling` | Read a refusal correctly and decide whether to retry. |
| `credits-and-plans` | Explain what a generation costs and whether it fits a balance. |
| `marketing-studio-ad` | Brief, quote, approve, start and poll a finished ad or presenter video. |
| `canvas-board` | Plan on a board and render its cards with a quote and a yes for each. |
| `competitor-ad-research` | Search, save and decode the ads competitors run. |
| `video-agent` | Hand a whole piece of work to Drop and approve each paid step by its quote. |
| `recreate-reference` | Make a video in the manner of a reference, from the rights question to the scorecard. |
| `drama-studio` | Write, accept, price, render and cut a short vertical drama. |
| `short-drama-writer` | Write a short drama as text, from premise tests to an ending that demands the next episode. |

## The money rule

Every step that charges credits is quoted first (`estimate_credit_cost`, `estimate_ad_cost`, `quote_drama_episode`, or the run's `pending_approval`), put to you in words with the number, and run only after you say yes. The one exception is the drama writing step: `start_drama` and `revise_drama` have no quote tool, so the command tells you it is a paid step before calling and reads `credits_reserved` back to you the moment the call returns. Credits are charged only when the step succeeds; a failed, refused or timed-out step charges nothing. Reading a reference, planning, storyboards, boards and the judge's scores cost nothing. Questions about what a plan includes go to <https://aicontentdrop.com/plans>, and the plugin says nothing else about plans.

## Install

### Claude Code

Inside a session:

```text
/plugin marketplace add aicontentdrop/claude-plugins
/plugin install aicontentdrop-studio@aicontentdrop
```

Or from the shell:

```bash
claude plugin marketplace add aicontentdrop/claude-plugins
claude plugin install aicontentdrop-studio@aicontentdrop
```

The commands appear as `/aicontentdrop-studio:drop`, `/aicontentdrop-studio:make` and so on, and `/mcp` shows the connector. The first tool that needs an account asks you to sign in; `/mcp` runs the sign-in. `/aicontentdrop-studio:models` works without one.

### claude.ai and Claude Desktop

1. Open **Customize > Plugins**, select **Add > Add marketplace**, and enter `https://github.com/aicontentdrop/claude-plugins` (or `aicontentdrop/claude-plugins`).
2. Install **AI Content Drop Studio** from the marketplace.
3. Open the plugin, select its **Connectors** tab, add **AI Content Drop**, and sign in with your AI Content Drop account.

In chat the commands load as skills that Claude applies when the task fits; in Cowork you run them by name.

To use the connector on its own, without the plugin, add it as a custom connector from this link and set **Authentication** to **Sign in when needed**:

<https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=AI%20Content%20Drop&connectorUrl=https%3A%2F%2Faicontentdrop.com%2Fmcp>

### Claude mobile

Add the connector on the web or in Claude Desktop first, as above. It is then available in the mobile apps on the same account.

### MCP alone

Any MCP client that speaks streamable HTTP and OAuth can use the server without the plugin:

```bash
claude mcp add --transport http aicontentdrop https://aicontentdrop.com/mcp
```

### The CLI

For a terminal or a script, the npm package `aicontentdrop` installs the `acd` command:

```bash
npm install -g aicontentdrop
acd models --type video --max-credits 20       # the catalogue, no account needed
acd cost kling_3_0 --quantity 4                # a quote, no account needed
acd analyze ./reference.mp4 --rights own_work  # read a clip or image into words
acd drop "a 15-second vertical ad in this style" --reference ./reference.mp4 --rights own_work
acd drop status <run_id> --notes               # phase, credits, each piece's score, the Director's notes
```

For scripts, create an API key at <https://aicontentdrop.com/settings/integrations> and keep it in `ACD_API_KEY`. Never paste a key into a conversation; the plugin and the connector never ask for one.

## What it reads and sends

- **Tool arguments go to our server** at `aicontentdrop.com`: the prompts, briefs, URLs, ids and settings Claude passes when you run a command or it calls a tool. Nothing is sent before then.
- **Sign-in is OAuth** on `aicontentdrop.com`. The plugin's files hold no credential, and the connector reads no key from a conversation.
- **Reference files you choose to send** (a public URL passed to `analyze_reference`, with your rights answer) are read into words and deleted once the reading succeeds; a reading that fails keeps the file for one retry, then deletes it. Only the words stay on the run. The rights basis is always your answer, never chosen for you.
- The plugin stores nothing itself. Generations, boards, drops and dramas live on your account at `aicontentdrop.com`.

## Facts the commands rely on

- The model catalogue and cost quotes come from the connector (`list_models`, `estimate_credit_cost`) or from <https://aicontentdrop.com/v1/models>, never from memory. Most models cost a flat price per generation; a few video models price by the second and by resolution.
- Credits are charged only when a step succeeds.
- Most tools that need an account are listed before sign-in and ask for it on first use; the drop and drama tools are listed only once you have signed in, and the commands say what to do when a tool is missing.

## Source

The skills under `skills/` are exported byte for byte from the [`aicontentdrop`](https://www.npmjs.com/package/aicontentdrop) package by `scripts/build-claude-plugin-studio.mjs`, which also assembles the marketplace copy, and a test fails the build if any copy drifts. See [SETUP.md](./SETUP.md) for a local install and the first commands to try. MIT licensed. Support: <support@aicontentdrop.com>.
