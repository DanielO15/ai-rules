---
name: implement
description: Execute an approved plan step by step, with small commits and targeted validation. Use when the user runs `/flow:implement <plan>` or asks to "execute the plan", "implement the plan in .claude/plans/...", or "start working on this plan".
user-invocable: true
---

Do the following sequentially. After each step, report what you did.

## Step 1: Read and understand the plan

Use the confirmed plan from this conversation — it will be the numbered list agreed at the end of `/flow:orient`. If no plan is visible in the conversation and no file was passed as `$ARGUMENTS`, ask the user to paste the plan before proceeding.

Print the full plan as a numbered list so the user can see exactly what will be executed.

If the plan is ambiguous or clearly wrong based on the codebase, stop and ask the user before proceeding.

## Step 2: Pre-flight checks

Before making any changes:

1. Run `git status`. The working tree must be clean — no uncommitted or unstaged changes. If it isn't, stop and ask the user how to proceed.
2. Run `git branch --show-current`. If on `main` or `master`, create a feature branch:
   - Look for a Linear ticket reference in the plan file or the conversation.
   - If a Linear ticket is found, fetch it with the Linear MCP (`mcp__claude_ai_Linear__get_issue`) and use the `branchName` field from the response as the branch name — this matches the branch name suggested in the Linear UI.
   - If no Linear ticket is found, derive a descriptive branch name from the plan.
   - Switch to the new branch before continuing.

## Step 3: Implement step by step

Follow the plan in order, one step at a time.

For each step:

1. Before starting, reprint the plan with completed steps marked ✓ and the current step marked →
2. Implement the changes described
3. Before committing, show the code context using the template below
4. Run the smallest relevant validation you can:
   - targeted lint/typecheck/test when possible
   - broader checks only when necessary
5. Show the code context template, then tell the user: "I've made the changes — take a look in your editor and come back when you're happy." STOP and wait.
6. Only commit once the user confirms in chat (e.g. "looks good", "okay", "commit it"). Do NOT commit before receiving explicit confirmation.
7. After committing, reprint the plan with the just-completed step marked ✓ and ask: "Any questions before I move to the next step?" — then STOP and wait for confirmation before proceeding.

Do NOT batch the whole plan into one big commit. Do NOT commit without explicit user confirmation in chat. Do NOT move to the next step without explicit confirmation.

### Code context template (before each commit)

```
### Step N: [step name]

**Code written:**
[Read the modified file(s) and show the actual lines — the complete function or key snippet, with 1-2 lines of surrounding context. Use the correct language tag.]

**What it does:**
[2-3 sentences. Be direct and technical — assume the reader knows the domain.]

**Why it's solved this way:**
[1-2 sentences on the constraint or design decision that drove this approach. E.g. "This lives in the middleware because auth state is immutable by the time it reaches route handlers."]

**Related code:**
- [function/file]: [why this relationship matters]
- [function/file]: [why this relationship matters]
```

### When to skip code context

Skip for:
- Pure config/YAML changes — just describe what was added
- Trivial one-liners or variable renames
- Deletions with no new logic

Still show code for anything with logic, even if it's small.

## Step 4: Final verification

Once all steps are complete:

- Run the appropriate broader validation for the changed areas
- Run the main test suite if appropriate for the repo and scope of the change
- Sanity-check that the implementation still matches the plan
- Fix any issues found

## Step 5: Generate tests

For each new or significantly changed function, run `/gen-tests <function name>`.
Skip this step only if the change is purely config, docs, or a trivial one-liner with no logic.

## Step 6: Launch ship loop

Run `/flow:ship-loop`.

## Important rules

- Follow the plan.
- If you think the plan is wrong, stop and ask rather than silently deviating.
- Prefer targeted checks during implementation over rerunning the entire repo after every small step.
