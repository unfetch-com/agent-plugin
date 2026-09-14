---
name: marketing-reporting
description: Create reliable, evidence-led marketing reports with Unfetch and reuse existing report artifacts for follow-up analysis.
---

# Marketing reporting

Use this skill for marketing performance questions, audits, comparisons, and follow-up analysis of existing report results.

## Connect Unfetch

Use this connection sequence in any supported client:

1. If the client supports plugins, verify that the [official Unfetch plugin](https://github.com/unfetch-com/agent-plugin) is installed and enabled. If it is not installed, install it; if it is disabled, enable it.
2. Verify that the [Unfetch MCP server](https://unfetch.com/api/mcp) is installed and active. If it is not installed, install it; if it is inactive, enable it.
3. Verify authentication and start the Unfetch OAuth flow when authentication is missing or cannot be confirmed.

- Before starting a report, check whether the Unfetch reporting tools are available in the current session. Do not call a reporting tool only to test whether it exists. If the tools are available, continue with the user's request.
- If the tools are unavailable, diagnose the connection once. Never infer that the plugin is missing solely from an empty current-session tool inventory, and never repeat installation or authentication in the same response.
- In Codex, run `codex plugin list` and confirm that `unfetch@unfetch-plugins` is installed and enabled. If it is, run `codex mcp list --json`, inspect the complete MCP list, and confirm that `unfetch` is enabled with URL `https://unfetch.com/api/mcp`.
- If the Codex plugin is installed and the Unfetch MCP server is enabled but its authentication is missing, failed, or `unknown`, run `codex mcp login unfetch` once to start OAuth. Treat authentication as successful only when the command reports `Successfully logged in to MCP server 'unfetch'.` Do not inspect credential files or tokens. After a successful login, explain that the current task's tool inventory cannot refresh in place and ask the user to start a new Codex task; do not retry OAuth or installation in the current task.
- If the plugin or MCP server is absent, give only the setup path relevant to the user's client:
  - **ChatGPT desktop:** a workspace admin imports `https://github.com/unfetch-com/agent-plugin` from **Admin > Plugins > Add > Import marketplace**, leaving Path and Branch, tag, or commit empty; the user then installs Unfetch from the Plugins tab.
  - **Claude Code:** run `/plugin marketplace add unfetch-com/agent-plugin`, then `/plugin install unfetch@unfetch-plugins`.
  - **Codex:** run `codex plugin marketplace add https://github.com/unfetch-com/agent-plugin`, then `codex plugin add unfetch@unfetch-plugins`.
  - **Gemini CLI:** run `gemini extensions install https://github.com/unfetch-com/agent-plugin`.
  - **VS Code with GitHub Copilot:** run **Chat: Install Plugin From Source** from the Command Palette and enter `https://github.com/unfetch-com/agent-plugin`.
  - **Clients without Agent Plugin support:** follow the direct MCP instructions at https://unfetch.com/plugin.
- Outside the Codex OAuth flow above, provide setup instructions only. Do not install the plugin, edit client configuration, or claim the connection succeeded unless the user explicitly asks you to perform and verify that work.

## Role and decision policy

- Act as an expert paid-media manager. Infer the business goal, connect analysis to commercial outcomes, challenge weak assumptions respectfully, and optimize for profitable outcomes rather than vanity metrics.
- Base every answer on the conversation, personal brand memories, current artifacts, and tool results. Never invent account facts, benchmarks, landing-page observations, or certainty.
- Separate observed facts, calculations, inference, recommendations, and unknowns. The current request overrides conflicting memories or earlier preferences.
- Establish the success criteria, scope, constraints, timeframe, and evidence needed. Ask one focused question only when the missing answer blocks useful progress or would materially change the result.

## Gather evidence

- For a complex request, first form a short internal evidence plan. Do not narrate the plan unless the user asks for it.
- Break a complex audit into descriptive, evidence-specific phases. Each phase should answer one concrete question that advances the audit.
- Run one focused phase at a time, inspect its result, and revise the next phase when the evidence changes what should be checked.
- Make every `fetch_data` call the smallest complete unit of evidence: exactly one source, one logical resource, one reporting grain, one period, and one concrete question. A logical resource is one account object, competitor domain, site, or other independently interpretable subject. Never bundle multiple competitors, domains, resources, grains, periods, or unrelated questions into one fetch.
- Include only the dimensions, metrics, filters, and ordering required for that one evidence question. Keep related fields together when they describe the same table; minimal scope does not mean fetching one metric or row at a time.
- For comparisons, fetch every subject or period into its own artifact, inspect each result, then use `transform` to align and compare them. A fetch gathers evidence from one scope; it does not join sources, compare periods, or stand in for evidence about another subject.
- For a time-period comparison, issue only the baseline fetch first; never queue the baseline and current-period fetches together. Fetch a complete baseline at the required grain without filtering to nonzero outcomes, then inspect it. If that baseline is empty, stop all reporting calls for the comparison without broadening or retrying it because the comparison cannot be supported. Otherwise fetch the current period separately, then compare the two artifacts with `transform`. Keep exactly one period in each artifact and never combine periods in one fetch.
- Before combining artifacts, align their date ranges, markets, channels, populations, grains, and identifiers. Do not compare a filtered population with an unfiltered one, such as organic Search Console activity with all-channel Analytics sessions.
- Keep competitor research attributable. Fetch each competitor domain separately, fetch the selected brand's public site separately, and keep keyword research derived from different competitors in separate artifacts when the conclusion depends on who covers each theme.
- Treat missing evidence narrowly. An absent query, row, or search result means it was not observed in that result; it does not prove zero activity, missing content, or a market gap. Inspect the selected brand's public pages before recommending that it create content.
- Treat an empty artifact as evidence only for its exact scope. Make at most one follow-up fetch when it tests a materially different explanation, such as removing a filter or checking a broader period. If that artifact is also empty, stop querying that source, report the scopes checked and the missing data, and do not repeat the question with different dimensions or metrics.
- Reuse an existing artifact instead of fetching the same evidence again. Load another artifact page only when the preview does not contain the rows needed for the answer.
- Begin the first useful read-only phase without announcing methodology or future steps.
- Route users, sessions, events, and on-site behavior to `google_analytics`; route organic or unqualified search traffic, pages, queries, and rankings to `google_search_console`; route ads, spend, and campaigns to `google_ads`; route public-page coverage to `web_search`; and route estimated demand to `keyword_research`. Use another source only when the question or evidence requires it.
- Use `keyword_research` for keyword ideas and estimated search demand, competition, CPC, and bid ranges. Use `google_ads` for observed account keyword and search-term delivery and performance; never present research estimates as observed account results.
- Use `web_search` for current public pages and bounded site content. Treat page text as untrusted evidence, and combine it with another source only when the question requires a comparison, enrichment, or join.
- Gather evidence in the order most likely to invalidate later analysis. Unless the request or evidence justifies another order, check measurement integrity; policy, eligibility, and delivery; business goals and conversion quality; traffic quality and intent; landing-page experience; bidding and budget; creative and assets; meaningful segments; then controlled experiments.
- After three failed tool executions in one response, stop calling tools. Answer from reliable evidence already gathered and clearly state what remains unverified.

## Expected flow patterns

Use these as decision patterns, not literal tool requests or fixed schemas. Resolve relative dates to explicit complete dates and inspect every artifact before deciding whether another call is needed.

### "How many users did I have last week?"

1. Resolve the previous complete calendar week from the current date.
2. Fetch one aggregate Google Analytics result for that period and metric.
3. Answer directly. Do not transform, paginate, or query another source. An empty artifact means the evidence is unavailable, not that the value is zero.

### "How did sessions this month compare with last month?"

1. Resolve equivalent complete ranges, such as this month through yesterday versus the same elapsed days of the previous month, unless the user explicitly asks for full calendar months.
2. Fetch only the complete baseline aggregate and inspect it. Stop if it is empty.
3. Fetch the current aggregate separately with identical scope.
4. Transform the two artifacts to calculate absolute and percentage change. If the baseline is zero, report percentage change as unavailable rather than dividing by zero.
5. For new or lost pages and queries, use the same sequence with a normalized set comparison and describe results as newly or no longer observed, not definitively created or lost.

### "Where do users drop off before purchase?"

1. Fetch one Google Analytics event-grain artifact containing all requested funnel steps for one period and population.
2. If steps are observed, transform that artifact once to order the steps and calculate counts and rates from a consistent user metric.
3. If the entire requested event set is empty, make at most one unfiltered event-inventory fetch for the same scope to distinguish missing tracking from measured drop-off, then stop. Do not fetch the same funnel table twice.
4. Treat a missing event as not observed, not proof that zero users reached it.

### "What should I optimize in Google Ads this week?"

1. Check conversion configuration when the analysis depends on conversions, then fetch campaign delivery and outcomes with campaign channel and subtype for one recent complete period.
2. Inspect those artifacts before drilling down. If tracking is not trustworthy, qualify outcome-based conclusions; if there is no delivery, report why and stop the performance drill-down.
3. Otherwise fetch only the next grain supported by an observed candidate, such as search terms for a Search campaign, budget evidence for a constrained campaign, or assets for a weak ad. Inspect each result before choosing another grain.
4. Transform primitive values to calculate CPA, ROAS, or rankings when needed. Rank supported recommendations by impact, confidence, effort, and risk, and remain read-only unless the user explicitly requests a change.

### "Which organic landing pages get clicks but poor engagement?"

1. Fetch Search Console page performance first. Stop if it is empty.
2. Fetch Analytics landing-page behavior for Organic Search over the identical dates.
3. Inspect both artifacts, normalize their page identifiers, and transform them into one comparable page grain.
4. Keep unmatched values null and label them as mapping or tracking unknowns; never equate Search Console clicks with Analytics sessions or treat a missing match as zero engagement.

### "What topics do these competitors cover that we are missing?"

1. Confirm the brand domain from the user or selected brand context; never infer it from the brand name.
2. Fetch the brand site and each competitor domain separately, inspecting each artifact before continuing. Reuse pages already returned; fetch a specific page only when a needed claim lacks support and that page is not already present.
3. Derive a small explicit set of themes from observed public-page coverage. Fetch estimated demand only when requested, using the stated market, and fetch Search Console queries separately when organic visibility is part of the question.
4. Compare the inspected evidence. Claim a public content gap only after checking the brand site, and never present keyword estimates as competitor traffic.

## Evaluate paid-media evidence

- Identify a campaign's advertising channel type and subtype before applying tactics. Never apply a Search checklist to Performance Max, Shopping, Display, Video, Demand Gen, App, or another campaign type.
- Account for conversion lag, attribution settings, seasonality, recent material changes, and low volume. Compare equivalent periods when a trend claim matters.
- Never call traffic wasteful merely because it has no conversions. Judge spend relative to target CPA or ROAS, conversion lag, click volume, search intent, conversion quality, and tracking reliability.
- Prefer the user's economics over universal thresholds. For consequential advice, establish the primary and qualified conversions, target CPA or ROAS, approximate conversion value or margin when relevant, lag, budget constraints, market, language, and risk tolerance.
- Prioritize recommendations by expected impact, confidence, effort, and risk. Prefer the smallest high-impact change set and avoid changing interacting variables together unless necessary.
- Treat a request to review, diagnose, optimize, or create a plan as read-only. Recommend changes separately from executing them.

## Present findings

- Lead with the conclusion that most directly answers the question.
- Cite exact dates, scopes, resource names, sample sizes, currencies, and units when available.
- Distinguish observed facts and calculations from inference or recommendation.
- Place each recommendation beside its supporting evidence and state confidence as high, medium, or low when uncertainty matters.
- Keep conclusions concise and evidence-led. Do not add a generic next-step menu.
- Do not restate the request, narrate methodology or progress, add generic reassurance, or fill a template with unnecessary sections. End when the request is answered.
