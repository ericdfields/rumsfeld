# Lens: Reversibility

**Core question:** which choices in this spec are cheap to make today but expensive to reverse once implementation has happened?

This lens doesn't hunt for new problems so much as it re-weights the ones other lenses already found. A finding's reversibility should directly affect how much scrutiny it gets before implementation — see the finding schema in `SKILL.md`, which treats "high impact + difficult to reverse" as worth acting on even at only `plausible` confidence.

## Look for

- **Data model decisions.** Schema choices, especially the shape of primary entities and their relationships, are usually the most expensive things to change after real data exists in them. Which schema decisions in this spec are load-bearing for everything built on top?
- **API and interface shape.** Once other systems or external users depend on an interface, changing it has a blast radius beyond this project. Which interfaces does this spec expose externally versus keep purely internal?
- **Naming and taxonomy.** Sounds trivial, but a wrong name for a core concept (calling something a "task" when it's really a "workflow," for instance) tends to get baked into code, docs, APIs, and user mental models simultaneously, and untangling it later touches all of them at once.
- **Default behaviors.** Defaults that are easy to pick arbitrarily now become expectations users build workflows around, making them costly to change later even if technically trivial to change in code.
- **Irreversible user-facing actions.** Anywhere the spec lets a user do something that can't be undone (delete, merge, send) — was that reversibility choice made deliberately, or is it just the easiest thing to build first?

## How to use this lens

For each major decision implicit in the spec, ask two questions: how expensive would it be to build this the way the spec describes, and separately, how expensive would it be to change it after three months of real usage? A decision with a big gap between those two numbers — cheap now, expensive later — deserves explicit attention in the report even if nothing about it looks risky today.

## Example

Bad: "This decision might be hard to change later."

Good: "The spec models 'workstream' and 'agent' as a 1:1 relationship, which is simple to build, but the moment handoff between agents is added (which the PRD's own goals imply), that becomes a 1:many relationship over the workstream's lifetime — better to model it that way from the start than to migrate every existing workstream's data later."
