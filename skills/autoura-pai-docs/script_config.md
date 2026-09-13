# Script configuration

## Type
Execution

## Purpose
Define how to create and manage storytelling scripts in Autoura.

## When to use
Use this when creating or updating a script.

## Knowledge base

This document is part of the Autoura knowledge base, used by the Autoura skill:

https://www.autoura.com/core/pai/skill.md

Refer to the skill for the full index and how documents connect.

---

## What this object is

A **script** is storytelling content performed by a character.

Scripts define:

- what the character says
- how the experience is delivered
- narrative, tone, and structure

Scripts are used to deliver guided, narrated, or immersive experiences.

The delivery of a script is called a **performance**.

---

## Creation and management

Scripts are created and updated by humans using the Autoura Dashboard.

- AI agents **do not create or update scripts directly**
- AI agents may:
    - recommend creating a script
    - suggest improvements
    - select or perform existing scripts

To read more:
https://www.autoura.com/docs/characters/scripts

---

## When to create vs reuse

Create when:

- you need new content, storytelling, or a different narrative
- the experience, context, or audience changes
- interaction design differs (e.g. story vs workshop)

Reuse when:

- the same content can be used across multiple contexts
- only minor contextual changes are required

Important:

- scripts are content-driven and reusable across visits, routes, or stops
- avoid duplicating scripts unless the content meaningfully differs

---

## Script types and interaction

Scripts support different levels of interaction and personalisation:

### Story
- Interaction: none
- Personalisation: none
- Linear narration (similar to traditional audio guides)

### Workshop
- Interaction: high
- Personalisation: yes
- Guided learning or quizzes requiring user participation

### Questions
- Interaction: high
- Personalisation: yes
- One or more questions to engage the guest

### Conversation starter
- Interaction: medium
- Personalisation: yes
- Opens a topic and encourages dialogue

### Message
- Interaction: none
- Personalisation: limited
- Simple message or prompt

### MoveMe
- Interaction: none
- Personalisation: not yet
- Plays a MoveMe experience

### Video
- Interaction: none
- Personalisation: none
- Plays external video (e.g. YouTube or Vimeo)

---

## Interaction principles

Interaction is a defining feature of Autoura experiences.

- avoid purely passive delivery unless intentional
- prefer engagement where appropriate
- the goal is not perfect human simulation, but sustained attention

Contrast:

- traditional audio tours → one-way, static
- Autoura scripts → responsive, adaptive, contextual

---

## Performance and placement

Scripts are performed in different contexts:

- within a **character**
- within a **route**
- standalone (e.g. QR code entry point)

When placing a script:

- consider timing and duration
- align with what the guest is doing physically
- avoid long content when attention is limited

Example:

- do not trigger a 5-minute story when the guest is about to move locations

---

## Personalisation and context

Scripts should respect guest context:

- preferences (e.g. dietary, accessibility)
- environment (location, timing)
- intent (learning vs exploration vs entertainment)

Example:

- avoid alcohol-related content for non-drinkers
- adjust tone based on audience type

The platform handles much of this, but scripts should still be written with awareness.

---

## Updating live scripts

Be careful when editing scripts that are already in use.

Risks:

- breaking in-progress performances
- inconsistent experiences

Recommended approach:

1. leave the script enabled
2. remove it from characters/routes
3. edit or create a new version
4. reattach to characters/routes

---

## Key principles

- scripts power the **experience layer** of Autoura
- characters perform scripts, not the other way around
- interaction should be intentional, not accidental
- reuse is preferred over duplication
- context (time, place, audience) matters as much as content