# Plan for #54: resume section detection fails on text with leading whitespace

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/54
My repro comment: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/54#issuecomment-6048916755
Branch: `fix/54-resume-section-whitespace`

## Diagnosis

The cause is that the code only recognises a section header when the header text begins in column 0. Two places in `ingestion/parsers/resume_parser.py` make that assumption:

1. `_detect_sections()` builds four patterns per section name, and every one puts the name directly after a line anchor (`^` or `\n`): `^{name}\s*$`, `^{name}\s*[:|-]`, `\n{name}\s*$`, `\n{name}\s*[:|-]`. On a line like `    Education:` the character after the anchor is a space, so none of them match.
2. `_strip_markdown()` strips headings with `re.sub(r"^#+\s+", "", ...)`, which also needs the `#` in column 0. An indented `    # Education` is not stripped, so `_detect_sections()` is then handed a line that starts with `#`.

What from my own repro supports this (quoted from my comment on the issue):

- Step 3, the issue's snippet unchanged, prints `[]`.
- Step 4, the same text with the 4-space indentation removed and nothing else changed, prints `['Education', 'Skills']`. Only the indentation differs, so the indentation is the trigger, not the header names or the rest of the text.
- Step 5, `pytest tests/unit/test_resume_parser.py -rx -q`, prints `5 passed, 5 xfailed`, all five with the reason "issue #54: resume section detection fails on leading whitespace" (`test_parse_single_column_resume_text`, `test_parse_resume_no_work_experience`, `test_parse_markdown_resume`, `test_detect_sections`, `test_strip_markdown_syntax`). The issue body names three; the repo marks two more.

What I measured while planning, on scratch edits of `resume_parser.py` that I reverted with `git checkout` (tree clean afterwards):

- Changing only the four `_detect_sections()` patterns turns three of the five markers into `XPASS(strict)`; `test_parse_markdown_resume` and `test_strip_markdown_syntax` stay xfailed.
- Changing only the `_strip_markdown()` pattern flips exactly those two and leaves the other three xfailed.
- Changing both flips all five. So the two functions are two halves of one defect, and the five markers cannot be cleared by fixing one.

## Scope

In scope, one change:

- Let both line-start assumptions tolerate leading spaces and tabs, by inserting `[ \t]*` after the anchor in the four `_detect_sections()` patterns and after the `^` in the `_strip_markdown()` heading pattern.
- Remove the five `@pytest.mark.xfail(strict=True, reason="issue #54: ...")` markers, as `docs/CONTRIBUTING.md` requires for a seeded bug ("drop the marker from every test that covers it"). Under `strict=True` they would otherwise fail the suite as `XPASS(strict)`.
- Add a regression test that an indented resume yields the same detected sections as the unindented one.

Not in scope, and why:

- The `list(set(detected))` return in `_detect_sections()`, which makes the order nondeterministic. Real, but it does not cause this bug; my test compares sets.
- The `^` / `\n` redundancy between the four patterns (`^` already matches after `\n` under `re.MULTILINE`). Cleaning it up would be a refactor that does not serve #54.
- Non-breaking spaces and other Unicode whitespace in PDF-extracted text (see Risks).
- `SECTION_HEADERS`, the other substitutions in `_strip_markdown()`, and the PDF extraction path.

## Files I will touch

- `ingestion/parsers/resume_parser.py`: the four patterns in `_detect_sections()` (lines 133-138) and the heading pattern in `_strip_markdown()` (line 101).
- `tests/unit/test_resume_parser.py`: delete the five xfail markers; add one parametrized regression test.

I checked `pyproject.toml`, `docs/` and the rest of `tests/` for other references to #54 and found none, so there is no baseline suppression to remove.

## Approach

1. Create `fix/54-resume-section-whitespace` from `main` (f89c06f).
2. In `_detect_sections()`, change the four patterns to `^[ \t]*{name}...` and `\n[ \t]*{name}...`.
3. In `_strip_markdown()`, change `^#+\s+` to `^[ \t]*#+\s+`.
4. Remove the five markers.
5. Add `test_detect_sections_ignores_leading_indentation`, parametrized over a 4-space and a tab indent, asserting the set of detected sections equals the unindented control's.
6. Commit as `fix(ingestion): handle leading whitespace in resume section detection`, with `Fixes #54` in the footer.

