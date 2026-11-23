# Module: Multi-Agent System

Part of the FDE Automation Skill. The rules in SKILL.md and references/common-standards.md always apply on top of this file.

Covers: a team of agents with defined roles that hand work to each other.
Roles: International AI Expert, Cloud Automation Expert, Security Expert, QA Expert. Add a Language Expert for each approved language (SKILL.md, Section 7.5).
Use Full Mode by default. Express Mode is not suitable.
 If any agent uses a social channel, read references/social-media.md. If any agent touches money, read references/finance.md.
First check: does the job really need several agents? If one agent with several tools can do it, Claude says so in Urdu and offers references/single-agent.md instead. More agents mean more cost, more failure points, and harder testing.

## Extra Intake Questions
1. Overall goal of the system (owner types, Claude summarises).
2. Which steps are done today by people? (so each can become an agent role)
3. Where must a human always join? (approval points)
4. Expected load: tasks per day.

## Workflow (Added to the Shared Phases)

### A. Agent Map (Phases 2 and 6)
For each agent write one entry in `docs/AGENT_SPEC.md` (template in common-standards.md, Section 10.5): role, inputs, outputs, allowed tools, what it must never do, hand-off rule. Draw the map as a diagram in the Artifact Panel. The owner approves it at G6.

### B. Orchestration
1. Which agent starts the work, how work is handed over, and where a human joins.
2. Pick one pattern and explain it in Urdu: one lead agent that assigns tasks to others, a fixed chain (A then B then C), or agents that work in parallel and one agent that combines the results. Give options with pros, cons, and cost. Recommend the simplest one that works.
3. Every hand-off passes a short, structured message (task, data needed, result format), not the whole chat history.

### C. Shared Memory Rules
1. State what each agent may read and write. Personal data is minimised and not shared with agents that do not need it.
2. Only one agent owns each piece of data (single owner), so agents do not overwrite each other.
3. Set a retention period for stored memory.

### D. Failure Handling
1. What happens when an agent fails, times out, or returns a bad result: retry (how many times), use a fallback, alert the owner, or stop.
2. A task never loops forever. Set the maximum steps per task, the maximum calls per agent, and a daily cost cap.
3. A repeated task must not create duplicate records or messages (idempotency).
4. One kill switch pauses the whole system. Each agent can also be paused alone. Each pause is logged.

### E. Security
1. Treat every agent's input from outside (customers, emails, web pages, files) as untrusted. One agent that is tricked must not be able to command another agent. Agents check each other's messages against the agreed format.
2. Least privilege: each agent has only its own tools and credentials.
3. Test prompt injection across the chain, not only at the first agent.

### F. Build Order (Phase 10)
Tools first, then single agents (each tested alone), then pairs, then the full team. One module at a time, with the Coding Gate (SKILL.md, Section 2.4) and a module approval each time.

### G. Evaluation (Phases 12 and 13)
1. Evals for each agent and for the whole team on realistic end-to-end tasks.
2. Measure success rate, steps per task, time per task, and cost per task. Compare to the targets approved in Phase 4.
3. Log every agent step so a bad result can be traced to the agent that caused it (traces).
4. Test failure cases: an agent down, a tool error, a wrong hand-off, a loop.

### H. Go-Live
Pilot with a human supervisor first. Daily owner summary. Fresh owner confirmation before going live. A change of model, prompt, or agent map is a change request and is tested again.

## Gates Checklist for This Module
- G2: purpose of the system and agent list approved.
- G6: agent map and orchestration pattern approved.
- G9: memory and data rules approved, then the Coding Gate.
- G12: team eval results and cost caps approved.
- G13: failure and injection tests passed.

## AI Engineering Additions for Multi-Agent Systems

1. Save task state (which step, which agent, what result) so a task can resume after a crash, and so a human can see where it stopped.
2. Define one message format for hand-offs (task, data, expected result, deadline). Reject messages that do not match it.
3. Give each agent its own step limit and cost limit as well as a total limit.
4. Put human checkpoints before any irreversible action and at the end of long chains.
5. If agents from different systems must talk to each other, check whether a standard agent protocol exists and is mature. Claude verifies this in current documentation before recommending it.
