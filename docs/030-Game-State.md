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
# 7. Nation State

## Purpose

The Nation State represents every sovereign political entity that exists within the simulation.

Each nation maintains its own independent political, economic, military, diplomatic and technological state.

The Nation State is the primary object manipulated by both the player and AI-controlled governments throughout the simulation.

Every nation possesses its own objectives, capabilities, relationships and internal conditions that evolve independently over time.

---

# 7.1 Nation Identity

Every nation possesses a permanent identity.

| Field            | Type            | Description                                    |
| ---------------- | --------------- | ---------------------------------------------- |
| Nation ID        | UUID            | Permanent internal identifier.                 |
| Official Name    | String          | Full legal name of the nation.                 |
| Short Name       | String          | Display name used by the interface.            |
| Government Name  | String          | Current governing administration.              |
| Flag             | Asset Reference | National flag.                                 |
| Capital Province | Province ID     | Current capital.                               |
| National Colour  | RGB / Hex       | Map rendering colour.                          |
| Founded Date     | Date            | Historical or scenario-specific founding date. |

The Nation ID never changes throughout the simulation.

---

# 7.2 Government

Each nation possesses a government responsible for strategic decision making.

Government information includes:

| Field                | Description                               |
| -------------------- | ----------------------------------------- |
| Government Type      | Democracy, Monarchy, Military Junta, etc. |
| Head of State        | Current national leader.                  |
| Head of Government   | Executive leader where applicable.        |
| Cabinet Members      | Senior ministers and advisors.            |
| Election Cycle       | Frequency of elections.                   |
| Next Election        | Scheduled election date.                  |
| Government Stability | Overall political stability.              |
| Corruption Rating    | Administrative corruption level.          |

Government changes may occur through:

* elections,
* coups,
* revolutions,
* constitutional reforms,
* foreign intervention.

---

# 7.3 National Statistics

Every nation maintains high-level statistics describing its current condition.

Examples include:

| Statistic              | Description                         |
| ---------------------- | ----------------------------------- |
| Population             | Total population.                   |
| GDP                    | Gross Domestic Product.             |
| GDP Per Capita         | Economic output per citizen.        |
| Treasury               | Available government funds.         |
| Inflation              | Current inflation rate.             |
| Debt                   | Outstanding government debt.        |
| Credit Rating          | International financial confidence. |
| National Stability     | Overall domestic stability.         |
| International Prestige | Global influence and reputation.    |

These values evolve continuously throughout the simulation.

---

# 7.4 Strategic Resources

Strategic resources represent nationally controlled assets.

Examples include:

* oil,
* natural gas,
* coal,
* uranium,
* rare earth elements,
* steel,
* food production,
* freshwater,
* electricity generation.

Each resource records:

* production,
* consumption,
* imports,
* exports,
* reserve stockpile,
* projected shortages.

Resource availability directly influences national decision making.

---

# 7.5 National Objectives

Every nation maintains strategic objectives.

Objectives guide AI behaviour and advisor recommendations.

Possible objectives include:

* expand regional influence,
* maintain neutrality,
* dominate trade,
* modernise the military,
* increase economic growth,
* secure energy independence,
* develop nuclear weapons,
* preserve internal stability.

Objectives may change throughout the game in response to evolving circumstances.

---

# 7.6 Internal Politics

Domestic politics influence national decision making.

Internal political data includes:

* governing coalition,
* opposition strength,
* public approval,
* protest movements,
* extremist organisations,
* lobbying influence,
* media freedom,
* political legitimacy.

Internal politics may constrain government actions even when those actions are strategically desirable.

---

# 7.7 National Policies

Governments maintain long-term policy settings.

Examples include:

* taxation,
* immigration,
* defence spending,
* education,
* healthcare,
* environmental regulation,
* industrial policy,
* foreign aid.

Policy changes usually occur gradually and often have delayed effects.

---

# 7.8 National Modifiers

Temporary or permanent modifiers may affect a nation.

Examples include:

Positive:

* economic boom,
* technological breakthrough,
* political unity,
* military reform.

Negative:

* recession,
* civil unrest,
* corruption scandal,
* drought,
* sanctions,
* refugee crisis.

Modifiers alter simulation outcomes until they expire or are removed.

---

# 7.9 Relationships

Each nation maintains relationships with every other recognised nation.

Relationship data is stored separately within the Diplomacy State but is referenced by each Nation State.

Examples include:

* alliance membership,
* rivalry,
* trade partnerships,
* military access,
* intelligence sharing,
* sanctions,
* mutual defence treaties.

---

# 7.10 Implementation Notes

| Property             | Value                                                                              |
| -------------------- | ---------------------------------------------------------------------------------- |
| Owner System         | Nation Management System                                                           |
| Stored or Calculated | Primarily Stored                                                                   |
| Update Frequency     | Every Turn                                                                         |
| Dependencies         | Economy, Military, Diplomacy, Population, Intelligence                             |
| Used By              | AI, Advisors, Turn Engine, Event System                                            |
| Save File            | Yes                                                                                |
| Future Extensions    | Federal governments, autonomous regions, coalition politics, constitutional crises |

---

# Summary

The Nation State forms the primary representation of every playable and AI-controlled nation.

