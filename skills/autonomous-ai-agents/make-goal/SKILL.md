---
name: make-goal
description: Use when the user says "$make-goal", "make-goal", or asks to create Codex /goal Markdown files in a project. Interview the user with detailed focused questions, then generate GOAL.md, PLAN.md, EXPERIMENTS.md, and EXPERIMENT_NOTES.md filled with objective, metrics, constraints, commands, baseline, scope, stop rules, and tracking instructions.
version: 1.0.0
author: Hermes Agent
license: MIT
platforms: [linux, macos]
metadata:
  hermes:
    tags: [codex, goal-mode, markdown, generator, interview, command-like]
    related_skills: [codex-goal-md, codex]
---

# Make Goal

## Overview

This is a command-like Hermes skill. When the user says `$make-goal` or asks for Codex goal files, **do not immediately dump generic templates**. First run an interview to gather enough information to make the four files genuinely useful for Codex `/goal` mode. The output should be as detailed as possible.

Create the standard Codex `/goal` Markdown pack in the chosen target directory:

- `GOAL.md`
- `PLAN.md`
- `EXPERIMENTS.md`
- `EXPERIMENT_NOTES.md`

The purpose is to turn a vague intention into a measurable autonomous-agent contract: clear target, metrics, feedback commands, constraints, stop conditions, and durable tracking files.

## When to Use

Use when the user says things like:

- `$make-goal`
- `$make-goal . "Retriever speedup"`
- `make-goal 만들어줘`
- `이 repo에 Codex /goal용 md 파일 만들어줘`
- `GOAL.md PLAN.md EXPERIMENTS.md EXPERIMENT_NOTES.md 자동 생성해줘`
- `Codex goal mode 돌릴 수 있게 질문하면서 md 만들어줘`

## Core Behavior

1. Start an interview instead of writing files immediately, unless the user already provided enough detail.
2. Ask focused questions in small batches. Prefer 3-5 questions per message, not a giant form.
3. Keep asking follow-up questions until the files can be made detailed and useful.
4. Do not repeat questions the user already answered.
5. If the user does not know a command, metric, or scope, inspect the repository when possible and propose a default instead of blocking.
6. After enough information is gathered, create the four Markdown files with project-specific content.
7. Do not overwrite existing files unless the user explicitly asks for overwrite/force.
8. Report created and skipped files.
9. Include a ready-to-paste Codex `/goal` prompt.

## Interview Flow

A good first message after `$make-goal` should match the user's language.

For English users:

```text
Great. Before I create the four Codex /goal files, I’ll ask a few questions so the files are detailed enough to be useful.

1. Which folder/repo should I create them in? If it is the current folder, say `.`.
2. What is the goal in one sentence?
3. Which files/folders is Codex allowed to modify, and which files/folders must it not touch?
4. What command should Codex use to verify success? Examples: pytest, benchmark, lint, eval script.
5. What should the success criteria be as numbers or checklist items? Examples: 0 failing tests, 20% lower latency, 100% checklist completion.
```

For Korean users:

```text
좋아. Codex /goal용 4개 파일을 만들기 전에 몇 가지만 물어볼게.

1. 어느 폴더/repo에 만들까? 현재 폴더면 `.`라고 해줘.
2. 목표를 한 문장으로 말하면 뭐야?
3. Codex가 바꿔도 되는 파일/폴더와 건드리면 안 되는 파일/폴더가 있어?
4. 성공 여부를 어떤 명령어로 확인하면 돼? 예: pytest, benchmark, lint, eval script.
5. 성공 기준을 숫자나 체크리스트로 만들면 뭐가 좋을까? 예: failing tests 0개, latency 20% 감소, checklist 100% 완료.
```

If the user answers partially, continue with only missing pieces. Use concise but persistent follow-up questions.

## Required Information Before Writing

Do not create final files until these are known or explicitly left as placeholders:

- Target directory
- Goal title
- Concrete objective
- Target files/modules/outputs
- Allowed modification scope
- Forbidden modification scope
- At least one success criterion
- At least one verification/scoring command, or a clear note that no command exists yet
- No-regression constraints
- Stop/report conditions

## Detailed Interview Checklist

Use these questions to make the files maximally useful. Ask only what is missing.

### Destination and layout

