# Unfetch Agent Plugin

Connect your AI assistant to your marketing data and turn live account evidence into clear, actionable reports.

Unfetch supports:

- Google Ads campaign, keyword, search-term, conversion, budget, and performance reporting
- Google Analytics traffic and conversion reporting
- Google Search Console query and landing-page reporting
- Keyword research, including demand, competition, CPC, and bid estimates
- Web research for current public-page and landing-page evidence
- Follow-up analysis across report results
- Private memories for reporting preferences and business context

Reporting access is read-only for connected advertising and analytics accounts. The plugin cannot change campaigns, budgets, bids, ads, analytics properties, or Search Console data. Personal memory tools can save or delete private reporting preferences in your Unfetch account.

## Before you install

1. [Sign in to Unfetch](https://unfetch.com/login).
2. Create or select a brand, then open **Integrations** in that brand's navigation to connect its data sources. [Create a brand](https://unfetch.com/add-brand) if you do not have one yet.
3. Confirm that your [team membership and brand access](https://unfetch.com/team) include every brand you want the assistant to use.

OAuth opens automatically when a supported client connects. Access always follows your current Unfetch team membership and brand assignments.

## Install in ChatGPT desktop

A ChatGPT workspace admin can import Unfetch from GitHub:

1. Open **Admin > Plugins** in ChatGPT desktop.
2. Select **Add**, then **Import marketplace**.
3. Enter `https://github.com/unfetch-com/agent-plugin` as the **Source**. Leave **Path** and **Branch, tag, or commit** empty.
4. Import the marketplace, review Unfetch, and make it available in the workspace.
5. Open the **Plugins** tab and install Unfetch.

The installation includes the reporting skill and MCP connection. Direct GitHub marketplace imports require a ChatGPT workspace admin.

## Install in Claude Code

Run these commands inside Claude Code:

```text
/plugin marketplace add unfetch-com/agent-plugin
/plugin install unfetch@unfetch-plugins
```

Restart Claude Code or run `/reload-plugins` if the plugin is not available immediately.

## Install in Codex

Run:

```sh
codex plugin marketplace add https://github.com/unfetch-com/agent-plugin
codex plugin add unfetch@unfetch-plugins
```

Start a new Codex session after installation if the plugin is not available in the current session.

## Install in Gemini CLI

Run:

```sh
gemini extensions install https://github.com/unfetch-com/agent-plugin
```

Restart Gemini CLI after installation if the extension is not available in the current session.

## Install in VS Code with GitHub Copilot

1. Open the Command Palette.
2. Run **Chat: Install Plugin From Source**.
3. Enter `https://github.com/unfetch-com/agent-plugin`.
4. Review the source and confirm the installation.

You can also open **Chat: Open Customizations**, select **Plugins**, and choose **Install Plugin from Source**.

## ChatGPT web and other MCP clients

Use the guided instructions on the [Unfetch plugin setup page](https://unfetch.com/plugin). It provides the production Streamable HTTP connection and handles OAuth without API keys or credentials in this repository.

## Access and privacy

- Manage or revoke connected AI clients from [AI client settings](https://unfetch.com/ai-clients).
- Manage members and brand access from [Team settings](https://unfetch.com/team).
- Manage data-source connections from **Integrations** inside each brand.
- Review the [privacy policy](https://unfetch.com/about/privacy) and [terms of service](https://unfetch.com/about/terms-of-service).
