# Procedure: how this skill grades a plan package

This procedure grades a plan. It does not write or improve one. Follow the steps in order. Keep the notes the steps ask for: the checks are graded from those notes, not from a fresh read.

## Read order

Read in this order, and write the note each step names before moving on. The repro evidence is read before the plan on purpose: if you read the plan first, its story anchors you and you will read the evidence to fit it.

1. Read the repo-facts block. Copy out, verbatim, the sentence(s) of the contribution or AI-use policy that mention AI, disclosure, or who must write comments. Note which surface each sentence applies to: issue comments, comments to maintainers, the pull request, or the code. Note if the policy is silent.
2. Read the issue body. Write one line for the symptom, with the exact command or input and the expected and actual result. Write down any cause the reporter asserts, and label it "asserted by reporter".
3. Read the thread highlights. List each commenter with their association (OWNER, MEMBER, COLLABORATOR, CONTRIBUTOR, NONE). For each OWNER, MEMBER, or COLLABORATOR, and for anyone else the thread treats as deciding the fix, copy their direction, ruling, rejected approach, or request, verbatim, as a "maintainer direction". Separately list every pull request or prior fix the thread names as a "named PR". If the thread has no comments, write "thread: empty".
4. Read the repro-evidence block. Number every step, control run, and artifact in it. For each number write: what was done, what happened, and what that rules in or out (for example "control without the flag passes: the flag is needed to fail"). Mark the first point at which the failure is visible, and mark anything shown to be already wrong before a later stage runs. This numbered list is the "evidence list".
5. Read the candidate plan. Copy out: (a) the stated cause, verbatim, as the "cause line"; (b) each proposed change as its own numbered line, as the "change list"; (c) every file, module, or area named; (d) the approach and order of work; (e) the in-scope and not-in-scope statements; (f) the test plan, verbatim; (g) every stated risk, unknown, or deferral. If a part is missing, write "absent: <part>".
6. Read the candidate plan comment. Write what it claims the plan does, what it commits to, which thread comments or PRs it mentions, and any AI disclosure sentence (tool and extent) verbatim. If it says nothing about AI, write "no AI statement".

## Evidence gathering

Each check has one gathering move. Do it using only the notes from the read order, plus the package text for quotes. In eval mode the package text is the whole world: do not fetch anything. In live mode, take the same facts from the places named in references/evidence-guide.md.

1. diagnosis-fits-evidence: put the cause line next to the evidence list. For each numbered item in the evidence list write one line: "if the cause line were true, this item would show ___; it shows ___; consistent yes or no". Do this for every item, including controls and the item where the failure first appears. Do not skip an item because the plan ignores it.
2. change-targets-cause: take the cause line and the change list. For each change write which component it modifies. Write whether that is the component the cause line names. Then take the repro's failing step as written and write what it would print or do after the change.
3. scope-bounded: take the change list. For each item write the answer to: "if this item were deleted from the plan, would the repro's failing step still stop failing?" Mark each item needed (no), a test for the fix, a repo-required follow-through, or extra (yes). Items under the plan's explicit "not in scope" or deferral statements are not part of the change list and are not marked.
4. executable-from-plan: take the change list and the named files, modules, or areas. For each change write "place named: <the place or none>; change stated: <the concrete edit or none>". Quote any word such as investigate, look into, figure out, somewhere, whichever is easier, maybe also, or experiment with that attaches to a change.
5. test-plan-decisive: take the test plan and the repro's failing step. Write: the step the test plan re-runs, the result it names, and whether unfixed code would produce a different result. Write "no named result" if none.
6. thread-direction-respected: take the maintainer-direction list and the named-PR list. For each entry write "comment or plan says: <quote>" or "not addressed". Treat "followed" or "reason given for departing" as addressed. Treat silence as not addressed.
7. ai-disclosure-compliant: take the policy sentences from step 1 and the AI statement from step 6. Decide first whether the policy requires disclosure on a surface that includes a plan comment (see the rubric). Then write whether the comment's AI statement names a tool and an extent.
8. unknowns-stated: take the risk, unknown, and deferral list from step 5(g). Write each in one line, or "none stated".

## Check execution

1. Execute the checks in the rubric's table order, one at a time. Grade each from its gathered notes. Do not re-read the whole package for a check unless a note is missing or two notes seem to conflict; then re-read only the part named.
2. For each check, name the deciding fact: a quote or a numbered note. A grade with no named fact is not allowed.
3. Apply the rubric's fail conditions as written. A check passes only when none of its fail conditions holds. A check fails when any one holds, even if the plan is otherwise strong, long, polished, or confident.
4. Grade the thing, not the write-up. Do not pass a cause because the thread or the issue states it, and do not fail a plan for being short. Do not count headings or sections. A terse plan that names a grounded cause, one place, and a decisive test passes every check.
5. When the rubric's "Not extra" or "passes when" sentences cover what you found, apply them. In particular: an explicitly deferred item is not scope; an unknown about a detail of a chosen change is not an executability failure; a plan that applies the same fix at sibling sites the issue names is not extra.
6. Use `unclear` only when the part a check reads is missing from the package and the rubric gives no pass for its absence (for example the plan has no test plan at all). An absent policy statement and an empty thread are not unclear: the rubric says those checks pass. Do not use `unclear` to avoid a decision when the evidence is present.
7. Do not let one check's result decide another's. Two checks may fail for the same defect. Grade each on its own condition.
8. If you find a gap in this procedure (a case it does not cover), say so in one line in the summary before the JSON. Do not invent a step silently.

## Verdict assembly

1. Collect the graded checks in the rubric's order. List the required checks that are `fail` or `unclear`.
2. Apply the rubric's verdict rule exactly: `accept` only if every required check is `pass`; otherwise `reject`. Treat each `unclear` on a required check as `fail`. Do not weigh, count, or average. The preferred check never changes the verdict.
3. Before the JSON block, write one short summary line per check ("check: grade, deciding fact"), then one line naming the verdict and, if reject, each failed required check.
4. For every failed or unclear check, the JSON `evidence` field must quote the deciding line from the package (or the number of the evidence-list item) and say in a few words why it decides. For passing checks, the `evidence` field names the one fact that decided it.
5. Emit the fenced JSON block last, with the item id given in the prompt, every check in the rubric (the preferred one included), and a `verdict` of `accept` or `reject`. Nothing follows it.
