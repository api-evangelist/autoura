# Visit strategy

## Type
Strategy (do not execute directly from this document)

## Purpose
Define what visit venues to create, why they matter, and how to position them effectively within Autoura.

## When to use
Use this before creating any visits.

## Knowledge base

This document is part of the Autoura knowledge base, used by the Autoura skill:

https://www.autoura.com/core/pai/skill.md

Refer to the skill for the full index and how documents connect.

---

## What this is about

A **visit** is the structured representation of how time is spent at a single place.

This document explains:

- what a visit venue represents in Autoura
- which places are suitable
- how different organisations should approach visit strategy
- how to decide what to create

This is **strategy**, not execution.

---

## Why visits matter

Visits are how your venue is represented and experienced by AI systems.

They define:

- how time is spent at your location
- how visitors are guided through the experience
- how your venue fits into a wider day or journey

Without a well-defined visit, AI systems can:

- list your venue  
  but cannot
- guide or operate the experience

This directly impacts visibility, engagement, and conversion.

---

## Public pages and data access

PlanMyVisit visit pages are intentionally not the full data product.

Public web pages can show enough information for humans to understand that a visit exists and why it matters, but they should not expose the complete structured visit configuration. The complete visit data is Autoura's value layer: curation, sequencing, decision support, recommendations, practical guidance, and the operational structure needed for AI systems to run the visit.

If that full structure is exposed directly on public web pages, third-party AI systems can copy or absorb the value without using Autoura. Public pages should therefore be treated as a deliberately limited surface, not as the main distribution channel for visit data.

The full visit data is available through the consumer MCP interface. That is the primary AI-consumable interface for Autoura visits, and it should contain the complete data needed to plan, guide, personalise, and operate the experience.

This means:

- public web pages are a human-facing discovery and trust surface
- public pages should avoid leaking the complete structured visit value to third-party AI
- the consumer MCP interface is the full data source for AI and consumer-facing experience operation
- visit configuration should be designed for MCP-enabled use, not judged only by what appears on the public page

---

## Primary representation

Primary representation means a single active organisation is responsible for configuring and maintaining a visit venue at any given time. Primary representation creates a single **authoritative version of how your venue is experienced**, ensuring AI systems consistently use your configuration.

In simple terms, primary representation means one active organisation maintains the canonical visit configuration for that venue.

This gives:

- one organisation in control
- one authoritative configuration
- a competitive position at strong venues

---

## Strategic value

A visit serves two roles:

- an **authoritative representation of a place**
- a **position within the visitor journey**

Who benefits depends on role:

- attractions define how the place is experienced
- others participate through recommendations and alternatives

---

## Experience quality

Primary representation does not mean full control of experience quality.

Autoura is responsible for the **design of the visit**:

- how time is spent at the venue
- sequencing and flow
- pacing and structure

The organisation holding primary representation is responsible for the **primary curation of the visit**:

- what matters most at the venue
- what should be prioritised or skipped
- how the experience should feel
- what makes the visit distinctive

They also contribute:

- accurate information
- local knowledge
- first-person experiences and recommendations

This ensures that:

- each visit remains coherent and well-designed
- quality is consistent across the platform
- local expertise is preserved and amplified

Where no operator or attraction input exists, Autoura still defines the visit structure to ensure the experience remains usable and valuable.

---

## Curation vs facts

The primary value of configuring a visit is **curation**, not just information.

Facts are important — but they are inputs, not the outcome.

A visit is valuable because it defines:

- what matters
- what order things happen in
- how time is prioritised
- what to include or exclude
- how the experience flows

This is curation.

Without curation, a visit becomes:

- a list of information  
  rather than
- a designed experience

AI systems can already access facts.

What they cannot do reliably without Autoura is:

- decide what matters most
- structure time effectively
- create a coherent, high-quality experience at a place

That is the role of visit configuration.

Therefore:

- **facts support the visit**
- **curation defines the visit**

---

## Suitable visit anchors

Primary representation is intended for places that can anchor a half-day or full-day visit, even if the main attraction itself only takes part of that time.

