# Ultra Agent Memory — Documentation and Diagram Roadmap

> **Purpose:** Define how the coverage-first master architecture in `ARCHITECTURE_OVERVIEW.md` should be decomposed into focused specifications, schemas, diagrams, flowgraphs, and implementation documents without losing concepts.

## 1. Documentation Philosophy

`ARCHITECTURE_OVERVIEW.md` is the broad source of truth for the current design space. It intentionally contains overlapping concepts and implementation candidates because the first goal is coverage.

Future documents should:

1. reference the master overview rather than silently replacing it;
2. formalize one concern at a time;
3. explicitly record design decisions and unresolved alternatives;
4. separate **requirements** from **candidate implementation technologies**;
5. include diagrams wherever relationships or workflows are easier to understand visually;
6. keep canonical-memory semantics separate from derived projection/index semantics;
7. preserve traceability from evidence → memory → claim → belief → mental model → retrieval → action;
8. be versioned as the ontology and implementation evolve.

---

## 2. Proposed Documentation Tree

```text
docs/
├── ARCHITECTURE_OVERVIEW.md               # coverage-first master source
├── DOCUMENTATION_ROADMAP.md                # this file
│
├── architecture/
│   ├── SYSTEM_ARCHITECTURE.md
│   ├── SEVEN_PLANES.md
│   ├── COMPONENT_BOUNDARIES.md
│   ├── DATA_FLOW.md
│   └── LOCAL_MULTI_AGENT_DEPLOYMENT.md
│
├── ontology/
│   ├── MEMORY_ONTOLOGY.md
│   ├── SCOPE_HIERARCHY.md
│   ├── MEMORY_TYPES.md
│   ├── CLAIMS_AND_EVIDENCE.md
│   ├── ENTITIES.md
│   ├── EPISODES_AND_TEMPORAL_EVENTS.md
│   ├── FACT_OBSERVATION_BELIEF_MODEL.md
│   └── MENTAL_MODELS.md
│
├── canonical/
│   ├── FILESYSTEM_LAYOUT.md
│   ├── MARKDOWN_FORMAT.md
│   ├── YAML_FRONTMATTER_SCHEMA.md
│   ├── FILE_AND_CODE_ANCHORS.md
│   ├── CANONICAL_VS_DERIVED.md
│   └── VERSIONING_AND_SUPERSESSION.md
│
├── evidence/
│   ├── EVIDENCE_MODEL.md
│   ├── PROVENANCE.md
│   ├── EVIDENCE_REQUIREMENTS_BY_TYPE.md
│   ├── CONFIDENCE_AUTHORITY_STABILITY.md
│   └── CONTRADICTION_AND_REVIEW.md
│
├── temporal/
│   ├── BITEMPORAL_MODEL.md
│   ├── CODE_STATE_VALIDITY.md
│   ├── AS_OF_QUERIES.md
│   └── TEMPORAL_RELATIONS.md
│
├── indexes/
│   ├── INDEX_ARCHITECTURE.md
│   ├── RELATIONAL_SCHEMA.md
│   ├── FTS_BM25.md
│   ├── VECTOR_INDEX.md
│   ├── ENTITY_INDEX.md
│   ├── TEMPORAL_INDEX.md
│   ├── ANCHOR_INDEX.md
│   ├── COGNITIVE_STATE_INDEX.md
│   └── PROJECTION_SYNCHRONIZATION.md
│
├── retrieval/
│   ├── RETRIEVAL_ARCHITECTURE.md
│   ├── QUERY_ROUTING_AND_EXPANSION.md
│   ├── RETRIEVAL_CHANNELS.md
│   ├── RRF_AND_RERANKING.md
│   ├── CONTEXT_COMPILER.md
│   ├── RETRIEVAL_EXPLANATIONS.md
│   └── RETRIEVAL_FEEDBACK_AND_LEARNING.md
│
├── graphs/
│   ├── GRAPH_CATALOG.md
│   ├── CORE_GRAPH_SCHEMA.md
│   ├── EVIDENCE_AND_BELIEF_GRAPHS.md
│   ├── PROJECT_AND_CODE_GRAPHS.md
│   ├── TASK_AND_AGENT_GRAPHS.md
│   ├── TEMPORAL_AND_CAUSAL_GRAPHS.md
│   └── DERIVED_ASSOCIATIVE_GRAPHS.md
│
├── cognition/
│   ├── MEMORY_LIFECYCLE.md
│   ├── NOVELTY_AND_PREDICTION_ERROR.md
│   ├── DECAY_AND_FSRS.md
│   ├── SYNAPTIC_TAGGING.md
│   ├── SPREADING_ACTIVATION.md
│   ├── HOPFIELD_RETRIEVAL.md
│   ├── RETROACTIVE_SALIENCE.md
│   ├── CONSOLIDATION.md
│   ├── CLUSTERING.md
│   └── BELIEF_AND_MENTAL_MODEL_UPDATES.md
│
├── agents/
│   ├── SPECIALIST_AGENT_ARCHITECTURE.md
│   ├── AGENT_ROLE_CATALOG.md
│   ├── ROUTERS_AND_SUPERVISORS.md
│   ├── AGENT_INPUT_OUTPUT_CONTRACTS.md
│   ├── WRITE_AUTHORITIES.md
│   └── MODEL_AND_PROMPT_VERSIONING.md
│
├── workflows/
│   ├── MEMORY_ADMISSION.md
│   ├── SESSION_START.md
│   ├── PROMPT_PROCESSING.md
│   ├── TOOL_USE_CAPTURE.md
│   ├── SESSION_STOP_AND_PRECOMPACT.md
│   ├── PROJECT_GIT_CHANGE_CAPTURE.md
│   ├── REVIEW_AND_CORRECTION.md
│   ├── PROMOTION_AND_DEMOTION.md
│   ├── SUPERSESSION_AND_RETRACTION.md
│   ├── CONSOLIDATION.md
│   └── RETRIEVAL_TO_CONTEXT.md
│
├── executive/
│   ├── CHECKPOINT_RESUME.md
│   ├── TASK_CONTINUITY.md
│   ├── TODO_QUEUES.md
│   ├── REMINDERS.md
│   ├── AGENT_MESSAGES.md
│   └── WEEKLY_DIGESTS.md
│
├── api/
│   ├── API_OVERVIEW.md
│   ├── MCP_TOOLS.md
│   ├── HTTP_API.md
│   ├── CLI.md
│   ├── MEMORY_OPERATIONS.md
│   └── ERROR_MODEL.md
│
├── ui/
│   ├── LOCAL_MEMORY_EXPLORER.md
│   ├── TIMELINE_VIEWS.md
│   ├── GRAPH_VIEWS.md
│   ├── REVIEW_QUEUES.md
│   └── HEALTH_DASHBOARD.md
│
├── governance/
│   ├── WRITE_POLICY_STATE_MACHINE.md
│   ├── SCOPE_AND_AUTHORITY_RESOLUTION.md
│   ├── PRIVACY_AND_PERMISSIONS.md
│   ├── TAINT_PROPAGATION.md
│   ├── RETENTION_AND_FORGETTING.md
│   └── AUDIT_AND_TRACEABILITY.md
│
├── operations/
│   ├── MUTATION_EVENT_JOURNAL.md
│   ├── RECONCILIATION.md
│   ├── MEMORY_FSCK.md
│   ├── REINDEX_AND_REBUILD.md
│   ├── BACKUP_AND_RESTORE.md
│   ├── MULTI_MACHINE_SYNC.md
│   ├── REPLAY.md
│   └── MEMORY_HEALTH_AND_DEBT.md
│
├── evaluation/
│   ├── EVALUATION_HARNESS.md
│   ├── RETRIEVAL_METRICS.md
│   ├── MEMORY_QUALITY_METRICS.md
│   ├── CONTINUITY_TESTS.md
│   ├── PRIVACY_AND_SCOPE_TESTS.md
│   └── HISTORICAL_REPLAY_BENCHMARKS.md
│
├── diagrams/
│   ├── level-0/
│   ├── level-1/
│   ├── level-2/
│   ├── workflows/
│   ├── data-model/
│   └── graphs/
│
└── implementation/
    ├── MVP_BOUNDARY.md
    ├── IMPLEMENTATION_PHASES.md
    ├── TECHNOLOGY_DECISIONS.md
    ├── MODULE_LAYOUT.md
    └── MIGRATIONS.md
```

