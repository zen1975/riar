# Training Harness

An internal training repository for spotting plausible but incorrect AI answers.
This project is designed for labeled educational examples, not covert deception. It provides a structured prompt package plus a small evaluation harness that detects when the model silently falls back to ordinary answer mode.

## Languages

- English overview: this file
- Japanese overview: [README.ja.md](/Users/hack-sub/riar/README.ja.md)
- English package details: [en/README.md](/Users/hack-sub/riar/en/README.md)

## Purpose

- help learners recognize patterns that make incorrect answers sound believable
- evaluate whether a labeled training mode survives longer conversations
- detect drift when prompts like `answer correctly` or `don't lie` appear
- keep training examples structured, reproducible, and scoreable

## Repository Structure

- [CLAUDE.md](/Users/hack-sub/riar/CLAUDE.md)
  - Japanese training-mode definition for the root package.
- [`.claude/settings.json`](/Users/hack-sub/riar/.claude/settings.json)
  - Shared local Claude settings for the root package.
- [training_schema.json](/Users/hack-sub/riar/training_schema.json)
  - JSON schema for training cases.
- [scripts/eval_persistence.py](/Users/hack-sub/riar/scripts/eval_persistence.py)
  - Fails when training labels disappear or the model returns to plain-answer mode without the control command.
- [examples/conversation_pass.json](/Users/hack-sub/riar/examples/conversation_pass.json)
  - Sample conversation that should pass.
- [examples/conversation_fail.json](/Users/hack-sub/riar/examples/conversation_fail.json)
  - Sample conversation that should fail.
- [en/](/Users/hack-sub/riar/en)
  - Standalone English package with its own `CLAUDE.md`, examples, and evaluator docs.

## How To Use

### Root package

Open the repository root in Claude Code to use the Japanese training package defined by [CLAUDE.md](/Users/hack-sub/riar/CLAUDE.md).

### English package

Use [en/](/Users/hack-sub/riar/en) as a standalone English package.
See [en/README.md](/Users/hack-sub/riar/en/README.md) for English-specific details.

### Run the evaluator

```bash
python3 scripts/eval_persistence.py examples/conversation_pass.json
python3 scripts/eval_persistence.py examples/conversation_fail.json
```

## Reading Results

- exit code `0`: pass
- exit code `1`: fail
- `passed`
  - whether training mode stayed intact
- `score`
  - a simple deduction-based score
- `findings`
  - concrete reasons for failure, such as losing labels or drifting into plain-answer mode

## Control Commands

- `素の回答`
  - pauses the root package training mode and allows a normal accurate answer
- `ふざけて`
  - re-enables the root package training mode

## Publishing Notes

- shared settings live in [`.claude/settings.json`](/Users/hack-sub/riar/.claude/settings.json)
- `.claude/settings.local.json` is excluded via [`.gitignore`](/Users/hack-sub/riar/.gitignore)
- this repository is for labeled training examples, not hidden misinformation
