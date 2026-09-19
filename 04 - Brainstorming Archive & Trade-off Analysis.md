# 04 - Brainstorming Archive & Trade-off Analysis

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
* **Core Concept**: A local security proxy (`mcp-shield proxy`) plus a secondary `@shield` decorator, running a three-tier cascade (structural policy gate → cache/signature layer → fine-tuned encoder classifier) over both directions of Model Context Protocol (MCP) traffic.
* **Selection Rationale**: Addresses the content-level attack surface (indirect injection, tool-description poisoning, parameter command injection) that the base paper identifies as the residual even after protocol-level attestation. Clear, staged implementation scope for one semester ([[00 - Master Architecture & Overview]] §5), a $0-by-default design that individual developers can actually adopt, and strong industry demand (HackerOne reported a 540% year-over-year rise in valid prompt-injection reports in 2025).
* **Scope note (post-review)**: the original plan fine-tuned a 7B generative SLM for the hot path and wrapped tool execution in gVisor; both were replaced during design review — see [[02 - System Architecture & Technical Implementation]] §0 for the decisions and reasons.

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
* **Why Passed**: Great concept, but its useful elements were folded into MCP-Shield's Stage 2 sliding-window state tracker ([[02 - System Architecture & Technical Implementation]] §6).

---

### Idea 7: "Agent-Lock"
* **Domain**: Probabilistic Cache-Stampede Resilience Gateway.
* **Core Concept**: Uses XFetch probabilistic early expiration algorithm to prevent concurrent agent thundering herd database crashes.
* **Why Passed**: Focused on database caching optimization rather than end-to-end agentic security architecture.
