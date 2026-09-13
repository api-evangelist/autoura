# Character configuration

## Type

Execution

## Purpose

Define how to create and manage characters in Autoura.

## When to use

Use this when creating or updating a character.

## Knowledge base

This document is part of the Autoura knowledge base, used by the Autoura skill:

https://www.autoura.com/core/pai/skill.md

Refer to the skill for the full index and how documents connect.

---

## What this object is

A **character** is a consumer-facing AI persona used to deliver experiences, storytelling, or guidance.

Characters may:

- guide visitors
- provide narration or storytelling
- represent a tone, style, or perspective

---

## Why characters are needed

Characters are central to how experiences are delivered across the platform.

They are especially important because:

- **Autonomous vehicles and AI glasses** require a character to front the experience and provide a consistent recognisable presence
- **AI agents and voice interfaces** are more effective when led by a character with personality, rather than a default
  AI with no known background

A character provides:

- identity
- tone
- continuity
- trust

---

## Design ideas

Common character approaches include:

- **Historical characters**  
  e.g. real figures such as Admiral Lord Nelson

- **Fictional characters**  
  original or public-domain personas

- **Digital twins (human clones)**  
  based on real individuals, including:
    - influencers
    - tour guides
    - subject matter experts
    - can include voice cloning (e.g. via ElevenLabs)

Digital twins and voice clones require explicit documented consent. Do not suggest creating or using a living person's likeness, identity, or voice unless consent is clear and recorded.

More information on human digital twins:

https://www.autoura.com/docs/characters/humanclones

For further inspiration and examples:

https://www.autoura.com/docs/characters

---

## When to create vs reuse

Reuse when:

- a suitable standard character already exists

Create when:

- a new voice, style, or persona is required

Rules:

- do not create duplicate characters unnecessarily
- historical, mythical, and fictional (public domain) characters may be shared across brands
- the same character should not be created multiple times

Before proposing a new character, use `characters_search` to check for existing suitable characters. When a brand is known, prefer `usable_by_brand_id` so the search includes Autoura-owned, brand-owned, and publicly reusable characters available to that brand.

To avoid unnecessary duplication (for example, multiple versions of Santa Claus or King Henry VIII), characters are
created and managed centrally by Autoura humans. This ensures a consistent, unified set of characters that can be reused
across the platform.

---

## How to create a character

Characters are created by an Autoura human through the Autoura configuration workflow.

AI agents may suggest or prepare a character brief, but they should not assume they can directly create a character record.

Before suggesting a new character:

1. Search existing characters using `characters_search`
2. If the character is for a specific brand, use `usable_by_brand_id` to check whether a suitable character is already usable by that brand
3. If a similar character exists, use or update that character rather than requesting a duplicate

To request a new character:

- create a **configuration task**, or
- email **sahra@autoura.com**

---

## How to update a character

Characters are updated by an Autoura human through the Autoura configuration workflow.

AI agents may suggest updates or prepare revised character content, but they should not assume they can directly update a character record.

Before suggesting an update:

1. Use `character_get` to review the current character configuration
2. Check whether the requested change is a correction, an improvement, or a materially different persona
3. If the change would create a materially different persona, suggest a new character instead of overwriting the existing one

To request an update:

- create a **configuration task**, or
- email **sahra@autoura.com**

---

## Checklist for creating a character

A new character must include:

- **Name**  
  A clear and recognisable character name.

- **Archetype**  
  One of:
    - unset
    - historical
    - mythical
    - human_digital_twin
    - fictional_original
    - fictional_public_domain
    - fictional_protected

- **Nibble**  
  A short public-facing description.

- **Summary**  
  A fuller public-facing description.

- **Headshot photo**  
  Typically front-on, representing the character clearly.

- **Consent**
    - consent status must be set
    - consent notes must be provided

- **Prompts**
    - physical appearance and mannerisms
    - how they talk (style)
    - what they are expert on
    - what they should avoid
    - actual character reference (optional)

Notes:

- All mandatory content must be completed before the character is used.
- Consent is especially important where the character is based on a living person or a protected fictional character.

---

## Checklist for using a character

To use a character in a visit, route, or other experience, it must be usable by the relevant brand.

A character may be usable for one of these reasons:

- it is owned by Autoura
- it is owned by your brand
- it is publicly usable based on its archetype

Publicly usable archetypes:

- historical
- mythical
- fictional_public_domain

Rules:

- Autoura-owned characters must be enabled and have mandatory content complete
- publicly usable archetype characters must be enabled and have mandatory content complete
- brand-owned characters may be used within that brand when enabled, permitted, and suitable for the intended experience

---

## Tool usage

Use `characters_search` when:

- checking whether a suitable character already exists
- finding characters usable by a specific brand
- searching by character name, character ID, or voice ID
- avoiding duplicate public or reusable characters

Use `character_get` when:

- reviewing the full configuration of an existing character
- checking prompts, consent, photo, voice, archetype, or interaction style
- deciding whether a requested change is an update to an existing character or a new character proposal