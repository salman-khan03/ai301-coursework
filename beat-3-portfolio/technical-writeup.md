# Updating Outdated pyGSL Documentation Links in FitBenchmarking

## Overview

For my CodePath AI Open Source Capstone, I worked on FitBenchmarking, an open-source Python project that helps researchers compare different numerical optimization and fitting algorithms. I chose GitHub issue #1767 because parts of the documentation still linked to pyGSL's old SourceForge website, even though the project had moved to GitHub.

What looked like a straightforward documentation update became a more involved investigation. I found 12 outdated links, reproduced the failures, and opened a pull request replacing them with verified destinations. I also used my own issue-search strategy, validation script, and automated quality checks to support the contribution.

I decided which issue to work on, investigated possible replacement links, selected the final approach, and reviewed the changes. Claude Code assisted with searching, scripting, editing, and testing. My pull request, [#1803](https://github.com/fitbenchmarking/fitbenchmarking/pull/1803), is still open and awaiting review.

## What happened

### Finding an issue worth working on

I started by building an issue-scout workflow to search GitHub for beginner-friendly contributions matching my Python and software development experience.

My first searches were not very effective. They returned recently updated issues, including tasks from small repositories and issues that other contributors had already claimed. The tool was prioritizing activity instead of actual availability.

The first scan evaluated five issues and accepted three, but two rejected candidates already had open pull requests. That showed me my search strategy needed improvement.

**Evidence 1 — Initial search results**

From my October 31 scan:

```text
4. MieuxVoter/presidentielle2027#157 (ranked #1): failed Unclaimed. A pull request (#302) was opened on 2026-10-31 and is still open.
5. Arize-ai/openinference#3774 (ranked #3): failed Unclaimed. The issue author's PR #3776 is open, and someone posted "I'd like to take this one" on 2026-10-29.
```

I revised my search strategy by adding date and comment filters, excluding linked pull requests with `-linked:pr`, and changing the tie-break to prioritize older updates instead of newer ones.

After rerunning the search, both claimed issues disappeared while the three accepted candidates remained.

The scout initially ranked another issue higher, but it required Isaac Sim and a CUDA GPU. I chose FitBenchmarking because I could reproduce and validate the documentation problem locally on my Windows laptop.

This experience showed me that a search tool can rank issues, but I still need to evaluate whether an issue is realistic for my environment.

### Reproducing the documentation problem

The issue concerned pyGSL, a Python interface for the GNU Scientific Library.

I expected each documentation link to take readers to the correct project page or the specific optimization method being described. Instead, some returned errors while others redirected to an outdated project page.

I used Claude Code to help create a Python reproduction script, `repro1767.py`, that requested each URL and followed redirects.

**Evidence 2 — Before the fix**

Command:

```bash
python repro1767.py
```

Output excerpt from the original branch:

```text
  FAIL 200 line 275: https://sourceforge.net/projects/pygsl/
  FAIL 404 line 284: http://pygsl.sourceforge.net/api/pygsl.html#pygsl.multifit__nlin.lmder
       lands on https://pygsl.sourceforge.net/api/pygsl.html
12 pygsl links, 12 dead or still on SourceForge
```

The script exited with code 1. Nine links returned HTTP 404 errors, while three still reached SourceForge. An important detail was that an HTTP 200 response did not necessarily mean a link was useful. A page could load successfully while still sending readers to the wrong destination.

### Choosing the solution

My first thought was to replace the old website address with pyGSL's new documentation domain. However, I found that its Read the Docs website did not contain equivalent pages for the nine individual minimizers.

A simple domain replacement would therefore have created another set of broken links.

I also examined the GNU Scientific Library documentation, but it did not provide suitable destinations for every method.

I decided to link the nine minimizers directly to their corresponding classes in pyGSL's GitHub source code. I pinned those links to version `v2.6.7` and included line numbers so readers would reach specific implementations.

**Evidence 3 — Actual change from commit `228ae3fc`**

```diff
-* `Levenberg-Marquardt (unscaled) <http://pygsl.sourceforge.net/api/pygsl.html#pygsl.multifit__nlin.lmder>`__ (:code:`lmder`)
+* `Levenberg-Marquardt (unscaled) <https://github.com/pygsl/pygsl/blob/v2.6.7/pygsl/multifit_nlin.py#L75>`__ (:code:`lmder`)
```

I used the same general pattern already present in FitBenchmarking's documentation for a SciPy method.

The remaining links were updated to pyGSL's GitHub repository and its current installation documentation.

The changes were limited to two `.rst` documentation files. I intentionally left the broader GSL installation instructions alone because those were covered by another issue, #1588.

### Testing the changes

After updating the links, I ran the reproduction script again.

**Evidence 4 — After the fix**

```text
12 pygsl links, 0 dead or still on SourceForge
```

This time, the script exited successfully. It also checked that the GitHub source links identified the expected method classes rather than merely returning a successful HTTP status.

I ran additional checks on the contribution branch:

- Ruff linting passed.
- Ruff formatting checks passed.
- The Sphinx documentation build succeeded, with the same three existing warnings as the baseline.
- The focused default test suite reported 871 passed and 110 skipped.

These checks gave me evidence that the documentation changes worked without introducing failures in the tested parts of the project.

### Improving my checks before opening the PR

One of the most useful parts of the project happened while preparing my pull request.

I had configured a voice guard to check whether my PR explanation followed the writing and evidence rules I had established.

It caught a problem: I had included before-and-after results without showing the command used to produce them.

**Evidence 5 — Voice guard results, October 31**

```text
- 2026-10-31 19:01 [manual] stop: rule broken: "In the Testing section I paste the before and after of the repro, with the command, and the output of each check I ran." | target: pr_draft.md
- 2026-10-31 19:02 [manual] pass | target: pr_draft.md
```

I added the missing command and complete output, then reran the guard successfully.

My ship-gate also caught a corrupted Windows path in my evidence file. I corrected the path and reran the checks.

These failures were useful because they showed that my automated tools needed to verify the actual evidence, not simply report that a process had completed.

### Opening the pull request and following up

I opened [PR #1803](https://github.com/fitbenchmarking/fitbenchmarking/pull/1803) on November 1, 2026.

On November 2, a maintainer, Letizia97, replied:

> Hi @salman-khan03, thanks for your contribution ! We'll have a look and let you know soon

On November 6, a maintainer merged the upstream master branch into my contribution branch. The project's automated checks passed again. This was not a merge of my pull request into the project; the PR remains open.

I also built a PR-monitoring workflow to help decide when to follow up. Its first version proposed contacting the maintainer too early because I had based the waiting period on time-to-merge rather than time-to-first-response. I corrected the measurement and updated the workflow. No automated follow-up was sent.

On November 20, the project adopted an AI contribution policy requiring disclosure and human responsibility. It also restricts primarily AI-generated contributions to issues labeled `good first issue`, and prohibits AI-written maintainer communication except translation or grammar assistance.

Because my issue has that label and my PR used Claude Code assistance, the maintainers will need to decide whether the contribution satisfies their policy. I disclosed the assistance in my PR and will respect their decision.

## Limitations and what's next

My contribution addresses 12 documentation links, not every possible documentation issue in FitBenchmarking.

The biggest technical limitation is that the nine source-code links are pinned to pyGSL version `v2.6.7`. That makes their destinations stable, but they will not automatically follow newer releases. The contribution also does not update the remaining GSL installation instructions or add a continuous link-checking system.

If I continue this work, I would investigate whether a lightweight automated link checker could detect broken documentation links before future releases. I would also review whether more maintainable reference pages become available in newer pyGSL versions.

Before making additional contributions, I would read the project's current contribution policy and clarify the acceptable scope of AI assistance.

The biggest lesson I took from this experience is that even a small documentation change deserves careful investigation and testing. I learned to verify where links actually lead, compare results against a baseline, improve my tools when they miss something, and communicate my work with evidence rather than assumptions.