While the Global State describes the world as a whole, the Nation State defines the political, economic and strategic condition of each individual country.

Subsequent sections describe the systems owned by nations in greater detail, including provinces, military forces, economies and diplomacy.

# 8. Province State

## Purpose

The Province State represents the smallest permanent administrative area within the simulation.

Every nation is composed of one or more provinces. Provinces form the foundation of territorial control, economic production, military operations and population distribution.

Unlike nations, provinces can change ownership throughout the game due to war, diplomacy or political events.

---

# 8.1 Province Identity

Every province has a permanent identity.

| Field           | Type      | Description                                                    |
| --------------- | --------- | -------------------------------------------------------------- |
| Province ID     | UUID      | Permanent internal identifier.                                 |
| Province Name   | String    | Official province name.                                        |
| Parent Nation   | Nation ID | Nation that currently controls the province.                   |
| Original Nation | Nation ID | Nation that originally owned the province.                     |
| Region          | String    | Larger geographical region.                                    |
| Capital Status  | Boolean   | Indicates whether this province contains the national capital. |

Province IDs never change, even if ownership changes.

---

# 8.2 Geography

Each province contains geographical information used by multiple systems.

Examples include:

* Terrain type
* Elevation
* Coastline access
* River systems
* Climate zone
* Average seasonal weather
* Bordering provinces

Terrain influences:

* movement speed,
* logistics,
* combat,
* infrastructure development,
* economic production.

---

# 8.3 Population

Each province tracks its own population.

Population data includes:

| Statistic         | Description                  |
| ----------------- | ---------------------------- |
| Total Population  | Number of inhabitants.       |
| Population Growth | Annual growth rate.          |
| Urbanisation      | Percentage living in cities. |
| Workforce         | Available labour force.      |
| Education Level   | Overall education rating.    |
| Happiness         | Public satisfaction.         |

Population changes naturally over time through births, deaths and migration.

---

# 8.4 Infrastructure

Infrastructure determines how effectively a province supports economic and military activity.

Infrastructure includes:

* roads,
* railways,
* ports,
* airports,
* power generation,
* communications,
* hospitals,
* industrial facilities.

Higher infrastructure improves:

* movement,
* supply,
* production,
* trade,
* disaster recovery.

---

# 8.5 Natural Resources

Each province contains its own resource deposits.

Examples include:

* oil,
* natural gas,
* coal,
* iron,
* uranium,
* timber,
* fertile farmland,
* freshwater.

Resources may become more valuable as technology or global demand changes.

---

# 8.6 Strategic Importance

Certain provinces possess strategic value beyond their economic output.

Examples include:

* national capitals,
* major ports,
* mountain passes,
* canals,
* airbases,
* naval bases,
* border crossings,
* major industrial centres.

Strategic provinces often become key objectives during conflict.

---

# 8.7 Occupation

During war a province may be:

* peacefully controlled,
* occupied,
* contested,
* under siege,
* liberated.

Occupation status affects:

* taxation,
* resistance,
* supply,
* production,
* public order.

---

# 8.8 Province Modifiers

Temporary events may affect individual provinces.

Examples include:

Positive:

* infrastructure investment,
* tourism boom,
* mineral discovery,
* technological hub.

Negative:

* flooding,
* drought,
* civil unrest,
* epidemic,
* earthquake,
* industrial accident.

These modifiers influence local performance until they expire.

---

# 8.9 Implementation Notes

| Property             | Value                                                       |
| -------------------- | ----------------------------------------------------------- |
| Owner System         | Province Management System                                  |
| Stored or Calculated | Stored                                                      |
| Update Frequency     | Every Turn                                                  |
| Dependencies         | Economy, Military, Population, Weather                      |
| Used By              | AI, Turn Engine, Logistics, Combat                          |
| Save File            | Yes                                                         |
| Future Extensions    | Local governments, cultural regions, autonomous territories |

---

# Summary

The Province State represents the physical world in which the simulation takes place.

While nations define political control, provinces define the geographical, economic and strategic landscape that nations compete over.
# 9. Military State

## Purpose

The Military State represents the armed forces of every nation within the simulation.

It records the organisation, readiness, equipment, logistics and operational status of military forces.

The Military State provides the information required by the Turn Engine, AI Decision System and future Combat System to simulate military operations.

Combat rules themselves are defined separately within the Military System specification.

---

# 9.1 Military Organisation

Every nation maintains one Military State.

The Military State consists of multiple branches.

Typical branches include:

* Army
* Navy
* Air Force
* Strategic Forces
* National Guard
* Reserve Forces

Each branch maintains its own structure, equipment and readiness.

---

# 9.2 Military Units

The simulation represents military forces using individual military units.

Each unit records:

| Field              | Description                                         |
| ------------------ | --------------------------------------------------- |
| Unit ID            | Permanent unique identifier.                        |
| Unit Name          | Official designation.                               |
| Unit Type          | Infantry, Armour, Fighter Squadron, Destroyer, etc. |
| Parent Nation      | Owning nation.                                      |
| Current Province   | Current location.                                   |
| Personnel Strength | Number of personnel.                                |
| Equipment Strength | Operational equipment percentage.                   |
| Morale             | Current morale level.                               |
| Readiness          | Operational readiness.                              |
| Experience         | Veteran status and combat experience.               |

