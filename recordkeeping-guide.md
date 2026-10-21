# Recordkeeping Guide

This document defines the fields, units, validation rules, retention, and confidentiality requirements for every template in this repository. It is the procedural reference for data entry and data management. Every technician, planner, and contractor who contributes records to this repository should read this document once and refer back to it whenever a field's meaning is uncertain.

Records are the audit trail of the maintenance strategy. Without complete, accurate, and consistent records, the strategy cannot be evaluated, faults cannot be trended, regulatory evidence cannot be assembled, and the repository degrades over time into an unreliable collection of opinion. The discipline of recordkeeping is therefore not administrative overhead; it is the foundation on which the entire maintenance program rests.

---

## 1. Universal Conventions

### 1.1 Character Encoding and Line Endings

All CSV templates use UTF-8 character encoding without byte order mark (BOM) and Unix-style line endings (LF). The BOM is omitted because it interferes with some CSV import tools; Windows-style line endings (CRLF) are acceptable but create unnecessary diffs in git. The CSV header row is always present and uses lowercase snake_case field names.

### 1.2 Date and Time Format

Dates use ISO 8601 `YYYY-MM-DD` (e.g. `2025-01-14`). Timestamps use ISO 8601 `YYYY-MM-DDThh:mm:ss` with the site's local timezone offset (e.g. `2025-01-14T08:00:00+05:30`). Do not use localized date formats (`14/01/2025`, `Jan 14, 2025`); the ambiguity between DD/MM and MM/DD has caused enough incident in the industry that ISO 8601 is the only safe default.

If the BMS export uses a different format, transform it before importing to the repository. The transformation script should be in the site's `scripts/` folder (not in this template repository, which is vendor-neutral).

### 1.3 Numeric Format

Decimal separator is always `.` (period), never `,` (comma). Thousand separators are not used inside CSV cells — they conflict with the CSV column delimiter. Negative numbers use the leading minus sign (`-12.5`). Zero is recorded as `0` for integers and `0.0` for floats, not as a blank cell.

Blank cells mean "not measured" or "not applicable", never "zero". A pump that has not been vibration-tested has a blank `de_vibration_mm_s` cell, not a `0` cell. A zero is a measured value that happens to be zero; a blank is an absent value. Conflating the two corrupts trend analysis.

### 1.4 Text Fields

Text fields are not quoted in the templates, but values containing commas, double quotes, or newlines must be quoted per RFC 4180. The recommendation is to avoid commas in free-text `notes` fields; use semicolons or hyphens instead. If a comma is unavoidable, the entire field must be wrapped in double quotes.

Text fields use mixed case (sentence case for sentences, title case for asset names). All-uppercase is reserved for asset IDs, equipment class codes, and acronym fields. All-lowercase is reserved for field names and values that are intentionally lowercase (e.g. `yes`, `no`, `active`).

### 1.5 Boolean Fields

Boolean fields use `yes` and `no` (lowercase). The values `true` / `false`, `1` / `0`, `Y` / `N` are not used. If a field is unknown at the time of recording, leave it blank — do not write `unknown` or `TBD`, which complicates filtering.

### 1.6 Identifier Fields

Identifier fields (`asset_id`, `inspection_id`, `service_id`, `fault_id`, `pm_task_id`, `reading_id`, `refrigerant_service_id`) follow the format `<PREFIX>-<YYYY>-<NNN>` where PREFIX indicates the record type (e.g. `AHU`, `CHL`, `FLT`, `PM`), YYYY is the four-digit year, and NNN is a zero-padded sequence within the year. Identifiers are unique across the entire repository and never reused — even if a record is entered in error, the identifier is not reused for the corrected record.

---

## 2. Unit Conventions

### 2.1 Dual Metric / Imperial

The templates use dual metric and imperial columns for every parameter where both systems are in common use. The metric column is named `<parameter>_<unit>` (e.g. `supply_temp_c`); the imperial column is named `<parameter>_<unit_imperial>` (e.g. `supply_temp_f`). Both columns are populated for every reading; do not leave one blank and infer from the other. The two values must be consistent — a 7 °C supply temperature must appear as 44.6 °F, not as 45 °F or 44 °F. If the conversion is uncertain, populate only the measured value and leave the converted value blank with a note in `notes`.