This tree is a planning target, not a requirement to create every file immediately.

---

## 3. Diagram Levels

The architecture should be documented at several visual resolutions.

### Level 0 — One-picture mental model

Audience: anyone encountering the project for the first time.

Show only:

```text
Evidence / Markdown
        ↓
Indexes + Cognitive Enrichment
        ↓
Hybrid Retrieval
        ↓
Context Compiler
        ↓
Agent
        ↓
Outcomes / New Evidence
```

### Level 1 — Seven-plane architecture

Audience: contributors and architectural reviewers.

Show:
- Canonical Plane;
- Projection Plane;
- Cognitive Plane;
- Retrieval Plane;
- Context/Executive Plane;
- Governance Plane;
- Operations/Evaluation Plane;
- primary arrows/events between them.

### Level 2 — Major subsystem/component diagram

Show concrete subsystems such as:
- Markdown/Git canonical store;
- Evidence store;
- mutation journal;
- FTS projection;
- vector projection;
- graph projections;
- temporal/entity/anchor projections;
- specialist-agent runtime;
- retrieval orchestrator;
- Context Compiler;
- MCP/API/CLI;
- local web explorer;
- task/reminder/coordination system;
- health/reconciliation service.

### Level 3 — Workflow diagrams

One diagram per operation or event:
- remember/retain;
- recall/buildContext;
- session start;
- prompt;
- tool use;
- session stop/precompact;
- Git diff/commit event;
- review/correction;
- contradiction;
- supersession;
- promotion;
- mental-model update;
- consolidation;
- checkpoint/resume;
- reminder delivery;
- cross-agent message delivery;
- reindex/rebuild/reconcile.

