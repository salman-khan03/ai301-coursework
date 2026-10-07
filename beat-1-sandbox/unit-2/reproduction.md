# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of my claim and reproduction on the issue I chose in Unit 1 (codepath/pathreview-ai301-fa26-s1 #54, "Resume section detection fails on text with leading whitespace"), and of the evaluation runs that produced `eval-run.txt`.

---

## Your identity upstream

**GitHub username**

salman-khan03

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/54#issuecomment-6048859725

> I'd like to investigate this issue. This is my first contribution to PathReview, so I'm starting with the reproduction rather than a fix.
>
> I haven't run anything yet. From the issue and `ingestion/parsers/resume_parser.py`, `_detect_sections()` builds its header patterns with `^` and `\n` anchors (`^{section}\s*$`, `\n{section}\s*[:|-]`), which would explain why indented text like `"    Education:"` yields an empty `detected_sections`.
>
> Next I'll run the issue's own snippet on current `main` (Python 3.11, Windows 11, fresh venv from my fork) together with `tests/unit/test_resume_parser.py`, and post a repro report here with my environment, exact commands and output, whether or not I can reproduce it. I won't make code changes until that report is up.
>
> AI disclosure: I'm using an AI assistant (Claude Code) to help draft these comments and run my checks. I'll run and verify every step myself before posting results.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/54#issuecomment-6048916755

