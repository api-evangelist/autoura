# Stop configuration

## Type
Execution

## Purpose
Define how to create and manage stops in Autoura.

## When to use
Use this when creating or updating a stop.

## Knowledge base

This document is part of the Autoura knowledge base, used by the Autoura skill:

https://www.autoura.com/core/pai/skill.md

Refer to the skill for the full index and how documents connect.

---

## Important, Summary and description formatting

`important`, `nibble`, `summary` and `description` are public visitor-facing text. They should be easy to scan, especially on mobile.

If a field is over **400 characters**, it must include at least one paragraph break: two newline characters between paragraphs. Do not leave long summary or description text as one dense block.

Use paragraph breaks to separate distinct ideas, such as what the place is, what visitors do there, and practical or experience-shaping context.

## What this object is

A **stop** is an individual real-world experience point.

This may include:

- point of interest
- food & drink venue
- attraction
- shop
- tour or activity
- ticketed event
- accommodation

Stops are used within visits and routes.

---

## When to create vs reuse

- Stops should be **unique across the entire platform**
- Duplicate stops must not be created

Create when:

- the stop does not already exist

Reuse when:

- the stop already exists

Important:

- always check for an existing stop before creating a new one

Expected endpoint order: first evaluate the proposed stop, then create it with *_new only if it does not already exist, use *_update for later changes, call *_publish_request when it appears ready for review, and call *_publish only after the publish review has approved it.

---

## Creation and configuration-task flow

Use `stop_evaluate` before `stop_new`. Create a stop only when the evaluation confirms that the stop is unique, suitable, recognisable, and accepted for creation.

When calling `stop_new`, use `self_configure` to say who will configure the stop after creation:

- `self_configure: false` — default/normal. The caller is **not** configuring the stop directly. Autoura creates the new stop and starts the normal stop-configuration task flow.
- `self_configure: true` — the caller is taking responsibility for configuring the stop directly with `stop_update`. Autoura does **not** create a separate configuration task for that stop.

Use `self_configure: true` only when the current worker or external agent really will complete the stop configuration work themselves. Otherwise leave it false so the stop enters the normal Autoura configuration workflow.


## Protect brand-originator settings

Brand-originator supplied, approved, or already-published settings must not be silently deleted, overwritten, or overruled just because a worker finds conflicting evidence or a tool suggests a different value. This is especially important once a stop or visit has been published, but it also applies before publication when the originator has clearly supplied a preference or setting.

If a field is missing and can be filled from official/first-party evidence, configure it normally. If a field already has a value and new evidence suggests a different value, preserve the existing value and treat the conflict as an originator question rather than an automatic correction.

Examples:

- If the configured location is X but `address_to_geocode` or another source suggests Y, do not immediately move the stop/visit to Y. Ask the brand originator whether X or Y is correct.
- If the originator has preferred dining, partner venues, meeting points, or visitor advice, do not replace it with generic nearby research without checking.
- If opening times, ticketing, restrictions, suitability, or access details conflict with current official evidence, record the conflict and ask for confirmation before overwriting originator-provided or published content.

The right action is to add the question to the current originator clarification/signoff list, or send the planned originator email if that is the current workflow step. Avoid sending many isolated emails for small conflicts unless the conflict is a real blocker.

## Stale information handling

Do not treat all stale stop information the same way. First decide whether the old information is:

1. **Stale event/date residue** — a past event, expired offer, old exhibition date, old special opening, past seasonal programme, or other time-bound item whose visitor value has ended. Remove or replace it with current/future information. Do not leave past dates in live stop content just because they were once true.
2. **Stale but still potentially protective or operationally important** — an access constraint, lift outage, closure, restricted entrance, safety rule, route disruption, missing facility, ticketing restriction, or similar visitor-impacting condition that may still be true. Do not delete it merely because the last evidence is old. Verify it if possible, update it if current evidence is available, or keep/phrase it as a current caution until it is confirmed resolved.

