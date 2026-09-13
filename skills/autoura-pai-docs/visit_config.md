# Visit configuration

## Type
Execution

## Purpose
Define how to create and manage visits in Autoura.

## When to use
Use this when creating or updating a visit.

## Knowledge base

This document is part of the Autoura knowledge base, used by the Autoura skill:

https://www.autoura.com/core/pai/skill.md

Refer to the skill for the full index and how documents connect.

---

## What this object is

A **visit** is a structured configuration for experiencing a single place (e.g. museum, attraction, venue), describing how time could be spent there.

It defines:

- how a visitor spends time at the place
- sequencing and pacing within the venue
- surrounding context (before, during, after)

A visit is the **primary representation of a place** in Autoura.

---

## When to create vs reuse

- A visit is created **once per venue across the entire platform**
- There must not be duplicate visits for the same place

Create when:

- the venue does not yet exist as a visit

Reuse when:

- the venue already exists

Note:

- consumers / guests may visit multiple times
- but the visit configuration itself is shared and authoritative

---

## Text field audience

## Summary and description formatting

The public visit page can expose `name`, `questions`, `facts`, `nibble`, `summary`, `description`, `brand_tip_summary`, `curator_why`, `comingup.summary`, `comingup.items`, and `location.name`. Treat these as public visitor-facing or curator-facing text, not internal resolver surfaces. They should be easy to scan, especially on mobile.

These public fields must never contain inline `[stop:<stop_id>]` references, bare supporting-object ids such as `food-*`, `attraction-*`, `poi-*`, or any other raw resolver syntax. If a public field names a place that needs structured resolution elsewhere, keep the public text clean and put the resolver reference in an appropriate deeper AI-facing field.

Currency in these public fields must use consumer-facing local currency symbols in natural prose, not ISO-style currency codes. For example, write `£10` for a UK visitor-facing price, not `GBP10` or `GBP 10`. Other AI-facing, operational, structured, or data-normalisation fields may use currency codes where that is clearer or required.

All visitor-facing visit fields should use consumer-facing language, not internal content/design terminology. Avoid words such as `hook`, `anchor`, `stop`, `configured`, `field`, or `surface` when they are being used as Autoura/workflow terms rather than ordinary visitor language. Say what the visitor experiences or decides instead: main thing to notice, key part of the visit, place to eat, useful choice, practical constraint, visual focus, or planning detail.

If either field is over **400 characters**, it must include at least one paragraph break: two newline characters between paragraphs. Do not leave long summary or description text as one dense block.

Use paragraph breaks to separate distinct ideas, such as what the place is, what visitors do there, and practical or experience-shaping context.

Other configurable text fields are primarily for AI agents to read and use when guiding, planning, or answering questions. These can be written in concise note form where that is clearer. For AI-facing fields, information density, specificity, and factual usefulness matter more than polished prose flow.

## Data fields, scheduled events, Coming Up, and fresh

Most visit fields are **output fields**. They should hold the current visitor-usable or AI-usable answer, not notes about how the answer was researched, where it came from, when it was configured, or what a future worker should check.

This applies to public and AI-facing fields such as summary, description, location, opening/availability, practical, getting-there, suitability, ticketing, dining, planning, recommendations, scripts, and surrounding context.

Do not put source/provenance wording in ordinary data fields, such as:

- `official site checked on...`
- `according to...`
- `source: ...`
- `configured from...`
- `check this page before travelling...`
- `use the official visit page rather than a ticket checkout...`
- `worker should update this later...`

Do not write ordinary visit fields in a way that repeats or closely paraphrases third-party source copy. Links to useful third-party pages can be stored where a link field or maintenance context genuinely needs them, but visitor-facing and AI-facing text must be Autoura's own configured guidance. This applies even when naming the source appears to add credibility.

Do not turn source-selection guidance into live content. If a field needs to be maintained from one source rather than another, put that instruction in refresh/source context, not in the field itself. Ordinary fields should contain the configured answer, such as the current entry rule, price, timing, access condition, food option, or planning implication. They should not say which page a future worker or AI should use to research that answer.

Bad pattern:

`GO TOKYO lists wheelchair ramp, elevator and multi-purpose toilet facilities for Sensō-ji.`

Better pattern:

`Plan step-free movement carefully around the busiest temple approach areas, and use the venue/accessibility links for current facility detail before relying on a specific route.`

Use official/first-party sources for factual configuration wherever possible. If a third-party source helps discover an access, ticketing, facility, or visitor-instruction detail, verify it against official/first-party evidence before turning it into live visit guidance. If verification is not possible, either omit the unverified detail, link the source only in a suitable link/maintenance context, or raise it as a curation/fact question rather than copying or paraphrasing it into live fields.

Do not put Autoura system-state wording in ordinary data fields. Public and AI-facing visit text should explain the visitor reason, not the internal content-management state. Avoid phrases such as:

- `already published`
- `published stop`
- `configured stop`
- `nearest configured`
- `nearby stop`
- `closest candidate`
- `draft`
- `ready for review`
- `publication request`

Instead, say why the place, date, route, ticket, recommendation, or practical detail matters to the visitor: close to the exit, best for families, quieter after lunch, useful in bad weather, a strong local option, accessible, scenic, worth the detour, or a sensible fallback.

Do not explain internal maintenance boundaries in live visit data fields. Ordinary data fields must not say that a field is visit-level, stop-level, task-level, fresh-check-only, maintained elsewhere, or not to be used for another workflow. Those are configuration and workflow rules, not visitor or consumer-AI guidance.

Curation should be positive and action-oriented across **all** live visit fields, not just dining or recommendations. Live visit text should tell the visitor, planner, or consumer AI what to use, choose, notice, book, avoid missing, or consider first. It should not be built around what failed curation, what was rejected, what was ruled out, or what not to do. Negative guidance is allowed only where it materially protects the visitor experience, and should be brief, visitor-facing, and secondary to the positive recommendation.

Where a public link is itself the data, it belongs in the appropriate URL/link field, such as website, ticketing direct link, recommendation link, or alternative link. Otherwise, source URLs and maintenance instructions belong in `fresh_instructions` / `fresh_frequency`, not in visitor-facing or AI-facing content fields.

### URL placement

Use the main visit website URL for the venue homepage or canonical public page for the visit as a whole. Do not set the main website to a narrow product page, event page, accessibility page, booking step, PDF, social page, map listing, or third-party directory when a suitable official homepage/canonical venue page exists.

Specific URLs belong with the specific thing they describe:

- ticket booking pages → `ticketing.direct_link`
- different-ticket products, premium experiences, tours, afternoon tea, abseiling, bar/lounge products, or other alternatives → the relevant `ticketing.alternatives` link
- specific recommendations → the relevant recommendation link
- accessibility, travel, dining, or practical source pages → use only in the specific field or maintenance/fresh-check context where that page is the right evidence

For example, a museum visit's main website should normally be the museum homepage or canonical visit page. A special guided tour page should be linked from the matching alternative, not used as the visit's main website.

### General admission opening times and `ticketing.direct`

`ticketing.direct` should explain the normal direct-public-admission path in visitor-facing language: whether visitors can go independently, whether prebooking is sensible or required, what the standard admission product covers, and any basic timing needed to plan around that direct admission.

When a connected/configured stop carries structured opening or availability data for the visit, the detailed operating hours belong on that stop. In that case, `ticketing.direct` may mention only the visitor-useful booking/admission implication and should not duplicate every structured opening row.

When there is no connected stop, no API-backed availability, or no structured opening-hours record for the base visit, put a rough general-admission opening pattern in `ticketing.direct` if the information is known from official/first-party evidence. This should be concise, such as `General Admission is normally available daily 10:00-17:00, with last entry at 16:30`, and should include the key direct-admission constraint rather than a full source log.

Ordinary opening hours do not belong in `practical.scheduled_events` or `comingup.items`. Use `practical.scheduled_events` for the fuller schedule or temporary-condition guidance when timing materially changes the visit. Use Coming Up for the key highlighted current/future items from that wider dated context. Use `practical.restrictions`, `planning.gotchas`, or access/practical fields for stable limitations when they are not specifically part of the direct admission path.

### `practical.scheduled_events`

Use `practical.scheduled_events` for date-based or schedule-based things that materially shape the visit, such as ceremonies, performances, parades, timed displays, seasonal programmes, recurring public events, temporary closures, reduced-service days, or days when a usual feature is explicitly absent.

`practical.scheduled_events` is not mandatory. It may be empty when there is no useful schedule to expose or when the visit is not schedule-dependent.

If `practical.scheduled_events` is populated, it should contain the relevant schedule or temporary-condition guidance itself. It can include specific upcoming dates, times where applicable, recurrence patterns when they are the useful visitor answer, event type/name, and any material visitor difference such as music, no music, reduced ceremony, altered route, limited access, ticketed-only entry, weather dependency, or seasonal exception.

