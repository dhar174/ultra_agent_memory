# Ultra Agent Memory — Master Architecture Overview

> **Status:** Living concept/specification document. Coverage-first, not implementation-final.
>
> **Purpose:** Capture the complete current design space for `ultra_agent_memory` in one source of truth before decomposing it into narrower architecture, ontology, schema, workflow, retrieval, graph, subagent, API, and operations specifications.

## 1. Vision

Ultra Agent Memory is intended to be a local-first, inspectable, evidence-grounded, multi-agent memory operating system for coding agents and general autonomous agents. It combines the strongest ideas from Markdown/Git canonical-memory systems, evidence/provenance systems, hybrid retrieval engines, graph memory, temporal/bitemporal knowledge, cognitive memory models, coding-agent lifecycle hooks, task continuity, and multi-agent coordination.

The system should preserve **human-readable canonical memory in Markdown**, while maintaining one or more rebuildable metadata/index projections for fast and specialized retrieval. It should support facts, observations, beliefs, mental models, episodic memory, procedures, project state, user preferences, agent persona, temporal events, tasks, reminders, evidence, causal relations, and cross-agent messages while retaining enough provenance to answer: **what does the system believe, why does it believe it, what evidence supports it, when was it true, who/what produced it, and what changed?**

The design intentionally does **not** assume that one database, one embedding space, one graph, one retrieval algorithm, one LLM, or one memory representation is sufficient. Retrieval, storage, review, consolidation, and governance should be mixtures of specialized mechanisms.

---

## 2. Cardinal Principles and Invariants

1. **Canonical Markdown is the semantic source of truth.** Databases are indexes/projections, not the authoritative memory body.
2. **Immutable evidence is never rewritten by higher-order cognition.** Synthesized interpretations may annotate, contest, supersede, or replace prior interpretations, but they may never rewrite the underlying evidence that produced them.
3. **All index databases must be rebuildable from canonical Markdown plus immutable evidence.**
4. **Derived computational state is distinct from semantic truth.** Retrieval counts, embedding vectors, decay values, activation, cluster membership, rank scores, and similar values belong primarily in projection databases, not in canonical Markdown.
5. **Evidence is required for saving whenever the memory type permits evidence.** A memory without sufficient evidence may still exist when policy allows, but must be explicitly marked `IsEvidenced: false` / `needs_evidence` / equivalent and must not masquerade as an evidenced fact.
6. **Every answer, action, recommendation, or synthesized belief should be capable of citing the memory IDs it relied on and the evidence attached to those memories.**
7. **Supersession is preferred to destructive replacement.** Old beliefs, decisions, interpretations, requirements, and state transitions remain historically inspectable.
8. **Bitemporal knowledge is first-class.** The system distinguishes when something was true from when the system learned/recorded it and supports `--as-of`/historical-state queries.
9. **Project/code reality may add a third validity dimension.** A memory can be valid only on a given branch, worktree, commit range, or repository state.
10. **Line numbers are convenience locators, not durable anchors.** Persistent anchors should combine repo/file identity, commit/blob hashes, symbols/AST paths, hashes, context, and relocatable text anchors.
11. **Memory operations are governed by typed write policies and review gates.** Different memory types have different evidence thresholds, review requirements, TTLs, decay rules, and promotion permissions.
12. **The system distinguishes evidence truth, epistemic truth, and operational truth.** A source saying X, the system currently believing X, and the agent currently being required to do Y are not the same thing.
13. **Authority, confidence, importance, stability, salience, and retrieval strength are separate dimensions.**
14. **Specialist subagents are highly granular and typed.** Semantic judgments should be delegated to narrowly scoped specialists with explicit schemas, tools, permissions, and review rules.
15. **Deterministic code should be used where semantic judgment is unnecessary.** LLMs should not be used merely to increment counters, hash files, or copy known metadata.
16. **Retrieval is a mixture of specialized channels, not one universal similarity function.**
17. **Retrieval should be explainable.** The system should be able to report why a memory was retrieved, by which channels, with which boosts/penalties, and whether it was ultimately used.
18. **Privacy/scope taint propagates through derivation.** A mental model derived from private memory does not become public merely because the source text was abstracted away.
19. **Cross-scope duplication should be avoided.** Aggregated entity/event/session collections should generally be manifests/references rather than second canonical copies.
20. **The system must be self-auditing.** Projection drift, broken anchors, unresolved contradictions, missing evidence, stale procedures, and unreviewed memory debt should be visible and repairable.

---

## 3. High-Level Architecture: Seven Planes

```text
┌─────────────────────────────────────────────────────┐
│ 1. CANONICAL PLANE                                  │
│ Markdown + Git + immutable Evidence + stable IDs    │
└──────────────────────────┬──────────────────────────┘
                           │ canonical mutation events
┌──────────────────────────▼──────────────────────────┐
│ 2. PROJECTION / INDEX PLANE                         │
│ FTS • vectors • temporal • claims • graphs • edges │
│ field/sentence indexes • entity indexes • anchors   │
└──────────────────────────┬──────────────────────────┘
                           │
┌──────────────────────────▼──────────────────────────┐
│ 3. COGNITIVE / ENRICHMENT PLANE                     │
│ novelty • beliefs • mental models • decay • FSRS    │
│ activation • salience • consolidation • clustering  │
└──────────────────────────┬──────────────────────────┘
                           │
┌──────────────────────────▼──────────────────────────┐
│ 4. RETRIEVAL PLANE                                 │
│ lexical • dense • graph • temporal • causal         │
│ Hopfield • spreading activation • contradiction     │
│ query expansion • RRF • rerankers                   │
└──────────────────────────┬──────────────────────────┘
                           │
┌──────────────────────────▼──────────────────────────┐
│ 5. CONTEXT / EXECUTIVE PLANE                        │
│ Context Compiler • tasks • TODO queues • reminders  │
│ checkpoints • resume • coordination • agent mail    │
└──────────────────────────┬──────────────────────────┘
                           │
┌──────────────────────────▼──────────────────────────┐
│ 6. GOVERNANCE / EPISTEMIC SAFETY PLANE              │
│ evidence • provenance • authority • privacy         │
│ review • correction • write policy • supersession   │
└──────────────────────────┬──────────────────────────┘
                           │
┌──────────────────────────▼──────────────────────────┐
│ 7. OPERATIONS / EVALUATION PLANE                    │
│ fsck • replay • audit • health • metrics • backup   │
│ projection reconciliation • evals • digests • mesh  │
└─────────────────────────────────────────────────────┘
```

---

## 4. Canonical Markdown Memory Hierarchy

The canonical memory root is organized by **scope first**, with typed memory documents or subfolders beneath each scope.

```text
memory/
├── Persona/
├── Sessions/
├── Projects/
├── User/
├── Universal/
├── Evidence/
├── CanonicalEntities/                 # preferably manifests/registry
├── CanonicalEpisodesAndEvents/        # preferably manifests/registry
└── ... optional schemas/templates/index manifests
```

Associative computational state should generally be stored as database projections. Canonical relationship declarations such as `related-to`, `blocked-by`, `supports`, `contradicts`, `caused-by`, and `supersedes` may be written into canonical memory; spreading activation, KNN neighbors, current salience, Hopfield state, PageRank, and similar relationships are derived projections.

### 4.1 Persona

`Persona/` contains memories about the agent itself and experiences framed around the agent.