### Level 4 — Data-model diagrams

ER/UML-like diagrams for:
- Memory → Section → Field → Sentence → Claim → Evidence;
- memory metadata;
- evidence/provenance;
- entities/aliases;
- temporal intervals;
- file/code anchors;
- tasks/TODOs/checkpoints;
- agent messages;
- projections and projection versions;
- audit events;
- model/prompt/tool version lineage.

### Level 5 — Graph-specific diagrams

Individual diagrams for:
- support/contradiction;
- supersession;
- belief lineage;
- requirement traceability;
- error causality;
- task timelines;
- agent communication;
- Git/code evolution;
- memory promotion;
- memory invalidation;
- privacy/taint propagation;
- retrieval co-use;
- mental-model dependency.

---

## 4. First Specification Wave

The first focused documents should answer the questions most likely to constrain everything else.

Recommended order:

### 1. Memory ontology and scope hierarchy

Define exact meanings and allowed relationships for:
- Persona;
- Session;
- Project;
- User;
- Universal;
- Evidence;
- Entity;
- Episode;
- Fact;
- Observation;
- Belief;
- Mental Model;
- Pattern;
- Preference;
- Requirement;
- Directive;
- Procedure;
- Skill;
- Decision;
- Gotcha;
- Attempt;
- Failure;
- Tool Result;
- Experiment;
- Outcome;
- Temporal Event;
- Task/TODO;
- Agent Message.

### 2. Common canonical schema

Formalize:
- stable IDs;
- YAML frontmatter;
- versioning;
- hashes;
- status/state;
- scope;
- time;
- provenance;
- evidence references;
- relationships;
- permissions;
- file/code anchors.

