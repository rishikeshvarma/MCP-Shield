# 05 - Literature Survey & Research Papers

> **This is the canonical bibliography.** The presentation script's §5 and §9 and the README citation are derived from this list. Every entry below has been checked against its arXiv page or primary source; entries from earlier drafts that could not be verified (three 2026 papers listed without authors or identifiers, a "TechStories" survey, and a mis-attributed MCP-Guard citation) have been removed.

---

## 🌟 Primary Base Paper

**[1] Maloyan, N. & Namiot, D.** "Breaking the Protocol: Security Analysis of the Model Context Protocol Specification and Prompt Injection Vulnerabilities in Tool-Integrated LLM Agents." arXiv:2601.17549, January 2026. — Local copy: [[Breaking the Protocol.pdf]]

- **Role**: primary base paper.
- **What it establishes**: the first formal security analysis of the MCP specification (v1.0). Three protocol-level vulnerabilities: (1) absence of capability attestation, (2) bidirectional sampling without origin authentication, (3) implicit trust propagation in multi-server configurations. PROTOAMP framework; 847 attack scenarios across five server implementations; MCP amplifies ASR by 23–41% over non-MCP baselines; overall ASR 52.8%; sampling-based attacks 58–72%; one compromised server among five reaches 78.3% ASR with a 72.4% cascade rate.
- **Its defence, ATTESTMCP**: capability attestation + message authentication; ASR 52.8% → 12.4% (76.5% reduction); median overhead 8.3 ms cold / 2.4 ms warm per message; pinning is explicitly trust-on-first-use.
- **Why it motivates MCP-Shield**: the paper states the residual 12.4% ASR "primarily reflects indirect injection through legitimately-retrieved content" — the content-level surface attestation cannot see. MCP-Shield targets exactly that residual and is complementary to ATTESTMCP.

---

## Part I — MCP-Specific Security Research

**[2] Yang, Y., Wu, D. & Chen, Y.** "MCPSecBench: A Systematic Security Benchmark and Playground for Testing Model Context Protocols." arXiv:2508.13220, 2025.
- 17 attack types across four MCP attack surfaces; standardised evaluation playground. **Used as a held-out evaluation suite** (never for training).

**[3] Wang, Z. et al.** "MCP-Bench: Benchmarking Tool-Using LLM Agents with Complex Real-World Tasks via MCP Servers." arXiv:2508.20453, 2025.
- 28 servers / 250 tools; a *capability* benchmark with no security focus (as the base paper also notes). Used only as a source of realistic benign tool traffic.

**[4] Xing, W., Qi, Z., Qin, Y., Li, Y., Chang, C., Yu, J., Lin, C., Xie, Z. & Han, M.** "MCP-Guard: A Multi-Stage Defense-in-Depth Framework for Securing Model Context Protocol in Agentic AI." arXiv:2508.10991, 2025 (v2 2026).
- Three-stage pipeline: static scan → E5-based neural detector (96.01% accuracy) → LLM arbitrator; introduces MCP-AttackBench (70,448 samples). Closest academic architecture; differs from MCP-Shield in keeping an LLM on the decision path and in having no pinning or taint state.

**[5] Turgut, İ. A. & Gümüş, E.** "CASCADE: A Component Ablation and Corpus Audit of a Layered Local Defense for MCP-Based Systems." arXiv:2604.17125, April 2026 (rev. September 2026).
- Rule → semantic → local review-LLM layers on a 5,000-sample corpus. Key findings we rely on: the review LLM ran on 32.56% of requests at 2.51 s each and changed no classification outcomes; reported FPR swings from 11.70% to 1.51% depending on how referrals are counted; recall differs sharply between original (86.20%) and template-generated (99.88%) material. Direct evidence for keeping Tier 3 an encoder and for reporting FPR precisely.

