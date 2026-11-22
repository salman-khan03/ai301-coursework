# Contribution: Docs link to pygsl source forge repository, which pygsl no longer maintains

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Salman Khan
**GitHub Username:** salman-khan03
**Issue:** https://github.com/fitbenchmarking/fitbenchmarking/issues/1767
**Pull Request:** https://github.com/fitbenchmarking/fitbenchmarking/pull/1803

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built issue-scout, a GitHub-wide search and grading funnel over my unit 1 rubric**

With Claude Code, I installed the scout over my unit 1 skill and wrote three `gh search issues` queries from my fit profile, then ran one scan that graded five issues and accepted three. My first queries returned only issues updated that day, mostly tiny new repos and Hacktoberfest farms, and in the scan both rejected issues already had open linked PRs because my newest-first tie-break rewarded fresh claims. I added date and comment windows, `-linked:pr`, and an oldest-first tie-break, then re-ran the searches by hand and saw both claimed issues drop while the accepted ones stayed. The rubric also ranked a 0-star repo that needs Isaac Sim and a CUDA GPU first, so I added preferred checks for Verifiable locally and Adoption.

**Unit 6: Installed the voice guard, added my Upstream rules, and wrote repo memory for fitbenchmarking**

I merged my three voice guide copies into one guide at `~/.claude/voice-guard/`, added rules for claims, questions, and pings, swapped repro-check and plan-check to their real-repo scopes, and distilled the repo's contributor docs into `CLAUDE.local.md`. The guard earned its place on my PR draft: it stopped the text because my Before and After blocks showed output without the command, which broke my own "show the run" rule, so I added the command and the full output. I used the pointer-line fallback for `voice-guide.md`, since a symlink isn't reliable in Git Bash on Windows. I have not applied the scope patch to my scout's rubric yet, because checking it needs a run of the unit 1 eval.

**Unit 7: Installed the ship-gate and, with Claude Code, wrote a Windows manifest for fitbenchmarking's five checks**

The ship-gate passed my real branch three times, and the first pass is the one that taught me something: its pr-precheck noticed that my script had turned `docs\build` into `docs` plus a backspace character in `test_evidence.md`, so I fixed the file and ran it again. The manifest fought me twice. On Windows the checks run through `cmd.exe`, so `.venv/bin/pytest` had to become `.venv\Scripts\pytest.exe`, and I first excluded `test_galahad` as a Windows failure before finding it only needed the optional `galahad` package, so I installed that and deleted the exclusion. The hook test also ran without any stop at first, because `claude` picked its PowerShell tool and both hooks only watch Bash; I reran it and opened the PR with PowerShell disabled, and then I saw the stops logged by both hooks. I didn't need to change my rubric.

**Unit 8: Ran the PR monitor on #1803, and rewrote the playbook that let it nudge too early**

On 2026-11-22 the monitor read my PR as quiet for 20 days and drafted a nudge to Letizia97, and that draft was not sent. My first playbook, which Claude Code drafted from merged PR data, was the problem: it used days to merge instead of days to a first maintainer response, it counted a collaborator's PR as an outside one, and the draft guessed that the maintainer's Nov 6 merge of master into my branch was my own push. Claude Code then corrected the sample, made a maintainer's push reset the count, and added a step to read the contributor docs and other PR threads before any nudge, and the second run drafted a wait note with Nov 27 as the earliest date. That same check found what the monitor can't see, because it only reads my own thread: the repo merged an AI policy on Nov 20 that bars AI-written messages to maintainers, so my first response rule is now that a draft is notes and I write every reply myself.

---

## Why I Chose This Issue

I searched with my scout's three `gh search issues` queries from my unit 1 fit profile, and the scan graded five issues and accepted three. This one, fitbenchmarking#1767, ranked second of the three, behind a one-file bug in CAD-for-isaacsim. My scout's verdict was accept, with one preferred check failed: the issue doesn't say which files to change. I chose it over the top pick because checking that one needs Isaac Sim and a CUDA GPU, which I don't have, and I could prove a docs link fix on my own Windows laptop. It fits my profile: a small, bounded change in a Python project that scientists actually use. I hope to learn how a real project's review works, starting with a change small enough that the conversation is about the contribution and not my code.