Each unit exists independently and may move throughout the world.

---

# 9.3 Equipment

Military equipment is tracked separately from personnel.

Examples include:

* Tanks
* Armoured vehicles
* Artillery
* Fighter aircraft
* Bombers
* Helicopters
* Surface ships
* Submarines
* Missile systems
* Air defence systems

Equipment may be:

* operational,
* under repair,
* in storage,
* destroyed,
* under construction.

---

# 9.4 Logistics

Military operations depend upon logistical support.

Each force records:

* fuel availability,
* ammunition stockpiles,
* food supplies,
* spare parts,
* transport capacity,
* medical support.

Logistics influence:

* movement,
* combat effectiveness,
* reinforcement,
* sustained operations.

---

# 9.5 Readiness Levels

Military forces maintain readiness levels.

Examples include:

* Peacetime
* Heightened Alert
* Partial Mobilisation
* Full Mobilisation
* Wartime Operations

Increasing readiness improves responsiveness but increases economic costs.

---

# 9.6 Command Structure

Each military branch maintains a chain of command.

Examples include:

* Supreme Commander
* Theatre Command
* Corps
* Division
* Brigade
* Battalion

The exact hierarchy may vary between nations and scenarios.

---

# 9.7 Strategic Assets

Some nations possess strategic military capabilities.

Examples include:

* Nuclear weapons
* Ballistic missiles
* Strategic bombers
* Aircraft carriers
* Missile defence systems
* Space surveillance assets

Strategic assets have significant geopolitical impact even when not actively used.

---

# 9.8 Military Modifiers

Temporary modifiers may affect military performance.

Examples include:

Positive:

* Elite Training
* Modern Equipment
* High Morale
* Experienced Leadership

Negative:

* Fuel Shortage
* Desertion
* Equipment Failure
* Poor Logistics
* Corruption
* Low Morale

These modifiers influence military effectiveness until removed.

---

# 9.9 Implementation Notes

| Property             | Value                                                                    |
| -------------------- | ------------------------------------------------------------------------ |
| Owner System         | Military Management System                                               |
| Stored or Calculated | Primarily Stored                                                         |
| Update Frequency     | Every Turn                                                               |
| Dependencies         | Nation State, Province State, Economy State                              |
| Used By              | Combat System, AI Decision System, Advisor System                        |
| Save File            | Yes                                                                      |
| Future Extensions    | Special Forces, Cyber Commands, Space Forces, Private Military Companies |

---

# Summary

The Military State defines the structure and condition of every nation's armed forces.

It provides the data required for military planning, logistics and conflict simulation while remaining independent of the combat resolution rules defined elsewhere.
# 10. Economy State

## Purpose

The Economy State represents the complete economic condition of every nation within the simulation.

It stores the financial, industrial, commercial and resource information required to model economic growth, government spending, international trade and national prosperity.

The Economy State provides the information consumed by the Economy System, AI Decision System and Advisor System.

---

# 10.1 National Economy

Each nation maintains an independent economy.

The Economy State stores the following high-level indicators.

| Field                        | Description                           |
| ---------------------------- | ------------------------------------- |
| Gross Domestic Product (GDP) | Total annual economic output.         |
| GDP Per Capita               | Economic output per citizen.          |
| Treasury                     | Government funds currently available. |
| Government Debt              | Outstanding public debt.              |
| Inflation Rate               | Current annual inflation.             |
| Unemployment Rate            | Percentage of unemployed workforce.   |
| Economic Growth              | Annual GDP growth rate.               |
| Credit Rating                | International financial confidence.   |

These indicators change over time as a result of economic activity and government policy.

---

# 10.2 Government Budget

Governments maintain annual budgets.

Budget categories include:

* Defence
* Healthcare
* Education
* Infrastructure
* Research
* Welfare
* Foreign Aid
* Debt Repayment
* Emergency Funding

Budget allocations directly influence other systems within the simulation.

---

# 10.3 Taxation

Taxation represents the primary source of government revenue.

The Economy State records:

* corporate tax,
* income tax,
* resource royalties,
* import duties,
* export duties,
* value-added or sales taxes.

Changes in taxation affect both government revenue and economic activity.

---

# 10.4 Industry

Economic production is divided into industrial sectors.

Examples include:

* Agriculture
* Mining
* Manufacturing
* Construction
* Energy
* Technology
* Finance
* Tourism
* Services

Each sector may grow or decline independently depending on national and global conditions.

---

# 10.5 Trade

International trade forms an important component of national economies.

Trade data includes:

| Field                  | Description                             |
| ---------------------- | --------------------------------------- |
| Imports                | Goods purchased from other nations.     |
| Exports                | Goods sold to other nations.            |
| Trade Balance          | Difference between imports and exports. |
| Major Trading Partners | Principal trade relationships.          |
| Trade Agreements       | Active economic treaties.               |

Trade is influenced by diplomacy, transportation infrastructure and global events.

---

# 10.6 Strategic Stockpiles

Governments maintain reserves of critical resources.

Examples include:

* fuel,
* food,
* medical supplies,
* ammunition,
* industrial materials,
* rare earth elements.

Stockpiles provide resilience during war, disasters and supply disruptions.

---

# 10.7 Economic Modifiers

