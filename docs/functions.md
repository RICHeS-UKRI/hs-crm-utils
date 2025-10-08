# Function Notes and Commentary

This page records notes, comments, and development decisions about the PHP
functions included in **hs-crm-utils**.  
It is meant as a lightweight technical log rather than a formal manual.

---

## easyRDFAddThing()

**Purpose:**  
Create a new entity with a label, UUID-based URI, and (by default)
a CIDOC CRM E41 Appellation carrying the same label.

**Key points**
- Uses `UUIDManager` → deterministic UUID from label + group.
- Adds `rdfs:label` to all entities.
- Adds an E41 Appellation unless `isSymbolicObject` is set.
- Accepts optional `isPartOfUri` for nested resources.

**Discussion / open questions**
- Should symbolic objects (like literal text or titles) always skip E41, or only
  when explicitly flagged?
- Consider whether `crm:P1_is_identified_by` should use typed appellations other
  than `primary_appellation` in some contexts.

---

## easyRDFAddTypes()

**Purpose:**  
Attach one or more types (AAT or local) to an existing resource.

**Key points**
- Takes `$aatIndex` for URI lookup.
- Adds `crm:P2_has_type` relations.
- Should support multiple entries gracefully.

**Discussion / open questions**
- Need to confirm handling of types that lack a URI.
- Should we add `skos:prefLabel` to local type nodes for readability?

---

## To add a new function

When introducing a new helper:
1. Add a section here with a brief purpose statement.
2. Note any dependencies (EasyRdf, UUIDManager, other helpers).
3. Summarise expected output and open questions.

---

## Change log

| Date | Function | Summary of change |
|------|-----------|-------------------|
| YYYY-MM-DD | easyRDFAddThing | First draft of documentation |
| YYYY-MM-DD | easyRDFAddTypes | Added initial comments |
