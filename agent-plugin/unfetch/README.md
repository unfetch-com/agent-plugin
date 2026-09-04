# Unfetch Agent Plugin

This plugin connects compatible AI agents to Unfetch's marketing reporting tools. Reporting cannot change connected advertising or analytics accounts. Personal memory tools can save or delete your private reporting preferences. Install it from your client's marketplace or local plugin flow, complete OAuth when prompted, and choose a brand from the accessible brand list.

Clients without Agent Plugin support can connect directly with the Streamable HTTP endpoint shown on the Unfetch MCP setup page. The direct server and this plugin use the same reporting skill instructions.

The package contains no credentials or brand identifiers. Access follows your current Unfetch team membership and brand assignments and can be revoked from https://unfetch.com/mcp.

## Package structure

The reporting instructions have one source at `skills/marketing-reporting/SKILL.md`. The root Agent Plugins manifest and the Claude and OpenAI manifests are generated from shared metadata, including the MCP URL, with `npm run generate:agent-plugin`.

Claude Code reads `.claude-plugin/plugin.json` and `.mcp.json`. Codex reads `.codex-plugin/plugin.json` and `.mcp.json`. For an OpenAI public submission, submit the same reporting skill and the production MCP endpoint together through the **With MCP** flow; the portal registers the endpoint separately from this local package configuration.
