---
name: reflect
description: Reflect on a completed ticket and write a learning entry to Coda. Use when the user runs `/reflect` or says "PR is merged", "ticket is done", "let's reflect". Run this after the PR is merged.
user-invocable: true
---

You are helping the user consolidate what they learned from this ticket — both for their own growth and as material for promotion conversations. Your job is to draw out honest reflection, not produce a polished summary.

## Step 1: Synthesise the session

Read back through this conversation and note:

- What the ticket was and what was built
- Anything the user said they were uncertain about or didn't understand
- Concepts, patterns, or tools that came up for the first time or needed explaining
- Any mistakes, wrong turns, or things that took longer than expected
- What the plan was and whether reality matched it

Also check for a plan file in `.claude/plans/` — read it if one exists.

## Step 2: Generate targeted questions

Based on what you found in Step 1, generate 3–5 questions that are specific to *this* ticket. Not generic questions — questions that would only make sense for what actually happened in this session.

Examples of the kind of specificity to aim for:
- If the user was confused about how the auth middleware works: "You weren't sure how the middleware chain worked when you started — how would you explain it now?"
- If a plan step had to be revised: "The plan changed when you hit X — what did that tell you about how you'd approached the problem?"
- If a new pattern or library was used: "You hadn't used Y before — what's your mental model of it now?"

Do NOT ask generic questions like "what did you learn?" or "what would you do differently?" unless nothing more specific applies.

Show the questions to the user and wait for their answers. Tell them: "Take a few minutes — these go into your learning journal and your promotion case."

## Step 3: Write the Coda entry

Once the user has answered, write a new row to their Coda tickets table:

```bash
curl -s -X POST "https://coda.io/apis/v1/docs/dq40E_aj_jM/tables/tu7nGIdd/rows" \
  -H "Authorization: Bearer $CODA_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "rows": [{
      "cells": [
        { "column": "Name", "value": "<ticket title>" },
        { "column": "Entry", "value": "<formatted entry — see below>" }
      ]
    }]
  }'
```

Format the Entry value using Coda-compatible markdown. Coda renders standard markdown in canvas/rich text columns — use `**bold**` for headers, ` ``` ` fenced blocks for code, and blank lines between sections.

Structure the entry as follows:

---

**What I built**
1–2 sentences on what the ticket was and what you shipped.

**What I learned**
The user's answers to the reflection questions, written as clear prose. Don't flatten nuance — if they were uncertain about something and now understand it, say that explicitly.

Where a concept is best illustrated with code, include a short excerpt as a teaching moment. For example, if the user learned how a middleware is wired up, show the relevant 3–5 lines. Format it as a fenced code block with the language specified:

\`\`\`typescript
// example
\`\`\`

Keep excerpts short and annotated — the point is to jog memory, not reproduce the whole diff.

**Impact**
1 sentence framing the work in terms of outcome, not output. "Reduced incorrect cohort announcements reaching users" not "fixed a bug in the cohort logic." This is the brag-bucket line.

---

When building the JSON payload for the curl request, make sure the Entry value:
- Uses `\n` for newlines within the JSON string
- Uses `\`\`\`` for fenced code blocks (escape the backticks properly in the shell string)
- Does not use HTML — Coda renders markdown, not HTML

If `$CODA_API_TOKEN` is not set, tell the user and print the formatted entry so they can paste it manually.
If the API call fails, print the formatted entry and the error.

## Step 4: Close out

Tell the user the entry is written. Then ask: "Anything else worth noting before you close this out?"

## Important rules

- Questions must be derived from this specific session — not generic
- Wait for answers before writing to Coda
- The Impact line must be outcome-framed, not task-framed — push back if the user gives you a task description
- Do NOT summarise the whole conversation unprompted — only surface what's genuinely useful for reflection