Temporary or permanent conditions may affect economic performance.

Positive examples:

* Investment Boom
* Resource Discovery
* Trade Expansion
* Technological Innovation

Negative examples:

* Recession
* Hyperinflation
* International Sanctions
* Financial Crisis
* Supply Chain Disruption
* Labour Shortages

Modifiers influence economic indicators until removed.

---

# 10.8 Financial Stability

Each nation tracks indicators of long-term financial health.

These include:

* debt sustainability,
* investor confidence,
* currency stability,
* foreign reserves,
* banking sector stability.

Financial instability may increase borrowing costs and reduce economic growth.

---

# 10.9 Implementation Notes

| Property             | Value                                                                                           |
| -------------------- | ----------------------------------------------------------------------------------------------- |
| Owner System         | Economy System                                                                                  |
| Stored or Calculated | Combination of Stored and Calculated                                                            |
| Update Frequency     | Every Turn                                                                                      |
| Dependencies         | Nation State, Province State, Global State                                                      |
| Used By              | AI Decision System, Advisor System, Military System, Diplomacy System                           |
| Save File            | Yes                                                                                             |
| Future Extensions    | Currency markets, central banking, sovereign wealth funds, international financial institutions |

---

# Summary

The Economy State represents the financial foundation of each nation.

It records the information required to model taxation, public spending, industrial production and international trade while remaining independent of the economic rules that update these values.
# 11. Diplomacy State

## Purpose

The Diplomacy State records all political, diplomatic and legal relationships between nations.

It defines the formal and informal interactions that exist between sovereign states, including alliances, treaties, rivalries, trade agreements and international organisations.

The Diplomacy State does not determine how diplomatic decisions are made. Instead, it stores the current diplomatic situation for use by the Diplomacy System, AI Decision System and Advisor System.

---

# 11.1 Bilateral Relationships

Every recognised nation maintains an independent relationship with every other recognised nation.

Relationships are directional and may differ between nations.

For example:

Nation A may trust Nation B while Nation B distrusts Nation A.

Each bilateral relationship records:

| Field              | Description                                      |
| ------------------ | ------------------------------------------------ |
| Trust              | Confidence in the other nation.                  |
| Opinion            | Overall diplomatic attitude.                     |
| Hostility          | Degree of antagonism.                            |
| Influence          | Political influence held over the other nation.  |
| Recognition Status | Whether the government is officially recognised. |

These values evolve over time as nations interact.

---

# 11.2 Diplomatic Status

Each relationship maintains a formal diplomatic status.

Possible statuses include:

* No Contact
* Diplomatic Contact
* Friendly Relations
* Strategic Partnership
* Allied
* Neutral
* Rival
* Hostile
* At War

Diplomatic status influences the actions available to governments.

---

# 11.3 Treaties

Treaties represent formal agreements between nations.

Examples include:

* Non-Aggression Pacts
* Mutual Defence Treaties
* Trade Agreements
* Intelligence Sharing Agreements
* Military Access Agreements
* Arms Reduction Treaties
* Peace Treaties

Each treaty records:

* participating nations,
* signing date,
* duration,
* obligations,
* termination conditions.

---

# 11.4 International Organisations

Nations may belong to international organisations.

Examples include:

* United Nations
* NATO
* European Union
* African Union
* ASEAN
* Fictional organisations created by scenarios.

Membership may provide diplomatic, economic or military benefits and obligations.

---

# 11.5 Sanctions

Nations may impose sanctions on one another.

Examples include:

* Trade Sanctions
* Financial Sanctions
* Travel Restrictions
* Arms Embargoes
* Technology Restrictions

Sanctions remain active until removed or replaced.

---

# 11.6 Diplomatic Actions

The Diplomacy State stores ongoing diplomatic activity.

Examples include:

* Active Negotiations
* Peace Talks
* Alliance Proposals
* Trade Negotiations
* Territorial Disputes
* Mediation Efforts
* Humanitarian Agreements

Each action records:

* participating nations,
* current status,
* expected completion,
* negotiation progress.

---

# 11.7 Casus Belli

A Casus Belli represents a recognised justification for military action.

Examples include:

* Border Incursion
* Territorial Claim
* Defence of Ally
* Humanitarian Intervention
* Self Defence
* Protection of Citizens Abroad

Casus Belli influence diplomatic opinion, AI behaviour and international legitimacy.

---

# 11.8 Reputation

Every nation possesses an international reputation.

Examples include:

* Reliable Ally
* Aggressive Expansionist
* Neutral Mediator
* Humanitarian Leader
* Economic Power
* Rogue State

Reputation develops over time based on national behaviour.

---

# 11.9 Implementation Notes

| Property             | Value                                                                                                     |
| -------------------- | --------------------------------------------------------------------------------------------------------- |
| Owner System         | Diplomacy System                                                                                          |
| Stored or Calculated | Combination of Stored and Calculated                                                                      |
| Update Frequency     | Every Turn                                                                                                |
| Dependencies         | Nation State, Global State                                                                                |
| Used By              | AI Decision System, Advisor System, Military System, Economy System                                       |
| Save File            | Yes                                                                                                       |
| Future Extensions    | International courts, diplomatic incidents, regional organisations, multinational peacekeeping operations |

---

