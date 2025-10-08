# Conventions and Agreed Practices

This document records agreed conventions for creating and managing RDF data
within the **hs-crm-utils** environment.  
It will evolve as new functions, models, and decisions are introduced.

---

## 1. UUID and URI conventions

- **UUIDs** are deterministic, generated from a combination of `label` and `group`.
- **URIs** follow the structure:  
  `{base}/{group}/{uuid}`  
  e.g. `https://data.example.org/painting/3c8e91d1-...`
- **Sub-resources** (such as appellations) append a suffix:  
  `{uri}/primary_appellation`

---

## 2. Label and symbolic content

- All entities carry an `rdfs:label`.
- When the entity has a linguistic name or title, it is represented through an
  **E41 Appellation** linked via `crm:P1_is_identified_by`.
- The symbolic content of that appellation is recorded with
  `crm:P190_has_symbolic_content`.

---

## 3. Controlled vocabularies

- AAT terms are referenced by URI when available.
- Local controlled lists (e.g. preparation types, component sizes) are maintained
  in JSON or CSV lists under `/data/controlled/`.
- Each controlled list should have:
  - a **label** (display text)
  - a **URI or local ID**
  - an optional **AAT mapping**

---

## 4. Graph naming and namespaces

- The default prefix set includes:
  - `crm:` → `http://www.cidoc-crm.org/cidoc-crm/`
  - `rdfs:` → `http://www.w3.org/2000/01/rdf-schema#`
  - `aat:` → `http://vocab.getty.edu/aat/`
- Project-specific prefixes can be added in each script.

---

## 5. Versioning

- Any change that affects output RDF structure or field naming should be noted
  here with date and brief explanation.

| Date | Change | Notes |
|------|---------|-------|
| YYYY-MM-DD | Initial conventions added | — |
