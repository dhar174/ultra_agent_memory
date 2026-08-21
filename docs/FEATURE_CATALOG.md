# Ultra Agent Memory Feature Catalog

## 1. Canonical Memory & Storage

Everything that defines durable memory itself.

### Canonical storage

* Markdown as human-readable semantic source of truth
* YAML frontmatter
* Git-backed history
* Stable memory IDs
* Memory versioning
* Revision history
* Supersession without destructive overwrite
* Retraction
* Tombstones
* Archival
* Hard/soft deletion policies

### Canonical scopes

* Persona memory
* Session memory
* Project/repository memory
* User memory
* Universal memory
* Evidence store
* Canonical entity registry
* Canonical episode/event registry

### Memory granularity

* Memory
* Section
* Field
* Sentence
* Claim
* Evidence

### Canonical memory types

* Facts
* Observations
* Beliefs
* Mental models
* Episodes
* Temporal events
* Entities
* Patterns
* Preferences
* Requirements
* Directives
* Decisions
* Gotchas
* Commands
* Procedures
* Skills
* Attempts
* Failures
* Tool results
* Experiments
* Outcomes
* Rules
* Prompts
* Errors
* Tasks/TODOs
* Reminders
* Agent messages
* Learned conventions

---

# 2. Evidence, Provenance & Epistemics

Everything concerning **why the system believes something**.

### Evidence

* First-class immutable evidence objects
* Evidence source types

  * user statements
  * Git
  * documents
  * web sources
  * tool outputs
  * tests
  * external APIs
  * agent observations
  * human-entered evidence
* Evidence semantic roles

  * requirement evidence
  * decision evidence
  * correction evidence
  * constraint evidence
  * citation evidence
  * observation evidence
  * failure evidence
  * outcome evidence

### Provenance

* Memory → claim → evidence lineage
* Agent/tool/user attribution
* Capture timestamps
* Source hashes and versions
* Derivation history
* W3C PROV-inspired relationships
* Provenance-on-use

### Epistemic dimensions

* Epistemic confidence
* Source reliability
* Evidence strength
* Evidence coverage
* Contradiction score
* Consensus score
* Model confidence
* Stability
* Authority
* Importance
* Utility
* Novelty
* Salience

### Derived epistemic metrics

* Composite certainty score
* Explainable component contributions
* Versioned scoring formulas

### Epistemic lifecycle

* Candidate
* Proposed
* Accepted
* Contested
* Confirmed
* Superseded
* Retracted
* Expired
* Archived
* Tombstoned

---

# 3. Ontology & Semantic Organization

This is the system's conceptual skeleton.

### Memory ontology

* Types and subtypes
* Scope hierarchy
* Allowed parent/child relationships
* Allowed edge types
* Type-specific metadata
* Type-specific evidence requirements
* Type-specific authority semantics
* Type-specific retention/decay policies

### Ontology evolution

* Ontology induction
* Ontology proposals
* Ontology review
* Ontology versioning
* Ontology migration
* Reclassification
* Compatibility checking
* Reindex/recluster after ontology changes

### Specialist ontology agents

* OntologyInductionAgent
* OntologyReviewAgent
* OntologyMigrationAgent
* OntologyRoutingAgent
* OntologyCompatibilityAgent

### Distinct concepts

Especially preserve semantic differences such as:

* Fact vs Observation
* Observation vs Belief
* Belief vs Mental Model
* Procedure vs Skill
* Requirement vs Preference
* Directive vs inferred convention
* Evidence truth vs epistemic truth vs operational truth

---

# 4. Temporal & Historical Memory

Everything involving time and historical reconstruction.

### Bitemporal knowledge

* Valid time
* Transaction/record time
* `--as-of` queries
* Historical belief reconstruction

### Additional temporal semantics

* Observed time
* Asserted time
* Expected/future time
* Scheduled events
* Temporal confidence/probability

### Code-state validity

* Repository
* Branch
* Worktree
* Commit
* Commit validity ranges
* Git blob identity

### Temporal relationships

* Before
* After
* During
* Overlaps
* Begins
* Ends
* Temporal chains

---

# 5. Projection & Indexing System

Everything derived from canonical memory for computational use.

### Projection architecture

