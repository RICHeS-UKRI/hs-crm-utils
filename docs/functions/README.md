# Function Index

A clean list of all EasyRdf helpers in **hs-crm-utils**, grouped by purpose.  
Each function page is **stand‑alone**, but to avoid repetition they reference shared **[Common parameters](common-parameters.md)**.

> Tip: Every function takes the same core inputs (`Graph $g`, `UriFactory $uri`, `UUIDManager $u`, and usually an `aatIndex`).  
> We document these once in *Common parameters* and keep each function page focused on what’s unique.

---

## Categories

- **Creation** – make new CRM nodes with labels/appellations.
- **Typing** – assert `crm:P2_has_type` or other typing relations.
- **Linking** – connect existing resources with CRM properties.
- **Utility** – normalisation, CURIE expansion, lookups.

---

## Functions

| Function | Category | What it does | Doc |
|---|---|---|---|
| `easyRDFAddThing` | Creation | Mint a URI for a resource, add `rdfs:label`, create **E41 Appellation**, and (optionally) attach types. | [`easyRDFAddThing.md`](easyRDFAddThing.md) |
| `easyRDFAddTypes` | Typing | Resolve **AAT** or construct **local E55** types, assert required triples, then link to a parent via `crm:P2_has_type`. | [`easyRDFAddTypes.md`](easyRDFAddTypes.md) |

> Add new rows as you add functions. Keep descriptions short (one sentence).

---

## How to write a new function page

1. Copy `_template.md` and rename it to match your function.  
2. Fill in the **Purpose / When to use**, **Parameters**, **Example**, **Expected Turtle**, and **Mermaid** diagram.  
3. Include a small note that it uses the **Common parameters**.  
4. Optionally, add a *Behaviour & edge cases* list.

