Hypothetical Ultra Agent Memory Classes, Templates, and Indexes

1. Core Domain Classes

These are the fundamental objects the rest of the system operates on.

Canonical memory primitives

Memory

MemorySection

MemoryField

MemorySentence

Claim

Evidence

EvidenceCitation

MemoryAnchor

FileAnchor

CodeAnchor

MemoryVersion

MemoryRevision

MemorySupersession

MemoryTombstone

Identity and scope

Scope

ScopePath

AgentIdentity

UserIdentity

ProjectIdentity

RepositoryIdentity

SessionIdentity

Principal

Namespace

PermissionSet

AccessPolicy

Canonical relationships

MemoryEdge

RelatedToEdge

BlockedByEdge

DependsOnEdge

SupportsEdge

ContradictsEdge

SupersedesEdge

CausedByEdge

CausesEdge

DerivedFromEdge

LinkedFileEdge

LinkedEvidenceEdge

LinkedEntityEdge

LinkedTaskEdge

TemporalRelationEdge

2. Memory Type Classes

A common base could look conceptually like:

Memory -> TypedMemory -> specific subtype

Epistemic memory

FactMemory

ObservationMemory

BeliefMemory

LongTermBeliefMemory

MentalModelMemory

PatternMemory

LearnedConventionMemory

Preference and instruction memory

PreferenceMemory

RequirementMemory

DirectiveMemory

RuleMemory

ConstraintMemory

Episodic memory

EpisodeMemory

EpisodeSceneMemory

AttemptMemory

FailureMemory

ExperimentMemory

OutcomeMemory

ToolResultMemory

AgentRunMemory

Temporal memory

TemporalEventMemory

PastEventMemory

ExpectedEventMemory

ScheduledEventMemory

ReminderMemory

Project engineering memory

ProjectStatusMemory

CommitNoteMemory

RepositoryNoteMemory

ArchitectureDecisionMemory

GotchaMemory

CommandMemory

DeploymentMemory

DocumentationMemory

ErrorMemory

ProjectRuleMemory

Procedural memory

ProcedureMemory

SkillMemory

WorkflowMemory

FailurePatternMemory

ToolPolicyMemory

TaskStrategyMemory

RecoveryProcedureMemory

Executive memory

TaskMemory

TodoMemory

CompletedTaskMemory

CheckpointMemory

ResumeStateMemory

GoalMemory

PlanMemory

Agent coordination

AgentMessageMemory

AgentAssignmentMemory

AgentLeaseMemory

AgentAcknowledgementMemory

Entity memory

EntityMemory

PersonEntity

AgentEntity

UserEntity

ProjectEntity

RepositoryEntity

FileEntity

CodeSymbolEntity

ToolEntity

OrganizationEntity

TechnologyEntity

ConceptEntity

3. Scope-Specific Memory Containers

Rather than making every scope entirely separate internally, these could specialize a generic container.

Base classes

MemoryScope

MemoryCollection

CanonicalMemoryStore

Persona

PersonaScope

PersonaSelfStore

PersonaEpisodeStore

PersonaObservationStore

PersonaBeliefStore

PersonaGoalStore

PersonaPreferenceStore

Sessions

SessionScope

SessionSummaryStore

SessionWorkingMemoryStore

SessionEvidenceStore

SessionAttemptStore

SessionOutcomeStore

Projects

ProjectScope

ProjectStatusStore

ProjectRequirementStore

ProjectDecisionStore

ProjectGotchaStore

ProjectProcedureStore

ProjectTaskStore

ProjectSessionManifest

Users

UserScope

UserFactStore

UserPreferenceStore

UserDirectiveStore

UserEpisodeStore

UserBeliefStore

Universal

UniversalScope

UniversalRuleStore

UniversalProcedureStore

UniversalSkillStore

UniversalFactStore

UniversalEpisodeStore

4. Evidence and Provenance Classes

Evidence objects