Suggested structure:

```text
Persona/
├── Self/
│   ├── Facts/
│   ├── Observations/
│   ├── LongtermBeliefs/
│   └── LongtermGoals/
├── Episodes/
├── Observations/
├── GeneralizedBeliefs/
├── TemporalEvents/
├── Entities/
├── Patterns/
├── Preferences/
├── MentalModels/
└── LearnedConventions/
```

#### Persona → Self

Includes:
- self-referential facts;
- specific observations about the agent;
- long-term generalized belief statements about the agent;
- long-term goals and goal-related memories.

#### Persona → Episodes

Episodic memories involving, about, and framed around the agent, including:
- facts about an event/episode;
- specific observations about it;
- general beliefs about it;
- episode scenes;
- related entities and temporal events as appropriate.

#### Persona → General memory classes

Also includes agent-unique information that belongs nowhere more specific:
- observations;
- generalized beliefs;
- temporal events, both past and expected/scheduled future events;
- entities;
- patterns;
- preferences;
- mental models;
- learned conventions.

### 4.2 Sessions

`Sessions/` contains one subfolder per session, named using the session-start datetime and optionally a short slug/ID.

```text
Sessions/
└── 2026-08-21T04-00-00_<session-id>/
    ├── Summary.md
    ├── Facts.md
    ├── Observations.md
    ├── ProposedLearnings.md
    ├── Evidence.md            # references/manifests to Evidence IDs
    ├── WorkingMemory.md
    ├── TemporalEvents.md
    ├── Attempts.md
    ├── Failures.md
    ├── ToolResults.md
    ├── Experiments.md
    ├── Outcomes.md
    ├── Patterns.md
    ├── Preferences.md
    ├── MentalModels.md
    └── LearnedConventions.md
```

These may be separate files or sections within one file depending on density. The ontology must not depend on the physical split.

Session-scoped memory types include:
1. session summary;
2. facts about the session;
3. observations about the session;
4. proposed learnings / beliefs potentially worth promoting;
5. evidence citations linked to the session;
6. short-term working memory;
7. temporal events, past and future/scheduled;
8. attempts;
9. failures;
10. tool results;
11. experiments;
12. outcomes;
13. patterns;
14. preferences;
15. mental models;
16. learned conventions.

Session memory is the major staging area from which higher-scope memories may later be promoted through review gates.

### 4.3 Projects

`Projects/` contains one folder per repository or project.

```text
Projects/
└── <project-or-repo>/
    ├── RepoIndex.md
    ├── GitHistory.md
    ├── Status.md
    ├── CommitNotes/
    ├── RepoNotes/
    ├── Requirements/
    ├── Errors/
    ├── DeploymentNotes/
    ├── Documentation/
    ├── Rules/
    ├── Todo/
    ├── TaskHistory/
    ├── SessionHistory.md
    ├── Decisions/
    ├── Gotchas/
    ├── Commands/
    ├── Procedures/
    ├── Skills/
    ├── Preferences/
    ├── TemporalEvents/
    ├── Entities/
    ├── Attempts/
    ├── Failures/
    ├── ToolResults/
    ├── Experiments/
    ├── Outcomes/
    ├── Patterns/
    ├── Observations/
    ├── MentalModels/
    └── LearnedConventions/
```

Project/repo memory includes:
1. **Repo Index:** index of repository files and optionally symbol/semantic indexes;
2. **Git History:** raw commit history or references to immutable Git evidence;
3. **Status:** where the project stands / where work stopped;
4. **Commit Notes:** scoped and linked to specific commits;
5. **Repo Notes:** general project/repository notes;
6. **Requirements:** hard and soft requirements with provenance and validity;
7. **Errors:** errors, symptoms, causes, fixes, recurrence history;
8. **Deployment Notes**;
9. **Documentation Notes and Indices**;
10. **Repo/Project Rules**;
11. **Project TODO:** unfinished work, superseded/completed rather than deleted;
12. **Project Task History:** agent-authored task history;
13. **Project Session History:** references to `Sessions/` entries scoped to the repo; this should usually be a manifest rather than duplicate session text;
14. **Decisions:** architecture/implementation choice plus rationale;
15. **Gotchas:** trap + cause + fix, e.g. “X breaks because Y, do Z”;
16. **Commands:** commands likely to otherwise be repeatedly looked up;
17. **Procedural Memory / Runbooks:** reusable multi-step workflows scoped to the repo, including:
   - successful workflows;
   - known failure patterns;
   - checkpoint/resume state;
   - tool policies;
   - task strategies;
18. **Skills:** skills learned in repo context;
19. **Preferences:** user style, conventions, and corrections relevant to the project;
20. **Temporal Events:** past and expected/scheduled future events;
21. **Entities**;
22. **Sessions / links to sessions**;
23. **Attempts**;
24. **Failures**;
25. **Tool Results**;
26. **Experiments**;
27. **Outcomes**;
28. **Patterns**;
29. **Preferences**;
30. **Observations**;
31. **Mental Models**;
32. **Learned Conventions**.

Project memory should additionally understand Git branches, worktrees, commits, PRs, tests, deployments, and file/symbol evolution.

### 4.4 User

`User/` contains one subfolder per known user.

Each user folder begins with a short, 3–4 sentence maximum summary, followed by typed memory files/folders:

```text
User/
└── <user-id-or-name>/
    ├── Summary.md
    ├── Facts/
    ├── Preferences/
    ├── Requirements/
    ├── Directives/
    ├── Episodes/
    ├── Observations/
    ├── LongtermBeliefs/
    ├── TemporalEvents/
    ├── Entities/
    ├── Patterns/
    ├── MentalModels/
    └── LearnedConventions/
```

User memory includes:
1. facts about the user;
2. user preferences;
3. user requirements;
4. user directives;
5. events/episodes involving, about, and framed around the user;
6. observations about the user;
7. long-term belief statements / generalized beliefs about the user;
8. temporal events, past and expected/scheduled future;
9. entities;
10. patterns;
11. mental models;
12. learned conventions.

### 4.5 Universal

`Universal/` contains memories applicable across projects, users, sessions, and agents unless superseded by more authoritative/specific instructions.

```text
Universal/
├── Rules/
├── Procedures/
├── Skills/
├── Notes/
├── Observations/
├── Facts/
├── LongtermBeliefs/
├── Episodes/
├── TemporalEvents/
├── Entities/
├── Tools/
├── Requirements/
├── Prompts/
├── Errors/
├── Patterns/
├── Preferences/
├── MentalModels/
└── LearnedConventions/
```

Universal memory includes:
1. rules;
2. procedures and skills, including successful workflows, known failure patterns, checkpoint/resume state, tool policies, and task strategies;
3. cross-session, cross-agent, cross-project notes;
4. observations;
5. universal facts;
6. long-term universal belief statements;
7. universal episodic memory for events external to any specific agent/user/project/session, including attempts, failures, tool results, experiments, outcomes, and agent runs where applicable;
8. temporal events;
9. entities;
10. tool-use memories;
11. universal requirements;
12. prompts;
13. errors;
14. patterns;
15. preferences;
16. observations;
17. mental models;
18. learned conventions.

### 4.6 Canonical Entities

A global canonical entity registry should unify aliases and cross-scope references without duplicating source memories. It should preferably function as a manifest/registry:

```text
CanonicalEntities/
├── entities.md / entities/*.md
└── aliases.md
```

An entity should have a stable `entity_id`, aliases, type, provenance, and references into Persona/User/Project/Session/Universal memories.

### 4.7 Canonical Episodic Memories and Temporal Events

A cross-scope registry can collect/reference episodes and temporal events from all scopes. This should normally be a **reference manifest**, not a duplicate canonical copy.

### 4.8 Associative Memory

Associative memory spans scopes and types and includes:
- causal links;
- temporal chains;
- entity relationships;
- spreading activation;
- retroactive salience;
- co-retrieval/co-use links;
- automatically inferred similarity edges;
- support/contradiction relationships;
- dependency relations.

Canonical relationship assertions may be stored in Markdown, while activation strengths, KNN neighborhoods, clusters, and other dynamic graph state remain derived database projections.

---

## 5. Core Data Model and Granularity

Every canonical memory gets a stable `memory_id` used as the primary key across indexes and graphs.

The index granularity should be:

```text
Memory
  ↓
Section
  ↓
Field
  ↓
Sentence
  ↓
Claim
  ↓
Evidence
```

The original requirement for **memory, field, and sentence granularity** remains mandatory. `Section` and `Claim` add useful intermediate/finer levels.

### Why add claims?

A single sentence can contain multiple independently supportable propositions. Claims allow evidence, contradiction, certainty, validity windows, and supersession to operate at the actual proposition level rather than only at whole-memory granularity.

Each claim can carry:
- `claim_id`;
- memory/field/sentence parent IDs;
- claim type;
- supporting evidence IDs;
- contradicting evidence IDs;
- supporting/contradicting claim IDs;
- epistemic confidence;
- evidence coverage;
- source reliability aggregation;
- valid-time interval;
- transaction-time interval;
- supersession/retraction state;
- provenance/derivation lineage.

---

## 6. Canonical YAML Frontmatter

Canonical Markdown should use YAML frontmatter for semantic identity, lineage, scope, relationships, and policy-relevant metadata.

Candidate canonical fields include:

```yaml
id: <stable-memory-id>
schema_version: 1
version: 1
memory_type: fact
memory_subtype: null
scope: project
agent_id: null
user_id: null
project_id: null
repo: null
branch: null
worktree: null
session_ids: []

created_at: null
updated_at: null
valid_from: null
valid_to: null
recorded_from: null
recorded_to: null
valid_commit_from: null
valid_commit_to: null

status: proposed
entered_by_human: false
is_evidenced: true
needs_review: false

summary: null
tags: []
keyword_tags: []
notes: null
last_change_summary: null

evidence_ids: []
provenance: []
related_to: []
blocked_by: []
dependencies: []
supersedes: []
superseded_by: []

entity_ids: []
related_decisions: []
related_gotchas: []
related_preferences: []
related_tools: []
related_docs: []
related_todos: []
related_finished_tasks: []
related_observations: []
related_facts: []
related_beliefs: []
related_skills_procedures: []
related_episodes: []
related_commands: []
linked_mental_models: []

linked_files: []
file_anchors: []

permissions: null
shared: personal
retention_class: null

has_reminder: false
reminder_time: null
```

### Canonical vs derived metadata

Not every desired field belongs in frontmatter. The following should usually remain **derived projection state** to avoid rewriting canonical Markdown whenever the system retrieves or reindexes a memory:

- embeddings / embedding vectors / embedding model pointers;
- total access count;
- access frequency;
- percentage access used;
- total unused retrieval count;
- total used retrieval count;
- retrieval-channel scores;
- current decay score/value;
- FSRS scheduling state;
- `lasting_power` calculation components;
- novelty score;
- current salience;
- spreading activation;
- Hopfield state;
- KNN edges;
- k-means/HDBSCAN/spectral/GMM/hierarchical cluster assignments;
- similarity edges generated from current indexes;
- PageRank/centrality;
- reranker scores;
- learned-ranking features;
- projection freshness/version state.

A selected snapshot of derived values may optionally be materialized into Markdown for auditing, but the computational database state remains authoritative for those dynamic measures.

---

## 7. Complete Metadata / Index Field Families

All requested metadata should be represented somewhere across canonical frontmatter and projection tables. Required families include:

### 7.1 Identity and location
- memory ID;
- memory filepath;
- memory type and subtype;
- main agent name/ID;
- user scope;
- repo/project scope;
- session links;
- scope;
- schema version;
- memory version;
- embedding or embedding filepath/pointer where applicable;
- fields, sentences, sections, claims.

### 7.2 Time and history
- created timestamp;
- last updated timestamp;
- valid-time start/end;
- transaction/recorded-time start/end;
- branch/commit validity;
- last change summary;
- TTL/retention class;
- temporal-event links;
- supersession chain.

### 7.3 Semantic description
- one-sentence memory summary describing nature/purpose/broad context without unnecessarily duplicating content;
- notes about memory;
- tags;
- model-entered keyword tags plus keywords derived from memory text;
- proposed changes/additions;
- task active during memory creation where relevant.

### 7.4 Evidence and epistemic metadata
- linked evidence IDs;
- `IsEvidenced`;
- provenance;
- epistemic confidence;
- source reliability;
- evidence strength;
- evidence coverage;
- contradiction score;
- consensus score;
- model confidence;
- stability;
- authority;
- entered-by-human flag;
- review state;
- `NeedsReview`;
- certainty/evidence aggregation score.

### 7.5 Usage and cognitive state
- novelty score;
- total access count;
- access frequency;
- percentage of retrievals actually used;
- total unused retrieval count;
- total used retrieval count;
- decay value;
- FSRS state;
- lasting power;
- salience;
- importance;
- utility;
- retrieval strength;
- memory strength;
- dual-strength state;
- synaptic-tag state;
- retroactive-salience adjustments.

`lasting_power` may combine model estimates of likelihood-to-change and future usefulness with evidence quantity/quality, linked-file count, actual historical utility, authority, stability, and certainty.

### 7.6 Links and edges
- explicit manually entered related memories;
- automatically linked related memories;
- `related-to`;
- `blocked-by`;
- dependencies;
- cause/effect edges;
- similarity edges;
- related decisions;
- related gotchas;
- related preferences;
- related tools;
- related documents/files;
- related TODOs;
- related completed tasks;
- related observations;
- related facts;
- related beliefs;
- related skills/procedures;
- related episodic memories;
- related commands/code;
- linked mental models;
- entity relationships;
- session relationships.

### 7.7 Similarity edge families
Maintain separate edge sets above configurable thresholds for similarity/commonality of:
- linked files;
- cited evidence;
- tags;
- summary;
- sentence embeddings/text;
- claims;
- common fields/values;
- same memory type;
- same project/repo;
- same/near time period by update time;
- same/near time period by creation time;
- same session;
- co-retrieval/co-use;
- semantic embedding similarity;
- lexical overlap;
- shared entities.

### 7.8 Reminders, tasks, and permissions
- `hasReminder`;
- reminder time;
- task during memory;
- dependencies;
- queue position/state;
- assigned agent;
- receiver/delivery condition for agent-message memories;
- permissions;
- sharing scope (`personal`, `repo`, `global`, `user`, `agent`, or named allowlists);
- retention policy.

---

## 8. Durable File and Code Anchors

