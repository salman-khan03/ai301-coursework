# Search strategy: where your scout looks, and why

<!--
THIS IS THE PART YOU WRITE. The scout in SKILL.md executes whatever
queries and filters you define here. It ships empty on purpose: the
strategy is your judgment about where YOUR first contribution lives,
and a scan without authored queries is just someone else's defaults.

A filled strategy must contain:

1. At least one query in the Queries section. Each query is a real
   `gh search issues` invocation, and each should trace to a sentence
   of your fit profile in scope.md: if you cannot say which sentence
   produced a query, the query is probably not yours. Search breadth
   is the point at this stage: prefer generous limits and let the
   ranking filters do the narrowing.

2. At least one ranking filter in the Ranking filters section. A
   filter is a condition or an ordering over fields the search
   results already carry (last activity date, assignee presence,
   label set, comment count, repo). It must be checkable from the
   result fields alone: if deciding it would need opening the issue
   page, it belongs in your rubric, not here. Ranking is the free
   stage; keep it free. Make at least one filter an ordering, or
   state your tie-break: with drop conditions alone, the scout takes
   the top of whatever order the searches happened to return, and
   the graded candidates stop being traceable to you.

3. The top-N line, kept or deliberately changed.
-->

## Queries

<!-- Real gh search invocations, one per line, each with a comment
naming the fit-profile sentence it came from. Example SHAPE (write
your own; these values belong to nobody):

gh search issues --label "good first issue" --language python --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url
-->

<!-- Shared window, tried by hand first. Without it every result was
updated today, mostly on tiny new repos and Hacktoberfest farms.
Move both dates forward when re-scanning. -->

Profile: "I'm most comfortable in Python" + "a small, well-scoped bug fix"
gh search issues --label "good first issue" --label bug --language python --comments "1..6" --created "2026-06-01..2026-10-01" --updated ">=2026-09-15" --no-assignee --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url -- -linked:pr

Profile: "JavaScript/TypeScript (React)" + "a small, well-scoped bug fix"
gh search issues --label "good first issue" --label bug --language typescript --comments "1..6" --created "2026-06-01..2026-10-01" --updated ">=2026-09-15" --no-assignee --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url -- -linked:pr

Profile: "docs/UI cleanup in a language I already know"
gh search issues --label "good first issue" --label documentation --language python --comments "1..6" --created "2026-06-01..2026-10-01" --updated ">=2026-09-15" --no-assignee --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url -- -linked:pr

## Ranking filters

<!-- Conditions and orderings over the free result fields, applied in
the order written. State each so a stranger could apply it and get
your ordering. Example SHAPES (write your own):

- drop any result with an assignee
- drop any result not updated in the last 60 days
- prefer fewer comments over more
-->

1. Drop any result whose repo owner is `codepath`: those are classmates' classrooms, not the wild.
2. Drop any result whose title or labels contain "hacktoberfest" (case-insensitive): farmed tasks.
3. Drop any result with a label containing "wontfix", "duplicate", "invalid", "blocked", "stale", "stalled", or "student" (case-insensitive).
4. Drop any result whose title contains "compiler", "simulation", "numerical", or "solver" (case-insensitive): profile says to avoid deep domains.
5. Drop every result from a repo with 8 or more results in the combined list: one owner's backlog would fill the top five.
6. Order by comment count, fewest first: fewer comments means fewer strangers already on it.
7. Break comment-count ties by `updatedAt`, oldest first: newest-first rewarded issues touched because someone had just claimed them.
8. Keep at most 2 results per repo, the highest-ranked ones, then take the top N.

## How many to grade

Grade the top **5** ranked results per scan.

<!-- Five is the default because grading is the only stage that costs
money: about $0.20 per candidate, so a scan is about $1, and you can
iterate the strategy several times for less than one full regression
run. You need one good issue, not ten. Raising this number is
allowed, but it is a decision you state here in a sentence, with the
cost you are accepting; it is not a number you bump because a scan
disappointed you once. -->
