# Fixing 12 Outdated pyGSL Documentation Links in FitBenchmarking

## Overview

For my CodePath AI Open Source Capstone, I contributed to FitBenchmarking, an open-source Python project that helps researchers compare numerical fitting algorithms. I worked on GitHub issue #1767, which concerned documentation links pointing to pyGSL's outdated SourceForge website. I found that nine links returned errors and three redirected to the old project page. Using Claude Code to locate the links, create a verification script, make the edits, and run checks, I prepared a documentation update covering all 12 links. I chose the replacement approach and reviewed the changes and results myself. I also developed the rubric and, with Claude Code, the search strategy used by the course-provided issue scout. My [pull request #1803](https://github.com/fitbenchmarking/fitbenchmarking/pull/1803) remains open and is awaiting maintainer review.

## What happened

### Finding the issue

I started with the issue-scout tool provided by CodePath. My responsibility was to develop the rubric and search strategy to identify contributions matching my Python experience and available development environment.

Before the first scan, I added date and comment filters to reduce irrelevant results. However, the scan still included issues already claimed by other contributors.

**Evidence 1 — Initial scan results, October 31, 2026**

```text
4. MieuxVoter/presidentielle2027#157 (ranked #1): failed Unclaimed. A pull request (#302) was opened on 2026-10-31 and is still open.
5. Arize-ai/openinference#3774 (ranked #3): failed Unclaimed. The issue author's PR #3776 is open, and someone posted "I'd like to take this one" on 2026-10-29.
```

The scan evaluated five issues and accepted three. After seeing the claimed issues, I revised my search strategy by adding `-linked:pr` and changing the tie-break from newest-first to oldest-first.

I checked the search results again and confirmed that both claimed issues disappeared while the three accepted candidates remained.

FitBenchmarking was my second-ranked accepted issue. I chose it over the highest-ranked candidate because that project required Isaac Sim and a CUDA GPU, while I could reproduce the FitBenchmarking issue locally.

This taught me that a tool's ranking is only useful when I also verify whether the work is practical.

### Reproducing the problem

The issue initially appeared to involve replacing outdated web addresses. However, I wanted to confirm what the links actually did before changing anything.

I expected the 12 documentation links to open the current pyGSL repository, installation documentation, or information about specific minimizers.

Claude Code helped locate the links and wrote a Python script called `repro1767.py` to request each URL and follow redirects.

**Evidence 2 — Original reproduction**

Command:

```bash
python repro1767.py
```

Output excerpt:

```text
  FAIL 200 line 275: https://sourceforge.net/projects/pygsl/
  FAIL 404 line 284: http://pygsl.sourceforge.net/api/pygsl.html#pygsl.multifit__nlin.lmder
       lands on https://pygsl.sourceforge.net/api/pygsl.html
12 pygsl links, 12 dead or still on SourceForge
```

Nine links returned HTTP 404 errors. The other three reached SourceForge rather than the current project documentation.

I learned that a successful HTTP response does not necessarily mean a documentation link works correctly. A page can load while still sending users somewhere unhelpful.

### Choosing and implementing the fix

My original expectation was that replacing SourceForge with pyGSL's new documentation website would solve the problem.

After investigating, I found that pyGSL's Read the Docs website did not contain equivalent pages for the nine individual minimizers. The GNU Scientific Library documentation also lacked suitable destinations for some of them.

I decided to link those nine methods directly to their corresponding classes in pyGSL's GitHub source code.

I pinned the links to version `v2.6.7` with specific line numbers. This followed an existing pattern in FitBenchmarking's documentation and reduced the risk of links changing when the repository's default branch changed.

**Evidence 3 — Actual change from commit `228ae3fc`**

```diff
-* `Levenberg-Marquardt (unscaled) <http://pygsl.sourceforge.net/api/pygsl.html#pygsl.multifit__nlin.lmder>`__ (:code:`lmder`)
+* `Levenberg-Marquardt (unscaled) <https://github.com/pygsl/pygsl/blob/v2.6.7/pygsl/multifit_nlin.py#L75>`__ (:code:`lmder`)
```

Claude Code made the documentation edits and ran the checks. I reviewed the diff and verified the replacement destinations.

The final changes updated 12 links across two `.rst` documentation files without modifying the application's source code.

I deliberately left the broader GSL installation instructions unchanged because another issue, #1588, already covered that work.

### Testing and improving the submission

After making the changes, I ran the same reproduction script against the updated branch.

**Evidence 4 — Verification after the fix**

```text
12 pygsl links, 0 dead or still on SourceForge
```

The script exited successfully. It also checked that source-code links pointed to the expected method classes rather than merely returning HTTP 200.

Additional verification included Ruff linting and formatting checks, a successful Sphinx documentation build, and a test run reporting 871 passed and 110 skipped.

Before opening the pull request, I used the course's voice guard to check my draft against my writing rules.

The guard stopped my first version because I included test results without showing the commands that produced them.

**Evidence 5 — Voice guard log, October 31**

```text
- 2026-10-31 19:01 [manual] stop: rule broken: "In the Testing section I paste the before and after of the repro, with the command, and the output of each check I ran." | target: pr_draft.md
- 2026-10-31 19:02 [manual] pass | target: pr_draft.md
```

I added the missing commands and complete output, then reran the guard successfully.

The ship-gate also caught a corrupted Windows path in my evidence file. I corrected it before proceeding.

These checks showed me that validation tools are most helpful when I investigate their failures and correct the underlying problem.

### Opening the PR and monitoring progress

I opened pull request #1803 on October 31, 2026, in my local time zone.

On November 2, maintainer Letizia97 replied:

> Hi @salman-khan03, thanks for your contribution ! We'll have a look and let you know soon

On November 6, a maintainer merged the upstream master branch into my contribution branch, and all 12 CI checks passed. My PR itself was not merged.

During Unit 8, I used CodePath's supplied PR-monitoring tool. Claude Code drafted its first playbook and then revised it after discovering that the initial rules measured time to merge instead of time to a maintainer's first response.

The initial monitor run drafted a follow-up too early. I corrected the playbook's measurements and waiting rules. The second run recommended waiting until November 27. No automated message was sent.

I also discovered that FitBenchmarking adopted an AI contribution policy on November 20.

The policy requires contributors to disclose AI assistance, understand their submissions, and write their own communications with maintainers. Importantly, **the project does not accept primarily AI-generated pull requests for issues labeled `good first issue`.**

My issue has that label, so the maintainers may determine that my PR does not qualify under the new policy. I disclosed Claude Code's involvement and understand that the maintainers have the final decision.

## Limitations and what's next

My contribution updates 12 documentation links, but it does not automatically detect future broken links.

The nine source-code links are pinned to pyGSL version `v2.6.7`. This keeps their destinations stable, but they will not automatically reflect newer versions.

I also left the remaining GSL installation documentation unchanged because that work belongs to issue #1588.

If I continue the project, I would investigate adding an automated documentation link checker so broken links could be detected during future updates.

I would also confirm the acceptable scope of AI assistance with maintainers before starting another issue, especially one labeled `good first issue`.

The most important lesson from this contribution was that small changes still require careful engineering. I learned to reproduce an issue before changing anything, verify the actual outcome rather than trusting a successful command, improve a tool when its first result falls short, and take responsibility for work completed with AI assistance.

Although my pull request is still awaiting review, the experience helped me better understand testing, technical documentation, open-source collaboration, and how to make decisions independently while using development tools.