My scout was right about the fit and wrong about the size. It called this a "link swap in 2 .rst files", but 9 of the 12 links had no direct replacement, because pygsl's new docs have no page for those minimizers. A grep can count links, but it can't tell you whether they still resolve, so I would add a step to the scout's actionable-detail check that requests any URLs an issue names. It also flagged the "Linked/duplicate of #1588" comment without a rule for what to do about it, so I read #1588 myself and raised the overlap in my claim. My claim comment: https://github.com/fitbenchmarking/fitbenchmarking/issues/1767#issuecomment-6550679350

**Toolkit**

- Tool: issue-scout. I ran my three queries as a scan, then re-checked the live state of the top candidates before picking.
- What it produced: five graded issues, three accepted. This one was accepted with one preferred failure, Actionable detail: "body names replacement URLs but no files, repro or acceptance criteria".
- Difficulties: it undersold the work, since it read "link swap" from the issue text without testing a single link, and it had no rule for a "duplicate of" comment.
- What changed: nothing in the scout yet. I would add a URL check to Actionable detail, and a rule that a "duplicate of" comment sends you to read the other issue before claiming.

---

## Understanding the Issue

**Problem Description**

FitBenchmarking's documentation sends readers to pyGSL's old SourceForge site. pyGSL moved to GitHub in 2021, so the links are either dead or lead to a project page that is no longer maintained.

**Expected Behavior**

Every pygsl link in the docs should open a live page on the project's current home, and each minimizer's link should lead to something about that minimizer.

**Current Behavior**

On master at 1ae498d3, all 12 pygsl links fail. The 9 per-minimizer links return 404, and the other 3 land on the SourceForge project page.

**Affected Components**

Two documentation files: the GSL section of `docs/source/users/options/minimizer_option.rst` (11 links) and the GSL section of `docs/source/users/install_instructions/externals.rst` (1 link). No code is involved. The minimizer names on that page match `fitbenchmarking/controllers/gsl_controller.py`.

**Toolkit**

- Tool: `CLAUDE.local.md`, plus Claude Code to read the contributor docs and find every pygsl reference. No CONTRIBUTING.md exists here, so the rules came from `docs/source/contributors/workflow.rst` and `guidelines.rst`.
- What it produced: a short repo memory file with the branch format `<nnn>-description_of_work`, the past-tense PR title rule, and the `Fixes #<nnn>` rule, and a list of the two affected files.
- Difficulties: my scan said two files, which was right, but the first count of the links was 14. It's 12: that count came from reading grep lines, not links.
- What changed: my reproduction script counts the links, so the number in my claim comes from a run and not from reading a grep.

---

## Reproduction Process

**Environment Setup**

Windows 11, Python 3.14.3, Git Bash, FitBenchmarking master at 1ae498d3, tested on 2026-10-31. I forked the repo with `gh repo fork`, then made a virtual environment with `python -m venv .venv` and ran `.venv/Scripts/python -m pip install -e ".[dev]" -r docs/requirements.txt`. The reproduction needs only Python's standard library and internet access, but the test suite needs more. It failed at collection until I installed the extras from `.github/workflows/main.yml` (`bumps`, `DFO`, `gvar`, `minuit`, `numdifftools`, `lmfit`, `nlopt`, `paramonte`, `SAS`) plus `galahad`. A second worktree failed with `Filename too long`, because the repo holds long data file names and my temporary path was already deep, so I compared against master by switching the clone instead.

**Steps to Reproduce**

1. Find every pygsl link: `git grep -n -i -E "sourceforge|pygsl" -- docs`.
2. Request each one and follow redirects. My script `repro1767.py` does this for the two `.rst` files, and fails a link that errors or stays on SourceForge.
3. Observed result: `12 pygsl links, 12 dead or still on SourceForge`, exit 1. For example, `http://pygsl.sourceforge.net/api/pygsl.html#pygsl.multiminimize.nmsimplex` returns 404, and `http://pygsl.sourceforge.net/` lands on `https://sourceforge.net/projects/pygsl/`.

Control: `https://github.com/pygsl/pygsl`, `https://pypi.org/project/pygsl/`, and `https://pygsl.readthedocs.io/` all return 200, so my network and script aren't the failing part.

**Reproduction Evidence**

- **Commit showing reproduction:** https://github.com/salman-khan03/fitbenchmarking/commit/1ae498d31e08a818089356559e659ce2017d01b3
- **Screenshots/logs:** the full 12-line output is in my PR description: https://github.com/fitbenchmarking/fitbenchmarking/pull/1803
- **My findings:** the issue is real, and worse than it reads. pygsl's Read the Docs site has API pages only for `pygsl.statistics` and `pygsl.testing`, so its `api/pygsl.html` has no anchors for `multifit_nlin` or `multiminimize`, and changing the host would have produced 9 new dead links. Also, the old line 284 URL spelled `multifit__nlin` with two underscores, a typo of its own that the replacement removes.

