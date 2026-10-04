# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/54

**Verdict output**

```
Ranked read-out: accepted. #54, Resume section detection fails on text with leading whitespace.
- Repo liveness: pass. Not archived, last push 2026-09-16 (18 days ago).
- Unclaimed: pass. No assignees or linked PRs. Ten classmate claim and repro comments were ignored per the Path Review house rule.
- Bounded scope: pass. One bug in `_detect_sections()`, with a repro and three named failing tests. It was opened by the maintainer and has no disqualifying signals.
- AI-contribution policy: pass. `docs/CONTRIBUTING.md` and the PR template have no AI ban.
- Newcomer-friendly label (preferred): pass. `good first issue`.
- Actionable detail (preferred): pass. It names the function and file, a repro snippet and the failing tests.

Fit: it's a small Python bug fix in an unfamiliar ingestion codebase, with a clear repro and tests that define "done".

{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/54",
  "checks": [
    {"name": "Repo liveness", "grade": "pass", "evidence": "archived: false; pushedAt 2026-09-16, 18 days before 2026-10-04"},
    {"name": "Unclaimed", "grade": "pass", "evidence": "No assignees, no linked PRs; 10 classmate claim/repro comments ignored per Path Review house rule"},
    {"name": "Bounded scope", "grade": "pass", "evidence": "Single bug in _detect_sections() with repro and 3 named failing tests; maintainer-opened, no umbrella/placeholder/usage-question signals"},
    {"name": "AI-contribution policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PR template contain no AI ban; no AI_POLICY/AGENTS files"},
    {"name": "Newcomer-friendly label", "grade": "pass", "evidence": "Labels: bug, good first issue, ingestion, tier-1"},
    {"name": "Actionable detail", "grade": "pass", "evidence": "Names _detect_sections() in resume_parser.py, repro snippet, failing test names"}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**
Run 1: partial run, not scored. The harness crashed on 7 of the 20 items with a Windows UnicodeEncodeError, because it encodes prompts as cp1252. The 13 items that did run all agreed with the gold labels. Run 2: 20/20 agreement, after setting PYTHONUTF8=1. I didn't change the rubric between runs. Run 2 is the one saved in eval-run.txt.

**Issue analysis**

Issue: issue-05 (sympy/sympy#28806, "Adding more type annotations to the codebase")
My rubric verdict: reject
Gold label: reject
Reasoning: The issue has the good first issue and Easy to Fix labels, and the repo is healthy (last push the day before capture, no AI ban, no assignee). But the body asks contributors to incrementally add type annotations to parts of the codebase "where it makes sense", with no list of files and no stopping point. That is open-ended scope, so it fails my Bounded scope check on condition (2). The check is a required one, so the verdict is reject however good the labels look.

**Check rationale**

Check (Bounded scope), copied from my rubric.md: "Fail if any of the following observable conditions hold: (1) the body is itself an index/checklist linking 3 or more other issue numbers meant to be split among contributors (an umbrella/tracking issue); (2) the request is open-ended — asks for a change applied "across the codebase" / "wherever it makes sense" / incrementally with no enumerated stopping point; (3) the issue has been open more than 2 years AND has 2 or more closed, unmerged linked PRs in its history, with no maintainer comment in the thread confirming a final settled approach; (4) the requester's own spec contains an explicit unresolved placeholder for a needed design decision ("TBD", "not yet decided", "unclear if X or Y", etc.); (5) the issue was opened by a bot account (username ending in `[bot]`) and no maintainer/owner/collaborator has since confirmed the request is wanted in the thread; (6) the issue is phrased purely as a "how do I get this to work" usage question with no requested change to code or docs. A terse body, missing reproduction steps, or informal writing do NOT by themselves fail this check — grade the size of the requested work, not the polish of the writeup. Pass if none of the above hold."

Why it is written this way: it is a list of conditions I can point to in the text ("wherever it makes sense", a list of 3 or more linked issues, a "TBD"), not a judgment of whether the issue feels small, so the same issue gets the same grade each time. The sentence "A terse body, missing reproduction steps, or informal writing do NOT by themselves fail this check" keeps short maintainer-written issues from being rejected just for being brief.

**Trade-offs**

This check only looks at wording and history, so it can miss two kinds of issue. It can pass an issue that sounds bounded but is hard in practice. For example, #59 in Path Review says the checker's support "should not depend on shared wording" and gives no replacement approach. It passes, because no explicit placeholder is written into the spec, but I'd expect the work to grow. It can also reject an issue that is open-ended in wording but easy in practice, like an "incrementally, start small" cleanup where one small PR would be welcome. I accepted that, because for a first contribution a wrongly rejected issue costs me little, since there are other candidates. A wrongly accepted one costs me days. The check also has no vague "too hard" condition, so it only fails on things I can quote from the issue.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. Fit: I know Python, and #54 is a small bug with a clear repro and three failing tests that define when it's done. It lets me practice reading an unfamiliar ingestion module and making a fix a reviewer will accept, without needing deep domain knowledge.
2. Rubric vs. me: The rubric correctly checked that the repo is live, nobody is assigned, there is no linked PR, there's no AI ban, and the scope is one function. It couldn't tell me how well I understand regex or text parsing, or how interesting the work is. Those were my judgment, and so was picking #54 over the docs-only #47.
3. Claiming difficulty: Probably high. #54 has 10 comments, and many classmates have already posted repro reports. The house rule means claims don't block anyone, so I expect several overlapping PRs. I plan to claim early with a short plan and focus on a clean PR with tests instead of being first.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
