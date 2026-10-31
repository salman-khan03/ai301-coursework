The scan found three issues that pass your rubric. The best fit is **VFS-Go-Aero/CAD-for-isaacsim#8**, a one-file Python bug fix with the fix and acceptance criteria already written out. Before starting any of them, leave a short claim comment on the issue, as `scope.md` asks.

**What the scan did:** I ran your three queries exactly as written in `search-strategy.md` and got about 300 results. Filters 1–5 dropped `codepath/*` repos, Hacktoberfest issues, bad labels (including `stale`) and two repos with too many results (redux-space/atraport-api with 10, onfranciis/OID with 9). Every top-5 result has 1 comment, so the newest-first tie-break decided the order. I graded each one with your rubric. Shell scripts, Python and jq piping needed approval in this session, so I did the ranking by hand from compact `gh --jq` rows.

## Accepted, best fit first

1. **VFS-Go-Aero/CAD-for-isaacsim#8**: the bridge script rejects `--device cuda` and `--headless`. Came from query 1 (Python bugs), ranked #2 (1 comment, updated Oct 30).
   - **Why it fits:** small Python bug fix in one file (`simulation/px4_bridge.py`). Both preferred checks pass: it has the `good first issue` label, and it names the line, gives the fix and lists acceptance criteria.
   - **Catch:** checking the acceptance criteria needs Isaac Sim and a CUDA GPU.
2. **fitbenchmarking/fitbenchmarking#1767**: the docs still link to pygsl's old SourceForge page. Came from queries 1 and 3, ranked #5 (1 comment, updated Oct 29).
   - **Why it fits:** a small docs fix in a Python repo, the kind your profile asks for. The old link is in two `.rst` files. It has the newcomer label, but the body doesn't say which files to change.
   - **Catch:** a contributor commented "Linked/duplicate of #1588". That issue is open and unassigned; read it before you start.
3. **torch-spyre/spyre-inference#672**: a native RMS norm change broke benchmarking. Came from query 1, ranked #4 (1 comment, updated Oct 29).
   - **Why it's last:** it passes every rubric check, but it's a compiler/backend bug for IBM's Spyre accelerator. That's the deep domain your profile says to avoid, and it needs Spyre hardware.
   - **Also:** a bot comment says the repo's automation "may pick it up itself" if nobody claims it.

## Rejected

4. **MieuxVoter/presidentielle2027#157** (ranked #1): failed Unclaimed. A pull request (#302) was opened on 2026-10-31 and is still open.
5. **Arize-ai/openinference#3774** (ranked #3): failed Unclaimed. The issue author's PR #3776 is open, and someone posted "I'd like to take this one" on 2026-10-29.

## Friction to log

I couldn't save these to `scan-notes.md`: you'd need to approve writing that file. Each is a possible change to your strategy or rubric.
- **Stale date window:** the queries still use created on or before 2026-10-01 and updated on or after 2026-09-15. Your strategy says to move both forward; I ran them as written.
- **Newest-first tie-break:** this pushed freshly claimed issues to the top. Ranks #1 and #3 were recent *because* someone had just claimed them.
- **Filter 3:** `stage/stalled` gets through, because "stalled" doesn't contain "stale".
- **Filter 4:** it only reads titles, so torch-spyre#672 got through without any of the trigger words.
- **Rubric gaps:**
  - It doesn't say how to treat "duplicate of #N" comments.
  - It doesn't cover a bot that might auto-fix an unclaimed issue.
- **Classroom labels:** Arize's `good student issue` label marks an issue for another course (CS146S), similar to why you drop `codepath/*` repos.

