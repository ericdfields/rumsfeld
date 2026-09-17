# Lens: Economics

**Core question:** which operations that look cheap or insignificant in the spec become materially costly once they happen at real volume or repetition?

## Review

- **Tokens and compute.** If this spec involves LLM calls, does anything in the described flow repeat a call unnecessarily (re-sending full context, re-running a step on every retry, polling with an expensive check) in a way that's fine once but adds up at scale?
- **Storage.** Does anything in the spec accumulate without bound (logs, history, checkpoints, generated artifacts)? Is there a described retention or cleanup policy, or does the spec implicitly assume storage is free?
- **API calls.** For each external integration, does the spec's flow call it once per unit of work, or could a retry loop, polling loop, or fan-out multiply that unpredictably?
- **Human review.** If any step in the spec requires a human to look at something, does the volume implied by the rest of the spec make that sustainable, or does it only work at the scale of the example the author had in mind?
- **Third-party charges.** Does any described operation map to a metered cost on a provider's side (per-call, per-token, per-seat, per-GB)? Is that cost visible anywhere in the spec, or only discoverable from a bill later?
- **Support burden.** Does the spec's design imply a support cost that grows with usage in a way nothing in the spec accounts for (e.g., every failure requires manual intervention)?

## How to use this lens

Take the spec's core operation and ask: what does this cost once? What does it cost at the volume the product would need to hit to be considered successful? If those two numbers imply a very different design decision, that's a finding.

## Example

Bad: "Costs could add up."

Good: "The spec has the agent re-send the full conversation context to the provider on every retry within a workstream, which is fine for occasional retries but means a workstream that gets stuck in a retry loop for an external reason silently multiplies token cost with no described ceiling or circuit breaker."