### 2.2 Standard Unit Conventions

The table below lists the standard units used in the templates. Always use the unit listed; do not substitute a similar unit (e.g. use `kPa` not `bar`, `L/s` not `m³/h`, `kg` not `lb` for the metric column).

| Parameter class | Metric unit | Imperial unit | Notes |
|----------------|-------------|---------------|-------|
| Temperature | °C (degC) | °F (degF) | Use `degC` and `degF` in column suffixes |
| Pressure (refrigerant) | kPa | psig | Use `kPa` and `psig` — gauge pressure |
| Pressure (air, water low) | Pa | in w.g. (inwg) | Use `Pa` and `inwg` |
| Pressure (vacuum) | Pa | microns | Vacuum for dehydration |
| Vibration | mm/s rms | in/s rms | Per ISO 10816 |
| Flow (air) | L/s | CFM | Litres per second, cubic feet per minute |
| Flow (water) | L/s | gpm | US gallons per minute |
| Mass | kg | lb | |
| Volume (liquid) | L | gal (US) | |
| Electrical (current) | A | A | Same unit both systems |
| Electrical (voltage) | V | V | Same unit both systems |
| Electrical (power) | kW | kW | Same unit both systems |
| Frequency | Hz | Hz | Same unit both systems |
| Conductivity | µS/cm | µmho/cm | Same numeric value, different name |
| Distance | mm | in | |
| Time | s, min, h | s, min, h | Same units both systems |

### 2.3 Unit Conversion Reference

The repository's docs use the following conversion factors. The technician does not need to convert manually; both columns are populated from the source instrument reading. If the source instrument produces only one unit, the conversion is done by the entry tool (spreadsheet formula, BMS export script).

| From | To | Factor |
|------|-----|--------|
| °C | °F | `°C × 1.8 + 32` |
| kPa | psig | `kPa × 0.145038` (gauge, so subtract atmospheric if starting from absolute) |
| Pa | in w.g. | `Pa × 0.00401463` |
| L/s | CFM | `L/s × 2.11888` |
| L/s | gpm | `L/s × 15.8503` |
| kg | lb | `kg × 2.20462` |
| L | gal (US) | `L × 0.264172` |
| mm/s rms | in/s rms | `mm/s × 0.0393701` |
| µS/cm | µmho/cm | `1:1` (same value) |

---

## 3. Field Definitions by Template

### 3.1 hvac-asset-register.csv

The asset register is the master inventory. Every row is one asset, identified by `asset_id`. Other templates reference this `asset_id`. The asset register is updated whenever an asset is added, decommissioned, or has a significant change (replacement of motor, replacement of controller, conversion of refrigerant).

| Field | Type | Description | Validation |
|-------|------|-------------|------------|
| `asset_id` | text | Unique asset identifier per the scheme in `system-overview.md` Section 2.1 | Regex `^[A-Z0-9-]+$`; unique |
| `asset_name` | text | Human-readable name | Non-empty |
| `asset_type` | text | Equipment class code | From controlled vocabulary |
| `manufacturer` | text | OEM name | Non-empty |
| `model` | text | OEM model number | Non-empty |
| `serial_number` | text | OEM serial number | Non-empty |
| `year_built` | integer | Year of manufacture | 4-digit year |
| `capacity_kw` | decimal | Design capacity in kW | ≥ 0 |
| `capacity_ton` | decimal | Design capacity in refrigeration tons | ≥ 0 |
| `location_id` | text | Location code | From controlled vocabulary |
| `system_id` | text | System code (e.g. `CHW-PRIMARY`) | From controlled vocabulary |
| `refrigerant` | text | Refrigerant ASHRAE designation (R-134a, R-514A, R-123, N/A) | From controlled vocabulary |
| `charge_kg` | decimal | Factory charge in kg | ≥ 0; 0 if N/A |
| `charge_lb` | decimal | Factory charge in lb | ≥ 0; 0 if N/A |
| `voltage` | integer | Design voltage | > 0 |
| `phase` | integer | Number of phases (1 or 3) | 1 or 3 |
| `status` | text | `active`, `standby`, `decommissioned` | From controlled vocabulary |
| `install_date` | date | Date of installation | YYYY-MM-DD |
| `warranty_expiry` | date | OEM warranty expiry | YYYY-MM-DD |
| `criticality` | text | `critical`, `high`, `medium`, `low` | From controlled vocabulary |
| `notes` | text | Free text | Use `# EXAMPLE — anon` for example rows |