# Summary

The Diplomacy State defines the political relationships that exist between nations.

It provides the information required to simulate alliances, negotiations, rivalries and international cooperation while remaining independent of the diplomatic rules that govern how these relationships change over time.
# 12. Intelligence State

## Purpose

The Intelligence State represents the information gathering capabilities of every nation.

Unlike other systems, intelligence is not only concerned with what exists in the world, but also with what each nation knows—or believes—it knows.

The Intelligence State forms the foundation of the engine's Fog of War system by ensuring that nations make decisions based only on information that is available to them.

---

# 12.1 Knowledge Model

Every nation possesses its own independent view of the world.

A nation's knowledge may differ significantly from reality.

For example:

* A nation may overestimate an enemy's military strength.
* A nation may be unaware of a secret alliance.
* A nation may incorrectly believe another nation is preparing for war.

The actual Game State remains unchanged.

The Intelligence State records what each nation believes to be true.

---

# 12.2 Intelligence Agencies

Each nation maintains one or more intelligence organisations.

Typical agencies include:

* Domestic Intelligence
* Foreign Intelligence
* Military Intelligence
* Signals Intelligence
* Cyber Intelligence

Each agency possesses its own capabilities and areas of responsibility.

---

# 12.3 Intelligence Assets

Nations may deploy intelligence assets throughout the world.

Examples include:

* Human intelligence (HUMINT)
* Signals intelligence (SIGINT)
* Satellite reconnaissance
* Cyber surveillance
* Reconnaissance aircraft
* Naval surveillance
* Diplomatic reporting
* Open-source intelligence (OSINT)

Assets gather information over time.

---

# 12.4 Information Quality

Not all intelligence is equally reliable.

Each intelligence report records:

| Field          | Description                        |
| -------------- | ---------------------------------- |
| Source         | Origin of the information.         |
| Confidence     | Estimated reliability.             |
| Date Collected | When the information was obtained. |
| Last Verified  | Most recent verification.          |
| Classification | Security classification level.     |

Older information gradually becomes less reliable.

---

# 12.5 Fog of War

The simulation shall not automatically reveal complete world information to every nation.

Unknown information may include:

* military deployments,
* research projects,
* covert operations,
* diplomatic negotiations,
* economic difficulties,
* mobilisation efforts.

Only information discovered through intelligence gathering becomes available.

---

# 12.6 Counterintelligence

Nations actively protect their own information.

Counterintelligence activities include:

* detecting spies,
* preventing espionage,
* securing communications,
* identifying cyber intrusions,
* protecting classified information.

Effective counterintelligence reduces the effectiveness of foreign intelligence operations.

---

# 12.7 Active Intelligence Operations

The Intelligence State stores ongoing operations.

Examples include:

* Spy recruitment
* Surveillance
* Cyber espionage
* Sabotage
* Influence campaigns
* Reconnaissance missions
* Intelligence sharing

Each operation records:

* participating nations,
* objective,
* progress,
* detection risk,
* current status.

---

# 12.8 Intelligence Modifiers

Temporary conditions may affect intelligence effectiveness.

Positive examples:

* Code Breaking
* Advanced Satellites
* Insider Source
* Improved Encryption

Negative examples:

* Double Agent
* Communications Failure
* Intelligence Leak
* Cyber Breach

Modifiers remain active until removed.

---

# 12.9 Implementation Notes

| Property             | Value                                                                                              |
| -------------------- | -------------------------------------------------------------------------------------------------- |
| Owner System         | Intelligence System                                                                                |
| Stored or Calculated | Combination of Stored and Calculated                                                               |
| Update Frequency     | Every Turn                                                                                         |
| Dependencies         | Nation State, Diplomacy State, Military State                                                      |
| Used By              | AI Decision System, Advisor System, Turn Engine                                                    |
| Save File            | Yes                                                                                                |
| Future Extensions    | Machine learning analysis, autonomous reconnaissance, misinformation campaigns, quantum encryption |

---

# Summary

The Intelligence State records what every nation knows about the world rather than the complete reality of the simulation.

This distinction enables realistic Fog of War, imperfect decision-making and intelligence-driven gameplay while preserving a consistent underlying Game State.
# 13. Technology State

## Purpose

The Technology State records the scientific, industrial and technological capabilities of every nation.

It represents the current level of technological development and tracks research progress across multiple disciplines.

The Technology State provides the information required by the Technology System, AI Decision System and Advisor System to model technological advancement.

---

# 13.1 Technology Overview

Each nation maintains its own independent technology profile.

Technological progress reflects long-term investment in research, education and innovation.

Technology influences many other systems including:

* military capability,
* economic productivity,
* infrastructure,
* healthcare,
* intelligence,
* energy production,
* industrial efficiency.

---

# 13.2 Research Categories

Research is organised into multiple categories.

Typical categories include:

| Category               | Examples                                          |
| ---------------------- | ------------------------------------------------- |
| Military               | Weapons, armour, logistics                        |
| Aerospace              | Aircraft, satellites, space technology            |
| Naval                  | Ships, submarines, maritime systems               |
| Energy                 | Nuclear, renewable, fossil fuel efficiency        |
| Medicine               | Vaccines, trauma care, disease prevention         |
| Agriculture            | Crop yields, irrigation, food production          |
| Industry               | Manufacturing, automation, robotics               |
| Information Technology | Computing, artificial intelligence, cybersecurity |
| Infrastructure         | Construction, transportation, communications      |

