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
