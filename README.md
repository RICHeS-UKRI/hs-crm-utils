> [!NOTE]
> This is all being built at the moment so much of the follow is not live yet

# 🧩 hs-crm-utils

**Heritage Science CIDOC CRM Utilities**  
Reusable PHP helpers for creating and managing CIDOC CRM–compliant RDF graphs  
within the **RICHeS / HSDS** heritage science infrastructure.

---

## 🏛️ Overview

The **hs-crm-utils** repository provides a set of **practical, well-documented PHP functions** built around [EasyRdf](https://www.easyrdf.org/) to help researchers and developers generate consistent, interoperable RDF graphs aligned with the **CIDOC CRM** and designed for use in **ResearchSpace** and related environments.

These utilities have been developed as part of the **RICHeS – Heritage Science Data Service (HSDS)** initiative, to support the **FAIR** (Findable, Accessible, Interoperable, Reusable) publication of heritage data and to encourage repeatable, transparent modelling of research outputs.

---

## 🎯 Key goals

- 🧱 Simplify **CIDOC CRM graph construction** for heritage data projects.  
- 🪪 Ensure consistent **UUID generation and URI naming** conventions.  
- 🧩 Support integration with **ResearchSpace**, **Cordra**, and **other HSDS tools**.  
- 📖 Provide accessible, step-by-step documentation for both developers and curators.  
- ⚙️ Encourage **reproducibility and COPE (Create Once, Publish Everywhere)** workflows.  

---

## 🧰 Core functionality

| Function | Purpose |
|-----------|----------|
| `easyRDFAddThing()` | Create a new CIDOC CRM entity (E22, E41, etc.) with a stable label and URI |
| `easyRDFAddTypes()` | Attach one or more AAT or local types to a resource |
| `UUIDManager` | Deterministically mint UUIDs based on label + group |
| `UriFactory` | Construct URIs using base namespace and group conventions |
| *(more coming soon)* | |

Each function is documented with:
- clear parameter tables  
- minimal working examples  
- expected RDF/Turtle output  
- links to conceptual background and recipes

---

## 🚀 Quick start

### 1. Requirements
- PHP ≥ 8.0  
- [EasyRdf](https://www.easyrdf.org/docs/)  
- Composer (optional, for dependency management)  

### 2. Clone the repository
```bash
git clone https://github.com/RICHeS-org/hs-crm-utils.git
cd hs-crm-utils
```

### 3. Example use
```php
require_once 'vendor/autoload.php';
require_once 'src/functions/easyRDFAddThing.php';

$g = new Graph();
$g->setNamespace('crm', 'http://www.cidoc-crm.org/cidoc-crm/');
$g->setNamespace('rdfs', 'http://www.w3.org/2000/01/rdf-schema#');

$uri  = new UriFactory('https://data.example.org');
$uuid = new UUIDManager();

$painting = easyRDFAddThing(
  $g, $uri, $uuid,
  'painting',
  ['crm:E22_Human-Made_Object'],
  [],
  'Portrait of a Lady'
);

echo $g->serialise('turtle');
```

**Output (simplified)**

```turtle
<https://data.example.org/painting/uuid> a crm:E22_Human-Made_Object ;
  rdfs:label "Portrait of a Lady" ;
  crm:P1_is_identified_by <https://data.example.org/painting/uuid/primary_appellation> .

<https://data.example.org/painting/uuid/primary_appellation>
  a crm:E41_Appellation ;
  crm:P190_has_symbolic_content "Portrait of a Lady" .
```

---

## 📘 Documentation

Comprehensive documentation is provided in the [`/docs`](docs/) folder:

| Section | Description |
|----------|-------------|
| [Background](docs/background/) | Non-technical overview of CIDOC CRM and ResearchSpace context |
| [Conventions](docs/conventions/) | UUID, URI, naming, and modelling decisions |
| [Functions](docs/functions/) | PHP function reference and examples |
| [Recipes](docs/recipes/) | Step-by-step guides for common modelling tasks |
| [Examples](docs/examples/) | Full example scripts and sample Turtle output |
| [Troubleshooting](docs/troubleshooting.md) | Common errors and how to resolve them |

A published version will also be available at:  
👉 **https://riches-org.github.io/hs-crm-utils/** *(or ReadTheDocs if linked)*

---

## 🧭 Naming and UUID conventions

- **Deterministic UUIDs:** derived from `label + group` using `UUIDManager`.  
- **URIs:** structured as `{base}/{group}/{uuid}` by default.  
- **Labels:** stored as both `rdfs:label` and `crm:P190_has_symbolic_content` (within an E41 Appellation).  
- **Symbolic content:** handled directly via the `isSymbolicObject` flag.  

See full details in [`docs/conventions/`](docs/conventions/).

---

## 🧪 Testing and reuse

You can safely re-run scripts without creating duplicates if your UUID inputs are deterministic (same label and group).  
New versions of entities or scripts should be documented in `/changelog.md`.

If you adapt or reuse this code, please cite:
> *hs-crm-utils: Heritage Science CIDOC CRM Utilities. RICHeS / Heritage Science Data Service.*

---

## 🌍 Related initiatives

- [RICHeS – Research Infrastructure for Conservation of Heritage Science](https://riches-ukri.org)  
- [HSDS – Heritage Science Data Service](https://heritagesciencedata.uk)  
- [E-RIHS – European Research Infrastructure for Heritage Science](https://www.e-rihs.eu)  
- [CIDOC CRM](https://www.cidoc-crm.org)  
- [ResearchSpace](https://www.researchspace.org)  

---

## ⚖️ Licence

Released under the [MIT Licence](LICENSE).  
You are free to reuse and adapt this code with attribution.

---

## 🧑‍💻 Contributing

Contributions are welcome!  
Please open an issue or pull request if you would like to:
- suggest a new helper function,  
- clarify documentation, or  
- align the code with updated CRM versions (e.g., v7.3.1+).

---

## 📅 Maintainers

**National Gallery, London – Heritage Science Department**  
in collaboration with the **RICHeS HSDS initiative**.

Contact: [heritagescience@nationalgallery.org.uk](mailto:heritagescience@nationalgallery.org.uk)

---

> **hs-crm-utils** helps make heritage science data FAIR, reproducible, and linked — one graph at a time.