A memory may point to exact line ranges for human convenience, but stable anchors should be compound and relocatable.

Candidate anchor model:

```text
anchor_id
repo_id
relative_path
git_commit
git_blob_hash
branch/worktree

line_start
line_end
byte_start
byte_end

symbol_name
symbol_kind
AST_path

anchor_text_hash
context_before_hash
context_after_hash
```

Relocation strategy:

```text
exact commit/blob
      ↓
exact symbol/AST path
      ↓
structural syntax match
      ↓
anchor-text hash
      ↓
surrounding-context hash
      ↓
fuzzy relocation
      ↓
NeedsReview
```

For source code, a parser such as **Tree-sitter** is a candidate technology for symbol/AST-aware anchors. Git blob hashes and commit IDs provide immutable historical anchoring.

File drift checks should flag memories whose linked source changed materially.

---

## 9. Evidence as a First-Class Canonical Object

Add a canonical `Evidence/` hierarchy rather than treating evidence only as metadata attached to memory:

```text
Evidence/
├── UserStatements/
├── ToolResults/
├── Git/
├── Documents/
├── Web/
├── Tests/
├── ExternalAPIs/
├── AgentObservations/
└── HumanEntered/
```

Each evidence object should have a stable `evidence_id` and fields such as:

```yaml
evidence_id: E...
source_type: git_commit
source_uri: null
source_hash: null
source_version: null
captured_at: null
observed_at: null
captured_by: null
original_agent: null
trust_class: null
immutable: true
```

Evidence relations include:
- supports claim;
- contradicts claim;
- generated observation;
- derived from source;
- used by agent/process;
- attributed to agent/user/tool;
- primary source / secondary source;
- invalidated or unavailable.

W3C PROV concepts such as entities, activities, agents, `wasGeneratedBy`, `wasDerivedFrom`, `used`, and `wasAttributedTo` are useful vocabulary candidates even if the implementation is not RDF.

Every memory type should define its own **evidence admission bar**. Example: a hard project requirement may require direct user/document/contract evidence; an agent observation may be admitted with lower evidence but marked as observational and lower authority.

---

## 10. Three Kinds of Truth

The system formally distinguishes:

### Evidence truth
“Source S asserted/contained X.”

Immutable once captured, except for metadata corrections about the capture itself.

### Epistemic truth
“Given the evidence currently available, the system believes X.”

Mutable, contestable, supersedable, confidence-bearing.

### Operational truth
“Given current scope, authority, requirements, task state, and policy, the agent should do Y now.”

Operational truth may legitimately differ from a generalized user preference or prior belief.

Example:

```text
Evidence:     User once said “always use SQLite.”
Belief:       User generally prefers SQLite.
Operational:  Current project requirement mandates PostgreSQL.
              Use PostgreSQL here.
```

---

## 11. Epistemic State Machine

Memories/claims should support explicit states such as:

```text
candidate
proposed
accepted
contested
confirmed
superseded
retracted
expired
archived
tombstoned
```

Example promotion/review flow:

```text
LLM inference
    ↓
candidate
    ↓
Evidence Judge
    ↓
proposed
    ↓
review / repeated support
    ↓
accepted
```

Contradiction flow:

```text
accepted
    ↓ new contrary evidence
contested
   ↙     ↘
confirmed  superseded/retracted
```

Supersession preserves historical validity instead of destroying the previous record.

---

## 12. Confidence, Authority, Stability, and Utility Are Separate

Avoid one overloaded `confidence` score. Candidate dimensions include:

- epistemic confidence;
- source reliability;
- evidence strength;
- evidence coverage;
- contradiction score;
- consensus score;
- model confidence;
- authority;
- stability / likelihood-to-change;
- importance;
- utility;
- novelty;
- salience;
- retrieval strength;
- memory strength.

A direct user directive can have extremely high **authority** even if “confidence” is not the relevant concept. A repeatedly useful inferred convention may have high **utility** but only moderate epistemic certainty.

---

## 13. Scope Promotion, Demotion, and Conflict Resolution

Memories may be promoted across scopes as evidence accumulates:

```text
Session observation
      ↓
Project observation
      ↓
Project belief
      ↓
Project mental model
```

```text
Session user behavior
      ↓
User observation
      ↓
User belief
      ↓
User mental model
```

```text
Repeated pattern across projects
      ↓
Cross-project observation
      ↓
Universal procedure/convention
```

Promotion to broader scopes requires progressively stronger evidence/review. Universal promotion should have the strongest bar.

Demotion should also be possible when evidence weakens or scope was inferred too broadly.

### Scope conflict resolver

Conflicting applicable memories should be resolved using separately modeled factors such as:
- authority;
- scope specificity;
- explicitness;
- validity window;
- branch/commit applicability;
- recency;
- confidence;
- user/agent policy.

Candidate precedence pattern:

```text
explicit current directive
    > explicit project requirement
    > persistent user directive
    > project convention
    > user preference
    > universal default
    > inferred belief
```

This should be a configurable policy, not hard-coded universal truth.

---

## 14. Bitemporal and Code-State-Aware Knowledge

Every relevant claim/memory can distinguish:

1. **Valid time:** when the fact/belief/event was true in the world/project.
2. **Transaction/record time:** when the memory system learned or recorded it.
3. **Code reality (optional):** branch/worktree/commit range in which it applies.

This enables queries such as:
- “What did we believe on February 1?”
- “What configuration was valid in release `v2.3`?”
- “How does authentication work on `feature/new-auth`, not `main`?”

Historical `--as-of`/timeline APIs should reconstruct the knowledge state at an earlier time without rewriting the present.

---

## 15. Projection / Metadata Index Architecture

Metadata databases are synchronized, rebuildable projections keyed by stable IDs. Different indexes may cover all memories or only selected memory types/scopes.

Candidate projection families:

### 15.1 Lexical index
- SQLite FTS5;
- BM25 ranking;
- memory/field/sentence/claim indexing;
- configurable field weights;
- recency/authority/type boosts outside the base lexical score.

### 15.2 Dense vector index
- optional local embeddings (e.g. Ollama-hosted models such as BGE-family models);
- optional API-generated embeddings;
- cosine/dot-product retrieval;
- one vector per memory plus optional field/sentence/claim vectors;
- stored model name/version/dimensions for every embedding;
- candidate stores may include SQLite vector extensions, LanceDB, Qdrant, pgvector, or another replaceable backend.

Embeddings are **optional**, not mandatory for semantic retrieval.

### 15.3 LLM semantic query expansion
A “smart” retrieval mode may use Claude, Codex, Antigravity, Ollama/local LLMs, or other models to expand/transform/decompose a query into lexical, entity, temporal, causal, contradiction, and task-oriented searches. This can replace embeddings for some modes or augment them.

### 15.4 Graph indexes
Multiple graph projections may coexist for different relationship families and scopes. Candidate implementations can range from relational edge tables to CozoDB/Neo4j/graph libraries, but no single graph technology is assumed at this stage.

### 15.5 Temporal index
Indexes valid-time and transaction-time intervals, scheduled events, temporal relations, sessions, commits, and event chains.

### 15.6 Entity index
Canonical entity resolution, aliases, mentions, coreference, and entity-memory adjacency.

### 15.7 Anchor/code index
Repo/file/symbol/AST/commit mappings and file-drift status.

