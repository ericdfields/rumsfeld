# Lens: Integrations

**Core question:** treat every external dependency as partially unreliable — what does the spec assume that stops being true the moment that dependency misbehaves?

Specs are usually written assuming external systems behave as documented, respond promptly, and fail cleanly. None of the three is reliably true.

## Consider, for each external dependency in the spec

- **Latency.** What does the spec assume about response time? What happens to the user-visible flow if this call takes 30x longer than expected?
- **Mismatched semantics.** Does the spec assume the external system's concepts map cleanly onto its own (e.g., the external system's "cancelled" means the same thing as this spec's "cancelled")? Where might that mapping actually be lossy or wrong?
- **Partial failure.** If this call is really three external calls, what happens if the first two succeed and the third fails? Does the spec have a notion of partial completion for this integration, or does it treat the call as atomic when the provider doesn't?
- **Changed API behavior.** The spec was written against the provider's current behavior. What happens when the provider changes something (a deprecation, a new required field, a behavior change) without asking?
- **Unavailability.** If this dependency is fully down, does the spec degrade gracefully, queue, or hard-fail? Does it say?
- **Stale responses.** Is there anywhere the spec might read a cached or previously-fetched value from this integration and treat it as current?
- **Retries and duplication.** If a call to this integration times out but actually succeeded on the provider's side, and the spec retries, does that retry double-charge, double-create, or double-send anything?

## Example

Bad: "The external API might go down."

Good: "The spec calls the provider to check job status and treats a timeout as 'still running,' but doesn't distinguish that from 'provider accepted the job and is running it but the status endpoint itself is degraded' — a prolonged status-endpoint outage would make every in-flight job look permanently stuck even though the underlying work may have completed."
