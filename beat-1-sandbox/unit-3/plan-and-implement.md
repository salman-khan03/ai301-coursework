# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of my plan, the branch I built it on, and the evaluation run that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

---

## Posted upstream

**GitHub username**

salman-khan03

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/54#issuecomment-6093981042

> Plan for #54, following my repro report above (the issue's snippet prints `[]`; the same text with the indentation removed prints `['Education', 'Skills']`; the test file gives `5 passed, 5 xfailed`).
>
> **Cause.** `ingestion/parsers/resume_parser.py` assumes a header starts in column 0 in two places: the four patterns in `_detect_sections()` put the section name straight after `^` or `\n`, and `_strip_markdown()` strips headings with `^#+\s+`. On scratch edits (reverted) I measured that fixing only `_detect_sections()` turns 3 of the 5 `xfail(strict=True)` tests tagged #54 into `XPASS`, and fixing only `_strip_markdown()` turns the other 2, so the fix needs both.
>
> **Change.** Insert `[ \t]*` after the anchor in those five patterns, remove the five #54 xfail markers (they are strict, and CONTRIBUTING asks for the markers to come off with the fix), and add one regression test: indented text detects the same sections as the unindented control, for a 4-space and a tab indent. Files: `ingestion/parsers/resume_parser.py` and `tests/unit/test_resume_parser.py`, on branch `fix/54-resume-section-whitespace` in my fork. I'm using `[ \t]*` rather than `\s*` because the defect is horizontal indentation and `\s` also matches newlines.
>
> **Not in this change.** The `list(set(...))` return order, the `^`/`\n` redundancy between the patterns, `SECTION_HEADERS`, and the PDF extraction path. None of them causes this bug.
>
> **Test plan.** Re-run my repro: the snippet should print Education and Skills instead of `[]` (compared sorted, since the function returns `list(set(...))`), the unindented control should be unchanged, and the test file should go from `5 passed, 5 xfailed` to all passing with no xfail and no `XPASS(strict)`. I'll also confirm the new test fails with the fix set aside, and run the unit suite, ruff, black and mypy.
>
> **What I'm not sure of.** After the change, text indented with non-breaking spaces (U+00A0) is still not detected (I measured that on the scratch edit). I haven't tried a real PDF, so I don't know whether pypdf produces them; I'd leave that to a follow-up. Also, an indented line like `    Skills: Python` nested under another section will now count as a section, the same as an unindented one does today.
>
> **Thread.** No maintainer has commented here yet. My plan overlaps with the earlier ones on the regex change itself, and #100 is open for it. Where I add something is that I measured the per-function split and the non-breaking-space limitation on scratch edits rather than assuming them, and the regression test covers a tab indent as well as spaces. The `[ \t]*` over `\s*` choice is my reasoning, not something I tested. Under the course's house rule I'm posting my own plan rather than pointing at theirs. If a maintainer would rather have a different shape, for example the "strip the line before matching headings" wording in CONTRIBUTING's example commit, I'll follow that instead.
>
> **Status.** I have already built this locally on `fix/54-resume-section-whitespace` (not pushed yet). The snippet now prints `['Education', 'Skills']`, the parser test file is `12 passed`, and `tests/unit` is `382 passed, 48 xfailed`, with ruff, black and mypy clean. Next I'll push the branch to my fork and post the before/after output here.
>
> I'm using Claude Code to help draft this plan and run the checks; I read the code and verified the results myself.

---

## Your branch

**Branch**

fix/54-resume-section-whitespace

**Evidence**

Repro steps from Unit 2, re-run against the built change. `repro54.py` holds the issue's snippet unchanged (the indented input) followed by the same text with the indentation removed (the control). Both runs are from the repo root with the repo's Python 3.11.16 venv on Windows 11 (Git Bash).

Before (unmodified `main`, f89c06f):

```
$ git rev-parse --short HEAD
f89c06f
$ .venv/Scripts/python.exe repro54.py
indented: []
control : ['Education', 'Skills']
$ .venv/Scripts/python.exe -m pytest tests/unit/test_resume_parser.py -rx -q
xx.x..xx..                                                               [100%]
=========================== short test summary info ===========================
XFAIL tests/unit/test_resume_parser.py::TestResumeParser::test_parse_single_column_resume_text - issue #54: ...
XFAIL tests/unit/test_resume_parser.py::TestResumeParser::test_parse_resume_no_work_experience - issue #54: ...
XFAIL tests/unit/test_resume_parser.py::TestResumeParser::test_parse_markdown_resume - issue #54: ...
XFAIL tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections - issue #54: ...
XFAIL tests/unit/test_resume_parser.py::TestResumeParser::test_strip_markdown_syntax - issue #54: ...
5 passed, 5 xfailed in 0.34s
```

After (branch `fix/54-resume-section-whitespace`, commit fa7b725):

```
=== AFTER: repro54.py ===
indented: ['Education', 'Skills']
control : ['Education', 'Skills']

=== AFTER: pytest tests/unit/test_resume_parser.py -rx -q ===
............                                                             [100%]
12 passed in 0.26s

=== NEGATIVE GATE: fix set aside (resume_parser.py reverted), tests kept ===
(empty above = fix is set aside)
FAILED tests/unit/test_resume_parser.py::TestResumeParser::test_parse_single_column_resume_text
FAILED tests/unit/test_resume_parser.py::TestResumeParser::test_parse_resume_no_work_experience
FAILED tests/unit/test_resume_parser.py::TestResumeParser::test_parse_markdown_resume
FAILED tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections
FAILED tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections_ignores_leading_indentation[spaces]
FAILED tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections_ignores_leading_indentation[tab]
FAILED tests/unit/test_resume_parser.py::TestResumeParser::test_strip_markdown_syntax
7 failed, 5 passed in 0.37s
--- restored; fix back in place: ---
 1 file changed, 7 insertions(+), 7 deletions(-)

=== AFTER restore: test file again ===
............                                                             [100%]
12 passed in 0.24s
```

