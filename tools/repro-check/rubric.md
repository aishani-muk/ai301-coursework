# Rubric: is this reproduction package ready to post?

Eight `required` checks decide the verdict. Each names one way a reproduction
package wastes a maintainer's time: the artifact shows a different failure than
the issue reports, the central claim rests on nobody's word but the writer's,
the run cannot be placed because the environment is unrecorded, the run quietly
differed from what the issue targets, a stranger cannot re-run the steps, the
conclusion outruns the evidence, the claim comment says nothing only this person
could say, or the repo's stated disclosure policy went unmet.

One `preferred` check ranks accepted packages and never gates them.

Every pass condition below judges the outcome — what the artifacts show against
what the issue reports. None of them judges the write-up's shape. Length,
headings, bullet style and tone are not evidence: a four-line report with the
right transcript is ready, and a polished eight-section report over the wrong
artifact is not.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| behavior-matches-issue | The repro report's artifacts (command output, log excerpt, console transcript, produced CSS) read line by line against the behavior the issue's own body reports: its error text, exit status, wrong value, or missing output. | Pass if the artifact shows the failure the issue names, identified by the same failure mode: same error class or message, same exit status, same wrong output, not merely some failure in the same feature. Pass also when the report's stated result is a cannot-reproduce and the artifact shows what the attempt actually produced instead. Fail when the artifact shows a different failure mode than the issue reports, shows the software behaving normally while the text calls the run a confirmation, or when no artifact is shown at all. | required |
| claims-backed-by-artifact | Every sentence of the repro report that asserts what happened on the contributor's machine, set against the transcripts, logs, and outputs the report actually contains. | Pass if the report's central factual claim (it reproduces, it does not reproduce, this is the root cause) points at something in the report a reader can inspect and judge independently. Fail if that central claim rests only on the writer's say-so, or if the shown artifacts establish something adjacent instead (that the tool is installed, that a version banner prints, that a session exists). | required |
| environment-recorded | The repro report's environment statement, read against the versions and run conditions the issue's body names, and against the condition the issue itself says decides the outcome. | Pass if a stranger can tell from the report whether their own setup is comparable: the version of the software under test and the platform it ran on are both stated, plus any run condition the issue makes decisive (the driver, the shell, the build profile, the browser language order). Fail if any of those is missing, even when the artifact is otherwise convincing. | required |
| deviation-declared | The version, platform, and inputs the repro report states it used, compared against the version the issue targets and the exact command, expression, or snippet the issue gives. | Pass if the run matches what the issue targets, or the report names the difference in its own words (an older or newer release, a different OS or shell, a reduced or substituted input) so a reader can weigh it. Fail if the report ran a different version, syntax, expression, or input than the issue describes and presents the outcome as confirmation without naming the change. | required |
| steps-rerunnable | The steps section of the repro report, read as the instructions a stranger with no access to the contributor's machine would have to follow. | Pass if a reader could obtain or construct every input the outcome depends on (files, configuration, commands, or the issue's own reproduction referenced precisely enough to rebuild) and reach the action the issue says triggers the bug. Fail if any input the outcome depends on is private, unshared, or unspecified, or if the steps stop short of the trigger. | required |
| outcome-honest | The repro report's conclusion sentences, including their certainty words (confirmed, 100 percent, guaranteed, conclusively, verifiably), weighed against what its own artifacts establish. | Pass if the stated conclusion is no stronger than the artifacts support. A plainly stated cannot-reproduce backed by a real attempt is a pass, not a fail. Fail if the report claims confirmation, a root cause, reproducibility across runs or machines, or a scope wider than the issue (other releases, other platforms) that its shown evidence does not establish. | required |
| claim-comment-specific | The candidate claim comment alone, read against the issue it would be posted on. | Pass if the comment tells that thread something only this contributor could say: what they ran and what came of it (an honest failure to reproduce counts), plus a next step naming a file, function, configuration, or open question from this issue. Fail if the comment would read identically on any other issue, asks for the issue to be reserved or assigned in place of showing work, or promises a fix, a date, or an outcome the writer cannot guarantee. | required |
| disclosure-when-required | The repo-facts contribution-policy line, read first and treated as the controlling rule, then the claim comment and repro report searched for a disclosure sentence. | Treat every package as AI-assisted work, so a disclosure requirement is always live where one exists. Pass if the policy line states no AI-disclosure requirement reaching issue comments: silent, permissive, responsibility-only, own-words-only, or disclosure asked only in pull requests. A rule that comments be written in the contributor's own words is a voice rule, not a disclosure rule, and is satisfied by a specific human-sounding comment. If the policy does require disclosing AI use in any form or in comments, pass only when the comments state that AI assistance was used and how much; fail when they do not. | required |
| control-run-shown | The repro report, searched for a second run that differs in exactly the dimension the issue blames and produces the other outcome. | Pass if the report shows a contrasting or control run isolating the trigger, or names why one is not possible. Records whether an accept is strong or thin. Never changes the verdict. | preferred |

## Verdict rule

The verdict is accept only when every check weighted `required` grades `pass`.
A single required fail holds the package, and the verdict is reject. The
`control-run-shown` check is `preferred`: its grade is reported in the JSON and
never moves the verdict in either direction, so a package with no control run
can still be accepted and a package with a beautiful one can still be rejected.

On a required check, `unclear` counts as fail. Evidence I cannot verify is proof
that is not ready to post, and a required check whose evidence is genuinely
absent has already told me the package is missing something a reader will need.
On the preferred check, `unclear` is simply recorded and ignored.

The checks are independent. A package does not earn a pass on one check by
excelling at another: a flawless artifact does not excuse a missing environment
record, and a complete environment record does not excuse an artifact showing
the wrong failure. Equally, no check may be failed for the write-up's shape.
Length, headings, tables, bullet style, and tone are not evidence of anything.

In live mode on a claim-only draft, `claim-comment-specific` and
`disclosure-when-required` are the only two checks whose evidence exists yet, so
they alone decide that verdict; the other seven are reported `unclear` with
evidence `not yet applicable: claim-only draft` and are left out of the
combination above.
