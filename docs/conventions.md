# Conventions and Agreed Practices

This file records the *current, agreed* conventions for creating and managing RDF in **hs-crm-utils**.  
It is intentionally brief and practical. When something changes, append it to the changelog at the end.

---

## 1) Minimum requirements for any entity (ResearchSpace)

- Every entity **MUST** have at least one **CIDOC CRM class** (`rdf:type`), e.g. `crm:E22_Human-Made_Object`.
- Every entity **MUST** be identified by a **primary appellation**:
  - Create an **E41 Appellation** node at `{entity}/primary_appellation`.
  - Link from the entity using `crm:P1_is_identified_by` → that E41.
  - Put the human-readable text on the E41 using `crm:P190_has_symbolic_content`.
- Additionally, we **ALWAYS** set `rdfs:label` on the entity for generic tooling and indexing (outside strict CRM semantics).

> Rationale: ResearchSpace UX expects an asserted class and an appellation; `rdfs:label` improves interoperability with non-CRM tooling.

---

## 2) UUID / URI strategy

- **Deterministic UUIDs** are generated from the pair `(nslabel, nsgroup)`; if not supplied, defaults to `(label, group)`.
- Entity URIs are minted as: `{base}/{group}/{uuid}` by default.
- For predictable sub-paths, pass `isPartOfUri`, resulting in `{isPartOfUri}/{group}` (no UUID minting at this level).
- Re-running the same build with the same `(nslabel, nsgroup)` **MUST NOT** create duplicates.

---

## 3) Appellation pattern

- Appellation URI: `{entity}/primary_appellation`
- Appellation type: `crm:E41_Appellation`
- Appellation label field: `crm:P190_has_symbolic_content`
- We also assert `rdfs:label` on the appellation when useful for search (optional), but **not required**.

---

## 4) Types (AAT and local)

When adding **types** to an entity:

1. **Attempt AAT resolution first** via a normalised key lookup (`aatIndex`), accepting `aat:CURIE` or full IRI.
2. If no AAT match:
   - Accept a **ResearchSpace built-in** type IRI if provided (no extra node creation).
   - Otherwise **mint a local `crm:E55_Type`** with an appellation (via `easyRDFAddThing`), and add `skos:prefLabel`.
3. Always link the parent entity to the type via `crm:P2_has_type` (or the configured predicate).
4. Tag type membership with `crm:P71i_is_listed_in` → one of:
   - `.../aat_type`
   - `.../local_type`
   - (extendable list; see below)

> Conventions for `aatIndex` keys: lowercase, trim, spaces → underscores (e.g., “Inventory Number” → `inventory_number`).

---

## 5) Controlled lists

- Global controlled vocabularies: **Getty AAT** (prefer authoritative URIs).
- Local lists: maintain as JSON/CSV under `/data/controlled/` (TBD).
- When local terms are promoted to AAT matches, update `aatIndex` and backfill mappings when feasible.

---

## 6) Prefixes and namespaces (baseline)

- `crm:` → `http://www.cidoc-crm.org/cidoc-crm/`
- `rdfs:` → `http://www.w3.org/2000/01/rdf-schema#`
- `skos:` → `http://www.w3.org/2004/02/skos/core#`
- `aat:` → `http://vocab.getty.edu/aat/`

Projects may add more as needed.

---

## 7) Determinism and normalisation

- Normalise labels for UUID-name input: trim, collapse whitespace, case-fold.
- Keep `group` names stable and lowercased (e.g., `painting`, `sample`, `type`).
- If a human label changes but you require a stable URI, use `isPartOfUri` to pin the path.

---

## 8) Open questions (to resolve as we iterate)

- When should we allow `isSymbolicObject` to bypass E41? (current default: only when explicitly set)
- Do we want a standard secondary appellation path (e.g., `/alt_appellation/{n}`) for multilingual / variant titles?
- Should local E55 types always carry `skos:prefLabel` (current: yes)?

---

## Changelog

| Date       | Change |
|------------|--------|
| 2025-10-08 | Initial conventions: class + primary appellation required; rdfs:label added for interoperability; deterministic UUID/URI; AAT-first type policy. |
