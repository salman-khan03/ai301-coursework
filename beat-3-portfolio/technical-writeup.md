# [YOUR TITLE: says what you did]

## Overview

[WRITE: one paragraph. What fitbenchmarking is, the problem, what you did, which part of the scout you wrote yourself, how Claude Code was used, and where the PR stands today.]

PR: https://github.com/fitbenchmarking/fitbenchmarking/pull/1803

## What happened

### Finding the issue

[WRITE: the first scan, what it got wrong, what you changed in the search strategy, and what the re-run dropped.]

Proof D, from `scan-output.md` (2026-10-31):

```text
4. MieuxVoter/presidentielle2027#157 (ranked #1): failed Unclaimed. A pull request (#302) was opened on 2026-10-31 and is still open.
5. Arize-ai/openinference#3774 (ranked #3): failed Unclaimed. The issue author's PR #3776 is open, and someone posted "I'd like to take this one" on 2026-10-29.
```

From `search-strategy.md` after the change (the first version ordered ties newest first and had no `-linked:pr`):

```text
gh search issues ... --no-assignee --state open --limit 100 ... -- -linked:pr
7. Break comment-count ties by `updatedAt`, oldest first: newest-first rewarded issues touched because someone had just claimed them.
```

From `scan-notes.md` (2026-10-31):

```text
Checked the search by hand: 157 and 3774 are gone, the three accepted issues remain.
```

### Reproducing it

[WRITE: what you expected, and what the run showed.]

Proof A, `python repro1767.py` on master at 1ae498d3 (excerpt, exit 1):

```text
  FAIL 200 line 275: https://sourceforge.net/projects/pygsl/
  FAIL 404 line 284: http://pygsl.sourceforge.net/api/pygsl.html#pygsl.multifit__nlin.lmder
       lands on https://pygsl.sourceforge.net/api/pygsl.html
12 pygsl links, 12 dead or still on SourceForge
```

### Choosing and checking the fix

[WRITE: why a host swap would not work, and why pinned source links.]

Proof B, from commit 228ae3fc:

```diff
-* `Levenberg-Marquardt (unscaled) <http://pygsl.sourceforge.net/api/pygsl.html#pygsl.multifit__nlin.lmder>`__ (:code:`lmder`)
+* `Levenberg-Marquardt (unscaled) <https://github.com/pygsl/pygsl/blob/v2.6.7/pygsl/multifit_nlin.py#L75>`__ (:code:`lmder`)
```

The same script on the branch (exit 0):

```text
12 pygsl links, 0 dead or still on SourceForge
```

### Opening the PR

[WRITE: what the voice guard stopped, what you changed in the draft, and what happened next.]

Proof C, from `guard-log.md` (2026-10-31):

```text
19:01 [manual] stop: rule broken: "In the Testing section I paste the before and after of the repro, with the command, and the output of each check I ran." | target: pr_draft.md
19:02 [manual] pass | target: pr_draft.md
```

### Where it stands

[WRITE: the maintainer's reply, the merge of master into your branch, the passing checks, the AI policy, and the nudge you did not send.]

Proof E, Letizia97 on the PR (2026-11-02):

> Hi @salman-khan03, thanks for your contribution ! We'll have a look and let you know soon

## Limitations and what's next

[WRITE: at least one real limit, then the next step that follows from it.]