**[6] Rostamzadeh, M., Narula, S., Birhan, N., Ghasemigol, M. & Takabi, D.** "MCP-DPT: A Defense-Placement Taxonomy and Coverage Analysis for Model Context Protocol Security." arXiv:2604.07551, April 2026.
- Organises threats across six architectural layers and maps where mitigation responsibility should sit (host, intermediary, server, ecosystem). Frames MCP-Shield's placement: an intermediary (proxy) covering the host-orchestration and transport gaps the taxonomy identifies as under-served.

---

## Part II — Prompt Injection, Defences, and Benchmarks

**[7] Greshake, K., Abdelnabi, S., Mishra, S., Endres, C., Holz, T. & Fritz, M.** "Not What You've Signed Up For: Compromising Real-World LLM-Integrated Applications with Indirect Prompt Injection." ACM AISec 2023. arXiv:2302.12173.
- Defines indirect prompt injection as a distinct class.

**[8] Zhan, Q., Liang, Z., Ying, Z. & Kang, D.** "InjecAgent: Benchmarking Indirect Prompt Injections in Tool-Integrated LLM Agents." Findings of ACL 2024. arXiv:2403.02691.
- 1,000+ indirect-injection test cases for tool-integrated agents. **Used in the training pool**, never for evaluation.

**[9] Debenedetti, E., Zhang, J., Balunović, M., Beurer-Kellner, L., Fischer, M. & Tramèr, F.** "AgentDojo: A Dynamic Environment to Evaluate Prompt Injection Attacks and Defenses for LLM Agents." NeurIPS 2024 Datasets & Benchmarks. arXiv:2406.13352.
- Dynamic agent environment (629 security test cases). **Used as a held-out evaluation suite** for ASR and utility retention.

**[10] Debenedetti, E., Shumailov, I., Fan, T., Hayes, J., Carlini, N., Fabian, D., Kern, C., Shi, C., Terzis, A. & Tramèr, F.** "Defeating Prompt Injections by Design." arXiv:2503.18813, 2025.
- CaMeL: extracts control and data flow from the trusted query so untrusted data can never alter program flow; capabilities enforce policies at tool calls; 77% of AgentDojo tasks solved with provable security. The principled (host-rewriting) solution to taint propagation; MCP-Shield's proxy-held taint state is the deployable approximation for hosts that cannot be rewritten.

**[11] Hines, K., Lopez, G., Hall, M., Zarfati, F., Zunger, Y. & Kiciman, E.** "Defending Against Indirect Prompt Injection Attacks With Spotlighting." arXiv:2403.14720, 2024.
- Host-side marking of untrusted content; complementary to proxy-side classification.

**[12] Chennabasappa, S., Nikolaidis, C., Song, D. et al.** "LlamaFirewall: An open source guardrail system for building secure AI agents." arXiv:2505.03574, May 2025.
- Includes Prompt Guard 2 (mDeBERTa-base 86M and DeBERTa-xsmall 22M), a state-of-the-art small encoder for injection/jailbreak detection. MCP-Shield's Tier 3 is the same model class made MCP-aware; Prompt Guard 2 is our evaluation baseline (Llama licence prevents reuse of its weights in an Apache-2.0 project).

**[13] Meta.** "Llama Prompt Guard 2" model card, Hugging Face (`meta-llama/Llama-Prompt-Guard-2-86M`, `-22M`), 2025.
- Reports 19.3 ms (22M) and 92.4 ms (86M) per 512-token classification on an A100 — the latency reference behind the Tier 3 targets in `02`.

**[14] Bhagwatkar, R., Kasa, K., Puri, A., Huang, G., Rish, I., Taylor, G. W., Dvijotham, K. D. & Lacoste, A.** "Indirect Prompt Injections: Are Firewalls All You Need, or Stronger Benchmarks?" arXiv:2510.05244, 2025 (rev. 2026).
- Simple input/output filters at the agent–tool boundary reach near-perfect security on the four public benchmarks — because the benchmarks are weak. Supports the firewall placement; mandates the adaptive red-team set in `03` §9.4.