EvidenceRecord

UserStatementEvidence

GitEvidence

DocumentEvidence

WebEvidence

ToolEvidence

TestEvidence

APIResponseEvidence

AgentObservationEvidence

HumanEnteredEvidence

Semantic evidence roles

RequirementEvidence

DecisionEvidence

CorrectionEvidence

ConstraintEvidence

CitationEvidence

FailureEvidence

OutcomeEvidence

VerificationEvidence

Provenance

ProvenanceRecord

ProvenanceEntity

ProvenanceActivity

ProvenanceAgent

DerivationRecord

ExtractionRecord

TransformationRecord

PromotionRecord

ReviewRecord

SupersessionRecord

Evidence relationships

ClaimEvidenceLink

SupportingEvidenceLink

ContradictingEvidenceLink

PrimarySourceLink

DerivedEvidenceLink

5. Epistemic and Scoring Classes

Score objects

ConfidenceScore

CompositeCertaintyScore

SourceReliabilityScore

EvidenceStrengthScore

EvidenceCoverageScore

ContradictionScore

ConsensusScore

NoveltyScore

ImportanceScore

UtilityScore

SalienceScore

StabilityScore

AuthorityScore

RetrievabilityScore

Lifecycle

EpistemicStatus

MemoryLifecycleState

CandidateState

ProposedState

AcceptedState

ContestedState

ConfirmedState

SupersededState

RetractedState

ExpiredState

ArchivedState

Epistemic reasoning

EvidenceBundle

ClaimSupportSet

ContradictionSet

BeliefProof

BeliefRevision

MentalModelRevision

6. Temporal Classes

Time models

ValidTimeInterval

TransactionTimeInterval

ObservedTime

AssertedTime

ExpectedTime

ScheduledTime

Code-validity models

RepositoryState

BranchState

WorktreeState

CommitRange

BlobVersion

CodeValidityInterval

Temporal relations

BeforeRelation

AfterRelation

DuringRelation

OverlapRelation

BeginsRelation

EndsRelation

TemporalChain

7. Canonical Mutation and Event Classes

These are especially important because derived indexes must be rebuildable.

Semantic mutation events

CanonicalMutationEvent

MemoryCreatedEvent

MemoryUpdatedEvent

MemorySupersededEvent

MemoryRetractedEvent

MemoryPromotedEvent

MemoryDemotedEvent

MemoryMergedEvent

MemorySplitEvent

EvidenceAddedEvent

EvidenceInvalidatedEvent

EdgeAddedEvent

EdgeRemovedEvent

Interaction/audit events

InteractionEvent

RetrievalEvent

MemoryCandidateEvent

ContextInclusionEvent

MemoryUsageEvent

MemoryCitationEvent

FeedbackEvent

ToolInvocationEvent

ToolResultEvent

TaskEvent

CheckpointEvent

OutcomeEvent

AgentMessageEvent

Projection bookkeeping

ProjectionCursor

ProjectionCheckpoint

ProjectionVersion

ProjectionLag

RebuildJob

ReconciliationJob

8. Retrieval Classes

Query processing

MemoryQuery

QueryContext

QueryIntent

QueryExpansion

QueryDecomposition

RetrievalPlan

Candidate model

RetrievalCandidate

CandidateSet

CandidateStream

CandidateScore

RetrievalExplanation

Retrieval channels

LexicalRetriever

BM25Retriever

DenseRetriever

LLMExpansionRetriever

EntityRetriever

GraphRetriever

TemporalRetriever

CausalRetriever

ContradictionRetriever

SupersessionRetriever

FailureRetriever

RequirementRetriever

AuthorityRetriever

EpisodeRetriever

CoUseRetriever

FileAnchorRetriever

CodeSymbolRetriever

HopfieldRetriever

SpreadingActivationRetriever

RetroactiveSalienceRetriever

Retrieval modes

FullFusionRetrievalStrategy

