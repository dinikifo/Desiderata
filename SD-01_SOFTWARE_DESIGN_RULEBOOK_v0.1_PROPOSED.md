# SD-01 — Evidence-Governed Software Design Rulebook

**Version:** 0.1 (draft)  
**Status:** PROPOSED — not yet approved or validated  
**Owner:** Human director (`H`)  
**Governed by:** RB-01 v0.1; RB-01 Core v0.2 is a proposed simplified operating companion  
**Scope:** Requirements, architecture, components, interfaces, data, security, quality attributes, design tradeoffs, design validation, and design changes.  
**Outside scope:** Detailed code style, CI/CD configurations, incident operations, and deployment procedures (a possible future ENG-01), except where necessary to verify a design.

> **One-sentence principle:** Every consequential software design decision must connect an identified requirement to a proposed solution, explain tradeoffs and uncertainties, and identify how its suitability can be checked.

## 1. Roles and boundaries

- `H` defines product objectives, required quality and risk limits, and authorizes goal/architecture decisions or changes to authoritative state.
- `A` proposes designs or prototypes and keeps the original sources and revision pins traceable.
- `B` reviews requirements and design claims, looks for counterexamples, and may propose alternatives. `A`/`B` can rotate.
- `E` optionally checks the reasoning across design handoffs. An examiner finding is contestable and not automatic proof.
- **No AI approval by implication:** A written design, prototype result, or examiner endorsement does not authorize implementation, deployment, or changes to authoritative state.

## 2. Five software-design rules

### SD1 — Define the problem before the solution

Before committing to an architecture, record:

- target users and the job they need to complete;
- goal, scope, and explicit non-goals;
- functional requirements and acceptance examples;
- quality and constraint requirements (at least applicable security/privacy, data integrity, reliability, performance, accessibility, maintainability, cost, and operational limits); and
- inputs and assumptions, with unknowns honestly labeled `UNKNOWN`.

**Protection:** Stops building the wrong product, inventing requirements, and quietly changing success criteria.  
**Minimum evidence:** an owner-traceable brief and acceptance criteria; unresolved assumptions are called out rather than silently filled.  
**RB-01 link:** G1 (authority), G3 (fidelity).

### SD2 — Design understandable boundaries

For each significant component, identify its responsibility, inputs/outputs, interface or API contract, owned data, dependencies, failure behavior, and trust boundary. Avoid unnecessary complexity; prefer the smallest architecture that satisfies the stated requirements. Make data relationships explicit: a link or dependency in a schema does not automatically prove a semantic relationship. Do not invent relationships merely to produce an answer.

**Protection:** Limits hidden coupling, ambiguous ownership, unsafe data flows, and made-up system relationships.  
**Minimum evidence:** a component diagram or structured component inventory plus interfaces, key data flow, and dependencies.  
**RB-01 link:** G2 (supported claims), G3 (faithful translation).

### SD3 — Explain consequential choices and alternatives

For a consequential design choice, record the selected proposal, at least one realistic alternative (including a simpler option when feasible), the reasons and evidence for/against each, predicted tradeoffs, assumptions and possible failure modes, and what observation could change the recommendation. Important nonfunctional tradeoffs must remain visible (e.g., latency versus correctness, simplicity versus extensibility). Avoid treating an AI's confidence or a formal-looking argument as proof.

**Protection:** Limits groupthink, premature architecture lock-in, and unexplained design choices.  
**Minimum evidence:** a lightweight design decision record (ADR), proportional to risk.  
**RB-01 link:** G2 (evidence), G4 (challenge).

### SD4 — Verify critical design assumptions

Define how each consequential requirement/design claim will be evaluated. Match methods to claims: examples and review for behavior, executable tests for implemented behavior, threat modeling for security assumptions, representative load tests for performance, fault-injection or recovery tests for resilience, and prototypes for uncertain technical feasibility. Distinguish a design review, a simulated result, an assistant's test report, and an independently inspectable executed test. A design proposal is not an implementation, and a prototype is not proof of production readiness.

If evidence cannot be obtained because the procedure forbids all safe tests, mark `PROCEDURAL_DEADLOCK` and seek `H`'s permission for a bounded disposable experiment — **not** blanket permission to change authoritative state.

**Protection:** Prevents overconfident design approval and impossible evidence requirements.  
**Minimum evidence:** acceptance matrix linking requirement → proposed verification → observed result/remaining uncertainty.  
**RB-01 link:** G2 (proof), G5 (investigability and testing).

### SD5 — Control changes and keep an exit route

Design changes must identify which requirements and interfaces change, expected benefits, regressions or sacrificed capabilities, migration/data compatibility risks, validation needed, and whether a rollback or safe fallback exists. Consequential changes require `H` authorization, distinct from permission to explore. Record rejected alternatives and failed tests; do not claim success because a diagram or document is finished. Set a review/time budget and stop or simplify a process that no longer yields useful evidence.

**Protection:** Prevents scope drift, surprise breaking changes, irreversible experiments, and endlessly revisiting architecture.  
**Minimum evidence:** a revision-linked change record plus validation outcome and decision reference.  
**RB-01 link:** G1 (authorization), G3 (fidelity), G5 (stop/fallback).

## 3. Risk-proportionate design review

**R0 — Routine design (default):** Low-risk, easily reversible choice; requirements are clear. Produce a brief decision, implement/test within existing permission, and record the outcome. No mandatory examiner or full ADR.

**R1 — Design proof gate:** Triggered by a consequential or disputed requirement, security/privacy or data-integrity boundary, high-cost or hard-to-reverse architecture, uncertain semantic relationship, significant external handoff, repeated defect, or circular evidence demand. Check the affected decision boundary, not every sentence. Record source/pin, claim, evidence, uncertainty, test, and reviewer challenge. Allow an evidence-backed rebuttal. Finish within a human-approved effort budget.

