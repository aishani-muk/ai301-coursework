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
I'd like to take this one. I've confirmed the current state at f89c06f before claiming: `make eval` runs `scripts/run_evals.py`, prints "Evaluation complete. Results written to eval_results.json", exits 0, and writes no file, so the success line is not true. Reading `.github/workflows/eval.yml`, the RAG Evaluation step falls back to its `results = 'Eval results not found.'` default inside an empty `catch`, so on that reading the gap never surfaces as a failure — though I have only read the workflow, not watched a run of it. `EvalSuite` itself works when called directly; `grep -rn "EvalSuite" --include="*.py" .` returns only its own class definition, so the parts exist and nothing is wired to them. Full reproduction in a follow-up comment.

Two things I'd like a steer on before I write much code. First, scoring: `EvalSuite.run` returns relevance and faithfulness, but the TODO in `run_evals.py` also asks for actionability, and there is no actionability scorer in `rag/evaluator/`. Is adding one in scope here, or should the first cut report the two scorers that already exist? Second, the mock path: `ReviewGenerator.__init__` constructs its own `openai.OpenAI` client, so "run each through the full RAG pipeline with mock LLM" needs an injection seam that does not exist yet — I'd rather agree on where that seam goes than invent one and have it sent back.

My next step is the fixture loader plus scoring over `basic_profile.json`, and a branch with the `eval_results.json` it produces; I'll say then what the remaining slices look like. If someone else is already mid-flight on this, say so and I'll take a narrower piece.
`````

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/14#issuecomment-5804203738

Posted 2026-09-23. Text as posted:

`````
Environment: macOS 15.7.7 (arm64), Python 3.11.9, pathreview 0.1.0 installed with `pip install -e ".[dev]"`, structlog 26.1.0, pytest 9.1.1, repo at commit f89c06fc3ff292df2a04a39ac51319d32a76b779 on main. `.env` is an unmodified copy of `.env.example`, so `LLM_PROVIDER=mock`. No Docker services were started and none are needed: `make eval` is just `.venv/bin/python scripts/run_evals.py`, and nothing below touches Postgres, Redis or Chroma. The project pins 3.11 in `pyproject.toml`, in the four Python jobs in `ci.yml` and in the eval workflow, so this is the version CI runs on.

Steps: cloned at that commit, created a venv on Python 3.11, `pip install -e ".[dev]"`, `cp .env.example .env` with no edits. Then three independent probes — `make eval` with a before-and-after check for the file it says it wrote, a grep for callers of `EvalSuite` plus a direct call to it, and a read of the workflow that consumes the output.

The runner claims success and writes nothing:

```
$ git rev-parse HEAD
f89c06fc3ff292df2a04a39ac51319d32a76b779

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

The second printed line is false, and the exit status is 0, which is what lets it pass unnoticed.

`EvalSuite` is defined and never called. The grep returns one hit, its own class statement, and `rag/evaluator/__init__.py` is zero bytes so it is not re-exported either:

```
$ grep -rn "EvalSuite" --include="*.py" .
rag/evaluator/eval_suite.py:22:class EvalSuite:

$ wc -c rag/evaluator/__init__.py
       0 rag/evaluator/__init__.py
```

It is not broken, only unwired — calling it directly scores a query and feedback with no extra plumbing:

```
$ .venv/bin/python -c 'from rag.evaluator.eval_suite import EvalSuite
r = EvalSuite().run("python fastapi", [{"text": "python expert building fastapi services"}], "This developer knows Python and FastAPI.")
print(r)'
2026-09-24 04:12:23 [info     ] relevance_scored               avg_score=1.0 chunks_count=1 query_len=2
2026-09-24 04:12:23 [info     ] faithfulness_checked           claims_count=1 score=1.0 supported_count=1
2026-09-24 04:12:23 [info     ] eval_suite_complete            faithfulness=1.0 overall=1.0 relevance=1.0
EvalResult(relevance_score=1.0, faithfulness_score=1.0, overall_score=1.0)
```

The workflow that consumes the output tolerates the missing file. `.github/workflows/eval.yml` seeds a default string and overwrites it only if the file reads, inside a `try` with an empty `catch`:

```
$ sed -n '29,33p' .github/workflows/eval.yml
            const fs = require('fs');
            let results = 'Eval results not found.';
            try {
              results = fs.readFileSync('eval_results.json', 'utf8');
            } catch (e) {}
```

Reading those lines, the missing file is swallowed and the step reports the literal text "Eval results not found." rather than failing. That is from the workflow file; I have not watched a CI run of it, and the workflow only fires on pull requests touching `rag/**`, `ingestion/chunking/**` or `ingestion/embeddings/**`.

As a control on the input side, the fixture the runner is meant to load is present and parses, so none of this is blocked on the missing-fixture problem from #43:

```
$ .venv/bin/python -c 'import json
d = json.load(open("tests/fixtures/sample_profiles/basic_profile.json"))
print("valid JSON; keys:", sorted(d))
print("repos:", [r["name"] for r in d["repos"]])'
valid JSON; keys: ['github_username', 'portfolio_url', 'repos', 'resume_filename']
repos: ['transit-delay-tracker', 'recipe-scaler']
```

Behavior observed matches the issue: `run_evals.py` prints two lines and returns, no code calls `EvalSuite`, and `eval_results.json` is never produced for the RAG Evaluation workflow to read. Reproduced, not a cannot-reproduce case.

Three things I hit that the issue body does not mention, and that I think shape the work. The TODO in `run_evals.py` asks for relevance, faithfulness and actionability, but `rag/evaluator/` holds only `relevance_scorer.py` and `faithfulness_checker.py` — there is no actionability scorer to call. `ReviewGenerator.__init__` builds its own `openai.OpenAI` client with no dispatch on `settings.llm_provider`, so "the full RAG pipeline with mock LLM" has no seam to inject a stub through, even though the workflow runs the step with `LLM_PROVIDER: mock`. And `basic_profile.json` is a single profile where the issue asks for a benchmark portfolio set, so authoring more fixtures is part of this. I have not started on any of it; flagging it here so the scope is visible before I pick an approach.
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
