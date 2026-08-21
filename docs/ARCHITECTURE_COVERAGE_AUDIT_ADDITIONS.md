# Ultra Agent Memory — Architecture Coverage Audit Additions

> **Status:** Normative addendum to `ARCHITECTURE_OVERVIEW.md` produced from a direct coverage audit against the originating design conversation.
>
> **Interpretation rule:** Where this document strengthens, clarifies, or makes explicit a requirement that is expressed more weakly or only implicitly in `ARCHITECTURE_OVERVIEW.md`, this addendum is the intended current interpretation. These items should later be folded into the focused specifications and, when the master overview is next normalized, into the relevant sections of the overview itself.

This addendum preserves several requirements and candidate capabilities that were either softened during normalization of the initial architecture document or were discussed explicitly but not named strongly enough in the master overview.

---

## 1. Provenance-on-Use Is a Runtime Requirement

The architecture must do more than merely be *capable* of citing memory provenance.

When a memory materially contributes to an answer, reply, action, recommendation, tool invocation, decision, synthesized belief, mental-model update, or other agent output, the runtime must record:

- the memory IDs that were actually used;
- the claim IDs used where claim-level attribution is available;
- the evidence IDs supporting those memories/claims;
- the provenance lineage of the evidence;
- the retrieval event(s) that surfaced the memory;
- whether the memory was only retrieved, included in context, or actually used;
- the resulting action/output/run ID.

User-facing citation formatting may vary by client, output mode, privacy policy, or task, but **internal provenance-on-use recording is mandatory**.

This enables questions such as:

```text
Why did the agent say this?
Which memories materially influenced this answer?
Which evidence supports those memories?
Was a retrieved memory ignored or actually used?
Which action was based on a later-invalidated memory?
```

---

## 2. Full-Fusion Retrieval Is the Reference / Correctness Mode

The system must preserve a retrieval mode in which **all applicable retrieval channels and metadata-index projections are searched during a retrieval pass** for the eligible memory scopes/types.

This is the reference/correctness mode against which optimized routing can be evaluated.

```text
FULL-FUSION / REFERENCE RETRIEVAL

query
  ↓
all applicable candidate channels
  ├── BM25 / FTS
  ├── dense semantic
  ├── LLM query expansion searches
  ├── entity adjacency
  ├── graph retrieval
  ├── temporal retrieval
  ├── causal retrieval
  ├── contradiction retrieval
  ├── supersession retrieval
  ├── failure/gotcha retrieval
  ├── requirements/authority retrieval
  ├── co-use retrieval
  ├── episode-sequence retrieval
  ├── Hopfield retrieval
  ├── spreading activation
  ├── retroactive-salience-assisted retrieval
  ├── file/symbol adjacency
  ├── same-session / same-project / same-time channels
  └── other registered channels
  ↓
normalization / RRF / fusion
  ↓
optional reranking
  ↓
Context Compiler
```

The production runtime may additionally support an optimized mode:

```text
ADAPTIVE / ROUTED RETRIEVAL

query
  ↓
Retrieval Planner / Router
  ↓
selected likely-useful channels
  ↓
fusion / reranking
  ↓
Context Compiler
```

The routed mode is an optimization, **not a replacement for the full-fusion baseline**. Evaluation should measure the quality/latency tradeoff between them.

---

## 3. Ontology Induction, Evolution, Versioning, and Migration

The ontology is not merely versioned manually. The system may actively **induce, propose, review, evolve, and migrate ontology structure** as accumulated evidence and memories reveal stable new categories, relationships, or routing needs.

Candidate specialist roles include:

```text
OntologyInductionAgent
OntologyReviewAgent
OntologyMigrationAgent
OntologyRoutingAgent
OntologyCompatibilityAgent
OntologyAuditAgent
```

Candidate lifecycle:

```text
accumulated memories + evidence + retrieval/use statistics
                    ↓
          ontology induction proposal
                    ↓
       compare with current ontology
                    ↓
     compatibility / collision analysis
                    ↓
       specialist and/or human review
                    ↓
           new ontology version
                    ↓
      migration / reclassification queue
                    ↓
 reindex + re-cluster + graph/projection rebuild
```

Requirements:

- every memory retains the ontology/schema version under which it was originally classified;
- ontology changes must be auditable;
- old ontology versions remain interpretable for historical replay/as-of queries;
- migrations must preserve canonical evidence and provenance;
- an ontology migration may change classification or derived routing state, but must not silently rewrite underlying evidence;
- migration tools should be able to identify memories requiring re-review after a changed ontology;
- ontology-induced clusters/categories are proposals until admitted through the appropriate write/review policy.

The current ontology should also be usable by routing, clustering, retrieval planning, evidence admission, promotion/demotion, and specialist-agent selection.

---

## 4. Mental Models Are Available Per Memory Type / Subtype

Mental models are not only global or scope-wide summaries. **Any applicable memory type or subtype may register one or more standing questions whose answers are continuously maintained as evidence accumulates.**

Examples:

```text
Project.Requirements
  "What constraints currently define this project?"

Project.Architecture.Decisions
  "What architecture does this project currently use, and why?"

Project.Failures
  "What failure modes recur in this repository?"

User.Preferences
  "What stable implementation/style preferences has this user demonstrated?"

Persona.Self
  "What does this agent currently believe about its own strengths and limitations?"

Universal.Tools
  "What durable lessons have been learned about using this tool?"
```

A memory type may have zero, one, or many registered mental models.

Every mental-model version should preserve:

- standing-question ID;
- memory type/subtype and scope;
- supporting claim/evidence IDs;
- contradicting evidence;
- proof/support counts where useful;
- confidence and evidence coverage;
- ontology version;
- model/prompt/judge versions involved in synthesis;
- valid and transaction times;
- prior mental-model version;
- review state.

---

## 5. Procedure and Skill Are Distinct Ontology Types

The architecture must preserve a semantic distinction between **Procedures** and **Skills**.

### Procedure

An outcome-oriented reusable workflow or runbook describing a sequence of steps used to achieve a goal.

Examples:

- release procedure;
- recovery drill;
- repository bootstrap workflow;
- incident triage procedure;
- deployment procedure.

A procedure may include:

- successful workflows;
- known failure patterns;
- checkpoints/resume points;
- tool policies;
- task strategies;
- prerequisites;
- branches/conditions;
- validation steps;
- rollback/recovery steps.

### Skill

A more specifically scoped capability or action instruction analogous to a traditional agent skill.

Examples:

- how to use a particular tool safely;
- how to perform one code transformation;
- how to validate a specific artifact type;
- how to query a particular system;
- how to execute one bounded specialist capability.

Skills may be invoked inside Procedures. Procedures may depend on multiple Skills.

The ontology and graph layer should therefore support relations such as:

```text
Procedure --requires--> Skill
Procedure --uses--> Tool
Skill --requires--> Tool
Skill --prerequisite--> Skill
Procedure --produces--> Outcome
```

---

## 6. Evidence Source Type and Evidence Semantic Role Are Separate

Evidence must be classifiable along at least two independent axes.

### Evidence source type

Where/how the evidence originated:

```text
UserStatement
Git
Document
Web
ToolResult
Test
ExternalAPI
AgentObservation
HumanEntered
SystemEvent
```

### Evidence semantic role

What epistemic/operational role the evidence serves:

```text
RequirementEvidence
DecisionEvidence
CorrectionEvidence
ConstraintEvidence
CitationEvidence
ObservationEvidence
FailureEvidence
OutcomeEvidence
PreferenceEvidence
ProcedureEvidence
```

This explicitly preserves the important canonical/evidentiary classes:

- requirements;
- architectural decisions and their rationale;
- user corrections;
- hard constraints;
- source citations.

One evidence object may have one source type and multiple semantic roles.

---

## 7. Learned Ranking Must Support Multiple Candidate Methods

Retrieval should support several reranking strategies as interchangeable tools/profiles rather than assuming one universal reranker.

Candidate reranking methods include:

```text
cross-encoder reranker
LLM pairwise reranker
LLM listwise reranker
heuristic/rule-based reranker
LightGBM / learning-to-rank model
task-specific learned reranker
agent-specific learned reranker
project-specific learned reranker
```

The ranker/reranker version and feature schema must be recorded for replay and audit.

### Ranking feature families

Candidate final-ranking features explicitly include combinations of:

- base retrieval score(s);
- RRF contribution;
- age decay / recency;
- access frequency;
- total used access count;
- retrieved-but-unused rate;
- empirical retrieval utility;
- epistemic confidence;
- composite certainty score;
- authority;
- evidence strength/coverage;
- memory stability;
- scope match;
- branch/commit validity;
- task similarity;
- same-session/project/file/entity boosts;
- contradiction/supersession penalties;
- outcome history.

