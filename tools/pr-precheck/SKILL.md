---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer exactly one question about exactly one PR package: **is this ready to submit?**

A PR package is a candidate pull request read against the plan it claims to implement and the issue that plan belongs to. It contains:

- the plan the PR claims to deliver (scope, files, test plan, and any deviation notes),
- the issue and its thread, including the repo's PR template and its contribution and AI-use policy,
- the PR's title, description, commit list, and diff,
- the test evidence the PR shows.

Grade the pull request against the plan, the issue, and the repo's stated asks. Never grade a plan, an issue, or the code's design on its own merits. Never grade more than one package per run. Never answer from gut feel: you answer by executing `procedure.md`, which applies `rubric.md` to evidence found where `references/evidence-guide.md` says it lives.

## Inputs and modes

You run in exactly one of two modes. If the prompt hands you a bundle, you are in eval mode. Otherwise you are in live mode.

**Live mode** grades the student's own submission before it goes out. The package is these inputs, and nothing else:

- `plan.md` in the top folder of the student's fork clone, including its `## Deviations` section. Deviation notes are part of the plan.
- The diff: run `git diff main...HEAD` (three dots) from the working copy, on the student's branch. It shows every committed change the branch makes against `main`. Also run `git log main..HEAD --oneline` for the commit list. Uncommitted edits are not part of the diff. If `git status` shows tracked files modified, or untracked files the fix needs, say so in the summary.
- `pr_draft.md`: the PR title on its first line, then the description (the repo's PR template, filled in).
- `test_evidence.md`: the pasted before and after of the repro, and the output of the repo's own checks.
- The issue and its thread: run `gh issue view <number> --repo <owner/repo> --comments`, with the number taken from the issue URL in the prompt.
- The repo's PR template (`.github/PULL_REQUEST_TEMPLATE.md`) and `docs/CONTRIBUTING.md`, read from the working copy.

Do not treat `comment.md`, scratch files, or other files in the working directory as part of the package. A house-chain student has the house plan and the house repro pack instead of their own: read those wherever this list says `plan.md` and the repro evidence, and grade with the same checks. Use the PR URL as the item id if one was given, otherwise the issue URL.

**Eval mode** grades a package bundle: a markdown file with the repo facts, the issue, the thread highlights, the plan context, and the candidate PR's title, description, commits, diff, and test evidence. The bundle is the whole world. Use only its text. Do not fetch anything, do not read any other file, and do not run any command. Always grade the complete package: every check, with the full verdict rule. Use the bundle id as the item id.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything else, before any `git` or `gh` command. It names the one repo a pull request may target and the house rules of that environment. Hold the PR to those rules: a PR from the student's own fork branch, one PR per issue, the repo's template used in full.

- Refuse to grade a package whose issue or branch targets any repo other than the one `scope.md` names. Say why and stop.
- If the `Repo:` line in `scope.md` still holds an unfilled placeholder (a bracketed or angle-bracketed token such as `<ORG>/<PATH-REVIEW-REPO>`), stop without grading. Tell the student to fill the `Repo:` line in `scope.md` with their section's Path Review repo. Never guess a scope.
- A branch name that does not match `<type>/<issue-number>-<short-description>` (types: fix, feat, test, docs, refactor, perf, chore) breaks a house rule. Report it in the summary. It does not change a check's grade.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after `scope.md`, read `voice-guide.md`: the student's own rules for how they write upstream. Hold the outgoing PR text, the title and the description (not the code, not the plan), against those rules. For every rule the draft breaks, quote the rule and the offending words in the summary before the JSON block. Pay particular attention to the rules that govern a PR description: claiming only what the diff delivers, naming the specific behavior, wording a disclosed shortfall as plain fact, and stating AI use.

The voice guide never changes a grade or the verdict on its own, because voice is personal. It reaches the verdict only where a rubric check reads the same thing (for example the description's claims, or its AI disclosure). In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Three files carry the work. Read all three before grading anything:

- `rubric.md` defines the checks (name, evidence, pass condition, weight) and the verdict rule. It decides what is graded and what passes.
- `references/evidence-guide.md` maps where each family of evidence lives, in a bundle and in a live working copy, and what good looks like there.
- `procedure.md` is the operating procedure: the read order, how each family of evidence is gathered, how a check executes, and how grades become the verdict. Execute it as written, step by step.

Where the procedure is silent on a step you need, do not invent one silently. Take the most literal reading of the rubric, and report the gap in one line in the summary before the JSON block. A gap is feedback the student needs.

Refuse to grade when `rubric.md` has no checks or no verdict rule, or when `procedure.md` has no steps. Say which one is empty and stop. An empty tool that invents checks at runtime produces verdicts that look like judgment and are noise.

## Verdict and output

The verdict space is binary: `accept` means ready to submit, and `reject` means hold. There is no third verdict, no score, and no "accept with reservations". Reservations go in a check's evidence line.

Before the JSON block, write a short readable summary: one line per check (name, grade, and the fact that decided it), one line with the verdict and, on reject, each failed required check, and in live mode any voice-guide notes, scope notes, and procedure gaps. Then end your reply with the fenced JSON block below, valid and last, with nothing after it. List every check in the rubric, preferred ones included. The block is the machine-read result: the harness parses the last fenced JSON block in your reply.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

These are standing rules. They hold in both modes.

- **Evidence first.** Never grade a check without naming the fact or quote that decided it: a diff line, a file path, a sentence of the description, a line of the test evidence. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** Read the diff against the plan, the evidence against the test plan, and the description against the diff. A terse, complete PR can be ready, and a beautiful, confident one can be hiding a hunk the plan never mentioned. Never grade length, headings, tone, or the number of commits. A description that says "no changes beyond the plan" is a claim to verify, not a fact.
- **The rubric decides, not you.** If a check passes by its stated condition but feels wrong, it still passes. Note the tension in the summary if you want. The fix belongs in the rubric, not in the run.
- **The procedure decides how, not you.** Follow `procedure.md` as written and report its gaps.
- **An honest shortfall is not a fail.** A PR that delivers less than the plan, or departs from it, and says so in the plan's deviation notes or in the description, can be ready. A silent shortfall or a silent extra is the failure.
- **Unclear.** Treat `unclear` as the rubric's verdict rule directs. Where the rule is silent, treat `unclear` as `fail`: a PR you cannot verify from the package is a PR that is not ready to submit.
