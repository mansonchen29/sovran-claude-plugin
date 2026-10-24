# Sovran for Claude and Codex

Create editable video ad drafts from media saved in your [Sovran](https://sovran.ai) projects, review them in the Sovran editor, and render only the drafts you choose.

Ask your agent in your own words. For example:

- "List my Sovran projects and show the ready media in one project."
- "Create four Video Variations from my saved product demo. Show the drafts before export."
- "Find audiences for my product demo video. Draft ads only for the audiences I approve."
- "Estimate the cost to render the three drafts I picked, then render them."

## What it can do

- **Create drafts** with Video Variations, Smart Variations, Audience variations, Modular Builder sequences, Voiceover Remix, B-roll Remix, and Creative Copilot.
- **Review drafts** through links that open each saved draft in the Sovran editor.
- **Render selected drafts** after you approve the credit cost. Up to ten drafts from one project render together as one batch.
- **Check progress** of creation and render jobs, and read finished video links again without another render.

The agent stops for you three times: before it creates audience drafts, before it exports drafts, and before it spends render credits.

## What you need

- A Sovran account with an active paid workspace plan. Creation and rendering use your workspace credits.
- Media already saved in a Sovran project. Upload local files in the Sovran dashboard or with the [Sovran CLI](https://sovran.ai/docs/api/cli).

## Install

**Claude Code**

```
/plugin install sovran --marketplace mansonchen29/sovran-plugin
```

On Claude Code before 2.1.275, run `claude plugin marketplace add mansonchen29/sovran-plugin`, then `claude plugin install sovran@sovran`. Then run `/mcp`, select **sovran**, and sign in.

**Claude and Cowork**

Open **Customize → Plugins**. Add `mansonchen29/sovran-plugin` as a marketplace and install **Sovran**. Then connect **Sovran** on the plugin's **Connectors** tab.

**Codex**

```
codex plugin marketplace add mansonchen29/sovran-plugin
codex plugin add sovran@sovran
codex mcp login sovran
```

Start a new Codex session after you sign in.

**Sign in**

Sign in to Sovran. Choose one workspace and approve **Read**. Sovran asks for Write, Generate, Delete, or Publish later, only when a task needs them. Then ask: "List my Sovran projects."

Team and Enterprise administrators may need to allow the plugin or its connector first.

## What the plugin contains

The plugin runs no code on your computer. It has two parts:

- **A skill** (`skills/video-ads`) that tells the agent how to choose a Sovran flow, when to stop for your approval, and how to check jobs.
- **A remote MCP server** at `https://sovran.ai/api/mcp`. The agent sends the tool calls it makes, such as project IDs, draft selections, and ad briefs, to this server under your Sovran sign-in. Sovran sends results back to the agent. The plugin sends nothing anywhere else.

The connection uses OAuth. The agent never sees your Sovran password. You can disconnect at any time from **Tools → MCP Server** in the Sovran dashboard.

## Privacy and support

- Privacy policy: https://sovran.ai/legal/privacy
- Terms of service: https://sovran.ai/legal/terms
- Documentation: https://sovran.ai/docs/api/mcp
- Support: support@sovran.ai
