# Function: easyRDFAddThing

> **Purpose:** Create a new CIDOC CRM entity with a human-readable label and (by default) an **E41 Appellation** carrying the label as `crm:P190_has_symbolic_content`.  
> **When to use:** Whenever you need a stable, mintable URI and primary appellation for a resource (e.g., painting, sample, person, type).

---

## Signature

```php
function easyRDFAddThing(
  Graph $g,
  UriFactory $uri,
  UUIDManager $u,
  string $group,
  array $classes,
  array $types,    
  string $label,    
  ?string $nslabel = null,    
  ?string $nsgroup = null,
  ?array $aatIndex = [],
  ?string $isSymbolicObject = null,
  ?string $isPartOfUri = null
): string
```

## Parameters

| Name | Type | Req | Meaning | Notes |
|---|---|:--:|---|---|
| g | Graph | ✓ | EasyRdf graph to mutate | Must include prefixes `crm`, `rdfs` |
| uri | UriFactory | ✓ | Mints base URIs | `mint($group, $uuid)` |
| u | UUIDManager | ✓ | Deterministic UUIDs | `get(nslabel, nsgroup)` |
| group | string | ✓ | Logical group / path segment | e.g., `painting`, `sample` |
| classes | array | ✓ | CRM classes to assert | e.g., `['crm:E22_Human-Made_Object']` |
| types | array |  | Types to attach to the thing | Passed to `easyRDFAddTypes` |
| label | string | ✓ | Human-readable text | Also used to seed UUID if `nslabel` null |
| nslabel | ?string |  | UUID name | Defaults to `label` |
| nsgroup | ?string |  | UUID namespace | Defaults to `group` |
| aatIndex | ?array |  | Lookup for types | For `easyRDFAddTypes` |
| isSymbolicObject | ?string |  | If truthy, write P190 on the thing and **skip E41** | Use sparingly |
| isPartOfUri | ?string |  | Override parent URI | Final URI: `{isPartOfUri}/{group}` |

## Returns
- **string** — the minted entity URI.

---

## Example input

**PHP call**
```php
$g = new Graph();
$g->setNamespace('crm', 'http://www.cidoc-crm.org/cidoc-crm/');
$g->setNamespace('rdfs', 'http://www.w3.org/2000/01/rdf-schema#');
$g->setNamespace('skos', 'http://www.w3.org/2004/02/skos/core#');
$g->setNamespace('aat', 'http://vocab.getty.edu/aat/');

$uri  = new UriFactory('https://data.example.org');
$uuid = new UUIDManager();

$aatIndex = [
  "oil_paint" : {"id":"aat:300015050","label":"oil paint"}
];

$thingUri = easyRDFAddThing(
  $g, $uri, $uuid,
  'painting',
  ['crm:E22_Human-Made_Object'],
  ['Oil paint' => []],   // passed through to easyRDFAddTypes
  'Portrait of a Lady'
);
```

---

## Expected output (RDF/Turtle excerpt)

```turtle
<https://data.example.org/painting/UUID> a crm:E22_Human-Made_Object ;
  rdfs:label "Portrait of a Lady" ;
  crm:P1_is_identified_by <https://data.example.org/painting/UUID/primary_appellation> ;
  crm:P2_has_type <http://vocab.getty.edu/aat/300015050> .

<https://data.example.org/painting/UUID/primary_appellation>
  a crm:E41_Appellation ;
  crm:P2_has_type <http://www.researchspace.org/resource/system/vocab/resource_type/primary_appellation> ;
  crm:P190_has_symbolic_content "Portrait of a Lady" .

<http://vocab.getty.edu/aat/300015050>
  a crm:E55_Type ;
  rdfs:label "oil paint" ;
  crm:P190_has_symbolic_content "oil paint" ;
  skos:prefLabel "oil paint" ;
  crm:P71i_is_listed_in <http://www.researchspace.org/resource/vocab/aat_type> .
```

---

## Visual: how input becomes CRM triples

```mermaid
flowchart TD
  subgraph Inputs
    I1[group="painting"]
    I2[label="Portrait of a Lady"]
    I3[classes: E22]
    I4[types: "Oil paint"]
    I5[UUID from nslabel+nsgroup]
  end

  I1 --> U[URI mint via UriFactory]
  I2 --> U
  I3 --> T[Thing node]
  U --> T

  T ---|"rdf:type"| C1[crm:E22_Human-Made_Object]
  T -->|"rdfs:label"| L1["Portrait of a Lady"]

  T -->|"P1_is_identified_by"| A[E41 Appellation]
  A -->|"P190_has_symbolic_content"| L2["Portrait of a Lady"]
  A -->|"P2_has_type"| AT[RS primary_appellation]

  T -->|"P2_has_type"| TY[E55 Type: oil paint]
  TY -->|"skos:prefLabel"| L3["oil paint"]
  TY -->|"P71i_is_listed_in"| V1[aat_type]
```

---

## Behaviour & edge cases

- **Idempotent URIs** when `(nslabel, nsgroup)` don’t change; if labels are volatile, consider `isPartOfUri`.
- **Symbolic objects:** setting `isSymbolicObject` skips E41 and writes `P190` on the main node — only for truly textual resources.
- Requires prefixes `crm`, `rdfs` (and `skos`, `aat` if you pass types).

---

## Related

- [`easyRDFAddTypes`](./easyRDFAddTypes.md)
- Conventions: [/docs/conventions.md](../conventions.md)

---

## Changelog

| Date       | Change |
|------------|--------|
| 2025-10-08 | Initial function doc created |