**R1-S — Strategic challenge (rare):** Only when an actual product/design direction is contested and `H` explicitly approves a bounded round. Keep RB-01's randomized, labeled straw-man mechanism, competing Toulmin arguments, and fair defense of the current goal. It yields options for `H`, not an automatic design change.

| Example | Mode | Proportional deliverable |
|---|---|---|
| Rename a private helper or adjust copy | R0 | Small change + test/review |
| Choose between two internal data structures with limited impact | R0 or bounded R1 if disputed | Short rationale + relevant benchmark if needed |
| Add a new API carrying personal data | R1 | Data-flow/trust-boundary review + contract + explicit checks |
| Replace the core persistence model | R1 | ADR + migration/compatibility assessment + isolated test |
| Abandon original user need for a narrower product | R1; R1-S only if H approves | Goal-change ledger with original and lost requirements |

## 4. Five-step software design loop

1. **Brief:** `H` states the user problem, goals, constraints, acceptance criteria, budget, and pins (or `UNKNOWN`).
2. **Propose:** `A` drafts component boundaries, interfaces/data flow, architecture options, and assumptions.
3. **Challenge:** `B` or `E` checks requirement traceability and the inference from evidence to claims, including possible counterexamples; report disagreements precisely.
4. **Investigate:** Conduct only bounded, authorized validation; preserve both successful and failed/inconclusive results. Use `NOT_ESTABLISHED` when evidence remains insufficient.
5. **Decide:** `H` accepts/rejects/defers or requests revision. Record the actual decision, versions, and evidence separately from any contemplated proposal.

## 5. Required design deliverables (scale to risk)

For a **routine** change, a task brief, implementation artifact, and relevant check may be sufficient.

For a **consequential** design decision, prepare a compact *design packet*:

1. **Problem and criteria:** user need, requirements IDs, non-goals, constraints, and acceptance examples.
2. **Context and pins:** original sources, artifact versions, external dependencies, and known unknowns.
3. **Design:** responsibilities, interfaces, data flows, trust boundaries, dependency map, and failure behavior.
4. **Decision:** chosen approach, alternatives, rationale, quality tradeoffs, and uncertainties.
5. **Validation:** proof obligations, independent or reproducible check where feasible, actual results, and untested risks.
6. **Human disposition:** `PROPOSED`/`AUTHORIZED`/`REJECTED`/`DEFERRED`/`STOPPED`, decision reference, and next bounded action.

The packet is a template, not a requirement to produce six separate documents.

## 6. Design quality checklist

Use these as prompts for applicable decisions, not mandatory paperwork for every change.

- **Requirements:** What user outcome does this design satisfy? Which original requirements remain uncovered?
- **Architecture:** What are the component responsibilities, dependencies, boundaries, and plausible simpler alternatives?
- **Data:** What data is stored or inferred? Is its meaning established? How are integrity, migration, and deletion handled?
- **Interfaces:** Are input/output formats, error handling, compatibility, and trust boundaries explicit?
- **Security/privacy:** What can go wrong across privilege, identity, and sensitive data boundaries? What mitigations are testable?
- **Reliability:** What happens when dependencies fail? How does the system recover, degrade, or roll back?
- **Performance/cost:** Which targets and representative measurements are justified by the requirements?
- **Maintainability/accessibility:** Is the design understandable, testable, supportable, and accessible where applicable?
- **Evidence:** Which claims are verified, supported, contradicted, or not established? Who actually ran which checks?
- **Authority:** What has `H` actually authorized, and what remains only a proposal?

## 7. Example design decision (illustrative; not a verified result)

**Goal G0:** Answer natural-language questions about PVS project data accurately.  
**Claim under review:** A generated Python query will answer a user's question correctly.  
**Concern:** A workflow dependency in the PVS tree does not by itself establish evidential reliance. A generated query may invent a semantic join.  
**Option A:** Direct generated Python query.  
**Option B:** Schema-aware answerability checking before generating or executing a query.  
**Review finding:** Option B is a candidate mitigation, not a verified superior design. Both options need evaluation on the same pinned test questions, including unanswerable queries.  
**Status:** `NOT_ESTABLISHED` (factual superiority); `PROPOSED` (design change).  
**Next step:** Seek `H` authorization for an offline, disposable comparison; do not mutate authoritative PVS state from this discussion alone.

## 8. Minimal Architecture Decision Record (ADR)

```yaml
adr_id: ADR-UNKNOWN
sd_rulebook: SD-01/0.1-PROPOSED
governance: RB-01/0.1
mode: R1
source_and_revision: UNKNOWN
requirements: []
design_question: ""
options_and_tradeoffs: []
proposed_choice: ""
assumptions_and_failure_modes: []
verification_plan: []
observed_evidence_refs: []
claim_status: NOT_ESTABLISHED
authorization_status: PROPOSED
human_decision_ref: null
rollback_or_fallback: UNKNOWN
```

## 9. Evaluation and stewardship

SD-01 is useful only insofar as it produces better decisions or lowers total cost compared with a simpler process. Before claiming benefits, compare a few matched design tasks under R0, R1, and a short-checklist baseline with the same requirements and independently assessed acceptance criteria. Record correctness, missed requirements, false objections, time/human effort, and useful design outcomes; include low-risk tasks where extra gates may be counterproductive. Predeclare measures and stopping rules. A small exploratory pilot informs refinement but cannot prove general superiority.

**Initial request to `H`:** approve the rulebook *for a bounded design-review pilot only*, with no implied authorization to change PVS source, deployment, product goal, or authoritative project state. Keep RB-01 v0.1 unchanged, and preserve decisions and proposed rulebook revisions by version.
