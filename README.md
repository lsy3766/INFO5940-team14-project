# INFO5940-team14-project

INFO 5940 Team 14 course project: designing and building an LLM-powered product using AI-DLC v1.0.1.

> Inception is complete: requirements, user stories/personas, execution plan, Application Design and unit artifacts have been explicitly approved. The workflow stopped before Construction. No application or tests have been implemented.

## Team
| Member | NetID |
| --- | --- |
| Shuyue Li | sl3766 |
| Danying Wei | dw698 |
| Yichen Tong | yt625 |
| Yuyi Long | yl4429 |
| Shining Cao | sc3625 |

## Problem and rationale
Our cooking assistant aims to help users decide what to eat. It will consider preferences, available ingredients, serving counts, and time when proposing meals. LLMs help interpret natural-language requests and feedback, ask clarifying questions, and explain recommendations. The audience is people who cook for themselves or others and need help deciding what to eat; success criteria are defined in the [approved requirements](aidlc-docs/inception/requirements/requirements.md).

## Planned LLM integration
An agent will use explicit tools to retrieve recipes, read inventory, validate meal plans, and revise proposals based on results. Quantity calculations and inventory writes will be handled by deterministic application logic. The planned product model is Google Gemini Developer API `gemini-3.5-flash-lite`, free tier only (account access remains to be verified during Construction); OpenAI Codex is the planned development assistant, using AI-DLC v1.0.1.

## Approved scope and future feasibility
The approved product scope supports personalized five-day menus covering breakfast, lunch, and dinner, a recipe library, manually maintained inventory, shopping-list generation, and changes to meals. Users can select a dish they actually cooked, review and correct estimated ingredient usage, and confirm inventory deductions. The agent can then suggest adjustments to remaining meals. Menus are suggestions that users can dynamically adjust at any time; planning does not consume inventory. Users customize party size, and the agent proposes an appropriate combination of dishes and scaled quantities, subject to user changes. Cookware constraints are excluded.

The local technical baseline is Python 3.12, FastAPI, Streamlit, SQLite, and pytest. Recipes are retrieved from a source-attributed local collection (RAG), supplemented by supported public recipe URLs. General live internet search is outside the MVP scope. Model access, supported recipe sources and dependency versions remain future Construction verification tasks.

## Collaboration plan
The confirmed collaboration plan is two one-hour meetings each week, combining online and in-person sessions, with WeChat communication between meetings. This is a planned arrangement, not a claim that any meetings have already taken place. Work is organized into five responsibilities described in [the collaboration plan](docs/COLLABORATION.md). Confirmed responsibilities are listed below; module ownership is shared, with no exclusive coding assignments. One workflow operator runs the shared AI-DLC workflow; all members participate in questions, review, and approval. These responsibilities coordinate participation rather than split Inception artifacts into independently generated workflows.

| Responsibility | Confirmed member |
| --- | --- |
| Workflow & GitHub | Shuyue Li |
| Product & Requirements | Shining Cao |
| UX & User Stories | Danying Wei |
| Technical & LLM | Yuyi Long |
| Evaluation & Delivery | Yichen Tong |

## Documents
- [Historical product vision input](docs/inputs/vision.md)
- [Historical technical environment input](docs/inputs/technical-environment.md)
- [Preparation checklist and workflow start prompt](docs/PREPARATION.md)
- [AI-DLC release provenance](docs/aidlc-setup.json)

## Workflow status
AI-DLC v1.0.1 rules are installed in AGENTS.md and .aidlc-rule-details/; release provenance is recorded in docs/aidlc-setup.json. All required Inception artifacts are approved. See [workflow state](aidlc-docs/aidlc-state.md), [append-only audit](aidlc-docs/audit.md), [approved application design](aidlc-docs/inception/application-design/application-design.md), [approved unit definition](aidlc-docs/inception/application-design/unit-of-work.md), and [submission readiness review](aidlc-docs/inception/submission-readiness-review.md). Original inputs are retained for provenance; approved requirements/design and subsequent explicit collaboration confirmation take precedence. Construction has not started.
