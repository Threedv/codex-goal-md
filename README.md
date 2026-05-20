# codex-goal-md

A practical Markdown pack and Hermes skill set for running OpenAI Codex `/goal` mode effectively.

This repo is based on Chris Hayduk's article, "Using Codex Goals Effectively", but packages the idea into a reusable workflow:

- make goals measurable, quantitative, and stoppable;
- keep Codex's feedback loop tight with fast scoring commands;
- give long-running agents filesystem memory through Markdown tracking files;
- use an interactive `$make-goal` interview to create detailed, project-specific files instead of generic placeholders.

## What is included

- `skills/autonomous-ai-agents/codex-goal-md/SKILL.md` — core Hermes Agent skill explaining the Codex `/goal` Markdown method.
- `skills/autonomous-ai-agents/make-goal/SKILL.md` — command-like Hermes skill for `$make-goal`: interview the user, gather details, then write the four files.
- `bin/make-goal` — simple CLI helper that creates blank starter templates in a target directory.
- `templates/GOAL.md` — measurable goal contract template.
- `templates/PLAN.md` — high-level strategy and decision log template.
- `templates/EXPERIMENTS.md` — curated experiment/results log template.
- `templates/EXPERIMENT_NOTES.md` — chronological scratchpad template.

## Recommended use: `$make-goal` interactive workflow

The intended workflow is **not** to immediately dump blank Markdown files.

The intended workflow is:

```text
User: $make-goal
Agent: asks focused questions about the repo, goal, metrics, constraints, tests, and stop conditions
User: answers, possibly over several turns
Agent: creates detailed GOAL.md, PLAN.md, EXPERIMENTS.md, and EXPERIMENT_NOTES.md
Agent: gives a ready-to-paste Codex /goal prompt
```

The generated files should be as detailed as possible so Codex can run for hours without drifting, flailing, stopping early, or making unsafe broad changes.

### What `$make-goal` should ask

A good first message is:

```text
좋아. Codex /goal용 4개 파일을 만들기 전에 몇 가지만 물어볼게.

1. 어느 폴더/repo에 만들까? 현재 폴더면 `.`라고 해줘.
2. 목표를 한 문장으로 말하면 뭐야?
3. Codex가 바꿔도 되는 파일/폴더와 건드리면 안 되는 파일/폴더가 있어?
4. 성공 여부를 어떤 명령어로 확인하면 돼? 예: pytest, benchmark, lint, eval script.
5. 성공 기준을 숫자나 체크리스트로 만들면 뭐가 좋을까? 예: failing tests 0개, latency 20% 감소, checklist 100% 완료.
```

Then the agent should ask follow-up questions only for missing details. It should avoid a giant one-shot form, but continue the interview until the goal contract is useful.

### Minimum details before writing files

Before writing the final files, `$make-goal` should know, or explicitly mark as unknown/placeholders:

- Target directory
- Goal title
- Objective
- Target files/modules/outputs
- Allowed modification scope
- Forbidden modification scope
- Success criteria
- Baseline or current failure state
- Fast iteration command
- Primary scoring command
- Final verification command
- No-regression constraints
- Stop/report conditions
- Whether new dependencies are allowed
- Whether files should live at repo root or under `.codex-goals/<task-name>/`

## Output files

`$make-goal` creates these four files:

- `GOAL.md` — the contract Codex should optimize against.
- `PLAN.md` — strategy, assumptions, milestones, and decision log.
- `EXPERIMENTS.md` — curated record of baselines and attempts.
- `EXPERIMENT_NOTES.md` — chronological scratchpad / audit trail.

## Final Codex prompt

After the files are created, start Codex with something like:

```text
/goal Read GOAL.md, PLAN.md, EXPERIMENTS.md, and EXPERIMENT_NOTES.md. Work autonomously until all Success Criteria in GOAL.md are satisfied. Keep the tracking files updated after each meaningful attempt. Do not declare success until the Final verification command in GOAL.md passes. Stop and report if blocked by environment, missing data, or impossible constraints.
```

For a nested goal directory:

```text
/goal Read .codex-goals/<task-name>/GOAL.md and the related tracking files in that directory. Follow the success criteria and stop/report rules exactly.
```

## Alternative: blank template CLI

If you only want blank starter files, use the included script:

```bash
./bin/make-goal /path/to/target/repo --name "Retriever speedup"
```

Or add `bin/` to your `PATH` and run:

```bash
make-goal . --name "Fix flaky tests"
```

This creates blank versions of:

- `GOAL.md`
- `PLAN.md`
- `EXPERIMENTS.md`
- `EXPERIMENT_NOTES.md`

Use `--force` to overwrite existing files.

## Quality checklist for generated goal files

Before handing the files to Codex, verify:

- The objective names specific files, modules, tests, datasets, or outputs.
- At least one success criterion is numeric or checklist-count based.
- There is a clear no-regression constraint.
- The fast iteration command is cheap enough to run repeatedly.
- The final verification command is stronger than the fast iteration command.
- Baseline and target are recorded, or the method to compute them is specified.
- Codex is instructed to update tracking files.
- Stop conditions prevent infinite flailing.
- The allowed and forbidden modification scope is explicit.
- The files contain enough context for a fresh Codex session to continue after compaction.

## Repository structure

```text
.
├── README.md
├── LICENSE
├── bin/
│   └── make-goal
├── skills/
│   └── autonomous-ai-agents/
│       ├── codex-goal-md/
│       │   └── SKILL.md
│       └── make-goal/
│           └── SKILL.md
└── templates/
    ├── GOAL.md
    ├── PLAN.md
    ├── EXPERIMENTS.md
    └── EXPERIMENT_NOTES.md
```

## Credit

Inspired by Chris Hayduk's LinkedIn article, "Using Codex Goals Effectively".

## License

MIT
