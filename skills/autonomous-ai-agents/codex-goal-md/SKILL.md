---
name: codex-goal-md
description: Use when preparing Markdown files for OpenAI Codex /goal mode. Turns a vague coding or research objective into a measurable goal package with GOAL.md, PLAN.md, EXPERIMENTS.md, and EXPERIMENT_NOTES.md so Codex can run long loops with tight feedback and auditable progress.
version: 1.0.0
author: Hermes Agent
license: MIT
platforms: [linux, macos]
metadata:
  hermes:
    tags: [codex, goal-mode, agents, markdown, planning, experiments]
    related_skills: [codex, writing-plans, test-driven-development, systematic-debugging]
---

# Codex Goal Markdown Pack

## Overview

Use this skill to create the Markdown files that make Codex `/goal` mode effective. The core idea, based on Chris Hayduk's "Using Codex Goals Effectively", is that goal mode is a loop: Codex acts, scores progress, checks whether the score satisfies the goal, and continues or stops. Therefore the goal must be measurable, the feedback loop must be fast, and Codex needs filesystem-based memory for long runs.

The output is usually a small pack of files in the target repo:

- `GOAL.md` — the contract Codex should optimize against.
- `PLAN.md` — high-level plan and constraints.
- `EXPERIMENTS.md` — curated record of attempts and results.
- `EXPERIMENT_NOTES.md` — chronological scratchpad / audit trail.

For short tasks, `GOAL.md` alone may be enough. For multi-hour or multi-day loops, create all four files.

## When to Use

Use this when the user wants to:

- Run Codex with `/goal` on a coding, ML, paper, benchmark, refactor, or debugging task.
- Convert a vague objective like "make this better" into a measurable agent objective.
- Prepare durable Markdown context files before giving a long-running autonomous task to Codex.
- Track experiments, failures, and progress across many Codex compaction windows.
- Give Codex a checklist or scoring rubric it can repeatedly evaluate.

Do **not** use this for:

- One-shot prompts that should finish in a single Codex turn.
- Tasks with no safe automated verification or feedback signal.
- High-risk production changes where an autonomous loop could cause broad damage without human gates.

## Principles

### 1. Make the goal quantitative

Bad goal:

```text
Make the code better.
```

Better goal:

```text
Reduce p95 runtime of `src/retriever.py::retrieve()` by at least 20% on `bench/retriever_bench.py`, while keeping all existing unit and integration tests passing.
```

A good goal contains:

- **Target:** the exact artifact or behavior to change.
- **Metric:** runtime, accuracy, failing tests, lint count, checklist items, size, latency, cost, etc.
- **Threshold:** how much improvement or what completion standard is required.
- **Constraints:** what must not regress.
- **Stop condition:** when Codex should stop and report.

If the task is qualitative, convert it into a checklist. Example: for paper formatting, ask Codex or Hermes to extract formatting requirements into `CHECKLIST.md`, then define success as all checklist items being satisfied without technical-content changes.

### 2. Keep the feedback loop tight

Codex needs a fast scoring command. Prefer commands that run in seconds or minutes, not hours.

Examples:

- Use a subsampled dataset for ML architecture search.
- Use a small model or fewer epochs for iteration, then reserve full validation for the end.
- Use a benchmark script with a fixed seed and JSON output.
- Use focused tests before full test suites.
- Add a smoke test if no automated test exists.

Every `GOAL.md` should include:

- `Primary scoring command`
- `Fast iteration command`
- `Final verification command`
- Expected pass/fail or numeric success criteria

### 3. Give Codex Markdown files for tracking

Long-running goal mode can span many hours. Do not rely on model memory alone. Tell Codex to update files as it works:

- `PLAN.md`: update when strategy changes.
- `EXPERIMENTS.md`: record each meaningful attempt with result and interpretation.
- `EXPERIMENT_NOTES.md`: append chronological notes, observations, and next hypotheses.
- Checklist files: mark items complete only when verified.

This creates persistent state for both Codex and the human operator.

## File Pack Template

Create these files at the root of the repo or inside a clear task directory such as `.codex-goals/<task-name>/`. If Codex may not automatically read nested files, mention the file paths explicitly in the `/goal` prompt.

### GOAL.md

```markdown
# Goal: <short task name>

## Objective

<One sentence with the measurable outcome.>

## Success Criteria

Codex may stop only when all of the following are true:

- [ ] <Quantitative criterion 1: metric + threshold + target file/module>
- [ ] <Quantitative or checklist criterion 2>
- [ ] <No-regression criterion: tests, API behavior, output equivalence, etc.>
- [ ] <Documentation or handoff criterion, if needed>

## Non-Goals

- <What Codex must not attempt.>
- <Large rewrites or risky scope expansions to avoid.>

## Constraints

- Preserve: <public APIs, data formats, technical content, user-visible behavior>
- Do not modify: <protected files/directories>
- Allowed to modify: <specific files/directories>
- Dependencies: <whether new deps are allowed; if yes, constraints>
- Runtime budget: <per-iteration and final verification budget>

## Scoring / Feedback Loop

### Fast iteration command

```bash
<fast command Codex should run often>
```

Expected signal:

- Pass condition: <what output means progress/success>
- Fail condition: <what output means regression>
- Typical runtime: <seconds/minutes>

### Primary scoring command

```bash
<benchmark/eval/test command that directly measures the goal>
```

Metric to optimize:

- Baseline: <baseline number or how to compute it>
- Target: <required improvement or threshold>

### Final verification command

```bash
<full test/eval/lint command>
```

Codex must run this before declaring success.

## Tracking Requirements

Codex must keep these files updated:

- `PLAN.md`: current strategy and next steps.
- `EXPERIMENTS.md`: curated table/list of attempts and results.
- `EXPERIMENT_NOTES.md`: chronological scratchpad.

After each meaningful attempt, append an entry to `EXPERIMENTS.md` before starting another attempt.

## Stop and Report Conditions

Stop and report if:

- All success criteria are checked and final verification passes.
- The same approach fails twice without new information.
- The goal appears impossible under the constraints.
- A required command cannot run due to missing credentials, missing data, or environment problems.

Final report must include:

- What changed.
- Evidence for each success criterion.
- Commands run and final outputs/metrics.
- Remaining risks or follow-up suggestions.
```

