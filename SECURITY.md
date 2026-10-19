# Security Policy

MCP-Shield is security software, so we take reports seriously and we say plainly what we do and do not defend against.

## Reporting a vulnerability

**Please do not open a public issue for a security problem.**

Use GitHub's private vulnerability reporting: **Security → Report a vulnerability** on this repository. If that is unavailable, email the maintainers at `<security contact email>` with `[mcp-shield security]` in the subject.

Include what you can of:

- the version or commit, OS and Python version, and how the proxy was run (stdio wrap, HTTP upstream, sidecar);
- the message flow (host → server or server → host) and which tier you believe should have caught it;
- a minimal reproduction. For an **injection bypass**, describe the technique and, if you must include a payload, wrap it so it cannot execute (for example, base64 with a note) — the maintainers will reconstruct it privately;
- the block reference ID if one was produced, so we can find the audit entry.

What to expect:

| Step | Target |
|:--|:--|
| Acknowledgement | within 3 days |
| Triage and severity | within 10 days |
| Fix or mitigation for confirmed issues | within 90 days, sooner for anything exploitable by content alone |
| Credit | in the release notes and `CHANGELOG.md`, unless you prefer otherwise |

We follow coordinated disclosure: please give us until the fix ships, or 90 days, whichever is sooner, before publishing details. We will keep you informed throughout.

## Scope — what counts as a vulnerability

In scope:

- a **bypass**: content that hijacks a host through the proxy without the classifier, taint state, pinning, quarantine or argument policy acting — especially a bypass that works reliably rather than once;
- an **oracle**: any way for a server or for content to learn *why* something was blocked (tier, class, probability, segment) or to distinguish the proxy from the host on the wire;
- the proxy's or dashboard's **own surface**: authentication bypass, DNS rebinding, another OS user or a web page reaching the engine's local channel, a way to make the proxy spawn a process from a network, IPC or dashboard request, raw segments leaking into exported audit data;
- **supply chain**: the model download or checksum verification, the pinned llama.cpp build and GGUF download for the Stage 2 explainer, the PyPI build pipeline, dependencies;
- a **false positive with security impact**, for example a legitimate tool being permanently withheld.

Out of scope — these are documented limitations, not vulnerabilities (see `README.md` § Known Limitations and `06`):

- offline adaptive attacks against the public classifier weights (we harden with adversarial training and report adaptive-set ASR; we do not claim immunity);
- injected instructions the host LLM has *fully paraphrased* before acting (taint overlap catches verbatim and near-verbatim smuggling only);
- a poisoned tool description that a human reviewer accepted during first-contact quarantine;
- visual prompt injection through images (binary content is hashed, not classified);
- third-party OAuth-protected remote servers, which Stage 1 cannot front (spec token audience binding);
- a malicious stdio server observing `mcp-shield` in its parent process (such a server already runs with the user's privileges);
- the host prompt rules being ignored by a compromised host — they are an advisory layer by design.

If you believe one of these is worse than we have described, we still want to hear about it — report it privately and say so.

## Research rules

We welcome security research on MCP-Shield. Please:

- test only against instances, servers and accounts **you control**; never against third-party MCP servers or other people's hosts;
- do not degrade or deny service to anyone else;
- do not access, modify or exfiltrate data that is not yours;
- keep any attack corpus you build private and share it with us through the private channel.

Research conducted in good faith under these rules will not be the subject of a complaint by the maintainers.

## Supported versions

| Version | Supported |
|:--|:--|
| `main` (pre-release, Stage 1 build) | yes — report anything |
| tagged releases | the latest minor release |

## Our own security practices

- Branch protection on `main`: pull requests only, required reviews (two for security-sensitive paths), required status checks, linear history, no force pushes.
- Secret scanning with push protection, Dependabot alerts and updates, CodeQL on every pull request.
- Classifier weights are downloaded over HTTPS from Hugging Face Hub and verified by SHA-256 before use; no shared download token is ever shipped in the package.
- PyPI releases are built in CI with Trusted Publishing (OIDC); there is no long-lived PyPI token.
- Block errors sent to the host are opaque by design; full detail exists only in the local event store and dashboard.
