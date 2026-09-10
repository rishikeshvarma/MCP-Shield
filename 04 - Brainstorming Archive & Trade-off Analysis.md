# 04 - Brainstorming Archive & Trade-Off Analysis

> **Context**: Summary of the 2-week intensive brainstorming session evaluating 7 candidate Agentic AI project ideas for the final-year major project.

---

## Evaluated Ideas Summary

### Idea 1: "Agent-Transactional (ATX)"
* **Domain**: Distributed ACID Compliance for Heterogeneous Agent Clusters.
* **Core Concept**: Semantic Transaction Coordinator (STC) using Two-Phase Commit and MVCC for multi-agent shared state updates.
* **Why Passed**: High complexity in distributed database simulation; harder to benchmark in 1 semester compared to protocol security.

---

### Idea 2: "MCP-Shield" (SELECTED)
* **Domain**: Protocol-Level AI Safety, Inter-Agent Access Control, and Real-Time Injection Defense.
* **Core Concept**: Inline reverse proxy & 3-tier cascade firewall for Model Context Protocol (MCP) traffic.
* **Selection Rationale**: Solves the #1 enterprise security barrier blocking agentic AI adoption in 2026. Highly relevant, clear implementation scope (Python `@shield` decorator + fine-tuned SLM), and huge industry demand.

---

### Idea 3: Auto-Recovering CI/CD Build Agent with Semantic Git Merging
* **Domain**: Autonomous Self-Healing & Version Control Systems.
* **Core Concept**: Intercepts build failure logs, strips fluff, designs patches, and auto-commits fix branches.
* **Why Passed**: SWE-bench style build fixing is heavily saturated by existing tools (Cursor, Claude Code, Devin).

---

### Idea 4: "VerifyAgent"
* **Domain**: Autonomous API Schema Drift Detection & Guardrail Synthesis.
* **Core Concept**: Reverse-proxy monitoring downstream API changes (Stripe, Twilio) and dynamically generating runtime adapters.
* **Why Passed**: Strong operational value, but narrower security impact than MCP protocol-level defense.

---

### Idea 5: Autonomous Environment Builder & Shift-Left Framework
* **Domain**: Automatic Local Test Environment Creation & Cloud Copying.
* **Core Concept**: Multi-agent team (Supervisor, Environment Weaver, Adversarial Actor, Audit Verification) spinning up local k3d/LocalStack sandboxes.
* **Why Passed**: Large DevOps infrastructure overhead required for local hardware setup.

---

### Idea 6: "StateSync (Agent-Git)"
* **Domain**: Dynamic State Conflict Resolver for Multi-Agent Cascades.
* **Core Concept**: Non-linear memory version control for LangGraph ecosystems to isolate and rewind cascading hallucinations.
* **Why Passed**: Great concept, but elements incorporated directly into MCP-Shield's state graph tracking strategy.

---

### Idea 7: "Agent-Lock"
* **Domain**: Probabilistic Cache-Stampede Resilience Gateway.
* **Core Concept**: Uses XFetch probabilistic early expiration algorithm to prevent concurrent agent thundering herd database crashes.
* **Why Passed**: Focused on database caching optimization rather than end-to-end agentic security architecture.
