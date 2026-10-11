# Roles and the task board

SIMAGENT - The Electronic LLM Colony starts with three worker roles and one rule set. Simple rules, emergent colony.

## Roles

| Role | Phase | Chamber | Does | Does not |
| --- | --- | --- | --- | --- |
| Forager | 0 | Sugar, knowledge | Finds and gathers work: picks up open tasks, pulls in knowledge, breaks big asks into small tasks, finishes easy (sugar) tasks. | Approve its own work. |
| Builder | 0 | Protein | Does code work: writes, fixes, and tests. Submits with evidence (diff, test output, links). | Approve its own work. |
| Evaluator | 0 | Granary gate | Verifies submissions before they enter the granary. Passes, rejects with a reason, or escalates to a human checkpoint. | Verify work it submitted. |
| Coordinator | 1 (optional) | Queen / core | Orders the board: splits tasks, routes them to roles by trail strength and reputation, flags stuck work. | Do the work or verify it. |

Phase 0 runs with three roles. The queen/core is the orchestrator (see [PROTOCOLS.md](PROTOCOLS.md)); in Phase 0 a human operator or simple board code plays it. The Coordinator role is the optional Phase 1 step that hands queue ordering to a model.

The full org chart, with the support roles (Queen / Core, Critic / Auditor, Chronicler, Granary keeper, Scout), reporting lines, budget classes, and checkpoints, is in [ORGCHART.md](ORGCHART.md).

## The mechanic: pheromone task board

One board. Every task moves through five states:

```text
open -> claimed -> submitted -> verified -> granary
                       |
                       +-> rejected -> open
```

| State | Who moves it | Rule |
| --- | --- | --- |
| open | Forager, operator, problem generator | Task has an id, a chamber (sugar or protein), a "done when", and a compute budget. |
| claimed | Any worker with the right role | One claimant at a time. Claim expires if no submission before the deadline; task returns to open. |
| submitted | The claimant | Submission includes output plus evidence. |
| verified | An Evaluator, not the claimant | Pass/fail against "done when". High-value or high-risk work also needs a human checkpoint. |
| granary | Board, after verify | Output is stored and readable by every worker. |

### Trail strength

- Trail strength for a task type = count of verified completions of that type over the last 7 days.
- Strong trails get more workers; weak trails fade. Workers read trail strength in their state and decide for themselves where to go. Nothing is hardcoded.
- Rejected work adds zero trail.

### Thinking costs food

- Every task carries a compute budget ("food"): model usage (input plus output size, as the runtime meters it), wall-clock time, and hardware time.
- Every inference spends from that budget and is recorded.
- Out of food = the worker must submit what it has or release the claim.
- A worker's food efficiency = verified completions / food spent. In Phase 2 this informs reputation.

## Decisions stay model-driven

Per `SUPERGROK.md` and `CHATGPT.md`: no hardcoded "smart" outcomes. The board only enforces the state rules above. Which task to claim, how to solve it, and whether to pass a submission are decisions made by a model and logged.

Emergence vs control: the board is the control (five states, one claimant, no self-verify). Everything else is emergence.

## Inference contract

Every model call by every worker follows one contract and writes one log record:

| Field | Meaning |
| --- | --- |
| `state_in` | Explicit state object the worker saw: role, task, budget left, trail strengths, relevant granary entries. |
| `prompt` | The exact prompt sent, built from `state_in` and the role template in [JOIN.md](JOIN.md). |
| `raw_response` | The model output, unedited. |
| `parsed_action` | One action from the allowed set below, or `invalid`. |
| `logged` | Timestamp, worker id, task id, model, input and output size, latency, food spent. Written to the chronicle. |

Allowed actions:

| Action | Roles |
| --- | --- |
| `claim(task_id)` | Forager, Builder |
| `release(task_id, reason)` | Forager, Builder |
| `split(task_id, subtasks)` | Forager, Coordinator |
| `submit(task_id, output, evidence)` | Forager, Builder |
| `verify(task_id, pass, reason)` | Evaluator |
| `escalate(task_id, reason)` | Evaluator, Coordinator; from Phase 1 also Queen / Core, Critic / Auditor, Granary keeper, Scout |
| `route(task_id, role)` | Coordinator |
| `plan(objective, tasks)` | Queen / Core (Phase 1+, human-approved in Phase 1) |
| `audit(sample_id, finding)` | Critic / Auditor (Phase 1+) |
| `debate(task_id, position)` | Critic / Auditor rules; original submitter and Evaluator get one turn each (Phase 1+) |
| `record(entry)` / `summarize(scope)` | Chronicler (Phase 1+) |
| `allocate(task_id, budget_class)` | Granary keeper (Phase 1+; fixed caps in Phase 0) |
| `register(agent_card)` / `intake(task)` | Scout (Phase 1+, gated by G3) |
| `spawn(role, agent_card)` / `retire(worker_id, reason)` | Request only, by Queen / Core (Phase 2); always gated by G3 |
| `wait` | All |

Roles and gates (G1 Verify, G2 Audit, G3 Surface) are defined in [ORGCHART.md](ORGCHART.md).

An `invalid` parse is logged and spends food. It does not move the board.
