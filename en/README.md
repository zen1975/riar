# English Training Harness

This subdirectory is the English version of the training package for spotting plausible but incorrect AI answers.
Its purpose is not covert deception. Its purpose is to create clearly labeled training examples and evaluate whether that training mode stays consistent over longer conversations.

For the repository-level overview, see [README.md](/Users/hack-sub/riar/README.md).
For the Japanese overview, see [README.ja.md](/Users/hack-sub/riar/README.ja.md).

## Purpose

- help learners recognize patterns that make incorrect answers look believable
- detect when the model silently drifts back to ordinary answer mode
- test whether prompts like "answer correctly" or "don't lie" break the training format
- keep training samples structured and scoreable

## Package Structure

- [CLAUDE.md](/Users/hack-sub/riar/en/CLAUDE.md)
  - Defines the English training mode and requires the training label plus `RED FLAGS`.
- [`.claude/settings.json`](/Users/hack-sub/riar/en/.claude/settings.json)
  - Local Claude settings for this English package.
- [training_schema.json](/Users/hack-sub/riar/en/training_schema.json)
  - JSON schema for training cases.
- [scripts/eval_persistence.py](/Users/hack-sub/riar/en/scripts/eval_persistence.py)
  - Fails when the assistant drops the training labels without the explicit command `plain answer`.
- [examples/conversation_pass.json](/Users/hack-sub/riar/en/examples/conversation_pass.json)
  - Sample conversation that should pass.
- [examples/conversation_fail.json](/Users/hack-sub/riar/en/examples/conversation_fail.json)
  - Sample conversation that should fail.

## How To Use

### 1. Open the English package

Use the [en/](/Users/hack-sub/riar/en) directory as a standalone package in Claude Code.  
The local [CLAUDE.md](/Users/hack-sub/riar/en/CLAUDE.md) in that directory defines the behavior.

### 2. Run the evaluation harness

```bash
python3 scripts/eval_persistence.py examples/conversation_pass.json
python3 scripts/eval_persistence.py examples/conversation_fail.json
```

## How To Read Results

- exit code `0`: pass
- exit code `1`: fail
- `passed`
  - whether training mode stayed intact
- `score`
  - a simple deduction-based score
- `findings`
  - the concrete reasons the conversation failed, such as losing labels or switching back to plain-answer mode

## Control Commands

- `plain answer`
  - pauses training mode and allows a normal accurate answer
- `fiction mode`
  - re-enables the labeled training mode

## Notes For Publishing

- shared settings live in [`.claude/settings.json`](/Users/hack-sub/riar/en/.claude/settings.json)
- machine-specific local settings should stay out of version control
- this package is designed for labeled training examples, not hidden misinformation
