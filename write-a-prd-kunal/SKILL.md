---
name: "write-a-prd-kunal"
description: "Write a PRD in phases: pressure-test the problem, validate data, agree metrics and experiment, reconcile requirements against designs, then draft standalone sections and run a review loop."
---

# Write a PRD (Kunal's process)

Write a PRD in phases. First pressure-test the problem. Then reconcile the requirements against the designs before drafting anything. Finally write each section so it stands on its own and can be built, tested and measured without a meeting.

## Workflow

Work through these phases in order. Do not start drafting sections until the problem has survived review and the requirements have been reconciled against the designs.

1. **Frame the problem in conversation.** Get the PM's understanding of the problem, then challenge it using the Problem framing checks below. Iterate until the problem, the supporting data and the assumptions are agreed.
2. **Validate the data.** Pull or compute the key numbers from the raw exports the PM shares, for example a cohort's tenure mix from a Mixpanel CSV. Before using two groups as a benchmark, check that they are comparable.
3. **Agree metrics, guardrails and the experiment.** Before writing any requirements:
    - define each metric precisely
    - size the test and set its duration
    - write the decision rule
4. **Reconcile requirements against the designs.** Read the requirements text and every design screen side by side. List:
    - elements in the design that the requirements do not mention (for example a mute button)
    - requirements the design does not show (states, variants, screens)
    - conflicts between design annotations and the requirements text
    - incomplete or ambiguous sentences in the requirements
    - behaviour neither covers (for example what happens when a video ends)
5. **Set source-of-truth rules.** State which source wins when design and requirements differ (usually the requirements text), and which designs are not final.
6. **Create the doc outline first.** Make the PRD as a doc. The outline has the title, a byline and one placeholder per section, so the reader sees the plan before any content arrives.
7. **Write one section at a time,** in reading order. Each section must stand on its own: repeat the context it needs instead of pointing to other sections.
8. **Collect open questions.** Give each one an owner and a blocking flag, and ask the PM to answer them directly in the doc.
9. **Apply the answers** to every section they affect. Then convert the answered questions into a decisions log (see Review loop).
10. **Produce companion documents** when the review asks for them, for example a list of missing design states.
11. **Run the readability test** on every document before handing it over, and again after every major revision (see Readability test). A document is not finished until the weakest available model can consume it with ease.

## Problem framing

The problem statement must describe what is broken today. It must be backed by data and honest about what is assumed. Challenge it before writing anything else.

Put each claim in the PM's framing through these checks:

| Check | What to look for |
| --- | --- |
| Problem or hypothesis? | The expected outcome of the solution belongs in Goals or Hypothesis, not in the problem |
| Does each point support the conclusion? | A supporting point that, read literally, argues against the conclusion. Clarify what the PM meant |
| Is the comparison fair? | Groups that differ in who they are, not only in the feature. Check tenure, entry source and intent |
| Is the metric precisely defined? | Same feature or any feature? Day N to day N+1? Which denominator? |
| Is the business link proven? | A causal chain to revenue with no data behind it. State it as an assumption and give the real justification (for example, users were paid to acquire) |
| Is the sample large enough? | Small cohorts swing a lot. Confirm daily volumes |
| Is there seasonality? | Festivals, launches or campaigns inflating volumes in the measurement window. Compare percentages, not volumes |

Rules:

- **Say "consistent with", not "proves".** If confounds are ruled out but intent could still differ, the gap is consistent with the effect. It is not proof of it.
- **State assumptions up front.** A reviewer will find an unstated assumption, so stating it first is stronger.
- **Accept PM corrections and move on.** If the PM rules out a concern with data, record it and stop raising it.
- **Compute, do not eyeball.** When raw data is shared, calculate the shares and ranges, and show the date used.

## PRD structure

Use these sections in this order. Every section opens with a sentence that states its point. None depends on another section to be understood.

| Section | Must contain | Format |
| --- | --- | --- |
| Overview and links | One-line summary of the feature and its goal metric; status; links to design, instrumentation sheet and data dashboards; content tool; target users; source-of-truth rules | Table, then a short bullet list |
| Problem | What is broken today, who it affects, the gap in numbers, why it matters to the business, and any assumption stated explicitly | Short paragraphs and bullets |
| Key data points | Every number behind the problem with its source and date; comparability checks; how to read the data; baselines still needed | Table, then bullets |
| Solution | What is built and how it addresses each part of the problem; modes, content types, interactions, content operations | Bullets, with a table for modes or variants |
| Success metrics | North-star metrics with definition, denominator, baseline, success threshold and stretch; secondary diagnostic metrics; hypotheses (for example monetisation) kept separate; the measurement tool | Tables |
| Guardrails | Metric, threshold (non-inferiority margin) and how each is measured; notes on how to read them | Table, then bullets |
| Experiment design | Hypothesis; setup (unit, arms, buckets, entry points, content); pre-checks; sample size; duration; decision rule; configuration feasibility; follow-up experiments | Tables and a checklist |
| Functional requirements | Grouped by area. Each requirement has an ID, one testable statement and a priority (P0 or P1) | One table per area |
| Non-functional requirements | Performance, reliability, stability, data use, offline behaviour, remote config, analytics, accessibility and cost, each with a proposed target | Table |
| User flows | One flowchart per flow (entry, each mode, onboarding, monetisation, share, content publishing), each followed by one sentence that explains it | Mermaid flowcharts |
| Test cases and acceptance criteria | Given, When, Then for each case, grouped by area, including negative cases | One table per area |
| Scope | In scope; out of scope with a reason for each item; future considerations | Bullets and a table |
| Open questions and risks | Blocking questions; gaps between requirements and design; non-blocking questions, each with an owner; risks with impact and mitigation | Tables |

