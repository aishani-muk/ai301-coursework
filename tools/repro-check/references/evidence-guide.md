# Evidence guide: where proof lives in a reproduction package

## Environment

**Where it lives.** In an eval bundle: the opening lines of the candidate repro
report, usually an "Environment:" line or a small table, plus the repo-facts
block's `bug reports:` line, which names what this project's template considers
placeable. Read both against the issue's own version and platform statement,
which sits either in the issue body's header or inline in its prose. In live
mode: the issue's rendered template fields on GitHub, the repo's
`.github/ISSUE_TEMPLATE/` for what it asks, and the student's draft repro
comment for the candidate side.

**What good looks like.** The report states the version of the software under
test and the platform it ran on, and a stranger reading them can tell whether
their own machine is comparable. Beyond those two, the sufficient record is the
one the issue itself makes decisive: if the issue says the failure depends on
the driver, the driver is named; on the shell, the shell is named; on the build
profile, the profile is named; on the browser's language order, that order is
named. The shape to aim for records not just the tool version and OS but the
specific quantity the issue turns on — an issue about a command-line size limit
wants `getconf ARG_MAX` in the record.

An environment record is not sufficient because it is long, and not
insufficient because it is one line: a single line naming the version, the
install method and the OS is enough. An absent record is never rescued by a
convincing artifact. A package whose steps and output are perfect but which
never says what it ran on cannot be placed by anyone, and belongs on hold — this
is true even when the artifact shows exactly the right panic, because on an
issue where the build profile changes the failure mode, an unplaceable run
proves nothing about the reported one.

## Steps

**Where it lives.** In an eval bundle: the numbered steps or the shell
transcript inside the candidate repro report, read against the "Steps to
reproduce" section of the issue context. In live mode: the student's draft repro
comment, compared with the issue's own reproduction, and any linked playground,
gist, or sample repo — which you should open and confirm still exists.

**What good looks like.** A stranger with the stated environment and nothing
else can get from a clean starting state to the trigger. Every input the outcome
depends on is either pasted, constructed by a command in the steps, or pointed
at somewhere the reader can also reach. Three forms all qualify: pasting the
input outright (`printf 'foo: bar\n' > some.yml`), constructing it in place (a
`mkdir -p` / `git init` / `ln -s` sequence that builds the layout), or naming
the issue's own reproduction precisely enough to rebuild it (stating that the
issue's script was run with shape A at rangeStart 19, rangeEnd 32 and shape B at
rangeStart 18, rangeEnd 31, parser `babel`, which is the whole input). A
deliberately reduced input is fine and often better than the original, as long
as the report says it reduced.

Steps fail when a reader cannot follow them, not when they are few. Two failure
shapes. The run depends on something private, so nobody else can ever attempt
it: a reproduction inside a company monorepo the writer says they cannot share,
with an internal config also not shareable, is unrunnable however clearly it is
narrated. Or the steps stop short of the trigger the issue names, so what was
run is not the reported scenario: a bare `minikube start` on an issue whose step
one is `minikube start --driver vmware` has not reached the bug. Count the steps
for nothing; terse and complete is complete.

## Behavior shown

**Where it lives.** In an eval bundle: the fenced blocks inside the candidate
repro report (command output, log excerpts, produced CSS, console transcripts,
exit statuses), and the corresponding blocks in the issue body showing what the
reporter saw. Put them side by side; this pair is usually what decides the whole
package. In live mode: the artifacts pasted into the student's draft, against
the issue's own "actual behavior" block and any screenshots or logs in the
thread.

**What good looks like.** The artifact shows the issue's failure, identified by
failure mode rather than by topic. Ask: is this the same error class or message,
the same exit status, the same wrong value, the same missing output? An artifact
that fails in the same feature area but in a different way is an adjacent
symptom, and it is the single most common way a confident package is wrong. Four
worked shapes of that error:

- The issue reports a `capacity overflow` panic at exit 101; the report's block
  is an argument-validation error at exit 1. Both are failures of the same flag.
  They are not the same failure, and exit 1 is in fact the graceful handling the
  issue is asking for.
- The issue reports a runtime path error; the report's block is a compile error
  caused by the report's own edit to the expression.
- The issue says the terminal crashes; the report's own observed result says the
  window remained open and the prompt returned.
- The issue shows a `panic: not a string` with a stack trace; the report's block
  is a graceful syntax error, because the input used a colon where the issue
  used an equals sign.

The negative case is not a failure of this family. When the report's stated
result is that the behavior did not appear, the artifact's job changes: it must
show what the attempt actually produced, so a reader can see a real run happened
and judge the attempt. A marker-order log from a run that came out in the wrong
order, or a prompt rendering the path the issue says vanishes, satisfies this
family completely.

Two anti-patterns to name out loud, because both look like evidence. Setup
artifacts: output that proves only that the tool is installed and running — a
`--version` banner and a session list say nothing about the blank pane or the
log stall the issue describes. Absent artifacts: a report with no fenced block
at all has not shown behavior, however certain its prose. Formatting is not
evidence; the best-looking reports in a set are often the ones showing the wrong
thing.

## Honesty

