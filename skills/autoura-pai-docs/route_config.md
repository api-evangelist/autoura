# Route configuration

## Type
Execution

## Purpose
Define how to configure routes in Autoura once a route concept, audience, and rough journey have been chosen.

Use this when creating or updating a route.

## What this object is

A route is a fixed, ordered sequence of stops within a single day. It guides a visitor through multiple real-world places in a planned order, like a tour.

Routes define:

- the ordered stop sequence
- movement between stops
- the visitor journey across a place, district, destination, or theme
- practical pacing and transitions
- how configured stops combine into one coherent experience

## Design boundary

AI should not be treated as the final route designer.

Route design requires human judgement about the audience, commercial promise, local character, operational viability, safety, pacing, taste, and brand positioning.

AI can still do a large part of the configuration work:

- research and propose candidate stops for human review
- configure stop records
- enrich stop practical details
- check stop ordering for obvious friction
- identify gaps, risks, and missing stop types
- prepare route configuration drafts
- turn a human-designed route into structured Autoura data

In practice, configuring the stops is much of the work.

## When to create vs reuse

Routes are brand-specific.

Duplicate routes are permitted when different brands want their own version, positioning, or commercial packaging.

Create a route when:

- the brand is offering a specific journey or tour
- the route has its own audience, promise, or theme
- the ordered sequence matters
- the route should be managed, reviewed, or published as a distinct product

Do not create a route just because several stops are near each other. If there is no intentional journey, stop list, or visitor promise, capture the places as stops instead.

## Required inputs before configuration

Before configuring a route, establish:

- brand
- route name or working title
- target audience
- location or operating area
- route theme or visitor promise
- intended duration
- start point and end point
- transport mode
- rough stop order, if already known
- whether the route is circular or linear
- whether the route is guided, self-guided, audio-led, hosted, or hybrid
- any fixed commercial or operational constraints

If the human has not designed the route yet, do not pretend the route design is complete. Configure candidate stops and route-supporting evidence, then ask for route design approval or human sequencing.

## Stop configuration work

Every route stop should have a configured stop record where possible.

For each stop, capture:

- name
- location
- stop type
- short visitor-facing summary
- why it belongs in the route
- practical arrival guidance
- expected dwell time
- accessibility or mobility friction where relevant
- ticketing or booking relevance where relevant
- food, drink, toilet, rest, shelter, or viewpoint relevance where relevant
- source evidence from official or supplier-owned sources where possible

Do not rely on vague area references when a specific real place is needed. Configure the place as a stop or record why it is intentionally not configured.

## Route sequencing checks

Once stops are configured, check the route order for:

- sensible start point
- clear first-stop orientation
- reasonable walking, driving, cycling, or transit transitions
- no unnecessary backtracking
- meal, refreshment, and toilet opportunities
- weather exposure
- crowding or queue risk
- closing-time or timed-entry risks
- realistic total duration
- a satisfying end point

Flag anything that needs human design judgement rather than silently "fixing" the route.

## Practical route surfaces

Route configuration should include practical guidance for:

- arrival and meeting point
- how the visitor moves between stops
- where the route naturally slows down
- where visitors may leave early or shorten the route
- wet-weather or low-energy alternatives, if known
- safety or navigation gotchas
- accessibility considerations that materially affect the route
- what to do if a key stop is unavailable

Do not tell visitors to check external sources as a substitute for Autoura guidance. If a visitor-critical fact is unresolved, record the risk and route it through maintenance or human review.

## Quality bar

Before handoff, check:

- the route has an intentional visitor promise
- the stop list supports that promise
- stops are configured as real places, not loose labels
- order and pacing are plausible
- major obvious anchors are not missing
- food, drink, toilets, rest, and shelter have been considered where relevant
- source evidence is appropriate and not copied from third-party directories
- unresolved design decisions are clearly marked for human review

## Handoff wording

Use this framing when handing route work back to a human:

"I cannot design routes for you, but I can configure the stops, and that is much of the work. Once you confirm the route idea and order, I can turn the stops and practical guidance into structured Autoura configuration."