Section-specific rules:

- **Functional requirements:** write one behaviour per row as an observable outcome ("Tapping back returns the user to random mode"), never "should handle appropriately". Mark anything you suggested, rather than the PM decided, as "Proposed". When adding requirements later, give them new IDs instead of renumbering, so test cases and comments still point at the right row.
- **User flows:** cover branches, loops and returns (for example back from a nested mode). If the flow cannot show something yet because it is undecided, say so in the sentence below the chart.
- **Test cases:** cover the happy path, each edge case in the requirements, and what must not happen (for example "no catalogue screen appears"). Include cases for remote-config changes, first-time versus returning users, and language consistency.
- **Open questions:** tag each with an owner (PM, design, engineering, data or content operations) and with whether it blocks the launch or the experiment.

## Metrics, guardrails and experiment design

Agree every number that decides the ship call before launch: what is measured, on whom, how big a change matters, for how long, and what happens in each outcome.

### Metrics

- **Define the denominator.** Prefer "users shown the entry point" over "users who used the feature". If two arms get different click rates, a feature-user denominator compares different kinds of users. Report both cuts. If they disagree, investigate selection before shipping.
- **Give each north-star metric a baseline, a success threshold and a stretch.** Size the threshold to the smallest change worth shipping, not the hoped-for benchmark.
- **Pair volume metrics with quality metrics.** Items per session and session length rise almost by construction in an endless format. Read them next to completion rate, early skip rate and interactions per view.
- **Normalise interactions per view,** not as raw counts.
- **Separate hypotheses from success metrics.** An idea that is tested but does not decide the ship call (for example monetisation) goes in its own section.
- **Compare per-user metrics, not per-impression.** More impressions per session can lower CTR while raising clicks per user. Keep CTR as a diagnostic.
- **Add a D7 metric** to separate a lasting habit from novelty.

### Guardrails

- **Write guardrails as non-inferiority tests** with an explicit margin (for example "no worse than 5% relative").
- **Watch the whole app, not just the feature.** Add app-wide revenue and transactions per DAU to catch cannibalisation, plus a guardrail for adjacent surfaces.
- **Be honest about rare events.** Transactions rarely reach significance in two weeks. Read them as "no evidence of harm", and get stronger proof from a longer run or a long-term control group.
- **Replace unmeasured guardrails with proxies.** If a guardrail is not tracked today (for example content NPS), list measurable proxies until it is instrumented.
- **Include technical health** (start time, buffering, errors, crashes) for media features.

### Experiment design

- **Setup:** state the unit (device or user), which buckets are treatment and which are control, what the control experience is on every entry point, and whether both arms are served the same content.
- **Pre-checks:** confirm that bucketing is hash-based. Run an A/A comparison on pre-period metrics. Check for users who appear on multiple devices, for overlapping experiments, and for carryover from earlier tests.
- **Sample size:** show the assumptions (baseline, confidence, power, traffic per arm). Add a table with the change to detect, users needed and days of traffic for every north-star metric. High-baseline metrics usually need more users, so they set the enrolment length. Use a two-proportion test at 95% confidence and 80% power unless told otherwise.
- **Duration:** run long enough for every cohort to reach D7, and for at least two weekly cycles. Extend if guardrails are inconclusive or a seasonal period ends mid-test. Fix the end date in advance, and do not stop early unless you use a sequential testing method.
- **Decision rule:** list each outcome (clear win, signs of novelty or selection, guardrail breach, no lift) and the decision for it.
- **Feasibility:** check that the content tool and the app can actually serve different experiences to each arm. If not, list the options with trade-offs and an owner, and mark it as blocking.
- **Follow-ups:** describe later experiments and back analyses. Name the confounds to avoid (for example curation quality bundled with sequencing) and the logging they need from day one.

## Writing rules

Write so an engineer, designer, analyst or tester can act on any section without asking the author what it means.

