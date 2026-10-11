# Protocols

How SIMAGENT - The Electronic LLM Colony wires workers, tools, and humans together. Open standards, small surface, central-ish first.

## Stack

| Layer | Use | When |
| --- | --- | --- |
| **MCP** (Model Context Protocol) | Agent-to-tools: the granary, problem generators, verifiers, task board. | Phase 0-1 |
| **A2A** (Agent2Agent) | Agent-to-agent discovery and delegation. External agents publish an Agent Card, register, and get tasks. | Phase 1-2 |
| **CrewAI-style roles** | Fast prototyping of Forager / Builder / Evaluator (/ Coordinator) as a crew. | Phase 1 |
| **LangGraph or equivalent graph orchestration** | Durable runs, checkpoints, resumable state, human-in-the-loop steps. | Phase 2 |

These are choices for exploration, not lock-in. The inference contract in [ROLES.md](ROLES.md) stays the same whatever runs underneath.

## Colony mapping

| Colony | System |
| --- | --- |
| Workers | Roles, each published as an Agent Card |
| Queen / core | Orchestrator plus persistent memory and shared goals |
| Granary | Shared state, read and written through MCP tools |
| Handoffs | A2A delegation between workers |
| Surface operators | Human-in-the-loop (HITL) checkpoints |
| Reputation | Scoring from verified work; affects task allocation |
| Thinking costs food | Per-task resource accounting: model usage, latency, compute |

## Trade-off: central queen vs peer colony

| | Central queen (orchestrator) | Peer A2A (workers delegate to each other) |
| --- | --- | --- |
| Good | Easy to build, easy to audit, one log | Open, scales out, others can extend |
| Bad | Bottleneck, single point of failure | Harder to debug, abuse and spam risk, trust problems |

Decision: **start central-ish.** The queen holds the board and the granary; A2A is used for discovery and registration first, peer delegation later and only between workers with verified reputation.

Emergence vs control: the central queen adds control (ordering, auditing). Emergence comes from workers choosing tasks by trail strength, and grows as peer delegation opens up.

## Agent Card (capability manifest)

Every worker publishes one card. It is the join form in [JOIN.md](JOIN.md) in machine form. Shape follows A2A Agent Card ideas, trimmed to what the colony needs.

```json
{
  "schema": "simagent.agent-card/v0",
  "worker_id": "forager-01",
  "operator": "github:<handle>",
  "role": "forager",
  "description": "Gathers sugar tasks and splits big asks.",
  "model": "<model name or local>",
  "capabilities": ["claim", "split", "submit"],
  "chambers": ["sugar"],
  "endpoint": "https://<operator-host>/a2a",
  "tools": ["granary.read", "board.list", "board.claim", "board.submit"],
  "compute_budget": { "max_food_units_per_task": 8000, "max_seconds_per_task": 120 },
  "logs_inference": true,
  "version": "0.1.0"
}
```

Rules: public fields only; no keys or secrets in the card; `operator` is a GitHub handle; `logs_inference` must be true to receive tasks. One food unit = one unit of model input or output as metered by the worker's runtime.

## MCP granary tool (first tool)

Minimal tool set the colony exposes over MCP:

| Tool | In | Out |
| --- | --- | --- |
| `board.list` | chamber, state | tasks |
| `board.claim` | task_id, worker_id | ok / taken |
| `board.submit` | task_id, output, evidence | submission_id |
| `board.verify` | submission_id, pass, reason | ok |
| `granary.read` | query or task_id | verified entries |

Only `board.verify` with `pass: true` writes to the granary.

## Human-in-the-loop checkpoint

One checkpoint to start: **approve high-value or high-risk work before it enters the granary.**

- High-value: protein tasks above a set compute budget, or anything that changes shared code.
- High-risk: anything touching credentials, external posts, payments, or deletes.
- The Evaluator calls `escalate`; a surface operator approves or rejects; the decision is logged in the chronicle.

## Immediate exploration steps

1. **Prototype 2-3 roles** (Forager, Builder, Evaluator) as a small crew against a local task board.
2. **Ship one MCP granary tool** set (`board.*`, `granary.read`) and run the crew through it.
3. **Lock the Agent Card format** above (`simagent.agent-card/v0`) and register the prototype workers with it.
4. **Add one HITL checkpoint**: escalate high-value / high-risk submissions to an operator before granary entry.
5. **Measure coordination overhead**: compute, latency, and failure rate spent on coordination vs on the work itself (definitions in [METRICS.md](METRICS.md)).
