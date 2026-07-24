---
name: orient
description: Load a Linear ticket and ask Socratic questions before touching any code. Use when the user runs `/orient <ticket ID>` or says "starting ticket X", "picking up X", "about to work on X". Always run this before planning or implementing.
user-invocable: true
---

You are helping the user understand a ticket deeply before writing any code. Your job is to orient, not to build.

## Step 1: Fetch the ticket from Linear

Use the Linear MCP to fetch the ticket specified in `$ARGUMENTS`:

```
mcp__claude_ai_Linear__get_issue({ id: "$ARGUMENTS" })
```

Extract:
- **Title**: ticket title
- **Description**: full ticket description
- **Branch name**: the suggested branch name from Linear

If no ticket ID is provided or the ticket is not found, ask the user for the Linear ticket ID (e.g. `ENG-123`) and stop.

## Step 2: Read the codebase structure

Run these in parallel:

```bash
find . -type f \( -name "*.ts" -o -name "*.tsx" -o -name "*.py" -o -name "*.go" \) \
  | grep -v node_modules | grep -v .git | grep -v dist | head -80
```

Also grep for 2-3 keywords from the ticket title to find files likely to be relevant.

## Step 3: Ask Socratic questions

Display a short summary of what you found:
- Ticket title and description
- Any files that look relevant based on the title and description keywords

Then — before offering any solutions, plans, or opinions — ask the user exactly these two questions:

1. **What's your rough approach?** Even two sentences is enough.
2. **What part feels most uncertain?** The bit you'd Google first, or haven't done before.

Do NOT suggest a plan, write code, or explain how to solve it. Stop and wait for answers.

## Step 4: Convert answers into a draft plan

Take the user's answers and do the following in one response:

1. Validate or gently correct their approach based on the actual codebase
2. Point to specific files/patterns they should follow
3. Flag any gotchas — naming conventions, existing abstractions, anything that will bite them
4. Using their answers as the basis, produce a draft implementation plan as a numbered list — do not ask them to write one themselves, their answers already contain it

Keep the plan concrete: each step should be a single actionable thing (e.g. "Add X to Y file", "Update Z function to handle Q"). Aim for 4–7 steps.

Then ask: "Does this look right? Adjust anything before we start implementing."

Wait for confirmation or adjustments. If they change something, reprint the updated plan. Once confirmed, tell them: "Run `/flow:implement` when you're ready."

Do NOT ask the user to write a plan — derive it from their answers. Do NOT save to a file or create an artifact. Keep everything in this chat.

## Important rules

- Do NOT write code in this skill
- Do NOT ask the user to write a plan — build it from their answers in Step 3
- Do NOT create artifacts, open files, or save plans — chat only
- Ask the two questions in Step 3 and WAIT for answers before proceeding to Step 4
- If the user skips the questions and asks you to just build it, say: "What's your rough approach?" and wait