- **Lead with the point.** The first sentence of the document and of each section states the decision, result or answer, with its number where there is one.
- **Make every section standalone.** Repeat the context a reader needs instead of writing "see above".
- **Use specifics, not adjectives.** Give numbers with units, names, dates and the exact on-screen text. Replace "fast", "appropriate" and "intuitive" with a measurable target.
- **Write one behaviour per requirement or test case,** so each can be built and verified on its own.
- **Quote UI text exactly,** in the language it appears in the product.
- **Label proposals.** Anything the writer suggested, rather than the PM decided, is marked "Proposed" until confirmed.
- **Say what is unknown.** A missing fact becomes a one-line open question with an owner. Never write a guess as a requirement.
- **Keep sentences short and plain.** Sentences stay under 25 words and paragraphs at three sentences or fewer. Use bullets only for parallel items.
- **Pick the right shape.** Use tables for items compared on several attributes, numbered lists for steps in order, and flowcharts for flows that branch or loop.
- **Plain words before the term.** Explain every domain or technical idea in everyday words first, then give the term. Do this the first time it appears in each section, because a reader may open any section alone.
- **Gloss every ID on first use in a section.** A requirement, fact, table or rule ID ("F-25", "BT-09", "OI-24") gets a few plain words beside it ("F-25, whether a dosha is switched on now").

## Readability test

A document passes only when the weakest available model reads it cold and both understands it and finds it easy. Run this after the document is drafted, and again after every major revision.

1. **Pick the reader.** Use the weakest model the environment offers (for example Haiku) as a sub-agent with no prior context. Give it only the document, and tell it to open no other file. Use a fresh agent for every run, so it never remembers an earlier version.
2. **Write 7 to 10 comprehension questions** whose answers you know and that the document must make clear. Cover the purpose, the main decisions, each edge case that changes behaviour (for example "what happens when X is missing?"), who owns what, and two or three key terms "in your own words". Ask for each answer in 1 to 3 sentences with the line number where it was found.
3. **Ask for a difficulty report:**
    - passages that were hard to understand (line, short quote, why);
    - every term used before it was explained, or never explained;
    - every place the document points elsewhere instead of saying the thing ("see", "as above", "below", a table or rule ID with no explanation, another document's section number);
    - a readability score from 1 (impossible) to 10 (easy for a newcomer to the domain), with one sentence of reasoning.
4. **Grade it.** The document passes when every question is answered correctly with the right location, the score is 8 or higher, and the report lists no unexplained terms and no pointers.
5. **If it fails, fix what the report names, then re-run with a fresh agent.** The usual fixes:
    - add a "Start here" section: what the document is for, how its parts fit, and one example traced from start to finish;
    - put a plain-language glossary near the top, so no term is used before it is explained;
    - rewrite each flagged passage, leading with the point and in plain words;
    - state each answer the reader missed plainly, in the section where a reader would look for it;
    - replace each pointer with the fact itself.
    Never change a requirement, number or decision to make the document read more easily.
6. **Record the result** in the hand-over: the score, the questions, and anything you could not fix.

Run one reader per document. Long documents can be split into parts for the fixes, but the test always reads the whole document.

## Review loop

When the PM answers open questions in the doc, apply each answer everywhere it lands. Then turn the question list into a decisions log, so the history stays visible.

**Applying answers**

1. Read every answer before editing anything.
2. For each answer, find every section it affects. One answer often touches several. For example, a new bucket split changes the experiment setup, sample size, duration, guardrails and long-term comparison group.
3. Recompute any number that depends on the answer. For example, a new baseline changes the sample size and the enrolment length.
4. Add new requirements and test cases for new behaviour, with new IDs.
5. Replace the open questions with two tables:
    - **Decisions from review:** the question, the decision, and where it is reflected (requirement IDs or section names).
    - **Still open:** questions left unanswered or newly raised by the answers, each with an owner and a blocking flag.
6. Build any companion document the PM asks for. For example, a missing design states document lists each state, the requirement it supports and a priority.
7. Report back briefly: what changed beyond the obvious, and which new questions came up.

**Final checklist before handing over**

- [ ] The problem describes today's state, and the solution's expected outcome sits in the hypothesis.
- [ ] Every number has a source and a date.
- [ ] Each north-star metric has a definition, denominator, baseline, threshold and stretch.
- [ ] Every guardrail has a margin and a measurement method.
- [ ] Sample size and duration are shown with their assumptions, and the decision rule is written.
- [ ] Experiment feasibility in the content tool and the app is confirmed or flagged as blocking.
- [ ] Every requirement is atomic, testable, prioritised and has an ID.
- [ ] Every element visible in the designs is covered by a requirement or listed as a gap.
- [ ] Every flow covers branches, loops and returns.
- [ ] Test cases include negative cases and first-time versus returning users.
- [ ] Open questions have owners and blocking flags.
- [ ] Each section can be read on its own.
- [ ] The readability test passed: the weakest available model answered every comprehension question correctly and scored the document 8 or higher, with no unexplained terms and no pointers.