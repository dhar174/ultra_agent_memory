A Day in the Life of Ultra Agent Memory

A fictional end-to-end memory workflow

Imagine an AI coding agent named Nova is working with a fictional user, Maya, on a project called Helios, a self-hosted team collaboration application.

Helios lives in:

~/projects/helios

and has its own Git repository.

At 2:14 PM, Maya opens a new coding-agent session and says:

“The production login flow started randomly logging users out after yesterday’s deployment. Can you figure out what happened and fix it?”

That one request activates nearly the entire memory system.

Session Start: Before Nova Even Answers

The session-start hook fires.

The system knows:

Agent: Nova User: Maya Project: Helios Repo: ~/projects/helios Branch: main Current commit: 8c9142f Session: 2026-09-18T14-14-03 

Before Nova reasons about Maya’s request, a Session Initialization Agent creates the new session scope:

Sessions/ └── 2026-09-18T14-14-03/ 

It initializes canonical session memory for:

Summary Facts Observations Proposed Learnings Evidence Working Memory Temporal Events Attempts Failures Tool Results Experiments Outcomes Patterns Preferences Mental Models Learned Conventions 

Nothing meaningful has happened yet, so most of these files or sections are empty.

The initialization itself is recorded in the authoritative interaction/audit event history.

Identity and Scope Resolution

Before searching memory, the system determines what Nova is actually allowed to see.

A Scope Resolution Agent derives:

principal_agent = Nova current_user = Maya current_project = Helios current_repo = helios current_branch = main current_commit = 8c9142f current_session = 2026-09-18T14-14-03 

The current applicable scopes therefore include:

Universal User/Maya Persona/Nova Projects/Helios Current Session 

Other users' private memories do not qualify.

Another agent's private Persona memories do not qualify.

Memories applicable only to another Git branch do not qualify.

Expired, retracted, or superseded memories may remain searchable for historical reasoning, but they cannot silently masquerade as current truth.

Query Understanding

Maya's request is handed to a Query Decomposition Agent.

Instead of treating the request as one blob of text, it identifies several implied questions:

1. What changed in the last deployment? 2. Have authentication/session problems happened before? 3. Are there project requirements concerning login/session behavior? 4. Are there known Helios gotchas involving tokens or cookies? 5. What deployment procedure does this project use? 6. What work was recently performed on authentication? 

The retrieval planner turns these into parallel searches.

Retrieval Begins

The system does not simply embed Maya's sentence and find the nearest vectors.

Several retrieval channels run.

One lexical search queries FTS5/BM25 for terms such as:

login logout session token cookie authentication deployment 

An optional embedding channel searches semantic similarity.

A graph retrieval channel starts from entities such as:

Helios AuthenticationService JWT production 

A temporal channel asks:

What relevant memories were created or updated near yesterday's deployment?

A Git-aware channel asks:

Which memories are linked to files changed between the previous production commit and 8c9142f?

A failure/gotcha channel specifically searches:

Project/Helios/Gotchas Project/Helios/Failures Project/Helios/Errors 

A requirement channel searches:

Project/Helios/Requirements 

And an LLM-based query-expansion specialist generates related concepts:

refresh token access token cookie expiry session duration token rotation SameSite secure cookie JWT lifetime 

This semantic expansion works even if the embedding service happens to be unavailable.

Security and Applicability Filtering

Each retrieval channel may initially find dozens of candidates.

But none of those candidates are allowed to enter fusion yet.

Every candidate first passes the Security / Applicability Gate.

One candidate says:

“Staging authentication uses 15-minute tokens for automated testing.”

But its scope says:

environment: staging 

Nova is debugging production.

It is rejected as currently inapplicable.

Another candidate is linked to:

branch: experiment/passkeys 

Nova is on main.

Rejected from current operational context.

Another historical memory says:

“Helios access tokens expire after 30 minutes.”

But its validity interval ended three months ago.

It remains available for historical reasoning but is marked:

superseded not-current 

Only after permission, privacy, scope, temporal, branch, commit, and disclosure checks does a candidate become eligible for ranking.

Retrieval Fusion

Now the surviving candidate streams are combined.

Suppose the most interesting candidate is:

Memory ID: HELIOS-GOTCHA-0028 Type: Project.Gotcha Summary: Environment variable AUTH_TOKEN_TTL must be specified in seconds, although the deployment template historically documented it in minutes. 

Why did it rank highly?

BM25 rank: 4 Dense semantic rank: 7 Graph rank: 2 Same-project rank: 1 Git-linked-file rank: 3 Failure retrieval rank: 1 

Reciprocal Rank Fusion places it near the top.

Its ranking is further affected by:

high certainty same-project scope high previous usefulness strong evidence moderate access frequency no meaningful decay 

A learned reranker then scores it highly.

Another memory surfaces:

Decision HELIOS-ADR-0019 Authentication configuration should remain environment-driven rather than hardcoded because staging and production require different expiration policies. 

And a recent session memory says:

Yesterday Nova helped Maya refactor deployment.yaml to standardize environment variable names. 

Interesting.

The Context Compiler Takes Over

Nova still does not receive all retrieved memories.

The Context Compiler constructs a bounded package.

It gives Nova:

Hard requirements Authentication config must remain environment-driven. Recent project state deployment.yaml was edited yesterday. Known gotcha AUTH_TOKEN_TTL is interpreted as seconds. Relevant historical decision environment-specific TTL values are intentional. Recent session evidence yesterday's deployment refactor changed variable names. 

It also includes exact evidence references.

Nova now has perhaps 1,700 highly relevant tokens instead of 40,000 tokens of loosely related memory.

Nova Investigates the Repository

Nova responds:

“Yesterday’s deployment work touched authentication configuration, and there is an existing Helios gotcha around token TTL units. I’m going to compare the deployment diff against the currently deployed auth configuration first.”

That statement is recorded as part of the interaction history.

Nova runs:

git diff HEAD~1 -- deploy/ 

The tool result shows:

- AUTH_TOKEN_TTL_SECONDS=1800 + AUTH_TOKEN_TTL=30 

There it is.

The deployment refactor changed both the variable name and the apparent unit.

The application interprets:

AUTH_TOKEN_TTL=30 

as:

30 seconds 

not 30 minutes.

Evidence Capture

The tool result becomes an immutable evidence object.

Evidence ID: EV-20260918-0417 Source type: Git Repository: Helios Commit: 8c9142f File: deploy/production.env Lines: 18–19 Captured by: Nova 

The actual source content is hashed.

Another evidence object records the code that parses the variable:

Evidence ID: EV-20260918-0418 Source type: Repository Code File: src/auth/config.ts Symbol: loadAuthConfig 

It proves that AUTH_TOKEN_TTL is interpreted as seconds.

Claim Extraction

A Fact Extraction Agent does not save an entire paragraph as one indivisible truth.

It produces claims.

Claim C1: Production currently sets AUTH_TOKEN_TTL to 30. Claim C2: Helios interprets AUTH_TOKEN_TTL as seconds. Claim C3: The previous production configuration represented 30 minutes as 1800 seconds. Claim C4: The deployment refactor changed AUTH_TOKEN_TTL_SECONDS=1800 to AUTH_TOKEN_TTL=30. 

Each claim points to its supporting evidence.

Prediction-Error and Novelty Gating

Before creating new memories, the system asks:

Is this actually new?

C2 is nearly identical to the existing gotcha.

The system does not create another duplicate memory.

Instead, the new evidence strengthens:

HELIOS-GOTCHA-0028 

Its evidence coverage increases.

Its certainty increases.

Its access/usefulness statistics increase because it materially contributed to diagnosing the issue.

But C4 is new.

The system proposes a new factual project memory:

“The September 18 deployment refactor incorrectly converted a token TTL value from seconds representation to an unchanged numeric value interpreted as seconds.”

Contradiction Checking

Before saving that claim, a Contradiction Agent searches for evidence against it.

It finds yesterday's session observation:

“Deployment variable rename appears behavior-preserving.”

That observation now conflicts with stronger evidence.

It is not deleted.

Instead it becomes:

status: superseded superseded_by: HELIOS-OBS-0104 

Its historical validity remains intact:

At the end of yesterday's session, Nova believed the refactor was behavior-preserving.

The system therefore preserves both:

what was believed then 

