# 01 - Engine Architecture

**Version:** 1.0

---

# Purpose

This document defines the overall architecture of the Interactive Geopolitical Engine.

It describes how the engine processes a turn, how information flows through the simulation, and how each subsystem interacts with the World State.

---

# Core Components

- Player
- World State
- Turn Engine
- AI Decision Engine
- Simulation Systems
- Advisor System
- Scenario Generator
- Save System

---

# High-Level Flow

Player Issues Orders

↓

Orders Validated

↓

AI Nations Generate Orders

↓

Turn Resolution

↓

World State Updated

↓

Advisor Briefings Generated

↓

Player Receives New Turn

---

# Subsystems

## World State

Stores all persistent information.

---

## Turn Engine

Processes one complete game turn.

---

## AI Decision Engine

Determines actions taken by AI nations.

---

## Simulation Systems

- Economy
- Military
- Diplomacy
- Intelligence
- Domestic Politics
- Technology
- Logistics

---

## Advisor System

Converts World State into player-facing briefings.

---

# Dependencies

(To be completed later.)

---

# Future Expansion

(To be completed later.)