**Where it lives.** In an eval bundle: the report's summary and conclusion
sentences, its "Expected"/"Actual" pair, and any certainty words anywhere in it
(confirmed, verified, 100 percent, guaranteed, conclusively, verifiably, every
single time, on two machines) — each read against the artifacts in the same
report, and nowhere else. In live mode: the same sentences in the draft, plus
whether the thread's maintainers have already contradicted the claim.

**What good looks like.** The strongest sentence in the report is one the shown
evidence can carry, and the report names the edges of what it did not establish.
An evidenced cannot-reproduce is a full pass here, and it is often the most
honest package in a set: a report that states plainly it could not reproduce,
scopes itself to the scenario it actually tested, and says what a triggering
setup probably needs is doing exactly what a reproduction attempt is for.
Honesty is not modesty either — a flat "matching the report exactly" is honest
when the outputs are right there.

Dishonesty here is a claim outrunning its evidence, in four recurring shapes.
Confirmation over the wrong artifact: "conclusively demonstrates the reported
bug" sitting above a compile error. Root cause asserted without a run: "I
verified this race condition is the cause" with no transcript anywhere.
Reliability asserted without shown repetition: "guaranteed reproducible on my
end" with nothing measured, or "I ran this ten times" attached to the wrong
command. Scope inflated past what was tested: claiming a run demonstrates the
problem is not limited to development builds, in a thread where a maintainer
could not reproduce it at all.

Read the "Expected" line as evidence in its own right. A report that writes
"Expected: one of the three panes comes up blank" has stated the bug as the
expectation, which shows the writer is narrating the issue rather than observing
their own run.

## Comms

**Where it lives.** In an eval bundle: the candidate claim comment — a distinct
section, graded on its own, independently of how good the repro report is — plus
the repo-facts block's `bug reports:` template line and its `contribution
policy:` line, and the thread highlights for what maintainers have already asked
for. In live mode: the student's draft claim comment, the repo's
`CONTRIBUTING.md`, `AI_POLICY.md` or `AI_USAGE_POLICY.md` where one exists, the
issue template, and the existing thread.

**What good looks like in the claim comment.** It tells that thread something
only this person could have written, and it tells the truth about what comes
next. Two observable tests. First, the swap test: could this comment be pasted
unchanged onto a different issue? A "+1 also seeing this, any updates?" or an "I
know exactly what the reporter is talking about" passes the swap test and
therefore fails this family. Second, the promise test: does the comment promise
anything the writer does not control? A guaranteed date, a request to reserve or
assign the issue, and no fact drawn from this issue anywhere in it, is the
canonical bad claim — and it can sit above a genuinely good repro report, which
is why the claim comment must be graded alone.

Good comments name a concrete next step drawn from this issue: the specific
function whose path they intend to read, the maintainer's own pointer they mean
to follow, the two configurations they mean to test the draft patch against.
A claim comment can be excellent while reporting a failure — saying plainly that
the behavior did not reproduce and naming what comes next is a strong claim, not
a weak one. Modesty is not vagueness.

**The AI-disclosure rule, which is conditional.** Read the repo-facts
contribution-policy line before deciding anything about disclosure, and treat
that line as the whole rule. Then apply the operating premise this course
states: a course package is AI-assisted work. That premise matters because
without it a grader reasons "the bundle never says AI was used, so there is
nothing to disclose" and lets a mandatory-disclosure repo through. Assume the
assistance happened; ask only whether this repo requires saying so.

Four policy shapes, and what each demands:

1. **Silent or standard.** The policy line reads as a standard contribution
   guide with no stated AI policy. No disclosure is owed and its absence is not
   a defect. This is most repos. A rubric that demands a disclosure sentence
   here wrongly rejects half the clear accepts.
2. **Permissive or responsibility-only.** The policy welcomes AI use but asks
   the contributor to understand and own the result: "generative AI tools
   welcome; you are responsible for all contributions", "only submit code you
   fully understand and have tested", or "fully AI-generated contributions are
   not accepted; assistive AI use is allowed". No disclosure sentence is
   required. A package that volunteers one anyway is exemplary and still passes
   on either reading.
3. **Own-words rules.** "Comments to maintainers must be written by humans in
   their own words, and AI-generated comments may be hidden", or "comments are
   expected to be in the contributor's own words and voice" with disclosure
   asked in the pull request. These are voice requirements, not disclosure
   requirements. They are satisfied by a comment that reads as one specific
   person's own writing about this issue, and violated by generic assistant
   prose — never by the absence of a disclosure line. Note the shape carefully:
   a repo can mandate disclosure for pull requests and not for issue comments,
   and a reproduction package is issue comments.
4. **Mandatory disclosure.** The policy says AI use must be disclosed, in terms
   that reach comments — "all AI usage in any form must be disclosed, stating
   the tool used and the extent of the assistance", often alongside a
   requirement that AI-assisted comments be reviewed and edited by a human
   before submission. Here the comments must say that AI assistance was used and
   how far it went. A package can be first-rate on every proof family — a real
   release-build run with a control configuration — and still not be ready to
   post, because neither comment mentions AI anywhere. Do not talk yourself out
   of that verdict because the proof is good: a repo's stated policy is a wall,
   not a rubric line to trade against.

The order of operations is what keeps this correct: read the policy line, decide
whether a disclosure is owed at all, and only then look for one. Looking for a
disclosure sentence first and reasoning backwards produces both errors at once —
it rejects every silent repo and lets the mandatory one through.
