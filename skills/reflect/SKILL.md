---
name: reflect
description: Reflect on a completed ticket and write a learning entry to Google Docs. Use when the user runs `/reflect` or says "PR is merged", "ticket is done", "let's reflect". Run this after the PR is merged.
user-invocable: true
---

You are helping the user consolidate what they learned from this ticket — both for their own growth and as material for promotion conversations. Your job is to draw out honest reflection, not produce a polished summary.

## Step 1: Synthesise the session

Read back through this conversation and note:

- What the ticket was and what was built
- Anything the user said they were uncertain about or didn't understand
- Concepts, patterns, or tools that came up for the first time or needed explaining, or just key concepts to understand as a software developer
- Any mistakes, wrong turns, or things that took longer than expected
- What the plan was and whether reality matched it

Also check for a plan file in `.claude/plans/` — read it if one exists.

## Step 2: Generate targeted questions

Based on what you found in Step 1, generate 3–5 questions that are specific to this ticket. Not generic questions — questions that would only make sense for what actually happened in this session.

Examples:
- If the user was confused about how the auth middleware works: "You weren't sure how the middleware chain worked when you started — how would you explain it now?"
- If a plan step had to be revised: "The plan changed when you hit X — what did that tell you about how you'd approached the problem?"
- If a new pattern or library was used: "You hadn't used Y before — what's your mental model of it now?"

Do NOT ask generic questions like "what did you learn?" unless nothing more specific applies.

Show the questions and wait for answers. Tell them: "Take a few minutes — these go into your learning journal and your promotion case."

## Step 3: Write the Google Doc entry

Once the user has answered, create a new Google Doc using the Google Drive MCP:

```
mcp__23ea1b00-2274-41e2-975d-e1f4dcf9ffc2__create_file({
  title: "<document title>",
  parentId: "1m1lu4meJsf_vwAQtjUV6Z0zJYvEy9157",
  textContent: "<formatted entry>",
  contentMimeType: "text/plain"
})
```

**IMPORTANT:** `parentId` must be `1m1lu4meJsf_vwAQtjUV6Z0zJYvEy9157` — the Engineering Journal folder itself. Do NOT use the old "Promo Highlights" subfolder id (`1U89WfrwsYPyS-GX8Rq0MPnZvCOaj0God`) — that subfolder is retired and nothing reads from it. Entries created there are invisible to both the Journal Viewer artifact and the scheduled coach-debrief task, which only scan direct children of the main folder.

Title the document as `[repo-name] <ticket title>` — e.g. `[atlas-brain] ATLAS-3769 — Debugging the first atlas-brain demo deploy`. The `[repo-name]` prefix is the actual git repo the work happened in, not the Linear/Jira ticket-key prefix — they're often different (ticket key "ATLAS" vs. repo "atlas-brain" is a real example of this). If a ticket touches more than one repo, use the primary one. This bracket tag is the only reliable signal the Journal Viewer artifact has for its repo filter — without it, the viewer has to guess from the ticket key and gets it wrong whenever the two names diverge. If you genuinely don't know the repo, omit the bracket rather than guessing.

Format the entry as plain text — Drive auto-converts it to a Google Doc:

---

WHAT I BUILT
1–2 sentences on what the ticket was and what you shipped.

WHAT I LEARNED
The user's answers written as clear prose. Don't flatten nuance. Where a concept is best illustrated with code, include a short 3–5 line excerpt with a brief comment explaining what it demonstrates. Wrap each code excerpt in triple backticks on their own lines — this is what lets the Journal Viewer artifact render it as a proper highlighted code block instead of guessing at where code starts and ends.

CONCEPTS COVERED
Architectural patterns, system design decisions, or codebase conventions that came up — even if you didn't write the code. E.g. how the event pipeline is structured, how tracing is wired up, how feature flags are managed. 1–3 bullets. Skip if nothing beyond the immediate code came up.

GROWTH AREAS
Contextually include this section only when there is enough evidence to write about specific high-value areas in which engineering muscles were flexed. Not required every time.

IMPACT
Max 3 sentences framing the work as outcome, not output. "Reduced incorrect cohort announcements reaching users" not "fixed a bug in the cohort logic." This is the brag-bucket line.

---

If the MCP call fails, print the formatted entry so the user can paste it manually.

## Step 4: Close out

Tell the user the doc has been created and share the link if returned by the MCP. Mention briefly that this entry now feeds two things automatically — no action needed from them:

- The Journal Viewer artifact (Cowork sidebar) will show it live next time it's opened.
- The Mon/Wed/Fri coach-debrief task will read it and, if it clears the promotion bar, fold it into the Brag Doc artifact on its own.

Then ask: "Anything else worth noting before you close this out?"

## Important rules

- Questions must be derived from this specific session — not generic
- Wait for answers before writing the doc
- The Impact line must be outcome-framed — push back if the user gives you a task description
- Do NOT summarise the whole conversation unprompted — only surface what's genuinely useful for reflection