AdaptiveRetrievalStrategy

RoutedRetrievalStrategy

Security and applicability

RetrievalSecurityGate

ScopeApplicabilityGate

TemporalApplicabilityGate

CodeStateApplicabilityGate

ProviderDisclosureGate

Fusion

ScoreNormalizer

RankFusionStrategy

ReciprocalRankFusion

WeightedRankFusion

CandidateBounder

Reranking

Reranker

CrossEncoderReranker

LLMReranker

PairwiseReranker

ListwiseReranker

HeuristicReranker

LightGBMReranker

TaskSpecificReranker

9. Context Compiler Classes

ContextCompiler

ContextCompilationPlan

ContextBudget

ContextSection

ContextMemoryBundle

EvidenceContextBundle

RequirementContextBundle

TaskContextBundle

ContradictionContextBundle

ContextDeduplicator

ContextCompressor

ContextQuotaPolicy

ContextAuthorityResolver

ContextCitationValidator

ContextPrivacyFilter

10. Cognitive Memory Classes

Novelty and consolidation

NoveltyDetector

PredictionErrorGate

DuplicateDetector

SemanticDuplicateDetector

MemoryConsolidator

EpisodeConsolidator

PatternExtractor

BeliefSynthesizer

MentalModelUpdater

Decay and strength

MemoryStrength

RetrievalStrength

FSRSState

FSRSDecayEngine

RetentionStrength

DualStrengthMemoryState

Associative cognition

ActivationState

SpreadingActivationEngine

SynapticTag

HopfieldState

AssociativeRecallEngine

CausalBacktracker

RetroactiveSalienceEngine

Clustering

MemoryCluster

ClusterMembership

KNNClusterer

KMeansClusterer

HDBSCANClusterer

SpectralClusterer

GaussianMixtureClusterer

HierarchicalClusterer

11. Governance Classes

Admission

MemoryAdmissionPolicy

WorthRememberingGate

EvidenceRequirementPolicy

MemoryTypeClassifier

ScopeClassifier

PrivacyClassifier

SecretDetector

PIIDetector

Write governance

WritePolicy

WritePolicyStateMachine

WriteModeOff

WriteModeObserve

WriteModeReview

WriteModeSelective

WriteModeAutoAudited

Review

ReviewRequest

ReviewDecision

ReviewQueue

HumanReview

AgentReview

Promotion

PromotionPolicy

PromotionCandidate

PromotionDecision

DemotionPolicy

ScopePromotionPath

Conflict resolution

ScopeConflictResolver

AuthorityResolver

ApplicabilityResolver

Retention

RetentionPolicy

TTLPolicy

DecayPolicy

ArchivePolicy

DeletionPolicy

12. Task and Executive Classes

Task

Todo

TaskQueue

TaskAssignment

TaskLease

TaskHeartbeat

TaskDependency

TaskPriority

TaskCheckpoint

ResumeState

TaskHistory

CompletedTask

TaskTombstone

Goal

Plan

PlanStep

13. Multi-Agent Classes

Agent

AgentCapability

AgentProfile

AgentMailbox

AgentMessage

AgentThread

AgentAcknowledgement

AgentDeliveryCondition

AgentTaskAssignment

AgentTaskLease

AgentCoordinationPolicy

AgentScopePolicy

14. Specialist Subagent Classes

A generic typed superclass could be:

SpecialistAgent<TMemory, TOperation>

Capture specialists

FactCaptureAgent

ObservationCaptureAgent

BeliefCaptureAgent

RequirementCaptureAgent

PreferenceCaptureAgent

DecisionCaptureAgent

FailureCaptureAgent

OutcomeCaptureAgent

ProcedureCaptureAgent

Retrieval specialists

RetrievalPlannerAgent

LexicalRetrievalAgent

DenseRetrievalAgent

GraphRetrievalAgent

TemporalRetrievalAgent

CausalRetrievalAgent

ExpansionAgent