Do not use `practical.scheduled_events` for ordinary opening hours, source names, source URLs, last-checked dates, task notes, fresh cadence, or instructions to check a schedule elsewhere. If the schedule is maintained from an external source, describe that maintenance in `fresh_instructions`.

### `comingup.summary` and `comingup.items`

Use Coming Up for a curated highlight set of current or future date-specific things that materially shape visit planning, such as temporary closures, exceptional opening changes, late openings, timed-access changes, seasonal access windows, major exhibitions, notable scheduled events, cancellations, reduced-service days, or days when a usual feature is explicitly absent.

Coming Up does not replace `practical.scheduled_events`. `practical.scheduled_events` can carry the fuller schedule or temporary-condition guidance; Coming Up is the compact highlights layer for the key dated items visitors are most likely to need next.

Coming Up is not mandatory. It may be empty when there is no useful dated planning information to expose or when the visit is not date-dependent.

If `comingup.items` contains one or more items, `comingup.summary` is required. The summary should be 40-240 characters and explain why the dated Coming Up information matters for planning this visit. It should summarise the practical significance of the items, not list every item.

`comingup.items` is an optional chronological array of up to 20 current or future dated visitor-relevant items. Each item has:

- `start_date`: required, `YYYY-MM-DD`
- `end_date`: optional, `YYYY-MM-DD`, for date ranges
- `title`: required, 8-80 characters
- `nibble`: required, 10-160 characters, explaining the practical visitor implication

Coming Up is a high-level curated planning signal, not an exhaustive event feed. It does not have to include every event. Prioritise significant closures, major visitor-facing events, exceptional opening/timed-access changes, seasonal access windows, and other items that materially change whether, when, or how someone should plan the visit.

Do not use `comingup.items` for normal opening times, standard last-entry notes, recurring weekly patterns, stable key facts, past items, repetitive filler, minor internal/admin changes, or food unless the food item is itself a dated visitor-relevant event.

See the dedicated Coming Up guidance: `https://www.autoura.com/core/pai/docs/visit_comingup.md`.

Good pattern:

`comingup.summary`: `Summer palace openings and a gallery closure change which dates and areas are worth planning around.`

`comingup.items`: `[{ "start_date": "2026-07-20", "end_date": "2026-09-27", "title": "State Rooms summer opening", "nibble": "Book ahead for the seasonal interior route; outside these dates, plan the palace as an exterior-area visit." }]`

Bad pattern:

`Official calendar checked 14 May 2026. Visitors should check https://... before travelling because dates may change.`

Do not use `comingup.summary` or `comingup.items` for source names, source URLs, last-checked dates, task notes, fresh cadence, or instructions to check a schedule elsewhere. Autoura is the configured schedule; if the schedule is maintained from an external source, describe that maintenance in `fresh_instructions`.

Keep dated events current. Remove outdated dates unless a past date is intentionally useful context, which should be rare. If the source gives only a recurring pattern and no dates, use the recurrence only until specific dated entries are available.

### Stale information handling

Do not treat all stale information the same way. First decide whether the old information is:

1. **Stale event/date residue** — a past event, expired offer, old exhibition date, old scheduled entry, past seasonal programme, or other time-bound item whose visitor value has ended. Remove or replace it with current/future information. Do not leave past dates in live visit content just because they were once true.
2. **Stale but still potentially protective or operationally important** — an access constraint, lift outage, closure, restricted entrance, safety rule, route disruption, missing facility, ticketing restriction, or similar visitor-impacting condition that may still be true. Do not delete it merely because the last evidence is old. Verify it if possible, update it if current evidence is available, or keep/phrase it as a current caution with an appropriate fresh-check path until it is confirmed resolved.

For example, “lift out of action in February 2026” should not simply be removed from a wheelchair-relevant visit because February has passed. The correct action is to establish whether the lift is now working. If current status cannot be established, preserve the visitor-impacting risk in the relevant practical/accessibility/gotcha content and set or update `fresh_instructions` so the lift status is rechecked. Do not tell visitors to check with the venue before travelling or before relying on Autoura guidance; Autoura content should give the usable answer or hold a clear maintenance path for resolving the unknown. By contrast, “special half-term aircraft talk on 18 February 2026” should be removed or replaced after the date has passed.

If stale information is material to accessibility, safety, mobility, ticketing, closures, or whether a visitor can complete the visit, preserve the risk until there is reliable evidence that the issue no longer applies.

Preserving the risk in visitor-facing wording is not the same as resolving the underlying unknown. If current truth cannot be established for a visitor-critical stale/missing fact, preserve the risk in visitor-facing content and keep a clear fresh-check or maintenance path where one is available. Do not present cautious wording as a confirmed resolution of the underlying fact. Visitor-facing fields must not offload the work to the visitor with wording such as “check with the venue before travelling”, “check current availability”, or “check before relying on this”; that belongs in Autoura maintenance/fresh instructions, not in the visit guidance.

### `fresh_instructions` and `fresh_frequency`

Use `fresh_instructions` and `fresh_frequency` for operational maintenance instructions when visit data depends on information that changes after configuration.

`fresh_frequency` is the cadence for checking the relevant source or maintaining the field. Use the narrowest useful cadence:

- `fortnightly` for visitor-critical schedules, frequent date changes, live calendars, seasonal programmes, closure patterns, or anything likely to change month to month
- `monthly` for moderately changeable information such as temporary exhibitions, seasonal dining/retail changes, or recurring offer updates
- `quarterly` for stable factual information that still benefits from periodic verification

Most visits should have `fresh_frequency` set to `quarterly`. Use a faster cadence when a visit depends on a known changing source, such as weekly schedules, seasonal programmes, closures, exhibitions, events, offers, or date-sensitive ticketing. Leave `fresh_frequency` unset, or use a slower/non-quarterly cadence, only when there is a clear reason and the reason is obvious from the visit configuration or task evidence.

`fresh_instructions` should explain exactly how to keep the visit content fresh. It may include source names, source URLs, extraction rules, field names to update, and edge cases to preserve. This is an internal/operational field, so it can describe inputs and maintenance workflow.

`fresh_instructions` must be about maintaining the **visit record itself**. It should tell the worker how to keep visit fields current, such as `ticketing.direct`, `ticketing.direct_link`, `practical.scheduled_events`, `comingup.summary`, `comingup.items`, `planning.gotchas`, `getting_there.*`, `practical.*`, `dining.*`, or `recommendations.*`.

Do not use visit `fresh_instructions` as a stop-maintenance checklist. If a named stop, dining venue, market, shop, cafe, viewpoint, garden, parking place, or other supporting object needs its own recurring check, that belongs on the stop or in the relevant stop/fresh-check task flow, not inside the parent visit's `fresh_instructions`.

It is fine for visit `fresh_instructions` to mention a supporting stop only when the instruction is to update a visit field that depends on it. For example, `If the cathedral cafe closes, update dining.refreshments and any related gotcha` is visit-level. By contrast, `Check the cafe page monthly and update the cafe stop opening times` is stop-level and should not be stored in the visit `fresh_instructions`.

Good `fresh_instructions` should say:

- what source(s) to check
- which visit field(s) to update
- what data to extract
- what distinctions to preserve, such as `NO MUSIC`, reduced ceremony, ticketed-only entry, seasonal closure, or route change
- whether old dated entries should be removed
- what to do when the source is empty, ambiguous, or conflicts with existing configured data

Example:

`Fortnightly: check the Household Division Changing the Guard calendar. Use only the Buckingham Palace section. Update practical.scheduled_events with the fuller useful schedule guidance, including date, time, ceremony type and musical support. Update comingup.items and comingup.summary only with the key current/future highlights that materially affect visit planning. Explicitly record Buckingham Palace NO MUSIC rows when they affect planning. Remove outdated dates. Do not add every minor calendar row to Coming Up if it does not change the visitor decision.`

### `facts`

Use `facts` for exactly six short preview facts shown as useful "before you go" highlights on the public visit page.

Each fact must be one discrete visitor-planning detail in one array item. Do not combine several facts into one item just to fit the six-item limit. Facts are neutral visit-planning details, which may include seasonal, volatile, or event-based information where useful. Keep preview facts separate from curator advice: the curator tip is opinionated guidance from the curator, while preview facts are factual highlights that help a visitor understand the visit before they start planning.

Preview facts should be useful, specific, and decision-shaped. They should feel like six things a smart local would tell someone before they go: concrete, visitor-relevant, mildly interesting, and different from the curator tip. Use practical friction, surprising context, option differences, timing consequences, current/seasonal details, or things worth noticing. Do not let them read like bland product descriptions, generic marketing copy, or a summary of configured fields.

Facts must not make the same point as `brand_tip` or `brand_tip_summary`. Facts may describe the factual conditions that support the curator tip, but the curator tip owns the opinionated advice about what to choose, prioritise, or do. Avoid industry/framing words in public facts, such as `hook`; write the visitor meaning instead, such as visual focus, main thing to notice, useful choice, or practical constraint.

