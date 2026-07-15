# ONEDGE Protocol Advancement Standard

**Identifier:** `ADVANCEMENT-1.0.0`  
**Version:** 1.0.0  
**Status:** Operative / Registered / Governance-bound  
**Effective vector:** `UVS-2 / S-0.0.4 / I-1.1.0-rc3 / P-1.0.0 / G-1.4.0`  
**Issuer:** IKKUNA SpA (StandardCo), by Principal instruction  
**Effective date:** 2026-07-14  
**Change class:** Normative governance control; no S/I/P technical semantics changed

## 1. Authority, purpose and scope

This Standard governs how an ONEDGE change advances from a recommendation or specification
candidate into an active, enforceable and publicly legible protocol state. It applies whenever a
change affects one or more of the following surfaces:

1. the Standard, Implementation, Proof or Governance axis;
2. a canonical schema, codec, hash construction, proof relation, invariant, reason code, operator,
   parameter, ABI or compatibility binding;
3. a reference or production implementation;
4. registered conformance vectors or reproducible-build evidence;
5. normative, implementation, audit, evidence-room, legal-reliance or public documentation; or
6. the ONEDGE website, public downloadable artifacts or machine-readable conformance claims.

`onedge-manifest.json` remains authoritative for the active vector, registered artifacts and
compatibility. `CONFORMANCE_UPGRADE_BACKLOG.md` remains the unique recommendation inventory.
`CHANGELOG.json` remains the structured semantic-delta history. This Standard creates neither a
competing recommendation ledger nor a technical source above a LaTeX canonical specification.

Where this Standard conflicts with an S/I/P technical definition, the source hierarchy in
`AGENTS.md` governs the technical definition and the conflict MUST block advancement until it is
resolved. This Standard governs the advancement process and the evidence required to change the
active state.

## 2. Design principles

### 2.1 Proposal is not adoption

A draft, backlog admission, implementation, successful test or public discussion MUST NOT be
represented as active ONEDGE protocol behavior. Adoption occurs only through the state transition
and release-capsule rules in this Standard.

### 2.2 Compatibility is a proved relation, not a label

The word “compatible” MUST be qualified by surface and direction. Every advancement MUST evaluate
wire/codec, commitment, verification, historical replay, producer/consumer, documentation and
public-claim compatibility. A declaration without the matrix in §6 is incomplete.

### 2.3 Unknown profiles fail closed

Bitcoin soft forks can leave older nodes operating without enforcement of the new rule because
upgraded network participants collectively enforce the restriction. ONEDGE has no equivalent
consensus majority that can confer that protection on an obsolete verifier. An ONEDGE verifier
that does not support a record's declared profile MUST return `UNSUPPORTED` or `REJECT`; it MUST
NOT return `CONFORMANT`, `VERIFIED` or an equivalent affirmative result under newer semantics.

### 2.4 Dual-read, single-write

During a compatible migration, current software MAY verify both the registered historical profile
and the new profile, but after enforcement it MUST emit only the active profile. A retired profile
remains available for historical replay and MUST NOT silently regain issuance authority.

### 2.5 No semantic rollback

Publication infrastructure MAY restore a prior public snapshot before activation. Once a protocol
state is active, it cannot be erased or made never-active by replacing files or lowering a version
number. A defect requires containment and a new versioned successor, with the affected interval and
records preserved for audit.

### 2.6 One release, one legibility state

The canonical specifications, implementation artifacts, vectors, documentation, evidence-room
instructions, machine-readable claims and public website MUST identify the same advancement state.
An advancement is not publicly complete while any normative or reliance-bearing public surface
describes a different active vector, profile, codec, hash or conformance status.

## 3. Separate lifecycles

Recommendation lifecycle, proposal maturity and deployment activation are distinct.

1. Recommendation identity, priority, dependencies, status and history are governed only by the
   stable `CUR-NNNN` record in `CONFORMANCE_UPGRADE_BACKLOG.md`.
