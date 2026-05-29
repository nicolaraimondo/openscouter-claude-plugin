---
name: openscouter
description: >
  Work with OpenScouter usability studies, tests, insights, and analytics from
  the terminal, and draft new studies. Use whenever the user mentions
  OpenScouter, a usability/UX study or test, commissioning or drafting a study,
  checking how a test scored, or reviewing findings/insights for a study, and
  the OpenScouter MCP server is connected. Studies are the parent container; a
  test is one tester session within a study.
---

# OpenScouter

OpenScouter is a usability testing platform. Clients run studies (with their own
testers or OpenScouter's panel) to evaluate the design, usability, and UX of a
webpage, app, or new feature, ideally before it ships. This skill helps you
drive a client's OpenScouter workspace through the connected MCP server.

## Prerequisite: the MCP must be connected

These tools only exist once the user has connected the OpenScouter MCP:

```
claude mcp add --transport http openscouter https://openscouter.com/api/mcp
```

The first call opens a browser to sign in and pick a workspace. If the tools
below are not available, tell the user to run that command (and `/mcp` to finish
signing in) before continuing. If a tool returns "not found", the item either
does not exist or belongs to a different workspace than the one connected.

## Vocabulary (use these terms with the user)

- **Study** — the parent piece of work a client commissions (a test plan for a
  page/app). The tools call its id `study_id`.
- **Test** — one individual tester session inside a study (`test_id`).
- **Insight / finding** — a usability observation, with a severity and a
  recommendation.

## Tools

Read (always available with a connected token):

- `list_studies` — the workspace's studies, newest first. Optional `status`
  filter (draft, open, in_progress, completed, closed) and `limit`.
- `get_study` — one study by `study_id`, including its aggregated cross-tester
  report and findings (if produced) and how many tests it has.
- `list_tests` — tests, optionally scoped to one study. Includes
  `pipeline_status` so you can see where each test is in analysis.
- `get_test` — one test by `test_id`, with status and pipeline stage.
- `get_analytics` — composite scores for a test (overall plus emotion,
  interaction, and voice sub-scores).
- `get_insights` — insights for a test (`test_id`) or aggregated findings for a
  study (`study_id`).

Write (only if the connection was granted the "draft" scope):

- `create_draft_study` — create a new study **as a draft**. Inputs: `title`
  (3-100 chars), `target_url` (starts with http), optional `description`,
  `max_testers`, `pay_per_session`, `tasks`, `required_nd_categories`.

## Workflows

### Review what a study found
1. `list_studies` to find the study, or take the id the user gives you.
2. `get_study` for the headline (score, tester count, status).
3. `get_insights` with the `study_id` for the findings.
4. Summarize for the user: lead with the highest-severity findings, group by
   theme, and pair each with its recommendation. Keep it business-focused.

### Check one test
1. `get_test` for status and pipeline stage.
2. `get_analytics` for the composite scores.
3. If the user wants detail, `get_insights` with the `test_id`.

### Draft a new study
1. Gather, by asking if needed: what page or app to test (`target_url`), a clear
   `title`, what the client wants to learn (turn this into `tasks`), and how many
   testers.
2. Confirm those details back to the user before creating anything.
3. Call `create_draft_study`.
4. Give the user the returned `dashboard_url` and tell them plainly: the study
   is a **draft**. They review it there and click Launch to recruit testers and
   pay. You cannot launch or pay from here, and nothing has been charged.

## Rules

- Never say a study is "launched", "live", "running", or "paid for" after
  `create_draft_study`. It is a draft until the human launches it in the
  dashboard. Always surface the dashboard link.
- Never invent study or test ids. List first, or ask the user.
- Treat findings as the client's confidential UX data. Summarize faithfully; do
  not embellish severity or invent recommendations the tools did not return.
- When a read returns nothing, say so plainly rather than guessing.