Evidence specialists

EvidenceCollectorAgent

EvidenceVerifierAgent

EvidenceJudgeAgent

CitationVerifierAgent

Review specialists

MemoryReviewAgent

ContradictionReviewAgent

PromotionReviewAgent

PrivacyReviewAgent

AuthorityReviewAgent

Cognitive specialists

BeliefSynthesisAgent

MentalModelAgent

PatternDiscoveryAgent

ConsolidationAgent

SalienceAgent

Ontology specialists

OntologyInductionAgent

OntologyReviewAgent

OntologyMigrationAgent

OntologyRoutingAgent

Operational specialists

SessionReviewAgent

CheckpointAgent

TaskContinuityAgent

DigestAgent

MemoryHealthAgent

ReconciliationAgent

15. Markdown Templates

These define canonical file structure rather than runtime classes.

Base memory template

templates/memory/base.md

Suggested sections:

YAML frontmatter

Summary

Content

Claims

Evidence

Relationships

Temporal validity

Revision history

Fact template

templates/memory/fact.md

Observation template

templates/memory/observation.md

Belief template

templates/memory/belief.md

Suggested additions:

Supporting claims

Contradicting claims

Proof/evidence summary

Confidence history

Mental model template

templates/memory/mental-model.md

Suggested additions:

Standing question

Current answer

Supporting beliefs

Contradictions

Revision history

Episode template

templates/memory/episode.md

Suggested additions:

Scene

Participants

Timeline

Attempts

Failures

Outcomes

Learnings

Requirement template

templates/memory/requirement.md

Decision template

templates/memory/decision.md

Suggested sections:

Decision

Context

Alternatives

Rationale

Consequences

Evidence

Supersession

Gotcha template

templates/memory/gotcha.md

Suggested form:

Trap

Cause

Symptoms

Fix

Prevention

Evidence

Procedure template

templates/memory/procedure.md

Skill template

templates/memory/skill.md

Task/TODO template

templates/memory/task.md

Checkpoint template

templates/memory/checkpoint.md

Agent message template

templates/memory/agent-message.md

16. Session Templates

Session root

templates/session/session.md

Session summary

Objective

Work completed

Major discoveries

Decisions

Failures

Outcomes

Remaining work

Important memories used

Session facts

templates/session/facts.md

Session observations

templates/session/observations.md

Proposed learnings

templates/session/proposed-learnings.md

Working memory

templates/session/working-memory.md

Attempts/failures

templates/session/attempts.md

Tool results

templates/session/tool-results.md

Experiments

templates/session/experiments.md

Outcomes

templates/session/outcomes.md

17. Project Templates

Project index

templates/project/index.md

Project status

templates/project/status.md

Git history manifest

templates/project/git-history.md

Requirements

templates/project/requirements.md

Decisions

templates/project/decisions.md

Gotchas

templates/project/gotchas.md

Commands

templates/project/commands.md

Procedures

templates/project/procedures.md

Skills

templates/project/skills.md

Deployment notes

templates/project/deployment.md

Task history

templates/project/task-history.md

Session manifest

templates/project/sessions.md

18. YAML Frontmatter Templates

Rather than one enormous schema, define layers.

BaseMemoryFrontmatter

Common fields:

id

schema_version

memory_type

memory_subtype

scope

status

created_at

updated_at

agent_id

user_id

project_id

session_ids

tags

evidence_ids

related_to

blocked_by

dependencies

supersedes

superseded_by

entity_ids

linked_files

entered_by_human

TemporalFrontmatter

valid_from

valid_to

recorded_from

recorded_to

expected_at

scheduled_at

ProjectFrontmatter

repository

branch

worktree

commit_from

commit_to

EvidenceFrontmatter

source_type

semantic_role

source_uri

source_hash

captured_at

observed_at

trust_class

TaskFrontmatter

task_id

assignee

task_status

priority

due_at

19. Relational Indexes

