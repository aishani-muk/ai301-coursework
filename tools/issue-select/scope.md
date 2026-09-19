# Scope: where to look, and who is looking

<!--
This file is the skill's field of view. The rubric (rubric.md) decides
whether an issue is GOOD; the scope decides which issues are candidates
at all, and whose hands the issue would land in. It applies in live mode
only: in eval mode the bundle is the whole world and this file is
ignored.

Two parts. Staff wrote the first; you write the second.
-->

## Where candidates come from

Only issues in the course's Path Review repository are candidates:

- Repo: `codepath/pathreview-ai301-fa26-s1`

Do not search, fetch, or grade issues from any other repository, however
promising. The wider GitHub comes later in the course; for now the field
is Path Review.

**Path Review house rule.** Path Review is a classroom, and your
classmates are not strangers. Ignore the usual claim signals here: other
students' claim comments (and there may be several on one issue) do not
block an issue, and finding some on the issue you want is normal. Claim
anyway: course credit attaches to the pull request you open, not to
whether it merges, so a shared issue costs nobody anything. Everything
else in the rubric applies as written.

## Your fit profile

<!-- YOU write this part: a few sentences about you. What languages and
tools you have actually used, what you want to get better at, anything
you want to avoid. The skill uses this only to RANK the issues your
rubric accepts, never to change a verdict: fit cannot rescue an issue
your rubric rejects, and cannot sink one it accepts. -->

I hold an MS in Computer Science with substantial research experience in AI,
and I also work as a full-stack developer. Day to day that means Python and
JavaScript/TypeScript: FastAPI-style backends, SQLAlchemy models, pytest, and
React on the front end. I have spent real time inside large unfamiliar
codebases, so tracing behaviour across several modules is ordinary work for me
rather than an obstacle.

**Rank Tier 3 issues first.** Tier 1 and Tier 2 issues are below my level and I
do not want one; a localized bug fix or a test-fixture correction is not worth
my two weeks even when it is the cleanest available PR. Rank an issue higher,
not lower, when it requires understanding how the whole system fits together.
Where the tier label and the apparent size of the work disagree, trust the
depth of understanding the issue demands.

What I am looking for is work that is research-worthy or genuinely useful in a
production setting — ideally both. Concretely, rank highest:

- Evaluation and measurement infrastructure: offline eval runners, benchmark
  sets, quality metrics, regression detection for model output.
- Retrieval quality and the RAG pipeline itself: re-ranking, chunk scoring,
  embedding freshness and staleness, cross-document synthesis.
- AI safety and robustness: prompt-injection defense and red-teaming, bias and
  fairness auditing, content-filter bypass, safety telemetry.
- Agent architecture: planning, tool orchestration, state persistence across
  restarts, end-to-end agent testing.

Rank lower: front-end and UI work, pure CI or dependency-management chores,
documentation-only tasks, and anything whose substance is de-indenting a
fixture or renaming things.

I am willing to take on ambiguity and a larger surface in exchange for a
problem worth solving. An issue with no failing test, an open design question,
or a body that only describes a symptom is acceptable here — I would rather
scope the problem myself than be handed a one-line fix. I have roughly two
weeks of part-time capacity, so the constraint is that the work must be
demonstrable within one substantial pull request, not that it must be small.
