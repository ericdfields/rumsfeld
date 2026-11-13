# Lens: Contrarian

**Core question:** what if the fundamental abstraction this spec is built on is wrong?

This lens carries the strongest mandate of the thirteen (see `SKILL.md`, Phase 3). Every other lens works *within* the spec's framing — it asks whether the details are right. This lens questions the framing itself. It targets category errors, not implementation gaps, and a single good finding here can be worth more than everything the other twelve lenses produce combined.

## The move

Take the spec's central noun or central verb — the thing it says this *is* — and ask what it would look like if that were actually a different, structurally distinct thing. Some stock reframes, adapt freely:

```
What if this isn't a routing problem but an ownership-transfer problem?
What if this isn't a document but a graph?
What if this isn't a user preference but persistent state?
What if this isn't a background task but a durable workflow?
What if this shouldn't be synchronous at all?
What if the thing we're calling one object is actually several objects?
What if the thing we're calling several objects is actually one?
What if this is a permissions problem wearing a UI-feature costume?
What if the "temporary" thing in this spec actually needs to be permanent?
What if the direction of ownership/control described here is backwards?
```

## How to use this lens

Don't just apply the stock list mechanically — the point is genuine reframing, not a fill-in-the-blank exercise. Read the spec's opening description of what it's building, and ask: if I described this system's actual behavior to someone with no context, using different words than the spec uses, what would they call it? If that name is meaningfully different from the spec's own framing, follow that thread — it usually leads somewhere the other lenses can't reach, because they all inherited the spec's own vocabulary.

A useful test: does the reframe explain something the spec currently treats as a special case or an afterthought, by making it central instead? If "model switching" is really "ownership transfer," then all the handoff, interruption, and duplicate-execution questions stop being edge cases and become the main design problem — that's the signature of a genuine reframe, not a speculative one.

## Example

Bad: "Have we considered other architectures?"

Good: "The PRD frames this as 'let an agent workstream move from one model/provider to another,' which reads as a routing decision. But everything the PRD's own goals require — resuming after interruption, not losing work, not duplicating side effects — is actually about who owns a long-running unit of work and how that ownership can be safely transferred. This isn't primarily a routing problem; it's a durable-ownership-transfer problem that happens to be triggered by a provider switch."