**Toolkit**

- Tool: `repro1767.py`, which Claude Code wrote with me, and then repro-check on my posted claim.
- What it produced: the 12-of-12 failure above. repro-check returned accept on the claim, with passes on `claim-specific-and-honest` and `ai-disclosure-compliant`.
- Difficulties: repro-check graded a claim-only draft, so its other checks came back "not yet applicable". It also pointed out that my steps weren't re-runnable from what I gave it.
- What changed: I added the environment line and exact steps to this section, and I recorded the `multifit__nlin` typo it found.

---

## Solution Approach

**Analysis**

The cause is hardcoded URLs in two `.rst` source files; Sphinx just copies them through. In `docs/source/users/options/minimizer_option.rst`, the 9 links on lines 284 to 300 point at `http://pygsl.sourceforge.net/api/pygsl.html`, which no longer exists, and lines 275 and 302 point at the SourceForge site. Line 42 of `docs/source/users/install_instructions/externals.rst` does the same. The control shows the replacement hosts are live, and `docs/source/conf.py` only mocks the `pygsl` import, so it isn't involved.

**Proposed Solution**

Replace all 12 URLs. The 9 per-minimizer links go to each class in pygsl's GitHub source, pinned to tag `v2.6.7` with a line number, which is how the same file already pins the SciPy `leastsq` link. The install link goes to pygsl's install page on Read the Docs, and the two general links go to `https://github.com/pygsl/pygsl`, with the label changed to `GitHub - PyGSL` to match the other entries.

I am deliberately NOT changing the rest of the GSL install text, including the pip 20.0.2 note, because that is #1588. I'm also not touching any code or `conf.py`, and not adding a link-checking test or CI step, because the fix doesn't need one.

**Implementation Plan**

1. In `minimizer_option.rst`, point line 275 at the GitHub repo, lines 284 and 286 at `pygsl/multifit_nlin.py` at `v2.6.7` (`#L75`, `#L88`), lines 288 to 300 at `pygsl/multiminimize.py` at `v2.6.7` (`nmsimplex` `#L181`, `nmsimplex2` `#L209`, `conjugate_pr` `#L131`, `conjugate_fr` `#L141`, `vector_bfgs` `#L160`, `vector_bfgs2` `#L176`, `steepest_descent` `#L117`), and line 302 at `GitHub - PyGSL`.
2. In `externals.rst`, point line 42 at `https://pygsl.readthedocs.io/en/latest/ref/install.html`.
3. Add no test: the change is documentation only and the repo has no test that reads docs links, so the reproduction script is the check.

No deviations: the built diff is these 12 URLs and the one label, on one commit.

**Toolkit**

- Tool: plan-check, run from my clone on `plan.md` and `plan-comment.md`.
- What it produced: accept on the first run, every required check passing, plus voice notes on the comment. It also reported that my procedure expects `comment.md` and a separate repro comment, while here it's `plan-comment.md` and one claim comment that carries the repro.
- Difficulties: the real-repo scope won't grade a plan for an unclaimed issue, so my claim had to go up before the plan step and not at the end. The first accept also came with my comment breaking my own rule, since it said nothing about AI.
- What changed: I added a line saying how Claude Code helped and a reason for the Read the Docs install link, and re-ran it: accept, with no rule broken.

---

## Testing Strategy

The decisive check is the reproduction itself. `python repro1767.py` on master at 1ae498d3 ends `12 pygsl links, 12 dead or still on SourceForge` and exits 1. On the branch at 228ae3fc it ends `12 pygsl links, 0 dead or still on SourceForge` and exits 0. For each `#L` link it also reads that line of pygsl's source and requires `class <method>(` for the method named on the docs line, so a link that resolves to the wrong class still fails. `git grep -n -i sourceforge -- docs` returns no pygsl line on the branch.

The repo's own checks on the branch:

- `ruff check .`: exit 0, `All checks passed!`
- `ruff format --check .`: exit 0, `179 files already formatted`
- `.custom_hooks/minimizer_spelling.py` on both files: exit 0
- Sphinx HTML build: exit 0, with 3 docstring warnings, the same 3 as master
- `pytest` over the folders in `ci/unit_tests_default.sh` with `--test-type default`: `871 passed, 110 skipped, 37 warnings, 16 subtests passed`