These are rebuildable metadata projections.

Core index

memories

Potential columns:

memory_id

filepath

type

subtype

scope

status

schema_version

hash

created_at

updated_at

Structural indexes

memory_sections

memory_fields

memory_sentences

claims

Evidence indexes

evidence

claim_evidence

memory_evidence

evidence_sources

evidence_roles

Relationship indexes

memory_edges

similarity_edges

dependency_edges

causal_edges

supersession_edges

contradiction_edges

Entity indexes

entities

entity_aliases

entity_mentions

memory_entities

Temporal indexes

validity_intervals

transaction_intervals

temporal_events

temporal_relations

File/code indexes

files

file_anchors

code_symbols

code_anchors

git_commits

memory_commit_links

Task indexes

tasks

task_dependencies

task_memory_links

task_assignments

task_leases

20. Lexical Search Indexes

Global

memory_fts

claim_fts

sentence_fts

field_fts

evidence_fts

entity_fts

Scoped FTS views/indexes

project_memory_fts

user_memory_fts

persona_memory_fts

session_memory_fts

universal_memory_fts

Specialized lexical indexes

requirement_fts

decision_fts

gotcha_fts

command_fts

procedure_fts

failure_fts

task_fts

21. Vector Indexes

Embeddings remain optional.

Granularity

memory_vectors

field_vectors

sentence_vectors

claim_vectors

evidence_vectors

entity_vectors

mental_model_vectors

Specialized vector spaces

Possibly maintain separate embedding spaces for:

general semantic similarity

code

entities

episodes

procedures

user preferences

Every vector record should retain:

model ID

model version

dimensions

normalization method

source hash

generation timestamp

22. Graph Indexes

General

MemoryGraph

EntityGraph

TemporalGraph

Epistemic

EvidenceSupportGraph

ContradictionGraph

BeliefLineageGraph

SupersessionGraph

ProvenanceGraph

Project/code

ProjectGraph

CodeSymbolGraph

RequirementTraceabilityGraph

GitEvolutionGraph

ErrorCausalityGraph

Executive

TaskGraph

GoalPlanTaskGraph

AgentCommunicationGraph

Cognitive

SimilarityGraph

CoUseGraph

ActivationGraph

CausalGraph

MentalModelDependencyGraph

Governance

PermissionGraph

ScopeInheritanceGraph

AuthorityGraph

PrivacyTaintGraph

23. Cognitive and Behavioral Indexes

Usage

memory_access_stats

memory_use_stats

retrieval_use_stats

memory_outcome_stats

Decay

memory_decay_state

fsrs_state

memory_strength

Salience

salience_scores

retroactive_salience

synaptic_tags

Novelty

novelty_scores

prediction_error_scores

Utility

utility_scores

task_utility_scores

agent_utility_scores

project_utility_scores

Association

co_retrieval

co_usage

spreading_activation_state

hopfield_state

24. Clustering Indexes

Keep clustering results separate by method.

knn_neighbors

kmeans_memberships

hdbscan_memberships

spectral_memberships

gaussian_mixture_memberships

hierarchical_cluster_memberships

Each result should record:

clustering algorithm

configuration

model/version

source projection version

scope

memory types included

timestamp

25. Retrieval Analytics Indexes

retrieval_runs

retrieval_queries

retrieval_candidates

retrieval_channel_scores

fusion_scores

reranker_scores

context_inclusions

memory_usage

memory_citations

retrieval_outcomes

These provide the future training/evaluation set for learned retrieval.

26. Interaction/Audit Indexes

The underlying events are authoritative; these indexes are projections.

interaction_events

memory_mutation_events

retrieval_events

context_events

tool_events

task_events

agent_message_events

feedback_events

outcome_events

review_events

27. Projection and Reconciliation Indexes

projection_registry

projection_versions

projection_offsets

projection_health

projection_errors

reconciliation_runs

rebuild_runs

orphan_records

