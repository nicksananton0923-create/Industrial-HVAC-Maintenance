# Changelog

All notable changes to this project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html)
with the adaptation that, because this is a documentation framework rather
than a software project, the version reflects template and documentation
changes rather than API changes.

## [Unreleased]

No unreleased changes.

## [1.0.0] — 2025-10-20

### Added — Initial public release

#### Templates (10 CSV schemas)
- `templates/hvac-asset-register.csv` — master asset inventory with capacity, refrigerant charge, location, criticality.
- `templates/ahu-inspection-checklist.csv` — AHU inspection with thermal, airflow, filter, fan, belt, bearing, coil, damper readings.
- `templates/chiller-service-record.csv` — chiller service with pressures, approaches, electrical, oil, leak check.
- `templates/cooling-tower-log.csv` — cooling tower inspection with thermal, fan, water chemistry, biocide dosing.
- `templates/pump-fan-bearing-inspection.csv` — pump and fan inspection with vibration, bearing temperature, electrical.
- `templates/filter-replacement-log.csv` — filter replacement with pre/post differential pressure, batch, supplier.
- `templates/refrigerant-service-log.csv` — refrigerant service with certification, cylinder, vacuum, leak check (F-Gas compliant).
- `templates/fault-corrective-log.csv` — fault log with detection, classification, root cause, corrective action, FDD flag.
- `templates/preventive-maintenance-schedule.csv` — PM schedule with frequency, procedure reference, parts, priority, regulatory reference.
- `templates/operating-readings.csv` — BMS operating readings with point class, range, alarm state.

All templates carry dual metric/imperial unit columns and one anonymized example row marked with `# EXAMPLE — anon`.

#### Documentation (6 Markdown documents)
- `docs/system-overview.md` — plant architecture, equipment class details, asset ID scheme, criticality, BMS topology.
- `docs/inspection-methodology.md` — daily / weekly / monthly / quarterly / annual / multi-year inspection routines with acceptance criteria.
- `docs/preventive-maintenance-planning.md` — PM strategy, FMEA by equipment class, FDD integration, risk-based prioritization, KPIs.
- `docs/recordkeeping-guide.md` — field definitions, unit conventions, validation rules, retention schedule, confidentiality.
- `docs/control-logic-and-wiring.md` — control sequences, Mermaid sequence diagrams, wiring block diagram, signal categories, safety interlocks.
- `docs/safety-and-compliance.md` — ASHRAE 15 / 34 / 62.1 / 90.1 / 185 / 188, EU F-Gas 517/2014 + EN 378, AS/NZS 1677 / 3666 / 5149, refrigerant safety, electrical safety, confined space, compliance evidence.

#### Diagrams (5 PNG schematics)
- `diagrams/refrigerant-cycle.png` — water-cooled centrifugal chiller refrigerant cycle.
- `diagrams/chilled-water-system.png` — CHW/CW loop topology with chiller, pumps, cooling tower, AHU.
- `diagrams/control-wiring-block.png` — BMS DDC to field device wiring topology with AI/AO/DI/DO categories.
- `diagrams/bms-network-architecture.png` — three-tier BMS network (field / supervisor / UI) with protocols.
- `diagrams/pm-schedule-gantt.png` — sample PM schedule Gantt for Q1.

#### Data
- `data/sample-records.csv` — anonymized aggregated sample records across all template types.

#### Supporting files
- `README.md` — project overview, scope, structure, usage, safety notice, license.
- `LICENSE` — MIT.
- `CONTRIBUTING.md` — contribution workflow and review criteria.
- `CHANGELOG.md` — this file.
- `.gitignore` — OS / editor / scratch file exclusions.

### Standards alignment
- ASHRAE 15, 34, 62.1, 90.1, 185, 188 (international).
- EU F-Gas Regulation 517/2014 + EN 378.
- AS/NZS 1677.1/.2, 3666.1/.2/.3, 5149 (Australia / New Zealand).
- US EPA Section 608.
- Pressure equipment directives: PED 2014/68/EU, ASME BPVC Section VIII, AS 1210.

### Refrigerants in scope
- R-134a (HFC, ASHRAE Group A1).
- R-123 (HCFC, ASHRAE Group B1) — global phaseout in progress.
- R-514A (HFO blend, ASHRAE Group B1) — R-123 low-GWP replacement.

### Units
- Dual metric / imperial columns throughout (e.g. `supply_temp_c` and `supply_temp_f`).
- Conversion factors documented in `docs/recordkeeping-guide.md` Section 2.3.

### Known limitations
- Templates have not been tested against any specific CMMS import schema. Sites using Maximo, Niagara, Planon, or other CMMS should test import with one asset before bulk upload.
- OEM operating limits mentioned in docs are illustrative. Sites must verify against the current revision of the relevant OEM manual before authorizing work.
- Regulatory citations reflect the standards as of the document date. Regulations change; sites must confirm currency before relying on the citations for compliance evidence.
- PNG diagrams are illustrative schematics, not scaled engineering drawings. Sites must produce site-specific P&IDs and wiring diagrams as part of commissioning.

### Verification performed at release
- CSV headers internally consistent: `asset_id` is the universal foreign key across all templates.
- Mermaid diagrams in `docs/` rendered successfully in the GitHub Mermaid renderer.
- Cross-references between docs and templates use the correct filenames.
- All example rows are marked with `# EXAMPLE — anon` for clear separation from any future real data.

---

## Versioning Policy

Going forward, the version will increment as follows:

- **MAJOR** (1.0.0 → 2.0.0): breaking template change (column renamed or removed) or major restructuring of the repository.
- **MINOR** (1.0.0 → 1.1.0): non-breaking template change (column added) or new template added.
- **PATCH** (1.0.0 → 1.0.1): documentation update, example update, or typo fix.

Sites adopting the framework should pin to a specific MINOR version and review CHANGELOG entries before upgrading.