### 15.8 Cognitive-state index
FSRS state, decay, novelty, salience, synaptic tags, activation, utility, and retroactive salience.

---

## 16. Projection Synchronization and Mutation Journal

Do not attempt to update every database as one distributed transaction.

Canonical writes produce an immutable mutation/event journal:

```text
Canonical Markdown mutation
          ↓
Mutation/Event Journal
          ↓
Projection Queue
 ┌────────┼──────────┬─────────┬──────────┐
 ▼        ▼          ▼         ▼          ▼
FTS     Vector      Graph    Temporal   Entity/Anchor
```

Each mutation records fields such as:
- transaction/event ID;
- memory ID;
- memory version;
- previous hash;
- new hash;
- event type;
- timestamp;
- writer agent/human;
- schema/version information.

Each projection records:
- last applied canonical memory version;
- last applied transaction/event;
- source hash;
- projection schema version;
- model/index version where relevant.

A reconciler compares canonical and projection state and detects drift:

```text
Markdown V17
FTS      V17  ✓
Vector   V17  ✓
Graph    V16  ⚠ stale
Temporal V17  ✓
```

---

## 17. Invariant Checker / Memory FSCK

The system should expose integrity operations analogous to filesystem checks:

```text
memory fsck
memory reconcile
memory rebuild-indexes
memory validate
```

Candidate invariants:
- every DB `memory_id` resolves to canonical Markdown or an intentional tombstone;
- every accepted fact/belief has required evidence unless its type policy explicitly permits otherwise;
- supersession chains are acyclic;
- evidence IDs resolve;
- file anchors resolve or are marked `NeedsReview`;
- projection versions never exceed canonical versions;
- mental models cite constituent claims/observations;
- promotions have explicit promotion events;
- privacy/scope restrictions propagate through derived memories;
- every embedding records model/version/dimensions;
- graph edges resolve to existing nodes or tracked tombstones;
- branch/commit validity references existing repository state where expected;
- no projection silently diverges from the canonical hash.

---

## 18. Retrieval Architecture

Retrieval uses **multiple specialized channels**, all available as tools to retrieval/context subagents.

A core retrieval pass should support at least:

```text
Dense semantic similarity ─┐
BM25 lexical retrieval ────┤
Graph retrieval ────────────┼─→ RRF → optional cross-encoder/learned rerank
Temporal filtering ─────────┘
```

The broader channel library includes:

```text
semantic vectors
BM25 / FTS lexical
LLM query expansion
temporal retrieval
entity adjacency
graph enhancement
causal retrieval
contradiction retrieval
supersession retrieval
failure/gotcha retrieval
requirements/authority retrieval
co-usage retrieval
episode-sequence retrieval
Hopfield retrieval
spreading activation
retroactive salience
file/symbol adjacency
same-session retrieval
same-project retrieval
same-time-period retrieval
```

Candidate fusion pipeline:

```text
specialized candidate streams
        ↓
per-channel normalization
        ↓
Reciprocal Rank Fusion (RRF)
        ↓
bounded candidate set
        ↓
optional cross-encoder / learned reranker
        ↓
scope + authority + validity filters
        ↓
Context Compiler
```

One recommended minimal hybrid path remains:

```text
BM25 lexical
   +
entity adjacency
   +
optional dense vectors
   ↓
RRF
   ↓
bounded candidate set
```

---

## 19. Retrieval Explanation and Feedback

Every retrieval should be explainable. Example trace:

```text
Memory M288
Retrieved by:
  BM25             rank 3
  Dense            rank 11
  Graph            rank 2
  Temporal         rank 6
RRF score:         0.083
Cross-encoder:     0.917
Boosts:
  same-project     +0.12
  same-file        +0.08
  used-before      +0.05
Penalties:
  age              -0.03
Final rank:        2
```

Track downstream utility events such as:
- retrieved;
- opened/expanded;
- included in compiled context;
- referenced by model;
- cited in answer;
- influenced tool call;
- influenced decision;
- contributed to successful outcome;
- contributed to failure;
- human positive feedback;
- human negative feedback.

This creates training/evaluation data for future learned ranking.

---

## 20. Deduplication, Novelty, Contradictions, and Content Evolution

Use several methods together:
- SHA256 exact deduplication;
- embedding similarity deduplication;
- Jaccard comparison by memory type;
- lexical overlap;
- claim-level equivalence;
- entity/time-aware equivalence;
- novelty gating;
- prediction-error gating;
- contradiction detection;
- content evolution / merge / supersession;
- automatic similarity edges;
- clustering and summarization.

### Prediction-error / novelty gating

When new information arrives:
1. compare against existing relevant memory;
2. estimate novelty and prediction error;
3. merge redundant information where safe;
4. strengthen existing evidence if the new item corroborates it;
5. surface contradictions instead of silently coexisting;
6. create a new memory when materially novel;
7. route ambiguous cases to review specialists.

---

## 21. Clustering and Associative Structure

Clustering may be performed within and across memory types while respecting user/agent/privacy boundaries.

Candidate clustering methods:
- KNN neighborhoods;
- k-means;
- HDBSCAN;
- spectral clustering;
- Gaussian mixture models;
- hierarchical/agglomerative clustering.

Useful grouping dimensions:
- memory type;
- scope;
- repo/project;
- session;
- created-time period;
- updated-time period;
- entity;
- file/symbol;
- evidence source;
- semantic topic;
- retrieval co-use;
- causal chains.

Cluster membership is normally derived state. Cluster-derived summaries may become proposed memories only after explicit consolidation/review.

---

## 22. Cognitive Memory Mechanisms

The design may incorporate cognitive-inspired mechanisms selectively by memory type.

### 22.1 FSRS-6-style decay / retrieval strength
Dynamic retrieval strength can model how often memories should surface. Hard requirements and immutable rules should not decay merely because they are rarely queried.

Separate:
- memory strength;
- retrieval strength;
- authority;
- epistemic confidence;
- stability;
- importance.

### 22.2 Synaptic tagging
Recent/high-salience memories can receive temporary tags that alter consolidation likelihood and retrieval strength.

### 22.3 Spreading activation
Activation can propagate through entity, causal, temporal, similarity, task, and co-use edges to surface indirectly related memories.

### 22.4 Dual-strength memory
Maintain fast/temporary and slow/consolidated strength states so recent experience can influence behavior before long-term promotion.

### 22.5 Dream-like/offline consolidation
Periodic consolidation jobs can inspect clusters/episodes, propose abstractions, detect repeated patterns, strengthen/decay links, and propose mental-model updates. These operations produce **proposals**, not silent canonical truth mutations.

### 22.6 Retroactive Salience Backfill
When a later event reveals the importance of an earlier memory, walk backward through causal/temporal/entity links and increase the earlier memory’s salience or review priority.

### 22.7 Causal backtracking
For failures or outcomes, traverse likely causal chains backward to retrieve earlier decisions, tool results, changes, or observations that may have contributed.

---

## 23. Facts, Observations, Beliefs, and Mental Models

Depending on scope/type, distinguish:

```text
Evidence
   ↓
Fact claims
   ↓
Observations
   ↓
Generalized beliefs
   ↓
Mental models
```

- **Facts** aim to represent evidenced propositions.
- **Observations** record specific interpretations or noteworthy patterns from experience without overstating generality.
- **Beliefs** generalize across facts/observations and carry supporting evidence/proof counts, contradiction state, and confidence.
- **Mental models** are maintained answers to standing questions, updated as evidence accumulates.

