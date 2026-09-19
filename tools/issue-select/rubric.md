# Rubric: is this a good first issue?

Seven `required` checks decide the verdict. Each names one way a first
contribution dies outright: the repo cannot take a pull request, the project is
not moving, the contribution policy forbids the way I work, nobody with commit
rights has agreed the work should happen, the issue is bigger than one pull
request, the design is still being argued, or somebody is already on it.

Five `preferred` checks never change a verdict. They rank the issues the
required checks accept. Release cadence and maintainer response latency are
weighted `preferred` deliberately: they are the two signals most likely to look
damning on a perfectly healthy small project, so they rank rather than reject.

Day counts are measured against the bundle's capture date in eval mode, and
against today's date in live mode.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| repo-not-archived | Repo facts, the repo line: the `archived:` flag. Live mode: the read-only banner across the repo front page. | Pass when archived is no. Fail when archived is yes. An archived repo is read-only and cannot accept a pull request at all, so no other check can rescue it. | required |
| repo-still-committed | Repo facts, the `last 5 default-branch commits` list: the newest commit date in it. Live mode: the newest commit on the repo front page. | Pass when the newest of those 5 commits is dated within 180 days of the bundle capture date, or of today in live mode. A commit authored by a bot counts only when it merged a human pull request. Fail otherwise. Read the commit list itself, not the `last push to any branch` line, which keeps moving on stale side branches after the default branch has stopped. | required |
| ai-policy-allows-assisted-work | Repo facts, the `contribution policy` line. Live mode: CONTRIBUTING.md in the root or in .github, any contributor docs it links out to, AI_POLICY.md, AI_USAGE_POLICY.md, AGENTS.md, and the pull request template. | Pass when the policy is absent or silent, when it sets conditions such as disclosure, testing, human review or personal understanding of the change, when it merely discourages AI, or when it refuses fully AI-generated work while still allowing AI-assisted work. Fail only when it forbids AI-assisted contribution outright with no permitted path left open, for example a flat statement that the project does not accept AI-generated code or documentation. The question is whether a permitted path exists, not whether anything is prohibited. | required |
| maintainer-triaged | The issue header: the opener's author_association, the label list, and the author_association of every commenter in the thread. | Pass when at least one of these holds: the opener is OWNER, MEMBER or COLLABORATOR; or the issue carries 1 or more labels; or 1 or more comments come from an OWNER, MEMBER or COLLABORATOR. Fail when all three are absent, that is 0 labels plus a non-maintainer opener plus 0 maintainer comments. Nobody with commit rights has agreed this should be built, so the product decision is still open and a pull request is a gamble. | required |
| one-pr-sized | Repo facts, the `linked PRs` line with each pull request's state, plus the issue title and body. | Pass when fewer than 2 linked pull requests are merged AND fewer than 2 are closed unmerged AND the issue does not describe itself as a tracking, umbrella, meta or mega issue AND its body is not mainly a list of 5 or more other issue numbers. Fail otherwise. Two or more merged pull requests against a still-open issue mean the issue outlives its own pull requests, which is what an umbrella is; two or more closed unmerged ones are abandoned attempts. Touching several files, or listing several bullets or sub-steps that one pull request would close together, is not a fail. | required |
| design-settled | The issue `opened` date measured against the capture date, and the comment total in the Comments header. | Pass unless BOTH of these hold: the issue has been open 365 days or more AND the thread carries 10 or more comments. That combination is a design still being argued, with no settled spec to implement. Age on its own never fails this check: an old but quiet issue, with fewer than 10 comments, is usually a real task nobody got to. | required |
| nobody-already-on-it | Repo facts, `assignees` and `linked PRs` with state, plus every comment in the thread with its date. | Fail when any of these holds: assignees is non-empty; 1 or more linked pull requests are open; a claim comment such as working on this, can I work on this, /assign or a bot claim command is dated within 180 days of the capture date; or a maintainer reserves the issue for a named person. Pass otherwise. A claim older than 180 days with no open pull request behind it is stale and does not block, especially where a maintainer has invited takers. In live mode, apply any claim house rule stated in scope.md before grading this check. | required |
| maintainer-answers-issues | Repo facts, the `maintainer first-response sample` block. Live mode: the Issues tab sorted by recently updated, timing the first Owner, Member or Collaborator reply on each. | Pass when 1 or more sampled issues drew an owner, member or collaborator first response within 30 days. Weighted preferred rather than required because a thin or all-silent sample is a gap in the sample, not proof of a dead repo: plenty of healthy small projects answer issues in code rather than in comments, and commit recency already carries liveness. | preferred |
| ships-releases | Repo facts, the `latest release` line. Live mode: the Releases box in the right sidebar of the repo front page. | Pass when a release is tagged within 365 days of the capture date. Weighted preferred rather than required because a project that has never published a release at all can still be actively developed and actively used; requiring a release would reject healthy repos for a packaging habit they never adopted. | preferred |
| newcomer-signposted | The issue label list and the issue body. | Pass when the issue carries a good first issue, help wanted, docs or documentation label, or the body names the files or directories to start in, or the body states acceptance criteria. Ranks accepted issues only: signposting is the difference between a first issue I can start today and one I have to reverse-engineer first. | preferred |
| small-surface | The issue body: the files, directories or component areas it names as needing change. | Pass when the issue names 3 or fewer files, directories or component areas, or names one explicit file and line. Ranks accepted issues only. A larger named surface is still workable, it just costs more of the two weeks I have. | preferred |
| recently-opened | The issue `opened` date measured against the capture date. | Pass when the issue was opened within 180 days of the capture date. Ranks accepted issues only: an older issue that is still valid, still unclaimed and still wanted is perfectly takeable, it is just less likely to still match the code than a fresh one. | preferred |

## Verdict rule

Accept when every `required` check grades `pass`. Any single `required` check
graded `fail` rejects the issue, however strong the rest look: each required
check names a way a first contribution dies outright, and they do not trade off
against one another.

`preferred` checks never change a verdict. Grade and report them anyway. On an
accepted issue they are the ranking signal: prefer the accepted candidate with
the most `preferred` passes, and in live mode break any remaining tie with the
fit profile in `scope.md`. A failed `preferred` check is never a reason to
reject, and a clean sweep of `preferred` passes never rescues a failed
`required` one.

`unclear` on a `required` check counts as `fail`. A first issue whose safety I
cannot verify is not a first issue I should take. Two qualifications:

- Do not grade a check `unclear` when its pass condition already names the
  absent-evidence case. Silence in the contribution policy is a `pass` on
  `ai-policy-allows-assisted-work`. `assignees: none` together with
  `linked PRs: none` is a `pass` on `nobody-already-on-it`.
  `latest release: none published` is a `fail` on `ships-releases`, which is
  preferred and therefore costs a rank rather than the verdict.
- `unclear` on a `preferred` check behaves like any other non-pass there: it
  costs the issue a rank, never the verdict.
