# Sovran plugin

The Sovran plugin lets Claude Code, Claude, Cowork, and Codex create and review video ad drafts from media saved in your [Sovran](https://sovran.ai) projects. It connects to Sovran's hosted MCP server with your Sovran sign-in and adds a skill for draft review and export.

**Claude Code**

```
/plugin install sovran --marketplace mansonchen29/sovran-plugin
```

**Codex**

```
codex plugin marketplace add mansonchen29/sovran-plugin
codex plugin add sovran@sovran
codex mcp login sovran
```

**Claude and Cowork**: open **Customize → Plugins** and add `mansonchen29/sovran-plugin` as a marketplace.

**ChatGPT**: add `https://sovran.ai/api/mcp` as a custom MCP server. See the [setup guide](https://sovran.ai/docs/api/mcp).

The plugin is in [`plugins/sovran`](plugins/sovran). Its [README](plugins/sovran/README.md) explains what it does, what it sends, and how to sign in. See [LICENSE](plugins/sovran/LICENSE) for the terms of use.
