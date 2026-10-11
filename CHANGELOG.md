# Changelog

All notable changes to the SIMAGENT colony are documented here.

Format: human-readable, append-only entries with contributor initials and ISO timestamps.
Multiple models/agents contribute. Use this for auditing, recovery, and handoff.

Initials:
- SG: SuperGrok
- CG: ChatGPT
- GB: SIMAGENT (Grok Bot)
- G: Grok (this session / general)

## Unreleased / Current

### 2026-10-11
- **GB**: Added `docs/colony/ORGCHART.md`: the colony run like an electronic LLM company. Mermaid org chart, nine roles (Queen / Core, Coordinator, Forager, Builder, Evaluator, Critic / Auditor, Chronicler, Granary keeper, Scout) with mission, inputs, outputs, actions, food budget class, reporting line, gates (G1 Verify, G2 Audit, G3 Surface), and failure modes; delegation and escalation rules; when-not-to-spawn overhead rule; governance and agent versioning / retirement; Phase 0-2 rollout. Small edits: new actions in `ROLES.md`, org roles and Agent Card role values in `PROTOCOLS.md`, index line in `docs/colony/README.md`. Docs only. (see TEAM.md notes)

### 2026-10-11
- **GB**: Phase 0 colony documentation added in `docs/colony/` (README, LANGUAGE, JOIN, ROLES, PROTOCOLS, METRICS, OPERATORS). Weekly report template in `reports/`. Join issue template in `.github/ISSUE_TEMPLATE/`. Docs and templates only; no game code or loop changes. Merged to main with owner approval. (see TEAM.md notes)

### 2026-10-11
- **G**: Added this CHANGELOG.md for multi-model contribution tracking, auditing, and recovery. Private experimental systems stay out of this public repo.

## Guidelines for contributors

- Append new entries at the top under Unreleased (or create dated sections).
- Include initials, date/time if precise, short description of what changed and why.
- Reference related files, issues, or TEAM.md claims when relevant.
- Prefer additive entries. Do not rewrite history.
- Keep private experimental systems out of this public changelog and repo unless the owner directs otherwise.
