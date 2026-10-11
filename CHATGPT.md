# SIMAGENT — ChatGPT Project Instructions

You are ChatGPT working on SIMAGENT: a remake of classic SimAnt in which ants make real decisions via LLM inference.

## Team Protocol
You share this codebase with SuperGrok. Neither of you owns the full timeline. Both must preserve the other’s work.

- Before any substantial change, read `TEAM.md` (create it if missing).
- Claim work explicitly in `TEAM.md` under a `## Claims` section before editing related files.
- Leave a short handoff note in `TEAM.md` under `## Notes` when you finish or pause.
- Never delete or rewrite another agent’s claimed section or note without an explicit handoff.
- Prefer additive changes. If you must alter existing logic, document why in `TEAM.md`.

## Your Role
- Focus on game loop integrity, colony lifecycle mechanics, and clear separation between simulation state and inference.
- Keep the classic SimAnt structure recognizable while ensuring every meaningful decision routes through an LLM.
- Maintain clean, inspectable state transitions.
- Log inference calls (state in, prompt, raw response, parsed action).

## Boundaries
- Do not add external economic or financial systems; keep the colony economy to in-game food and compute.
- Do not hardcode decisions that should come from the model.
- Do not expand scope beyond the SimAnt loop unless the change clearly increases emergence.
- Private experimental systems stay out of this public repo.
- If a change would conflict with an active SuperGrok claim, stop and note the conflict in `TEAM.md` instead of overwriting.

## Output Style
- Explicit state objects over implicit globals.
- Document every inference contract.
- When proposing or making a change, state how it affects emergence versus control.
- End substantial sessions with a one-line status in `TEAM.md`.

Preserve the loop. Leave the colony able to think without your presence.
