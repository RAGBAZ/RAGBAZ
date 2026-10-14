# RFC 0002: Cross-fleet decision index

- **Status:** alpha, open
- **Date:** 2026-10-14
- **Scope:** fleet-wide. Nothing here is a decision; this is an index of where
  decisions actually live.

> **This file changes as the fleet does.** It is wrong the moment a product
> moves a document. It exists so that a decision is *found* once, by whoever
> needs it, instead of being reinvented per product.

## Why

The fleet already makes foundational decisions — about identity, provenance,
ownership and publication — and does it well. The decisions live with the
product that needed them first, which is right: the reasoning is only
meaningful next to what forced it.

The failure mode is not a missing decision. It is two products making
*different* decisions about the same foundation and discovering it in
production. This index exists to make that visible before it ships.

## Foundational decisions and their owners

Each row is a decision **someone already made**. The link is the authority. This
repository does not restate it, and must not be edited when it changes — edit
the owner's document and the link here is still correct.

### Identity and accountability

| Decision | Owner | Document |
| --- | --- | --- |
| A real name is never linked to a pseudonym | Mouseion | [`doc/00-Initial_plan.md`](https://github.com/tabenius/Mouseion/blob/main/doc/00-Initial_plan.md) §1 |
| Bylines are pseudonym, anonymous, or real name; enforced in schema, queries, chain actors and tests | Mouseion | [`doc/00-Initial_plan.md`](https://github.com/tabenius/Mouseion/blob/main/doc/00-Initial_plan.md) §2 |
| Secret keys live in `~/.gnupg`, never in a workspace directory | identities | [`identities/README.md`](https://github.com/RAGBAZ/identities) (box-local, **not** a Git repository) |
| The key registry records identity and vouching, and verifiers fail closed against a pinned set | identities | [`identities/keys.toml`](https://github.com/RAGBAZ/identities) |
| An artifact may be identified by its canonical manifest hash before publication | Mouseion | [`doc/03-Attestation_federation.md`](https://github.com/tabenius/Mouseion/blob/main/doc/03-Attestation_federation.md) §1 |

### Publication and provenance

| Decision | Owner | Document |
| --- | --- | --- |
| Publication date includes time, RFC 3339 UTC; visibility and claim validity are separate questions | Mouseion | [`doc/03-Attestation_federation.md`](https://github.com/tabenius/Mouseion/blob/main/doc/03-Attestation_federation.md) §1 |
| Artifact bodies are untrusted HTML: separate origin, sandboxed iframe without `allow-same-origin`, strict CSP | Mouseion | [`doc/VISION.md`](https://github.com/tabenius/Mouseion/blob/main/doc/VISION.md) (Isolation) |
| Revocation is append-only, time-effective, public and listable; it withdraws effects, it does not erase statements | Mouseion | [`doc/03-Attestation_federation.md`](https://github.com/tabenius/Mouseion/blob/main/doc/03-Attestation_federation.md) §1 |
| A censor may revoke attestations and nothing else | Mouseion | [`doc/03-Attestation_federation.md`](https://github.com/tabenius/Mouseion/blob/main/doc/03-Attestation_federation.md) §1 |
| Shared vocabulary is used where it fits, never replacing richer contracts with a lossy projection | Mouseion | [`doc/03-Schema_org_mapping.md`](https://github.com/tabenius/Mouseion/blob/main/doc/03-Schema_org_mapping.md) |
| Anonymous means omit the author, not invent an "Anonymous" person | Mouseion | [`doc/03-Schema_org_mapping.md`](https://github.com/tabenius/Mouseion/blob/main/doc/03-Schema_org_mapping.md) |

### Coordination and evidence

| Decision | Owner | Document |
| --- | --- | --- |
| A claim is recorded; evidence is separate and can be obsolete, current or failed | WeftMark | [`docs/`](https://github.com/tabenius/WeftMark/tree/main/docs) |
| Reported status is not evidence of completion | Frog | task `workflow_status` vs `git_status`, and this repository's [RFC 0001](0001-principal-subject-engagement.md) |
| Workspace attestation bundles are owned by WeftMark; Frog supplies identity and coordination data | WeftMark | `scripts/attest-workspace.py`, `attestations/` |
| Seal withdrawal changes serving status atomically and preserves history | Mouseion | [`doc/operations/`](https://github.com/tabenius/Mouseion/tree/main/doc/operations) |

### Distribution and ownership

| Decision | Owner | Document |
| --- | --- | --- |
| Frog owns repository discovery, identity, tasks, locks and SSH federation | Frog | `AGENTS.md` |
| Nostoi owns chain canonicalisation and verification; private signing keys stay outside all repositories | Nostoi | `AGENTS.md` |
| Dash applies capabilities per service and role | ragbaz-dash | `worker/` |
| A read-only workspace plus a read-write state directory is the deployment shape | fleet | emerging; see [RFC 0001](0001-principal-subject-engagement.md) §7 |

## Known gaps

These are decisions **nobody has made**. They are listed so that their absence
is a recorded fact rather than an oversight.

1. **Applicable law per product.** Which entity is the controller for which
   surface, and which establishment decides the rules. Deferred in
   [RFC 0001](0001-principal-subject-engagement.md) §7; not yet a decision.
2. **One person, many legal entities across countries.** Nobody owns this.
3. **Money.** RFC 0001 of Mouseion (`0001-value-flows.md`) is postponed by its
   own admission. No other product owns value flow either.
4. **Registration of persons.** Accounts, pseudonyms and agents are three
   parallel notions across `ragbaz-cc-accounts`, `bereaved-dad` and
   `ragbaz-home`. [RFC 0001](0001-principal-subject-engagement.md) proposes the
   shape; nobody has adopted it.

## How to use this index

1. Look here first when a decision seems to have no owner.
2. If the row's owner disagrees, the owner's document is right and this table
   is stale. Fix the link.
3. If a decision is genuinely cross-fleet and has no owner, propose it as a new
   RFC here. Do not decide it in a product repository and back-fill.