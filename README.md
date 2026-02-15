# OpenClaw Agent Score

Test how agent-friendly your website is using [OpenClaw](https://openclaw.ai). This skill sends a real AI agent to your site, where it tries to navigate, sign up, find MCP tools, and complete tasks — then reports back with a structured score.

## What it does

The agent will:

- Navigate your site and understand what it does
- Find key information (pricing, docs, features)
- Create an account if there's a signup flow
- Check for MCP tools (`navigator.modelContext`)
- Complete a representative task (search, fill a form, etc.)
- Note any blockers (CAPTCHAs, anti-bot measures, broken forms)

The output is a structured JSON report with:

| Field | Type | Description |
|-------|------|-------------|
| `score` | `0-100` | How agent-friendly the site is |
| `summary` | `string` | 2-3 sentence overview |
| `what_worked` | `string[]` | Things that went smoothly |
| `what_didnt` | `string[]` | Things that were hard or impossible |
| `mcp_found` | `bool` | Whether MCP tools were detected |
| `signup_attempted` | `bool` | Whether the agent tried to sign up |
| `signup_succeeded` | `bool` | Whether signup worked |
| `tasks_tried` | `array` | Each task attempted with pass/fail and detail |

## Install

Into your workspace (highest precedence):

```bash
clawhub install agent-score
```

Into the managed/global location:

```bash
git clone https://github.com/pillarhq/openclaw-agent-score.git ~/.openclaw/skills/agent-score
```

## Usage

```
Run agent-score on https://your-site.com
```

## Example output

```json
{
  "score": 72,
  "summary": "The site has good navigation and well-labeled forms, but signup was blocked by a CAPTCHA. No MCP tools found.",
  "what_worked": [
    "Homepage clearly explains the product",
    "Navigation links are descriptive and consistent",
    "Pricing page is easy to find"
  ],
  "what_didnt": [
    "Signup form blocked by reCAPTCHA",
    "No MCP tools registered"
  ],
  "mcp_found": false,
  "mcp_usable": false,
  "signup_attempted": true,
  "signup_succeeded": false,
  "tasks_tried": [
    {
      "task": "Navigate to pricing page",
      "succeeded": true,
      "detail": "Found pricing link in main nav, page loaded with clear tier comparison."
    },
    {
      "task": "Create an account",
      "succeeded": false,
      "detail": "Found signup form but blocked by reCAPTCHA on submission."
    },
    {
      "task": "Search for documentation",
      "succeeded": true,
      "detail": "Found docs link in footer, search worked and returned relevant results."
    }
  ],
  "full_report_url": "https://trypillar.com/agent-score?domain=your-site.com"
}
```

## Full multi-category report

This skill tests the **agent experience** — one piece of the puzzle. For the full picture including content accessibility, interaction quality, and WebMCP support, run a free scan at:

**[trypillar.com/agent-score](https://trypillar.com/agent-score)**

## License

MIT
