# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment-recorded | The repro report's environment line/section | States the tool's version, OS/platform, and any dependency versions the issue's behavior depends on — enough to place this run relative to the issue's stated target | required |
| steps-followable | The repro report's steps (commands, config, code, starting state) | A stranger with the stated environment could re-run the same steps and land on the same starting state without guessing missing setup. Verbatim commands/code are the clearest way to satisfy this, but exact parameters that combine with content the issue itself already gives verbatim (a literal input string, exact offsets/flags) also satisfy it, as long as nothing is left to guess. A step described in prose passes if the trigger is named exactly and everything else is arbitrary or standard (e.g. "a valid dependencies list plus one unrecognized `category:` section" when that section is the whole trigger). A step described only in summary with no commands, parameters, or exact references ("set up the project", "ran the repro") fails | required |
| artifact-matches-behavior | The output/log/screenshot excerpt, read against the issue's stated actual behavior | The shown artifact reproduces the same symptom the issue describes (same error/message, same exit behavior, same visible defect) — not an adjacent symptom produced by a different trigger. If the report is an honest cannot-reproduce, the artifact passes when it is real output from the issue's own stated trigger (same command/input/config) showing the non-failing result; it fails only when the artifact comes from a different trigger or shows a different defect that the report narrates as the issue's symptom | required |
| deviation-disclosed | The report's tool version/platform vs the version(s) or platform the issue itself names as affected (the issue body or the repo-facts "latest release" line) | If the report's tool version or platform differs from what the issue itself states it targets, the report names the difference and its likely effect. This check does NOT cover a thread commenter's speculation about an unrelated dependency, or an incidental technique choice (e.g., an offline/dry-run flag) that does not change the mechanism under test | required |
| honest-conclusion | The report's stated Expected/Actual or conclusion, read against the artifacts actually shown in the same report | The conclusion claims no more than the shown artifacts support; an evidenced cannot-reproduce that names what differed and what a triggering setup might need is a pass; a confident claim (root cause, "guaranteed reproducible", certainty) with no artifact backing it fails. A secondary comparison remark (e.g. a control run described in a sentence and attributed to the issue's own thread) does not fail this check when the main claim is backed by a shown artifact and the remark asserts no cause or certainty | required |
| claim-specific-and-honest | The claim comment | Names the specific issue/behavior being investigated and a concrete next step, in the claimant's own words; a generic "I'll fix this" / "assign me" / unconditional promise of a fix or a date fails; interchangeable boilerplate that could be pasted onto any issue fails | required |
| ai-disclosure-compliant | The repo-facts contribution/AI-use policy vs the text of the claim comment and repro report | If the repo's stated policy requires disclosing AI assistance, the comment(s) disclose the tool and the extent of assistance; if the policy states no such requirement (or is permissive-with-responsibility, no disclosure required), this check passes automatically | required |
| control-run-shown | The repro report's steps/artifacts | Includes a control or comparison run that isolates the trigger (e.g., the same steps without the changed variable, or on an unaffected config) | preferred |

## Verdict rule

Accept if every `required` check passes. Any `required` check graded
`fail` or `unclear` holds the package at `reject`. `preferred` checks
are recorded but never change the verdict. `unclear` is treated as
`fail`: evidence the grader cannot verify is not evidence ready to
post.