```json
[
  {"item": "https://github.com/VFS-Go-Aero/CAD-for-isaacsim/issues/8",
   "checks": [
     {"name": "Repo liveness", "grade": "pass", "evidence": "archived=false, pushed 2026-10-30T22:44Z (1 day before today)"},
     {"name": "Unclaimed", "grade": "pass", "evidence": "assignees [], no linked PRs, only comment is the author's 2026-10-30 rescope note"},
     {"name": "Bounded scope", "grade": "pass", "evidence": "single file fix in simulation/px4_bridge.py with code snippet, acceptance criteria and explicit 'Out of scope' list"},
     {"name": "AI-contribution policy", "grade": "pass", "evidence": "repo root has no CONTRIBUTING/AI policy/templates: silent"},
     {"name": "Newcomer-friendly label", "grade": "pass", "evidence": "labels: bug, good first issue"},
     {"name": "Actionable detail", "grade": "pass", "evidence": "names px4_bridge.py line 63, gives fix code and acceptance criteria"}],
   "verdict": "accept"},
  {"item": "https://github.com/fitbenchmarking/fitbenchmarking/issues/1767",
   "checks": [
     {"name": "Repo liveness", "grade": "pass", "evidence": "archived=false, pushed 2026-10-30T09:59Z"},
     {"name": "Unclaimed", "grade": "pass", "evidence": "assignees [], no linked PRs, only comment 'Linked/duplicate of #1588' (not a claim)"},
     {"name": "Bounded scope", "grade": "pass", "evidence": "'update any links to http://pygsl.sourceforge.net/' to the new repo: link swap in 2 .rst files"},
     {"name": "AI-contribution policy", "grade": "pass", "evidence": "PR template and docs/source/contributors/*.rst contain no AI policy text: silent"},
     {"name": "Newcomer-friendly label", "grade": "pass", "evidence": "label 'Good first issue'"},
     {"name": "Actionable detail", "grade": "fail", "evidence": "body names replacement URLs but no files, repro or acceptance criteria"}],
   "verdict": "accept"},
  {"item": "https://github.com/torch-spyre/spyre-inference/issues/672",
   "checks": [
     {"name": "Repo liveness", "grade": "pass", "evidence": "archived=false, pushed 2026-10-30T18:03Z"},
     {"name": "Unclaimed", "grade": "pass", "evidence": "assignees [], no linked PRs, only comment is hive bot invitation (no claimant)"},
     {"name": "Bounded scope", "grade": "pass", "evidence": "single regression from PR #509 with repro command and error; no umbrella/TBD/bot-opened condition"},
     {"name": "AI-contribution policy", "grade": "pass", "evidence": "bot comment 'welcomes contributions made with your own AI coding agent'; AGENTS.md present"},
     {"name": "Newcomer-friendly label", "grade": "pass", "evidence": "labels: good first issue, help wanted"},
     {"name": "Actionable detail", "grade": "pass", "evidence": "body gives reproduction command and the RuntimeError text"}],
   "verdict": "accept"},
  {"item": "https://github.com/MieuxVoter/presidentielle2027/issues/157",
   "checks": [
     {"name": "Repo liveness", "grade": "pass", "evidence": "archived=false, pushed 2026-10-31T15:14Z"},
     {"name": "Unclaimed", "grade": "fail", "evidence": "linked PR #302 by broija is OPEN (created 2026-10-31)"},
     {"name": "Bounded scope", "grade": "pass", "evidence": "fix the wrong filename for one of two poll groups in polls.csv"},
     {"name": "AI-contribution policy", "grade": "pass", "evidence": "CONTRIBUTING.md: 'Les contributions réalisées avec l'aide d'une IA ... sont autorisées' with conditions"},
     {"name": "Newcomer-friendly label", "grade": "pass", "evidence": "labels: good first issue, help wanted"},
     {"name": "Actionable detail", "grade": "pass", "evidence": "table of affected poll_ids and file polls.csv named"}],
   "verdict": "reject"},
  {"item": "https://github.com/Arize-ai/openinference/issues/3774",
   "checks": [
     {"name": "Repo liveness", "grade": "pass", "evidence": "archived=false, pushed 2026-10-31T04:25Z"},
     {"name": "Unclaimed", "grade": "fail", "evidence": "linked PR #3776 OPEN; claim comment 'I'd like to take this one' on 2026-10-29"},
     {"name": "Bounded scope", "grade": "pass", "evidence": "single wrapper fix in v1QueryWrapper.ts, labelled size:M"},
     {"name": "AI-contribution policy", "grade": "pass", "evidence": "CONTRIBUTING has no AI ban; AGENTS.md present"},
     {"name": "Newcomer-friendly label", "grade": "pass", "evidence": "label 'good first issue'"},
     {"name": "Actionable detail", "grade": "pass", "evidence": "repro script, expected behavior and source file named"}],
   "verdict": "reject"}
]
```