Preview facts may be stable, such as entry, scale, layout, access, facilities, or ticketing basics, or time-sensitive, such as current exhibitions, market dates, temporary closures, ceremony patterns, weather-sensitive routing, or school-holiday crowding. If a preview fact can go stale, support it with suitable `fresh_instructions` and `fresh_frequency`.

## Visit onboarding flow

Use this flow when onboarding a visit with the brand originator — the human representative of the brand who knows what should be included and is responsible for curating the experience. The goal is to respect their knowledge, avoid configuring the wrong things, and keep contact concise.

Terminology note: `brand originator` means the brand or party whose visit/product judgement is required for curation, recommendations, signoff, or publication confidence. It is not always the same as the task `subject`; task subject is a task-routing/context field and may be Autoura or an internal user for email/closure handling.

Be upfront in the first email that onboarding normally has three stages: facts, curation, and signoff. This helps the brand originator understand why the first email is a fact sense-check rather than the final review, and reassures them that Autoura values their time and will not turn onboarding into a long chain of small emails.

Expected endpoint order for creating and publishing a visit is: `visit_evaluate` → `visit_new` → `visit_update` as needed → brand-originator curation → curation applied to the visit → final brand-originator signoff where needed → `visit_publish_request` → approved review → `visit_publish`. Do not call `visit_publish_request` merely because the factual fields are complete; the visit must have passed through the curation phase first.

## Creation and configuration-task flow

Use `visit_evaluate` before `visit_new`. Create a visit only when the evaluation confirms that the visit is unique, suitable, recognisable, and accepted for creation.

When calling `visit_new`, use `self_configure` to say who will configure the visit after creation:

- `self_configure: false` — default/normal. The caller is **not** configuring the visit directly. Autoura creates the new visit and starts the normal visit-configuration task flow. For visits, that generated task covers the factual configuration stage up to the point where brand-originator curation input is needed.
- `self_configure: true` — the caller is taking responsibility for configuring the visit directly with `visit_update`. Autoura does **not** create a separate configuration task for that visit.

Use `self_configure: true` only when the current worker or external agent really will complete the visit configuration work themselves. Otherwise leave it false so the visit enters the normal Autoura configuration workflow.

If related stops need to be created during visit configuration, do not configure those stops inside the visit task unless the current task explicitly owns that stop work. Instead, normally create each accepted new stop with `stop_new` and `self_configure: false`. That creates the stop and starts the separate stop-configuration task flow. The visit task should then track the new stop IDs and wait for, reference, or coordinate the resulting stop work rather than becoming a stop-configuration task itself.

## Protect brand-originator settings

Brand-originator supplied, approved, or already-published settings must not be silently deleted, overwritten, or overruled just because a worker finds conflicting evidence or a tool suggests a different value. This is especially important once a stop or visit has been published, but it also applies before publication when the originator has clearly supplied a preference or setting.

If a field is missing and can be filled from official/first-party evidence, configure it normally. If a field already has a value and new evidence suggests a different value, preserve the existing value and treat the conflict as an originator question rather than an automatic correction.

Examples:

- If the configured location is X but `address_to_geocode` or another source suggests Y, do not immediately move the stop/visit to Y. Ask the brand originator whether X or Y is correct.
- If the originator has preferred dining, partner venues, meeting points, or visitor advice, do not replace it with generic nearby research without checking.
- If opening times, ticketing, restrictions, suitability, or access details conflict with current official evidence, record the conflict and ask for confirmation before overwriting originator-provided or published content.

The right action is to add the question to the current originator clarification/signoff list, or send the planned originator email if that is the current workflow step. Avoid sending many isolated emails for small conflicts unless the conflict is a real blocker.

### 1. Facts stage

Gather visit-specific facts before configuring the visit. Include ticketing, opening/availability, access, arrival, main entrance, practical tips, nearby dining, transport, suitability, and any first-party recommendations. Practical tips must be specific to this visit, not generic travel advice.

Find facts from official/first-party sources when they are reasonably available. The brand originator may be asked for help with facts that are unclear, missing, specialist, or brand-specific, but should not be asked to supply information that Autoura can readily verify itself. A key part of visit value is capturing useful local knowledge that may not be online, such as where to have a picnic, which entrance is easiest, where visitors should pause, or what nearby place is genuinely worth choosing.

Send one facts email to the brand originator summarising what was found in plain brand-friendly language. Ask whether the facts look right, whether they have preferred/partner dining or practical recommendations, and whether any ticketing or access nuance is missing. This is not asking for permission to configure; it is a quick sense-check so Autoura does not spend time configuring the wrong restaurants, tips, ticketing details, or visitor advice.

After the facts are clear enough to proceed, configure the factual baseline before moving to curation. Configure location first: get the venue address from the official venue website or another official/first-party source, then use the `address_to_geocode` MCP tool to set or validate the geocode. Do not rely on an LLM's remembered location for coordinates. This means filling the factual visit fields as completely as possible: location/main entrance, opening or availability, ticketing, access, facilities, getting there, practical tips, suitability, nearby factual context, and other confirmed source-based details. Do not leave known facts sitting only in notes or emails.

Named places identified during the facts stage may need to be configured and published as stops before curation continues. For example, if the factual baseline says visitors should use a specific restaurant, shop, viewpoint, car park, picnic spot, or nearby attraction, configure that place as a stop with its own official/first-party facts, location, opening/availability, and practical details.

Supporting stops do not require brand-originator signoff in the same way the visit does. If a supporting stop is factual, useful, and ready, send it through the normal Autoura stop publish-review flow. The stop can be approved and published through that flow before the visit reaches final brand-originator signoff. The visit itself still requires brand-originator signoff before publication.

Only move to the curation stage once the factual baseline has been configured, including any supporting factual stops that are needed for the visit, or once any unresolved fact blocker has been clearly recorded and routed back to the brand originator.

Before treating the factual baseline as ready for curation, run the relevant parts of the publishing checklist against every field being presented as configured or complete. Do not send weak, generic, unsupported, placeholder-like, or checklist-failing field content to the brand originator as if it is a finished baseline for curation; either fix it first, create/reroute the needed stop/research task, or present it explicitly as an unresolved question/blocker.

### 2. Curation stage

Curation is the brand-originator input stage. This is where the brand originator adds judgement beyond AI research and official facts: what visitors should actually experience, prefer, avoid, notice, recommend, and remember. Autoura trades on curation, so this stage is mandatory before publication. Without brand-originator curation, the brand would be putting its name on Autoura's draft rather than on an experience shaped with its own input.

AI agents and configurers may research, prepare, structure, and propose options, but they do not replace the brand originator as curator. Human brand-originator judgement is required for curation: what to recommend, avoid, prioritise, notice, and remember.

After the factual baseline has been configured, shape the visit experience: visitor flow, key moments, structure, pacing, what to notice, what the visit should feel like, and character/tone where relevant. The AI/configurer may prepare a proposed structure, but that proposal is not a substitute for brand-originator curation.

Curation is not just listing the nearest available options. Autoura visits are tourism / leisure experiences, so choose what is good, interesting, distinctive, useful, or brand-appropriate. A place that is a little further away may be better than the nearest match if it creates a stronger visitor experience.

Before asking the brand originator for curation input, do useful preparation work. Configure a reasonable factual baseline of likely dining and nearby context first, using official/first-party sources and normal stop configuration rules. Then ask the brand originator to curate from that baseline rather than asking them to do the initial research.

#### Curator Provenance and Big Curator Tip

Use the curator fields to explain the human/brand curation layer without putting provenance into ordinary visit fields.

##### Curator Provenance

`curator_why` powers the "about the curator" / curator provenance surface. It must be about this curator's relationship to this visit. If the visit brand or curator changes, this field needs to change too.

`brand_curator_role` classifies that relationship using one of:
`actual_venue`, `nearby_hotel`, `nearby_shop_or_restaurant`,
`tour_operator_to_visit`, `tour_operator_nearby`,
`travel_advisor`, `local`, `house`, `destination_organisation`, `other`,
or `retailer`.

Both tour operator roles include tour guides. Use `tour_operator_to_visit` for a tour operator or guide who runs tours to this place, or `tour_operator_nearby` for a nearby tour operator or guide. There is no separate tour guide curator role.

`travel_advisor`: A human travel advisor, itinerary designer, trip planner or concierge

`retailer`: Regional concierge or retailer

Use `retailer` for a region-focused service that helps visitors plan or sells relevant local experiences but does not operate the Visit.

`house`: Autoura house brand

Show why the guidance is credible by stating the real-world source of the curator's knowledge.

Do say where the knowledge comes from: commercial tours, daily venue operations, regular guiding, local residency, collection curation, event delivery, destination management, or another concrete relationship to the place or experience.

Use concrete frequency or operational context when known: takes guests there 40+ times a year, guides visitors there weekly, runs the venue every day.

