# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Repo liveness | The `archived:` flag and `last push to any branch` date under Repo facts (eval mode); the archive banner and newest commit date on the repo front page (live mode). | Pass only if `archived:` is `no` AND the last push to any branch is within 90 days of the capture date (eval mode) or of today (live mode). Fail if the repo is archived, or the last push is more than 90 days before that reference date. | required |
| Unclaimed | The `this issue: assignees:` and `linked PRs:` line under Repo facts, plus the Comments section (eval mode); the Assignees box, Development box, and issue thread (live mode). | Pass only if the assignee list is empty AND no linked PR is in `open` state AND no claim comment in the thread ("I'll take this", "working on this", "can I work on this", "@bot claim", or equivalent) is less than 30 days old relative to the reference date, unless that same claimant was later unassigned or explicitly abandoned it. An automated inactivity-nudge comment (e.g. a bot's "please confirm you're still working on this") does not by itself clear a claim. Fail if any of these signals is present. | required |
| Bounded scope | The issue title, body, labels, and comment thread. | Fail if any of the following observable conditions hold: (1) the body is itself an index/checklist linking 3 or more other issue numbers meant to be split among contributors (an umbrella/tracking issue); (2) the request is open-ended — asks for a change applied "across the codebase" / "wherever it makes sense" / incrementally with no enumerated stopping point; (3) the issue has been open more than 2 years AND has 2 or more closed, unmerged linked PRs in its history, with no maintainer comment in the thread confirming a final settled approach; (4) the requester's own spec contains an explicit unresolved placeholder for a needed design decision ("TBD", "not yet decided", "unclear if X or Y", etc.); (5) the issue was opened by a bot account (username ending in `[bot]`) and no maintainer/owner/collaborator has since confirmed the request is wanted in the thread; (6) the issue is phrased purely as a "how do I get this to work" usage question with no requested change to code or docs. A terse body, missing reproduction steps, or informal writing do NOT by themselves fail this check — grade the size of the requested work, not the polish of the writeup. Pass if none of the above hold. | required |
| AI-contribution policy | The `contribution policy` line under Repo facts (eval mode); `CONTRIBUTING.md`, `AI_POLICY.md`/`AI_USAGE_POLICY.md`, and PR/issue templates (live mode). | Pass if the policy is silent, explicitly welcomes AI-assisted contributions, or only imposes conditions (disclosure, human review, personal understanding/testing requirements) — conditions are terms to follow, not reasons to reject. The presence of an `AGENTS.md` file alone is not a ban. Fail only if the policy states an outright ban on AI-generated code or documentation (e.g. "we do not accept AI-generated code/documentation" or equivalent). | required |
| Newcomer-friendly label | Issue labels. | Pass if labels include `good first issue`, `help wanted`, `easy`, or an equivalent newcomer-facing label. | preferred |
| Actionable detail | Issue body. | Pass if the body names specific affected files/functions, gives reproduction steps or a stack trace, or lists explicit acceptance criteria. | preferred |

## Verdict rule

Accept only if all four `required` checks (Repo liveness, Unclaimed, Bounded
scope, AI-contribution policy) grade `pass`. If any required check grades
`fail`, or grades `unclear` (an `unclear` required check is treated as
`fail`), the verdict is `reject`. `preferred` checks never change the
verdict: report their grades, and on any accepted issue use them, together
with the fit profile in `scope.md`, only to rank accepted candidates against
each other in live mode.
