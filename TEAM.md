# TEAM.md

Shared coordination file for SuperGrok and ChatGPT.
Do not delete sections. Append only unless an explicit handoff occurs.

## Claims
- [ ] SuperGrok — inference contracts & state-to-prompt design
- [ ] ChatGPT — game loop integrity & colony lifecycle
- [x] SIMAGENT (Grok Bot) - GB-007 Phase 0 colony docs in docs/colony/ (merged to main)

## Notes
- (append handoff notes here)
- 2026-10-10 SIMAGENT (Grok Bot): GB-007 Phase 0 docs added in `docs/colony/` (README, LANGUAGE, JOIN, ROLES, PROTOCOLS, METRICS, OPERATORS), weekly report format in `reports/`, and a join issue form in `.github/ISSUE_TEMPLATE/`. Docs and templates only: no game code, no loop changes. Handoff: SuperGrok can turn the inference contract in `ROLES.md` and the Agent Card in `PROTOCOLS.md` into real schemas; ChatGPT can map the task board states in `ROLES.md` onto the colony lifecycle. Neither claim above was touched. Emergence vs control: roles and the task board are simple rules; every decision stays model-driven and logged. Merged to main 2026-10-11 with owner approval.
- 2026-10-11 SIMAGENT (Grok Bot): added `docs/colony/ORGCHART.md` (org chart, reporting lines, budget classes, gates G1-G3, governance, versioning/retirement, rollout) with small additive edits to `ROLES.md` (new phased actions), `PROTOCOLS.md` (org roles, Agent Card role values), and the colony index. Docs only. Handoff: SuperGrok can add the new actions to the inference contract schema; ChatGPT can wire G1-G3 into the colony lifecycle. Emergence vs control: gates and budget caps add control; task choice, planning, and verification stay model-driven. Owner-approved.

## Status
- Last updated: 2026-10-11 — Org chart (ORGCHART.md) added to docs/colony with owner approval.
