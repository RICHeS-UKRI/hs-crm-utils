# Function Notes and Commentary

A lightweight technical log for functions in **hs-crm-utils**.  
Focus: purpose, behaviour, and decisions—not exhaustive API docs.

---

## `easyRDFAddThing(...)`

**Purpose**  
Create a CIDOC CRM entity with a human-readable label and (by default) a proper **E41 Appellation**.  
Supports deterministic URI minting and optional fixed parent paths.

**Key behaviour**

- Always adds `rdfs:label` on the entity.
- If **not** `isSymbolicObject`, creates an E41 at `{thing}/primary_appellation`:
  - `rdf:type crm:E41_Appellation`
  - `crm:P190_has_symbolic_content` = label
  - `crm:P2_has_type` = ResearchSpace primary_appellation type
  - Link via `crm:P1_is_identified_by` from the entity.
- Asserts all supplied `rdf:type` classes.
- Calls `easyRDFAddTypes(...)` when a non-empty `$types` array is provided.
- URI minting:
  - If `$isPartOfUri` is supplied: `{isPartOfUri}/{group}`
  - Else: `UriFactory->mint($group, UUIDManager->get(nslabel, nsgroup))`

**Why we do this**  
ResearchSpace expects a class and an appellation. `rdfs:label` improves usability in generic RDF tooling and search.

**Open notes**  
- Only set `isSymbolicObject` for entities where the *thing itself* is the textual content.
- Consider whether we also put `rdfs:label` on the E41 (currently optional).

---

## `easyRDFAddTypes(...)`

```php
function easyRDFAddTypes(
  Graph $g,
  UriFactory $uri,
  UUIDManager $u,
  array $typeArray,
  ?string $parentUri = null,
  array $aatIndex = [],
  ?string $typePred = "crm:P2_has_type",
  ?string $typeClass = "crm:E55_Type"
): array
```

**Purpose**  
Attach one or more **types** to a parent resource, preferring **Getty AAT** concepts when available and minting local `crm:E55_Type` nodes otherwise.

**Input shape**  
`$typeArray` is an associative array: `label => [list of membership tags]`  
Example:
```php
[
  "Inventory Number" => [],
  "Oil paint"        => ["materials"]
]
```

**Resolution rules**

1) **AAT match** (via `$aatIndex[$key]`)
- `key = strtolower(trim(preg_replace('/\s+/', '_', $label)))`
- If found, expect fields: `id` (CURIE `aat:NNN` or full IRI), optional `label`.
- Expand CURIE with `expandCurieOrIri()`, assert:
  - `rdf:type` = `$typeClass` (default `crm:E55_Type`)
  - `rdfs:label`, `crm:P190_has_symbolic_content`, `skos:prefLabel` = resolved label
- Add membership tag `"aat_type"` to `$typeTypes`.

2) **ResearchSpace built-in type IRI**  
If `$label` matches `^https?://www\.researchspace\.org/resource/vocab/[^/]+/[^/]+$`, use it **as-is** (no new node).

3) **Local type**  
Otherwise, mint a new local type with:
- `easyRDFAddThing($g, $uri, $u, "type", [$typeClass], [], $label, ...)`
- Add `skos:prefLabel` = label
- Add membership tag `"local_type"`

**Linking and tagging**

- If `$parentUri` provided, link `parent --$typePred--> typeUri` (resource link).
- For each entry in `$typeTypes`, assert  
  `typeUri crm:P71i_is_listed_in <http://www.researchspace.org/resource/vocab/{typeType}>`.

**Returns**  
`array $typeUris` mapping the (possibly normalised) **label** to the **type URI** used/created.

**Example**

```php
$aatIndex = [
  "oil_paint" => ["id" => "aat:300015050", "label" => "oil paint"],
];

$typeUris = easyRDFAddTypes(
  $g, $uri, $uuid,
  [
    "Oil paint" => ["materials"],
    "Local term only" => []
  ],
  $parentUri = "https://data.example.org/painting/abc",
  $aatIndex
);
```

**Notes / gotchas**

- Normalisation makes `"Oil paint"` and `"oil   paint"` hit the same `aatIndex` key (`oil_paint`).
- If using a full IRI for AAT, ensure the graph has the needed prefixes for CURIE expansion, or pass a full IRI directly in `id`.
- `typePred` can be swapped (e.g., `crm:P137_exemplifies`) if you are encoding a different relationship.
- Membership tagging (`P71i_is_listed_in`) is simple by design; extend with project-specific registries if needed.

---

## Add a new helper: checklist

- Brief purpose (1–2 lines).
- Inputs and expected shape.
- What it asserts (triples) and on which nodes.
- Any deterministic behaviour or normalisation rules.
- Open questions for review.

---

## Change log

| Date       | Function          | Summary of change |
|------------|-------------------|-------------------|
| 2025-10-08 | easyRDFAddThing   | Initial note set. |
| 2025-10-08 | easyRDFAddTypes   | Initial behaviour and examples. |
