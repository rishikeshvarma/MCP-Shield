# Contributing to MCP-Shield

Thank you for helping build a semantic firewall for the Model Context Protocol. This document covers how to set up a development environment, how changes are reviewed, and the rules that apply to security-sensitive code. Please read it before opening a pull request.

MCP-Shield is a student-led research project (KMIT, Department of CSE — AI & ML). It is in its **design and Stage 1 build phase**: nothing in the benchmark table is measured yet, and the architecture documents (`00`–`07`) are the source of truth. If a change you propose contradicts them, open an issue first — the docs change before the code does.

## Ground rules

- **Be honest about numbers.** Every latency, ASR and FPR figure in this repository is a target until the Phase 5 benchmark publishes measured values. Do not add a number to a README, docstring or comment that has not been measured with the harness in `benchmarks/`.
- **Security-sensitive code gets two reviewers.** Anything under `proxy/`, `cascade/`, `taint.py`, `pinning.py`, `spotlight.py`, `auth.py`, `audit.py`, or `dashboard/` requires review from two maintainers (see `.github/CODEOWNERS`). These are the paths where a subtle bug is a bypass.
- **No attack data in this repository.** Adaptive red-team examples, exploit corpora and captured traffic never go here — not in code, tests, fixtures or issue attachments. Synthetic *benign* fixtures and the public benchmark loaders are fine. The adaptive set is released to researchers on request only (`03` §11).
- **No secrets, ever.** No API keys, tokens, `.env` files or real credentials, including in test fixtures. Secret scanning with push protection is enabled; a blocked push is a feature.
- **Report vulnerabilities privately** via [SECURITY.md](SECURITY.md), never in a public issue or PR.

## Development setup

```bash
git clone https://github.com/<org>/mcp-shield.git
cd mcp-shield
python -m venv .venv && source .venv/bin/activate      # Windows: .venv\Scripts\activate
pip install -e ".[dev]"
pre-commit install
mcp-shield doctor            # verifies ONNX Runtime provider, downloads the classifier, runs a 5-segment self-test
pytest
```

Python 3.10+ is required. The classifier (`mcp-shield-clf`, ~300 MB ONNX) is downloaded from Hugging Face Hub on first run to the platform cache directory; tests that need it are marked `@pytest.mark.model` and skipped when it is absent.

## Branches, commits and pull requests

1. **Branch from `main`**: `feat/<topic>`, `fix/<topic>`, `docs/<topic>`, `sec/<topic>` for security hardening.
2. **Commits** use [Conventional Commits](https://www.conventionalcommits.org/) (`feat:`, `fix:`, `docs:`, `test:`, `sec:`, `refactor:`), a body that explains *why*, and a DCO sign-off:
   ```bash
   git commit -s -m "fix(cascade): treat abstain-band verdicts as never cacheable"
   ```
   `-s` adds `Signed-off-by: Your Name <you@example.com>`, which certifies the [Developer Certificate of Origin](https://developercertificate.org/). PRs without sign-off are not merged. AI-assisted commits should add `Co-Authored-By:` for the tool used.
3. **Open a PR against `main`** using the template. CI must be green: `ruff`, `mypy --strict`, `pytest`, `pip-audit`, CodeQL. Keep PRs small — one logical change; a design change and its implementation are two PRs.
4. **Reviews**: one approving review for ordinary code and docs; **two** for the security-sensitive paths above. Reviewers check the PR against `02 - System Architecture & Technical Implementation.md` — if the code and the doc disagree, the doc wins and the PR changes.
5. **Squash-merge** with a Conventional Commit title. Force-pushes to `main` are disabled; history is linear.

## What a good PR includes

- Tests. Detection logic needs both a positive case (the attack is caught) and a benign twin (the same shape without the attack is allowed) — false positives are bugs too, and the shipped FPR target is 0.5% per document.
- No change to the four error classes' *wire* format: the host must continue to receive only `{"code": -32000, "message": "Request blocked by MCP-Shield policy (ref …)"}`. Detail belongs in the audit log and dashboard (`02` §8). A PR that puts a tier, class, probability or segment into the host-facing error will be declined.
- No new identifying headers on the server side (`Via`, `X-Forwarded-*`, `User-Agent`, `Server`) — the proxy is transparent to servers by design.
- Docs updated in the same PR when behaviour changes, including the relevant `Stage` tag.
- A changelog line in `CHANGELOG.md` under *Unreleased*.

## Changing the host prompt rules

`Host Prompt Rules.md` is versioned. Add new rules at the **end** with the next `H-n` ID, never renumber or delete existing ones (deprecate in place instead), update the short form if it is affected, and add a row to the changelog table with the date and the reason. Reference rule IDs in code comments and tests (`# H-16: markers denote untrust only`).

## Model and data changes

- A classifier change ships with an updated model card: held-out ASR, per-document FPR, LOSO gap, adaptive-set ASR, seeds, and the adversarial-round table (`03` §7.1, §9).
- Training-pool and held-out sources must stay separate: **AgentDojo and MCPSecBench are never used for training or threshold selection.** A PR that touches `benchmarks/` or the dataset builder must state which side each source is on.
- Every data source carries its licence in the dataset card; anything that cannot be redistributed stays out of released artefacts.

## Code style

- `ruff` for linting and formatting (config in `pyproject.toml`); `mypy --strict` on the package.
- No `shell=True`, anywhere. Subprocess calls take argument lists. This rule exists because the whole point of Tier 1 is that servers get this wrong.
- Fail closed: if the model is missing, inference raises, or a config value is unparseable, the proxy blocks server → host content and logs loudly. `--fail-open` exists for development only and is never a default.
- Comments explain *why*, not *what*. Reference the design doc section when a non-obvious constraint applies (`# 02 §2: annotations may only raise a privilege class`).

## Issues

Use the issue templates. For a false positive, include the segment (redact anything private), the tool and server, and the reference ID from the block message; for a bypass, **do not** post the payload — use SECURITY.md.

## Licence

By contributing you agree that your contributions are licensed under the [Apache License 2.0](LICENSE.md) and that you have the right to submit them (DCO).
