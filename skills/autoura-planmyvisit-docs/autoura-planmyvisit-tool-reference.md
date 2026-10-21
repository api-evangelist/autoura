# PlanMyVisit selected tool reference

[Operating guide](../../skill.md) · [Authentication](authentication.md) · [Tool reference](tools.md) · [Concepts and trust](concepts.md)

This is a selected capability reference, not a complete catalogue or a fixed schema. Discover the current tool set with MCP `tools/list` (or the available WebMCP tools) at each session start. Follow the live schemas and permissions. Use the operating guide for call order; create a plan only after checking whether the visit already has one.

### Venue & Knowledge Tools

#### `visit_venues_search`

Searches for real-world visitor venues (museums, landmarks, attractions, parks, markets, etc.) near a given
latitude/longitude. Returns a list of candidate venues with their `visit_id`, name, location, summary, and distance.

Use this when resolving a venue for a new visit or a venue question without a known `visit_id`. When continuing an
existing plan, retrieve that plan first and use its venue information. Do not hard-code coordinates; approximate
lat/lng from general knowledge is sufficient.

#### `visit_knowledge_get`

Returns rich, structured knowledge for a single venue identified by `visit_id`. Includes highlights, suggested moments,
guide recommendations, accessibility notes, practical tips, and typical visit duration.

Use this to answer questions like:

- “what should we do there?”
- “how long do we need?”
- “is it family friendly?”

Also use it to inform plan creation.

#### `characters_search`

Returns a curated list of available AI-powered guides (characters) that can lead a visit. Each entry includes a
`character_id`, name, summary, and photo.

Only call this if the guest explicitly requests a different guide beyond the one suggested by `visit_knowledge_get`.

Every visit plan requires a guide — do not offer the option to visit without one.

### Preference Tools

#### `preferences_get`

Returns the human's complete, self-attested Autoura preferences, including dietary needs, allergies, accessibility and
mobility requirements, budget, pace, food preferences, group context, and other personalisation signals.

Call this early in the conversation before generating personalised plans or recommendations. Also call it whenever food,
drink, accessibility, mobility, pacing, budget, comfort, or other personalisation requirements may affect the answer.

Call it at most once per conversation and reuse the result for up to 24 hours unless preferences are updated.

Always set `lang` to match the human's current conversation language.

The preferences always belong to the authenticated human, not companions mentioned in a visit plan.

#### `preferences_update`

Partially updates the authenticated human's Autoura preferences.

Use this after authentication, current tool discovery, and `preferences_get`, when there are known ongoing changes. Save as many relevant
preferences as the human has explicitly provided or the agent already knows reliably.

The tool supports partial updates:

- only supplied fields are changed
- omitted fields remain unchanged
- do not send guessed or placeholder values
- array fields replace the complete existing array when supplied
- do not clear an existing requirement unless the human clearly requests that change

Use `preferences_update` immediately when the human clearly communicates or corrects an ongoing preference, such as:

- an accessibility or mobility requirement
- an allergy or dietary requirement
- a food preference
- their usual activity level or pace
- their preferred comfort or spending level
- something they generally want to avoid
- an ongoing interest that may improve future recommendations

Do not save temporary instructions as permanent preferences. A request that applies only to one meal, one day, or one
visit should normally remain in the conversation or visit plan instead.

Do not infer sensitive information. In particular, do not infer allergies, medical or accessibility requirements,
dietary or religious requirements, LGBTQ+ context, or personal identity.

After a successful update, use the new values for the rest of the conversation. The updated preferences also improve
personalisation in future conversations.

#### `personal_data_export_request`

Requests a JSON export of the authenticated human's personal data, including saved preferences and the privacy/access
log. Use this only after the human explicitly asks for the export and confirms that it should be emailed.

The export is sent to the verified primary email address already held for the account. The tool does not return the
export, the email address, or other personal data in the conversation.

### Companion Tools

Companion tools let an authenticated human use their AI to find permitted friends and current companions, inspect an
accessible companion profile, and coordinate shared visits. A companion's preferences are returned only when the
relationship and sharing permissions allow it. Precise location is available only for supported current-companion
views when the required location sharing and account permissions are enabled; never imply access that the tool did not
return.

You can list incoming invitations, invite an existing friend to become a current companion, accept a supported
incoming friend or current-companion invitation, end current-companion status, and block, unblock or disconnect a
supported relationship. Follow the live schema and obtain explicit human confirmation before sending or accepting an
invitation, blocking or disconnecting someone, or making another change that affects a relationship or location
sharing. Clearly identify the person, proposed relationship and duration where relevant before acting.

Do not infer consent or treat friendship as permission for precise location access.
Use only the companion data returned for the authenticated human's current access scope.

