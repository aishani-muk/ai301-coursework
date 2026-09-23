# Voice guide: how I talk upstream

## Who I am in threads

I have an MS in CS, a few years of AI research behind me, and I build
full-stack systems for a living. In this repo I am a first-time contributor
working issue #14, the offline eval runner, and I had not read a line of this
codebase before this month. What you can expect from me: runnable evidence,
dated progress, and a straight answer when something did not work.

## Rules I write by

### Rule: I commit to the next artifact, not to the finish line

Issue #14 is big enough that I cannot see its end from here. I name the next
thing I will put in the thread, and I say nothing about when the whole feature
lands until its shape is settled.

- Wrong: "Taking #14 — I'll have the offline eval runner done and PR'd by end of next week."
- Right: "Taking #14. First slice is the fixture loader plus a single-profile scorer; I'll post a branch and its output, and say then what the remaining slices look like."

### Rule: my resume is not an argument

Nobody on this thread owes me trust because of where I studied or what I have
shipped. If experience is relevant it shows up as a tradeoff I can defend, not
as a credential in front of it.

- Wrong: "With my MS and my background building eval harnesses for research, I'm confident I'm the right person for this one."
- Right: "I've built offline eval harnesses before, and the tradeoff I keep hitting is caching: pinning fixtures makes runs cheap but hides drift. I'd pin and add an unpinned canary — happy to be argued out of it."

### Rule: one confidence level per sentence, and it has to be the real one

I hedge when I am nervous and overstate when I am tired, and both cost the
reader the same thing: they cannot tell what I actually know. If I ran it, I say
so flatly. If I did not, I say that flatly too. No "might possibly", no
"definitely" without a transcript.

- Wrong: "I think this might possibly be a caching issue, though I'm not totally sure — but it's definitely reproducible on my end."
- Right: "Runs 2 and 3 return in 40 ms against 1.4 s for run 1 (output below), so something is caching. I have not found where yet; I have only looked at the loader."

### Rule: the negative result goes in the thread, at full size

A run that did not reproduce, a slice I could not finish, a benchmark that came
back flat — those are results, and burying them wastes someone else's week. I
post them with the same detail I would post a success, including what I think
made the difference.

- Wrong: "Still working on the runner, making good progress!"
- Right: "Runner works on the JSON fixture and fails on the markdown resumes: the front-matter parser eats the first heading (trace below). I did not get to the scoring work this week."

### Rule: I post work instead of asking to be reserved

Credit here attaches to what I post, not to who called it first, so asking
someone to hold an issue for me buys nothing and blocks someone else. I claim by
showing a repro or a first slice, and I say what I am doing next.

- Wrong: "Please assign this to me and keep it reserved, I'd really like to be the one to do it."
- Right: "Starting on #14; here's my repro of the current runner doing nothing, and the interface I'm planning. If someone else is mid-flight on this, say so and I'll take the scorer half instead."

## Things I never post

- "Guaranteed", "100%", "definitely fixed", or any date for work I have not started.
- A credential used as an argument: my degree, my publications, my job title.
- "+1", "same here", or "any updates?" with no artifact attached.
- A root cause I have reasoned my way to but never actually run.
- Output I did not produce on my own machine, or a transcript I tidied after the fact.
- An assistant-drafted paragraph I have not rewritten in my own words — and in
  any repo whose policy requires it, never AI-assisted text without the
  disclosure line saying which tool and how much.
- Apologies for asking a question, or padding around a negative result to make
  it land softer. The result is the result.