2. A proposal MAY progress through `IDEA`, `DRAFT`, `CANDIDATE`, `ACCEPTED`, `COMPLETE` and
   `CLOSED`. Proposal status does not authorize production.
3. A deployment progresses only through the state machine in §8. Deployment state determines
   whether production or conformance claims are authorized.

No document MAY use proposal status as a synonym for deployment status.

## 4. Advancement record

Every normative or consensus-critical deployment MUST have one permanent advancement identifier
and one immutable update tuple:

$$
\mathcal{U} =
(id,V_{from},V_{to},C,H,D,G,A,E,R).
$$

The tuple components are:

| Symbol | Required meaning |
|---|---|
| $id$ | Permanent advancement/deployment identifier; never reused. |
| $V_{from}$ | Complete source UVS vector. |
| $V_{to}$ | Complete target UVS vector. |
| $C$ | Compatibility class from §5. |
| $H$ | Exact artifact, evidence, documentation and publication hashes. |
| $D$ | Backlog and artifact dependencies. |
| $G$ | Objective qualification and activation gates. |
| $A$ | Activation anchor. |
| $E$ | Enforcement anchor. |
| $R$ | Retirement, replay and containment policy. |

The advancement identifier MUST differ from its `CUR-NNNN` recommendation identifier. One backlog
record may require more than one deployment attempt; failed or aborted advancement identifiers are
never reassigned.

## 5. Compatibility classes

Every affected surface MUST be classified. The deployment class is the strictest class found on
any surface.

| Class | Definition | Required treatment |
|---|---|---|
| `C0-EDITORIAL` | No normative meaning, accepted language, bytes, commitment, verification result or claim changes. | No activation state machine; ordinary registered publication and hash refresh where applicable. |
| `C1-REPLAY-COMPATIBLE` | A correction or clarification preserves registered outputs and historical semantics under the pinned old profile. | Patch successor; byte-equality vectors plus negative tests for rejected alternative readings. |
| `C2-VERSIONED-EXTENSION` | New behavior is introduced under an explicit profile while historical records remain replayable. | Staged activation; old verifiers fail closed on the new profile; dual-read/single-write migration. |
| `C3-BREAKING-MIGRATION` | Codec, digest, proof relation, invariant, interpretation or required behavior changes incompatibly. | Appropriate major axis/profile movement, explicit migration, coexistence interval and new vectors. |
| `C4-EMERGENCY` | An active state is unsafe, compromised or materially misleading. | Emergency overlay on C1–C3: contain issuance, publish advisory, preserve history and activate a versioned successor. |

`C4-EMERGENCY` MUST NOT be used to describe an incompatible change as compatible or to bypass
evidence that remains safely obtainable.

## 6. Directional compatibility invariants

Every C1–C3 candidate MUST publish and execute this producer/verifier matrix:

| Record producer | Historical verifier | Successor verifier |
|---|---|---|
| Historical profile | Result unchanged | Same result under explicitly pinned historical profile |
| Successor profile | `UNSUPPORTED` or `REJECT` | Deterministic result under successor profile |

For every registered historical vector $x$:

$$
\operatorname{Verify}_{new}(x,V_{old})
=
\operatorname{Verify}_{old}(x,V_{old}).
$$

For every verifier $v$ and record $x$:

$$
\operatorname{profile}(x) \notin \operatorname{Supported}(v)
\Longrightarrow
\operatorname{result}_{v}(x) \in
\{\mathsf{UNSUPPORTED},\mathsf{REJECT}\}.
$$

The result MUST NOT be an affirmative whole-conformance or verification claim. Two implementations
that attach different semantics to the same version/profile identifier are incompatible even when
their parsers accept the same bytes.

## 7. Release capsule

Before `LOCKED_IN`, the candidate MUST be represented by one hash-frozen release capsule. The
capsule MUST contain at least:

- advancement ID and governing `CUR-NNNN` records;
- proposal type, affected layers/axes and security severity;
- full $V_{from}$ and $V_{to}$ vectors;
- compatibility class and completed directional matrix;
- exact paths, lengths, versions and SHA-256 hashes for every target artifact;
- canonical source, Markdown mirror, implementation and conformance-vector hashes;
- build provenance, resolved dependencies and reproduction commands;
- documentation and public-legibility bill of materials from §12;
- qualification evidence and unresolved limitations;
- activation, enforcement, grace, retirement and containment anchors;
- successor, replacement and historical-profile relationships;
- authorization mode, approving roles and signatures or permitted bootstrap evidence; and
- capsule codec/version and its own canonical digest.

The capsule codec and signature profile MUST be registered before the first non-bootstrap
deployment locks in. Any semantic byte change to a locked capsule or target returns the candidate
to `DEFINED` under a new advancement identifier. A bounded editorial-only delta MAY retain the
identifier only when an independent delta review records why the capsule meaning is unchanged and
all hashes are refreshed.

## 8. Deployment state machine

The only deployment states are:

`DEFINED`, `QUALIFYING`, `LOCKED_IN`, `ACTIVE_GRACE`, `ENFORCED`, `BURIED`, `FAILED`, `ABORTED` and
`CONTAINED`.

Allowed ordinary transitions are:

$$
\mathsf{DEFINED} \rightarrow \mathsf{QUALIFYING}
\rightarrow \mathsf{LOCKED\_IN} \rightarrow \mathsf{ACTIVE\_GRACE}
\rightarrow \mathsf{ENFORCED} \rightarrow \mathsf{BURIED}.
$$

Exceptional transitions are `DEFINED|QUALIFYING → FAILED`, `LOCKED_IN → ABORTED`, and
`ACTIVE_GRACE|ENFORCED|BURIED → CONTAINED`. `CONTAINED` MUST point to an incident record and a new
advancement attempt or an explicit issuance freeze.

### 8.1 State meanings

- **DEFINED:** the proposal and dependencies are identified; it has no production authority.
- **QUALIFYING:** an exact release candidate is under the gates in §9. Semantic changes restart
  qualification.
- **LOCKED_IN:** the capsule is immutable, authorized and scheduled. Only an abort or activation
  may follow.
- **ACTIVE_GRACE:** the target vector/profile is active. Legacy production is allowed only when the
  capsule explicitly grants a bounded grace interval.
- **ENFORCED:** new production MUST use the active profile. Historical profiles are verify-only.
- **BURIED:** the activation is sufficiently settled to simplify transitional logic, but the
  capsule, activation record, historical artifacts and replay verifier remain preserved.
- **FAILED:** a qualifying candidate missed its timeout or failed a mandatory gate.
- **ABORTED:** a locked deployment was stopped before activation; the exact reason is preserved.
- **CONTAINED:** an active deployment is unsafe or misleading; issuance and claims follow §11.

State transitions MUST use a monotonically increasing release sequence. A lower sequence or
version MUST NOT replace a higher trusted state.

## 9. Qualification and lock-in gates

Before `LOCKED_IN`, every applicable gate below MUST pass or be identified as not applicable with a
falsifiable rationale:

1. LaTeX canonical and Markdown mirror parity.
2. Complete UVS and per-surface compatibility classification.
3. All five `AGENTS.md` collision checks.
4. Reference implementation and an authorship-independent verifier for security-critical behavior.
5. Positive, boundary, malformed, replay, substitution and stateful adversarial vectors.
6. Historical-to-successor replay equality.
7. Successor-to-historical fail-closed behavior.
8. Two clean reproducible builds for registered executable or generated artifacts.
9. Independent recomputation of cryptographic outputs.
10. Source, dependency, build-platform and output provenance.
11. Security review proportionate to the backlog severity.
12. Documentation and cross-reference reconciliation.
13. Evidence-room, audit, lender, OEM, insurer and operator reliance-impact review where relevant.
14. Legal, IP, privacy, warranty and public-claim review where relevant.
15. Staged website, download, checksum, mirror and cache verification.
16. Capsule authorization and deterministic activation/enforcement anchors.
17. Tested abort, publication rollback and post-activation containment procedures.

