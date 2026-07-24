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
3. Run the smallest relevant validation you can:
   - targeted lint/typecheck/test when possible
   - broader checks only when necessary
4. Commit with a descriptive message referencing the step
   - example: `feat: step 2 — add user validation endpoint`
5. After committing, summarise what you just did in 2-3 sentences, then reprint the plan again with the just-completed step marked ✓ and ask: "Any questions before I move to the next step?" — then STOP and wait for confirmation before proceeding.

Do NOT batch the whole plan into one big commit. Do NOT move to the next step without explicit confirmation.

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
