# CE-RISE DPP Record Metadata

[![DOI](https://zenodo.org/badge/DOI/TOBEOBTAINED.svg)](https://doi.org/TOBEOBTAINED) [![Schemas](https://img.shields.io/badge/Schema%20Files-LinkML%2C%20JSON%2C%20SHACL%2C%20OWL-32CD32)](https://ce-rise-models.codeberg.page/dpp-record-metadata/)

Repository for the CE-RISE DPP Record Metadata data model, part of the DPP Metadata Layer in the Digital Product Passport architecture. This model provides a universal metadata envelope that declares which data models and schemas compose a DPP record, regardless of their origin. It supports any combination of CE-RISE models, industry standards, proprietary schemas, or established ontologies, enabling true interoperability. The metadata acts as a "table of contents" that tells consumers which models to use for interpreting each part of the composite DPP record, promoting an open and extensible ecosystem rather than a closed system.


---

## Data Model Structure
This data model provides a universal metadata framework for declaring the composition of any DPP record. It supports heterogeneous data sources by allowing references to any schema or ontology - whether CE-RISE models, GS1 standards, Schema.org types, industry-specific schemas, or proprietary formats. The metadata declares which models are used and in what sequence, enabling parsers to correctly interpret composite records built from diverse sources.

### Key Design Principles
1. **Schema-agnostic**: Supports any data model, ontology, or schema - not limited to CE-RISE
2. **Models-first approach**: The list of applied schemas is the primary and often only required metadata
3. **Composite record support**: Enables combination of heterogeneous data sources in a defined sequence
4. **Version awareness**: Tracks schema versions for proper interpretation
5. **Minimal overhead**: Thin metadata layer that doesn't duplicate what's already in the referenced schemas
6. **Open ecosystem**: Promotes interoperability by accepting any standard or proprietary format

### Core Hierarchy

```
DPPRecordMetadata (root)
├── AppliedSchemas (PRIMARY, multivalued)
│   ├── SchemaReference
│   │   ├── SchemaURL (or identifier)
│   │   ├── SchemaType (CE-RISE, GS1, Schema.org, custom, etc.)
│   │   ├── Version
│   │   ├── Namespace/Prefix
│   │   └── SequenceOrder
│   └── CompositionMethod (sequential, nested, mixed)
│
└── MetadataVersion
    ├── SchemaVersion (version of this metadata schema)
    └── CompatibilityLevel
```

### Workflow Sequence

#### **Step 1: Applied Schemas Declaration**
Primary metadata declaring which schemas/models compose this DPP record:
- **SchemaReference**: Reference to any data model (CE-RISE, GS1, Schema.org, industry-specific, etc.)
- **SchemaType**: Classification to help processors understand the schema origin
- **Version**: Specific version for proper interpretation
- **SequenceOrder**: Order in which schema data appears in the composite record
- **CompositionMethod**: How the schemas are combined (sequential blocks, nested, or mixed)

Examples of supported schemas:
- CE-RISE models (product-profile v1.0, usage-and-maintenance v1.0)
- GS1 EPCIS events
- Schema.org Product types
- Industry standards (IEC 61360, ISO 14040 series)
- Proprietary company schemas
- National/regional standards

### Data Properties

The metadata is intentionally minimal - it declares what schemas are used without duplicating their content. Each referenced schema brings its own validation rules, ontology bindings, and semantic definitions.

#### SQL Identifiers

Every data point in the model includes a `sql_identifier` annotation that serves as a unique, machine-friendly database identifier. These identifiers follow a structured namespace pattern to ensure uniqueness across the entire data model:

**Pattern**: `dpm_[category]_[specific_name]`

**Features:**
- **DPP Metadata Prefix**: All identifiers start with `dpm_` to clearly identify them as DPP Record Metadata
- **Hierarchical Namespacing**: Uses category prefixes (`schema_`, `version_`)
- **Database-Friendly**: Uses underscores and avoids special characters for SQL compatibility
- **Unique Across Model**: No duplicate identifiers within the metadata model
- **Metadata-specific**: Designed to coexist with product data identifiers from other models

**Examples:**
- `dpm_schema_reference_url` - URL or identifier of an applied schema
- `dpm_schema_type` - Type classification (CE-RISE, GS1, custom, etc.)
- `dpm_schema_version` - Version of an applied schema
- `dpm_schema_order` - Sequence position in composite record
- `dpm_metadata_version` - Version of this metadata schema

This identifier system enables the metadata to be stored alongside product data without naming conflicts.

---

## Development Roadmap

| Step | Component | Sub-Components | Criticalities Identified | Solutions Planned | Status | Missing/TODO |
|------|-----------|---------------|-------------------------|-------------------|--------|--------------|
| **1** | **Applied Schemas<br>(Core Metadata)** | • SchemaReference<br>• SchemaType classifier<br>• Version tracking<br>• SequenceOrder<br>• CompositionMethod | • Support any schema type<br>• Handle diverse versioning schemes<br>• Order matters for parsing<br>• Namespace conflicts<br>• Schema resolution | • Generic reference structure<br>• Type taxonomy<br>• Flexible version field<br>• Explicit sequencing<br>• Namespace isolation | **PLANNED** | • Schema registry integration<br>• Auto-discovery<br>• Compatibility checks<br>• Schema negotiation |
| **2** | **Metadata Versioning** | • MetadataSchemaVersion<br>• CompatibilityLevel | • Metadata schema evolution<br>• Backward compatibility<br>• Migration paths | • Semantic versioning<br>• Compatibility declaration<br>• Version detection | **PLANNED** | • Auto-migration<br>• Version negotiation |

### Integration Opportunities

- **W3C DCAT** - Data Catalog Vocabulary for dataset metadata
- **Schema.org Dataset** - For dataset-level metadata structures
- **DCMI Terms** - Dublin Core for basic metadata elements
- **VOID** - Vocabulary of Interlinked Datasets
- **JSON-LD** - For @context declarations when using multiple schemas
- **OpenAPI/AsyncAPI** - For API schema references
- **GS1 Web Vocabulary** - For GS1 schema references
- **ISO 15836** - Dublin Core Metadata Element Set
- **LinkML** - For schema definitions

---

## Publishing

Release artifacts for each version (`schema.json`, `shacl.ttl`, `model.owl`)  
are served directly from this URL:
```
https://ce-rise-models.codeberg.page/dpp-record-metadata/
```


---

## Accessing Previous Releases

If you want to view the files published for version `v1.2.0`, open:

```
https://codeberg.org/CE-RISE-models/dpp-record-metadata/src/tag/pages-v1.2.0/generated/
```

Files available in that directory typically include:

- schema.json
- shacl.ttl
- model.ttl
- index.html


---
<a href="https://europa.eu" target="_blank" rel="noopener noreferrer">
  <img src="https://ce-rise.eu/wp-content/uploads/2023/01/EN-Funded-by-the-EU-PANTONE-e1663585234561-1-1.png" alt="EU emblem" width="200"/>
</a>

Funded by the European Union under Grant Agreement No. 101092281 — CE-RISE.  
Views and opinions expressed are those of the author(s) only and do not necessarily reflect those of the European Union or the granting authority (HADEA).  
Neither the European Union nor the granting authority can be held responsible for them.

© 2025 CE-RISE consortium.  
Licensed under [Creative Commons Attribution–NonCommercial 4.0 International (CC BY-NC 4.0)](https://creativecommons.org/licenses/by-nc/4.0/).  
Attribution: CE-RISE project (Grant Agreement No. 101092281) and the individual authors/partners as indicated.

<a href="https://www.nilu.com" target="_blank" rel="noopener noreferrer">
  <img src="https://nilu.no/wp-content/uploads/2023/12/nilu-logo-seagreen-rgb-300px.png" alt="NILU logo" width="40"/>
</a>

Developed by NILU (Riccardo Boero — ribo@nilu.no) within the CE-RISE project.