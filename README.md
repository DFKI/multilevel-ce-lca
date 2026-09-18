# Multi-Level CE LCA

Supplementary material for the paper **"Harnessing Digital Product Passports and Multi-Level Circular Economy Information Modelling for Life Cycle Assessment"** — the detailed Life Cycle Assessment (LCA) results behind the paper's use case, and an Asset Administration Shell (AAS) example of how Circular Economy (CE) information can be modelled and linked across levels.

## Background

A successful Circular Economy implementation requires data at multiple levels: product-specific data at the **nano** level, company-specific data at the **micro** level, inter-company data at the **meso** level, and country-specific data at the **macro** level. At the nano level, Digital Product Passports (DPPs) are the most prominent information source. In practice, however, data at these levels is treated as distinct and serving different purposes — nano-level data informs consumers or recyclers about a product's properties, while meso-level data supports corporate sustainability reporting — and systematic integration across levels is lacking.

This work proposes an approach that links nano-level to micro-level data and demonstrates how the two can be integrated. The approach is implemented and validated in a real use case at the [SmartFactory-KL](https://smartfactory.de) model factory by performing an LCA of a toy truck based on its DPP (nano level) and, subsequently, of the model factory itself (micro level).

- **Functional unit:** one production order run yielding a single toy truck.
- **Life cycle inventory:** built from allocated mass and energy balances.
- **Impact assessment:** European Commission Environmental Footprint 3.1 (PEF-compliant) methodology.

The methodology is a starting point for integrating the meso and macro levels as a next step and, ultimately, for enabling systematic multi-level information modelling in the CE.

## LCA Overview

[`LCA_Overview.xlsx`](LCA_Overview.xlsx) holds the detailed LCA results for the toy truck and the SmartFactory-KL model factory, from the modelled process chain down to the characterised impact scores. Each sheet covers one step of the assessment:

| Sheet | Content |
| --- | --- |
| **Flow Diagram** | Process flow of the toy truck production chain at SmartFactory-KL. |
| **Life Cycle Inventory** | Inputs and outputs (mass in kg, electricity in MJ) for the nine modelled processes — additive manufacturing of the cabin and semitrailer, transport of ordered parts, four sub-assemblies, and final assembly. |
| **Nano and Micro GWP results** | Bridge between the two levels: per-product GWP at the nano level scaled to the annual production programme (60 trucks/week, 3120/year) and combined with the factory's non-attributable lighting and heating energy to obtain the micro-level total. |
| **Raw Results** | Full characterised impact results per Environmental Footprint category, broken down by contributing process. |
| **Normalised Results** | The same categories after normalisation, for cross-category comparison and hotspot identification. |
| **Sensitivity Analysis** | ±10% perturbation of each inventory item against the baseline, with the resulting Δ% on the GWP score. |

## AAS Model

[`aas/multilevel-ce-lca.json`](aas/multilevel-ce-lca.json) is an Asset Administration Shell example that models the information needed for multi-level CE LCA. It contains two AAS instances and eight submodels:

**Nano level — `Nano_SemitrailerBodyWhite`** (one identifiable product instance)

- `Nameplate` — IDTA 02006-3-0 Digital Nameplate (manufacturer, product designation, serial number, date and country of manufacture).
- `Nano_ProductionInventory` — per-product process energy, material inputs, circularity inputs (recycled/renewable content, collection rate, lifetime, disassembly time, repairability, take-back, preferred end-of-life route) and environmental factors.
- `Nano_CircularityAndLCA` — calculated per-product results: GWP under EF 3.1, circular input/output rates, circular material score, lifetime factor, and material mass split.

**Micro level — `Micro_SmartFactoryKL`** (the factory over an annual reporting period)

- `FactoryAutomationDataForPlant` — IDTA 02075 factory automation planning data (AutomationML payload intentionally unset until a factory-specific AML file is supplied).
- `Micro_ReportingAndEnergy` — reporting period, annual production programme, allocation key, and non-attributable site energy (lighting, heating, renewable share).
- `Micro_AggregatedInventory` — annual energy and material inventory derived from the nano product data plus factory-only flows, without double counting.
- `Micro_CircularityAndLCA` — factory-level GWP and CE indicators, plus a `DataProvenance` collection recording the source nano AAS, the aggregation rule, the allocation rule, and the last synchronisation.

**Linkage — `MultiLevel_Linkage`**

- The explicit join between the two levels: the shared product serial and join cardinality, `ReferenceElement`s to both AAS and to the energy source/target properties, the shared semantic IDs for energy, mass and GWP, and the aggregation direction (nano → micro) and allocation direction (micro → nano).

The AAS was hosted in [Eclipse BaSyx](https://www.eclipse.org/basyx/), where changing a nano-level value propagates to the corresponding micro-level values — data interoperability applied to the CE world. The file can be loaded into any AAS-compliant infrastructure, e.g. a BaSyx AAS environment or the [AASX Package Explorer](https://github.com/admin-shell-io/aasx-package-explorer).

> [!NOTE!]
> The real AAS modelling contains some in-progress IDTA submodels which, due to data privacy constraints, cannot be published at this time. The file in this repository is therefore an **example** that is close to the actual modelling. Once the official submodel publication becomes available, the complete AAS modelling of data points will be made accessible here.

## Citation

If you use this material, please cite the paper.

> *Harnessing Digital Product Passports and Multi-Level Circular Economy Information Modelling for Life Cycle Assessment.*

(Full bibliographic details will be added once the paper is published.)

## License

Released under the [GNU General Public License v3.0](LICENSE).
