# AGM Code Review Agent

Automated Grafana Monitoring (AGM) review workspace. This README is maintained automatically by the AGM Code Review Agent after every applied change.

## Agent Features
- Review/scan code for bugs, dead code, duplicates, unused imports (report-only).
- Beautify code (remove dead code, unused/duplicate imports, safe fixes) with approval.
- Fix a specific issue with a test-first workflow.
- Test-only runs (whole-codebase + dummy alert flow).
- Push changes to GitHub via a review branch + PR.
- Logs every action to `logs/audit.jsonl` and updates this README.

## Protected Logic (never modified without explicit instruction)
- Orchestrator logic (Webex 5XX orchestrator functions).
- nsigma algorithm logic and parameters.
- mars library calls and configurations.
- LighthouseDB query structure.
- pass/fail determination logic.
- 30-day training window logic.
- Alert cleaning / output formatting logic.

## User Preferences (current)
- auto_approve_unused_imports: true
- auto_approve_dead_code: true
- show_diff_preview: true
- auto_run_tests_after_fix: false
- beautify_confirmed_before: true

## Change Log
| Date | Change ID | File | Change | PR |
|------|-----------|------|--------|----|
| 2026-06-30 | CHG_20260630063837 | code2.ipynb | Beautify: removed 10 exact-duplicate top-level import lines (pandas x2, numpy, time, IPython.display, matplotlib.pyplot, mars SNSigmaCI/NSigmaCI, os, requests, sys). No logic changed; orchestrator + protected logic untouched. Backup created. | pending |

## Sample Report
Latest scan/beautify summary (code2.ipynb):
- BUGS: none changed (flagged only) — e.g. no-op line `failed_results[~...str.contains('total_hits')]...` (result discarded); `eval()` use in 5XX payload parsing. Left untouched (protected/adjacent area).
- CODE QUALITY: 10 duplicate import statements removed.
- SUGGESTIONS (not applied): consolidate unused mars imports; replace `eval` with `ast.literal_eval` — deferred as they sit next to protected logic.
- Protected logic (orchestrator, nsigma, mars calls, lighthouse queries, pass/fail, 30-day window, alert formatting): untouched.