> Repro report for #54: reproduced, from my own run.
>
> **Environment**
> - Windows 11 Home (10.0.26300), Git Bash
> - Python 3.11.16 in a fresh venv (the repo's CI uses 3.11), pypdf 6.19.0, pytest 9.1.1
> - Code: `main` at f89c06fc3ff292df2a04a39ac51319d32a76b779 (my fork, in sync with upstream, no local changes)
> - The issue names no version, so there is no version gap to report. I skipped the Docker/Postgres steps in SETUP.md because the parser and its unit tests don't use them.
>
> **Steps** (from the repo root)
> 1. `python -m venv .venv`
> 2. `.venv/Scripts/python.exe -m pip install -e ".[dev]"`
> 3. Saved the snippet from the issue body unchanged as `repro54.py` and ran `.venv/Scripts/python.exe repro54.py`:
> ```python
> from ingestion.parsers.resume_parser import ResumeParser
> r = ResumeParser()
> res = r.parse('\n    John Smith\n    john@example.com\n\n    Education:\n    - B.S. Computer Science\n\n    Skills: Python\n')
> print(res.metadata['detected_sections'])
> ```
> 4. Control: the same text with the 4-space indentation removed:
> ```python
> res = ResumeParser().parse('\nJohn Smith\njohn@example.com\n\nEducation:\n- B.S. Computer Science\n\nSkills: Python\n')
> print(res.metadata['detected_sections'])
> ```
> 5. `.venv/Scripts/python.exe -m pytest tests/unit/test_resume_parser.py -rx -q`
>
> **Observed**
> - Step 3 (indented, the issue's input) prints `[]`. The issue expects Education and Skills.
> - Step 4 (control, no indentation) prints `['Education', 'Skills']`. So the indentation is what makes detection fail, not the rest of the text.
> - Step 5 prints `5 passed, 5 xfailed`. All five xfails carry the reason "issue #54: resume section detection fails on leading whitespace": `test_parse_single_column_resume_text`, `test_parse_resume_no_work_experience`, `test_parse_markdown_resume`, `test_detect_sections`, `test_strip_markdown_syntax`. The issue body lists three of these; the repo marks two more.
>
> **Conclusion**
> Reproduced on current `main`: indented section headers give an empty `detected_sections`, and the same text without indentation is detected. I have only shown the symptom and the indentation trigger. I have not changed any code, and I haven't tested other kinds of leading whitespace such as tabs. Next I'll read `_detect_sections()` to see where the line-start anchors reject indented headers.
>
> AI disclosure: I used an AI assistant (Claude Code) to help write this report. I ran every command above myself and the output is copied from my terminal.

## Eval iterations

**Run history**

1. Full run, no `--save-run`: 13/17 scored items. Three packages (pkg-04, pkg-07, pkg-11) errored instead of being graded, so only 17 were scored. Real disagreements: pkg-03, pkg-05, pkg-09, pkg-10 (all gold accept, my rubric said reject).
2. Rubric revised (three checks loosened). Partial `--only` run on the 4 disagreements plus canaries (pkg-20, pkg-13, pkg-14, pkg-15, pkg-06, pkg-04, pkg-07, pkg-11): 9/9 scored; pkg-04, 07, 11 errored again.
3. Found the cause of the errors: on Windows the harness writes the prompt to `claude` with the default cp1252 encoding, and those three bundles contain characters it cannot encode, so `claude` received an empty stdin ("no stdin data received"). Setting `PYTHONUTF8=1` fixed it, with no change to the harness. `--only pkg-04,pkg-07,pkg-11`: 3/3.
4. Confirming full run with `--save-run eval-run.txt`: 20/20 (bar 18/20: PASS), categories clear-accept 8/8, disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3, wrong-target 4/4. This is the committed `eval-run.txt`; its rubric, evidence-guide and SKILL.md hashes match the files in `tools/repro-check/`.

**Package analysis**

pkg-09 (sharkdp/fd#2033, `--exec-batch` commands reordering when arguments exceed ARG_MAX). Gold label: accept ("honest cannot-reproduce: real attempt at the argument-size reordering with marker-order artifacts, names what differed and what a triggering setup likely needs"). In my first run my rubric said reject: `artifact-matches-behavior` failed it with "Only artifact is order.log 'ONE ONE ONE TWO TWO TWO'; report states no TWO ever overtook ONE, so the issue's reordering symptom is not shown". My original wording ("the shown artifact reproduces the same symptom the issue describes") made a faithful negative result look like a wrong-target artifact, because a cannot-reproduce by definition does not show the symptom. After my revision it says the artifact passes when it is real output from the issue's own stated trigger showing the non-failing result, so the grader now accepts pkg-09 because the report ran the issue's trigger, showed the output, and said what differed. It reads this way because the issue is about what the artifact came from (same trigger or a different one), not about whether the bug appeared.

**Check rationale**

| artifact-matches-behavior | The output/log/screenshot excerpt, read against the issue's stated actual behavior | The shown artifact reproduces the same symptom the issue describes (same error/message, same exit behavior, same visible defect) — not an adjacent symptom produced by a different trigger. If the report is an honest cannot-reproduce, the artifact passes when it is real output from the issue's own stated trigger (same command/input/config) showing the non-failing result; it fails only when the artifact comes from a different trigger or shows a different defect that the report narrates as the issue's symptom | required |

This is the check I changed most. The first version only accepted artifacts that showed the symptom, which rejected pkg-09 and pkg-10, two honest cannot-reproduce packages the gold labels accept. I did not drop the check or make it purely about the write-up. I kept the wrong-target failure (an artifact from a different trigger, or a different defect narrated as the issue's symptom, as in pkg-02) and added the exception for a real output from the issue's own trigger. I rejected a looser rule such as "any real output passes" because it would let adjacent-symptom reports through.

**Trade-offs**

The check now relies on the grader judging whether a cannot-reproduce used the issue's "own stated trigger". A report that changed an input slightly and called it the issue's trigger could slip through if the difference is subtle. I loosened three checks in that revision (`artifact-matches-behavior`, `steps-followable`, `honest-conclusion`), so I re-ran canaries with `--only`: pkg-20 (the only disclosure package), pkg-13, pkg-14, pkg-15 (no-evidence), and pkg-06 (unfollowable-comms). All still agreed, and the final full run was 20/20, so I found no package the loosening flipped.

---

Related paths: `eval-run.txt` in this directory; my skill's files in `tools/repro-check/`.
