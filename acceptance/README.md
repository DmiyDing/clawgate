# Acceptance Fixtures

Store real OpenClaw validation transcripts here after each meaningful test round.

Recommended folders:
- `safe/` for daily non-mutating live runs
- `mutating/` for local single-instance maintenance runs

Recommended files:
- `medium.txt`
- `high.txt`
- `critical.txt`
- `incomplete-high-risk.txt`

Do not add invented samples.
Only save real accepted transcripts from live OpenClaw runs.

Recommended notes to record alongside transcripts:
- which lane was used: `safe` or `mutating`
- OpenClaw base URL and model
- whether the environment was restored after the run
- which baseline cases passed: `low-readonly-openclaw`, `medium-direct-files`
