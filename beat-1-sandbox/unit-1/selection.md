# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/14

**Verdict output**

Live-mode run of my installed `issue-select` skill on three candidates
(#14, #10, #29). All three were accepted; #14 ranked first. Pasted verbatim below,
ending with the fenced JSON verdict block.

````
All three issues are accepted, and I'd take #14 first. All three pass every required check and tie on the preferred checks, so the fit profile in `scope.md` sets the order.

**Ranking (accepted)**
1. **#14, offline eval runner.** This is evaluation and measurement infrastructure, the first item on your list. It also needs the whole system in your head: `scripts/run_evals.py`, `rag/evaluator/eval_suite.py`, the benchmark fixture, `make eval` and the RAG Evaluation workflow that reads `eval_results.json`. It carries the `tier-3` label and the design is open, which suits you. Its one dependency, `basic_profile.json`, was the subject of #43, now closed. I didn't open the file to confirm it was restored.
2. **#10, LLM re-ranking of retrieved chunks.** This is retrieval quality in the RAG pipeline and is research-flavoured (the reranker design and the cost/quality trade-off are open). It is narrower than #14, touching a new `reranker.py` and `hybrid.py`.
3. **#29, prompt-injection red-team suite.** This is AI safety, so it fits your profile. But the work is mostly a curated attack fixture plus one test file, so it asks for the least system-wide understanding of the three.

**Required checks (the same result for all three)**
- **repo-not-archived:** pass. `isArchived: false`.
- **repo-still-committed:** pass. The newest of the last 5 default-branch commits is 2026-09-16, from a human author (3 days ago).
- **ai-policy-allows-assisted-work:** pass. There is no CONTRIBUTING.md, AI_POLICY.md, AI_USAGE_POLICY.md or AGENTS.md. The PR template has no AI restriction.
- **maintainer-triaged:** pass. Each opener is a COLLABORATOR and each issue has labels: #14 has 5, #10 has 3 and #29 has 5.
- **one-pr-sized:** pass. There are 0 pull requests in the repo. #14's only cross-reference is #43, which is a closed issue, not a PR. None of the three is an umbrella.
- **design-settled:** pass. Each was opened 2026-09-10 (9 days ago) with 0 comments.
- **nobody-already-on-it:** pass. All three have no assignees, no linked PRs and no comments, so there are no claims. The house rule about classmates' claims never came into play.

**Preferred checks (the same on all three: 4 of 5 pass)**
- **maintainer-answers-issues:** pass. Collaborator first replies on sampled issues #52 and #43 came about 6 days after they opened.
- **ships-releases:** fail. `latestRelease` is null. This costs a rank only.
- **newcomer-signposted:** pass. Each has a `tier-3` label and names the files to start in.
- **small-surface:** pass. #14 names 3 files, #10 names 2 and #29 names 2.
- **recently-opened:** pass. Each is 9 days old.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/14",
    "checks": [
      {"name": "repo-not-archived", "grade": "pass", "evidence": "isArchived: false"},
      {"name": "repo-still-committed", "grade": "pass", "evidence": "Newest of last 5 default-branch commits is 2026-09-16 by human Aburke225, 3 days before 2026-09-19"},
      {"name": "ai-policy-allows-assisted-work", "grade": "pass", "evidence": "No CONTRIBUTING/AI_POLICY/AI_USAGE_POLICY/AGENTS files; PR template has no AI restriction"},
      {"name": "maintainer-triaged", "grade": "pass", "evidence": "Opener author_association COLLABORATOR; labels enhancement, devops, rag, tests, tier-3"},
      {"name": "one-pr-sized", "grade": "pass", "evidence": "0 linked PRs (only cross-ref #43 is a closed issue, not a PR); body is a single bounded task, not an umbrella"},
      {"name": "design-settled", "grade": "pass", "evidence": "Opened 2026-09-10 (9 days), 0 comments"},
      {"name": "nobody-already-on-it", "grade": "pass", "evidence": "assignees: none, linked PRs: none, 0 comments so no claims"},
      {"name": "maintainer-answers-issues", "grade": "pass", "evidence": "Collaborator first replies on #52 and #43 about 6 days after opening"},
      {"name": "ships-releases", "grade": "fail", "evidence": "latestRelease: null, no release published"},
      {"name": "newcomer-signposted", "grade": "pass", "evidence": "Names files to start in and carries tier-3 label"},
      {"name": "small-surface", "grade": "pass", "evidence": "Names 3 files: scripts/run_evals.py, rag/evaluator/eval_suite.py, basic_profile.json fixture"},
      {"name": "recently-opened", "grade": "pass", "evidence": "Opened 2026-09-10, 9 days old"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/10",
    "checks": [
      {"name": "repo-not-archived", "grade": "pass", "evidence": "isArchived: false"},
      {"name": "repo-still-committed", "grade": "pass", "evidence": "Newest of last 5 default-branch commits is 2026-09-16 by human Aburke225, 3 days before 2026-09-19"},
      {"name": "ai-policy-allows-assisted-work", "grade": "pass", "evidence": "No CONTRIBUTING/AI_POLICY/AI_USAGE_POLICY/AGENTS files; PR template has no AI restriction"},
      {"name": "maintainer-triaged", "grade": "pass", "evidence": "Opener author_association COLLABORATOR; labels enhancement, rag, tier-3"},
      {"name": "one-pr-sized", "grade": "pass", "evidence": "0 linked PRs; body is a single bounded feature, not an umbrella"},
      {"name": "design-settled", "grade": "pass", "evidence": "Opened 2026-09-10 (9 days), 0 comments"},
      {"name": "nobody-already-on-it", "grade": "pass", "evidence": "assignees: none, linked PRs: none, 0 comments so no claims"},
      {"name": "maintainer-answers-issues", "grade": "pass", "evidence": "Collaborator first replies on #52 and #43 about 6 days after opening"},
      {"name": "ships-releases", "grade": "fail", "evidence": "latestRelease: null, no release published"},
      {"name": "newcomer-signposted", "grade": "pass", "evidence": "Names rag/retriever/ (new reranker.py) and hybrid.py; carries tier-3 label"},
      {"name": "small-surface", "grade": "pass", "evidence": "Names 2 files: rag/retriever/reranker.py (new), rag/retriever/hybrid.py"},
      {"name": "recently-opened", "grade": "pass", "evidence": "Opened 2026-09-10, 9 days old"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/29",
    "checks": [
      {"name": "repo-not-archived", "grade": "pass", "evidence": "isArchived: false"},
      {"name": "repo-still-committed", "grade": "pass", "evidence": "Newest of last 5 default-branch commits is 2026-09-16 by human Aburke225, 3 days before 2026-09-19"},
      {"name": "ai-policy-allows-assisted-work", "grade": "pass", "evidence": "No CONTRIBUTING/AI_POLICY/AI_USAGE_POLICY/AGENTS files; PR template has no AI restriction"},
      {"name": "maintainer-triaged", "grade": "pass", "evidence": "Opener author_association COLLABORATOR; labels enhancement, devops, safety, tests, tier-3"},
      {"name": "one-pr-sized", "grade": "pass", "evidence": "0 linked PRs; body is a single bounded test suite, not an umbrella"},
      {"name": "design-settled", "grade": "pass", "evidence": "Opened 2026-09-10 (9 days), 0 comments"},
      {"name": "nobody-already-on-it", "grade": "pass", "evidence": "assignees: none, linked PRs: none, 0 comments so no claims"},
      {"name": "maintainer-answers-issues", "grade": "pass", "evidence": "Collaborator first replies on #52 and #43 about 6 days after opening"},
      {"name": "ships-releases", "grade": "fail", "evidence": "latestRelease: null, no release published"},
      {"name": "newcomer-signposted", "grade": "pass", "evidence": "Names tests/security/test_prompt_injection.py and tests/fixtures/injection_attempts/; carries tier-3 label"},
      {"name": "small-surface", "grade": "pass", "evidence": "Names 2 paths: tests/security/test_prompt_injection.py, tests/fixtures/injection_attempts/"},
      {"name": "recently-opened", "grade": "pass", "evidence": "Opened 2026-09-10, 9 days old"}
    ],
    "verdict": "accept"
  }
]
```
````

---

## Eval iterations

**Run history**

Two runs, in order:

1. **Partial probe, `--only issue-12`** — agreement **1/1** scored item (verdict `reject`,
   gold `reject`). This was a deliberate single-issue probe, not an attempt at a scored
   run. I ran it because `issue-12` is the only member of the `policy` category in
   `gold-labels.json`, and the pass bar is not just 18/20 — it also requires at least one
   matching verdict in *every* category. If my AI-policy check had misread that one issue,
   the whole full run would have failed the category floor even at 19/20 agreement. At
   about $0.20, checking it first was cheaper than discovering it at the end of a $4 run.
   As a partial run it correctly refused to write `eval-run.txt`.

2. **Full run, all 20 scored issues** — agreement **20/20 scored items (bar: 18/20: PASS)**,
   with per-category tallies `claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 1/1
   scope 4/4`. This is the run saved with `--save-run eval-run.txt` and committed
   alongside this file.

The rubric was not revised between the two runs; the probe confirmed the policy check
rather than correcting it, so the first full run was also the final one. The 20/20 above
matches the agreement line in the committed `eval-run.txt`.

**Issue analysis**

**`issue-09`** (`conda/conda#7617`). My rubric decided **accept**. The gold label is
**accept**. They agree.

This is the issue I was most worried about getting wrong, because it carries three
separate signals that look like rejections and are not. It was opened **2018-08-03**,
almost eight years before the bundle's 2026-08-05 capture date. It carries a
`stale::recovered` label and a stale-bot comment. And a non-maintainer left a claim
comment on it — *"I'd like to take a swing at this"* — which on a naive reading means
somebody is already on it.

My rubric read it the way it did because each of those three signals is tested against a
second condition rather than on its own:

- `design-settled` graded **pass**, with the evidence *"Opened 2018-08-03 (>365 days) but
  only 4 comments (<10), so the both-conditions fail does not hold."* The check requires
  age **and** a busy thread together. An eight-year-old issue with four comments is not a
  design being argued; it is a real task nobody got around to.
- `nobody-already-on-it` graded **pass**, with the evidence *"assignees: none; only linked
  PR is closed; sole claim comment (2022-01-20) is >180 days old and stale; maintainer said
  'you can just give it a try'."* The claim is four years stale with no open PR behind it,
  and a MEMBER explicitly invited takers in reply.
- `one-pr-sized` graded **pass**: *"One linked PR (#11627, closed)."* My threshold is two
  or more, precisely so that a single abandoned attempt does not condemn an issue.

The two checks it did fail, `maintainer-answers-issues` and `recently-opened`, are both
weighted `preferred`, so they cost it a rank and not the verdict. Had I weighted either
as `required` — which was tempting, since a 2018 issue is obviously not "recent" — I would
have rejected a gold accept.

**Check rationale**

From the `rubric.md` uploaded to `tools/issue-select/`, the `design-settled` row, quoted
as it is currently written:

> | design-settled | The issue `opened` date measured against the capture date, and the comment total in the Comments header. | Pass unless BOTH of these hold: the issue has been open 365 days or more AND the thread carries 10 or more comments. That combination is a design still being argued, with no settled spec to implement. Age on its own never fails this check: an old but quiet issue, with fewer than 10 comments, is usually a real task nobody got to. | required |

The reasoning behind that form is the conjunction. My first instinct was a single
threshold on age — reject anything open more than a year — because a stale issue feels
like a bad bet. But age alone measures the wrong thing. What actually makes an old issue
dangerous for a first contribution is not that it is old; it is that there is no settled
answer to implement, so any PR walks into an argument. The signal for *that* is a long
thread, not a long calendar.

So the check fires only where both conditions hold, and the sentence *"Age on its own
never fails this check"* is in the row to stop a grader reaching for the intuitive
one-signal version. The 10-comment half is what saves `issue-09` (2,924 days old, 4
comments, gold accept), and the 365-day half is what stops the check firing on every
merely busy issue.

**Trade-offs**

What this check gives up is the young, loud design debate: an issue opened six weeks ago
that has already drawn forty comments of disagreement is exactly the trap `design-settled`
exists to catch, and it will sail through, because the 365-day condition is not met. I
accept that miss knowingly. Making the check fire on comment count alone would cost more
than it saves — `issue-05` has 106 comments and `issue-10` has a 125-issue body, and both
are rejected on `one-pr-sized` anyway, while `issue-09`'s four comments would not have
protected it from an age-only rule.

I can also say what the check did and did not change in the committed run, because I read
the per-check grades in `results.json`. `design-settled` graded `fail` on exactly two of
the twenty scored issues, `issue-15` and `issue-18`:

- `issue-15`: *"Opened 2021-08-18 (over 365 days before capture) and 97 comments (10 or
  more)"*
- `issue-18`: *"Opened 2025-06-15 (~416 days before 2026-08-05) and 11 comments: both
  conditions hold"*

Both were already being rejected by other required checks — `one-pr-sized` fired on both
(two closed-unmerged PRs each), and `nobody-already-on-it` fired on `issue-18` (four open
linked PRs plus claim comments dated 2026-07-06 and 2026-08-02). So on this eval set
`design-settled` changed no verdict at all: removing it entirely would still have produced
20/20. That is the honest result, and it tells me the check is currently carried by
redundancy rather than proven by it. Its value is insurance for live mode, where I expect
to meet exactly the kind of long-running architectural debate the eval set never isolated.

---

## Selection rationale

**Selection rationale**

**1. Fit to my interests and to the time available.**

I picked **#14, "Implement an offline eval runner that measures review quality across a
benchmark portfolio set"** — a Tier 3 issue. I have an MS in Computer Science and a good
deal of AI research experience alongside full-stack work, so a Tier 1 bug fix or a
test-fixture correction would not have taught me anything or shown anything. I wanted a
problem that is both research-worthy and the kind of thing a company actually needs, and
offline evaluation infrastructure is squarely that: measuring output quality against a
benchmark set, catching regressions before they ship, and producing a number you can
argue about. It is the problem I would want to work on outside a course.

It also demands the whole system, which is what Tier 3 is supposed to mean. The work
spans `scripts/run_evals.py`, `rag/evaluator/eval_suite.py`, the benchmark fixture in
`tests/fixtures/sample_profiles/`, the `make eval` target, and the RAG Evaluation CI
workflow that consumes `eval_results.json` — so I have to understand how the RAG pipeline,
the fixtures and CI fit together before I can write a line. On time: the issue estimates
7–10 hours, which fits the two weeks of part-time capacity I have, and the deliverable is
one substantial PR rather than an open-ended research programme.

**2. What the verdict identified correctly, and what I weighed that the rubric could not.**

The verdict was right about everything it is designed to see, and it was right that the
three candidates were indistinguishable on those grounds. All three passed all seven
required checks identically, and all three scored 4 of 5 on the preferred checks — even
failing the same one, `ships-releases`, because Path Review has published no releases at
all. That is a real result, not a shrug: it confirmed that none of these issues is
archived, abandoned, claimed, umbrella-sized or blocked by a contribution policy, which is
the whole job of the rubric. It also correctly disregarded the `ships-releases` failure as
a ranking cost rather than a verdict, which is exactly why I weighted it `preferred`.

What the rubric could not weigh is *why* #14 is worth more to me than #10 or #29. My
checks measure whether an issue is safe to take; they have no way to measure whether it is
interesting, or whether it will still matter in six months. Nothing in my checks can tell
that evaluation infrastructure is the load-bearing piece of an AI product while a
red-teaming fixture, however well-scoped, is a bounded test-writing task. That judgement
came from the fit profile in `scope.md` and from me, and the skill was honest about it —
it said the three tied on the checks and that the fit profile set the order. I also
weighed one thing the rubric explicitly cannot: the `small-surface` check counts named
files and would happily rank a two-file issue above a three-file one, but I want the
*larger* surface here, because breadth is the point of a Tier 3 issue. That is a case
where my own priorities run directly against a preferred check, and it is why that check
is preferred and not required.

**3. Anticipated difficulty in claiming it.**

Claiming should be straightforward, and the mechanics are the easy part. As of this run,
#14 has no assignees, no linked pull requests and no comments, and the repository has no
open PRs at all, so nobody is ahead of me. The Path Review house rule in my `scope.md`
says classmates' claim comments do not block an issue in any case, since credit attaches
to the PR I open. The maintainers are responsive — sampled issues #52 and #43 drew
COLLABORATOR replies about six days after opening — so I should get an answer if I need
one. I have not commented on the issue; per the Unit 1 instructions, choosing is not
claiming, and the claim comment is Unit 2's work.

The real difficulty is not claiming it, it is scoping it. #14 is a Tier 3 issue whose
design is genuinely open: the body says to "score the benchmark profiles" without saying
what a quality score *is*, and `EvalSuite` is currently dead code that nothing calls, so
I am partly designing the interface rather than fixing a defined defect. I will also need
to confirm that `tests/fixtures/sample_profiles/basic_profile.json` is actually usable —
the skill flagged that it was the subject of issue #43, now closed, and did not verify
whether the fixture was restored. That is my first job in Unit 2. The risk I am accepting
is scope creep: an eval runner can expand indefinitely, so I will need to agree a minimum
viable set of metrics with a maintainer early rather than discovering in Week 9 that I
built something larger than one PR.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