### 3.2 ahu-inspection-checklist.csv

One row per inspection event. Each inspection event creates one row; multiple inspections of the same AHU on the same day create multiple rows (with sequential `inspection_id` suffixes). The inspection captures a snapshot of the AHU's thermal, mechanical, and electrical state.

Fields are grouped: identification (inspection_id, date, asset_id, technician_id), environmental conditions (outdoor/return/supply temps), airflow (supply_airflow, outdoor_air_pct), filter (dp), fan electrical (amps, vfd_hz), fan mechanical (belt, bearings, vibration), and components (drain pan, coils, dampers, filters).

### 3.3 chiller-service-record.csv

One row per service event. A service event is any maintenance activity beyond a daily operator round — monthly inspection, quarterly inspection, annual service, fault repair, refrigerant top-up. The record captures the operating state at the time of service, the service performed, and the parts used.

### 3.4 cooling-tower-log.csv

One row per inspection event. The cooling tower log is also the water treatment record — water chemistry readings, biocide dosing, and descale events are all recorded here. The `notes` field distinguishes routine inspection from water treatment event.

### 3.5 pump-fan-bearing-inspection.csv

One row per inspection event on a pump, fan, or bearing-driven equipment. The `equipment_class` field allows the same template to be used for pumps, fans, blowers, and other bearing-driven equipment, with belt-related fields left blank for direct-driven equipment.

### 3.6 filter-replacement-log.csv

One row per filter replacement event. Records the pre and post differential pressure (to confirm the replacement restored flow), the filter specification (type, MERV, size, quantity), and the batch and supplier (for warranty and procurement traceability).

### 3.7 refrigerant-service-log.csv

One row per refrigerant service event — add, recover, recover-and-recharge, leak check, system pressure test. The technician's certification ID is mandatory; the certification must be valid in the applicable jurisdiction. The cylinder IDs allow traceability from cylinder to system to disposal record. The vacuum level (in Pa and microns) confirms system dehydration before recharge. The leak check method and result are required by EU F-Gas 517/2014 and equivalent regulations.

### 3.8 fault-corrective-log.csv

One row per fault. The fault record captures the full lifecycle: detection (date, detected by, symptom), classification (category, severity), investigation (root cause), response (immediate action, corrective action), and outcome (date corrected, downtime hours, recurrence count, FDD flag, BMS alarm ID, regulatory impact). The record is updated in place as the fault progresses through its lifecycle — the initial entry captures detection and immediate action; subsequent edits add root cause, corrective action, and outcome.

### 3.9 preventive-maintenance-schedule.csv

One row per PM task. The schedule is the operational expression of the maintenance strategy. The `last_completed_date` and `next_due_date` fields are updated when a task is completed; the scheduling system (BMS, CMMS, or spreadsheet) uses these to compute overdue status.

### 3.10 operating-readings.csv

One row per reading at one timestamp for one BMS point. The template is designed to receive BMS trend exports with minimal transformation. The `point_class` field supports filtering by reading type; the `out_of_range_flag` and `alarm_state` fields capture the BMS's interpretation of the reading.

---

## 4. Data Integrity and Validation

### 4.1 Referential Integrity

Every record (except asset register entries) references an `asset_id` from the asset register. A record with an `asset_id` that does not exist in the asset register is invalid and must be corrected. The validation is performed by the entry tool (spreadsheet formula, CMMS import check, custom script). A simple check is `=COUNTIF(asset_register_column, asset_id_cell) > 0`; if zero, the entry is flagged.

### 4.2 Range Validation

Each numeric field has an expected range. Values outside the range trigger a review (the value may be correct, but it must be confirmed). The recommended ranges are derived from physical limits and OEM specifications; the site should override with site-specific ranges where appropriate. Examples:

| Field | Reasonable range | Out-of-range action |
|-------|-------------------|---------------------|
| `supply_temp_c` (CHW) | -5 to 30 | Confirm with second instrument |
| `suction_pressure_kpa` | 100 to 1000 | Confirm gauge calibration |
| `bearing_temp_c` | 0 to 100 | Inspect immediately |
| `vibration_mm_s` | 0 to 10 | Inspect immediately |
| `compressor_amps_pctfla` | 0 to 110 | Investigate overloading |