Do not paraphrase the visit advice here.

Do not use internal wording like `our data`.

Do not reuse a generic sentence across visits unless the curator relationship is genuinely the same.

Example:

`Red Rock takes guests to Petra throughout the year, so this guidance comes from real tour experience.`

##### Big Curator Tip

`brand_tip_summary` powers the big curator tip surface. It should show one short, useful curator insight that proves the guidance contains human judgement.

Derive it from `brand_tip` where available.

Keep one idea only: the strongest practical or experiential tip.

Make it specific enough to be useful without extra context.

Do not duplicate the provenance sentence.

Do not include stop IDs, internal labels, or "check before you go" wording.

Example:

`Start early, enter through the Siq first, and do not treat the Treasury as the finish line.`

For dining, a good curation question is: “We have configured these nearby/on-site dining options as the factual baseline; which would you actually recommend for your visitors, and are there any preferred, partner, locally loved, or better-fit places we should add instead?” This lets Autoura do the work ahead of time while still requiring the brand originator to curate the baseline and provide real local judgement.

The brand originator’s answer may create follow-up stop work and should also improve the visit’s own curation text. If they recommend a new restaurant, café, bar, picnic spot, shop, meeting point, nearby attraction, or other named place that should form part of the visit experience, configure that place as a stop through the normal stop workflow before relying on it in the visit.

Also update the relevant visit dining/surrounding-context text fields — such as `dining.budget`, `dining.standard`, `dining.premium`, `dining.picnic`, `dining.refreshments`, `dining.street`, `dining.bar`, and `dining.dietary` — to record the curated judgement. These fields are not just factual placeholders; they should explain which options are actually good, recommended, brand-preferred, family-friendly, celebratory, quick, scenic, rainy-day suitable, dietary-suitable, or best avoided. They should feel like useful local advice, not generic category filler. This is one of the core valuable parts of the visit configuration: turning brand/local curation knowledge into structured data that future AI agents can use. For example, if the originator says “X is good for families after the visit” or “Y is the best premium option, even though it is slightly further away,” that curation insight belongs in the appropriate dining text as well as in the configured stop set.

Write dining guidance as visitor advice, not as commentary on Autoura's configuration state. The general data-field rule above applies here: do not describe a dining place as useful because it is configured, published, nearby in the system, or a candidate. Say why the place works for the visitor: close to the exit, good for a quiet drink, useful with children, best for a reflective post-visit meal, strong local character, late opening, good picnic supplies, wheelchair-friendly, or worth the detour.

Recommendations are a key curation surface. Use `recommendations.pre`, `recommendations.during`, and `recommendations.post` for the brand-originator's curated advice about what visitors should do before, during, and after the visit. Do not invent or pre-fill these fields just because the factual baseline is complete. It is often better to leave recommendations empty during factual configuration, then explicitly flag them in the curation email question list so the brand originator can provide brand-approved recommendations, preferred choices, useful warnings, local judgement, and experience-shaping advice.

When dining venues, shops, nearby attractions, or other places are named as part of the visit experience, they **must be configured as stops**. The visit text can then mention them naturally — for example, “go to X restaurant” — without carrying all the operational detail itself. The stop holds the accurate location, opening times, ticketing or booking information, practical facts, and other details that AI agents need to guide visitors properly. The visit dining text should hold the curated recommendation/context; the stop should hold the operational facts.

When AI-facing visit text names a specific configured stop that the consumer AI may need to resolve, include the stop id inline immediately after the name using `[stop:<stop_id>]`.

This applies beyond dining. Use inline stop references for named viewing spots, meeting points, route anchors, entrances, viewpoints, shops, markets, nearby attractions, picnic places, transport points, and other practical POIs when the visit guidance depends on that real place.

Pedestrian and arrival instructions are operational guidance, not decorative text. If `getting_there.pedestrian`, planning structures, moments, gotchas, or practical guidance tell a visitor to follow a named gate, station, street, entrance, meeting point, route anchor, public square, viewpoint, shop, market, or other real place, that place should be configured as a stop/POI where the consumer AI may need to resolve or guide it.

Do not leave step-by-step named walking instructions as unsupported free text.

Bad pattern:

`From Asakusa Station, walk about 5 minutes to Kaminarimon Gate, then continue along Nakamise toward the temple precinct and Main Hall.`

Better pattern, once the places are configured:

`From Asakusa Station [stop:poi-...], walk to Kaminarimon Gate [stop:poi-...], then use Nakamise Shopping Street [stop:food-...] as the main approach toward the temple precinct and Main Hall.`

If the named route anchors are not yet configured as stops, create or route the required stop/POI work before treating the pedestrian guidance as complete. If the instruction can be useful without named anchors, write it at a higher level and avoid precise route claims that the AI cannot ground.

Example:
`For coffee with a marina view, go to HarBAR on 6th [stop:food-15a5a9b2618daf1], a few hundred metres from Solent Sky.`

Example:
`For Changing of the Guard, consider Wellington Barracks [stop:poi-...], Buckingham Palace Forecourt [stop:poi-...], or St James’s Palace and The Mall [stop:poi-...] depending on whether the visitor wants inspection, the main palace view, or the marching route.`

The bracketed stop reference is AI-facing grounding metadata. The text should still read naturally if the bracket is ignored. Use verified stop ids only.

#### Character setup

A character can make a visit warmer, more distinctive, and easier for visitors to relate to, especially when the experience will be delivered through Autoura Connect or another Autoura-hosted guide surface.

Character setup is recommended where it improves the visit experience, but it is not mandatory for first publication. A visit may be published without a character when the factual baseline, curation layer, supporting stops, and publishing checks are otherwise complete.

Character work can happen in either of two places:

- **During the curation stage** — when the brand originator already has a clear view of the guide voice, personality, tone, or visitor relationship they want.
- **After first publication** — when it is better to get the visit live first, then add or refine the character once the visit structure and visitor use case are clearer.

Do not block publication solely because character setup is incomplete, unless the character is central to the promised visitor experience or the brand originator has explicitly made it a launch requirement.

Not every consumer will use the configured character. Some visitors may experience the visit through their own AI agent, a partner interface, or another route rather than Autoura Connect. For that reason, the visit configuration itself must still stand on its own: the summary, description, planning, recommendations, scripts, dining/surrounding context, and practical fields should contain enough structured guidance for any capable AI agent to support the visitor without relying on a specific character.

When asking the brand originator about character during curation, keep it lightweight. Ask what kind of guide voice or personality would suit the visit, whether the brand has an existing mascot/persona/spokesperson, and whether there are tones to prefer or avoid. If the brand is not ready to decide, record character setup as a possible follow-up rather than treating it as a blocker.

#### Curation email

Once the factual baseline, supporting stops, dining/recommendation questions, and any useful character/tone question are ready, send one curation email to the brand originator before publication, unless the brand originator has already provided explicit curation input in this workflow and that input has been applied to the visit.

Use their language: ask about the visitor experience, what guests should notice, what the visit should feel like, preferred places to include, which already-configured dining/nearby options they actually recommend, whether any better-fit places should be added, and any preferred character or tone. Either ask focused questions or propose the curated structure for correction. Do not send many small emails for isolated choices unless a genuine blocker exists.

Do not treat `mandatory_fields_ok=true`, an existing `published` workflow state, AI-generated recommendations, official-source factual completeness, or configured supporting stops as evidence that curation is complete. Curation is complete only when brand-originator input has been requested, received, applied where appropriate, and either accepted in final signoff or clearly recorded as blocked/waiting with the next owner.

### 3. Signoff stage

When the visit has been configured and curated, send one final signoff email to the brand originator. Keep it friendly and practical: explain that the visit is ready for review, ask them to check it in the Autoura Dashboard, or if they are an AI agent, to run `visit_get` via MCP. Ask them to approve publication or send final changes.

Recommended contact pattern: facts email → curation email → final signoff email. Add an extra question only for a real blocker such as unclear ticketing, ambiguous venue identity, or safety/accessibility uncertainty. The brand originator should feel consulted, not chased.

## Alternatives, structures, moments, gotchas, and recommendations

Alternatives, structures, moments, gotchas, and recommendations are separate configuration surfaces. Do not use one as a dumping ground for another.

Use each field for its intended purpose:

- `ticketing.alternatives` = different ways of experiencing the same venue instead of the standard/base visit
- `planning.structures` = suggested ways to organise and pace the main/base visit
- `planning.moments` = lightweight things to do, notice, pause for, or consider during the main/base visit
- `planning.gotchas` = important details that are easy to overlook and may disrupt the visit
- `recommendations.pre`, `recommendations.during`, and `recommendations.post` = curated local services, choices, or experience-shaping suggestions before, during, or after the visit

These fields should work together, not duplicate each other.

### `ticketing.alternatives`

Use this for genuinely different ways of experiencing the same venue instead of the standard/base visit, such as separately booked tours, afternoon tea as the main format, evening lounge visits, abseils, or other materially different products.