and:

what is believed now 

Nova Fixes the Bug

Nova changes:

AUTH_TOKEN_TTL=30 

to:

AUTH_TOKEN_TTL=1800 

Then runs tests.

Authentication tests pass.

A production-like container test confirms that access tokens now receive a 30-minute lifetime.

Those results become additional evidence.

Outcome Memory

The session now has a complete causal chain:

deployment refactor ↓ TTL value changed from 1800 to 30 ↓ application interpreted value as seconds ↓ tokens expired after 30 seconds ↓ users appeared to be randomly logged out ↓ TTL corrected to 1800 ↓ tests passed 

This becomes an Outcome and a causal relationship chain.

The graph receives edges such as:

Commit 8c9142f CAUSED Failure F204 Failure F204 CAUSED_BY Configuration C88 Configuration C88 FIXED_BY Commit 9f781ab Gotcha G28 PREDICTED Failure F204 

A New Observation Is Proposed

An Observation Agent notices something broader:

“Configuration refactors that rename environment variables can accidentally preserve numeric values while changing their units.”

That is an observation, not yet a universal truth.

It is stored with supporting evidence from this episode.

A Proposed Learning Appears

A Learning Agent goes one step further:

“When renaming configuration parameters that encode units in their names, explicitly verify whether the replacement parameter preserves the same unit semantics.”

This resembles a reusable procedure or convention.

But one incident is weak evidence for a universal rule.

So it is initially stored as:

Project/Helios/ProposedLearning 

not:

Universal/Rules 

Project Procedure Evolution

The existing Helios deployment procedure currently says:

1. Update deployment variables. 2. Run integration tests. 3. Deploy staging. 4. Deploy production. 

A Procedure Evolution Agent proposes:

Before deployment: Compare renamed configuration variables for semantic changes, including units, defaults, type conversions, and interpretation. 

The procedure is not silently rewritten.

The proposal enters review.

If accepted, it becomes a new version of the Helios deployment procedure.

The previous version remains in history.

Task Continuity

During the fix, Nova notices another issue:

There are no automated tests asserting token TTL configuration units.

A TODO is created:

TODO-HELIOS-044 Add configuration-unit regression tests for authentication TTL. Status: queued Project: Helios Created during: current session Depends on: none 

The memory that discovered this issue contains:

task_id: TODO-HELIOS-044 

The system validates that this task object actually exists.

Checkpoint

Before finishing, Nova creates a durable checkpoint:

Session: 2026-09-18T14-14-03 Project: Helios Branch: main Current commit: 9f781ab Completed: - identified TTL regression - corrected production config - ran authentication tests - verified production-like token lifetime Remaining: - add explicit configuration-unit regression tests Relevant memories: - HELIOS-GOTCHA-0028 - HELIOS-ADR-0019 - HELIOS-OBS-0104 Relevant task: - TODO-HELIOS-044 

If Nova disappears five seconds later, another agent can resume from here.

Session-End Review

Maya says:

“Great, that fixed it. Let's stop there for today.”

The session-stop hook fires.

Several specialist agents independently review the session.

The Session Summary Agent writes a compact narrative.

The Fact Agent extracts accepted facts.

The Outcome Agent records the successful fix.

The Failure Agent records the configuration regression.

The Evidence Agent verifies citations.

The Preference Agent finds nothing meaningful.

The Requirement Agent finds no new user requirement.

The Procedure Agent identifies the deployment-check proposal.

The Pattern Agent notices the configuration-unit pattern.

The Mental Model Agent checks whether any standing project model needs updating.

The Contradiction Agent verifies supersession relationships.

The Dedup Agent ensures no unnecessary duplicate memories are being created.

Finally, an Audit Agent reviews the proposed memory changes.

Canonical Writes

Approved memories are written to Markdown.

For example:

Projects/ └── Helios/ ├── Gotchas/ │ └── auth-token-ttl-units.md │ ├── Failures/ │ └── 2026-09-18-token-ttl-regression.md │ ├── Observations/ │ └── config-renames-can-change-unit-semantics.md │ ├── Outcomes/ │ └── 2026-09-18-auth-session-fix.md │ ├── Procedures/ │ └── deployment.md │ └── Todos/ └── TODO-HELIOS-044.md 

