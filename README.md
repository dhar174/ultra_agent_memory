# ultra_agent_memory

Custom memory system for coding agents like OpenAI Codex and Google Antigravity.

## Design documents

- [`docs/ARCHITECTURE_OVERVIEW.md`](docs/ARCHITECTURE_OVERVIEW.md) — coverage-first master description of the complete planned memory architecture, ontology, metadata, retrieval, cognitive mechanisms, agent workflows, governance, APIs, graphs, and operations.
- [`docs/ARCHITECTURE_COVERAGE_AUDIT_ADDITIONS.md`](docs/ARCHITECTURE_COVERAGE_AUDIT_ADDITIONS.md) — normative coverage-audit clarifications that preserve explicit requirements and candidate capabilities found during comparison against the originating design conversation.
- [`docs/ARCHITECTURE_REVIEW_CORRECTIONS.md`](docs/ARCHITECTURE_REVIEW_CORRECTIONS.md) — normative PR-review corrections. Currently authoritative where they strengthen retrieval security ordering and projection rebuildability through canonical interaction/audit event history.
- [`docs/DOCUMENTATION_ROADMAP.md`](docs/DOCUMENTATION_ROADMAP.md) — roadmap for decomposing the master design into focused specifications, schemas, diagrams, workflows, and implementation plans.

The project is currently in the architecture/specification phase. The master overview is intentionally broad. The coverage-audit addendum and review-corrections document are normative where they strengthen, correct, or make explicit requirements that were softened, implicit, or ordered incorrectly in the first overview; future focused documents should fold those clarifications into the relevant specifications without silently dropping recorded concepts.
