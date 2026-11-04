# Settle_AI

# Settle

**Purchase-Aware Household Orchestration for Proactive Home Preparation**

**Hanyang University · Fall 2026**  
`CSE406-SW-Projects-2026` · `ITE351-AI-Projects-2026`

Settle is a university project exploring how a smart home can prepare for incoming purchases before the user has to manually configure routines. A user-shared order is converted into structured items, combined with bounded household context, compared with similar purchase-intent cases, and interpreted to determine whether the home has any meaningful work to do.

The current direction is intentionally conservative about action scope. **Most orders should result in no home action.** Language models may support extraction, intent reasoning, and task proposal, while deterministic software and the home-side execution layer retain authority over device capabilities, safety constraints, execution, and feedback. Observed outcomes can later become household memory for future episodes.

> **Project status:** Academic prototype and proposed architecture. Settle is not an official LG Electronics product and does not claim production access to LG ThinQ appliance-control interfaces.

## ITE 351: AI & Applications — Group Project Proposal

### Team Members

| **Name** | **Organization** | **Email** |
| --- | --- | --- |
| Jeewon Lee | Department of Information Systems, Hanyang University | leejeewon916@gmail.com |
| Uijin Lee | Department of Information Systems, Hanyang University | smilelejn@gmail.com |
| Seojin Lee | Department of Information Systems, Hanyang University | leeseojini0303@gmail.com |
| Yejoon Kim | Department of Information Systems, Hanyang University | yejoonkim1340@gmail.com |

### Research Idea

**Title: Foundation-Model Synthetic Knowledge for Purchase-Intent Inference in Proactive Smart Homes**

Online purchases can reflect household situations such as a birthday party, ordinary replenishment, or no special intent. However, real household purchase-intent data is difficult to obtain before deployment. We propose using multiple foundation models to construct a synthetic knowledge base of diverse household purchase cases, where each case jointly represents an order, household context, purchase intent, and situation. Rather than fixing a small intent taxonomy in advance, we will first generate broad cases, inspect their distribution, and organize recurring semantic patterns afterward. Generated cases will be filtered and cross-validated using models from different providers and sampled human review. At runtime, similar cases will be retrieved as evidence for an LLM that interprets a new order. We will compare retrieval-grounded inference against the same model without the synthetic knowledge base and evaluate intent/no-intent accuracy, bundle identification, consistency, and unsupported inference. The resulting AI component will be integrated with a separate software-engineering prototype that maps inferred situations to bounded smart-home actions.

## CSE 406: Software Engineering — Settle

The software engineering project treats one incoming order as the beginning of a bounded household episode. The current requirements define the responsibilities from order intake through context assembly, intent interpretation, household-task generation, execution, observation, and memory. The next work refines those responsibilities into implementation-level specifications and interfaces.

### Conceptual Flow

```text
Shared Order
    -> Order Extraction / Item Structuring
    -> Household Context Assembly
    -> Retrieval from Purchase-Intent Knowledge Base
    -> Intent / Situation Judgment
    -> Household Task Generation
    -> Plan and Notification
    -> Home-Side Execution / Simulator
    -> Execution Feedback and Observation
    -> Household Memory
```

### Main System Boundaries

- **Purchase-Intent Knowledge Base:** population-level prior built from generated and reviewed order–intent–situation cases.
- **Settle Service:** assembles context, retrieves similar cases, interprets intent, and generates bounded household tasks.
- **Settle Home:** checks current device state and capability, executes permitted tasks, and returns command/readback results.
- **App:** supports order sharing, notifications, action disable/stop controls, and explanation of what Settle is doing.
- **Simulator / Devices:** exposes the same logical execution interface for the academic prototype, whether the appliance is simulated or physically connected.

## Representative Scenario

The primary prototype scenario is a child's birthday party. An order containing party decorations, cupcake ingredients, drinks, and unrelated routine items is combined with calendar context. Settle should identify the relevant bundle, leave unrelated items outside the inferred situation, prepare feasible household tasks such as cleaning, bounded child-safety settings, atmosphere changes, and oven standby, and record what actually happened after execution.

The scenario is a demonstration target, not a claim that every purchase should trigger automation.

## AI / Software Separation

Settle does not treat an LLM response as appliance authority.

- LLMs may perform structured extraction, semantic intent judgment, and task proposal.
- Retrieval supplies similar cases as evidence rather than treating the nearest case as the answer.
- Code validates schemas, device capabilities, supported ranges, and prohibited actions.
- The home-side runtime checks current physical state before execution and reports results afterward.
- User stop/disable controls remain available.

## Project Progress

The project has completed its initial problem framing and requirements work and is currently moving into specification, data-pipeline prototyping, system-boundary definition, and implementation planning. Current work includes:

- pilot generation and review of the purchase-intent knowledge base,
- runtime intent-retrieval and judgment contracts,
- service / home / simulator interfaces,
- app and demonstration flow,
- evaluation design for AI inference and end-to-end behavior.

## Scope and Limitations

Settle is an academic prototype. Synthetic data is used to explore cold-start knowledge construction and does not establish real household frequency or user preference. AI-generated labels and judgments require validation. Real LG appliance integration may be replaced by clearly labeled simulation where interfaces are unavailable. High-risk, irreversible, or money-spending actions are outside the intended autonomous-action scope.

*LG, ThinQ, and ThinQ ON are trademarks of their respective owners. This project is an independent academic project conducted in an LG-related course context.*