### PLAN.md

```markdown
# Plan

## Current Strategy

<Initial plan. Keep this high-level and update when strategy changes.>

## Assumptions

- <Assumption 1>
- <Assumption 2>

## Milestones

- [ ] Establish baseline and confirm scoring command works.
- [ ] Try first low-risk improvement.
- [ ] Compare metric against baseline.
- [ ] Iterate until target is met.
- [ ] Run final verification.
- [ ] Produce final report.

## Decision Log

- <YYYY-MM-DD HH:MM> — <decision> — <why>
```

### EXPERIMENTS.md

```markdown
# Experiments

Use this as the curated record. One entry per meaningful attempt.

## Baseline

- Command: `<baseline command>`
- Result: <baseline metric/output>
- Notes: <important context>

## Attempts

### Attempt 001 — <short title>

- Hypothesis: <why this might help>
- Change: <files/functions changed>
- Command(s): `<commands run>`
- Result: <metric/test result>
- Outcome: Success / Partial / Failed
- Interpretation: <what was learned>
- Next: <what to try next>
```

### EXPERIMENT_NOTES.md

```markdown
# Experiment Notes

Chronological scratchpad. Append-only unless cleaning obvious duplicates.

## <YYYY-MM-DD HH:MM>

- Observed: <fact from code/test/output>
- Thought: <short reasoning>
- Next action: <specific next step>
```

## Creating a Goal Pack

1. Identify the target repo and task.
2. Ask or inspect for the fastest meaningful verification commands.
3. Establish a baseline if the task is optimization-oriented.
4. Write `GOAL.md` with a quantitative success contract.
5. Write `PLAN.md`, `EXPERIMENTS.md`, and `EXPERIMENT_NOTES.md` if the task may take more than a few iterations.
6. Give Codex a `/goal` prompt that explicitly points to the files.

Example `/goal` prompt:

```text
/goal Read GOAL.md, PLAN.md, EXPERIMENTS.md, and EXPERIMENT_NOTES.md. Work autonomously until all Success Criteria in GOAL.md are satisfied. Keep the tracking files updated after each meaningful attempt. Do not declare success until the Final verification command in GOAL.md passes. Stop and report if blocked by environment, missing data, or impossible constraints.
```

For a nested task directory:

```text
/goal Read .codex-goals/retriever-speedup/GOAL.md and the related tracking files in that directory. Follow the success criteria and stop/report rules exactly.
```

## Goal Quality Checklist

Before handing the files to Codex, verify:

- [ ] The objective names specific files, modules, tests, datasets, or outputs.
- [ ] At least one success criterion is numeric or checklist-count based.
- [ ] There is a clear no-regression constraint.
- [ ] The fast iteration command is cheap enough to run repeatedly.
- [ ] The final verification command is stronger than the fast iteration command.
- [ ] Baseline and target are recorded, or the method to compute them is specified.
- [ ] Codex is instructed to update tracking files.
- [ ] Stop conditions prevent infinite flailing.
- [ ] The allowed and forbidden modification scope is explicit.

## Common Patterns

### Performance optimization

- Metric: runtime, p95 latency, memory, throughput, cost.
- Baseline: benchmark command output.
- Target: percentage improvement.
- Constraint: tests pass; outputs match golden files.

### Bug fixing

- Metric: failing test count goes from N to 0.
- Baseline: reproduction command fails.
- Target: reproduction passes plus regression tests pass.
- Constraint: minimal change; no unrelated refactors.

### Paper or document conversion

- Metric: all checklist items satisfied.
- Baseline: extracted checklist from target venue/template.
- Target: 100% checklist completion.
- Constraint: do not change technical content or claims.

### ML experiment loop

- Metric: validation score, loss, runtime, memory, or sample efficiency.
- Baseline: fixed seed, small dataset/model.
- Target: improvement on fast proxy, then confirmation on larger validation.
- Constraint: log every experiment and preserve reproducibility.

## Common Pitfalls

1. **Vague goals cause early stopping or endless loops.** Convert "improve" into a metric, threshold, and stop condition.

2. **The scoring command is too slow.** Create a fast proxy for iteration, then require stronger final verification.

3. **No baseline.** Without a baseline, Codex cannot know whether it improved anything.

4. **No no-regression guard.** Optimization without tests often breaks behavior while improving the chosen metric.

5. **Tracking files become noisy.** Keep `EXPERIMENTS.md` curated; put raw thoughts in `EXPERIMENT_NOTES.md`.

6. **Codex keeps trying the same failed approach.** Add a stop rule: if an approach fails twice without new information, change strategy or report.

7. **The file pack is hidden from Codex.** Mention exact paths in the `/goal` prompt.

## Verification Checklist

- [ ] `GOAL.md` exists and contains objective, success criteria, constraints, scoring commands, tracking requirements, and stop/report conditions.
- [ ] `PLAN.md` exists for multi-step tasks.
- [ ] `EXPERIMENTS.md` exists and has a baseline section when applicable.
- [ ] `EXPERIMENT_NOTES.md` exists for long-running loops.
- [ ] The `/goal` prompt explicitly references the Markdown files.
- [ ] The feedback loop can run in the current environment.