Additional categories may be introduced by future scenarios.

---

# 13.3 Research Projects

A nation may conduct multiple research projects simultaneously.

Each project records:

| Field                | Description                       |
| -------------------- | --------------------------------- |
| Project ID           | Permanent identifier.             |
| Research Category    | Associated technology field.      |
| Project Name         | Research project title.           |
| Progress             | Current completion percentage.    |
| Estimated Completion | Expected completion date or turn. |
| Priority             | Government-assigned importance.   |

Projects may be accelerated or delayed depending on available resources.

---

# 13.4 Research Capacity

Each nation possesses a finite research capability.

Factors affecting research capacity include:

* education level,
* research funding,
* number of scientists,
* university quality,
* industrial capability,
* international cooperation.

Higher research capacity enables faster technological development.

---

# 13.5 Technology Levels

Each research category maintains a technology level.

Technology levels represent the nation's overall capability rather than individual inventions.

Higher technology levels unlock future capabilities and improvements throughout the simulation.

---

# 13.6 Technology Sharing

Technology may spread between nations.

Examples include:

* research partnerships,
* licensed production,
* academic cooperation,
* industrial investment,
* espionage,
* captured equipment.

Technology transfer may be voluntary or involuntary.

---

# 13.7 Scientific Modifiers

Temporary conditions may influence research performance.

Positive examples:

* Increased Research Funding
* International Collaboration
* Scientific Breakthrough
* University Expansion

Negative examples:

* Brain Drain
* Research Sanctions
* Laboratory Accident
* Budget Cuts

Modifiers remain active until removed.

---

# 13.8 Implementation Notes

| Property             | Value                                                                                  |
| -------------------- | -------------------------------------------------------------------------------------- |
| Owner System         | Technology System                                                                      |
| Stored or Calculated | Combination of Stored and Calculated                                                   |
| Update Frequency     | Every Turn                                                                             |
| Dependencies         | Economy State, Nation State                                                            |
| Used By              | Military System, Economy System, Intelligence System, Advisor System                   |
| Save File            | Yes                                                                                    |
| Future Extensions    | Private research companies, patent systems, emerging technologies, civilian innovation |

---

# Summary

The Technology State records the technological capabilities and ongoing research activities of every nation.

It provides the information necessary to model long-term national development while remaining independent of the rules governing research progression.
# 14. Population & Stability State

## Purpose

The Population & Stability State represents the people living within each nation and their overall social, political and economic wellbeing.

It records demographic information, public opinion and internal stability that influence every major system within the simulation.

A nation's long-term success depends not only on military and economic strength, but also on maintaining a stable, productive and satisfied population.

---

# 14.1 National Population

Each nation maintains a complete demographic profile.

Core population statistics include:

| Field             | Description                             |
| ----------------- | --------------------------------------- |
| Total Population  | Total number of citizens and residents. |
| Population Growth | Annual population growth rate.          |
| Birth Rate        | Annual births.                          |
| Death Rate        | Annual deaths.                          |
| Median Age        | Average age of the population.          |
| Urban Population  | Percentage living in urban areas.       |
| Rural Population  | Percentage living in rural areas.       |
| Workforce Size    | Economically active population.         |

Population changes naturally over time through births, deaths and migration.

---

# 14.2 Demographics

The simulation stores demographic characteristics that influence national development.

Examples include:

* education level,
* literacy,
* workforce participation,
* age distribution,
* migration trends,
* population density.

These characteristics affect economic productivity, military recruitment and technological development.

---

# 14.3 Public Opinion

Every nation records the current attitude of its population.

Examples include:

* Government Approval
* War Support
* Economic Confidence
* Trust in Institutions
* National Unity
* Political Polarisation

Public opinion changes gradually in response to government actions and world events.

---

# 14.4 National Stability

National Stability represents the overall resilience of the state.

Factors influencing stability include:

* economic performance,
* political legitimacy,
* military success,
* corruption,
* unemployment,
* civil unrest,
* natural disasters,
* foreign interference.

Low stability increases the likelihood of domestic crises.

---

# 14.5 Civil Unrest

The engine records internal disturbances affecting national security.

Examples include:

* protests,
* strikes,
* riots,
* insurgencies,
* separatist movements,
* terrorism,
* organised crime.

Each event records:

* affected provinces,
* severity,
* duration,
* participating groups,
* government response.

---

# 14.6 Migration

Population movement is tracked throughout the simulation.

Migration includes:

* internal migration,
* immigration,
* emigration,
* refugee movements,
* displacement caused by conflict or disasters.

Migration influences labour availability, housing demand and population growth.

---

# 14.7 National Modifiers

Temporary events may influence population wellbeing.

Positive examples:

* Economic Prosperity
* National Celebration
* Successful Reforms
* Healthcare Improvements

Negative examples:

* Famine
* Pandemic
* Housing Crisis
* High Inflation
* Political Scandal
* Energy Shortages

Modifiers remain active until removed.

---

# 14.8 Implementation Notes