- Which repo or folder should receive the files?
- Should the files be at repo root or under `.codex-goals/<task-name>/`?
- Should existing files be preserved, skipped, or overwritten?

### Goal definition

- What is the goal in one sentence?
- Is this a bug fix, performance optimization, refactor, ML experiment, paper/document conversion, benchmark improvement, or feature implementation?
- What specific files, modules, commands, datasets, APIs, or outputs define the target?
- What is explicitly out of scope?

### Success criteria

- What must be true for Codex to stop?
- Can the goal be expressed numerically? Examples: tests from N failing to 0, latency -20%, accuracy +2 points, checklist 100% complete.
- If the goal is qualitative, can it become a checklist?
- What baseline should be recorded before changes?
- What target threshold is enough?

### Feedback loop

- What fast command can Codex run repeatedly?
- What primary scoring/eval command directly measures progress?
- What final verification command must pass before success?
- How long should each command take?
- Are there seeds, sample datasets, small configs, or smoke tests for cheap iteration?

### Constraints and safety

- Which files/directories may Codex edit?
- Which files/directories are forbidden?
- Are new dependencies allowed?
- Must public APIs, data formats, checkpoints, paper claims, UI behavior, or outputs remain unchanged?
- Are there credentials, datasets, GPUs, or external services Codex must not touch?

### Tracking expectations

- What should be logged in `EXPERIMENTS.md`?
- What should go into `EXPERIMENT_NOTES.md`?
- How should Codex update `PLAN.md` when strategy changes?
- Should Codex maintain a checklist and mark items complete only after verification?

### Stop conditions

- When should Codex stop and ask for help?
- How many repeated failed attempts are allowed before changing strategy?
- What environment problems should be reported instead of worked around?
- What risky changes require human review?

## File Generation Requirements

When writing files, do not leave generic placeholders if the user provided real information. Fill the files with concrete details.

### GOAL.md must include

- Objective
- Success Criteria
- Non-Goals
- Constraints
- Allowed Changes
- Forbidden Changes
- Scoring / Feedback Loop
- Baseline / Target
- Tracking Requirements
- Stop and Report Conditions
- Final Report Requirements

### PLAN.md must include

- Current Strategy
- Assumptions
- Milestones
- Decision Log
- Initial Investigation Steps
- Risk Controls

### EXPERIMENTS.md must include

- Baseline section
- Attempt template
- Result fields
- Interpretation field
- Next-step field

### EXPERIMENT_NOTES.md must include

- Chronological append-only note format
- Observation / thought / next action structure
- Reminder that verified facts go to `EXPERIMENTS.md`

## Implementation Guidance

For default `$make-goal`, generate the files directly with `write_file` after the interview, because the contents should reflect the user's actual answers.

If the user only wants blank templates, use the helper script from this public repo:

```bash
./bin/make-goal <target_dir> --name "<goal name>"
```

Or, on Bob's local machine if this repo is cloned at the known workspace path:

```bash
/Users/bobcho/.openclaw/workspace/public/codex-goal-md/bin/make-goal <target_dir> --name "<goal name>"
```

Use `--force` only when the user explicitly asks to overwrite existing files.

## Suggested Codex Prompt

After creating the files, give the user this prompt, adjusted for the chosen paths:

```text
/goal Read GOAL.md, PLAN.md, EXPERIMENTS.md, and EXPERIMENT_NOTES.md. Work autonomously until all Success Criteria in GOAL.md are satisfied. Keep the tracking files updated after each meaningful attempt. Do not declare success until the Final verification command in GOAL.md passes. Stop and report if blocked by environment, missing data, or impossible constraints.
```

For nested goal directories:

```text
/goal Read .codex-goals/<task-name>/GOAL.md and the related tracking files in that directory. Follow the success criteria and stop/report rules exactly.
```

## Verification Checklist

- [ ] Target directory exists.
- [ ] Existing files were not overwritten unless the user requested force/overwrite.
- [ ] `GOAL.md` contains concrete objective, criteria, constraints, commands, and stop rules.
- [ ] `PLAN.md` contains useful initial strategy, milestones, and assumptions.
- [ ] `EXPERIMENTS.md` contains baseline and attempt templates.
- [ ] `EXPERIMENT_NOTES.md` contains chronological scratchpad structure.
- [ ] The user received a ready-to-paste Codex `/goal` prompt.