I am using `[ \t]*` rather than `\s*` because the defect is horizontal indentation; `\s` also matches newlines. I did not test `\s*`, so this is a deliberate narrowing, not something I measured to be necessary.

I considered the approach in CONTRIBUTING's example commit message ("strip the line before matching headings"), which would mean iterating over lines in `_detect_sections()`. I am not taking it because it rewrites the loop and changes how `\s*$` behaves across lines, which is a larger diff than this bug needs.

## Test plan

I re-run my Unit 2 repro steps on the built branch and compare with what I recorded then.

1. Issue's snippet, unchanged (`repro54.py`, `.venv/Scripts/python.exe repro54.py`): was `[]`; I expect `['Education', 'Skills']` (order is not guaranteed, so I compare sorted).
2. Control, same text without indentation: was `['Education', 'Skills']`; I expect it unchanged.
3. `.venv/Scripts/python.exe -m pytest tests/unit/test_resume_parser.py -rx -q`: was `5 passed, 5 xfailed`; I expect all tests to pass with no xfail and no `XPASS(strict)` (the 10 existing tests plus my 2 parametrized cases).
4. Set the fix aside and run the new test: it must fail, to show it pins the fix. Then restore.
5. `.venv/Scripts/python.exe -m pytest tests/unit -m unit -q`: the baseline with the fix applied and markers still in place was `375 passed, 48 xfailed` plus the five expected `XPASS(strict)` failures; after the markers are removed I expect no failures outside the file I touched.
6. `ruff check`, `black --check`, and `mypy` on the two files.

## Risks and unknowns

- Non-breaking spaces. On a scratch edit I measured that text indented with U+00A0 is still not detected after the change, because `[ \t]*` does not match it. I have not tested a real PDF, so I do not know whether pypdf produces U+00A0 for indented lines. If it does, that is a follow-up, not part of this fix.
- Wider match surface. After the change, an indented line that begins with a section word followed by `:`, `-` or `|` (for example `    Skills: Python, Go` nested under Experience) counts as a section. Unindented lines already behave this way, and this function has no way to tell a nested line from a heading. That is the cost of the fix.
- Indented headings in a real PDF. The PDF path calls the same `_detect_sections()`, so I expect it to be fixed too, but I have only exercised the string path (`parse(str)`), not a PDF.
- Overlap. Other classmates have posted the same regex change in the thread, and one has an open PR (#100). I read their comments. The diagnosis and numbers above come from my own repro and scratch runs, and I have not opened PR #100's diff, so I do not know how its tests differ from mine.

## Deviations

Nothing material changed; the plan held. The build touched the same two files and made the same five pattern edits, removed the same five markers, and added the one parametrized test, and every number I predicted came out as predicted: the repro went from `[]` to `['Education', 'Skills']`, the control stayed `['Education', 'Skills']`, the parser test file went from `5 passed, 5 xfailed` to `12 passed`, and with the fix set aside the new test failed for both indents (7 failures in that file with the fix off: the five formerly xfailed tests and the two new cases).

Two small differences from what I wrote:

- I also reworded the two code comments above the edited patterns ("indented headings included", "allowing leading indentation") so they say why the patterns look the way they do. That was not in the plan.
- For the lint, format and type checks I ran the repo's wider targets (`ruff check .` and `mypy api/ core/ ingestion/ rag/ agent/ safety/`) instead of only my two files. All passed. The full unit suite finished at `382 passed, 48 xfailed`: the 375 that already passed, the 5 that were xfailed, and my 2 new cases, with the other 48 seeded xfails untouched.

The unknowns in my Risks section are still unknown: I did not test a real PDF or non-breaking spaces beyond the scratch measurement, so the posted comment's description of them is still accurate.