For tour operators and similar commercial originators, a bookable product may be a `ticketing.alternatives` item only when it is genuinely relevant to this visit. Relevance comes before brand inventory. Do not add a tour, route, private guide, taxi/private alternative, bike tour, walking tour, or broader product merely because the brand sells it or because it passes nearby. It must fit the visit's visitor need, location, theme, timing, and commercial intent.

For tour operators, the commercial reason to configure a venue visit is often **venue-first demand**. Some visitors begin with a tour operator and choose from its catalogue, but many begin with the venue itself and then look for a guide, tour, transport, planning help, or a better way to experience that place. Configure the visit so relevant tour-operator products are discoverable from that venue-first context when they genuinely help the visitor.

Do not make the visit a generic advert for the tour operator, and do not add every tour the operator sells. The tour/operator product belongs in `ticketing.alternatives`, `recommendations.*`, or another relevant field only when it answers the visitor’s venue-specific need. The point is to connect “I am going to this venue and need help” to the operator’s relevant offer, not to reproduce the operator’s tour catalogue inside the visit.

If a tour/product is a different way to experience the place, put it in `ticketing.alternatives`. If it is genuinely the recommended thing to do before, during, or after this visit, put it in `recommendations.pre`, `recommendations.during`, or `recommendations.post`. Do not mix tour-operator products into generic nearby recommendations.

Read the examples only if you are configuring or reviewing this field: https://www.autoura.com/core/pai/docs/visit_ticketing_alternatives.md

Quick checks:

- alternatives are different visit products or formats, not moments inside the base visit
- do not invent alternatives just to fill the field
- for tour-operator products, check relevance to this specific visit before adding them
- for tour-operator products, check whether they serve venue-first demand: a visitor who started with this venue and now needs a guide, tour, transport, planning help, or better way to experience it
- do not hide the main visitor moments here

### `planning.structures`

Use this for suggested ways to organise and pace the main/base visit.

Read the examples only if you are configuring or reviewing this field: https://www.autoura.com/core/pai/docs/visit_planning_structures.md

Quick checks:

- use when `planning.level = some`
- structure is about flow, order, pacing, and visitor fit
- do not use structures for different-ticket products or separate premium experiences

### `planning.moments`

Use this for lightweight things to do, notice, pause for, or consider during the main/base visit.

Read the examples only if you are configuring or reviewing this field: https://www.autoura.com/core/pai/docs/visit_planning_moments.md

Quick checks:

- moments must be available within the base visit product
- do not include separate booking, age-restricted, special-access, or materially different-format products unless they are genuinely part of the standard/base visit
- keep moments flexible and experience-shaping rather than turning them into a fixed schedule

### `planning.gotchas`

Use this for important details that are easy to overlook and may disrupt the visit.

Read the examples only if you are configuring or reviewing this field: https://www.autoura.com/core/pai/docs/visit_planning_gotchas.md

Quick checks:

- gotchas should be specific, actionable, and visitor-relevant
- each gotcha needs a prevention and mitigation path
- do not use this for generic travel advice

### `recommendations.pre`, `recommendations.during`, and `recommendations.post`

Use this for curated local services, choices, or experience-shaping suggestions before, during, or after the visit.

Read the examples only if you are configuring or reviewing these fields: https://www.autoura.com/core/pai/docs/visit_recommendations.md

Quick checks:

- recommendations should be geographically local to the visit
- do not duplicate ticketing or alternatives
- do not use recommendations as a dumping ground for tour-operator products; include a tour/product here only when it is genuinely the recommended pre-visit, during-visit, or post-visit action
- do not use recommendations for audio tours
- prefer brand-originator judgement, local knowledge, or clearly useful in-destination context

### `questions`

Use this for 4 suggested visitor-facing questions that help a visitor start a useful conversation with an AI about the visit.

Read the examples only if you are configuring or reviewing this field: https://www.autoura.com/core/pai/docs/visit_questions.md

Quick checks:

- questions should be plain visitor questions, not internal curation prompts
- do not use stop ids, resolver syntax, raw field names, or internal labels
- use named places when the question is about a specific place or comparison
- do not repeat the visit name unless the question may appear out of context or needs it for clarity
- prefer specific decision-shaped questions where the visit content supports them
- at least one question should connect to the brand's objective or intended visitor behaviour where that objective is known
- useful topic areas include tickets, food nearby, accessibility, visit planning, and nearby places; do not force every topic into every visit
- every question must be answerable from the configured visit content
- questions should not repeat the same point already exposed by the six preview facts unless the question genuinely opens a deeper planning decision; duplicating a fact as a question is boring and makes the page feel padded
- seasonal questions are allowed only when `fresh_instructions`, `fresh_frequency`, and fresh/task notes support keeping the answer and question current

### Field placement checks

When deciding which visit field should hold a piece of content, use the placement examples here: https://www.autoura.com/core/pai/docs/visit_field_placement_examples.md

Use that file when alternatives, structures, moments, gotchas, and recommendations could be confused.

## Dining and dietary context

Dining is a normal, expected part of a complete visit configuration.

Dining stops may be **inside the attraction or venue** (for example an on-site café, restaurant, bar, kiosk, picnic area, or food hall) or **outside the attraction nearby**. Both are valid. The important point is that they are useful to visitors and configured as stops where the visit depends on them.

For an average visit, there should usually be **at least 20 dining stops** available to the visit across on-site and nearby options. These should be configured as stops and resolvable from the visit context, not merely mentioned in text, notes, emails, or planning assumptions. A stop may be resolvable through an inline `[stop:<stop_id>]` reference in the relevant AI-facing free-text field, through direct stop lookup, or through other current configuration evidence; it does not have to appear in a sampled or highest-ranked nearby-food list to count as configured support.

In `visit_get`, check the `dining.*` text fields first. They explain the visitor-facing guidance. Nearby-food or display data, where present, is only a candidate-discovery hint and may be absent from customer/export surfaces; it is not the curated dining baseline, not exhaustive, and not proof that the visit has enough configured dining support.

Dining context is not a small optional extra. Visitors usually need somewhere nearby for food, drink, a break, a family-friendly pause, a pre-visit meal, or a post-visit option. For most real venues, most mainstream dining categories should be available nearby.

The dedicated `dining.dietary` field is mandatory. It should actively cover useful dietary choices and limits, especially halal, kosher, vegetarian, vegan, gluten-free, dairy-free, allergy-aware, or other clearly labelled needs where relevant. Vegetarian options are common visitor needs and must not be omitted from dietary thinking. Halal and kosher dining are more specialised and may be genuinely rare depending on location, but they still need a researched judgement rather than being ignored. Picnic options can also depend on the venue and local rules. General budget/standard dining, premium dining, bars/pubs, cafés, convenience food, and speciality food are normally expected to be findable in most visitor areas.

If mainstream dining or dietary options cannot be found, or if the worker believes a normal dining category is genuinely unavailable, treat this as a **classic brand-originator question**. Ask the brand originator whether there are preferred, partner, recommended, or locally known dining places that should be included. Do not casually write common dining or dietary categories as unavailable just because the first search was weak.

### Dining stops must be configured as stops

Do **not** set or imply that dining stops are available unless the relevant stops have actually been configured.

If the visit says that nearby dining is available, the corresponding dining stops must exist as configured stops and be linked/available through the visit so they appear in `visit_get`.

This is mandatory-field sensitive:

- saying dining is available without configured stops will fail mandatory-field checks
- named dining venues must be configured as stops
- dining categories should not be marked available unless there are real configured stops behind them
- “available” means operationally available to the visit, not merely known from research

The stop object should hold the operational facts: location, official website, opening/availability, booking or access notes, practical details, suitability, and any useful visitor guidance.

Visit text may mention the dining stop naturally once the stop exists, but the visit must not carry all the operational detail itself.

Do not “fix” an unbacked named dining recommendation by removing the specific recommendation and replacing it with vague area-level guidance. If the named place is useful and appropriate for the visit, the right action is to configure or link it as a stop, then keep the specific curated guidance. Only remove or generalise the recommendation if research shows the place is closed, unsuitable, too far away, not visitor-useful, duplicated, or otherwise wrong for this visit; record that judgement clearly.

The configured stops prove the places exist and hold operational facts. The visit dining.* free-text fields must explain the curated advice: why these options are useful for this visit, who they suit, when to use them, and any trade-offs. Do not use dining text as a bare list of nearby stops or duplicate operational stop facts.

### Quality expectations for dining selection

Dining curation should favour visitor usefulness and experience quality, not just proximity.

Do not assume a sampled nearby-food/display list is the correct dining set. Treat it as discovery evidence only, not as the whole answer. Search from first principles around the visit’s real entrance, exit, route, visitor timing, and likely meal/drink moments; then add, link, replace, or remove stops as needed so the configured dining support actually backs the best visitor guidance.