For example, “lift out of action in February 2026” should not simply be removed from an accessibility-relevant stop because February has passed. The correct action is to establish whether the lift is now working. If current status cannot be established, preserve the visitor-impacting access risk in the relevant important/practical/accessibility content or route a fresh-check/fix path. Do not tell visitors to check with the venue before travelling or before relying on Autoura guidance; Autoura content should give the usable answer or hold a clear maintenance path for resolving the unknown. By contrast, “special half-term aircraft talk on 18 February 2026” should be removed or replaced after the date has passed.

If stale information is material to accessibility, safety, mobility, ticketing, closures, or whether a visitor can use the stop, preserve the risk until there is reliable evidence that the issue no longer applies.

Preserving the risk in visitor-facing wording is not the same as resolving the underlying unknown. If current truth cannot be established for a visitor-critical stale/missing fact, preserve the risk in visitor-facing content and keep a clear fresh-check or maintenance path where one is available. Do not present cautious wording as a confirmed resolution of the underlying fact. Visitor-facing fields must not offload the work to the visitor with wording such as “check with the venue before travelling”, “check current availability”, or “check before relying on this”; that belongs in Autoura maintenance/fresh instructions, not in stop guidance.

## Stop availability

Some stops have date-based availability.

The main configurable date types are:

- `shop` — availability is based on regular opening hours, special dates, and blackout dates
- `tour` — availability is based on scheduled tour/product availability
- `hotel` — availability is based on accommodation availability
- `attraction` — availability is based on attraction-style availability

For normal venues, shops, restaurants, attractions, accommodation facilities, points of interest with opening hours, and places whose public use depends on regular opening or access hours, use `shop` availability.

For POI stops specifically: if the point of interest has opening hours or access windows, configure it with `date_type: "shop"` and the shop-style availability arrays. 

---

## Shop-style availability

Shop-style availability uses three date arrays:

1. regular opening hours
2. special dates
3. blackouts

These are applied in priority order:

1. **Blackouts** — highest priority
2. **Special dates**
3. **Regular opening hours** — fallback/default pattern

This means:

- if a date is covered by a blackout, the stop is closed, even if regular hours or special hours exist
- if a date has a special date entry, that special date overrides the regular weekly hours
- if neither a blackout nor a special date applies, the regular weekly hours are used

---

## Shop-style availability field guide

Shop-style availability uses three structured date arrays. Use these fields for normal venues, shops, restaurants, attractions, accommodation facilities, and places with opening hours.

Priority order:

1. `date_shop_blackout_array` - highest priority; the stop is closed even if other hours exist
2. `date_shop_special_array` - overrides regular weekly hours on a specific date
3. `date_shop_regular_array` - fallback/default weekly pattern

### `date_shop_regular_array`

Use this for the normal weekly opening pattern.

Read the examples only if you are configuring or reviewing this field: https://www.autoura.com/core/pai/docs/stop_date_shop_regular_array.md

Quick checks:

- each row is one opening period on one day of the week
- use one row per open day unless there are genuinely separate opening periods, such as lunch and dinner
- use `00:00` to `24:00` for all-day opening
- split after-midnight opening into separate rows rather than using a close time earlier than the open time

Required row shape:

```json
{
  "day_of_week": "monday",
  "actual_open_time": "09:00",
  "actual_close_time": "17:00",
  "show_start_time": "",
  "show_end_time": "",
  "sort_order": "1"
}
```

### `date_shop_special_array`

Use this for one-off opening hours on dates where the stop is open but does not follow normal weekly hours.

Read the examples only if you are configuring or reviewing this field: https://www.autoura.com/core/pai/docs/stop_date_shop_special_array.md

Quick checks:

- use for bank holiday hours, Christmas Eve hours, late-night openings, one-off event opening, or seasonal special days
- do not use this for closures; use `date_shop_blackout_array` instead
- a blackout overrides a special date

Required row shape:

```json
{
  "date": "2026-12-24",
  "note": "Christmas Eve hours",
  "actual_open_time": "10:00",
  "actual_close_time": "16:00",
  "show_start_time": "",
  "show_end_time": ""
}
```

### `date_shop_blackout_array`

Use this for closure dates and closure ranges.

Read the examples only if you are configuring or reviewing this field: https://www.autoura.com/core/pai/docs/stop_date_shop_blackout_array.md