**Toolkit**

- Tool: `repro1767.py` and `ship-manifest.md`, run with the same commands the ship-gate uses.
- What it produced: the before and after above, and a `test_evidence.md` the ship-gate graded.
- Difficulties: a first docs comparison printed "same warnings" only because a broken `sed` compared two empty files. The baseline test run also showed 1 failure on master, `test_galahad`, from a missing optional package.
- What changed: I redid the comparison in Python, which gave 3 and 3, identical. I installed `galahad`, so the branch run went to 871 passed with no exclusion.

---

## Implementation Notes

**2026-10-31**

Found that Unit 6 hadn't been done, so I installed the voice guard, my Upstream voice rules, and the real-repo scopes before the Unit 7 ship-gate. Forked and cloned fitbenchmarking, and picked #1767 from my scan. Reproduced it: 12 of 12 links failing.

**2026-10-31**

Decided on pinned source links after testing the alternatives. pygsl's Read the Docs has no pages for these modules, and GSL's own docs have anchors for 7 of the 9 methods but nothing for `lmder` or `lmsder`. The repo's own SciPy `leastsq` link is pinned to a tag with line numbers, so I followed that. Posted my claim first because plan-check won't grade an unclaimed issue.

**2026-10-31**

Built the fix in commit 228ae3fc on branch `1767-update_pygsl_docs_links`, following the repo's branch format and not the course's `fix/` habit. Wrong turn: I wrote a manifest exclusion for `test_galahad`, assuming a Windows failure, then installed the package, saw it pass, and removed the row.

**2026-10-31**

Ran the ship-gate. It passed, but pr-precheck caught corrupted Windows paths in `test_evidence.md`, so I fixed those and re-ran. The voice guard then stopped my PR draft for missing commands, so I fixed that and re-ran both. Opened PR #1803 through Claude Code with PowerShell disabled, so both hooks would fire.

**2026-11-06**

Letizia Protopapa merged master into my branch (06f7b9b8), so the PR now has two commits: my 228ae3fc and that merge. CI ran again and all 12 checks passed.

**2026-11-22**

Through Claude Code, installed the PR monitor, pulled the Nov 6 merge into my clone, and ran the monitor twice. No code changed, and nothing was posted. Wrong turn: the first playbook measured days to merge, so Claude Code measured days to a maintainer's first response on five merged PRs from outside contributors (1, 6, 16, 16, and 23) and rewrote the rules. Updated `CLAUDE.local.md`, which still said the repo had no AI policy.

**Toolkit**

- Tool: Claude Code on my clone, with `plan.md` as the boundary for what could go in the diff.
- What it produced: one commit, 12 insertions and 12 deletions across two `.rst` files, which plan-check and pr-precheck both held against the plan.
- Difficulties: a scripted write of Windows paths silently corrupted my evidence file, because `\b` and `\r` are escape sequences. I only found it because the ship-gate read the file.
- What changed: I rewrote that table with forward slashes. Next time I'd read generated evidence before handing it to a gate.

---

## Pull Request

**PR Link:** https://github.com/fitbenchmarking/fitbenchmarking/pull/1803

**PR Description:** "Updated pyGSL documentation links to the new repository", `Fixes #1767`. It replaces the 12 pygsl URLs in the two docs files: the 9 per-minimizer links go to each class in pygsl's source at tag `v2.6.7`, the install link goes to pygsl's install page on Read the Docs, and the two general links go to the GitHub repo. It leaves the rest of the GSL install text to #1588 and says the links won't follow newer pygsl releases. It includes the full before and after output of `python repro1767.py`, the checks with their outcomes, the repo's template checklist, and Notes for Reviewers on how I used Claude Code.

**Maintainer Feedback:**