In particular, ranking profiles should be able to combine **age decay + access frequency + confidence/utility** rather than treating those signals independently only.

---

## 8. Experimental Guarded Adaptive Capabilities

The architecture should preserve an explicit experimental category for adaptive capabilities discussed during comparison of advanced memory systems.

Candidate capabilities include:

```text
reward/outcome-derived learning signals
soft-prompt adaptation
guarded procedure evolution
guarded skill evolution
agent-specific learned retrieval profiles
project-specific learned retrieval profiles
adaptive routing policies
adaptive ranking profiles
```

These mechanisms are **derived/adaptive proposals**, not permission for uncontrolled self-modification.

Guardrails:

- learned changes are versioned;
- generated skill/procedure modifications enter `candidate` or `proposed` state;
- changes must pass evaluation and the relevant review/write policy;
- prior versions remain available for rollback/replay;
- outcome/reward signals are evidence, not unquestionable truth;
- adaptive changes must not rewrite underlying canonical evidence;
- a failed or regressed learned policy can be automatically quarantined and rolled back.

---

## 9. Composite Certainty Score May Exist as a Derived Convenience Metric

The architecture intentionally separates:

```text
epistemic confidence
source reliability
evidence strength
evidence coverage
contradiction score
consensus score
model confidence
authority
stability
importance
utility
```

However, a **derived composite certainty score** may still be calculated for ranking, dashboards, review prioritization, and policy thresholds.

Requirements for the composite:

- it is derived, never the sole canonical epistemic representation;
- its component scores remain individually accessible;
- its formula/profile is versioned;
- the system can explain the contribution of each component;
- it must not conflate authority with factual confidence;
- recalculating the composite must not mutate the underlying evidence or historical component values;
- historical runs should record which certainty profile/version they used.

Candidate inputs can include:

- epistemic confidence;
- source reliability;
- evidence strength and coverage;
- contradiction/consensus signals;
- provenance quality;
- corroborating evidence count;
- linked-file/evidence counts where meaningful;
- total used-access count;
- empirical outcome utility;
- human-entered/reviewed status.

---

## 10. Task Reference Integrity Is a Formal Invariant

If `task_during_memory`, `task_id`, or equivalent task linkage is populated, the referenced object must resolve to a known task/TODO/completed-task/tombstone record according to the task schema.

The invariant checker / `memory fsck` must detect:

```text
memory.task_id -> missing task
memory.task_during_memory -> unresolved TODO
memory.task_id -> task from forbidden scope
memory.task_id -> task version that cannot be reconstructed
```

Valid historical completed tasks may remain resolvable through completion/tombstone history.

No dangling task references should silently persist.

---

## 11. TTL Is a Policy Function, Not Necessarily a Fixed Duration

TTL/retention must support policy-driven calculation rather than only a literal duration.

A TTL/retention decision may be a function of:

```text
memory class/type/subtype
scope
created timestamp
last-updated timestamp
last-used timestamp
FSRS/decay state
stability
lasting_power
importance
utility
confidence/certainty
retention policy
supersession state
active task/project status
legal/governance policy
```

Conceptually:

```text
TTL / retention decision
  = f(memory_class,
      created_at,
      updated_at,
      decay_state,
      retention_policy,
      stability,
      utility,
      other policy inputs)
```

Possible outputs include:

```text
no-expiry
review-at timestamp
archive-at timestamp
expire-at timestamp
retain-until-superseded
retain-while-project-active
retain-while-evidence-linked
```

Decay should generally influence **retrieval probability or review priority**, not erase historical evidence.

---

## 12. Resulting Coverage Rule

The master architecture plus this addendum should be read with the following preservation principle:

> **When an implementation optimization, abstraction, or normalization makes an earlier requirement appear optional or less specific, the more explicit requirement remains part of the design until a recorded architectural decision intentionally supersedes it with rationale.**

This is particularly important for:

- provenance-on-use;
- full-fusion reference retrieval;
- evidence integrity;
- ontology evolution;
- per-type mental models;
- procedure/skill semantics;
- learned/adaptive mechanisms;
- task-reference integrity;
- TTL/retention semantics.