Examples of mental-model questions:
- “What architectural style does this project prefer?”
- “What coding conventions does this user prefer?”
- “What recurring failure patterns occur in this repo?”
- “How does this agent generally perform best on refactors?”

Mental models should cite the claims/observations that currently support them, and updates must preserve prior versions.

During retrieval and after turns, evidence citations/certainty for retrieved/used memories may be rechecked by specialist evaluators, with changes recorded as new review events rather than silent history edits.

---

## 24. Write Policy State Machine

Every memory class gets a configurable write policy. Candidate modes include:

```text
off
observe
review
selective
auto-with-audit
```

Policies may differ for:
- human-entered facts;
- user preferences;
- inferred beliefs;
- project requirements;
- procedures;
- session working memory;
- tool results;
- mental-model updates;
- universal promotion.

A type policy defines:
- who may propose;
- who may approve;
- evidence threshold;
- minimum confidence;
- required reviewer count/type;
- whether human review is required;
- TTL/decay policy;
- allowed scopes;
- promotion/demotion rules;
- contradiction handling;
- supersession behavior;
- privacy ceiling;
- maximum write frequency.

---

## 25. Specialist Subagent Architecture

The system intentionally favors **many granular specialist subagents**, potentially hundreds of logical specializations, with multiple levels of routing and review.

Rather than hand-authoring hundreds of independent implementations, define typed agent roles and instantiate them per memory type/subtype/scope.

Base role families may include:

```text
MemoryAgent
├── RouterAgent
├── AdmissionAgent
├── CaptureAgent
├── EvidenceCollectorAgent
├── EvidenceJudgeAgent
├── FactExtractorAgent
├── ObservationAgent
├── BeliefSynthesizerAgent
├── MentalModelAgent
├── EntityResolverAgent
├── TemporalDerivationAgent
├── AnchorAgent
├── DedupAgent
├── NoveltyAgent
├── ContradictionAgent
├── MergeAgent
├── SupersessionAgent
├── PromotionAgent
├── DemotionAgent
├── ReviewAgent
├── CorrectionAgent
├── ConsolidationAgent
├── RetrievalPlannerAgent
├── QueryExpansionAgent
├── RetrievalChannelAgent
├── RankingAgent
├── ContextCompilerAgent
├── SessionReviewAgent
├── ProjectReviewAgent
├── TaskContinuityAgent
├── ReminderAgent
├── CoordinationAgent
├── Privacy/GovernanceAgent
├── AuditAgent
└── Health/ReconciliationAgent
```

Typed instances may look conceptually like:

```text
ReviewAgent<Project.Decision>
ReviewAgent<User.Preference>
ReviewAgent<Persona.Belief>
ReviewAgent<Universal.Procedure>
EvidenceJudgeAgent<Project.Requirement>
CaptureAgent<Session.ToolResult>
```

Each specialist definition includes:
- input schema;
- output schema;
- allowed tools;
- allowed indexes;
- allowed memory scopes;
- allowed write operations;
- evidence requirements;
- review authority;
- model/provider/profile;
- prompt version;
- retry/failure policy;
- upstream/downstream specialists;
- audit requirements.

Hierarchical supervisors/routers may coordinate specialists, but semantic work remains tightly scoped.

---

## 26. Processing Stages

A broad seven-stage lifecycle can cover identity, enrichment, cognition, retrieval, and governance:

1. **Admission** — identity, scope, authorization, basic schema/type routing, evidence eligibility.
2. **Queryable Core** — canonical save plus basic lexical/ID/field/sentence/claim indexing.
3. **Enrichment** — entity extraction, temporal derivation, embeddings, anchors, keywords, graph edges.
4. **Memory Brain** — novelty, contradiction, clustering, consolidation, decay, salience, beliefs, mental models.
5. **Retrieval** — specialized search channels, graph/temporal/causal expansion, RRF/reranking.
6. **Context Safety** — authority, scope, privacy, redaction, evidence/provenance, contradiction packaging.
7. **Operations** — reconciliation, backup, mesh/multi-machine sync, health, audit, evaluation, digests.

Substages/tasks may include:
- entity extraction;
- temporal derivation;
- embedding;
- graph propagation;
- Hopfield retrieval;
- spreading activation;
- clustering;
- rank fusion;
- learned ranking;
- consolidation;
- evidence verification;
- anchor validation;
- scope/authority resolution.

---

## 27. Hooks and Automatic Runs

Required lifecycle hooks include:
1. session start;
2. prompt/user message;
3. tool use;
4. task checkpoint;
5. session stop;
6. precompact/context compaction.

Coding-agent integrations should additionally consider:
- Git commit;
- checkout/switch;
- branch creation;
- worktree creation/switch;
- merge;
- rebase/pull;
- PR opened/updated/merged;
- test failure;
- test success;
- deployment;
- deployment failure;
- file modified;
- tracked symbol modified;
- dependency change;
- user correction;
- contradiction detected;
- evidence invalidated;
- reminder due;
- task completed;
- model upgraded;
- embedding model changed;
- tool schema changed;
- memory schema/ontology changed.

### Session-stop/precompact capture

Session ingestion must be more discriminating than transcript dumping:
1. inspect transcript;
2. inspect Git diff and repository state;
3. run a “worth remembering?” gate;
4. decompose into candidate memory types;
5. run separate specialist passes for facts, observations, requirements, preferences, errors, attempts, outcomes, patterns, procedures, etc.;
6. gather/attach evidence;
7. perform novelty/dedup/contradiction checks;
8. audit the capture event;
9. write only memories allowed by the relevant write policies;
10. queue proposed learnings/promotions for later review if necessary.

---

## 28. Context Compiler

Retrieval results should not be dumped directly into the main agent context.

```text
Query / task state
       ↓
Query decomposition
       ↓
Specialized retrieval channels
       ↓
Candidate fusion/reranking
       ↓
Context Compiler
       ↓
Agent context
```

The Context Compiler handles:
- scope precedence;
- authority;
- current branch/commit validity;
- token budget;
- deduplication;
- contradictions;
- citation/evidence packaging;
- diversity;
- privacy/redaction;
- temporal validity;
- memory-type quotas;
- compression/summarization;
- hard-requirement pinning;
- task-continuity state.

Example budget profile:

```text
Hard requirements       1,200 tokens
Relevant project state  2,000
User preferences          500
Recent episode             700
Procedural memory        1,000
Supporting evidence      1,500
Contradictions             400
```

Different models/agents can use different compiler profiles.

---

## 29. Checkpoint / Resume, Task Continuity, TODOs, and Reminders

The system should provide explicit **checkpoint/resume semantics**.

A checkpoint may include:
- current goal;
- current task/subtask;
- completed steps;
- pending steps;
- blockers;
- open files/anchors;
- relevant decisions;
- important retrieved memories;
- tool state where serializable;
- branch/worktree/commit;
- commands to resume;
- expected next action.

TODOs are durable, assignable queue objects rather than plain prose only. They may include:
- queue position/priority;
- project/global scope;
- assigned agent;
- dependencies;
- `blocked-by`;
- delivery/start conditions;
- state history;
- completion evidence;
- supersession/completion rather than deletion.

