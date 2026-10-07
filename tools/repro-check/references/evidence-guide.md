# Evidence guide: where proof lives in a reproduction package

## Environment

- Where it lives: Eval bundle — the "Environment:" line that opens the
  candidate repro report. Live mode — the same line in the student's
  draft repro report file.
- What good looks like: names the tool's version and the OS/platform.
  A bare "latest version" with no number is not a version. Compare the
  stated version only against what the ISSUE ITSELF names as the
  target: the issue body's own version line, or the repo-facts
  "latest release" line. An older version than that target is not
  automatically wrong, but the report must name the gap (see
  deviation-disclosed below). Do not require the environment line to
  restate every dependency a thread commenter speculated might be
  involved — only what the report needs to place its own run.

## Steps

- Where it lives: Eval bundle — the "Steps:" section of the candidate
  repro report, including any inline commands, config blocks, or code
  snippets. Live mode — the same section of the draft.
- What makes them followable: every command, config value, or code
  snippet needed to reach the failing state is given verbatim, not
  paraphrased, OR the report gives exact parameters (offsets, flags,
  option values) that combine with content the issue's own body
  already states verbatim (a literal input string, an exact code
  block) — the test is whether a stranger has to guess anything, not
  whether the report re-pastes text the issue already pinned exactly.
  The starting state is stated (a fresh install, a given config file,
  a given input file) so a stranger does not have to infer it. A step
  like "reproduce the bug" or "ran the repro" or "set up the project"
  with no commands, parameters, or exact references is not a step.

## Deviation disclosed

- Where it lives: Eval bundle — the repro report's "Environment:" line
  (tool version/platform) compared against the issue's own stated
  target: the version(s) the issue body names, or the repo-facts
  "latest release" line when the issue doesn't name one itself. Live
  mode — the draft's environment line vs the live issue's stated
  version/OS.
- What good looks like: if the report's version or platform differs
  from what the issue itself targets, the report says so and names the
  likely effect (e.g., "the issue was filed against 1.9.4/1.10.0; it is
  still present on 1.11.7"). This family is scoped narrowly: it is not
  triggered by a thread commenter's speculation about some other
  library ("might be a multidict regression") that the report doesn't
  itself confirm or deny, and it is not triggered by an incidental
  technique choice (an offline/dry-run flag, a different but
  equivalent invocation) that leaves the mechanism under test
  unchanged. Judge the tool-version/platform gap against the issue's
  own stated target only.

## Behavior shown

- Where it lives: Eval bundle — the artifact block(s) in the candidate
  repro report (console output, log excerpt, screenshot description),
  read against the "## Issue" section's stated actual behavior. Live
  mode — the draft's artifact block(s), read against the live issue's
  body and pinned description (gathered via `gh`/API/web per scope.md).
- What it means to show the issue's behavior: the same error type or
  message, the same exit/crash behavior, or the same visible defect the
  issue names — not a different error produced by a modified
  repro (e.g., a changed flag, a changed input, a different code path)
  that merely looks similar. When the issue gives an exact command or
  input, the report's command/input must match it, or the mismatch
  must be called out under Honesty/deviation-disclosed.

- Cannot-reproduce reports: the artifact shown is the real output of the issue's own stated trigger; a non-failing result from that exact trigger is faithful evidence, not a mismatch.

## Honesty

- Where it lives: Eval bundle — the "Expected:"/"Actual:" or closing
  conclusion lines of the candidate repro report, compared against the
  artifacts shown earlier in the same report. Live mode — the same
  comparison inside the draft.
- What separates a pass from a fail: a report that says exactly what
  it found, including "I could not reproduce this, here is what
  differed and what might matter" backed by a real attempt and its
  artifacts, is honest and passes. A report that asserts a cause,
  certainty, or "guaranteed" result without a matching artifact in the
  same report — or that narrates an artifact as confirming behavior it
  does not show — fails, regardless of how confident or detailed the
  prose is.

## Comms

- Where it lives: Eval bundle — the "Candidate claim comment" section,
  read against the "## Issue" section and the repo-facts block's stated
  bug-report template asks and contribution/AI-use policy. Live mode —
  the student's draft claim (and repro) comment, read against the live
  issue and the repo's actual CONTRIBUTING.md / AI-policy file (per
  scope.md's house rules).
- What specific-and-honest looks like: the claim names the issue's
  concrete behavior (not just "this issue") and states a real next
  step the claimant intends to take, in their own words. Boilerplate
  that could be pasted onto any issue unchanged ("I'd like to work on
  this, assign me please") fails, as does a promise of a fix or a
  delivery date this early in the process.
- AI disclosure: check the repo-facts contribution policy line first.
  If it states AI usage must be disclosed, the claim or repro comment
  text must name the tool and the extent of assistance somewhere in
  the visible comment text — not merely something the student did
  privately. If the policy is silent, permissive, or only asks for
  responsibility (no disclosure requirement), the check passes without
  needing any disclosure language.
