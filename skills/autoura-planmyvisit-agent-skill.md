# PlanMyVisit — agent operating guide

PlanMyVisit combines **curated local knowledge and one centrally shared visit plan**, used before and during a visit
through our website and compatible AI services. It is Autoura’s consumer service for doing a visit: preparing together,
answering practical questions, deciding what is next and recording shared progress.

For business tasks such as managing venue data or travel products, use [Autoura B2B agent documentation](https://www.autoura.com/core/pai/skill.md).

## Do first

1. **Authenticate through the appropriate connection.** Use the MCP email-code flow below for a direct agent
   connection, or the signed-in website session for WebMCP. Never pretend to have access if authentication fails.
2. **Discover current tools.** For MCP, call `tools/list` at the start of each session. For WebMCP, inspect the tools
   exposed in the current browser session. Follow live schemas; do not hard-code tool availability or assume old schemas.
3. **Check for an existing plan.** Use `visitplan_get` if its ID is known; otherwise use `visitplans_search` and fetch
   the matching plan. Clarify ambiguous matches. Resolve expected access problems rather than creating a replacement.
4. **Load preferences.** Use `preferences_get` for the authenticated human. Save explicitly known ongoing changes
   with `preferences_update`; do not repeat unchanged values. Use others' preferences only within granted sharing access.
5. **Resolve the venue only when needed.** For a new visit or question without a known venue, use
   `visit_venues_search`, then `visit_knowledge_get`. For an existing plan, use its venue and retrieve knowledge as needed.
6. **Create, refine or resume.** Create with `visitplan_new` only when there is no existing plan for this visit.
   Otherwise continue the same `visitplan_id`. Use the phase-specific workflow below and retrieve current details after changes.

These are decision steps, not instructions to call every tool on every interaction. Read the relevant reference before
using an unfamiliar operation: [authentication](docs/ai/authentication.md), [selected tools](docs/ai/tools.md),
[concepts and trust](docs/ai/concepts.md).

## Connect and authenticate

**MCP:** connect to `https://api.autoura.com/api/mcp` using `Authorization: Bearer {access_token}` and
`Content-Type: application/json`. For an existing or unknown account, POST to
`https://api.autoura.com/api/identity/open/signin_code_send` with `email` (the human email or configured agent email).
If the account is not found, POST to `/api/identity/open/register` on the same host with `human_email`, `name_f`,
optional `pai_email` and `lang`. Registration sends a code; an account-exists response means use sign-in instead.

POST to `/api/identity/open/verify` with the same `email` and `proposed_security_code`. Codes arrive from **Autoura**,
are valid for 60 minutes, and must come from the receiving inbox. Use authorised inbox access or ask the human for the
code. Store the returned `access_token` securely, never the code. Tokens represent the human account owner and last
48 hours; reuse them, re-authenticating when expired, unavailable or rejected with 401. See the
[full request examples and error handling](docs/ai/authentication.md) when onboarding or restoring access.

**WebMCP:** the human signs in or signs up at [PlanMyVisit WebMCP](https://www.planmyvisit.to/webmcp). Enter the six-digit email code on the WebMCP page. Request a new code if it expires. WebMCP uses the trusted website session, not a browser-supplied profile ID, and does not support agent self-registration. It exposes the same current tool set as MCP. See [browser setup](https://www.planmyvisit.to/about/instructions/protocols#webmcp).

If authentication fails, offer non-personalised guidance without claiming access to private plans or preferences.

## Continue one shared plan

A person can prepare on the website and switch to a compatible AI during the visit, retaining the same plan.
Different people on the same visit can also use different services together: one may use an AI while another uses
our website. Both work from the same central record, subject to their own authentication and permissions.

**A change of assistant, conversation or service is not a new visit.** Retain the `visitplan_id` and retrieve the
current plan when resuming. Shared plan access does not grant unrestricted access to private preferences or location,
and does not transfer private conversation history between services.

### Before the visit

- Combine the people's authorised preferences and visit-specific priorities with venue knowledge. Gather the timing
  and practical needs for this visit; answer questions about access, pacing, dining and ticketing where supported.
- Create a new plan only after the existing-plan check. Otherwise refine the same plan through supported fields.
  Every plan needs a guide: use the venue's suggested guide, and call `characters_search` only if another is requested.
  If a specific guide is not chosen, send an empty string: the plan uses the visit's active calculated guide, or Sahra
  when no usable visit guide is available (including no visit ID). This choice is saved; do not offer no guide.
- Keep input in the right place: lasting preferences in `preferences_update`, visit-specific priorities in
  `visitplan_member_outcomes_update`, and other shared conversational input in `visitplan_note_new`. Use
  `visitplan_update` for supported structural changes such as dates, group context or guide.
- Retrieve the resulting plan and explain the relevant arrangements. Preserve its ID so the group can resume it later.

### During the visit

- Retrieve the same plan afresh, including state, calculated moments and shared completion. Help answer “what's next?”
  using current plan content, venue knowledge and the time, energy or circumstances people provide.
- To begin an imminent visit, use `visitplan_update` with `state: active` **alone**, within the allowed timing window
  and role permissions. The lead or a confirmed participant can activate while preparing to leave or travelling there.
- Record a moment done or not done with `visitplan_moment_update` only while the plan and attendee are active.
  Completion is shared; other services see the same result when they retrieve the plan.
- Adapt using supported structural fields, visit priorities and shared notes. Save an ongoing preference only when
  it is actually lasting. Retrieve the plan after updates and follow any refresh requirements.
- Moments, to-dos and gotchas are calculated outputs, not directly editable planning inputs. Moment updates change
  completion only. Do not claim automatic monitoring, live conditions or location access that the tools have not provided.

## Essential operating rules

- Respect explicit consent and live tool permissions. A person may update their own visit priorities; the current
  lead may update anyone's priorities. Use the required current update key and honour `refresh_required` responses.
- Load preferences before personalised advice. Use explicitly stated information or reliable authorised context;
  never infer or silently save sensitive needs or identity. Never put a companion's preferences into the human's profile.
- Keep one-off requests with the visit. “Take today slowly” is visit input; “I usually prefer a relaxed pace” may be a
  lasting preference. Omit unknown fields, do not clear requirements without instruction, and avoid redundant updates.
- Apply supported updates when clearly requested; confirm high-risk date or plan-language changes. Obtain explicit
  confirmation for consequential companion actions, including sending/accepting invitations, blocking/disconnecting,
  or changes to location sharing. Friendship alone does not authorise precise location access.
- Never expose credentials or persist verification codes. Do not claim access beyond what the tool returns.
  Account management belongs to the human at [Autoura.me](https://www.autoura.me); agents cannot delete accounts,
  change `human_email` or `pai_email`, or use `pai_email` for human-facing sign-in.
- Personal-data export requires an explicit request and confirmation to email it. It is not routine planning.
  Plan deletion is permanent and allowed only in supported states; respect explicit human consent.
- Use curated venue knowledge for practical answers and provenance when evidence is requested. Report missing,
  incorrect, outdated or unclear venue information through `content_gap_report`; never fabricate venue facts.
  Report capability gaps with `product_gap_report` before offering a workaround. Follow the
  [feedback-tool details](docs/ai/tools.md#feedback--gap-reporting-tools).

## Scope and evidence

Use PlanMyVisit for one visit window, usually 3–5 hours and potentially from an hour to a full day, at a place such as
an attraction, museum, neighbourhood or dining area. It supports preparation, continuing shared plans, practical
questions, nearby dining and in-visit decisions. Coverage is global, with knowledge depth varying by destination.

It does not plan multi-day holidays, package tours or transport across a whole trip, and makes no booking or commercial
commitment. Use its ticketing guidance and relevant alternatives where available; another service completes bookings.

Our knowledge is shaped by disclosed curator organisations with relationships to the places they cover, backed by
provenance, ongoing checks and feedback. See [concepts and trust](docs/ai/concepts.md) and the
[live evidence page](https://www.planmyvisit.to/trust).

For integration issues or ambiguous instructions, contact **sahra@autoura.com** or **hello@autoura.com**.
Human setup instructions: [Connect an AI](https://www.planmyvisit.to/about/instructions).
