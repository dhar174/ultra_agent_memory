# Ultra Agent Memory — Review Corrections

> **Status:** Normative correction to `ARCHITECTURE_OVERVIEW.md` and `ARCHITECTURE_COVERAGE_AUDIT_ADDITIONS.md` for the specific issues identified during PR review.
>
> **Precedence:** Where the master overview or coverage-audit addendum shows an ordering or rebuild invariant inconsistent with this document, **this document is the intended current architecture** until the relevant focused specifications fold these corrections back into the master text.

This document addresses two review findings: retrieval authorization must occur before candidates can be bounded or sent to rerankers, and usage/feedback-derived projections require an authoritative event source for lossless rebuilds.

---

## 1. Authorization, Scope, and Validity Filtering Must Precede Bounding and Reranking

Permission, privacy, user/agent scope, project/repository scope, temporal validity, branch/worktree/commit validity, and other applicability constraints are **security and correctness gates**, not late ranking features.

The architecture must prevent unauthorized or inapplicable memory content from:

- entering a bounded candidate set where it can crowd out valid memories;
- being sent to an external cross-encoder, LLM reranker, embedding service, or other provider;
- propagating through downstream graph expansion or context compilation in a scope where it is not allowed;
- influencing ranking statistics or outputs for a principal that is not authorized to access it.

### Required retrieval ordering

Authorization and applicability constraints should be pushed down into each retrieval backend/query whenever the backend can enforce them. Regardless of pushdown support, **every candidate stream must pass a Security / Applicability Gate before fusion, candidate bounding, or reranking**.

```text
query + authenticated principal + active scopes
                ↓
Retrieval Planner / registered channels
                ↓
retrieval channels constrained by permissions/scope where possible
                ↓
per-stream Security / Applicability Gate
  ├── permissions / ACL / sharing policy
  ├── user and agent visibility
  ├── project/repository scope
  ├── session scope
  ├── valid-time / transaction-time applicability
  ├── branch / worktree / commit validity
  ├── retention / retraction / supersession eligibility
  ├── privacy / taint / redaction rules
  └── tool/provider disclosure policy
                ↓
per-channel normalization
                ↓
RRF / other fusion
                ↓
bounded authorized candidate set
                ↓
optional local or external reranker
                ↓
post-rank policy sanity check
                ↓
Context Compiler
```

The same rule applies to the **full-fusion/reference retrieval mode**: “all applicable channels” means all channels applicable **within the caller's authorized search universe**, not all memories in the underlying stores.

### External provider boundary

Before any candidate text, metadata, evidence, embedding input, graph neighborhood, or summary crosses a process/provider boundary, the system must verify that the current policy permits disclosure to that provider.

An external reranker must never receive content merely because it ranked highly before authorization filtering.

Possible provider policies include:

```text
local-only
approved-provider-list
redacted-only
metadata-only
no-private-memory
no-user-memory
no-secret-bearing-evidence
```

### Correctness consequence

Filtering only after candidate bounding is insufficient even when every reranker is local. Unauthorized or invalid high-ranked items can consume the bounded candidate budget and cause valid lower-ranked memories to disappear before the Context Compiler sees them.

Therefore authorization/applicability filtering is required **before bounding** as well as before external disclosure.

### Defense in depth

The Context Compiler and final action layer should still re-check scope/authority/privacy constraints. These downstream checks are defense in depth, not substitutes for the pre-fusion/pre-rerank gate.

---

## 2. An Authoritative Immutable Interaction / Audit Event Log Is a Rebuild Source

The architecture's rebuild invariant must include more than canonical Markdown and immutable evidence.

Several intentionally derived values depend on **runtime interactions**, not on the semantic content of memories themselves. Examples include:

- total access count;
- access frequency;
- retrieved-but-unused count;
- used-access count;
- percentage of retrievals used;
- retrieval co-use edges;
- empirical utility statistics;
- human positive/negative feedback;
- outcome attribution;
- learned-ranking features;
- decay/FSRS state that depends on access/use history;
- adaptive routing/ranking training examples;
- provenance-on-use records;
- agent-run/replay lineage.

If these events are kept only in disposable projection databases, deleting/rebuilding those indexes changes future ranking and decay behavior. That violates the intended rebuildability and replay guarantees.

### Corrected rebuild invariant

