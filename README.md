# write-a-prd-kunal

A Claude skill for writing product requirement documents (PRDs) that an engineer, designer, analyst or tester can act on without a meeting.

It writes a PRD in phases: pressure-test the problem, validate the data, agree metrics and the experiment, reconcile requirements against designs, then draft standalone sections and run a review loop. Before hand-over, a readability test has the weakest available model read the document cold; the document passes only if that reader answers every comprehension question correctly and rates it 8 out of 10 or higher.

## What it enforces

- Lead every section with its point, and make each section stand on its own: no "see above".
- Plain words before every term, and a short gloss beside every ID.
- One testable behaviour per requirement and per test case.
- Every number with a source and a date; every proposal labelled "Proposed"; every unknown as an open question with an owner and a blocking flag.
- Metrics with a denominator, baseline, threshold and stretch; guardrails as non-inferiority tests; an experiment with sample size, duration and a decision rule.

## Install

**Claude Code:** copy the `write-a-prd-kunal` folder into `~/.claude/skills/` (for all your projects) or `.claude/skills/` inside a project. Then ask Claude to write a PRD, or type `/write-a-prd-kunal`.

```bash
git clone https://github.com/k4kd/write-a-prd-kunal.git
cp -r write-a-prd-kunal/write-a-prd-kunal ~/.claude/skills/
```

**claude.ai:** zip the `write-a-prd-kunal` folder and upload it under Settings, Capabilities, Skills.

## License

MIT. See `LICENSE`.
