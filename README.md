# Industrial HVAC, Chiller, AHU and Cooling System Maintenance Documentation Framework

> A professional, OEM-agnostic, standards-aligned documentation system for organizing HVAC asset information, inspection records, service schedules, operating readings, fault logs, corrective actions, and maintenance history on industrial and commercial sites.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Status: Active](https://img.shields.io/badge/Status-Active-success.svg)](CHANGELOG.md)
[![Standards: ASHRAE / EU F-Gas / AS-NZS](https://img.shields.io/badge/Standards-ASHRAE%20%7C%20EU%20F--Gas%20%7C%20AS%2FNZS-informational.svg)](docs/safety-and-compliance.md)
[![Units: Dual SI / Imperial](https://img.shields.io/badge/Units-Dual%20SI%20%2F%20Imperial-orange.svg)](docs/recordkeeping-guide.md)

---

## 1. Purpose

This repository provides a structured, vendor-neutral framework for documenting the operation, inspection, and maintenance of industrial HVAC and cooling systems. It is designed for building services engineers, maintenance planners, refrigeration mechanics, controls technicians, and facility managers who need a consistent, auditable record system that can survive staff turnover, contractor changes, and equipment replacements.

The framework deliberately separates **templates** (reusable CSV schemas), **documentation** (procedural guidance), and **data** (anonymized sample records) so that each site can adopt the templates without inheriting another site's data, and so that procedures can be revised without touching historical records. Every CSV carries dual metric/imperial columns to support international teams and US-based service contractors working on the same site. Every procedure references recognized international standards (ASHRAE, EU F-Gas, AS/NZS) so that compliance evidence is built into the documentation rather than bolted on.

This is **not** a substitute for OEM manuals, site-specific procedures, or the judgment of qualified personnel. It is a scaffolding that makes good practice easier to follow consistently.

## 2. Scope

The framework covers the following equipment classes. Not every template applies to every site — pick the templates that match installed assets.

| Class | Typical assets | Primary templates |
|------|-----------------|-------------------|
| Vapor-compression chillers | Centrifugal, screw, scroll, reciprocating; air-cooled and water-cooled condensers | `chiller-service-record.csv`, `refrigerant-service-log.csv`, `operating-readings.csv` |
| Air handling units (AHU) | Built-up and packaged AHUs, make-up air units, heat recovery units | `ahu-inspection-checklist.csv`, `filter-replacement-log.csv`, `operating-readings.csv` |
| Cooling towers & fluid coolers | Crossflow, counterflow, closed-circuit | `cooling-tower-log.csv`, `water-treatment-log.csv` |
| Pumps | Centrifugal chilled/hot/condenser water pumps, primary/secondary/variable-speed | `pump-fan-bearing-inspection.csv` |
| Fans | Supply, return, exhaust, relief; belt-driven and direct-driven (EC/plug fans) | `pump-fan-bearing-inspection.csv` |
| Motor & drive systems | ODP/TEFC motors, VFDs, belt drives, couplings | `pump-fan-bearing-inspection.csv`, `fault-corrective-log.csv` |
| Building Management System (BMS) | DDC controllers, sensors, actuators, integration gateways | `operating-readings.csv`, `fault-corrective-log.csv` |

The framework also covers cross-cutting processes: filter cleaning and replacement, refrigerant service (per authorized personnel), fault and corrective action logging, and preventive maintenance scheduling.

## 3. Repository Structure

```
industrial-hvac-maintenance/
├── README.md                              # This file — start here
├── LICENSE                                # MIT
├── CHANGELOG.md                           # Version history
├── CONTRIBUTING.md                        # How to propose additions
├── .gitignore
│
├── templates/                             # Reusable CSV schemas (no site data)
│   ├── hvac-asset-register.csv
│   ├── ahu-inspection-checklist.csv
│   ├── chiller-service-record.csv
│   ├── cooling-tower-log.csv
│   ├── pump-fan-bearing-inspection.csv
│   ├── filter-replacement-log.csv
│   ├── refrigerant-service-log.csv
│   ├── fault-corrective-log.csv
│   ├── preventive-maintenance-schedule.csv
│   └── operating-readings.csv
│
├── docs/                                  # Procedural and reference documentation
│   ├── system-overview.md                 # Plant architecture, components, topology
│   ├── inspection-methodology.md          # Visual / mechanical / electrical / controls routines
│   ├── preventive-maintenance-planning.md # PM intervals, FDD triggers, prioritization
│   ├── recordkeeping-guide.md             # Field definitions, units, data integrity, retention
│   ├── control-logic-and-wiring.md        # Sequence of operation, control logic, wiring blocks
│   └── safety-and-compliance.md           # ASHRAE 15/62.1/90.1/34/185, EU F-Gas, AS/NZS
│
├── data/
│   └── sample-records.csv                  # Anonymized example readings & service events
│
├── diagrams/                              # PNG schematics referenced by docs
│   ├── refrigerant-cycle.png
│   ├── chilled-water-system.png
│   ├── control-wiring-block.png
│   ├── bms-network-architecture.png
│   └── pm-schedule-gantt.png
│
└── assets/
    └── (logos, photos, supplementary images)
```

## 4. How to Use This Repository

### 4.1 For a Single Site

1. **Fork or clone** this repository into a private Git host (GitHub, GitLab, Bitbucket, or a self-hosted Gitea/Forgejo instance). HVAC maintenance data is operationally sensitive — keep the working repo private.
2. **Inventory your assets** using `templates/hvac-asset-register.csv`. Each row is one asset with a unique asset ID. This becomes the foreign key every other template references.
3. **Assign asset IDs** using a consistent scheme. The recommended pattern is `<PLANT>-<TYPE>-<NNN>`, e.g. `PLT-CHL-001` for chiller #1 in plant room "PLT", `PLT-AHU-014` for AHU #14. Document your scheme in `docs/system-overview.md`.
4. **Schedule recurring inspections** based on `templates/preventive-maintenance-schedule.csv`. Each PM task references an asset ID, a frequency, and a procedure.
5. **Use the matching template** for each inspection or service event. Save completed records in a `data/<site>/<year>/` subfolder so the templates stay clean.
6. **Log every fault** in `fault-corrective-log.csv`, even if resolved immediately. A fault log with no entries is suspicious; one with consistent entries is honest.
7. **Review trends monthly** using `operating-readings.csv` and `chiller-service-record.csv`. Drift in approach temperature, compressor amps, or refrigerant charge indicates emerging problems before they become failures.

### 4.2 For Multiple Sites

Maintain one repository per site, or one repository with subfolders per site (`sites/<site-code>/templates/`, `sites/<site-code>/data/`). Keep the root `templates/` and `docs/` directories as the canonical source — site folders inherit and override. Avoid editing templates in site folders; instead, fork the template, rename it (e.g. `chiller-service-record-<site>.csv`), and document the deviation in the site's own README.

### 4.3 For Contractors and Service Providers

Clone the templates, populate with your client's asset data, and deliver completed records as CSV exports alongside any PDF service reports. CSV is preferred because it is diff-friendly, scriptable, and survives vendor lock-in. If a client's CMMS (e.g. Maximo, Tridium Niagara, Planon) supports CSV import, the templates can be uploaded directly with minimal mapping.

## 5. Templates Overview

| Template | Rows | Primary key | Foreign key |
|----------|------|------------|-------------|
| `hvac-asset-register.csv` | 1 per asset | `asset_id` | `location_id`, `system_id` |
| `ahu-inspection-checklist.csv` | 1 per inspection event | `inspection_id` | `asset_id`, `technician_id` |
| `chiller-service-record.csv` | 1 per service event | `service_id` | `asset_id`, `refrigerant_service_id` |
| `cooling-tower-log.csv` | 1 per inspection event | `inspection_id` | `asset_id` |
| `pump-fan-bearing-inspection.csv` | 1 per inspection event | `inspection_id` | `asset_id` |
| `filter-replacement-log.csv` | 1 per replacement event | `replacement_id` | `asset_id` |
| `refrigerant-service-log.csv` | 1 per refrigerant event | `refrigerant_service_id` | `asset_id`, `technician_id` |
| `fault-corrective-log.csv` | 1 per fault | `fault_id` | `asset_id`, `corrective_action_id` |
| `preventive-maintenance-schedule.csv` | 1 per PM task | `pm_task_id` | `asset_id`, `procedure_ref` |
| `operating-readings.csv` | 1 per reading snapshot | `reading_id` | `asset_id`, `bms_point_id` |

Each CSV includes a header row, an example row marked with `# EXAMPLE` in the `notes` field, and dual metric/imperial columns where applicable. See `docs/recordkeeping-guide.md` for full field definitions.

## 6. Standards Alignment

This framework aligns with the following standards. Compliance with each is the responsibility of the site operator; the templates are designed to make evidence collection straightforward.

- **ASHRAE 15** — Safety Standard for Refrigeration Systems
- **ASHRAE 34** — Designation and Safety Classification of Refrigerants
- **ASHRAE 62.1** — Ventilation for Acceptable Indoor Air Quality
- **ASHRAE 90.1** — Energy Standard for Buildings (sets minimum efficiency targets and PM expectations)
- **ASHRAE 185** — Method of Testing Microbiocidal Devices in Air
- **EU F-Gas Regulation 517/2014** — Refrigerant phase-down, leak checks, certification, recovery
- **EN 378** — Refrigerating systems and heat pumps — Safety and environmental requirements
- **AS/NZS 1677** — Refrigerating systems — Refrigerant classification
- **AS/NZS 3666** — Air-handling and water systems of buildings — Microbial control
- **AS/NZS 5149** — Refrigerating systems and heat pumps — Safety and environmental requirements

See `docs/safety-and-compliance.md` for the mapping of each standard to specific templates and procedures.

## 7. Safety Notice (Important — Read Before Use)

> **This repository is documentation, not a procedure.** Nothing in this repository authorizes any person to handle refrigerants, open pressurized systems, energize electrical equipment, or perform any work for which they are not trained, certified, and authorized by the equipment owner and the relevant regulator.

Specific safety rules that override any template or procedure in this repository:

1. **Refrigerant handling is regulated.** Only certified technicians may add, recover, or dispose of refrigerants. Certification requirements differ by jurisdiction (e.g. EPA Section 608 in the US, F-Gas Company Certificate in the EU, ARCtick in Australia, NZ IIR Licensing in New Zealand). Verify the technician's certification before authorizing any refrigerant service.
2. **Electrical work is regulated.** Lockout/tagout (LOTO), arc-flash boundaries, and energized work permits are non-negotiable. Templates here record what was done; they do not authorize how it is done.
3. **Equipment-specific limits prevail.** OEM operating manuals specify acceptable pressure, temperature, vibration, and current ranges. The example values in `data/sample-records.csv` and any limits mentioned in docs are illustrative only. Always defer to OEM data and approved site procedures.
4. **Example data is anonymized and fabricated.** Sample records in `data/sample-records.csv` use synthetic values for template demonstration. They are not benchmark data and must not be used for performance comparison or warranty claims.
5. **Confidentiality.** Asset registers and service logs may reveal facility layout, equipment models, and operational patterns that are commercially or security-sensitive. Keep the working repository private and redact client names before sharing examples publicly.

## 8. Contributing

Contributions are welcome. See `CONTRIBUTING.md` for the workflow. In particular:

- Do not propose equipment-specific operating limits — those belong in OEM manuals.
- Do not propose changes that would break existing template columns. Add columns, do not remove or rename them.
- Cite a standard or reputable source for any new procedure or limit.

## 9. License

Released under the [MIT License](LICENSE). You are free to use, copy, modify, merge, publish, and distribute this framework, including for commercial purposes. The authors make no warranty; see the license for full terms.

## 10. Disclaimer of Performance Claims

This repository does not claim, imply, or guarantee energy savings, efficiency improvements, equipment lifespan extension, or downtime reduction. The framework supports good maintenance practice; outcomes depend on equipment condition, workmanship quality, and operational context. Any performance claims require verified site-specific data measured against a documented baseline.

## 11. Repository Status and Verification

What was verified at the time of commit:

- CSV headers are internally consistent across templates (`asset_id` is the universal foreign key).
- Mermaid diagrams in `docs/control-logic-and-wiring.md` are syntactically valid.
- Cross-references between docs and templates use the correct filenames.

What remains unverified:

- Whether the templates meet any specific site's CMMS import schema — test before bulk upload.
- Whether OEM limits mentioned in docs match the latest revision of any specific OEM manual — verify against the current revision before authorizing work.
- Whether the regulatory citations reflect the latest amendments — regulations change; confirm currency before relying on them for compliance evidence.

---

**Maintained as a community resource. Issues, suggestions, and pull requests are welcome via the standard GitHub workflow.**
