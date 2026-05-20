# codex-goal-md

A practical Markdown pack and Hermes skill for running OpenAI Codex `/goal` mode effectively.

Based on Chris Hayduk's article, "Using Codex Goals Effectively", this repo turns the playbook into reusable files:

- make goals measurable, quantitative, and stoppable;
- keep Codex's feedback loop tight with fast scoring commands;
- give long-running agents filesystem memory through Markdown tracking files.

## What is included

- `skills/autonomous-ai-agents/codex-goal-md/SKILL.md` — Hermes Agent skill.
- `bin/make-goal` — CLI helper that creates the four Markdown files in a target directory.
- `templates/GOAL.md` — measurable goal contract template.
- `templates/PLAN.md` — high-level strategy and decision log template.
- `templates/EXPERIMENTS.md` — curated experiment/results log template.
- `templates/EXPERIMENT_NOTES.md` — chronological scratchpad template.

## Quick use

### Option A: copy templates manually

Copy the templates into your target repository, fill in the measurable criteria, then start Codex with:

```text
/goal Read GOAL.md, PLAN.md, EXPERIMENTS.md, and EXPERIMENT_NOTES.md. Work autonomously until all Success Criteria in GOAL.md are satisfied. Keep the tracking files updated after each meaningful attempt. Do not declare success until the Final verification command in GOAL.md passes. Stop and report if blocked by environment, missing data, or impossible constraints.
```

### Option B: generate files with `make-goal`

From this repository:

```bash
./bin/make-goal /path/to/target/repo --name "Retriever speedup"
```

Or add `bin/` to your `PATH` and run it as a command:

```bash
make-goal . --name "Fix flaky tests"
```

This creates:

- `GOAL.md`
- `PLAN.md`
- `EXPERIMENTS.md`
- `EXPERIMENT_NOTES.md`

## Minimal checklist for a good Codex goal

- The objective names specific files, modules, tests, datasets, or outputs.
- At least one success criterion is numeric or checklist-count based.
- There is a clear no-regression constraint.
- The fast iteration command is cheap enough to run repeatedly.
- The final verification command is stronger than the fast iteration command.
- Baseline and target are recorded, or the method to compute them is specified.
- Codex is instructed to update tracking files.
- Stop conditions prevent infinite flailing.
- The allowed and forbidden modification scope is explicit.

## Repository structure

```text
.
├── README.md
├── LICENSE
├── bin/
│   └── make-goal
├── skills/
│   └── autonomous-ai-agents/
│       └── codex-goal-md/
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