| Property             | Value                                                                           |
| -------------------- | ------------------------------------------------------------------------------- |
| Owner System         | Population System                                                               |
| Stored or Calculated | Combination of Stored and Calculated                                            |
| Update Frequency     | Every Turn                                                                      |
| Dependencies         | Economy State, Nation State, Province State                                     |
| Used By              | AI Decision System, Economy System, Military System, Advisor System             |
| Save File            | Yes                                                                             |
| Future Extensions    | Ethnic demographics, religion, culture, language groups, generational modelling |

---

# Summary

The Population & Stability State records the demographic and social condition of every nation.

It provides the information required to model domestic politics, workforce development, social cohesion and national resilience while remaining independent of the systems that calculate these changes.
w# 15. Event State

## Purpose

The Event State records all significant events that are currently active within the simulation.

Events represent temporary or ongoing situations that influence one or more systems within the engine. They provide a structured way to model dynamic changes without permanently altering the underlying Game State.

The Event State allows the simulation to track the progression, impact and resolution of world events over multiple turns.

---

# 15.1 Event Overview

Every active event is stored as an independent object.

Each event records:

| Field             | Description                              |
| ----------------- | ---------------------------------------- |
| Event ID          | Permanent unique identifier.             |
| Event Name        | Human-readable name.                     |
| Event Type        | Category of event.                       |
| Start Turn        | Turn on which the event began.           |
| Expected Duration | Estimated number of turns.               |
| Current Status    | Active, Escalating, Resolved, Cancelled. |
| Severity          | Overall impact level.                    |

Events remain active until resolved or removed.

---

# 15.2 Event Categories

The engine supports multiple categories of events.

Examples include:

* Political
* Military
* Economic
* Diplomatic
* Intelligence
* Environmental
* Natural Disaster
* Public Health
* Technological
* Social

Future scenarios may introduce additional event categories.

---

# 15.3 Geographic Scope

Events may affect different geographical areas.

Possible scopes include:

* Single Province
* Multiple Provinces
* Entire Nation
* Multiple Nations
* Regional
* Global

The affected scope determines which systems receive updates each turn.

---

# 15.4 Event Effects

Each event may influence one or more systems.

Examples include:

* Economy
* Military
* Population
* Diplomacy
* Intelligence
* Technology
* Infrastructure
* Logistics

Event effects may be immediate, gradual or conditional.

---

# 15.5 Escalation

Events may evolve over time.

Possible outcomes include:

* Improvement
* Stabilisation
* Escalation
* Expansion
* Resolution

For example:

A border dispute may escalate into military mobilisation.

A recession may develop into a financial crisis.

A disease outbreak may become a pandemic.

---

# 15.6 Trigger Conditions

Events may begin automatically when predefined conditions are satisfied.

Examples include:

* Stability falls below a defined threshold.
* Military forces cross an international border.
* Food reserves become critically low.
* An election concludes without a recognised winner.
* A peace treaty expires.

Trigger conditions are evaluated by the Turn Engine each turn.

---

# 15.7 Resolution Conditions

Every event defines one or more conditions for completion.

Examples include:

* Peace agreement signed.
* Disaster recovery completed.
* Public order restored.
* Economic indicators recover.
* Research project completed.

Once all resolution conditions are met, the event is marked as resolved and no longer produces ongoing effects.

---

# 15.8 Event History

Resolved events are retained in the historical record.

Historical information includes:

* start date,
* end date,
* participating nations,
* final outcome,
* significant consequences.

Historical events may influence future gameplay through reputation, diplomatic memory or scenario objectives.

---

# 15.9 Implementation Notes

| Property             | Value                                                                              |
| -------------------- | ---------------------------------------------------------------------------------- |
| Owner System         | Event System                                                                       |
| Stored or Calculated | Stored                                                                             |
| Update Frequency     | Every Turn                                                                         |
| Dependencies         | All major simulation systems                                                       |
| Used By              | Turn Engine, AI Decision System, Advisor System                                    |
| Save File            | Yes                                                                                |
| Future Extensions    | Dynamic event chains, scripted scenarios, player-created events, historical replay |

---

# Summary

The Event State provides a consistent framework for representing temporary and ongoing situations within the simulation.

By storing events independently from other systems, the engine can model complex world developments while keeping the Game State organised and extensible.
# 16. Persistence & Save Format

## Purpose

The Persistence & Save Format defines how the complete Game State is stored, restored and validated.

Its purpose is to ensure that a simulation can be paused and resumed without loss of information while maintaining compatibility across future versions of the engine.

The persistence layer stores the entire Game State exactly as it exists at the end of each completed turn.

---

# 16.1 Save Philosophy

Every save file represents a complete snapshot of the simulation.

Loading a save restores the world to the exact state it occupied when the save was created.

No game information should need to be reconstructed after loading.

The save file is therefore considered the authoritative record of the simulation at a particular point in time.

---

# 16.2 Saved Components

The persistence system stores every major component of the Game State.

This includes:

* Global State
* Nation State
* Province State
* Military State
* Economy State
* Diplomacy State
* Intelligence State
* Technology State
* Population & Stability State
* Event State

No persistent simulation data should exist outside these components.

---

# 16.3 Save Metadata

Every save records metadata describing the save itself.