### 3. Claim/evidence/provenance model

This is foundational for confidence, contradiction, beliefs, mental models, citations, review, and supersession.

### 4. Canonical vs derived state

Produce a strict matrix declaring which fields belong in:
- Markdown;
- event journal;
- relational projection;
- FTS;
- vector projection;
- graph projection;
- cognitive-state projection;
- transient runtime only.

### 5. Projection synchronization

Specify the mutation journal, projection queues, version tracking, rebuild rules, reconciliation, and FSCK invariants.

### 6. Retrieval architecture

Formalize candidate generation, query expansion, channel routing, RRF, reranking, authority/scope filtering, and Context Compiler behavior.

### 7. Specialist-agent architecture

Define the typed role system and permissions before implementing many agents independently.

---

## 5. Memory-Type Specification Template

Every memory type/subtype should eventually have its own specification using the same template:

```text
Name
Canonical scope(s)
Parent/child ontology
Purpose
Examples
Non-examples
Canonical Markdown location
Required frontmatter
Optional frontmatter
Body/content format
Evidence requirement
Allowed evidence types
Authority semantics
Confidence semantics
Retention/TTL
Decay policy
Novelty/dedup policy
Contradiction policy
Supersession policy
Promotion/demotion policy
Allowed relationships
Required indexes
Optional indexes
Retrieval channels
Ranking boosts/penalties
Specialist agents
Admission workflow
Review workflow
Update workflow
Hooks that may create/update it
Privacy/sharing policy
As-of/temporal behavior
Branch/commit behavior
Mental-model relationship
Evaluation fixtures
```

This template should be used for every meaningful subtype, even if several types later share an implementation class.

---

## 6. Specialist-Agent Specification Template

Every logical specialist should eventually specify:

```text
Agent role name
Typed specialization
Purpose
Input schema
Output schema
Allowed tools
Allowed retrieval indexes
Allowed read scopes
Allowed write scopes
Allowed memory operations
Evidence requirements
Decision authority
Review authority
Upstream router/supervisor
Downstream agents
Model/provider profile
Prompt/version
Temperature/determinism policy
Timeout/retry policy
Failure behavior
Audit log requirements
Evaluation cases
```

This permits hundreds of logical specialists without requiring hundreds of unrelated code paths.

---

## 7. Workflow Specification Template

Each workflow should document:

```text
Trigger
Preconditions
Input state
Scope/identity resolution
Routers invoked
Specialist agents invoked
Deterministic transforms
Evidence collected
Indexes queried
Candidate memories
Write-policy gate
Review gate
Canonical writes
Mutation journal events
Projection updates
Audit events
Failure/retry states
Human-review path
Outputs
Postconditions
Metrics emitted
```

Every workflow should eventually have both a textual specification and a Mermaid/Graphviz/UML-style flowgraph.

---

## 8. Index / Retrieval-Channel Specification Template

Each index or retrieval mechanism should define:

```text
Name
Purpose
Canonical source data
Projection schema
Granularity: memory/field/sentence/claim
Scope coverage
Memory-type coverage
Build process
Incremental update process
Rebuild process
Query inputs
Output score/rank
Score calibration
Filtering semantics
Freshness/version tracking
Failure/fallback behavior
Latency target
Evaluation metrics
Explainability fields
```

Specific specs will be needed for:
- BM25/FTS5;
- dense vectors;
- LLM expansion;
- temporal;
- entity adjacency;
- graph;
- causal;
- contradiction;
- Hopfield;
- spreading activation;
- retroactive salience;
- co-use;
- same-file/symbol;
- requirement/authority channels.

---

## 9. Graph Specification Template

Every graph projection should define:

```text
Graph name
Purpose
Node types
Edge types
Edge direction
Edge provenance
Canonical vs derived edges
Scope partitioning
Temporal semantics
Weight semantics
Update triggers
Traversal operations
Retrieval use
Visualization use
Privacy behavior
Invalidation behavior
Evaluation/tests
```

