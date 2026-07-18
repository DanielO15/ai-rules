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

## Step 3: Write the Google Doc entry

Once the user has answered, create a new Google Doc in the learning journal folder using the Google Drive MCP:

```
mcp__23ea1b00-2274-41e2-975d-e1f4dcf9ffc2__create_file({
  title: "<ticket title>",
  parentId: "1U89WfrwsYPyS-GX8Rq0MPnZvCOaj0God",
  textContent: "<formatted entry — see below>",
  contentMimeType: "text/plain"
})
```

This will create a Google Doc (Drive auto-converts plain text to a Doc). Use the ticket title as the document title.

Format the entry as follows — use plain text with clear section headers since Drive will convert it to a Doc:

---

WHAT I BUILT
<1–2 sentences on what the ticket was and what you shipped.>

WHAT I LEARNED
<The user's answers to the reflection questions, written as clear prose. Don't flatten nuance — if they were uncertain about something and now understand it, say that explicitly.>

Where a concept is best illustrated with code, include a short excerpt as a teaching moment. Show the relevant 3–5 lines with a brief comment explaining what it demonstrates. Keep excerpts short — the point is to jog memory, not reproduce the whole diff.

CONCEPTS COVERED
<Any architectural patterns, system design decisions, or codebase conventions that came up — even if you didn't write the code yourself. These are the transferable bits: how the event pipeline is structured, how tracing is wired up, how feature flags are managed, how services communicate. 1–3 bullets. Skip this section if nothing beyond the immediate code came up.>

IMPACT
<1 sentence framing the work in terms of outcome, not output. "Reduced incorrect cohort announcements reaching users" not "fixed a bug in the cohort logic." This is the brag-bucket line.>

---

If the MCP call fails, print the formatted entry so the user can paste it manually and report the error.

## Step 4: Close out

Tell the user the doc has been created and share the link if returned by the MCP. Then ask: "Anything else worth noting before you close this out?"

## Important rules

- Questions must be derived from this specific session — not generic
- Wait for answers before writing the doc
- The Impact line must be outcome-framed, not task-framed — push back if the user gives you a task description
- Do NOT summarise the whole conversation unprompted — only surface what's genuinely useful for reflection