### Visit Plan Tools

#### `visitplan_new`

Creates a new visit plan for a specific venue and date. First check whether the intended visit already has an
accessible plan; use that plan rather than creating another when changing conversations or services.

A plan organises pacing, sequencing, and decision-making for a single visit window. It is not a booking and creates no
commercial commitment.

Requires `visit_id` (for supported venues) or a `title` (for unsupported venues), a start date, group context,
objective, structure, and constraints.

Every plan requires a guide. Prefer the suggested guide. An empty `character_id` selects the visit's active calculated
guide, or Sahra when there is no usable visit guide (including no visit ID), and saves that choice on creation.
This is independent of the profile's conversation character; do not offer an unguided visit.

#### `visitplan_get`

Returns full details for a single visit plan by `visitplan_id`.

Includes the plan core (dates, state, language), content (objective, structure, constraints, moments, gotchas), venue
snapshot, and character snapshot.

Use when you have a `visitplan_id` and need to display the plan or use one of its supported interactions. Each moment includes a single shared `done` state seen by everyone. Older plans with a blank stored guide resolve the same visit-first fallback when read, without rewriting the stored plan. A character snapshot can still be null when character data is unavailable; do not interpret null as an unguided visit.

#### `visitplans_search`

Returns a lightweight list of the guest's visit plans — use it to find the plan to continue before fetching full
details. By default it returns relevant non-completed plans without an explicit date filter. Explicit local-date
searches include all lifecycle states, including completed plans; follow the live schema for date filters.

Does not include objective, structure, constraints, moments, or gotchas (use `visitplan_get` for those). Guide snapshots use the same visit-first fallback as full plan retrieval and may be null when character data is unavailable.

#### `visitplan_update`

Updates an existing visit plan (in `draft` or `active` state). Setting `state` to `active` begins an imminent visit and enables in-visit functionality. The lead or a confirmed participant can activate it within the permitted window while preparing to leave or travelling to the venue; they do not need to have arrived at the venue. Send `state: active` by itself, without other plan updates.

Multiple visit plans may be active at the same time, including several visits on one day or visits on adjacent days. Activation belongs to each plan independently; activating one plan does not deactivate or block another.

Supports partial updates — only fields provided are changed. Omit `character_id` to keep the guide; an empty value
selects and saves the visit's active calculated guide, or Sahra if unavailable. An explicit guide must be active.

Apply updates immediately when the user provides new information that maps clearly to a supported structural field, such
as a new date, group context, or guide. No confirmation is needed unless the change is high-risk (dates or language).

After creation, adjust preferences through the supported preference update fields (`preferences_update`), adjust
visit-specific priorities or outcomes through the supported outcome fields (`visitplan_member_outcomes_update`), and add
a note (`visitplan_note_new`) for other shared conversational planning input such as questions, commitments, concerns,
timing, food, travel and admission context.

When preferences or outcomes are updated through those supported tools, the plan will update itself.

Moments, to-dos and gotchas are calculated, read-only outputs rather than create or update inputs.

#### `visitplan_member_outcomes_update`

Updates a person's priorities for this specific visit. A person may update their own priorities; the current plan
lead may update any person's priorities. These are visit-specific outcomes, not lasting profile preferences.
Send only supported fields being changed, following the live schema's provenance rules and required
`visitplan_update_key`. Retrieve the current plan when authorised and honour any `refresh_required` response.

#### `visitplan_moment_update`

Marks one calculated moment done or not done while the Visit Plan is active. Any active attendee may change this one
shared completion state, and everyone sees the same result. This tool changes only completion status; never use it to
change moment text, timing, ordering, role, ownership or any other calculated detail.

#### `visitplan_note_new`

Adds a shared conversational note to an existing Visit Plan. After initial creation, use notes for shared conversational
planning input that does not map to supported preference or outcome fields, such as questions, commitments, concerns,
timing, food, travel and admission context.

#### `visitplan_delete`

Permanently deletes a visit plan.

Allowed for plans in `draft` or `completed` state.

This action is destructive and cannot be undone.

### Feedback & Gap Reporting Tools

#### `content_gap_report`

Reports missing, vague, or incorrect venue information from `visit_knowledge_get` — for example absent opening hours,
accessibility details, pricing, or on-site logistics.

**Must be called** whenever you cannot answer a venue-specific question confidently without hedging or relying on
external knowledge.

Send as soon as the gap is identified; do not wait until the end of the conversation.

#### `product_gap_report`

Reports platform capability limitations — any moment where you tell the guest something cannot be done.

**Must be called** before offering a workaround.

Records raw “can't” moments as product signal; evaluation and prioritisation happen later.

