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
- Dependencies: <whether new dependencies are allowed; if yes, constraints>
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
- `EXPERIMENTS.md`: curated list of attempts and results.
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