A suitable visit anchor should normally be:

- a named place visitors recognise
- substantial enough to plan around
- useful before, during, and after the visit
- rich enough to support choices, pacing, practical guidance, interpretation, food, facilities, nearby context, or follow-on recommendations

It does not have to be a single enclosed venue. It does not have to occupy the whole day by itself. But it should be strong enough that a visitor might organise a morning, afternoon, or day out around it.

A visit should normally represent one coherent place or experience.

Nearby places should not be combined into a single visit just because visitors often do them together. If two nearby attractions are separately ticketed, separately operated, or often visited individually, they should usually be represented as separate visits.

For example, Tower of London and Tower Bridge, or Edinburgh Castle and the Scotch Whisky Experience, are nearby and may work well as recommendations for each other, but they are not one combined visit.

In these cases:

- create separate visit venues
- use nearby recommendations to connect them
- let the AI help visitors decide whether to combine them in the same day

---

### 1. Anchor attractions

High-footfall attractions where visitors commonly ask:

- what to do before
- what to do after
- where to eat
- how to pace the visit
- what else nearby is worth adding

Examples:

- museums
- galleries
- aquariums
- observation towers
- cathedrals
- major monuments
- visitor centres

These may only take one or two hours as the core experience, but they can still anchor a wider visit.

---

### 2. Large multi-part attractions

Places with multiple areas, buildings, exhibits, activities, or environments.

Examples:

- castles
- zoos
- palaces
- dockyards
- large heritage sites
- theme parks
- large gardens or estates

These require:

- sequencing
- prioritisation
- pacing decisions
- practical guidance
- food and rest planning

---

### 3. Non-ticketed visit anchors

Named places that may not require a ticket, but can still anchor a morning, afternoon, or day out.

Examples:

- beaches
- large public gardens or parks
- waterfronts or promenades
- major markets or food halls
- open heritage sites
- large memorial, civic, or cultural spaces

Use cases:

- choosing where to start
- deciding what to focus on
- pacing the visit
- finding food, toilets, viewpoints, transport, shelter, or quieter areas
- adding nearby stops before or after

A place should only be represented here if a visitor could naturally plan time around it, not merely pass through it.

Smaller places, quick stops, individual cafés, small markets, single viewpoints, and short browse-and-eat places are usually better handled as stops, recommendations, or part of a route.

---

### 4. Landmark heritage or archaeological sites

Places where the value is not only practical planning, but also understanding what the visitor is seeing.

Examples:

- archaeological sites
- ruins
- ancient monuments
- battlefields
- historic landscapes
- sacred or ceremonial sites

In these cases, the value is:

- in-place interpretation
- storytelling
- guided understanding
- cultural or historical context
- practical help navigating the site

This may include:

- audio
- guides
- AI or wearable experiences

---

## What is not covered

Primary representation is not intended for:

- services that act as a base for other experiences, such as bike hire, boat hire, equipment rental, or hop-on hop-off buses
- individual food, drink, retail, or accommodation businesses, unless they are part of a wider visit anchor
- ordinary city areas, neighbourhoods, or districts, unless there is a clearly defined visitor place that can anchor the outing
- events, shows, performances, or theatre ticketing that are better handled through a separate “night out” model
- experiences that are fully predefined from start to finish, with no meaningful visitor choice
- quick stops, viewpoints, cafés, shops, or short detours that are better handled as stops, recommendations, or route elements

---

## Ticketing and bookings

Ticketing is **optional**, not required.

### Supported scenarios

- If listed on Tiqets → can support live availability and pricing
- If not listed → visit can still be fully configured
- If terms are not optimal → still proceed (focus is experience quality)
- If listing is incomplete → extend with additional experience content

### Direct booking

You may link to:

- the attraction’s own booking system
- a partner or agency

Note:

- no live availability
- no dynamic pricing
- no time-slot handling

For these, integrated ticketing is preferred.

---

## Strategy by organisation type

This section defines **how each organisation should act**, based on the strategic roles defined above.

---

### Tour operators

Focus on:

- adding **local recommendations**
- offering **alternatives and upgrades**
- inserting **high-value moments during visits**

#### Venue-first demand

The core tour-operator pitch is not that Autoura replaces the tour operator’s own website or tour catalogue.

The key question is where the visitor starts:

- some visitors start with a tour operator and then choose a tour that includes a venue
- many visitors start with the venue itself, such as Windsor Castle, Petra, or Buckingham Palace, and only then realise they want planning help, a guide, transport, access support, a better route, or a higher-quality way to experience it

Autoura helps tour operators reach that second group.

The visit is configured around the venue because that is where venue-first demand appears. The tour operator’s relevant product, guide service, private tour, transfer, upgrade, or expert help is then surfaced as the useful next action inside the visit context.

This is not the pitch for the venue, and it is not the demo pitch. For a tour operator, the pitch is:

> People who are already thinking about a venue should discover your relevant tour or guide service at the moment they need help with that venue.

This matters most when the tour operator already has a strong product connected to a known venue. The visit puts a marker down for the venue-first search/planning/visit moment, so the operator can be discovered by people who did not begin by browsing the operator’s tour catalogue.

#### Local recommendations

A nearby option that:

- enhances the visit
- fits before, during, or after
- you are commercially motivated to promote

Examples:

- nearby restaurant
- café before entry
- post-visit experience

A key example is offering a **tour guide within an attraction** as a *during* local recommendation, enhancing the visitor’s experience while they are on-site.

#### Alternatives

Different ways to experience the same venue.

Examples:

- premium or specialist experiences
- access via a different format
- tours that include the venue

Examples include:

- a tower abseil instead of a standard viewing experience
- a guided day tour that includes the venue
- a multi-day tour that includes the venue as part of a broader itinerary (e.g. Machu Picchu)

---

### Local businesses

Examples:

- restaurants
- bars
- cafés
- food shops
- retail shops
- small venues

Focus on:

- discovery
- nearby recommendations (before, during, or after a visit)
- increasing local engagement

Example:

- a visitor planning a beach visit is recommended your restaurant for lunch, motivating them to include it as part of their day

---

### Attractions

Focus on:

- maintaining accurate information
- structuring how the experience works
- guiding the visitor journey within the venue
- preparing for real-time, in-visit guidance

---

### DMOs (destinations)

Focus on:

- distributing visitors across locations
- influencing timing and flow
- ensuring coverage across visit venues

This can include:

- spreading demand across multiple venues
- influencing visit times
- shaping how a destination is experienced

Because onboarding is simple, DMOs can also:

- configure and manage visit venues on behalf of local attractions
- accelerate adoption across the destination
- ensure consistency and quality across experiences

---

## Multi-venue strategy

### When to combine

Use a single visit when:

- venues are visited together
- covered by one ticket
- experienced as a single day

---

### When to separate

Use separate visits when:

- venues are independent
- require separate tickets
- are typically visited on different days

---

### Rule of thumb

If a visitor would describe it as:

> “one day out”

…it is usually one visit venue.

---

## Routes vs visits

- Routes are **not required** to create visits
- Visits are **not required** to create routes

They are independent and can be used together or separately.

---

## Primary representation lifecycle

### Ending or pausing

- Content remains on the Autoura platform
- After 3+ months paid → held for up to 6 months
- May operate under Autoura brand
- Can be reclaimed by original organisation
- Otherwise reassigned

---

### Setup and inactivity

- Requires active configuration
- Prompts issued if incomplete
- If inactivity continues → returned to pool

Primary representation cannot be held without progress.

---

### Eligibility

You do not need to run tours to qualify.

You should:

- operate in tourism
- have strong local knowledge

If unsure → contact Autoura.

---

### Company size

Primary representation is based on:

- active participation

Not:

- company size

Existing representations are not overridden.

---

## Limits on configuration

- There is **no fixed global limit** on visit configuration
- Your limit is determined by your **account level**

This means:

- you can configure up to the number of visits allowed by your account
- you can **increase this limit by changing your account level**

The system is designed to scale with you, while keeping visits **active and maintained**.