Readiness telemetry MAY inform a decision but MUST NOT replace a mandatory gate. ONEDGE does not
use implementer popularity or deployment counts as a substitute for specified evidence.

## 10. Activation anchors and notice

Every C1–C3 capsule MUST identify:

- a monotonic release sequence;
- a `not_before_utc` timestamp;
- an exact activation anchor;
- an exact enforcement anchor;
- any optional chain anchor as `(chain_id, block_height)`;
- the legacy production sunset; and
- the historical replay-retention rule.

Records MUST bind their own version/profile; wall-clock time alone MUST NOT select cryptographic or
verification semantics.

Unless a stricter governing instrument applies, the minimum public notice is seven calendar days
for C1, thirty calendar days for C2 and ninety calendar days for C3. A C4 emergency MAY shorten
notice only through an explicit, reasoned authorization record that states which evidence was
retained, which was impossible to obtain, and why delay creates greater risk.

## 11. Issuance, retirement and emergency containment

After `ENFORCED`, producers MUST NOT create new records under a retired profile. Successor verifiers
MUST retain historical replay for every registered profile unless a security advisory requires a
quarantined verification mode. Quarantined historical verification MUST expose the advisory and
MUST NOT convert an affected historical result into a current-conformance claim.

For C4 emergencies, StandardCo MUST:

1. freeze or bound affected issuance and public claims;
2. identify the affected versions, profiles, time/sequence interval and artifact hashes;
3. publish a machine-readable and human-readable advisory;
4. preserve evidence and distinguish unknown from invalid records;
5. create a new advancement identifier and successor version;
6. prohibit downgrade or removal of the incident from public status; and
7. re-run all safely obtainable gates before successor activation.

An emergency does not authorize in-place mutation of a released canonical artifact.

## 12. Documentation and institutional-legibility gate

Every C1–C4 advancement MUST carry a documentation bill of materials. Each affected item MUST be
classified as one of:

- canonical normative source;
- normative mirror or SDK documentation;
- implementation/reference documentation;
- explanatory, theoretical or training material;
- audit, evidence-room or machine-readable claim artifact;
- contractual, legal-reliance or communications material;
- public website or downloadable artifact; or
- obsolete but historically retained material.

For each item the bill MUST record current and target version/hash, required action (`UNCHANGED`,
`UPDATE`, `ERRATUM`, `DEPRECATE`, `ARCHIVE`), owner and verification evidence.

The legibility review MUST test the full chain:

$$
\text{physical event}
\rightarrow \text{statement/proof}
\rightarrow \text{Golden Record commitment}
\rightarrow \text{report/evidence room/publication}
\rightarrow \text{institutional interpretation}.
$$

The review MUST establish that a lender, OEM, insurer, auditor, operator or dispute reviewer can
identify the applicable vector/profile, obtain its immutable artifacts and reproduce the claimed
verification method. A digest without discoverable method, schema, vector and provenance identity
is not sufficient institutional legibility.

## 13. Public publication and machine-readable status

The public publication profile MUST provide the logical equivalents of:

```text
/protocol/root.json
/protocol/status.json
/protocol/releases/<sequence>/snapshot.json
/protocol/releases/<sequence>/targets.json
/protocol/releases/<sequence>/<artifact-hash>/<artifact>
```

- `root.json` identifies authorization roles, keys and thresholds.
- `status.json` is a short-lived authenticated pointer to active, locked, legacy, failed, aborted
  and contained deployments.
- `snapshot.json` commits one coherent manifest/backlog/changelog/specification/documentation tuple.
- `targets.json` commits the length and digest of every public artifact.

The registered codec, authentication and expiry profiles are completion evidence for `CUR-0033`.
Before that record is verified, no release may claim automated rollback, freeze or mix-and-match
protection.

