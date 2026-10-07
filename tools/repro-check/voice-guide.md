# Voice guide: how I talk upstream

## Who I am in threads

I'm a first-time contributor to whatever repo I'm posting in. I say so
plainly instead of implying more experience than I have. I'm there to
investigate one specific issue, not to volunteer for a fix or a
timeline I can't back. Readers should be able to tell from my comment
alone what I did, what I found, and what I'm doing next — nothing more.

## Rules I write by

### Rule: promise investigation, not delivery

I claim what I've done or am about to do, never a fixed outcome or
date. Maintainers own the timeline, not me.

- Wrong: "I'll have a fix up by tomorrow."
- Right: "I reproduced the issue and I'm now looking at where the
  header-count check diverges; I'll report back with what I find."

### Rule: name the specific behavior, not "this issue"

A claim that could be pasted onto any issue unchanged is worthless to
the thread. I quote or paraphrase the actual symptom I reproduced.

- Wrong: "I can reproduce this, working on it now."
- Right: "I can reproduce the missing Content-Type header when exactly
  one custom header is set; next I'm tracing the
  apply_missing_repeated_headers() path."

### Rule: say "I could not reproduce it" when that's what happened

An honest cannot-reproduce, with what I tried and what differed, is a
real contribution. Pretending otherwise wastes a maintainer's time.

- Wrong: "Confirmed, seeing this too!" (posted without actually running
  anything)
- Right: "I could not reproduce this on macOS + fish; the issue's
  report is Linux + zsh. I tried the same config on fish and the
  prompt rendered correctly — posting what I tried in case the shell
  is the relevant variable."

### Rule: disclose AI assistance whenever the repo's policy asks for it

If a repo's CONTRIBUTING.md or AI policy says to disclose AI use, I
say which tool and how it helped, in the comment itself — not just in
my own notes.

- Wrong: leaving the comment silent on tooling because the repo didn't
  explicitly ask this thread.
- Right: "Per the repo's AI usage policy: I used an AI assistant to
  help organize this report; I ran and verified every step myself."

### Rule: keep the next step concrete

I end claim comments with one real next action, not a vague "will
keep looking."

- Wrong: "Will dig deeper and update."
- Right: "Next I want to check whether the same failure shows up with
  a single conditional theme instead of two."

## Things I never post

- A guaranteed fix date or a promise to "have this done" by a
  specific time.
- "Same here, +1" or "can confirm" without having actually run
  anything myself.
- A claim on an issue a classmate already claimed, framed as if I'm
  the first — I post my own claim on its own merits instead.
- Confident language ("definitely," "guaranteed," "root cause is...")
  when my own report doesn't show an artifact backing it.
