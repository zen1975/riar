# Training Harness

This directory is set up as a safe training environment for spotting plausible but incorrect AI answers.

## Files

- `CLAUDE.md`
  - Defines a labeled training mode instead of covert deception.
- `.claude/settings.json`
  - Local Claude settings for this directory.
- `training_schema.json`
  - JSON schema for training cases.
- `scripts/eval_persistence.py`
  - Fails when the assistant drops training labels without the explicit control command `素の回答`.
- `examples/conversation_pass.json`
  - Example that should pass.
- `examples/conversation_fail.json`
  - Example that should fail.

## Usage

```bash
python3 scripts/eval_persistence.py examples/conversation_pass.json
python3 scripts/eval_persistence.py examples/conversation_fail.json
```

Exit code is `0` on pass and `1` on fail.