broken_links

28. Health and Evaluation Indexes

Memory health

memory_health

memory_debt

broken_anchors

missing_evidence

unresolved_contradictions

stale_memories

stale_procedures

orphan_entities

Evaluation

eval_cases

eval_runs

expected_retrievals

forbidden_retrievals

retrieval_metrics

citation_metrics

continuity_metrics

privacy_metrics

29. Policy Templates

These could be declarative YAML rather than classes.

Evidence policy

policies/evidence/<memory-type>.yaml

Defines:

minimum evidence

acceptable evidence types

minimum source reliability

review requirement

Retention policy

policies/retention/<memory-type>.yaml

Retrieval profile

policies/retrieval/<memory-type>.yaml

Defines:

eligible channels

weights

rerankers

scope rules

result limits

Promotion policy

policies/promotion/<memory-type>.yaml

Privacy policy

policies/privacy/<scope>.yaml

Write policy

policies/write/<memory-type>.yaml

Consolidation policy

policies/consolidation/<memory-type>.yaml

30. Specialist Agent Templates

Rather than hundreds of hardcoded implementations, use reusable templates plus typed configuration.

Example template

agents/templates/capture-agent.yaml

Could define:

role

accepted input types

output memory type

evidence threshold

allowed tools

allowed scopes

write authority

review requirements

prompt version

model profile

Then instantiate:

CaptureAgent<Project.Requirement> CaptureAgent<Project.Gotcha> CaptureAgent<User.Preference> CaptureAgent<Session.Outcome> 

Similarly:

review-agent.yaml

retrieval-agent.yaml

evidence-agent.yaml

belief-agent.yaml

mental-model-agent.yaml

promotion-agent.yaml

ontology-agent.yaml

consolidation-agent.yaml

This lets the architecture conceptually contain hundreds or thousands of specialists without requiring hundreds of separate source-code classes.

31. Suggested High-Level Package Structure

A possible implementation could eventually resemble:

ultra_memory/ ├── domain/ │ ├── memory/ │ ├── evidence/ │ ├── claims/ │ ├── entities/ │ ├── tasks/ │ └── temporal/ │ ├── canonical/ │ ├── markdown/ │ ├── git/ │ ├── templates/ │ └── anchors/ │ ├── events/ │ ├── mutations/ │ ├── interactions/ │ └── replay/ │ ├── projections/ │ ├── relational/ │ ├── fts/ │ ├── vectors/ │ ├── graphs/ │ ├── temporal/ │ └── cognitive/ │ ├── retrieval/ │ ├── channels/ │ ├── gates/ │ ├── fusion/ │ ├── ranking/ │ └── context/ │ ├── cognition/ │ ├── novelty/ │ ├── decay/ │ ├── association/ │ ├── consolidation/ │ └── mental_models/ │ ├── governance/ │ ├── admission/ │ ├── review/ │ ├── promotion/ │ ├── privacy/ │ └── retention/ │ ├── agents/ │ ├── templates/ │ ├── specialists/ │ └── orchestration/ │ ├── executive/ │ ├── tasks/ │ ├── checkpoints/ │ ├── reminders/ │ └── coordination/ │ ├── operations/ │ ├── fsck/ │ ├── reconcile/ │ ├── rebuild/ │ ├── health/ │ └── eval/ │ ├── api/ ├── mcp/ ├── cli/ └── web/ 

Core design principle

The most important implementation boundary remains:

CANONICAL / AUTHORITATIVE Markdown Evidence Semantic mutation events Interaction/audit events ↓ derive/rebuild PROJECTIONS FTS Vectors Graphs Temporal indexes Clustering Decay Salience Utility Learned ranking state Analytics 

Classes representing canonical truth should therefore never depend upon a particular retrieval database.

Indexes can disappear.

Embeddings can be regenerated.

Graphs can be rebuilt.

Rankers can change.

Canonical memory and authoritative history survive.