# 030 – Game State

**Project:** Interactive Geopolitical Engine
**Document Version:** 1.0
**Status:** Draft
**Depends On:**

* 000 – Vision
* 010 – Engine Architecture
* 020 – Entity Model

---

# 1. Purpose

The Game State defines every piece of information that exists within the simulation at any point in time.

It is the authoritative representation of the world and acts as the single source of truth for every subsystem within the engine.

Whenever the simulation advances by one turn, every change made by the player, AI-controlled nations, random events, or internal systems is applied to the Game State.

Likewise, saving a game is simply the process of serialising the current Game State. Loading a game restores the Game State exactly as it existed when the save was created.

No system should maintain conflicting or duplicate representations of world information. If information exists in the simulation, it should exist within the Game State.

---

# 2. Design Principles

The Game State should follow the following principles.

## Single Source of Truth

Every piece of world information exists in only one location.

For example:

* Nation treasury values exist only within Nation State.
* Military units exist only within Military State.
* Diplomatic agreements exist only within Diplomatic State.

Other systems reference these values rather than storing their own copies.

---

## Deterministic Simulation

Given:

* the same Game State,
* the same player decisions,
* the same AI decisions,
* and the same random seed,

the engine should always produce identical outcomes.

This makes debugging, testing, balancing, and save-game reliability significantly easier.

---

## Persistent World

The world continues to evolve regardless of whether the player directly observes it.

Examples include:

* foreign elections,
* economic growth,
* military recruitment,
* diplomatic negotiations,
* espionage operations,
* natural disasters,
* technological research.

The simulation should never "pause" for nations outside the player's immediate awareness.

---

## Modular Structure

The Game State is divided into logical components.

Examples include:

* Global State
* Nation State
* Province State
* Military State
* Economy State
* Diplomacy State
* Intelligence State
* Technology State
* Population State
* Event State

Each subsystem is responsible only for its own data.

---

## Extensibility

New mechanics should be capable of being introduced without redesigning the existing Game State.

Examples include:

* cyber warfare,
* climate systems,
* insurgencies,
* space programs,
* pandemics,
* energy markets.

The architecture should support future expansion while maintaining backwards compatibility where practical.

---

# 3. Responsibilities

The Game State is responsible for storing:

* the current world situation,
* every nation,
* every province,
* every leader,
* every military unit,
* every diplomatic relationship,
* every treaty,
* every alliance,
* every intelligence network,
* every technology level,
* every active conflict,
* every economy,
* every population,
* every ongoing event.

The Game State does **not** decide how these values change.

Instead:

* the Turn Engine updates them,
* AI systems modify them,
* player decisions modify them,
* simulation systems calculate new values.

The Game State stores information only.

---

# 4. Relationship to Other Documents

This specification defines **what information exists**.

It does **not** define:

* how combat works,
* how diplomacy functions,
* how AI makes decisions,
* how economies are calculated,
* how logistics operate.

Those behaviours are defined within their respective design documents.

This document instead specifies the information that those systems read and modify.

---

# 5. High-Level Structure

The complete Game State is composed of multiple major sections.

```
Game State
│
├── Global State
├── Nation State
├── Province State
├── Military State
├── Economy State
├── Diplomacy State
├── Intelligence State
├── Technology State
├── Population State
└── Event State
```

Each section will be defined in detail within the following chapters of this specification.

---

# End of Section 1

The following section defines the Global State, which contains all information that applies to the simulation as a whole rather than to any individual nation.
# 6. Global State

## Purpose

The Global State contains all information that applies to the simulation as a whole.

Unlike Nation State or Military State, these values are not owned by any individual entity. Instead, they describe the current status of the world itself and provide the context within which every subsystem operates.

The Turn Engine reads the Global State at the beginning of every turn and updates it before processing nation-specific actions.

---

# 6.1 Core Global Variables

The following variables exist for every scenario.

| Variable           | Type    | Description                                            |
| ------------------ | ------- | ------------------------------------------------------ |
| Current Turn       | Integer | Current simulation turn beginning at Turn 1.           |
| Current Date       | Date    | In-game calendar date.                                 |
| Scenario ID        | String  | Identifier for the active scenario.                    |
| Difficulty         | Enum    | Selected AI difficulty level.                          |
| Random Seed        | Integer | Seed used to ensure deterministic simulation outcomes. |
| Simulation Version | String  | Engine version used to create the save file.           |