### 4.3 Consistency Validation

Several fields are mutually consistent. For example, `compressor_amps_pctfla` should equal `compressor_amps_a / FLA × 100`. If the two do not agree, one of them is wrong. The entry tool should validate consistency and flag discrepancies.

Dual metric/imperial columns are also mutually consistent — the imperial value should equal the metric value multiplied by the conversion factor (rounded to the appropriate precision). The entry tool should compute one from the other to avoid transcription errors.

### 4.4 BMS Data Validation

BMS data integrity is often weaker than it appears. Before relying on a BMS reading for trend analysis or regulatory evidence, validate:

1. **Sensor calibration.** Compare the BMS reading to a calibrated reference instrument at the same point. Drift > 1 °C for temperature, > 2% for pressure, > 5% for flow warrants recalibration.
2. **Stale data.** Confirm the BMS trend is updating. A trend that has not changed in 1 hour while the equipment is running is suspect — the sensor may be failed, the communication may be down, or the point may be overridden.
3. **Unit consistency.** Confirm the BMS point's unit matches the unit in the trend export. BMS configurations sometimes have the unit set incorrectly (e.g. psig configured as kPa), producing readings that are off by the conversion factor.
4. **Range consistency.** Confirm the reading is within the physical range of the sensor. A reading of -50 °C from a 0–50 °C sensor indicates a sensor failure, not an actual condition.

The validation is documented in `recordkeeping-guide.md` (this section). The validation result is recorded in the `notes` field of the operating-readings entry as `validated` or `validated-suspect — <reason>`.

---

## 5. Retention and Disposal

### 5.1 Retention Schedule

| Record type | Minimum retention | Driver |
|-------------|-------------------|--------|
| Asset register | Life of asset + 7 years | Audit |
| AHU inspection | 7 years | Insurance / audit |
| Chiller service record | 10 years | Pressure equipment / OEM warranty |
| Refrigerant service log | 5 years (EU F-Gas) / 5 years (US EPA) | Regulatory |
| Fault log | 7 years | Insurance / regulatory |
| PM schedule (historical) | 7 years | Audit |
| Operating readings (BMS trend) | 2 years at full resolution; 7 years aggregated | Trend analysis / audit |
| Cooling tower water test | 5 years | ASHRAE 188 / AS-NZS 3666 |
| Pressure vessel inspection certificate | Life of vessel | Pressure equipment regulation |

Retention starts from the date of the record, not the date of the equipment. Records for decommissioned equipment are retained for the period above from the decommissioning date.

### 5.2 Disposal

Records that have passed their retention period may be deleted from the working repository. The deletion is committed with a commit message documenting the disposal (record type, count, date range). The git history retains the deleted records, so the audit trail is preserved; the working repository is kept clean.

Records are never deleted before their retention period, even for storage reasons. If the working repository grows large, archive older records to a separate `archive/` folder with a README documenting the archive scope; do not delete.

### 5.3 Backup

The repository is backed up daily. The backup is retained for at least 30 days (rolling) plus monthly snapshots retained for at least 12 months. The backup is tested quarterly by restoring to a test environment and verifying integrity. Backup integrity testing is the most neglected maintenance activity in IT-adjacent systems; schedule it and execute it.

---

## 6. Confidentiality and Access Control

### 6.1 Sensitivity Classification

HVAC maintenance records are not usually classified as highly sensitive, but they reveal information that warrants access control:

- **Asset register** reveals facility layout, equipment models, and capacity. Useful for competitive intelligence and for physical security planning.
- **Operating readings** reveal operational patterns (operating hours, load profiles). Useful for competitive intelligence.
- **Fault logs** reveal failure modes and operational disruptions. Useful for reputational damage and for adversarial targeting.
- **Refrigerant service logs** reveal refrigerant quantities. Useful for environmental activism.
- **Personnel information** (technician names, certification IDs) is personal data subject to GDPR / equivalent.

### 6.2 Access Control

The repository is private. Access is granted on a need-to-know basis:

- **Maintenance manager:** full read/write.
- **Maintenance planners:** read on asset register, PM schedule; write on PM schedule.
- **Technicians:** write on inspection, service, fault, refrigerant logs; read on asset register and PM schedule.
- **Contractors:** temporary write on the templates relevant to their scope; read on asset register; no access to other sites' data in a multi-site repository.
- **Auditors:** read on all templates for the period under audit.

Access is reviewed quarterly. Departures trigger immediate access removal.

### 6.3 Personal Data

Technician names and certification IDs are personal data. They are retained for the operational purpose of recordkeeping (who did what). Under GDPR / equivalent, the data subject has rights of access, rectification, and erasure. Erasure is balanced against the regulatory retention requirement — refrigerant service logs cannot be erased within their retention period even if the technician departs.

For examples shared publicly (in this template repository's `data/sample-records.csv`), technician IDs are anonymized to `T-NNNN` format and do not correspond to real personnel.

### 6.4 Anonymization for Public Sharing

Before sharing any record outside the organization, anonymize:

- Replace site name with `<site>` or a generic code.
- Replace technician names with `T-NNNN`.
- Replace equipment serial numbers with `SN-<TYPE>-NNN-A`.
- Replace specific addresses with the city and country only.
- Remove any client-identifiable information from `notes`.

The `data/sample-records.csv` file in this template repository has been anonymized; all values are synthetic and do not correspond to any real site.

---

## 7. Versioning and Change Management

### 7.1 Template Versioning

Templates are versioned by their filename and by a `template_version` field that may be added to each row if needed. When a template column is added, the change is backward-compatible (existing rows are valid in the new template; the new column is blank for existing rows). When a template column is renamed or removed, the change is breaking and requires a migration of all existing rows.

Breaking changes are documented in `CHANGELOG.md` with the date, the affected template, the old and new column names, and the migration procedure. Non-breaking changes are documented in `CHANGELOG.md` with the date and the added column.

### 7.2 Repository Versioning

The repository uses semantic versioning:

- MAJOR version: breaking template change (column renamed or removed).
- MINOR version: non-breaking template change (column added) or new template.
- PATCH version: documentation update or example update.

The current version is recorded in `CHANGELOG.md`. The version is incremented on every merge to main.

### 7.3 Migration Procedure

When a breaking change is made, the migration procedure is:

1. Create the new template file (e.g. `chiller-service-record-v2.csv`).
2. Write a migration script that reads the old template and writes the new template, transforming column names and values as required.
3. Run the migration script on the existing data, producing the new-format records.
4. Verify the migration: row count matches, every record's `asset_id` and `service_id` are preserved, every value in a renamed column is preserved.
5. Replace the old template with the new template (rename `chiller-service-record-v2.csv` to `chiller-service-record.csv`).
6. Commit with a message documenting the migration (date, old/new column names, row count migrated).

The migration script is preserved in `scripts/` so that the migration is reproducible.

---

## 8. Example Records

The `data/sample-records.csv` file contains anonymized example records for each template, marked with `# EXAMPLE — anon` in the `notes` field. These records are intended to demonstrate the template structure and field values; they are not benchmark data and must not be used for performance comparison or warranty claims. The values are illustrative; do not infer that any specific value is typical or recommended.

To add a real record: copy the relevant template, populate the fields with measured values, save the file in the appropriate `data/<site>/<year>/` folder, and commit. The first few real records will reveal any template weaknesses; submit a pull request to refine the template if a field is missing or a unit is incorrect.

---

## 9. Audit Trail

The repository's git history is the primary audit trail. Every commit records who made the change, when, and what changed. Commits are signed by the author's GPG key (where configured) for non-repudiation. Force-push is disabled on the main branch to preserve history integrity.

For regulatory audits (EU F-Gas, ASHRAE 188, pressure equipment), the audit trail consists of:

1. The records themselves (in `data/`).
2. The git history (showing when each record was added and by whom).
3. The CHANGELOG (showing template evolution).
4. The site's documented procedures (in `docs/`).
5. The OEM manuals referenced in the procedures (held in the maintenance office).

The audit trail is only complete if all five elements are present. A common audit failure is records being maintained without corresponding procedures, or procedures being maintained without corresponding records — the audit trail must show both that work was done and that the work followed an approved procedure.