The session summary goes under:

Sessions/ └── 2026-09-18T14-14-03/ 

The Markdown is canonical.

Projection Update

The canonical mutation journal records the writes.

Projection workers update:

FTS5 vector index entity index temporal index claim index graph projections code-anchor index 

Each projection records the canonical version it has processed.

The system can now report:

Canonical version: 8124 FTS: 8124 ✓ Vector: 8124 ✓ Graph: 8124 ✓ Temporal: 8124 ✓ Claims: 8124 ✓ 

Interaction History Is Preserved Too

Separate from the Markdown changes, the authoritative interaction log has recorded events such as:

memory retrieved memory included in context memory used by Nova memory cited tool invocation tool result memory strengthened task created checkpoint created user confirmed successful outcome 

This matters because some derived values now change.

For example:

HELIOS-GOTCHA-0028 retrieved_count: +1 used_count: +1 successful_outcome_count: +1 utility_score: increased access_frequency: increased 

Those values do not need to clutter the canonical Markdown.

They are derived projections.

But because the underlying interaction events are authoritative, those projections can be reconstructed later.

Three Months Later

Now imagine a completely different session.

Maya tells Nova:

“The billing worker started timing out after we renamed some environment variables.”

Lexically, this has little to do with authentication.

But several memory mechanisms activate.

The Pattern Agent recognizes:

renamed environment variables + behavior changed 

Spreading activation moves through the project graph.

Retroactive salience gives additional weight to the earlier Helios authentication episode.

The old observation surfaces:

“Configuration refactors that rename parameters can accidentally preserve numeric values while changing semantic units.”

Nova now asks immediately:

“Did any renamed variables previously encode units in their names, such as seconds, milliseconds, bytes, or counts?”

They discover:

WORKER_TIMEOUT_MS=5000 

had become:

WORKER_TIMEOUT=5000 

but the replacement variable expects seconds.

The earlier authentication episode has now predicted a second failure.

Belief Promotion

The system now has two independent episodes:

Authentication: seconds-name removed → semantic unit error Billing worker: milliseconds-name removed → semantic unit error 

The evidence is considerably stronger.

The project-level observation may be promoted to a project belief:

“Helios configuration refactors involving unit-bearing variable names are a recurring source of semantic regressions.”

The evidence graph records both incidents.

Mental Model Update

One standing mental model asks:

“What classes of changes are particularly risky in Helios?”

Previously it said:

Database schema migration Authentication changes Production secret rotation 

After accumulating these episodes, it becomes:

Database schema migration Authentication changes Production secret rotation Configuration renames that alter or obscure unit semantics 

That mental model is not magic truth.

Its derivation remains traceable:

Mental Model ↓ Belief ↓ Observations ↙ ↘ Episode A Episode B ↓ ↓ Evidence Evidence 

Possible Universal Promotion

Months later, suppose the same pattern appears across five unrelated projects.

Only then might the system propose promotion into:

Universal/Procedures/ configuration-refactor-safety.md 

with a reusable rule:

When changing configuration keys, explicitly compare names, units, defaults, types, ranges, parsing behavior, and fallback semantics rather than assuming a rename is behavior-preserving.

That proposal faces a much stricter evidence gate because it would become universally applicable.

If approved, every future agent and project can benefit from it.

What the System Has Actually Learned

The original session began with:

“Users keep getting logged out.”

The lasting memory system ultimately produced:

Immutable evidence ↓ Atomic claims ↓ Episode ↓ Failure ↓ Outcome ↓ Observation ↓ Repeated evidence ↓ Belief ↓ Mental model ↓ Procedure ↓ Potential universal skill/rule 

Yet the original evidence was never overwritten.

The system can still answer:

“What did Nova believe immediately after the first deployment?”

“When did the team discover that belief was wrong?”

“Which evidence caused the correction?”

“Which later incidents strengthened the generalized belief?”

“Why is this procedure now injected into configuration-refactor tasks?”

And every answer can walk backward through the lineage.

That is the core idea behind Ultra Agent Memory:

not merely remembering text, but preserving evidence, experience, interpretation, causality, utility, history, and learned behavior as distinct but connected forms of memory.