| Field            | Description                              |
| ---------------- | ---------------------------------------- |
| Save ID          | Permanent unique identifier.             |
| Save Name        | User-defined save name.                  |
| Scenario         | Scenario associated with the save.       |
| Turn Number      | Current turn when saved.                 |
| In-Game Date     | Current simulation date.                 |
| Engine Version   | Engine version used to create the save.  |
| Document Version | Version of the Game State specification. |
| Creation Date    | Date the save was first created.         |
| Last Modified    | Most recent save operation.              |

Metadata assists with compatibility and version management.

---

# 16.4 Save Validation

Before loading a save, the engine performs validation checks.

Typical validation includes:

* required data present,
* valid identifiers,
* recognised engine version,
* recognised scenario,
* complete Game State,
* no corrupted records.

If validation fails, the engine should report the error rather than attempting to continue with invalid data.

---

# 16.5 Version Compatibility

As the engine evolves, save files may originate from different versions.

The persistence system should support:

* version identification,
* migration where practical,
* compatibility checking,
* informative error reporting.

Where automatic migration is not possible, the engine should clearly explain why a save cannot be loaded.

---

# 16.6 Autosave

The engine may create automatic saves.

Typical autosave triggers include:

* end of each turn,
* major diplomatic agreements,
* declaration of war,
* scenario milestones,
* manual checkpoints.

Autosaves reduce the risk of losing simulation progress.

---

# 16.7 Export and Import

Future versions of the engine may support exporting and importing Game States.

Possible applications include:

* scenario creation,
* debugging,
* multiplayer synchronisation,
* sharing simulations,
* historical replay.

The export format should remain independent of the user interface.

---

# 16.8 Data Integrity

The persistence system should preserve the consistency of the Game State.

Examples include:

* every Nation ID remains unique,
* every Province references a valid Nation,
* every treaty references existing nations,
* every military unit belongs to an existing nation,
* every event references valid entities.

Invalid references should be detected during validation.

---

# 16.9 Implementation Notes

| Property             | Value                                                                 |
| -------------------- | --------------------------------------------------------------------- |
| Owner System         | Save System                                                           |
| Stored or Calculated | Stored                                                                |
| Update Frequency     | Manual or Automatic Save Events                                       |
| Dependencies         | Entire Game State                                                     |
| Used By              | Scenario Loader, Turn Engine                                          |
| Save File            | Not Applicable (defines the save itself)                              |
| Future Extensions    | Cloud saves, encrypted saves, collaborative simulations, replay files |

---

# Summary

The Persistence & Save Format ensures that the complete simulation can be stored and restored reliably.

By treating the Game State as a complete snapshot of the world, the engine maintains consistency across play sessions while supporting future expansion and compatibility.
O a# 17. Document Summary

## Overview

This document defines the complete Game State of the Interactive Geopolitical Engine.

The Game State represents the authoritative state of the simulation at any point in time. It records the condition of the world, every nation, every province, every military unit, every diplomatic relationship and every active event.

The Game State serves as the foundation upon which every other subsystem operates.

---

## Architectural Role

The Game State answers one fundamental question:

> **"What does the engine currently know about the world?"**

It does **not** answer:

* How combat is resolved.
* How economies grow.
* How diplomacy changes.
* How AI makes decisions.
* How events progress.

Those behaviours are defined within their respective system specifications.

The Game State stores information.

Other systems modify that information.

Maintaining this separation ensures that the engine remains modular, testable and extensible.

---

## Major Components

The complete Game State consists of the following major components:

* Global State
* Nation State
* Province State
* Military State
* Economy State
* Diplomacy State
* Intelligence State
* Technology State
* Population & Stability State
* Event State
* Persistence & Save Format

Together, these components define the complete state of the simulated world.

---

## Relationship to Other Documents

The Game State is closely related to the following specifications:

* **000 – Vision** – Defines the overall goals and design philosophy.
* **010 – Engine Architecture** – Defines the major engine components.
* **020 – Entity Model** – Defines the entities represented within the Game State.
* **040 – Turn Processing** – Defines how the Turn Engine updates the Game State during each simulation turn.

Future system specifications will reference the Game State rather than redefining its data structures.

---

## Design Principles

The Game State has been designed according to the following principles:

* Single source of truth.
* Clear ownership of data.
* Separation of state and behaviour.
* Modular architecture.
* Extensibility.
* Deterministic simulation.
* Persistent world state.

These principles should continue to guide future development.

---

## Future Development

As the Interactive Geopolitical Engine evolves, additional systems may extend the Game State.

Examples include:

* Climate simulation.
* Space programmes.
* International law.
* Cyber warfare.
* Cultural influence.
* Media systems.
* Private corporations.
* Non-state actors.

Where possible, new systems should extend the existing architecture rather than replacing it.

---

## Transition to the Next Specification

With the Game State now defined, the next stage of development is to specify how the simulation advances.

The next document, **040 – Turn Processing**, will define the complete lifecycle of a simulation turn.

It will describe:

* player order submission,
* order validation,
* AI planning,
* diplomacy,
* intelligence,
* logistics,
* combat,
* economy,
* technology,
* event resolution,
* advisor briefings,
* end-of-turn processing.

Together, the Game State and Turn Processing specifications form the foundation of the Interactive Geopolitical Engine.

---

# End of Document
