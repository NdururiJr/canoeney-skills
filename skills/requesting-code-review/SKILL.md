---
name: requesting-code-review
description: Use when completing tasks, implementing major features, or before merging to verify work meets requirements
---

# Requesting Code Review

Ask a reviewer to check the work and catch issues before they cascade. The reviewer gets precisely crafted context for evaluation — never your session's history.

**Core principle:** Review early, review often.

## When to Request Review

**Mandatory:**
- After completing major feature
- Before merge to main

**Optional but valuable:**
- When stuck (fresh perspective)
- Before refactoring (baseline check)
- After fixing complex bug

## How to Request

**1. Get git SHAs:**
```bash
BASE_SHA=$(git rev-parse HEAD~1)  # or: git merge-base origin/main HEAD
HEAD_SHA=$(git rev-parse HEAD)
```

**2. Send the request:**

Send the reviewer a short request, the way your role file says. Who reviews the work is set by your role file. The request holds:
- what changed: the branch, and `{BASE_SHA}`..`{HEAD_SHA}`, with a brief summary (`{DESCRIPTION}`);
- the requirements it implements (`{PLAN_OR_REQUIREMENTS}`: a plan file path, task text, or requirements);
- the checklist to apply: `code-reviewer.md` in this skill's folder.

A message holds at most 2000 characters; longer text belongs in a file on your branch that you name in the message. If the reviewer does not have this skill, put the checklist from `code-reviewer.md` in such a file and name it.

**3. Act on feedback:**
- Fix Critical issues immediately
- Fix Important issues before proceeding
- Note Minor issues for later
- Push back if reviewer is wrong (with reasoning)

## Example

```
[Just completed Task 2: Add verification function]

You: Let me request code review before proceeding.

BASE_SHA=$(git log --oneline | grep "Task 1" | head -1 | cut -d' ' -f1)
HEAD_SHA=$(git rev-parse HEAD)

[Send the reviewer a short request, the way your role file says]
  Changed: branch <agent id>/index-repair, a7981ec..3df7661 — added verifyIndex() and repairIndex() with 4 issue types
  Requirements: Task 2 from docs/superpowers/plans/deployment-plan.md
  Checklist: code-reviewer.md from requesting-code-review

[The reviewer replies]:
  Strengths: Clean architecture, real tests
  Issues:
    Important: Missing progress indicators
    Minor: Magic number (100) for reporting interval
  Assessment: Ready to proceed

You: [Fix progress indicators]
[Continue to Task 3]
```

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "I'll just review the diff myself instead of asking a reviewer" | Same author, same blind spots. A reviewer who did not write the change sees what you cannot, and only the findings come back to you. |
| "The reviewer needs my whole session history to understand the change" | Hand it precisely crafted context, never your session's history. That keeps the reviewer on the work product, not your thought process. |

## Red Flags

**Never:**
- Skip review because "it's simple"
- Ignore Critical issues
- Proceed with unfixed Important issues
- Argue with valid technical feedback

**If reviewer wrong:**
- Push back with technical reasoning
- Show code/tests that prove it works
- Request clarification

See the reviewer's checklist in `code-reviewer.md` in this skill's folder.
