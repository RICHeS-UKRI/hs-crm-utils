# Common parameters

These inputs recur across most functions. We document them once here and cross‑link from individual function pages to keep those pages short and focused.

---

## Shared inputs

| Name | Type | Meaning | Notes |
|---|---|---|---|
| `g` | `Graph` | The EasyRdf graph to mutate | Register `crm`, `rdfs`, `skos`, and any project prefixes up front. |
| `uri` | `UriFactory` | Mints URIs for new nodes | Conventionally `mint($group, $uuid)` → `{base}/{group}/{uuid}`. |
| `u` | `UUIDManager` | Generates deterministic UUIDs | Usually name‑based UUID from `(nslabel, nsgroup)`. |
| `aatIndex` | `array` | Normalised lookup for AAT types | Keys: lowercased, spaces→underscores. Values: `{id, label}` (CURIE or IRI). |

---

## Recommended namespace setup (copy‑paste)

```php
$g = new Graph();
$g->setNamespace('crm',  'http://www.cidoc-crm.org/cidoc-crm/');
$g->setNamespace('rdfs', 'http://www.w3.org/2000/01/rdf-schema#');
$g->setNamespace('skos', 'http://www.w3.org/2004/02/skos/core#');
$g->setNamespace('aat',  'http://vocab.getty.edu/aat/');
```
