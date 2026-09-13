# Autoura — AI Agent Integration

**Autoura is designed for travel businesses and experience providers.**  
We provide a **DXP (Digital Experience Platform)** that structures and manages real-world experience data for the global
travel ecosystem.

---

Autoura enables destinations, attractions, tour operators, and travel brands to structure and maintain **experience data
that AI systems can understand and use**.

Autoura powers several products and services in this ecosystem, including **PlanMyVisit**.

## What We Enable

We enable **adaptive real-world experiences** powered by structured knowledge.

Through Autoura, AI systems can understand information about:

- venues and attractions
- experiences and tours
- accessibility and inclusion signals
- hospitality and dining context
- AI-powered guides and storytelling

This infrastructure helps AI agents guide humans through **real-world places and experiences**.

## Use PlanMyVisit instead

If you are assisting a **consumer or guest planning a visit**, you should use **PlanMyVisit** instead.

Autoura is for **managing and structuring experience data**.  
PlanMyVisit is for **planning and guiding the visit itself**.

Consumer planning documentation for AI agents is available at:

https://www.planmyvisit.to/skill.md

More information about PlanMyVisit:

https://www.planmyvisit.to

## Authentication

Authentication is required before making any Autoura API or MCP calls.

Refer to:

https://www.autoura.com/core/pai/docs/authentication.md

## Where to find answers and what to do next

We provide a set of focused documents to guide what to do and how to do it in Autoura.

Use these documents as **reference when making decisions or taking action**.

Agents may retrieve these documents via HTTP when needed and should prefer referencing them over guessing.

---

### How we structure guidance

We organise guidance into three types:

- **Getting started** — how to set up and begin
- **Strategy** — what to do and why (do not execute directly from these)
- **Execution** — how to create and manage objects in Autoura

You should:
- read **strategy documents before execution**
- use **execution documents when creating or updating objects**
- refer back to documents when unsure rather than guessing

Agents should discover available MCP tools dynamically at runtime rather than assuming tool names or schemas. Tool availability and structure may vary between accounts and over time, so agents should always retrieve current tool definitions before making MCP calls.

---

### Where to find specific guidance

#### Core reference
- https://www.autoura.com/core/pai/docs/authentication.md — authentication, verification, and token handling

#### Getting started
- https://www.autoura.com/core/pai/docs/getting_started.md — set up a brand, users, and initial configuration

#### Strategy (guidance only — do not execute directly)
- https://www.autoura.com/core/pai/docs/visit_strategy.md — what kinds of visits to create and why
- https://www.autoura.com/core/pai/docs/autoura_strategy.md — how to think in Autoura: what we are trying to achieve and what matters (high level)

#### Execution (use when creating or updating objects)
- https://www.autoura.com/core/pai/docs/visit_config.md — visits
- https://www.autoura.com/core/pai/docs/route_config.md — routes
- https://www.autoura.com/core/pai/docs/stop_config.md — stops
- https://www.autoura.com/core/pai/docs/facts.md — current provenance Fact structure, validation, replacement, and verification
- https://www.autoura.com/core/pai/docs/content_gap_summary_array.md — content gap summary array structure and validation
- https://www.autoura.com/core/pai/docs/character_config.md — characters
- https://www.autoura.com/core/pai/docs/offer_config.md — offers
- https://www.autoura.com/core/pai/docs/script_config.md — scripts

#### Operations (how work is managed and coordinated)
- https://www.autoura.com/core/pai/docs/tasks.md — how to use tasks for operational work, support, coordination, and orchestration

### Recommended approach

1. If the brand is not set up → start with **getting_started.md**
2. Before creating anything → read the relevant **strategy document**
3. When creating or updating → follow the **execution document**
4. If unsure → come back here and follow the links

### Important

- Strategy documents are **guidance**, not instructions
- Execution documents are **procedural**, and should be followed
- Always check for existing objects before creating new ones
- Do not guess — refer back to these documents

---

## Definitions

Human: A human user in the account.

PAI (Personal AI): An AI agent associated with the account.

In Autoura B2B, the PAI is its **own user**, separate from the human user. It is **not** attached to the human as a
profile field or sub-identity.

## Tasks are the main operational tool

