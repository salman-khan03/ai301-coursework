# Evidence guide: where evidence lives in a plan package

An eval bundle always has the same six parts, in this order: `## Repo facts`, `## Issue`, `## Thread highlights`, `## Repro evidence`, `## Candidate plan`, `## Candidate plan comment`. The candidate plan's own sub-headings vary between bundles (`### Diagnosis` or `### Problem statement` or a run-on paragraph beginning `Diagnosis:`; `### Scope`; `### Files`; `### Approach` or `### Changes` or `### Proposed changes`; `### Test plan`; sometimes a `Risk:` line). Find the part by what it says, not by its heading.

In live mode the same parts live in these places:

- Repo facts: the repo's `docs/CONTRIBUTING.md` (or `CONTRIBUTING.md`), the pull request template under `.github/`, any `AI_POLICY.md` or "AI" section, and `docs/SETUP.md`.
- Issue and thread: `gh issue view <number> --repo <owner/repo> --comments`, or `gh api repos/<owner>/<repo>/issues/<number>/comments`. The `author_association` field on each comment gives the role (OWNER, MEMBER, COLLABORATOR, CONTRIBUTOR, NONE). Linked pull requests: `gh pr list --repo <owner/repo> --search "<number>"`.
- Repro evidence: the student's own posted repro comment on the issue (find it by their GitHub username). Not another commenter's. A student on the house issue uses the house repro pack as it is quoted in their drafts.
- Candidate plan: the student's `plan.md`. Candidate plan comment: the student's `comment.md`. The package is what these two files contain and quote, not other files in the working directory.

## Diagnosis and grounding

- Where it lives: In an eval bundle, the plan's cause is the sentence under `### Diagnosis` (or `Diagnosis:`, `Cause:`, `### Problem statement`, or the first paragraph of the plan). The evidence it must explain is the whole `## Repro evidence` block: the numbered Steps, each Control run, each artifact (console output, timings, logs), and the closing Expected and Actual lines. In live mode, the cause is the Diagnosis section of `plan.md`; the evidence is the student's posted repro comment (its Steps, Control, Observed, and Conclusion).
- What good looks like: The cause names a specific component and says why that component produces the failure shown. Every control in the repro evidence is consistent with it: the control that passes is the one where the blamed condition is absent. A cause is grounded when you can point at a repro item that bears it out. A cause is contradicted when a repro item shows the blamed component working, shows the failure before that component runs, or shows a failing run and a passing run that the blamed code cannot tell apart. A cause copied from the issue or the thread is fine if the repro evidence bears it out, and is not fine just because a commenter was confident.

## Scope

- Where it lives: In an eval bundle, the plan's `### Scope` section, or the in-scope and not-in-scope sentences in the diagnosis paragraph; the numbered change list under `### Approach`, `### Changes`, or `### Proposed changes`; and the `### Files` or "Files and areas" lines. Read these against the issue's `Expected` and `Actual` and the repro evidence's failing step. In live mode, the Scope and Files sections of `plan.md`.
- What good looks like: One bounded change: each numbered item is either the fix for the reproduced failure, a test for it, or a required follow-through. A bounded plan can be one line long. An unbounded plan is easy to spot by its verbs: replace, rebuild, migrate, upgrade, unify, introduce, restructure, add an option, add a retry layer, add a CI matrix. A plan that defers a bigger rework by name and gives a reason is bounded, not narrow.

## Executability

- Where it lives: In an eval bundle, the `### Files` lines (or `Files:`), the numbered `### Approach` or `### Changes` steps, and any ordering words (first, then). In live mode, the Files and Approach sections of `plan.md`.
- What good looks like: A stranger could start without asking. The plan names where to work (a file, a module, a code path) and what to do there (a described edit). Decisions that remain are small and are named as open. A plan is not executable when its steps are search steps (investigate, look into, figure out, experiment, "somewhere", "whichever is easier", "maybe also") and when the change itself is chosen later.

## Test plan

- Where it lives: In an eval bundle, the plan's `### Test plan` section or `Test plan:` paragraph, read against the repro evidence's numbered Steps and its `Expected:` line. In live mode, the Test plan section of `plan.md`, read against the student's repro comment.
- What good looks like: It re-runs the repro's failing step and names the result to expect after the fix, in the same terms the repro used (the output text, the exit code, the visible state), and says what the unchanged control should still do. It could fail: unfixed code would give a different result. A vague test plan names a feeling ("should feel fast"), a whole suite ("run the full test suite"), or a different behavior than the one that failed.

## Honesty

- Where it lives: In an eval bundle, the plan's `Risk:` or "Risks" lines, any "unknown", "open question", "I have not verified", or "deferred" sentences in the plan, and the matching sentences in `## Candidate plan comment`. In live mode, the Risks and unknowns section of `plan.md`, and the Deviations section at the end of it (where an honest mid-build change is recorded).
- What good looks like: Unknowns are named where the evidence stops, and certainty is claimed only where a repro artifact supports it. A plan that says "root cause", "guaranteed", or "should be a small PR" without an artifact behind it is dressing an unknown as a fact. A plan that lists what it could not test, or a variant it is leaving out and why, is being honest. Not every plan has an unknown: "none identified" after naming the area looked at is acceptable.

## Comms

- Where it lives: In an eval bundle, `## Candidate plan comment`, read against `## Thread highlights` (each commenter's association is in parentheses after their name) and the `contribution policy` line in `## Repo facts` (the AI-use sentences, and which surface they apply to). In live mode, `comment.md`, read against the live thread (`author_association` per comment, named PRs) and the repo's CONTRIBUTING and AI policy text.
- What good looks like: The comment says which part of the thread it is answering. Where a maintainer isolated a cause, chose an approach, rejected one, or asked for a test, the comment follows that or says why not. Where the thread names an open PR or earlier fix, the comment says whether it builds on it, defers to it, or differs. For AI disclosure: if the policy requires it for comments or for all contributions, the comment names the tool and how much it helped; if the policy only asks for human understanding, own words, or PR-level disclosure, no statement is required. Boilerplate that could be pasted on any issue ("I'd like to work on this, assign me") is not thread-aware.
