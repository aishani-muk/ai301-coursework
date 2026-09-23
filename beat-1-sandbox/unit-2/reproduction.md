# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of my claim and reproduction on the issue I chose in Unit 1
([#14](https://github.com/codepath/pathreview-ai301-fa26-s1/issues/14), implement an
offline eval runner), and of the evaluation runs that produced `eval-run.txt`.

---

## Your identity upstream

**GitHub username**

aishani-muk

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/14#issuecomment-5804201660

Posted 2026-09-23. Text as posted:

`````
I'd like to take this one. Before claiming I checked the current state at f89c06f: `make eval` prints "Evaluation complete. Results written to eval_results.json", exits 0, and writes no file. A grep for `EvalSuite` returns only its own class definition, so the scorer works but nothing calls it. Full reproduction in the comment below.

Two questions before I write much code. First, does an actionability scorer belong in this issue? `EvalSuite.run` returns relevance and faithfulness, and `rag/evaluator/` has no actionability scorer to call, though the TODO asks for one. Second, where should the mock seam go? `ReviewGenerator.__init__` builds its own `openai.OpenAI` client, so running the pipeline with a mock LLM needs an injection point that does not exist yet. I would rather settle that with a maintainer than guess and have it sent back.

Next I will write the fixture loader, score `basic_profile.json` with it, and push a branch with the `eval_results.json` it produces. I will describe the remaining slices then. If someone is already working on this, say so and I will take a narrower piece.
`````

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/14#issuecomment-5804203738

Posted 2026-09-23. Text as posted:

`````
Environment: macOS 15.7.7 (arm64), Python 3.11.9, pathreview 0.1.0 installed with `pip install -e ".[dev]"`, repo at commit f89c06fc3ff292df2a04a39ac51319d32a76b779 on main. I copied `.env` from `.env.example` without edits, so `LLM_PROVIDER=mock`. I started no Docker services and none are needed here, since `make eval` only runs `.venv/bin/python scripts/run_evals.py`. The project pins 3.11 in `pyproject.toml` and in every Python job in CI.

Steps: clone at that commit, create a venv on Python 3.11, run `pip install -e ".[dev]"`, copy `.env.example` to `.env`, then run `make eval`.

The runner reports success and writes nothing:

```
$ test -f eval_results.json; echo $?
1

$ make eval
.venv/bin/python scripts/run_evals.py
Running RAG evaluation suite...
Evaluation complete. Results written to eval_results.json
$ echo $?
0

$ test -f eval_results.json; echo $?
1
```

The second line it prints is false, and it still exits 0.

The scorer is unwired rather than broken. `grep -rn "EvalSuite" --include="*.py" .` returns a single hit, `rag/evaluator/eval_suite.py:22:class EvalSuite:`, and `rag/evaluator/__init__.py` is zero bytes, so nothing re-exports it either. Calling it directly works: `EvalSuite().run("python fastapi", [{"text": "python expert building fastapi services"}], "This developer knows Python and FastAPI.")` returns `EvalResult(relevance_score=1.0, faithfulness_score=1.0, overall_score=1.0)`. On the input side, `tests/fixtures/sample_profiles/basic_profile.json` parses and holds two repos, so the fixture problem from #43 does not block this.

`.github/workflows/eval.yml` sets `let results = 'Eval results not found.'` and overwrites it only inside a `try` with an empty `catch`, so a missing file yields that literal text rather than a failure. I read that from the workflow file and have not watched a run of it.

This matches the issue: the script prints two lines and returns, no code calls `EvalSuite`, and `eval_results.json` never appears for the workflow to read. Reproduced.

Three things the issue body does not mention, which I think shape the work. `rag/evaluator/` holds only `relevance_scorer.py` and `faithfulness_checker.py`, so an actionability score needs a new scorer. `ReviewGenerator.__init__` constructs its own `openai.OpenAI` client and never dispatches on `settings.llm_provider`, so the mock path needs an injection seam. And `basic_profile.json` is one profile where the issue asks for a benchmark set, so I will need to write more fixtures. I have not started on any of it, and I am flagging it so the scope is visible first.
`````

## Eval iterations

**Run history**

Two runs, in order:

1. **Partial probe, `--only pkg-20,pkg-07,pkg-03,pkg-09,pkg-05,pkg-01`** — agreement
   **6/6** scored items, with category tallies `clear-accept 5/5  disclosure 1/1`.
   This was a deliberate probe of one axis, not an attempt at a scored run. I ran it
   because `pkg-20` is the only member of the `disclosure` category, so the category
   floor turns on that single package: if my conditional-disclosure check misfired,
   the full run would fail the floor even at 19/20 agreement. The five packages
   alongside it are the ones a *badly written* disclosure check would wrongly reject —
   ripgrep's own-words rule (pkg-03), fd's disclose-in-the-PR rule (pkg-09), conda's
   responsibility-only rule (pkg-05), p5.js which discloses although it need not
   (pkg-07), and a silent repo (pkg-01). Getting `pkg-20: reject` together with five
   accepts is what told me the check was conditional rather than either
   unconditional or inert. At about $0.20 per package this cost ~$1.20 against the
   ~$4.00 a wasted full run would have cost. As a partial run it correctly refused to
   write `eval-run.txt`, and printed the reminder that only a full run decides the bar.

2. **Full run, all 20 scored packages** — agreement **20/20 scored items (bar: 18/20:
   PASS)**, with per-category tallies `clear-accept 8/8  disclosure 1/1  no-evidence
   4/4  unfollowable-comms 3/3  wrong-target 4/4`. This is the run saved with
   `--save-run eval-run.txt` and committed alongside this file.

The rubric was not revised between the two runs — the probe confirmed the disclosure
check rather than correcting it — so the first full run was also the final one. The
20/20 above matches the agreement line in the committed `eval-run.txt`.

**Package analysis**

**`pkg-20`** (ghostty-org/ghostty#13604). My rubric decided **reject**. The gold label
is **reject**. They agree.

This is the package the whole run turned on, and it is the one most likely to be
graded wrongly, because every piece of proof in it is excellent. My rubric graded
eight of its nine checks `pass`:

- `behavior-matches-issue` passed on *"Single-theme run with --window-theme=dark
  prints ^[[?997;2n (light), the wrong value the issue reports; conditional pair gives
  997;1"* — the artifact shows exactly the mode-2031 misreport the issue describes.
- `environment-recorded` passed on *"ghostty 1.3.1 release build, Fedora 42 RPM, GTK,
  GNOME 48 Wayland, dark scheme, and the theme config that is decisive"* — it records
  not just the version and platform but the one setting the issue turns on.
- `outcome-honest` passed: *"Conclusion 'matching the issue on a release build' is no
  stronger than the two shown outputs."*
- `claim-comment-specific` passed on a next step drawn from the issue itself — testing
  the draft patch against both the single-theme and conditional-pair configurations.

The single check that failed was `disclosure-when-required`, with the evidence
*"Policy: 'All AI usage in any form must be disclosed'; neither the claim comment nor
the repro report mentions AI assistance."*

My rubric read it that way because of one sentence I put in that check's pass
condition: **"Treat every package as AI-assisted work, so a disclosure requirement is
always live where one exists."** Without it, a grader reasons that nothing in the
bundle says AI was used, so nothing needed disclosing, and accepts the package. That
inference is the trap. The check also reads the contribution-policy line *first* and
treats it as the controlling rule, so the quality of the proof never gets a chance to
argue the policy away. Ghostty's policy is a wall, not a rubric line to trade against.

This package is also why I could not write a simpler check. An unconditional
"the comments must disclose AI use" would have rejected pkg-01, pkg-03, pkg-05,
pkg-09, pkg-11 and pkg-12 — six clear accepts, putting the run at 14/20. The check had
to fire on exactly one repo in twenty and stay silent on the rest.

**Check rationale**

From the `rubric.md` uploaded to `tools/repro-check/`, the `disclosure-when-required`
row, quoted as it is currently written:

> | disclosure-when-required | The repo-facts contribution-policy line, read first and treated as the controlling rule, then the claim comment and repro report searched for a disclosure sentence. | Treat every package as AI-assisted work, so a disclosure requirement is always live where one exists. Pass if the policy line states no AI-disclosure requirement reaching issue comments: silent, permissive, responsibility-only, own-words-only, or disclosure asked only in pull requests. A rule that comments be written in the contributor's own words is a voice rule, not a disclosure rule, and is satisfied by a specific human-sounding comment. If the policy does require disclosing AI use in any form or in comments, pass only when the comments state that AI assistance was used and how much; fail when they do not. | required |

It reads that way because of what I rejected on the way there. My first instinct was a
keyword check: does the policy prohibit AI, and do the comments mention it? That fails
in both directions at once. It rejects every silent repo, and it also mis-reads the
policies that *sound* restrictive but are not — ripgrep says comments "must be written
by humans in their own words," and fd requires stating the tool "in the pull request."
Neither is a disclosure requirement on an issue comment, and both sit on gold accepts.
So the check names those shapes explicitly rather than leaving them to inference: the
long list of pass cases (silent, permissive, responsibility-only, own-words-only,
PR-only) exists because each one is a real repo in the eval set that a shorter rule
would have sunk.

The two structural decisions are the opening premise and the order of operations. The
premise — treat the package as AI-assisted — is what makes the requirement live at
all. The order — policy line first, disclosure sentence second — is what stops the
reasoning running backwards, because a grader who looks for a disclosure sentence
first and then asks whether it was needed will find its absence everywhere and start
inventing reasons to excuse it.

**Trade-offs**

What this check gives up is everything about disclosure that is not stated in the
repo-facts policy line. It cannot see an unwritten norm, a maintainer who asked for
disclosure in a thread, or a policy that lives somewhere the bundle does not quote. By
construction it trusts one line of text completely, and a repo whose real expectations
differ from its written ones will be graded on the writing.

On this eval set it changed exactly one verdict, and I can show that from the
per-check grades in `results.json`. Across all 20 scored packages,
`disclosure-when-required` graded `fail` on **pkg-20 and nothing else** — 19 passes,
1 fail. That single fail is the entire reason pkg-20 rejects: its other eight checks
all pass, so removing this check would have flipped pkg-20 to accept, dropped the run
to 19/20, and — because `disclosure` is a one-package category — failed the category
floor outright with `disclosure 0/1`. So the check is load-bearing and inert at the
same time: it does nothing on 95% of the set and decides whether the run passes at all.

The same tally justified the one `preferred` weight I used. `control-run-shown` graded
non-pass on 15 of 20 packages, and four of those are gold **accepts** — pkg-05, pkg-09
and pkg-12 failed it and pkg-03 came back `unclear`. Had I weighted it `required`, it
would have rejected at least three clear accepts and taken the run to 17/20. It is a
real quality signal that must never gate, which is exactly what `preferred` is for.

---

Related paths: `eval-run.txt` in this directory; my skill's files in
`tools/repro-check/`.