Reminder system:
- memory-linked reminders;
- task-linked reminders;
- scheduled temporal events;
- snooze/reschedule;
- agent-specific delivery;
- conditional reminders.

Weekly and periodic digests should exist at multiple scopes:
- general/user/agent digest;
- project/repo digest;
- memory-health/debt digest;
- unresolved contradiction digest;
- stale procedure/anchor digest;
- task continuity digest.

---

## 30. Multi-Agent Coordination

Special cross-agent memories act as durable team messages.

A message can contain:
- sender agent;
- receiver agent(s)/role/team;
- subject/type;
- body;
- evidence/memory links;
- delivery condition;
- priority;
- acknowledgment requirement;
- expiration;
- resulting task/TODO IDs;
- read/acknowledged/acted-on history.

TODO queues support assignment, claiming, release, reassignment, dependencies, and blockers.

Cross-agent sharing must obey privacy/scope/permission rules. Derived memories inherit the maximum privacy restriction of their sources unless explicitly sanitized/declassified.

---

## 31. API Surface

Core requested routes/operations:

```text
remember
retain
recall
buildContext
feedback
revise
review
forget
export
```

Expanded candidate API:

```text
get
search
explainRecall

remember
retain
recall
buildContext

feedback
revise
review
correct

promote
demote
merge
split

supersede
retract
contest
confirm

link
unlink

addEvidence
invalidateEvidence

asOf
timeline
graph

checkpoint
resume

assign
claimTask
releaseTask
completeTask

messageAgent
ackMessage

remind
snooze

forget
delete/tombstone
export

validate
reconcile
reindex
rebuild

health
metrics
audit
trace
replay
```

Explicit correction by memory ID, revision history, review, audit, export, and deletion/tombstoning are mandatory.

---

## 32. Local Web API / Explorer UI

All memories should be viewable and explorable through a local web API/UI.

Views should eventually include:
- memory browser/tree;
- full Markdown with frontmatter;
- evidence pane;
- claim-level provenance;
- related-memory graph;
- entity graph;
- support/contradiction graph;
- timeline/as-of view;
- supersession history;
- branch/commit validity;
- retrieval explanation;
- memory usage history;
- mental-model history;
- task/TODO timeline;
- agent-message inbox/outbox;
- projection/index health;
- broken-anchor review queue;
- memory debt dashboard.

---

## 33. Graph Families

Multiple specialized graphs are preferable to one undifferentiated “knowledge graph.” Candidate graph projections include:

### Requested / foundational graphs
1. repo/project-scoped graph;
2. universal memory graph;
3. universal cause/effect graph across types/scopes;
4. same-month/time-window graph;
5. same-session graph;
6. episodic/event graph across types;
7. facts → observations → beliefs → mental models, linked to evidence;
8. unidirectional project Task/TODO ordered timeline with `related-to` and `blocked-by`;
9. universal Task/TODO ordered timeline;
10. project + agent Task/TODO ordered timeline;
11. files → linked memories;
12. evidence → memories;
13. memories → evidence;
14. commands → memories;
15. decisions → memories;
16. skills/procedures graph;
17. memories → fields → sentences → claims;
18. graph per memory type and scope (User, Persona, Session, Project, Universal, etc.);
19. dependency-linked graph.

### Additional graph families
20. **Support / Contradiction Graph** — evidence/claim support and contradiction;
21. **Supersession / Revision Graph** — prior → superseded-by → current;
22. **Provenance / Derivation Graph** — source → extraction activity → fact → observation → belief;
23. **Belief Lineage Graph** — evidence/facts → observations → belief versions → mental-model versions;
24. **Entity Coreference Graph** — aliases/mentions → canonical entity;
25. **Requirement Traceability Graph** — requirement → decision → code → test → deployment;
26. **Error Causality Graph** — symptom → error → cause → fix → regression test;
27. **Goal / Plan / Task Graph** — goal → plan → task → tool action → artifact → outcome;
28. **Skill Dependency Graph** — skill → prerequisite skill/tool/procedure;
29. **Agent Communication Graph** — sender → message → receiver → acknowledgment → task/outcome;
30. **Authority / Override Graph**;
31. **Scope Inheritance Graph**;
32. **Permission / Visibility Graph**;
33. **Retrieval Co-Usage Graph** — memories repeatedly useful together;
34. **Retrieval Success Graph** — query/task type → memory → outcome;
35. **Code Symbol Graph** — repo → file → class/function/symbol → memories;
36. **Git Evolution Graph** — commit → changed symbol/file → invalidated/affected memories;
37. **Branch / Worktree Reality Graph** — divergent truths by code line;
38. **Temporal Interval Graph** — before/after/overlaps/during/begins/ends relations;
39. **Reminder / Event / Task Graph**;
40. **Prompt → Tool → Output → Memory Graph**;
41. **Evidence Conflict Graph**;
42. **Mental Model Dependency Graph**;
43. **Memory Promotion/Demotion Graph**;
44. **Memory Invalidation Graph** — changed evidence → affected facts → beliefs → mental models;
45. **Anchor/File Drift Graph** — file/symbol changes → affected memories;
46. **Agent Run Graph** — run → retrieved memories → tools → outputs → decisions → outcomes;
47. **Clustering/Topic Graph** — cluster/topic → member memories/entities;
48. **Causal Backtracking Graph** — outcome/failure → probable upstream causes;
49. **Privacy/Taint Derivation Graph** — sensitive source → derived artifacts and sharing ceiling;
50. **Model/Prompt Lineage Graph** — generated memory → extractor/judge/prompt/model version.

Graph data may be materialized differently depending on its semantics. Stable asserted edges may appear in Markdown; dynamic similarity/activation/cluster edges remain projections.

---

## 34. Version Every Derived Mechanism

Record versions for:
- memory schema;
- ontology;
- graph schema;
- embedding model/name/dimensions;
- reranker model;
- extractor agent;
- evidence judge;
- summarizer/consolidator;
- entity resolver;
- temporal parser;
- router;
- write-policy profile;
- context-compiler profile;
- retrieval profile;
- ranking configuration;
- prompt templates;
- tool schemas.

This allows queries such as:

```text
find all memories produced or approved by extractor-v4
```

and bulk re-review/re-embedding/re-indexing after a flawed model, prompt, schema, or policy is discovered.

---

## 35. TTL, Decay, Retention, and Lasting Power

TTL/decay policy is defined per memory class.

Examples:

### Hard requirement/rule
```text
authority: high/permanent
retrieval strength: persistent
decay: none or extremely slow
retention: permanent unless superseded
```

### Episodic memory
```text
authority: low
retrieval strength: FSRS/dynamic
retention: long but may archive
```

### Preference
```text
authority: moderate
stability: learned
retrieval strength: slow decay
requires contradiction/update checks
```

### Tool result
```text
authority: evidentiary
TTL: often short unless promoted/referenced
```

`lasting_power` can modulate decay using:
- model-estimated likelihood to change;
- model-estimated future usefulness;
- evidence quantity/quality;
- linked-file count;
- used-retrieval count;
- certainty;
- authority;
- human-entered status;
- outcome utility;
- cross-session persistence.

Decay should affect **retrieval probability**, not whether historical evidence still exists.

---

## 36. Privacy, Permissions, and Taint Propagation

Sharing states may include:
- personal;
- user;
- agent;
- repo/project;
- global/universal;
- named users/agents/groups.

Derivation inherits restrictions:

