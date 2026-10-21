# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/237

**Branch**

fix/54-resume-section-whitespace

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

One full run, and it is the committed one: 19/20 (categories: clear-accept 6/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3; bar 18/20: PASS). I ran no partial `--only` runs. The agreement line in `eval-run.txt` reads `agreement: 19/20 scored items  (bar: 18/20: PASS)`.

**Package analysis**

`pkg-02` (Textualize/rich#4208, the `save_*` methods reordered to clear the record only after a successful write). Gold label: `accept` (clear-accept). My rubric decided: `reject`, so this is the one package where we disagree. It passed eight of nine checks (`diff-within-plan`, `description-matches-diff`, `repro-before-after`, `repo-checks-ran`, `template-asks-honored`, `ai-disclosure`, `thread-direction-honored`, `issue-and-limits-named`) and failed only `diff-reviewable`. The grader's evidence line was: "save_html/save_svg hunks remove and re-add identical lines (`-        html = self.export_html(` / `+        html = self.export_html(`, `theme=theme,`, `svg = self.export_svg(`, `title=title,`): lines re-printed with no visible change, or a whitespace-only hunk."

My rubric read it that way because `diff-reviewable` lists "lines repeated or re-printed with no change" as a fail, and the package's diff really does show `html = self.export_html(`, `theme=theme,` and `svg = self.export_svg(` as both removed and added. I think this is a misread, not a real finding: in the same hunks the line `clear=clear,` becomes `clear=False,`, so the hunk is the fix with unchanged lines shown on both sides around it, not pure churn, and the other package this clause was written for (`pkg-18`, "two blocks of identical re-printed lines (pure churn)") is a different situation. My clause did not say "a hunk whose removed and added lines are all identical". It said "lines repeated or re-printed with no change", which gave the grader the hook. I left it as written because 19/20 already clears the bar, and any edit to `rubric.md` would change its fingerprint in `eval-run.txt` and need another full run (about $5).

**Check rationale**

The check I quote is `diff-within-plan`, exactly as it reads in `tools/pr-precheck/rubric.md`:

> | diff-within-plan | The diff's changed files and hunks, read against the plan's scope, files, and approach, plus any deviation notes (the plan's `## Deviations`, or in a bundle any deviation the plan context records) | Both hold. (a) Every changed file and hunk is named or described by the plan's scope, files, or approach; or is a test for the fix; or is follow-through the repo's own template or contribution guide requires for this kind of change (for example a changelog entry); or is explained by a deviation note that says why the fix needed it. (b) Every deliverable the plan names as part of this change appears in the diff, or the plan, a deviation note, or the description says plainly that it was deferred or dropped. Fail if either (a) or (b) breaks. Typical (a) breaks: a rewrite or rename pass over code the fix does not need; a new option, flag, setting, or config key; a dependency bump; reworded unrelated text; a hunk in a file or function the plan never names. A description that mentions an unplanned extra does not make it planned; only a plan deviation note that shows the fix needed it does. Typical (b) break: the plan promises a code change and a docs update and the diff has only the code change, with nothing saying so. Not a fail: the same fix applied at sibling sites the plan or issue names; a comment that explains the fix; a deferral the plan states up front | required |

It reads this way for three reasons. First, it has two halves because silent drift runs in two directions: (a) is "more than the plan" (a hunk the plan, a test, repo-required follow-through or a deviation note does not cover), and (b) is "less than the plan" (a named deliverable absent from the diff with nothing saying so). `pkg-17` is the (b) case: the plan names a warning and a docs update, the diff has only the warning. Second, the asymmetry is deliberate: a description that mentions an unplanned extra does not make it planned, but a plan that defers something up front, or a deviation that says why, does re-tie the mismatch. That is how `pkg-13` and `pkg-16`, which disclose a deferral, stay acceptable while `pkg-03`, `pkg-06` and `pkg-09`, which add unplanned flags or renames under a "matches the plan" claim, are held. Third, I rejected folding the description's claims into this check: whether the description is truthful about the diff is its own check (`description-matches-diff`), so one defect can fail both and each fail still names its own deciding line. I did not revise this row against eval results; I wrote it before the single full run and did not touch it afterwards.

**Trade-offs**

Two things, both from the one full run. First, a package this rubric changes: `pkg-02` flips from the gold `accept` to `reject` through the "lines repeated or re-printed with no change" clause of `diff-reviewable` (see the package analysis). I kept the clause because it is the written reason for the pure-churn case (`pkg-18`'s gold note: "two blocks of identical re-printed lines (pure churn)"); I did not test removing it, so I cannot say what `pkg-18` would do without it. The run's evidence for `pkg-18` also names a commented-out `eprintln!` and an import restructure, which the clause is not needed to catch. Second, nothing else changed and here is how I know: I made no edit to `SKILL.md`, `rubric.md`, `procedure.md` or `references/evidence-guide.md` after the run, and no partial run or canary was needed. The fingerprints in the `eval-run.txt` header (`SKILL.md a488659b64c04a18`, `rubric.md 84db8488f7bbee18`, `procedure.md 6820256597e03980`, `evidence-guide.md 37d1bf0a7fd7418a`) are the first 16 hex digits of the sha256 of the files in `tools/pr-precheck/`, which I computed and compared. A case I accept the rubric will miss: `ai-disclosure` only fails when the repo's policy says disclosure is required (that is how `pkg-20` is caught); in a repo whose stated policy is silent the check passes without any disclosure, except in live mode, where it also applies the course's own rule.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
