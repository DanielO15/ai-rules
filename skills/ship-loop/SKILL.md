---
name: ship-loop
description: Run the post-implementation sequence — self-review, PR creation, then review loop. Use when the user runs `/flow:ship-loop` or asks to "ship this", "wrap up and PR", or after `/flow:implement` finishes.
user-invocable: true
---

Do the following sequentially. After each step, report what you did.

## Step 1: Self-review

Run `/flow:self-review` and fix any blocking issues found before proceeding.

## Step 2: Create the PR

Run `/flow:pr` to create the draft pull request.

## Step 3: Review loop

Run `/flow:review-loop` to iteratively review and fix the PR until it is ready.

## Step 4: Remind to reflect

Once the PR is open and the review loop is complete, tell the user:

"Once your PR is merged, run `/reflect` to log what you learned and add it to your Coda journal."

## Important rules

- This command is an orchestrator. Keep progress updates concise.
- Do not duplicate heavy validation if a previous step already completed it successfully.
