# Unknown-unknown generators

These are active probes, not passive checklists — each one asks you to transform the spec somehow (stop it, duplicate it, invert it, remove a dependency) and see what falls out. Run them after the lens reviews (Phase 4), using whatever findings the lenses already produced as raw material — a probe often turns a vague lens finding into a concrete one.

**Atomicity challenge.** What does this spec treat as a single, indivisible step that might actually contain multiple independently-changing parts? Anywhere the spec uses a single verb for something that could plausibly fail, pause, or be observed halfway through, ask what's actually inside that verb.

**Hidden-state challenge.** What state has to exist somewhere for the spec's described behavior to be possible at all, even though the spec never represents it explicitly? If the behavior requires remembering something between two points in time, that memory is state — name it.

**Boundary walk.** What happens in the moment immediately before the documented workflow begins, and the moment immediately after it ends? Specs are usually precise about the middle and vague about the edges — the edges are where handoff problems live.

**Interruption test.** Stop the process at every meaningful step, one at a time. At each stopping point: what survives, and who or what owns it in that stopped state?

**Double-execution test.** What happens if this exact operation happens twice — because of a retry, a duplicate event, a user double-click, or a replayed message?

**Concurrency test.** What happens if two different actors legitimately perform this operation at the same time, both acting in good faith?

**Partial-success test.** What happens if half of a multi-part operation succeeds and the other half doesn't? Does the spec have a notion of partial completion, or does it only model all-or-nothing?

**False-success test.** What happens if the system believes an operation succeeded, but it actually didn't (e.g., the confirmation was lost, not the operation)?

**False-failure test.** What happens if the system believes an operation failed, but the underlying effect actually went through (e.g., a timeout on the response, but the external side effect completed)? This is the mirror image of false-success and is just as dangerous, especially combined with a retry.

**Dependency-removal test.** Pick a major dependency this spec relies on and imagine it disappears tomorrow (a provider shuts down, a library is deprecated, a team that owns an internal service dissolves). What assumption about that dependency becomes visible only once it's gone?

**Role inversion.** What happens if the actor currently described as receiving something instead becomes the one initiating it? (E.g., if the spec assumes the server always initiates handoff, what if the client needs to initiate it instead?)

**Success consequence.** Suppose users love this capability — really love it, use it constantly. What's the very next thing they'll expect to be able to do, that the spec doesn't mention? (Overlaps with the product lens; use both.)

**Power-user projection.** What will a genuinely competent user try to do with this after two weeks of daily use, once they've internalized how it works and start pushing on its edges?

**Explainability test.** When something surprising happens in this system, what specific information will the user or operator demand in order to understand it — and does anything in the spec actually produce that information?

**Data-model regret.** What discovery, made after implementation, would force a painful change to the underlying data model — the kind of change that means a migration, not just a code change?

**Architecture regret.** Of all the assumptions in this spec, which single one, if wrong, would invalidate the largest amount of already-completed implementation work? That assumption deserves disproportionate scrutiny now, precisely because it's expensive to be wrong about later.