* Canonical mutation journal
* Projection queue
* Version tracking
* Projection freshness
* Reconciliation
* Disposable/rebuildable indexes

### Authoritative rebuild sources

* Canonical Markdown
* Immutable evidence
* Semantic mutation history
* Interaction/audit event history

### Relational metadata

* Memories
* Claims
* Fields
* Sentences
* Evidence links
* Entity links
* Temporal metadata
* Code anchors
* Task links

### Lexical indexes

* SQLite FTS5
* BM25
* Field weighting
* Memory/field/sentence/claim search

### Vector indexes

* Optional embeddings
* Local embeddings
* API embeddings
* Multiple embedding models
* Model/version/dimension tracking

### Entity indexes

* Canonical entities
* Aliases
* Mentions
* Coreference

### Temporal indexes

* Valid-time lookup
* Transaction-time lookup
* Event timelines

### Cognitive indexes

* FSRS state
* Decay
* Activation
* Salience
* Novelty
* Utility

---

# 6. Retrieval

Probably deserves one of the largest sections.

### Retrieval modes

* Full-fusion/reference retrieval
* Adaptive/routed retrieval
* Query decomposition
* Query expansion

### Retrieval channels

* BM25 / lexical
* Optional dense semantic
* LLM semantic expansion
* Entity adjacency
* Graph retrieval
* Temporal retrieval
* Causal retrieval
* Contradiction retrieval
* Supersession retrieval
* Failure/gotcha retrieval
* Requirement retrieval
* Authority retrieval
* Co-use retrieval
* Episode-sequence retrieval
* Hopfield retrieval
* Spreading activation
* Retroactive salience retrieval
* Same-file/symbol retrieval
* Same-session retrieval
* Same-project retrieval
* Same-time-period retrieval

### Retrieval security gate

Before fusion/reranking:

* Authentication
* Permissions
* ACLs
* Privacy
* User scope
* Agent scope
* Project scope
* Session scope
* Temporal applicability
* Branch/worktree/commit validity
* Retraction/supersession eligibility
* Provider disclosure policy

### Fusion

* Per-channel normalization
* Reciprocal Rank Fusion
* Weighted combinations
* Bounded candidate set

### Ranking/reranking

* Cross-encoder
* LLM pairwise
* LLM listwise
* Rule-based
* LightGBM / learning-to-rank
* Task-specific rankers
* Agent-specific rankers
* Project-specific rankers

### Ranking features

* Retrieval score
* RRF contribution
* Age/recency
* Age decay
* Access frequency
* Used-access frequency
* Retrieved-but-unused rate
* Utility
* Confidence/certainty
* Authority
* Evidence strength
* Scope match
* Branch validity
* Task similarity
* Outcome history

Including the explicit composite:

> **age decay + access frequency + confidence/utility**

### Retrieval explanation

* Why a memory was retrieved
* Per-channel ranks
* Fusion score
* Reranker score
* Boosts
* Penalties
* Final rank
* Whether retrieved/included/used/cited

---

# 7. Context Compilation

Separate retrieval from what the model actually receives.

### Context Compiler

* Token budgeting
* Scope precedence
* Authority resolution
* Deduplication
* Contradiction packaging
* Evidence/citation packaging
* Temporal filtering
* Privacy filtering
* Diversity
* Type quotas
* Compression
* Summary selection
* Hard requirement pinning
* Task continuity injection

### Context profiles

Different profiles for:

* coding agents
* review agents
* research agents
* user assistants
* small-context models
* large-context models

### Citation validation

* Only cite actually retrieved memories
* Verify evidence chains
* Record materially used memories
* Prevent unsupported provenance claims

---

# 8. Cognitive Memory

Everything that makes this more than a fancy database.

### Novelty and prediction error

* Exact duplicate detection
* Semantic duplicate detection
* Novelty scoring
* Prediction-error gating
* Contradiction detection
* Reinforcement of corroborated memories

### Memory strength

* FSRS-6-style scheduling
* Decay
* Retrieval strength
* Memory strength
* Dual-strength memory

### Associative cognition

* Spreading activation
* Synaptic tagging
* Hopfield-like associative retrieval
* Causal backtracking
* Retroactive salience backfill

### Consolidation

* Cluster formation
* Episode consolidation
* Pattern extraction
* Belief synthesis
* Mental-model updates
* Dream/offline consolidation