---

# 6.2 Calendar System

The engine supports multiple calendar models depending on the scenario.

Examples include:

* Modern-day calendar
* Historical calendar
* Custom fictional calendar

Each scenario specifies:

* starting date,
* turn length,
* leap year rules (where applicable),
* seasonal effects.

Example:

```text
Start Date:
1 January 2026

Turn Length:
7 Days

Turn 15:

15 April 2026
```

Future scenarios may choose different turn lengths without requiring modifications to the engine.

Examples:

* Daily turns
* Weekly turns
* Monthly turns

---

# 6.3 Difficulty

Difficulty affects the quality of AI decision-making rather than providing artificial bonuses.

Difficulty may influence:

* strategic planning,
* diplomatic behaviour,
* military coordination,
* economic efficiency,
* long-term planning,
* risk assessment.

Difficulty should not directly modify:

* resource production,
* combat statistics,
* troop strength,
* research speed.

This maintains fairness while increasing strategic challenge.

---

# 6.4 Random Seed

Every simulation is assigned a Random Seed when created.

The Random Seed ensures that:

* random events,
* AI behaviour,
* weather generation,
* economic fluctuations,
* political developments,

remain reproducible when identical inputs are supplied.

This supports:

* debugging,
* multiplayer synchronisation (future),
* save reliability,
* automated testing.

---

# 6.5 Global Tension

Global Tension represents the overall level of geopolitical instability.

It is a derived value calculated from world events rather than a manually assigned statistic.

Factors that may increase Global Tension include:

* wars,
* military mobilisation,
* nuclear threats,
* sanctions,
* terrorism,
* political crises,
* failed states,
* resource shortages.

Factors that may decrease Global Tension include:

* peace treaties,
* arms reduction agreements,
* humanitarian cooperation,
* successful diplomacy,
* international mediation.

Global Tension influences:

* AI aggression,
* alliance formation,
* defence spending,
* public opinion,
* market confidence.

---

# 6.6 Global Economy Index

The Global Economy Index provides a high-level measure of worldwide economic performance.

It aggregates:

* international trade,
* commodity prices,
* financial confidence,
* inflation,
* shipping activity,
* energy markets.

Individual national economies remain independent.

The index exists primarily to model worldwide economic shocks.

Examples:

* financial crises,
* oil embargoes,
* global recessions,
* supply chain disruptions,
* commodity booms.

---

# 6.7 Active Global Events

Certain events affect multiple nations simultaneously.

Examples include:

* pandemics,
* climate disasters,
* refugee crises,
* financial crashes,
* asteroid impacts,
* volcanic eruptions,
* global cyber attacks,
* major terrorist incidents.

Each event records:

* unique identifier,
* affected regions,
* severity,
* start date,
* expected duration,
* current status.

Events are processed each turn by the Event System.

---

# 6.8 World Configuration

Each scenario defines immutable configuration values.

Examples include:

* map dimensions,
* province definitions,
* resource distribution,
* climate zones,
* ocean regions,
* mountain ranges,
* navigable rivers.

These values normally remain constant throughout the simulation.

---

# 6.9 Victory Configuration

The Global State stores victory conditions for the active scenario.

Possible victory models include:

* sandbox (no victory),
* score-based,
* conquest,
* diplomatic,
* economic,
* survival,
* scenario-specific objectives.

Victory evaluation occurs at the conclusion of each turn.

---

# 6.10 Save Metadata

Every saved Game State includes metadata to ensure compatibility.

Metadata includes:

* save file version,
* engine version,
* scenario version,
* creation date,
* last played date,
* total turns completed,
* checksum,
* optional compression format.

This information assists with save validation and future migration between engine versions.

---

# Summary

The Global State establishes the overall context in which the simulation operates. It defines the current point in time, the scenario configuration, the world-wide strategic environment, and other information shared across all nations.

Subsequent sections describe the information owned by individual nations and other entities within the simulation.
