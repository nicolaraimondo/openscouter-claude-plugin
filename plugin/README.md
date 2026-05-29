# OpenScouter plugin for Claude Code

Run [OpenScouter](https://openscouter.com) usability studies from your
terminal. Once installed, you can read your studies, tests, analytics, and
insights, and draft new studies, without leaving Claude Code.

## What you get

- **MCP connection** to OpenScouter, auto-registered on install.
- **`/openscouter` command** to invoke it explicitly, e.g.
  `/openscouter list my studies`.
- **Skill** that guides Claude Code whenever you work with your studies.

## Install

```
/plugin marketplace add nicolaraimondo/openscouter-claude-plugin
/plugin install openscouter@openscouter
```

Then run `/mcp` and sign in (a browser opens, you pick your workspace and grant
read, and optionally draft, access).

## What it can and cannot do

- **Read** your studies, tests, analytics, and report findings.
- **Draft** new studies. Launching a study and paying for it always stays a
  click in your OpenScouter dashboard. A connected tool can never spend on your
  behalf or recruit testers.

You can disconnect anytime from your OpenScouter dashboard, under Settings →
Connections.

## Without the plugin

You can also connect manually:

```
claude mcp add --transport http openscouter https://openscouter.com/api/mcp
```

OpenAI Codex users: add a remote MCP server pointing at the same URL.