Prefer places that are:

- distinctive, local, independent, or experience-appropriate
- suitable for visitors before or after the visit
- useful for families, groups, accessibility needs, or common dietary needs where relevant
- close enough to be practical
- supported by official or first-party factual sources

Avoid fast food and chain coffee shops where possible, especially when better local or visitor-appropriate alternatives exist nearby.

Fast food, chain cafés, and generic convenience options may be included only when they are genuinely useful, unavoidable, or needed to cover practical visitor needs in an area with limited alternatives. They should not dominate the dining set.

### Expected dining coverage

A well-configured visit should normally include a broad mix of nearby dining stops, such as:

- budget or standard dining
- premium or special-occasion dining
- cafés or casual daytime food
- bars, pubs, or evening drink options where appropriate
- speciality food or locally distinctive venues
- convenience or quick practical food where useful
- for convenience coverage, include the nearest genuinely usable convenience shop or small supermarket to the visit’s normal arrival/exit area, unless there is a clear reason it is unsuitable or unavailable
- dietary-specific options in `dining.dietary`, including halal, kosher, vegetarian, vegan, gluten-free, dairy-free, allergy-aware, or other useful labelled options where relevant

Halal and kosher may be described as not useful or not found when genuinely not found after reasonable research, but this should be treated more carefully than ordinary dining. Vegetarian guidance should normally exist in most mainstream visitor areas. If there is uncertainty, ask the brand originator rather than guessing.

Do not write halal, kosher, vegetarian, vegan, gluten-free, allergy-relevant, street-food/market, or other dining categories as unavailable merely because no configured stop currently exists. First research what actually exists near the visit and on realistic visitor routes. If suitable options exist, create/link the supporting stops and write the dining guidance from those real options. Only state that a category is not useful when reasonable first-principles research shows that suitable options genuinely do not exist, are too far away, unsuitable, closed, or otherwise not useful for this visit.

For common categories such as budget/standard dining, premium dining, bars/pubs, cafés, convenience, and speciality food, “none available” should be unusual near a normal visit venue. If no options are found, escalate the question to the brand originator or create a fix task rather than silently marking the category unavailable.

Dining stops must be selected from first principles for this specific visit location and visitor context, not accepted merely because existing stops appear nearby in the system. Search and assess what is genuinely useful around the visit’s real arrival/departure points, likely walking routes, dwell time, visitor types, and meal moments; then link the best-fit stops or configure better ones where needed.

For picnic coverage, consider whether visitors have a realistic way to obtain picnic food, drinks, snacks, or supplies near the visit or on the likely route. This may be a convenience shop, small supermarket, bakery, deli, market stall, café takeaway counter, speciality food shop, or other suitable food stop. A picnic place alone may be insufficient if visitors would reasonably expect to buy supplies nearby.

### When dining information is missing

If dining coverage is incomplete:

1. search existing configured stops first
2. configure useful nearby dining stops where needed
3. use official/first-party sources for facts
4. avoid duplicates
5. prefer quality local options over chains where possible
6. ask the brand originator for recommendations if normal categories appear missing
7. write a truthful visitor-facing `dining.dietary` position covering halal, kosher, vegetarian, vegan, gluten-free, dairy-free, allergy-aware, or other relevant needs
8. only describe a category as unavailable or not useful when that is genuinely true or explicitly confirmed

Do not publish or request signoff for a visit that claims dining availability but lacks the configured dining stops needed to support that claim.

If the dining gap blocks publication, record the blocker clearly and route it to the correct owner:

- Autoura worker/fix task if the missing stops can be researched and configured
- brand originator if local knowledge, preferred partners, or confirmation of unavailability is needed

The brand originator should be asked practical questions such as:

> We would normally expect this visit to have useful dining options configured, either inside the venue or nearby. Are there any restaurants, cafés, bars, picnic spots, food halls, or partner venues you recommend visitors use before, during, or after the visit?

Or, where a category appears missing:

> We could not confirm suitable nearby [category] options from reliable sources. Is that genuinely unavailable, or is there a local/preferred place we should configure?

## Signoff checklist

Before requesting brand-originator signoff, the visit must already meet the publishing checklist below. Do not ask the brand originator to approve a visit that Autoura would not be willing to publish.

Use this signoff check to confirm:

- the configured facts match the latest originator/official-source facts, especially ticketing, opening/availability, access, and practical tips
- ordinary data fields contain the configured output, not source notes, last-checked dates, task instructions, provenance, or “check this page” wording; only `fresh_instructions` / `fresh_frequency` should describe source inputs and maintenance instructions
- `practical.scheduled_events` is either empty or contains useful schedule/temporary-condition guidance; ordinary opening hours, source notes, and maintenance instructions do not belong there
- `comingup.items` is either empty or contains a chronological, current/future, high-level curated set of key dated visitor-planning items; it is a highlights layer rather than a replacement for `practical.scheduled_events`, it must not try to include every event, and `comingup.summary` must be present when items exist
- `fresh_frequency` is normally `quarterly` at minimum for stable visits, unless there is a clear reason not to set quarterly; more changeable visits use a faster cadence
- schedule-dependent visit information has an appropriate `fresh_frequency` and actionable `fresh_instructions` explaining which source(s) to check, which fields to update, what to extract, and how to handle outdated dates or special cases
- `fresh_instructions` relate to maintaining visit fields, not maintaining supporting stops; any recurring check that exists only to keep a named stop, dining venue, market, shop, cafe, garden, viewpoint, parking place, or other supporting object current is stored on the stop or task flow instead
- the curation layer has been completed and placed correctly, including brand-originator curation input requested, received, and applied where appropriate
- supporting stops are configured where needed
- dining coverage is complete enough, including mandatory dietary options guidance, with dining availability backed by configured stops and inline stop references where needed
- alternatives, moments, structures, gotchas, and recommendations are in the right fields; empty recommendations should be explicitly flagged in the curation email question list for brand-originator input, while different-way alternatives must not be presented as base visit moments
- the visit is ready for the brand originator to review in the Autoura Dashboard, or via `visit_get` for AI agents
- the signoff request asks for approval to publish or a final list of changes

### Location description quality

`location.description` must briefly describe the **setting** of the visit: for example, inside a national park, within a historic city centre, along a coastal promenade, inside a shopping and leisure complex, on a university campus, or across several nearby venues rather than one single site.

Do **not** use `location.description` to duplicate information already held in other location fields. It must not simply repeat the venue name, street address, postcode, country, latitude/longitude, geocoding evidence, or other stored location data.

Do **not** use `location.description` for directions. If visitors need arrival guidance, entrance-finding help, walking instructions, nearby wayfinding, or “when you are nearby” directions, put that in `getting_there.pedestrian` instead.

Good examples:

- `Inside Gunwharf Quays, a waterfront shopping and leisure complex beside Portsmouth Harbour.`
- `Within the historic city centre, with most visit activity clustered around the cathedral quarter.`
- `Across several nearby venues rather than one single enclosed site.`

Bad examples:

- `Gunwharf Quays, Portsmouth, PO1 3TT.`
- `Address_to_geocode matched Gunwharf Quays, Spinnaker Tower, The Canalside, Portsmouth, PO1 3TT at 50.7955965,-1.1084489.`
- `Enter via Vernon Gate, walk to the waterfront, then turn right opposite Caffè Nero and Burger King.`

## Publishing checklist

Use this checklist when reviewing a visit publish request.

Publishing normally starts from a publish-request task. The reviewer should assess the visit, update the task with the decision, and either request publication, block the task for fixes, or create follow-up tasks.

Before accepting a visit for publication, be quizzical rather than box-ticking. Do not approve merely because a field is present; check whether the field is useful, accurate, non-duplicative, and in the right place. Check:

- **Quality benchmark:** compare the stop against a known good published example. For visits, Solent Sky in Southampton (`visit-9b6d23b05e5b35b`) is the current quality benchmark; for stops, use an equivalent good stop when available. The aim is complete, useful, visitor-facing fields rather than bare minimum data.
- **Uniqueness:** the visit is not a duplicate of an existing visit. This should normally have been checked before reaching publication review, but if a duplicate is found, do not publish.
- **English source content:** configurable text is written in clear English source text. The platform may translate this for visitors.
- **No configuration placeholders:** Do not publish, request signoff, or treat a visit as content-complete if visitor-facing fields still contain setup language such as “not configured”, “bare-minimum pass”, “to be added later”, “TBD”, “placeholder”, or similar internal workflow residue. Replace it with truthful visitor-facing content, leave the field empty where allowed, or mark the task blocked if the information is genuinely missing.
- **Visitor usefulness:** name, questions, facts, nibble, summary, description, public brand tip summary, brand/curator why, location name, website, opening/availability, suitability, and relevant visit fields are useful enough for a visitor to understand what the visit is and why/when to use it.
- **Public-field cleanliness:** public fields shown on the visit page must be clean human prose. Fail publication, signoff, or post-update verification if `name`, `questions`, `facts`, `nibble`, `summary`, `description`, `brand_tip_summary`, `curator_why`, or `location.name` contains raw resolver syntax such as `[stop:...]`, bare supporting-object ids such as `food-*`, `attraction-*`, or `poi-*`, or internal content/design terms such as `hook`, `anchor`, `stop`, `configured`, `field`, or `surface` used in their Autoura/workflow sense. `brand_tip` is not part of this public-field lint and may contain named `[stop:...]` references where AI-facing resolution is useful.
- **Website URL placement:** the main visit website should be the official homepage or canonical public page for the visit/venue as a whole. Do not use narrow product, event, accessibility, PDF, booking-step, social, map-listing, or third-party directory URLs as the main website when a suitable official homepage/canonical page exists. Put specific product, alternative, ticket, recommendation, accessibility, or practical links in the specific field they support, such as `ticketing.direct_link`, `ticketing.alternatives`, recommendations, or fresh/source context.
- **General admission timing placement:** if the visit has no connected stop, no API-backed availability, or no structured opening-hours record for the base visit, `ticketing.direct` should include the rough general-admission opening pattern where known from official/first-party evidence. If structured stop availability exists, detailed hours belong on the stop and `ticketing.direct` should focus on the booking/admission implication. Do not put ordinary opening hours in `practical.scheduled_events` or `comingup.items`.
- **Location precision:** use the visitor-facing main entrance or normal arrival point where one exists, not the centre of the venue. If there is no clear entrance or arrival point, the centre of the venue may be used. Use the official venue address and the `address_to_geocode` MCP tool to set or validate coordinates; do not rely on LLM-known coordinates.
- **Practical tech / Wi-Fi field check**: practical.tech must only contain technology-specific visitor guidance: Wi-Fi availability, network/login/password details, mobile signal/dead zones, charging, required apps, downloaded tickets, or offline access needs. Do not use this field for general planning advice, opening/schedule checks, ceremony timing, booking policy, or ordinary visit reminders. If no useful tech/Wi-Fi guidance is known, say that plainly or leave the field empty rather than filling it with unrelated advice.
- **Data fields are outputs:** ordinary visit data fields must contain the configured visitor/AI-facing answer, not source/provenance notes, last-checked dates, fresh instructions, internal task history, internal content-management state, workflow boundaries, or “check this page” wording. Do not describe something as “already published”, “configured”, “nearest configured”, “closest candidate”, “nearby stop”, “draft”, or “ready for review” in visitor-facing or AI-facing text. Do not explain that something is visit-level, stop-level, task-level, fresh-check-only, maintained elsewhere, or excluded from another workflow. Put source URLs, source names, extraction rules, and maintenance cadence in `fresh_instructions` and `fresh_frequency`, not in data fields.
- **Provenance coverage:** before publication, every source-backed factual claim already present in the Visit content must have a matching provenance Fact in the Facts database. This is evidence/trust work for existing content, not a new-fact discovery exercise. If a factual claim is unsupported or its matching provenance is missing, fail publication or route the Visit for further publication review rather than approving it.
- **Third-party source copy/provenance:** do not approve visit text that repeats or closely paraphrases third-party directory, tourism-board, review-site, listing-site, guidebook, blog, or other non-first-party source copy. Do not use credibility phrases such as “GO TOKYO lists...”, “Tripadvisor says...”, or “according to [third-party site]...” in ordinary visitor-facing or AI-facing fields. Links are fine in appropriate link or maintenance contexts, but live guidance must be Autoura's own configured wording and should be verified against official/first-party evidence where factual claims matter. Escalate to legal review if a field appears too close to a source's wording or relies on unverified third-party facts for access, ticketing, facilities, safety, or visitor instructions.
- **Scheduled events quality:** `practical.scheduled_events` may be empty, but if it is populated it must contain useful schedule or temporary-condition guidance, with specific dates/times where that is the useful answer and material differences such as music/no music, reduced ceremony, seasonal exception, altered route, limited access, or weather dependency. Do not include ordinary opening hours, source URLs, last-checked wording, or “check the schedule” caveats in this field.
- **Coming Up quality:** `comingup.items` may be empty, but if it is populated it must be a chronological, high-level curated highlights list of current or future visitor-relevant dated items. It should prioritise significant closures, exceptional opening changes, timed-access changes, seasonal access windows, major exhibitions, notable scheduled events, cancellations, and other items that materially affect visit planning. It does not replace `practical.scheduled_events`, does not need every event, and must not become an exhaustive calendar feed. `comingup.summary` is required when items exist and should summarise why the highlighted dated information matters for planning. Hold signoff if Coming Up is bloated with minor events, missing a known major closure/event, contains past filler, or includes source URLs, last-checked wording, or “check the schedule” caveats.
- **Stale information judgement:** distinguish expired date residue from old-but-still-important visitor constraints. Remove or replace past events, expired offers, old exhibition dates, and other time-bound entries whose visitor value has ended. Do not remove stale access, safety, mobility, closure, route, ticketing, or facility warnings merely because the evidence is old; verify them, update them, or preserve the visitor-impacting risk with a fresh-check path until reliable evidence says the issue is resolved. If current truth cannot be established for a visitor-critical stale/missing fact, preserve the risk and do not treat cautious wording as confirmed resolution. Do not satisfy this by telling visitors to check the venue/site/source before travelling or before relying on Autoura guidance. Hold signoff if stale visitor-critical information has been deleted without replacement, verification, or a truthful maintenance path.
- **Preview facts quality:** `facts` must contain six discrete "before you go" preview facts. Keep them factual, visitor-useful, and separate from `brand_tip` / curator advice. Facts must not make the same point as `brand_tip` or `brand_tip_summary`; the curator tip owns the opinionated advice about what to choose, prioritise, or do. Avoid industry/framing words such as `hook` in public facts. Seasonal, volatile, or event-based facts are allowed when useful, but they need matching fresh maintenance.
- **Fresh quality:** most visits should have `fresh_frequency=quarterly` so stable information is still periodically checked. Use a faster cadence for changing external schedules, seasonal programmes, closure calendars, exhibitions, events, offers, date-sensitive ticketing, or other mutable sources. Leave `fresh_frequency` unset, or use a slower/non-quarterly cadence, only when there is a clear reason. Confirm `fresh_instructions` are actionable where a source or field-specific maintenance process exists. The instructions should identify the source(s), exact field(s) to update, what to extract, what special cases to preserve, and whether outdated dated entries should be removed.
- **Fresh scope:** `fresh_instructions` must describe how to keep the visit fields current, not how to maintain supporting stops. Do not approve visit `fresh_instructions` that tell workers to maintain named stops, dining venues, markets, shops, cafes, gardens, viewpoints, parking places, or other supporting objects as objects in their own right. Move those checks to the relevant stop/task flow, and keep only the visit-level consequence here, such as updating `dining.*`, `practical.scheduled_events`, `comingup.*`, `ticketing.*`, `planning.gotchas`, or `getting_there.*` if the supporting fact changes.
- **Configuration completeness:** required or materially important fields are configured for the visit type. Visit configuration should include useful visitor-facing summary, description, location, planning, ticketing, practical, getting-there, recommendations, scripts, timing, curated surrounding context, linked/available dining stops where relevant, and mandatory `dining.dietary` guidance. A normal visit should usually have a broad set of configured dining stops backing the dining guidance; dining availability must not be claimed unless the stops are actually configured. Character is recommended where it improves the experience, but a visit may be published without a character when the rest of the visit is ready.
- **Curation quality:** recommendations and surrounding context should favour good, interesting, distinctive, useful, or brand-appropriate choices, not simply the nearest matching place. Moments should be in the main visit structure/product, not hidden inside alternatives. If dining or other nearby places are named as part of the experience, they must be configured as stops. Empty recommendations are acceptable during factual configuration, but publication/signoff review must show that recommendations were explicitly flagged for brand-originator curation input.
- **Positive curation:** all live visit fields should primarily say what to do, choose, use, book, notice, or consider first. Do not fill any live field with rejected options, failed checks, closed candidates, unsuitable alternatives, or internal “what not to do” notes. Negative guidance belongs only where it materially helps the visitor avoid a real problem, and should be brief, visitor-facing, and secondary to the positive recommendation.
- **Dining quality and completeness:** dining context should normally be broad, useful, and backed by configured stops. Cover the main visitor dining needs where relevant: budget, standard, premium, picnic, refreshments, street food/markets, bars, convenience, speciality/local options, and mandatory dietary-specific options in `dining.dietary`, including halal, kosher, vegetarian, vegan, gluten-free, dairy-free, allergy-aware, or other labelled choices where relevant. Avoid fast food and chain coffee shops where better local or visitor-appropriate options exist. Halal and kosher may be genuinely rare, but vegetarian and other common dietary needs should not be missed; if normal dining coverage is missing, ask the brand originator or create a fix task. Do not describe a recommendation as "configured", "already published", "published", "nearest configured", "closest candidate", "nearby stop", "draft", or similar in visitor-facing dining text; those are internal system qualities, not visitor reasons. Choose the place because it is genuinely the best practical or curated visitor option, and explain that human reason instead.
- **Dietary dining check:** Hold signoff if `dining.dietary` is empty, generic, or only says to check menus. It must provide a useful visitor-facing dietary position, naming configured choices where suitable options exist or recording a supported no-useful-option position where research shows that is true. The check must explicitly consider halal, kosher, vegetarian, vegan, gluten-free, dairy-free, allergy-aware, and other relevant labelled needs for the visit context.
- **Dining stop references:** If `dining.*` free text names specific venues, each named venue must be configured as a stop and, where the consumer AI may need to resolve it, include an inline `[stop:<stop_id>]` reference. Do not rely on the venue appearing in a sampled nearby-food/display list; the stop reference is the grounding link.
- **Named real-place references:** If any AI-facing visit text names specific viewing spots, meeting points, route anchors, entrances, viewpoints, shops, markets, nearby attractions, picnic places, transport points, or other practical POIs, each named place should be configured as a stop where the consumer AI may need to resolve it, and include an inline `[stop:<stop_id>]` reference. Do not leave operational place guidance only as free text.
- **Pedestrian route grounding:** if `getting_there.pedestrian` or another visit field gives walking/arrival instructions using named stations, gates, streets, entrances, precincts, meeting points, route anchors, or other POIs, those anchors must be configured as stops/POIs and referenced inline where the consumer AI may need to resolve them. Hold signoff if instructions such as “walk from X to Y, then continue along Z” rely on named real places that are not configured or referenced. Either configure the POIs, simplify the wording to non-operational high-level guidance, or route a supporting-stop task.
- **Chain-dominated dining check:** Dining recommendations and visitor-facing dining text must not be dominated by familiar chains, fast food, chain cafes, or generic convenience options. Those options may be included only where they solve a real visitor need, such as an obvious fallback, late opening, accessibility, family practicality, predictable budget cover, or lack of better alternatives. Hold signoff if the main dining story is mostly chains while credible local, independent, partner, distinctive, or experience-appropriate places nearby have not been considered, configured, or explained.
- **Nearby dining relevance check:** Confirm linked dining stops are genuinely among the best options for this specific visit, not just pre-existing stops within a broad city radius. Check proximity to the actual visit location/entrance/exit, likely walking route, visitor dwell time, pre/post-visit usefulness, visitor fit, category coverage, source quality, and whether better closer options should be configured instead. Hold signoff if dining stops look like generic city leftovers rather than intentional visit-specific choices.
- **Dining free-text curation:** Confirm `dining.budget`, `dining.standard`, `dining.premium`, `dining.picnic`, `dining.refreshments`, `dining.street`, `dining.bar`, and `dining.dietary` each answer their own visitor question, not just list nearby places or state availability: cheapest useful option; best normal meal; best premium/special-occasion choice; whether picnic is recommended/fallback/not suitable and where supplies/seating exist; where to get coffee/snacks/a short pause; best street-food/market option and when it works; best drink/evening option with any age, booking, timing, walk-in, atmosphere, or suitability limits; and best dietary choices/limits for halal, kosher, vegetarian, vegan, gluten-free, dairy-free, allergy-aware, or other relevant labelled needs. Where suitable options exist, each free-text dining field should normally name at least three specific recommendations and explain why each is useful for this visit. Do not use area-level guidance as a substitute for recommendations: visitor-facing dining text should name specific places wherever suitable options exist, with area/neighbourhood context used only to explain location, route fit, or trade-offs between named recommendations. Each field should say what the visitor should actually do, why it fits this visit, and any trade-offs. Specific named recommendations are preferred over vague area-level guidance where they can be supported by configured stops. Hold signoff if dining text gives only a vague setting description, generic nearby summary, recommendation-free place list, or if useful named recommendations were flattened into generic neighbourhood advice merely because the supporting stops had not yet been configured.
- **Convenience store check:** If convenience or quick practical food is marked available, confirm the configured convenience stop includes the nearest genuinely usable convenience shop or small supermarket to the visit’s normal arrival/exit area, unless a closer option is unsuitable, closed, inaccessible, or otherwise not useful for visitors. Do not satisfy convenience coverage with a random existing city stop simply because it is already configured.
- **Picnic practicality check:** If picnic is marked available, confirm the visit has a usable picnic place/context and consider whether visitors also need a realistic nearby source of picnic food, drinks, snacks, or supplies. This can be a convenience shop, small supermarket, bakery, deli, market stall, café takeaway counter, speciality food shop, or other suitable food stop. Hold signoff only where picnic is presented as practical but the supporting food/supply context is missing, misleading, or too far away.
- **Alternatives quality:** alternatives should be genuinely different ways to experience the same venue instead of the standard ticket/visit, such as specialty access, abseiling, afternoon tea, bar/lounge products, or broader tours that include the venue. Do not use alternatives for main structures, main moments, generic filler, unavailable speculative experiences, or repeated information already covered elsewhere.
- **Structures quality:** structures should describe practical ways to organise and pace the base visit. They should include clear sequencing, duration guidance, and who the structure suits. Do not use structures for separately booked alternatives or premium products.
- **Moments quality:** moments must be available within the main/base visit product. Do not include a moment that requires a different ticket/product, separate booking, age eligibility, or materially different format unless it is clearly marked as specific to that alternative. For example, afternoon tea can be an alternative experience, but it is not a main visit moment unless the visitor has booked afternoon tea. Fail/hold publication or signoff review if `planning.structures` or `planning.moments` contain different-way alternatives already represented in `ticketing.alternatives`.
- **Gotchas quality:** gotchas should be specific, actionable, and genuinely useful. They should highlight things visitors commonly miss that may disrupt the visit. Do not use gotchas for generic travel advice.
- **Recommendations quality:** recommendations should be local, curated, useful, and clearly placed in pre-visit, during-visit, or post-visit. They must not duplicate ticketing, alternatives, audio tours, or distant/regional attractions.
- **Questions quality:** configured questions should be 4 plain visitor-facing prompts that expose useful decisions, comparisons, risks, or next actions for this visit. Do not approve questions with stop ids, resolver syntax, raw field names, internal labels, or generic wording that could apply to any venue. At least one question should connect to the brand objective or intended visitor behaviour where that objective is known. Every question must be answerable from the visit content; if a good question is not answerable, improve the relevant visit content or supporting stops/POIs before signoff, or replace the question. Seasonal questions require adequate fresh instructions, cadence, and notes that the question itself may need review over time.
- **Planning level / approach check:** If `planning.level = some`, confirm `planning.approach` describes the planning need for the **base visit**, not the existence of separate ticketing alternatives. Use `choice` only when the base visit itself has more valid things to do than time allows and visitors need help choosing a focus. Use `constraint` when timings, logistics, access, weather, peak periods, or booking windows shape the visit. Use `guided` when a suggested order improves the normal base experience. Different tickets/products, premium experiences, age-restricted options, hospitality, or separately booked formats belong in `ticketing.alternatives`, not as evidence for `choice`.
- **Originator setting protection:** do not silently delete, overwrite, or overrule brand-originator supplied, approved, or published settings when new evidence conflicts. Preserve the current value, record the conflict, and ask the brand originator for confirmation, preferably in the existing clarification/signoff email flow.
- **Source quality:** use official venue, supplier, brand, or first-party sources for factual content such as opening times, ticketing, access, location, facilities, and visitor instructions. Do not use third-party directories, review sites, or copied listing text as source material. Third-party pages may help discover that a place exists, but facts must be verified against official/first-party sources before configuration. This protects accuracy and avoids copyright or provenance problems.
- **Do not undermine Autoura guidance:** do not tell visitors to “check before travelling”, “check current availability”, “check days/times before relying on it”, or similar as a substitute for configured guidance. If a visit field references a configured stop, market, event, attraction, facility, access route, or other object with structured or maintained data in Autoura, use that data to decide what appears. If a visitor-critical fact is uncertain or outside Autoura’s current data, either give the truthful known visitor-impacting constraint without sending the visitor away to verify it, or hold/signpost the item for maintenance through `fresh_instructions` / the configuration workflow.
- **Presentation normalisation:** expected text normalisation, such as final full stops being stripped from short fields, is not a rejection reason.

If the visit is not good enough:

- Do not silently close the publish-request task as “not publishable” if the problem can be fixed.
- Update the publish-request task with a clear review note explaining what was checked and what failed.
- If the issue can be solved, create one or more fix tasks for Autoura or the originating brand, depending on who should supply or correct the information.
- Block or hold the original publish-request task while fix tasks are outstanding.
- Reject/close only when the visit is genuinely unsuitable, duplicate, not fixable, or should not be published.