Other checks on the built branch:

```
=== unit suite: pytest tests/unit -m unit -q ===

-- Docs: https://docs.pytest.org/en/stable/how-to/capture-warnings.html
382 passed, 48 xfailed, 2 warnings in 12.79s

=== ruff check . (make lint) ===
All checks passed!

=== black --check on touched files ===
All done! \u2728 \U0001f370 \u2728
2 files would be left unchanged.

=== mypy api/ core/ ingestion/ rag/ agent/ safety/ (make typecheck) ===
Success: no issues found in 76 source files
```

---

## Eval iterations

**Run history**

Run 1 was the only run: a full 20-package run, 20/20 agreement (bar 18/20: PASS), with categories clear-accept 7/7, scope-creep 4/4, thread-convention 2/2, unbuildable 3/3 and wrong-cause 4/4. That is the run saved in `eval-run.txt`; the rubric, evidence guide, procedure and SKILL.md fingerprints in its header match the files uploaded to `tools/plan-check/`. I did not do any partial `--only` runs and did not change any of those files after the run, because there was no disagreement to fix. I ran it with `PYTHONUTF8=1`, the Windows encoding fix from unit 2.

**Package analysis**

pkg-16 (pandas-dev/pandas#57666, `read_csv(engine="pyarrow", dtype=str)` losing leading zeros). Gold label: reject. My rubric: reject. The plan is long and confident, and several of my checks pass it: `scope-bounded` (one change item), `executable-from-plan` (it names `arrow_parser_wrapper.py` and `_finalize_pandas_output`) and `test-plan-decisive` (it expects `000388907` and `0150`). What fails it is the cause. The plan blames the post-read cast, but step 4 of the repro evidence shows pyarrow's inferred table already holds int64 `1` "before any cast to string could run". So `diagnosis-fits-evidence` failed on condition (b), the failure is already present before the blamed component runs. `change-targets-cause` failed too, since re-padding to the column's maximum width cannot recover zeros that no longer exist, and `thread-direction-respected` failed because a MEMBER in the thread had already explained that pyarrow infers a numeric type first. My rubric reads it this way because every check reads the plan's cause against each numbered step in the repro evidence, not against how complete the plan looks.

**Check rationale**

The check, copied from `tools/plan-check/rubric.md`:

| diagnosis-fits-evidence | The plan's stated cause (its diagnosis or cause sentence), read against every numbered step, control run, and artifact in the repro-evidence block | The stated cause explains why the failing step fails AND is compatible with every control and artifact in the repro evidence. Fail if any one of these holds: (a) a control or artifact shows the component the cause blames working correctly (the control that exercises it passes, or the same component produces correct output elsewhere in the same build); (b) the repro evidence shows the failure already present at a point before the blamed component runs (the error is raised earlier, or the data is already wrong before the blamed step); (c) the cause cannot account for the difference between the failing run and a passing control (for example the failing and passing runs differ by version or flag and the blamed code does not depend on it); (d) the plan states no cause, only an intention to find one. A cause taken from the issue or the thread passes when the repro evidence bears it out and fails when it does not. A plausible mechanism that the repro evidence does not directly show passes, as long as nothing in the evidence contradicts it | required |

It reads this way for three reasons. First, I rejected "the cause must be proven by a repro artifact": that would hold plans like pkg-14, where gold accepts a handshake-ordering mechanism that no artifact shows directly, so the check fails only on contradiction and says a plausible mechanism the evidence does not show still passes. Second, I rejected "the cause agrees with the thread": calib-03 is a plan that adopts the thread's confident diagnosis while the package's own timing steps rule it out, so a cause taken from the thread counts only when the repro evidence bears it out. Third, I split a vague "is grounded" into four conditions a grader can test one at a time: the blamed component works in a control (a), the failure appears before it runs (b), the cause cannot explain the failing-versus-passing difference (c), and no cause is stated (d). I wrote those after reading all 24 packages and the notes in `gold-labels.json`, so each condition names a pattern that appears in them, but I phrased them as general patterns and not as package names.

**Trade-offs**

The cost of "fail only on contradiction" is that a plan with a wrong cause that no repro step happens to contradict passes this check. pkg-14's mechanism is the example: the check lets it through because nothing in its evidence contradicts it. I accept that because gold accepts pkg-14, and because `test-plan-decisive` and the stated risks are where such a plan would be found out. I did not loosen any check after the run, so no canaries were needed and nothing else changed: the single full run agreed on all 20 and on every category, and I made no edits afterwards. One gap I noticed when I ran the skill live on my own plan: the procedure's `scope-bounded` gathering question says "the repro's failing step" in the singular, which read literally would mark my second-function change as extra, because the issue's snippet is fixed by the first function alone. The grader resolved it correctly from the rubric's wording ("a failure the repro evidence demonstrates", plus the repo's own requirement to drop every covering marker). I left the procedure as it is because any edit would change the fingerprinted files and need another full run.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; my skill's files in
`tools/plan-check/`.