Human-readable pages and machine-readable status MUST be produced from the same release capsule or
must pass a deterministic equality check. Versioned URLs are immutable. Superseded pages MUST show
their state and canonical successor. Only an explicitly unversioned “latest” URL may redirect.

Post-publication verification MUST check origin, CDN/mirror, link, length, digest, content type,
status label and active-vector equality. Publication rollback is permitted before activation; after
activation, correction requires containment and a new signed snapshot or authorized successor.

## 14. Authorization profiles and bootstrap

A technical S/I/P deployment MUST use the registered cryptographic release-authorization profile
and satisfy the signature/human qualification gates governing `CUR-0022` and `CUR-0023`.

Until StandardCo threshold roles and keys are registered, `bootstrap-human-record` authorization
MAY be used only for a G-only governance change that does not activate or alter S/I/P technical
semantics. It requires explicit Principal approval in `CHANGELOG.json`, manifest provenance and
exact artifact hashes. It MUST NOT be represented as a cryptographic signature.

The adoption of this Standard as G-1.4.0 is bootstrap sequence `0`. This exception is single-use
for `ADVANCEMENT-1.0.0`; because the capsule, qualification and notice rules did not exist before
their own adoption, sequence `0` alone is exempt from §§7–10. The exception MUST NOT be cited to
activate a technical successor. The first non-bootstrap deployment sequence is `1` and cannot
enter `LOCKED_IN` until `CUR-0033` is verified.

## 15. Integration with existing governance

Agents and maintainers MUST:

1. reconcile every proposed advancement with its existing or new `CUR-NNNN` record;
2. add the advancement state and capsule reference to the manifest without duplicating backlog
   priority or lifecycle fields;
3. append the required structured delta to `CHANGELOG.json`;
4. treat `CUR-0021` machine-readable claims as an input to the public status profile;
5. treat `CUR-0033` as the operationalization gate for this Standard; and
6. treat `CUR-0023` as the terminal whole-release admission gate after `CUR-0033`.

The GRSCHEMA §5 / SDK successor governed by `CUR-0010` SHOULD be the first non-bootstrap pilot. Its
qualification MUST keep the schema, SDK bundle, conformance vectors, manifest/changelog tuple,
documentation and public-link transition within one release capsule.

## 16. Advancement SemVer

This Standard uses Semantic Versioning independently of the S/I/P/G axes:

- **MAJOR:** incompatible state semantics, compatibility classes, authority model or capsule
  identity rules;
- **MINOR:** additive state, field, gate, publication or authorization capability; and
- **PATCH:** clarification or correction that does not change required advancement behavior.

Any normative amendment requires the corresponding G-axis classification, manifest/hash update,
structured changelog entry and backlog reconciliation.

## 17. Informative design sources

The design adapts, without importing Bitcoin's decentralized authority model:

- Bitcoin BIP 3, proposal status, adoption evidence, preserved closed history and deployed-spec
  stability: <https://github.com/bitcoin/bips/blob/master/bip-0003.md>
- Bitcoin BIP 9, staged deployment states, timeouts, warning behavior and capability declaration:
  <https://github.com/bitcoin/bips/blob/master/bip-0009.mediawiki>
- Bitcoin BIP 90, burial of settled activation mechanics without discarding history:
  <https://github.com/bitcoin/bips/blob/master/bip-0090.mediawiki>
- Bitcoin BIP 141, the security limits of non-upgraded validation:
  <https://github.com/bitcoin/bips/blob/master/bip-0141.mediawiki>
- Bitcoin BIP 341, explicit versioning, reserved extension surfaces and tagged hashing:
  <https://github.com/bitcoin/bips/blob/master/bip-0341.mediawiki>
- The Update Framework, signed role separation, consistent snapshots and rollback/freeze/mix-and-
  match resistance: <https://theupdateframework.github.io/specification/draft/>
- SLSA provenance, source/dependency/build/output traceability:
  <https://slsa.dev/spec/v1.2/provenance>

---

End of `ADVANCEMENT-1.0.0`.