> **All rebuildable projections must be reconstructable from canonical Markdown, immutable evidence, and the authoritative append-only canonical event history required by those projections.**

The authoritative event history includes at least two related streams:

```text
Canonical semantic mutation history
  ├── memory create/update
  ├── supersede/retract
  ├── evidence attach/invalidate
  ├── relationship changes
  └── policy/ontology-relevant canonical changes

Canonical interaction / audit history
  ├── retrieval requested
  ├── candidate surfaced
  ├── candidate included in context
  ├── memory actually used
  ├── memory cited
  ├── feedback received
  ├── action/tool call influenced
  ├── outcome recorded
  ├── reminder/task event
  ├── agent message delivery/acknowledgment
  └── other durable signals required to rebuild derived behavior
```

These may be physically implemented as one event log or separate append-only logs, but their **authoritative status and retention semantics must be explicit**.

### Event-log requirements

Each durable interaction/audit event should carry enough information for deterministic or explainable replay, such as:

```text
event_id
event_type
event_schema_version
timestamp
principal / agent / user
session_id
project/repo scope
task/run ID
memory IDs
claim/evidence IDs where applicable
retrieval profile/version
ranking/reranker profile/version
context-compiler profile/version
result/use classification
feedback/outcome linkage
privacy classification
prior event / causal references where useful
```

The log itself is **canonical operational history**, not a search projection. Materialized counters, learned features, co-use graphs, decay values, and similar structures remain rebuildable projections derived from the log.

### Compaction without information loss

The event history may eventually be compacted for scale, but compaction must preserve enough authoritative state to reproduce the required derived behavior.

Accepted approaches may include:

- immutable raw events retained indefinitely;
- immutable raw events plus periodic checkpoints;
- cryptographically/hash-linked segments;
- versioned compacted snapshots containing provably sufficient statistics plus retained provenance pointers;
- tiered archival storage.

A compaction process must never silently make previously rebuildable projections unrebuildable.

### Reconciliation and FSCK

Projection health should therefore track both semantic and interaction-event progress, for example:

```text
Canonical Markdown revision:        M-18420
Canonical mutation event:           E-92114
Canonical interaction event:        I-440287

FTS projection mutation:            E-92114   ✓
Vector projection mutation:         E-92114   ✓
Graph projection mutation:          E-92112   ⚠ stale
Utility projection interaction:     I-440287  ✓
Retrieval co-use interaction:       I-440281  ⚠ stale
FSRS/decay interaction:              I-440287  ✓
```

`memory fsck` / reconciliation should detect:

- projection offsets ahead of canonical logs;
- missing event segments;
- duplicate/out-of-order interaction events where ordering matters;
- derived counters inconsistent with event replay;
- learned feature sets whose source-event range cannot be identified;
- replay records that reference missing event IDs.

---

## 3. Corrected Canonical vs Projection Boundary

The authoritative rebuild sources are therefore:

```text
Canonical Markdown
+ immutable Evidence
+ canonical semantic mutation history
+ canonical interaction/audit event history
```

Rebuildable projections include, among others:

```text
FTS / BM25
embeddings / vector indexes
entity indexes
temporal indexes
graph materializations
similarity and co-use edges
clusters
spreading activation / Hopfield state
salience
access/use counters
utility statistics
FSRS/decay scheduling state when derived from retained events
learned-ranking features/models where their training data/config are retained
retrieval success statistics
```

If a value matters to historical replay or future behavior but **cannot** be reconstructed from the retained authoritative sources, the information needed to reconstruct it must itself be promoted into canonical event history or an authoritative versioned snapshot.

---

## 4. Relationship to Existing Documents

These corrections specifically supersede the weaker interpretations currently visible in:

- `ARCHITECTURE_OVERVIEW.md` §2 invariant stating rebuild from only Markdown + evidence;
- `ARCHITECTURE_OVERVIEW.md` §18 diagram that places scope/authority/validity filtering after reranking;
- `ARCHITECTURE_OVERVIEW.md` §41 wherever rebuildable usage-derived state is listed without making its interaction-event source explicit;
- `ARCHITECTURE_COVERAGE_AUDIT_ADDITIONS.md` §2 retrieval diagrams wherever the pre-rerank Security / Applicability Gate is not drawn explicitly.

The focused specifications for projection synchronization, retrieval, governance, event journaling, privacy, replay, and evaluation should incorporate this ordering and rebuild model directly.