### Clustering

* KNN
* k-means
* HDBSCAN
* spectral clustering
* Gaussian mixtures
* hierarchical clustering

### Facts → beliefs → mental models

* Fact consolidation
* Observation extraction
* Generalized beliefs
* Standing-question mental models
* Proof/support counts
* Contradictory evidence tracking

### Per-type mental models

For example:

* Project.Requirements
* Project.Failures
* User.Preferences
* Persona.Self
* Universal.Tools

---

# 9. Memory Lifecycle & Governance

The rules controlling what gets to become memory.

### Admission

* Worth-remembering gate
* Evidence requirement
* Schema validation
* Scope assignment
* Type classification
* Secret/PII checking
* Privacy classification

### Write policy

* Off
* Observe
* Review
* Selective
* Auto-with-audit

### Review

* Human review
* Specialist review
* Evidence review
* Contradiction review
* Promotion review

### Scope movement

* Promotion
* Demotion
* Cross-scope learning
* Universal promotion
* Scope correction

### Conflict resolution

* Authority
* Specificity
* Explicitness
* Recency
* Validity
* Scope
* Confidence

### Invalidation propagation

Changed:

* evidence
* files
* APIs
* documentation
* tools

can invalidate:

* facts
* observations
* beliefs
* mental models
* procedures

---

# 10. Retention, Decay & Forgetting

I'd keep this separate from cognition because it is also governance.

### TTL policies

TTL is not merely a number. It may depend on:

* memory class
* scope
* creation time
* update time
* last-used time
* decay state
* FSRS state
* stability
* utility
* importance
* confidence
* retention policy
* supersession state
* active project/task status

### Possible retention results

* No expiry
* Review at date
* Archive at date
* Expire at date
* Retain until superseded
* Retain while project active
* Retain while evidence linked

### Memory-class behavior

* Hard requirement: effectively no decay
* Preference: slow decay
* Episode: dynamic decay
* Tool result: often short TTL
* Evidence: typically historical retention

---

# 11. Project & Code Intelligence

A major distinguishing feature for a coding-agent memory system.

### Repository memory

* Repo index
* Git history
* Status
* Commit notes
* Requirements
* Decisions
* Errors
* Deployment notes
* Rules
* Gotchas
* Commands
* Procedures
* Skills
* Preferences
* Project sessions

### Durable code anchors

* Repo ID
* Relative path
* Commit
* Blob hash
* Symbol
* Symbol kind
* AST path
* Line/byte ranges
* Text hash
* Context hashes

### Anchor relocation

* Exact blob
* Symbol
* AST
* Text hash
* Context hash
* Fuzzy relocation
* NeedsReview

### Git-aware invalidation

* Commit changes
* Branch changes
* File modifications
* Symbol modifications
* Dependency updates
* Merge/rebase events
* PR events

---

# 12. Agent Lifecycle Integration

How memory hooks into actual agent operation.

### Required hooks

* Session start
* User prompt
* Tool use
* Checkpoint
* Stop
* Precompact

### Coding hooks

* Git commit
* Checkout
* Branch creation
* Worktree changes
* Merge
* Rebase
* Pull
* PR creation/merge
* Test pass/fail
* Deployment pass/fail
* File modification
* Dependency change
* Tool schema change
* Model upgrade

### Session capture

* Transcript inspection
* Git diff inspection
* Worth-remembering gate
* Typed specialist extraction
* Evidence linking
* Dedup
* Contradiction detection
* Review queue
* Canonical write
* Audit events

---

# 13. Tasks, Checkpoints & Executive Memory

### Task continuity

* Current task
* Subtasks
* Completed steps
* Pending steps
* Blockers
* Resume instructions
* Relevant files/memories
* Expected next action

### Checkpoint/resume

* Durable checkpoints
* Branch/commit state
* Tool state
* Open files
* Commands
* Linked evidence

### TODO state machine

* Proposed
* Queued
* Assigned
* In progress
* Blocked
* Waiting
* Review
* Completed
* Cancelled

### Task metadata

* Assignee
* Lease
* Heartbeat
* Dependencies
* Priority
* Due date
* Completion evidence

### Formal invariant

If `task_id` or `task_during_memory` exists:

> the corresponding task/TODO/completed-task/tombstone must resolve.

---

# 14. Multi-Agent Coordination

### Agent messages

* Sender
* Receiver(s)
* Thread
* Priority
* Delivery condition
* Expiration
* Acknowledgement
* Linked task
* Evidence/memory links
* Idempotency

### Multi-agent task coordination

* Assignment
* Claiming
* Lease
* Release
* Reassignment
* Blocking dependencies
* Queue ordering

### Sharing

* Agent-private
* User-private
* Project
* Team
* Global
* Named allowlists

---

# 15. Specialist Agent System

This deserves its own feature family because it is such a major part of the design.

### Routing/supervision

* Memory router
* Scope router
* Type router
* Retrieval planner
* Review supervisor

### Capture specialists

* Fact extractor
* Observation extractor
* Preference extractor
* Requirement extractor
* Decision extractor
* Failure extractor
* Procedure extractor
* etc.

### Evaluation specialists

* Evidence judge
* Contradiction judge
* Novelty evaluator
* Promotion reviewer
* Privacy reviewer
* Authority resolver

### Retrieval specialists

* BM25 agent
* Dense retrieval agent
* Graph retrieval agent
* Temporal retrieval agent
* Causal retrieval agent
* Query-expansion agent

### Cognitive specialists

* Belief synthesizer
* Mental-model updater
* Consolidation agent
* Clustering agent
* Salience agent

### Typed instantiation

For example:

```text
ReviewAgent<Project.Decision>
CaptureAgent<Session.ToolResult>
EvidenceJudge<Project.Requirement>
MentalModelAgent<User.Preference>
```

---

# 16. Graph System

I’d list graph support by **semantic purpose**, not implementation technology.

### Core graphs

* Memory relation graph
* Entity graph
* Temporal graph
* Project graph

### Epistemic graphs

* Evidence/support graph
* Contradiction graph
* Belief lineage graph
* Supersession graph
* Provenance graph

### Project graphs

* Requirement → decision → code → test → deployment
* Error → cause → fix → regression test
* Repo → file → symbol → memory
* Git evolution graph

### Executive graphs

* Goal → plan → task → action → outcome
* Task dependency graph
* Agent communication graph
* Reminder/event/task graph

### Cognitive graphs

* Similarity graph
* Co-use graph
* Spreading-activation graph
* Cluster/topic graph
* Mental-model dependency graph

### Governance graphs

* Permission graph
* Privacy/taint graph
* Authority/override graph
* Scope inheritance graph

---

# 17. Audit, Replay & Operations

### Audit

* Every canonical mutation
* Every retrieval
* Every context inclusion
* Every material memory use
* Tool calls influenced by memory
* Decisions influenced by memory
* Feedback
* Outcomes

### Replay

* Model/version
* Prompts
* Context profile
* Retrieved memory IDs
* Scores
* Evidence
* Tool calls
* Results
* Historical memory state

### Interaction event log

Canonical operational events for:

* retrieval
* inclusion
* usage
* citation
* feedback
* outcomes
* tasks
* agent messages

### Rebuild

* Reindex
* Projection rebuild
* Event replay
* Full state reconstruction

### FSCK / reconciliation

* Broken memory IDs
* Missing evidence
* Broken anchors
* Dangling task IDs
* Projection drift
* Missing event ranges
* Invalid graph edges
* Stale indexes

---

# 18. Evaluation & Learning

### Retrieval evaluation

* Recall@K
* Precision@K
* MRR
* NDCG

### Memory evaluation

* Evidence coverage
* Citation correctness
* Contradiction accuracy
* Wrong-scope rate
* Stale-memory rate
* Privacy-leak rate
* Pollution rate

### Continuity evaluation

* Cross-session continuity
* Checkpoint/resume success
* Task completion improvement

### Historical replay

* As-of correctness
* Retrieval configuration comparisons
* Regression benchmarks

### Ranker training data

* Retrieved
* Included
* Used
* Cited
* Influenced decision
* Influenced tool call
* Good outcome
* Bad outcome
* Human feedback

---

# 19. Adaptive & Experimental Features

I'd intentionally keep these in a visibly separate **experimental** category.

### Adaptive retrieval

* Learned routing
* Learned ranking
* Project-specific retrieval profiles
* Agent-specific retrieval profiles

