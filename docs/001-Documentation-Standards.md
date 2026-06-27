# 001 – Documentation Standards

**Project:** Interactive Geopolitical Engine
**Document Version:** 1.0
**Status:** Active Standard

---

# 1. Purpose

This document defines the documentation standards used throughout the Interactive Geopolitical Engine project.

Its purpose is to ensure that every design document follows a consistent structure, uses common terminology, and provides sufficient technical detail to support future implementation.

These standards apply to all current and future documentation within the `docs/` directory.

---

# 2. Documentation Philosophy

Project documentation is intended to serve as the primary source of architectural knowledge.

Each document should describe:

* what a system is,
* why it exists,
* how it interacts with other systems,
* what responsibilities it has,
* what information it owns,
* how it may evolve in future versions.

Documentation should focus on design decisions rather than implementation details.

---

# 3. Naming Convention

All documents use the following filename format:

```text
NNN-Document-Name.md
```

Examples:

```text
000-Vision.md
001-Documentation-Standards.md
010-Engine-Architecture.md
020-Entity-Model.md
030-Game-State.md
040-Turn-Processing.md
```

Document numbers should remain stable once assigned.

---

# 4. Required Document Header

Every design document begins with a standard header.

Example:

```markdown
# 030 – Game State

**Project:** Interactive Geopolitical Engine
**Document Version:** 1.0
**Status:** Draft

**Depends On**

- 000 – Vision
- 010 – Engine Architecture
```

---

# 5. Standard Section Structure

Every major section should follow the same order where practical.

1. Purpose
2. Description
3. Design
4. Data or Rules
5. Examples (if helpful)
6. Implementation Notes
7. Summary

Not every section requires every element, but this structure should be used whenever it improves clarity.

---

# 6. Writing Style

Documentation should be:

* clear,
* concise,
* technically accurate,
* implementation independent,
* written in present tense,
* free from conversational language.

Avoid explaining how code will be written unless the document specifically concerns implementation.

---

# 7. Tables

Tables should be used whenever structured information is easier to understand than paragraphs.

Preferred examples include:

* data fields,
* responsibilities,
* dependencies,
* configuration values,
* system ownership.

---

# 8. Diagrams

Simple diagrams are encouraged where they improve understanding.

Use plain text diagrams where possible.

Example:

```text
Player Orders
        │
        ▼
Turn Engine
        │
        ▼
AI Systems
        │
        ▼
World State
```

Avoid tool-specific diagram formats unless the entire project adopts them consistently.

---

# 9. Terminology

The following terms are used consistently throughout the project.

| Term         | Definition                                                 |
| ------------ | ---------------------------------------------------------- |
| Game State   | Complete state of the simulation at a given point in time. |
| World Engine | Core system responsible for advancing the simulation.      |
| Turn Engine  | Processes one complete simulation turn.                    |
| Entity       | Any object represented within the simulation.              |
| System       | A functional component responsible for specific behaviour. |
| State        | Stored information owned by a system or entity.            |

Where possible, avoid introducing multiple terms for the same concept.

---

# 10. Implementation Notes

Every major subsystem should include an **Implementation Notes** section.

The purpose of these notes is to document architectural decisions without prescribing source code.

The preferred format is:

```markdown
### Implementation Notes

| Property | Value |
|----------|-------|
| Owner System | |
| Stored or Calculated | |
| Update Frequency | |
| Dependencies | |
| Used By | |
| Save File | |
| Future Extensions | |
```

These notes help bridge the gap between design documentation and future implementation.

---

# 11. Versioning

When a document undergoes significant architectural changes:

* increment the document version,
* update affected references,
* preserve numbering where possible.

Minor wording changes do not require a version increment.

---

# 12. Cross References

Where a topic is defined elsewhere, reference the appropriate design document rather than duplicating information.

Example:

> Combat resolution is defined in **060 – Military System**.

This keeps documentation modular and avoids inconsistencies.

---

# 13. Future-Proofing

Design documents should be written with future expansion in mind.

Whenever practical:

* separate data from behaviour,
* avoid unnecessary assumptions,
* leave room for additional mechanics,
* prefer modular systems over tightly coupled designs.

---

# 14. Repository Workflow

When adding or modifying documentation:

1. Create or update the document.
2. Review for consistency with this standard.
3. Save the document.
4. Commit the changes with a descriptive Git commit message.
5. Push the commit to the remote repository.

---

# Summary

These standards provide a consistent framework for all documentation within the Interactive Geopolitical Engine project.

Following these conventions will improve readability, reduce ambiguity, and make the transition from architectural design to implementation significantly easier.