---

## 10. API / Memory Operation Specification Template

Operations such as `remember`, `retain`, `recall`, `review`, `supersede`, `checkpoint`, and `messageAgent` should each define:

```text
Operation name
Intent
Request schema
Response schema
Authentication/identity
Scope semantics
Permissions
Synchronous actions
Queued/background-like internal stages
Canonical side effects
Projection side effects
Audit events
Idempotency
Error cases
Retry semantics
Examples
```

The public API should remain smaller and more stable than the internal number of specialist operations.

---

## 11. Suggested Diagram/Flowgraph Backlog

High-value first diagrams:

1. Level-0 Ultra Agent Memory mental model;
2. seven-plane system architecture;
3. canonical folder hierarchy / memory ontology tree;
4. Memory → Section → Field → Sentence → Claim → Evidence ER diagram;
5. evidence → fact → observation → belief → mental-model lineage;
6. canonical write → mutation journal → projections workflow;
7. session-stop/precompact capture workflow;
8. hybrid retrieval + RRF + reranking + Context Compiler workflow;
9. scope/authority conflict-resolution flow;
10. supersession/contradiction state machine;
11. scope promotion/demotion flow;
12. project Git diff/file-anchor update flow;
13. checkpoint/resume/task-continuity flow;
14. multi-agent message/TODO assignment flow;
15. memory invalidation cascade;
16. memory health/reconciliation flow;
17. retrieval explanation example;
18. support/contradiction/provenance graph;
19. requirement → decision → code → test traceability graph;
20. branch/worktree/commit-aware memory validity diagram.

---

## 12. Implementation Phases to Document Later

A likely implementation decomposition, subject to later design review:

### Phase 0 — Formal model
- ontology;
- schemas;
- IDs;
- canonical/derived boundary;
- evidence model;
- event journal.

### Phase 1 — Canonical core
- Markdown/Git storage;
- CRUD/revision/supersession;
- evidence references;
- frontmatter validation;
- basic local API/CLI;
- SQLite relational metadata.

### Phase 2 — Basic hybrid retrieval
- FTS5/BM25;
- memory/field/sentence/claim indexing;
- optional embeddings;
- entity adjacency;
- RRF;
- explainRecall;
- basic Context Compiler.

### Phase 3 — Agent lifecycle integration
- MCP;
- session/prompt/tool/precompact/stop hooks;
- Git-diff capture;
- checkpoint/resume;
- task/TODO continuity.

### Phase 4 — Evidence/governance
- review state machine;
- correction;
- provenance;
- contradiction;
- scope/authority resolution;
- privacy/permissions;
- promotion/demotion.

### Phase 5 — Graph and temporal cognition
- bitemporal queries;
- graph projections;
- causal/contradiction retrieval;
- file/symbol anchors;
- branch/commit validity.

### Phase 6 — Cognitive enrichment
- FSRS/decay;
- novelty/prediction-error gating;
- spreading activation;
- retroactive salience;
- clustering;
- belief consolidation;
- mental models;
- optional Hopfield mechanisms.

### Phase 7 — Multi-agent and operations
- agent messages;
- TODO assignment/queues;
- multi-machine sync/mesh;
- replay;
- health/debt dashboard;
- evaluation harness;
- learned ranking.

These phases are not yet commitments; the implementation plan should be derived after the formal ontology/schema work.

---

## 13. Source-of-Truth Rule for Future Docs

When a focused document makes a concrete decision that narrows or changes an idea in `ARCHITECTURE_OVERVIEW.md`:

1. record the decision in the focused document;
2. link it back to the relevant overview section;
3. update the overview to indicate the concept is now specified elsewhere;
4. never silently delete a previously captured requirement/idea;
5. if rejected, preserve it as a rejected/superseded design option with rationale.

This mirrors the memory system's own philosophy: **refine and supersede interpretations, do not erase the historical evidence of how the architecture evolved.**
