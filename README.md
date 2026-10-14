# RAGBAZ

Fleet-wide product design decisions for RAGBAZ.

This repository holds the decisions that are **theirs**, not any single
component's. A product in the fleet may implement these decisions, cite them,
or deliberately contradict them — but it does not decide them twice, in two
repositories, without noticing.

It is deliberately thin. The authoritative design documents live with the
projects that own them. This repository records which decisions are foundational,
points at the documents that hold them, and records the few that belong to
nobody yet.

## What belongs here

- **Foundational decisions** that more than one product depends on: identity,
  ownership, consent, provenance, publication, money.
- **An index** of the authoritative documents that already exist elsewhere.
- **Open questions** that are cross-fleet and therefore cannot be settled by one
  repository alone.

## What does not belong here

- Implementation detail of one product. That stays with the product.
- Restating a decision that already has an owner. Link to the owner's file.
- Speculation. An open question is welcome; an unexamined proposal is not.

## Documents

| RFC | Title | Status |
| --- | --- | --- |
| [RFC 0001](rfcs/0001-principal-subject-engagement.md) | Principals, subjects, engagements | Alpha |
| [RFC 0002](rfcs/0002-cross-fleet-decision-index.md) | Cross-fleet decision index | Alpha |

## The rule this repository enforces

> A decision that two products must both obey is decided **once**, here, and
> cited by both. A decision only one product obeys belongs to that product.

## Naming

Decided: this repository is **RAGBAZ** (`RAGBAZ/RAGBAZ`), matching the
organisation that owns it. Its role is **decision home**, not a product.

Rejected names and why, so the choice is not re-litigated:

| Candidate | Why not |
| --- | --- |
| `ragbaz-spec` | Implies normative specification. This repository holds decisions, and many are deliberately provisional. |
| `ragbaz-core` | Implies shared code. There is none, and coupling products to a library is exactly what this repository avoids. |
| `ragbaz-platform` | Implies a runtime. The fleet is not one runtime. |
| `ragbaz-design` | Too close to `design-system`, which owns visual language and is a different thing. |

## Relationship to the fleet

Existing authoritative documents are indexed in
[RFC 0002](rfcs/0002-cross-fleet-decision-index.md). The most load-bearing today:

- Mouseion — provenance, bylines, pseudonym boundaries, artifact isolation.
- Nostoi — the tamper-evident chain every publication decision assumes.
- Frog — tasks, claims, locks, and the coordination vocabulary.
- WeftMark — change sets and evidence, the prospective successor to task state.
- identities — box-local key material and the key registry, which is not a Git
  repository and must not become one.