**[15] Fomin, M. et al.** "When Benchmarks Lie: Evaluating Malicious Prompt Classifiers Under True Distribution Shift." arXiv:2602.14161, February 2026.
- Pooled cross-validation over-reports AUC by 8.0–16.5 points versus leave-one-dataset-out. Basis for the LOSO protocol and the strict train/held-out separation in `03`.

**[16] Fairoze, J., Garg, S., Lee, K. et al.** "Bypassing Prompt Guards in Production with Controlled-Release Prompting." arXiv:2510.01529, October 2025.
- Shows bounded input filters can be bypassed by prompts indecipherable to the filter yet tractable to the target LLM. Cited in Known Limitations; motivates defence in depth (taint state, pinning) rather than classifier-only protection.

**[17] Zhang, Z. et al.** "Agent-SafetyBench: Evaluating the Safety of LLM Agents." arXiv:2412.14470, 2024.
- Multi-turn agent safety evaluation; no evaluated agent exceeds 60% safety.

**[18] Lupinacci, M. et al.** "The Dark Side of LLMs: Agent-based Attacks for Complete Computer Takeover." arXiv:2507.06850, 2025.
- Chained tool calls escalate from a web-search tool to full host takeover — the multi-step trajectory the Stage 2 state tracker targets.

---

## Part III — Industry Reports, Specifications, and Tools

**[19] Model Context Protocol.** Specification revision `2026-07-28` — "Key Changes" changelog. modelcontextprotocol.io/specification/2026-07-28/changelog.
- SEP-2567 (sessions removed), SEP-2575 (stateless `_meta`, `server/discover`, `subscriptions/listen`), SEP-2243 (`Mcp-Method` / `Mcp-Name` headers, `HeaderMismatchError`), SEP-2549 (`ttlMs` / `cacheScope`), SEP-2322 (MRTR), SEP-2577 (Sampling / Roots / Logging deprecated).

**[20] Model Context Protocol.** "Security Best Practices." modelcontextprotocol.io/specification/2026-07-28/basic/security_best_practices.
- Confused-deputy patterns, token passthrough, and consent guidance.

**[21] Equixly.** "MCP Servers: The New Security Nightmare." Blog, 29 March 2025.
- Of tested MCP servers: 43% vulnerable to command injection, 22% to path traversal, 30% allowed unrestricted URL fetching.

**[22] HackerOne.** "9th Annual Hacker-Powered Security Report." October 2025.
- Valid prompt-injection reports up 540% year over year; AI-related vulnerability reports up 210%.

**[23] NVD.** CVE-2025-49596 — MCP Inspector: unauthenticated localhost HTTP server exploitable from a malicious web page. Cited by the base paper; the reason the MCP-Shield dashboard requires a token on every endpoint (`07` §3).

**[24] Invariant Labs.** `mcp-scan` — GitHub, 2025. Static scanning of installed MCP servers for tool poisoning and rug-pulls; optional runtime proxy with guardrail rules.

**[25] riseandignite.** `mcp-shield` — GitHub, 2025. Static scanner of installed MCP server configurations. **Unrelated to this project; name collision noted in `00` and README.**

**[26] Cisco AI Defense.** `mcp-scanner` — GitHub / PyPI (`cisco-ai-mcp-scanner`), 2025. Static / CI scanning of MCP servers and tools.

---

## How the survey maps onto the design

|Design element ([[02 - System Architecture & Technical Implementation]])|Supported by|
|:--|:--|
|Proxy placement, both directions|[1] residual analysis, [6] placement taxonomy, [14]|
|Encoder classifier on the hot path, not a generative SLM|[5] 2.51 s/call finding, [12], [13]|
|Held-out benchmarks, LOSO, adaptive set|[14], [15], [16]|
|Session taint state as a deployable approximation|[10], [11]|
|Pin + scan for descriptions|[1] TOFU discussion, [24]|
|Tier 1 argument policy for command injection|[21]|
|Dashboard token on every endpoint|[23]|
