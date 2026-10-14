# RFC 0001: Principals, subjects, engagements

- **Status:** alpha, open. Nothing here is adopted. Sections 1–4 are the
  proposal; sections 5–8 are deliberately deferred and exist only to name what
  is being deferred.
- **Date:** 2026-10-14
- **Related:** [RFC 0002](0002-cross-fleet-decision-index.md)
- **Vocabulary:** this document says **principal**, **identity**, **persona**,
  **scope**, **role binding**, **capability**, **subject**, **duty**,
  **engagement**, **suppression**. No new nouns are introduced beyond these.

> **Alpha — published as written.** This is a working document, not a
> specification. The parts being argued about are the parts most likely to be
> rewritten. It is published because the reasoning is part of the work: a design
> you cannot argue with is a design you cannot check.

## 1. What is being asked for

One vocabulary for **who is acting** and **whose data we hold**, so that
identity stops being re-decided per product.

The fleet currently has at least four partial answers to the same question:

| Surface | What its "account" actually is |
| --- | --- |
| `ragbaz-cc-accounts` | A human, keyed by email, with passkeys and sessions |
| `bereaved-dad` | A human **plus a separate pseudonym**, in zines with member roles |
| `ragbaz-home` | A **peer node**, authenticated by passkey and gift key, with tenants |
| `detcordon-marketing-leads` | A **subject without an account**: an email, an expiry, interests |

None is wrong. They answer different questions, and that is precisely the
problem: a person who is a lead, then an account, then a publisher, then an
agent's operator crosses four unrelated schemas.

## 2. The load-bearing distinction

```
principal  — who is acting:  human, agent, organisation, service
subject    — whose data we hold, and under what duty
```

A person can act, be written about, or both. Collapsing these into one row is
the error to avoid.

Three consequences follow immediately:

1. **A lead who never converts is a subject only.** At the scale this fleet
   actually operates at, that is the common case, and it should not require an
   account to represent honestly.
2. **Conversion links, it never migrates.** Consent, jurisdiction and retention
   do not move when a subject becomes a principal. Nothing is copied, so
   nothing can be lost.
3. **An agent is a principal with a role**, not a parallel entity type. This is
   the point where "a project may be an agent, an account may be an agent, a
   lead may be an agent" stops being three special cases.

## 3. The model

Six concepts, orthogonal by design:

| Concept | Question it answers |
| --- | --- |
| `principal` | Who is acting? |
| `identity` | How is it authenticated? |
| `persona` | How does it appear publicly? Optional; agents may have none. |
| `role_binding` + `scope` | What may it do, and where? |
| `subject` + `duty` | Whose data is this, and for how long? |
| `engagement` | What happened? |

- **Identity is separate from principal.** Passkey, TOTP, recovery code, OIDC,
  API secret. One principal, many identities; one identity, one principal.
- **Persona is per scope, not global.** A pseudonym reused across surfaces is a
  cross-surface linkage the user never asked for. Per-scope personas give more
  unlinkability *and* clearer accountability.
- **Capabilities live on the scope**, not the principal. `ragbaz-dash` already
  implements capabilities per service and role; this RFC does not replace that,
  it only names the concept.
- **Engagement is one stream with a kind** — interest, application, meeting,
  Q&A, session, note — so that retention and erasure become one problem rather
  than one per product.
- **Suppression** is a stable hash of a subject, retained after erasure, so that
  a later import does not resurrect someone who asked to be forgotten.

## 4. Why the role machinery already exists

This RFC does not introduce a new concept for roles. `bereaved-dad` already has
`zine_member_roles(zine_id, pseudonym_id, role)` with `editor`, `author` and
`proofreader`. A `principal` and a `role_binding` is the same machinery with
federated ids.

**The existing schema is already close to right.** This RFC maps onto it rather
than replacing it, and the migration cost is a `principal_id` column, not a
rewrite.

## 5. Deferred: publication and the responsible publisher

A scope may later become a regulated publication. Nothing below is decided; all
of it is absent by default and unreachable from product code.

```sql
-- A fact about the outside world. A row exists only after an external
-- authority has actually issued a certificate. Absent = no registration.
CREATE TABLE publication_registration (
  scope_id          TEXT PRIMARY KEY,
  certificate_no    TEXT NOT NULL,
  registered_at     TEXT NOT NULL,
  expires_at        TEXT NOT NULL,
  publisher_role_id TEXT NOT NULL REFERENCES zine_member_roles(id)
);

-- One more value in a table that already exists.
-- zine_member_roles.role: 'editor','author','proofreader'
--                                 + 'responsible_publisher'
```

Three rules make this safe to model now:

1. **The certificate is a claim about the outside world.** Only an external
   authority can make it true, so this row is written by an operator or a
   migration, never by an API handler and never by a user.
2. **Until the row exists, the role means nothing and nothing reads it.**
3. **The only decision required now** is `editorial_control` on the scope:
   `community` or `editorial`. That is an observation of what members actually
   do, not a legal conclusion.

Deferred deliberately: liability regime, press subsidy eligibility, defence
mechanics, and any transfer of money between a publication and an external
recipient. That last one is a different legal regime entirely and should stay
outside this model rather than exist as a disabled field.

## 6. Deferred: jurisdiction, consent and erasure

Applicable law is decided by **establishment** and **where the subject is when
the trigger activity happens** — never by a self-declared status. A declaration
is evidence about a fact; it is never an input that reduces an obligation.

So: replace any single `jurisdiction` scalar with `establishment`, `presence at
event`, and `declared` (with confidence and timestamp). Policy resolves as
`strictest(establishment, presence)`, and `declared` may only tighten.

Three facts that are routinely conflated and must stay separate in the model:

- **channel verification** — proves inbox or device control. Not identity, not
  consent.
- **bot mitigation** — abuse control. Not a legal concept, and a third-party
  transfer where a third party is involved.
- **consent** — a legal act with validity conditions, withdrawal, and an
  evidence burden that falls on the controller.

Deferred: which entity is controller for which surface, how obligations are
resolved per jurisdiction, and the parental-consent path for minors.

## 7. Deferred: deployment shape

The intended shape is a **read-only workspace with a read-write state
directory**, on a rootless service, with repositories bind-mounted read-only.
Separate coordinator and runner profiles, because builds and edits are not
coordination.

Deferred: transport between CLI and service, lease and fencing semantics, and
the file and scope locks that would make a new backend a drop-in for
`AGENTS.db`.

## 8. What would make this RFC falsified

Written down so it can be argued with rather than merely adopted:

- If a lead is always either an account or gone, the principal/subject split is
  unnecessary complexity.
- If per-scope personas measurably reduce safety or support, the persona model
  is wrong.
- If one engagement stream cannot hold a product's real data, the
  generalisation is premature.
- If federated `principal_id` columns cost more than the drift they prevent,
  each product should keep its own identity and this RFC should be withdrawn.