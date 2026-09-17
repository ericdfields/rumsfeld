# Lens: Data

**Core question:** follow every important piece of information from where it's born to where it dies — where does the spec's story break down?

## Procedure

For each important piece of data in the project model, trace it through as many of these stages as apply:

```
origin
creation
validation
storage
mutation
replication
conflict
expiration
deletion
recovery
```

## Look for

- **Hidden sources of truth.** Is there ever more than one place that could plausibly answer "what is the current value of X"? If the spec has a cache, a derived field, a client-side copy, or a denormalized view, what happens when two of these disagree?
- **Validation gaps.** Where does data actually get checked for correctness, and is there a path into the system that skips that check (an import, a migration, a direct API call, an internal service call)?
- **Mutation without versioning.** If a piece of data can change after creation, does the spec track who changed it, when, and what it was before — or does the old value simply vanish, which matters the moment someone asks "wait, what did this used to say"?
- **Conflict.** Can two actors legitimately modify the same data concurrently? What does the spec say happens — last write wins, merge, reject, lock? If it doesn't say, that's the finding.
- **Expiration and deletion.** Does anything in this spec's data model need to go away eventually (for cost, privacy, or relevance reasons)? What deletes it, and does deleting it break anything else that referenced it?
- **Recovery.** If a piece of data is lost or corrupted, is there any way to reconstruct it, or was it the only copy?

## Example

Bad: "We should validate the data."

Good: "The spec treats `current_version` as a single field on the parent object, but the workflow allows generating a new version while a previous generation is still in flight — there's no described behavior for what happens if the in-flight generation completes after a newer version has already been set as current."