- 2026-11-02: Letizia97 commented "thanks for your contribution ! We'll have a look and let you know soon" (https://github.com/fitbenchmarking/fitbenchmarking/pull/1803#issuecomment-6582767952). It asked for nothing.
- 2026-11-02: I answered with a thumbs-up and no comment, because there was nothing to add.
- 2026-11-06: Letizia Protopapa merged master into my branch (commit 06f7b9b8), and all 12 checks passed on it. No review or requested change came with it.
- 2026-11-22: no reply posted. I pulled that merge into my clone and left the branch as it is. There is still no review, and I didn't send the nudge the monitor drafted.

**Status:** Awaiting review

**Toolkit**

- Tool: the ship-gate, with my manifest of five repo checks and pr-precheck in live mode, then the voice guard, then `gh pr create` through Claude Code.
- What it produced: `ship-gate: PASS` on the final text, with all 5 checks ok and pr-precheck accept. The hook honored that manual pass when the PR opened, and the voice guard logged a pass.
- Difficulties: the hooks only watch the Bash tool, and `claude` chose PowerShell on my machine, so my first hook test proved nothing. Every text change also meant another gate run, at about two minutes and some course credit each.
- What changed: I opened the PR with `--disallowedTools PowerShell` so the hooks would really run, and set the repo default with `gh repo set-default` first. I left my rubric alone, since nothing in it asked for something this repo doesn't.

---

## Learnings and Reflections

**Technical Skills Gained**

- Checking links by what they land on, not only by status code. Three of the 12 old links returned 200 and still failed, because they redirected to the old SourceForge page. For `#L` links, a 200 doesn't prove the line is right, so the script also reads that line of pygsl's source and requires `class <method>(`.
- Sphinx and reStructuredText link syntax, and pinning a source link to a tag (`v2.6.7`) so its line number stays true.
- Building a baseline before trusting a result: the same tests, docs build, and script on master and on the branch, with a control showing that the new hosts were up.
- Windows details of running gates: manifest rows go through `cmd.exe`, `\b` in a Python string is a backspace, and a hook that matches the Bash tool doesn't see PowerShell.

**Challenges Overcome**

- The issue read like a host swap, and 9 of the 12 links had no page to swap to. I tested Read the Docs and GSL's own docs first, then used pinned source links, the way the repo's SciPy `leastsq` link does it.
- Several checks passed without testing anything: a docs comparison of two empty files, a hook test that ran in PowerShell, and an exclusion for `test_galahad` that only hid a missing package. Each time, I fixed the check and ran it again before using its result.
- The quiet PR was the hard part of Unit 8. My thread held one friendly comment and no reason for the wait. The reason was on #1815, where the maintainer wrote that they had to draft an AI policy before merging new PRs from first-time contributors. Reading beyond my own thread is what explained it.

**What I'd Do Differently Next Time**

- Read the issue's labels as carefully as its text. #1767 already had `Good first issue` when I claimed it on 2026-10-31. No AI policy existed then, and I disclosed Claude Code in my PR, but the policy merged on 2026-11-20 says PRs primarily generated by AI aren't accepted for issues with that label. Whether that applies to a PR opened earlier is the maintainers' call, and if they say it does, I'll close mine. Next time I'd leave that label for work I do by hand.
- Read the project's recent merged PRs and threads every week while mine is open. The policy conversation started on 2026-11-04, and Claude Code found it on 2026-11-22.
- Check the numbers behind a rule before a tool acts on it. The first staleness threshold Claude Code drafted looked reasonable, and it rested on the wrong measurement.
- Keep PR descriptions shorter. Mine carries the full before and after output, which is a lot to read for 12 links.

---

## Resources Used

- [FitBenchmarking's contributor workflow](https://github.com/fitbenchmarking/fitbenchmarking/blob/1ae498d3/docs/source/contributors/workflow.rst): the branch format, the past-tense title, and the `Fixes #<nnn>` rule. The repo has no CONTRIBUTING.md.
- [The repo's CI workflow](https://github.com/fitbenchmarking/fitbenchmarking/blob/1ae498d3/.github/workflows/main.yml): the list of optional extras, which got the test suite past collection.
- [minimizer_option.rst on master](https://github.com/fitbenchmarking/fitbenchmarking/blob/1ae498d3/docs/source/users/options/minimizer_option.rst): its pinned SciPy `leastsq` link is the pattern I followed.
- [pygsl's source at v2.6.7](https://github.com/pygsl/pygsl/blob/v2.6.7/pygsl/multiminimize.py) and [pygsl's Read the Docs site](https://pygsl.readthedocs.io/en/latest/): what the new links point at, and how I learned that there are no per-minimizer pages.
- [Issue #1588](https://github.com/fitbenchmarking/fitbenchmarking/issues/1588): the overlapping issue Letizia97 linked, which set the edge of my scope.
- [Letizia97's comment on #1815](https://github.com/fitbenchmarking/fitbenchmarking/pull/1815#issuecomment-6633562006) and [the AI policy](https://fitbenchmarking.readthedocs.io/en/latest/contributors/ai_policy.html): why new contributors' PRs waited, and what the repo now expects.
- Claude Code: it helped me search, script, test, and draft across the whole contribution and this story, as the Toolkit blocks describe.
