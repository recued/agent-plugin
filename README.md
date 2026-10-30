# Recued agent plugin

An [Agent Plugins](https://agent-plugins.org) package that connects your AI agent to your own
[Recued](https://recued.com) server, on the computer Recued runs on. It brings the `recued` skill,
which tells the agent how to work with Recued's approvals, and Recued's own local MCP server,
`recued --mcp`. It ships no code of its own.

Recued is a server you run yourself. It keeps your notes, tasks, mail, calendar and the things you
own, and it stands between your data and any AI: it checks every request against what you granted,
holds a change for your approval, and keeps a record of what was done.

## What you need

- Recued 26.10.30 or later, installed and running on this computer:
  [install it](https://recued.com/docs/setup/install-server/). The `recued` command must be on the
  `PATH` your agent app uses.
- A contract for your agent. In the Recued web app, open Contracts, make a contract, choose the
  tools the agent may use, and copy its key.

## Install

Codex:

```sh
codex plugin marketplace add recued/agent-plugin
codex plugin add recued@recued
```

Other apps that load Agent Plugins packages can install this repository with their own plugin
commands.

Cursor, and other apps that read a `.mcp.json` file: this repository's `.mcp.json` starts the same
server as `recued --mcp --app cursor`, so `recued mcp-key set --app cursor` gives Cursor a key of its
own. Agent Plugins apps read `mcp.json` only, as the specification requires.

## Give it the contract's key

The plugin carries no key, and your app does not pass one to it, so you save the key in Recued:

1. Start your agent app once, so that it starts Recued's MCP server. Recued notes the app.
2. In a terminal, run `recued mcp-key set`. Pick your app from the list, or pick "Every app", then
   paste the contract's key. Recued does not show the key as you paste it.
3. Restart your agent app. It now has the contract's tools.

`recued mcp-key list` shows which contract each app uses. You can give each app its own contract,
or one contract to all of them. `recued mcp-key clear` removes a saved key.

## How it works

- The tools you see are the ones the contract grants. To give the agent another tool, change the
  contract in the Recued web app, then restart the agent.
- Reading runs at once: Recued marks its read-only tools.
- A change waits for you. Recued holds it for your approval in the Recued app, from the bell at the
  top of the page. When you have answered, tell the agent, and it can ask Recued for the result.
- If Recued runs on another computer, this plugin's local server cannot reach it. Connect the agent
  over HTTP instead, as the contract's Connect tab shows.

## Privacy

Your data stays on your Recued server. The agent sees only what a tool returns, and Recued returns
only what the contract grants. Neither your app nor this plugin holds the contract's key. Recued's
privacy policy: https://recued.com/privacy

## Support

Write to us through https://recued.com/support, or open an issue in this repository.

## License

MIT