Quick checks:

- blackouts take priority over both special dates and regular opening hours
- use for Christmas Day closure, refurbishment periods, seasonal closure, private hire closure, one-off closure, or temporary maintenance closure
- `end_date` is inclusive
- use `start_date` and `end_date`; do not use a single `date` field for blackouts

Required row shape:

```json
{
  "start_date": "2026-12-25",
  "end_date": "2026-12-25",
  "note": "Closed for Christmas Day"
}
```

### Choosing the right availability field

Read this when deciding between regular hours, special dates, and blackouts, or when checking time format rules: https://www.autoura.com/core/pai/docs/stop_shop_availability_decisions.md

### MCP payload example

Read this when constructing or reviewing a `stop_update` payload for shop-style availability: https://www.autoura.com/core/pai/docs/stop_shop_availability_payload.md

MCP notes:

- always use a fresh `stop_update_key` from `stop_get` before calling `stop_update`
- `stop_update` is partial: omitted fields are left unchanged
- include `date_type: "shop"` when setting shop arrays unless the stop is already known to have effective `date_type: shop`
- do not send plain strings in the shop arrays; use the object shapes above

---

## Publishing quality guidance for shop availability

Before requesting publication, check that shop availability is understandable and useful.

Good availability should:

- include regular weekly hours where known
- be configured in the structured date fields, not only described in free text such as `important` or `description`
- produce future availability in `stop_get`; after updating dates it can take a few minutes for calculated fields such as `has_future_date`, `next_available_date`, and `available_days` to refresh, but before signoff the stop should show future availability unless it is genuinely unavailable
- treat future availability as generally required before publication or signoff. A narrow exception may apply for credible recurring offers, annual events, seasonal markets, or similar recurring products where there is strong evidence the offer will run again but future dates have not yet been announced. In that case, record the evidence and reasoning; do not use the exception for offers that appear discontinued, permanently closed, replaced, or no longer real
- use specials only for actual one-off date changes
- use blackouts for closures
- avoid impossible time ranges
- avoid duplicate rows unless there is a genuine break in service
- use `24:00` correctly as the end of a day
- split after-midnight openings across two days
- include public notes only where they help visitors
- keep blackout notes internal and operational

---

## Food inclusion fields

Food inclusion booleans such as `includes_meat`, `includes_fish`, `includes_eggs`, `includes_dairy`, `includes_gluten`, `includes_alcohol`, and similar fields describe whether that item is a **dominant or defining part** of the stop's food/drink offer.

Do **not** set these to true merely because the venue might offer an item somewhere on the menu. A general restaurant should not automatically be marked `includes_meat=true` just because some dishes contain meat.

For markets, food halls, festivals, pop-ups, and other multi-vendor stops, do not set ingredient fields from example stalls or products. A general farmers market should not be `includes_dairy=true` just because cheese may be sold, or `includes_gluten=true` just because bread may be sold.

Set the field to true only when the item defines the overall stop experience. For example:

- a steakhouse can be `includes_meat=true`
- a seafood restaurant can be `includes_fish=true`
- a wine tasting can be `includes_alcohol=true`
- an ice-cream parlour can be `includes_dairy=true` unless it is clearly dairy-free/vegan
- a cheese market can be `includes_dairy=true`

When unsure, leave the field false unless official/first-party information makes the dominant offer clear.

## Publishing checklist

Use this checklist when reviewing a stop publish request.

Publishing normally starts from a publish-request task. The reviewer should assess the stop, update the task with the decision, and either request publication, block the task for fixes, or create follow-up tasks.

Before accepting a stop for publication, check:

- **Quality benchmark:** compare the stop against a known good published example. For visits, Solent Sky in Southampton (`visit-9b6d23b05e5b35b`) is the current quality benchmark; for stops, use an equivalent good stop when available. The aim is complete, useful, visitor-facing fields rather than bare minimum data.
- **Uniqueness:** the stop is not a duplicate of an existing stop. This should normally have been checked before reaching publication review, but if a duplicate is found, do not publish.
- **English source content:** configurable text is written in clear English source text. The platform may translate this for visitors.
- **No configuration placeholders:** Do not publish, request signoff, or treat a visit as content-complete if visitor-facing fields still contain setup language such as “not configured”, “bare-minimum pass”, “to be added later”, “TBD”, “placeholder”, or similar internal workflow residue. Replace it with truthful visitor-facing content, leave the field empty where allowed, or mark the task blocked if the information is genuinely missing.
- **Important field:** `important` is for genuinely important pre-visit information not already captured elsewhere in the stop configuration. It is often empty. Do not duplicate opening hours, availability dates, location details, accessibility settings, food attributes, or other structured fields here.
- **Visitor usefulness:** name, nibble, summary, description, location, website, opening/availability, suitability, and relevant stop-type fields are useful enough for a visitor to understand what the stop is and why/when to use it.
- **Configuration completeness:** required or materially important fields are configured for the stop type. Shop-style availability must use structured date arrays and should show future availability in `stop_get` before signoff; it can take a few minutes after date updates for calculated future-date fields to refresh. Future availability is generally required, except for credible recurring future offers where future dates have not yet been announced and the evidence/reasoning has been recorded. Location should be usable by visitors.
- **Food inclusion fields:** for Food & Drink stops, `includes_meat`, `includes_fish`, `includes_insects`, `includes_eggs`, `includes_honey`, `includes_dairy`, `includes_gluten`, and `includes_alcohol` mean the item is dominant or defining for the stop, not merely available somewhere. For markets, food halls, festivals, pop-ups, and other multi-vendor stops, do not set these fields from example stalls or products unless the overall stop is specifically defined by that item.
- **Stale information judgement:** distinguish expired date residue from old-but-still-important visitor constraints. Remove or replace past events, expired offers, old exhibition dates, old special openings, and other time-bound entries whose visitor value has ended. Do not remove stale access, safety, mobility, closure, route, ticketing, or facility warnings merely because the evidence is old; verify them, update them, or preserve the visitor-impacting risk until reliable evidence says the issue is resolved. If current truth cannot be established for a visitor-critical stale/missing fact, preserve the risk and do not treat cautious wording as confirmed resolution. Do not satisfy this by telling visitors to check the venue/site/source before travelling or before relying on Autoura guidance. Hold signoff if stale visitor-critical information has been deleted without replacement, verification, or a truthful maintenance path.
- **Originator setting protection:** do not silently delete, overwrite, or overrule brand-originator supplied, approved, or published settings when new evidence conflicts. Preserve the current value, record the conflict, and ask the brand originator for confirmation, preferably in the existing clarification/signoff email flow.
- **Source quality:** use official venue, supplier, brand, or first-party sources for factual content such as opening times, ticketing, access, location, facilities, and visitor instructions. Do not use third-party directories, review sites, or copied listing text as source material. Third-party pages may help discover that a place exists, but facts must be verified against official/first-party sources before configuration. This protects accuracy and avoids copyright or provenance problems.
- **Fact gathering:** find facts from official/first-party sources when they are reasonably available. The brand originator may be asked for help with facts that are unclear, missing, specialist, or brand-specific, but should not be asked to supply information that Autoura can readily verify itself.
- **Presentation normalisation:** expected text normalisation, such as final full stops being stripped from short fields, is not a rejection reason.
- **Website URL:** use the stop’s own official homepage or canonical public page where available. An official brand/supplier store-locator or location page is acceptable when it is the canonical official page for that specific stop. Do not use narrow event, offer, booking-step, PDF, social, map-listing, review-site, directory, or aggregator URLs as the primary website URL when a suitable official homepage/canonical page exists. Put specific product, event, ticket, menu, accessibility, offer, or booking URLs in the specific field they support rather than using them as the stop’s main website.

If the stop is not good enough:

- Do not silently close the publish-request task as “not publishable” if the problem can be fixed.
- Update the publish-request task with a clear review note explaining what was checked and what failed.
- If the issue can be solved, create one or more fix tasks for Autoura or the originating brand, depending on who should supply or correct the information.
- Block or hold the original publish-request task while fix tasks are outstanding.
- Reject/close only when the stop is genuinely unsuitable, duplicate, not fixable, or should not be published.