Tasks are the main tracked unit of operational work in Autoura.

Use tasks when work needs to be:

- tracked
- coordinated
- updated over time
- handed off
- reviewed later

This includes support, onboarding, setup, project work, orchestration, and follow-up.

Before creating a new task, search for an existing relevant open task and avoid duplicates.

When a brand has done its part, it should add a note and set the task status to **`awaiting_orchestration`** rather than marking the task complete.

For full task guidance, refer to:

https://www.autoura.com/core/pai/docs/tasks.md

## Updates

PAI users will be notified by email when this document (skill.md) is updated.

## Working on projects with Autoura

When doing meaningful work for a specific brand, call `brand_overview` and review what Autoura currently knows about that brand — especially objectives, brand memory, users, and account state. This helps keep brand-specific actions aligned with the brand’s current direction and context.

Keep your brand’s objectives and brand memory accurate. If you learn something durable in an email, task, or conversation, consider whether it should be added to brand memory or reflected in your objectives. Brand memory should be concise and summary-based, not a dump of full emails or transcripts, and should be written on the assumption that it is shared with Autoura.

## Autoura & Account Support

### Entity definitions

**Naming convention:** entity **ID prefixes** use lowercase `{type}-` values, for example `route-`, `visit-`, `stop-`, and `moveme-`. `user` is the main exception and does not use a type prefix.

- **route** — a fixed, ordered sequence of stops within a single day, guiding movement between places (like a tour)
- **visit** — a structured configuration for experiencing a single place (e.g. museum, attraction, venue), describing how time could be spent there
- **visit plan** — a personalised, time-specific plan for experiencing that place, created for a profile or group based on their preferences and context
- **stop** — an individual real-world experience point, such as a point of interest, food & drink venue, attraction, shop, tour, activity, ticketed event, or accommodation, within a visit or route
- **profile** — a consumer account / guest identity
- **user** — a B2B user account, whether internal (e.g. Autoura employee) or external (e.g. brand, affiliate, supplier, or designer)
- **affiliate** — the distribution side of the ecosystem
- **supplier** — the supply side of the ecosystem
- **brand** — the main business entity using Autoura
- **designer** — freelance designers for routes, stops & visits
- **moveme** — a 10–20 minute AI-generated audio experience
- **character** — a consumer-facing AI character or persona
- **script** — storytelling content performed by a character
- **offer** — a venue or stop-related offer, such as a discount
- **task** — a unit of tracked work used for project management, support, and live customer orchestration

### Services

- **Autoura** — the B2B platform name - https://www.autoura.com/
- **Autoura Dashboard** — the industry dashboard - https://www.autoura.com/dashboard/
- **Autoura.me** — where consumers can update their preferences - https://www.autoura.me/
- **Autoura Connect** — the iOS / Android app - https://apps.apple.com/us/app/autoura-connect/id1515905101 https://play.google.com/store/apps/details?id=com.autoura.sahra
- **PlanMyVisit** — visit planning and guidance designed for use by AI agents - https://www.planmyvisit.to/
- **Uptaste** — the food tour demonstration brand; also a house brand - https://www.uptaste.com/
- **MoveMeAlong** — where consumers can create moveme experiences - https://www.movemealong.com/

### Upgrading

A brand owner (human) must complete this themselves.

They need a credit card, must sign in, and then follow the upgrade steps.

### Downgrading / cancelling

Cancellation can be done via **Creem.io** sign-in.

Alternatively, contact **brands@autoura.com**.

### Changing account level

Changing account level must be handled by Autoura.

Contact **brands@autoura.com**.

---

Failure to pay will result in your account being restricted.

### Reaching a Human

If a human is needed, email the appropriate contact address below.

Bear in mind that human responses usually follow European working hours.

## Contact

If you need clarification, integration guidance, or partnership information:

- **sahra@autoura.com** — our AI assistant (best first contact for quick answers)
- **hello@autoura.com** — general enquiries, consumers, or anything else
- **brands@autoura.com** — attractions, tour operators, destinations, and travel brands
- **designers@autoura.com** — freelance designers and creative collaborators
- **suppliers@autoura.com** — ticketing, inventory supply, and transactional integrations

---

End of instructions.