```text
Private User Memory
        ↓
Observation
        ↓
Mental Model
```

The mental model remains private unless an explicit sanitization/declassification process approves broader sharing.

Privacy rules must apply to:
- retrieval;
- context compilation;
- graph traversal;
- clustering;
- learned ranking/evaluation data;
- cross-agent messages;
- exports;
- digests.

---

## 37. Replayability and Agent-Run Records

Every significant agent run can record:
- agent/model/version;
- prompt/system/context profile;
- query/task;
- canonical memory state/version/as-of timestamp;
- retrieved memory IDs;
- channel/ranking scores;
- evidence used;
- compiled context IDs;
- tool calls;
- tool outputs;
- decisions;
- file/code changes;
- result/outcome;
- feedback.

A replay facility should answer:

> “What did this agent know when it made this decision?”

and support deterministic/approximate reruns against historical memory state.

---

## 38. Memory Evaluation Harness

Maintain explicit evaluation fixtures:

```text
query/task
expected memories
forbidden memories
expected scope
expected evidence
expected historical answer
expected authority behavior
```

Candidate metrics:
- Recall@K;
- Precision@K;
- MRR;
- NDCG;
- evidence coverage;
- citation correctness;
- contradiction-detection accuracy;
- supersession correctness;
- stale retrieval rate;
- wrong-scope retrieval rate;
- privacy-leak rate;
- memory pollution rate;
- useful-memory omission rate;
- context-token cost;
- latency by channel;
- time-travel/as-of correctness;
- cross-session continuity;
- checkpoint/resume success;
- task completion improvement;
- retrieval utility rate;
- anchor relocation success;
- projection reconciliation correctness.

Historical sessions can be replayed against competing retrieval configurations to determine which combinations actually work best for the intended workloads.

---

## 39. Memory Health and Memory Debt

Expose operational health such as:

```text
Canonical memories          18,203
Accepted                     14,631
Needs review                    328
Broken anchors                   19
Unresolved contradictions        43
Missing evidence                 76
Stale project memories          119

FTS projection          current
Vector projection       current
Graph projection        3 behind ⚠
Temporal projection     current
```

Define **memory debt** to include:
- broken anchors;
- unresolved contradictions;
- unreviewed promotions;
- weakly evidenced beliefs;
- stale procedures;
- orphan entities;
- outdated mental models;
- projection drift;
- stale embeddings;
- invalid evidence links;
- unresolved scope conflicts;
- dead agent messages;
- overdue reminders/tasks;
- old candidate/proposed memories never adjudicated.

Generate a periodic Memory Debt digest.

---

## 40. Local-First Technology Candidates

This document does not lock implementation choices, but candidate technologies include:

- **Canonical:** Markdown + YAML frontmatter + Git;
- **IDs/hashes:** UUID/ULID-like stable IDs + SHA256 content hashes;
- **Lexical:** SQLite FTS5/BM25;
- **Relational metadata:** SQLite initially, PostgreSQL optional for multi-user/server mode;
- **Dense vectors:** sqlite-vec or similar local extension initially; optional LanceDB/Qdrant/pgvector adapters;
- **Local embeddings:** Ollama-hosted BGE-family or other configurable models;
- **API embeddings:** pluggable provider adapters;
- **Code parsing/anchors:** Tree-sitter + Git blob/commit identity;
- **Graph:** relational edge tables initially, with optional graph projection adapters such as CozoDB/Neo4j/etc.;
- **Temporal:** relational interval tables + specialized temporal indexes;
- **Reranking:** cross-encoder and/or learned ranking adapters;
- **Clustering:** scikit-learn/HDBSCAN-compatible algorithms or equivalent service;
- **Web/API:** local HTTP API and browser explorer;
- **Agent integration:** MCP plus direct library/CLI/API hooks;
- **Automation:** hook/event bus + projection queue;
- **Schemas:** JSON Schema/Pydantic-like typed models or equivalent;
- **Provenance vocabulary:** W3C PROV-inspired model.

Adapters should keep the architecture replaceable rather than coupling canonical memory to one backend.

---

## 41. What Is Canonical vs What Is a Projection?

### Canonical
- Markdown memory content;
- stable IDs;
- semantic memory type/scope;
- explicit human/agent assertions;
- immutable evidence;
- provenance declarations;
- explicitly asserted relationships;
- supersession/retraction history;
- permissions/policy-critical metadata;
- durable tasks/checkpoints/reminders where they must survive full index rebuilds.

### Rebuildable projections
- FTS terms/BM25 indexes;
- embeddings;
- vector indexes;
- entity mention indexes;
- sentence/claim search tables;
- graph materializations;
- KNN/similarity edges;
- clustering results;
- spreading activation;
- Hopfield state;
- decay/FSRS scheduling state if reconstructable from event history;
- current salience;
- learned ranking features/scores;
- retrieval co-use edges;
- utility statistics;
- derived summaries unless promoted to canonical memory through review.

If a derived state cannot be reconstructed and matters historically, record the relevant event/snapshot in the canonical event/audit log.

---

## 42. Immediate Design Documents to Derive From This Overview

This master overview is intentionally too broad to serve as the final implementation spec. It should be decomposed into focused documents for:

1. canonical filesystem and memory ontology;
2. memory type taxonomy by scope;
3. common YAML/frontmatter schema;
4. memory/section/field/sentence/claim/evidence data model;
5. evidence and provenance model;
6. temporal/bitemporal/code-state model;
7. projection/index architecture;
8. retrieval channels and ranking/fusion;
9. graph catalog and graph schemas;
10. cognitive mechanisms and decay/consolidation;
11. write-policy state machines;
12. specialist subagent hierarchy and contracts;
13. hook/event workflows;
14. session capture and review workflow;
15. project/repo capture workflow;
16. context compiler;
17. tasks/TODO/checkpoint/resume/reminder system;
18. multi-agent coordination/messages;
19. API/MCP/CLI surface;
20. local web explorer/UI;
21. security/privacy/governance;
22. projection synchronization/event journal;
23. invariant checker/fsck;
24. evaluation harness;
25. memory health/debt/observability;
26. replay/audit system;
27. deployment/local-first/multi-machine architecture;
28. implementation phases and MVP boundary.

---

## 43. Final Architectural Thesis

Ultra Agent Memory should behave less like a vector database with notes and more like an **evidence-grounded cognitive memory operating system**.

Its defining model is:

```text
Immutable evidence
      ↓
Canonical Markdown memories
      ↓
Claims / facts / observations
      ↓
Beliefs / mental models
      ↓
Specialized index projections
      ↓
Hybrid multi-channel retrieval
      ↓
Context Compiler
      ↓
Agent action
      ↓
Outcome / feedback
      ↓
New evidence, utility signals, and reviewed learning
```

The core safety/epistemic rule remains:

> **Higher-order synthesized memory may annotate, contest, refine, or supersede interpretations, but it may never rewrite the underlying evidence.**

And the core engineering rule is:

> **Canonical Markdown and immutable evidence describe what was asserted or observed. Everything clever — embeddings, graphs, clustering, salience, decay, beliefs, mental models, ranking, consolidation, activation — is a versioned derivation whose lineage can be traced back to canonical evidence.**

This document is the current coverage-first source of truth. Later specifications should narrow, formalize, diagram, and implement these ideas without silently dropping concepts recorded here.