### Guarded self-improvement

* Soft-prompt adaptation
* Procedure evolution
* Skill evolution
* Ranking-policy evolution
* Routing-policy evolution

### Safeguards

* Candidate/proposed state
* Evaluation before adoption
* Versioning
* Rollback
* Quarantine
* Human/specialist review

No adaptive feature gets to rewrite immutable evidence.

---

# 20. Security & Privacy

This should be its own top-level feature group, even though it is cross-cutting.

### Access control

* Authentication
* ACLs
* Scope enforcement
* Role-based permissions
* User/agent/project isolation

### Privacy

* Privacy classification
* Taint propagation
* Visibility ceilings
* Derivation inheritance
* Sanitization/declassification

### Memory firewall

* Treat memory as untrusted input
* Prompt-injection detection
* Secret detection
* PII scanning
* Instruction/data separation

### External-provider protection

* Local-only policies
* Approved provider lists
* Metadata-only disclosure
* Redacted-only disclosure
* No-private-memory
* No-secret-bearing-evidence

---

# 21. API, MCP, CLI & UI

### Core memory operations

* remember
* retain
* recall
* buildContext
* feedback
* revise
* review
* forget
* export

### Extended operations

* search
* explainRecall
* promote/demote
* merge/split
* supersede/retract
* contest/confirm
* link/unlink
* add/invalidate evidence
* timeline/asOf
* graph
* checkpoint/resume
* assign/claim/release task
* message/ack agent
* remind/snooze
* validate/reconcile/reindex/rebuild
* audit/trace/replay

### Interfaces

* MCP
* HTTP API
* CLI
* Local browser UI

### Explorer UI

* Memory tree
* Timeline
* Graph views
* Evidence pane
* Retrieval explanation
* Review queues
* Task timeline
* Agent mailbox
* Projection health
* Memory-debt dashboard

---

# 22. Observability & Memory Health

### Health metrics

* Total memories
* Accepted
* Proposed
* NeedsReview
* Missing evidence
* Broken anchors
* Contradictions
* Stale project memory
* Stale indexes

### Memory debt

* Broken anchors
* Weak evidence
* Unreviewed promotions
* Unresolved contradictions
* Old mental models
* Stale procedures
* Orphan entities
* Projection drift
* Dead agent messages
* Overdue reminders/tasks

### Digests

* User/agent digest
* Project digest
* Memory health digest
* Contradiction digest
* Stale procedure digest
* Task continuity digest

---

## Then I’d add two orthogonal labels to every feature

This is the part that would make the list especially useful later.

Every feature could be tagged by **maturity**:

```text
CORE
REQUIRED
OPTIONAL
ADVANCED
EXPERIMENTAL
```

and by **architectural nature**:

```text
CANONICAL
AUTHORITATIVE EVENT
DERIVED
RUNTIME
POLICY
UI
```

So, for example:

| Feature                   | Domain            | Maturity     | Nature                      |
| ------------------------- | ----------------- | ------------ | --------------------------- |
| Markdown memory           | Canonical storage | CORE         | CANONICAL                   |
| FTS5/BM25                 | Retrieval         | REQUIRED     | DERIVED                     |
| Dense embeddings          | Retrieval         | OPTIONAL     | DERIVED                     |
| Interaction audit history | Operations        | REQUIRED     | AUTHORITATIVE EVENT         |
| FSRS                      | Cognition         | ADVANCED     | DERIVED                     |
| Hopfield retrieval        | Cognition         | OPTIONAL     | DERIVED                     |
| Soft-prompt adaptation    | Adaptive          | EXPERIMENTAL | DERIVED                     |
| Privacy gate              | Security          | REQUIRED     | POLICY/RUNTIME              |
| Mental models             | Cognition         | REQUIRED     | CANONICAL + DERIVED lineage |

That gives us **three ways to view the same design**:

1. **Feature catalog:** What capabilities exist?
2. **Implementation roadmap:** When do we build them?
3. **Architecture planes:** Where do they run/store state?

Those three views together would make the project documentation dramatically easier to navigate than one enormous flat checklist.

I think a `docs/FEATURE_CATALOG.md` organized roughly this way would be an excellent next artifact before we start exploding everything into dozens of focused specifications.
