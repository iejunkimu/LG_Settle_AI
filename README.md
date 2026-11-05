# SPIRA

**SPIRA: Proactively Preparing Smart Homes via Retrieval-Augmented Purchase Intent Inference over Synthetic Knowledge**

**Hanyang University · Fall 2026**  
`CSE406` · `ITE351`

## ITE 351 · AI & Applications — Group Project Proposal

### Team Members

| **Name** | **Organization** | **Email** |
| --- | --- | --- |
| Jeewon Lee | Department of Information Systems, Hanyang University | leejeewon916@gmail.com |
| Uijin Lee | Department of Information Systems, Department of Food & Nutrition Hanyang University | smilelejn@gmail.com |
| Seojin Lee | Department of Information Systems, Hanyang University | leeseojini0303@gmail.com |
| Yejoon Kim | Department of Information Systems, Hanyang University | yejoonkim1340@gmail.com |

### Research Idea

SPIRA uses synthetic purchase-intent cases as retrieval evidence for inferring why a household placed an order. A shared order and bounded household context, such as purchase-history signals and available calendar tags, are combined with similar cases from the knowledge base. The resulting intent and situation inform household preparation tasks.

The planned knowledge pipeline uses foundation models to generate diverse purchase cases, cross-model review, and sampled human inspection. **OPEN** remains the choice between joint full-case generation and intent-seeded assembly. The shared meaning of confidence and ambiguity metadata also requires alignment. Synthetic cases support cold-start inference, while consented actual cases may later contribute evidence under the project's privacy policy.

The planned evaluation compares retrieval-grounded inference with the same model without synthetic knowledge retrieval. It examines intent/no-intent accuracy, relevant-item bundle identification, consistency, and unsupported inference. Ordinary replenishment and no special intent remain valid outcomes.

## CSE 406 · Software Engineering — SPIRA

The software-engineering specification follows one shared order through inference, preparation, execution, observation, and household memory. Service sends versioned plans and tasks to Home. Home checks current execution eligibility, dispatches logical commands through simulator or physical-device connectors, and returns observable results.

### End-to-End Flow

```text
Shared Order + Bounded Household Context
    -> Order Extraction / Item Structuring
    -> Retrieval from Synthetic Purchase-Intent Knowledge
    -> Purchase-Intent / Situation Inference
    -> Household Preparation Tasks
    -> Versioned Plan and App Notification
    -> Service-to-Home Execution Request
    -> Home Eligibility Check
    -> Simulator / Physical-Device Connector
    -> Command Result, Readback, and Effect Evidence
    -> Service Observation and User Feedback
    -> Household Memory
```

### System Responsibilities

- **Knowledge Base** maintains allowed purchase-intent cases, labels, retrieval populations, and representative evidence.
- **SPIRA Service** assembles inference context, retrieves evidence, infers intent, generates bounded tasks, and finalizes versioned plans.
- **SPIRA Home** checks current household/device state and capability, evaluates applicable local triggers, executes eligible tasks, and reports readback, override, failure, and restoration outcomes.
- **App** supports order sharing, plan notifications, decision-time explanations, whole-preparation stop, and per-task line-off controls.
- **Simulator / Device Connector** exposes logical capabilities, command application, raw events, and observable state with explicit simulated or physical provenance.

The proposed capability contract separates connector capability, current execution state, and product policy. Final ownership of derived local-event recognition and the detailed Home/core execution allocation remain **OPEN**.

### B Specification v0.1

The shared specification starts at [document/main.tex](document/main.tex), with six editable sections under [document/sections/](document/sections/). It covers scope and authority, knowledge and inference, shared planning objects, Home execution, observation and App behavior, safety/privacy, and open decisions. **OPEN** marks unresolved or conflicting source semantics. **PROPOSED** marks candidate closures awaiting team adoption.

**B Specification v0.1 / team surface — not team-final and not professor-final submission structure.**

The document uses a maintainable LaTeX `article` layout. The professor-required B hierarchy remains **OPEN**. The primary drafting source and historical product baseline are linked below. Settle 3.2 is retained as historical design provenance. [Team drafting surface](https://app.notion.com/p/40f0a6eaf42e8182a3d2c5bc7e900008), [historical product baseline](https://app.notion.com/p/40f0a6eaf42e80ada927f4f417435fa5)

### Build the Specification

The [compile workflow](.github/workflows/compile-spec.yml) runs on pushes and pull requests that change the document source or workflow. It compiles `document/main.tex` and uploads `main.pdf` as the **spira-specification** workflow artifact. Download the PDF from the corresponding GitHub Actions run.

For a local build, install TeX Live with the LaTeX base, recommended, and extra packages, plus `latexmk`, then run from the repository root.

```sh
mkdir -p /tmp/spira-specification
cd document
latexmk -pdf -interaction=nonstopmode -halt-on-error \
  -outdir=/tmp/spira-specification main.tex
```

The resulting PDF is `/tmp/spira-specification/main.pdf`. Local build output can remain outside the working tree. The CI build directory and TeX auxiliary files are ignored by Git.

## Representative Scenario

The primary demonstration target is a child's birthday party. An order containing party decorations, cupcake ingredients, and unrelated routine items is combined with calendar context. SPIRA should identify the relevant bundle, leave unrelated items outside the inferred situation, and propose feasible preparation such as cleaning, supported child-safety settings, atmosphere changes, and oven standby. Execution results record observable effects and their evidence limits.

The specification also includes proposed boundary probes for first-guest arrival, light readback, and preservation of a later manual TV change during restoration. Runtime implementation and demonstration validation are upcoming work.

## AI and Execution Responsibilities

- LLMs may perform structured extraction, purchase-intent inference, and bounded task proposal.
- Retrieval supplies representative purchase cases and decision-time evidence.
- Code validates structure, supported capabilities, ranges, timing, and product-policy restrictions.
- Home checks required current state before starting an effect and returns actual command/readback results. Missing required physical-state evidence blocks execution.
- User stop and per-task line-off controls affect preparation. Direct user changes take precedence when automation ownership and restoration are evaluated.
- Service interprets execution observations and explicit user reactions for household memory. Future task generation may use this household-specific feedback.

The current runtime inference contract uses one-shot judgment with structural validation. **OPEN** remains whether evidence/confidence thresholds should become a runtime gate. Offline dataset-quality thresholds and runtime action policy require separate decisions.

## Current Work and Open Decisions

This repository currently provides the B Specification source, README, and PDF build workflow. The next implementation work includes the knowledge pipeline, retrieval/inference contract, Service/Home/connector interfaces, App flow, and evaluation.

The specification keeps the following decisions visible for team discussion.

- Synthetic case generation method and shared confidence/ambiguity semantics.
- Runtime evidence gating and derived local-event ownership/versioning.
- Stop, stale plan, in-flight command, reconnect, and restoration ordering.
- Physical manual-change attribution and observation/memory terminal semantics.
- Exact capability/schema fields, transport, host, camera, and sensor selection.

## Scope and Limitations

SPIRA is an academic prototype proposal. Synthetic data requires validation for inference quality and household applicability. Physical integrations depend on supported interfaces, and simulated behavior carries explicit provenance. Autonomous actions are bounded by product policy, capability, current-state checks, and user control. High-risk, irreversible, or money-spending operations are outside the intended autonomous-action scope.
