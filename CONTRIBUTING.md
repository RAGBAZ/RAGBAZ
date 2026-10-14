# Contributing

A decision that two products must both obey is decided once, in this
repository, and cited by both. A decision only one product obeys belongs to that
product.

## Adding an RFC

1. Take the next number. Never reuse one, never renumber.
2. Open it as `rfcs/NNNN-short-slug.md`.
3. Start with: status, date, related RFCs, vocabulary, and a blockquote saying
   it is alpha and may change.
4. Use the document's own nouns. If a word is new, say so in the vocabulary line.
5. End with **What would make this falsified.** An RFC that cannot be wrong
   cannot be reviewed.

## Writing a deferral

A deferred question is a first-class part of an RFC. Name it, say why it is not
decided, and say what would unblock it. An omitted question reads as a decision
nobody noticed they were making.

## Adding to the decision index

Only link to a document that **already exists and is authoritative**. Do not
restate the decision here; a copy drifts from its source and then contradicts
it.

If the owner disagrees with the row, the owner's document wins. Fix the link.

## Before merging

- No secrets, credentials, personal data, board snapshots or live addresses.
- No absolute paths from one machine.
- Every relative link resolves.
- Claims about law or regulation are attributed and marked unverified where
  they were not verified by a lawyer.