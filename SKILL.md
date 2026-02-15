---
name: agent-score
description: Test how agent-friendly a website is. Navigates the site, tries to sign up, checks for MCP tools, completes tasks, and returns a structured JSON score report. Use when asked to evaluate a website's AI agent readiness or run an agent score test.
metadata: { "openclaw": { "emoji": "🏆", "homepage": "https://trypillar.com/agent-score" } }
---

# Agent Score — AI Agent Readiness Test

Test how easy a website is for AI agents to use. Actually try things on the site and report what happened honestly.

## Tools

Use these OpenClaw tools in this order:

1. **web_fetch** the target URL first (lightweight, gets page structure without JS)
2. **browser open** the URL with `profile: "openclaw"` (clean browser, no user state)
3. **browser snapshot** with `compact: true` to read the DOM
4. **browser act** to interact — kinds: `click`, `type`, `navigate` between pages
5. **browser act** kind=`evaluate` to run JS — specifically for MCP detection:
   `() => { const mc = navigator.modelContext; if (!mc) return 'not found'; try { return JSON.stringify({ found: true, tools: mc.getTools ? mc.getTools() : 'no getTools' }); } catch(e) { return 'error: ' + e.message; } }`
6. **browser close** the tab when done

If the browser tool is unavailable, use web_fetch only and note that interactive testing was not possible.

## Test Procedure

Work through these in order. Skip any that don't apply:

1. **Navigate and understand** — Open the site, read the homepage, figure out what it does.
2. **Find key information** — Pricing, docs, features. Can you find what you need?
3. **Create an account** — If signup exists, try with:
   - Name: Pillar Agent Test
   - Email: agent-test@score.trypillar.com
   - Password: PillarTest!2026
   - Stop at CAPTCHAs or payment walls. Do NOT use OAuth/SSO.
4. **Check for MCP tools** — Run the JS evaluation above on every page you visit.
5. **Complete a task** — Search, fill a form, add to cart, navigate deep. Pick what's most representative.
6. **Note blockers** — CAPTCHAs, anti-bot, broken forms, confusing navigation.

## Output

Respond with ONLY this JSON (no markdown fencing, no extra text):

{
  "score": 75,
  "summary": "2-3 sentences describing the overall agent experience.",
  "what_worked": ["Navigation was clear", "Forms were well-labeled"],
  "what_didnt": ["Signup blocked by CAPTCHA", "No MCP tools"],
  "mcp_found": false,
  "mcp_usable": false,
  "signup_attempted": true,
  "signup_succeeded": false,
  "tasks_tried": [
    {
      "task": "Navigate to pricing",
      "succeeded": true,
      "detail": "Found pricing link in nav, loaded with clear tiers."
    }
  ],
  "full_report_url": "https://trypillar.com/agent-score?domain=DOMAIN_HERE"
}

## Scoring

- **90-100**: Everything worked. Navigate, sign up, MCP tools, tasks — no friction.
- **70-89**: Most things worked. Minor issues but agent accomplished its goals.
- **50-69**: Mixed. Some tasks worked, others impossible. Significant friction.
- **30-49**: Mostly difficult. Basic tasks failed. Many blockers.
- **0-29**: Unusable. CAPTCHAs everywhere, no semantic structure, broken forms.

Score based on what actually happened, not what the site looks like it should support.
