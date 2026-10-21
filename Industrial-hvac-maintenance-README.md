<!-- ================================================================== -->
<!-- Industrial-hvac-maintenance · README.md                                -->
<!-- Professional HVAC maintenance documentation framework                  -->
<!-- ================================================================== -->

<div align="center">

# 🏭 Industrial HVAC Maintenance

**Professional HVAC, Chiller, AHU &amp; Cooling System maintenance documentation framework**

<p>
  <a href="https://github.com/nicksananton0923-create/Industrial-hvac-maintenance/stargazers"><img src="https://img.shields.io/github/stars/nicksananton0923-create/Industrial-hvac-maintenance?style=flat-square&color=F5B800" alt="Stars"/></a>
  <a href="https://github.com/nicksananton0923-create/Industrial-hvac-maintenance/network/members"><img src="https://img.shields.io/github/forks/nicksananton0923-create/Industrial-hvac-maintenance?style=flat-square&color=0A4D8C" alt="Forks"/></a>
  <img src="https://img.shields.io/github/last-commit/nicksananton0923-create/Industrial-hvac-maintenance?style=flat-square&color=F5B800" alt="Last commit"/>
  <img src="https://img.shields.io/badge/Format-CSV%20%2F%20Markdown-0A4D8C?style=flat-square" alt="Format"/>
  <img src="https://img.shields.io/badge/Records-12%2B%20templates-F5B800?style=flat-square" alt="Records"/>
  <img src="https://img.shields.io/badge/Domain-HVAC%20%2F%20Building%20Services-0A4D8C?style=flat-square" alt="Domain"/>
</p>

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Background &amp; Motivation](#-background--motivation)
- [What's Inside](#-whats-inside)
- [Repository Structure](#-repository-structure)
- [Templates (CSV)](#-templates-csv)
- [Documentation (Markdown)](#-documentation-markdown)
- [Diagrams (PNG)](#-diagrams-png)
- [Getting Started](#-getting-started)
- [How to Use the Templates](#-how-to-use-the-templates)
- [Safety &amp; Accuracy](#-safety--accuracy)
- [Roadmap](#-roadmap)
- [License](#-license)

---

## 📝 Overview

The **Industrial HVAC Maintenance** repository is a complete documentation framework for organizing and managing maintenance work on industrial heating, ventilation, air-conditioning, and cooling systems. It contains reusable CSV templates for every common record a building-services team needs — asset registers, chiller service records, AHU inspection checklists, cooling-tower logs, fault and corrective action logs, preventive maintenance schedules, and more — alongside Markdown documentation that explains the purpose of each field, the methodology behind each inspection, and how the templates fit into a coherent maintenance program.

This is not a software project in the traditional sense; it is a structured set of files that an HVAC maintenance team, a building services manager, or a student of facility engineering can drop into a shared folder and start using immediately. The templates are deliberately vendor-neutral and use plain column names so they open cleanly in Excel, Google Sheets, LibreOffice Calc, or any CSV-aware tool. Every record includes clear field descriptions, sensible defaults, and explicit notes on which fields are mandatory versus optional, which makes them easy to onboard for new technicians without long training sessions.

The repository is being built incrementally — every file that exists is already production-useful, and every file that is still on the roadmap is explicitly called out in the *Roadmap* section. Nothing in this repository should be considered final until it has been reviewed by a qualified maintenance engineer for the specific equipment and jurisdiction in which it will be used.

---

## 🔭 Background &amp; Motivation

Industrial HVAC systems — chillers, air handling units, cooling towers, pumps, fans, and the control networks that tie them together — represent some of the most expensive and most failure-prone equipment in any modern building. A single chiller failure can take an entire process plant offline; a single poorly documented AHU inspection can mean a filter is replaced a month late, an indoor-air-quality complaint is dismissed, or a bearing fails without warning. The cost of these events is almost always higher than the cost of the documentation that would have prevented them.

And yet, in many facilities, maintenance records are scattered: some live on paper, some live in spreadsheets, some live in the heads of senior technicians, and some are not recorded at all. The result is that when a technician leaves, knowledge walks out the door; when an audit arrives, no one can prove what was inspected and when; and when a fault recurs, no one can trace whether the same fault has been seen before. This repository was built to provide a simple, reusable starting point for fixing that gap. It does not replace a full CMMS, but it gives a team a structured, vendor-neutral place to start collecting records in a consistent format — which is the first step toward any future digital system.

---

## 📦 What's Inside

This repository currently includes:

- **12 CSV templates** covering the full maintenance record lifecycle from asset registration through inspection, service, fault logging, and preventive maintenance scheduling.
- **6 Markdown documentation files** covering system overviews, inspection methodology, preventive maintenance planning, recordkeeping practice, control logic and wiring, and safety and compliance.
- **5 PNG diagrams** including the refrigerant cycle, chilled-water system, BMS network architecture, control wiring block, and a Gantt-style preventive maintenance schedule.
- **1 anonymized sample records file** so readers can see what a completed dataset looks like without exposing any real facility data.
- **Standard project files** — `CHANGELOG.md`, `CONTRIBUTING.md`, `LICENSE`, `.gitignore` — to make the repository straightforward to contribute to and fork.

---

## 📂 Repository Structure

```
Industrial-hvac-maintenance/
├── README.md                         # This file
├── CHANGELOG.md                      # Version history
├── CONTRIBUTING.md                   # How to contribute
├── LICENSE                           # License file
├── .gitignore
│
├── # === CSV templates ===
├── hvac-asset-register.csv
├── ahu-inspection-checklist.csv
├── chiller-service-record.csv
├── cooling-tower-log.csv
├── filter-replacement-log.csv
├── refrigerant-service-log.csv
├── pump-fan-bearing-inspection.csv
├── operating-readings.csv
├── fault-corrective-log.csv
├── preventive-maintenance-schedule.csv
├── sample-records.csv                # Anonymized sample data
│
├── # === Markdown documentation ===
├── system-overview.md
├── inspection-methodology.md
├── preventive-maintenance-planning.md
├── recordkeeping-guide.md
├── control-logic-and-wiring.md
├── safety-and-compliance.md
│
└── # === Diagrams (PNG) ===
    ├── refrigerant-cycle.png
    ├── chilled-water-system.png
    ├── bms-network-architecture.png
    ├── control-wiring-block.png
    └── pm-schedule-gantt.png
```

---

## 📊 Templates (CSV)

| Template | Purpose |
|---|---|
| `hvac-asset-register.csv` | Master list of every HVAC asset: ID, location, type, model, serial, install date, OEM, service contact. |
| `ahu-inspection-checklist.csv` | Air handling unit inspection: filter, coil, belt, fan, drain pan, dampers. |
| `chiller-service-record.csv` | Chiller service log: refrigerant, oil, electrical, controls, performance. |
| `cooling-tower-log.csv` | Cooling tower inspection: water treatment, fill, drift eliminator, fan. |
| `filter-replacement-log.csv` | Filter cleaning / replacement history: asset, type, pressure drop, date, technician. |
| `refrigerant-service-log.csv` | Refrigerant handling: type, quantity added / removed, leak check, technician certification. |
| `pump-fan-bearing-inspection.csv` | Bearing inspection: vibration, temperature, lubrication, sound. |
| `operating-readings.csv` | Routine operating readings: temperature, pressure, humidity, flow, current. |
| `fault-corrective-log.csv` | Fault and corrective action log: symptom, cause, action, parts, downtime. |
| `preventive-maintenance-schedule.csv` | PM schedule: asset, task, frequency, last done, next due, responsible. |
| `sample-records.csv` | Anonymized sample rows showing how the templates are used in practice. |

> Each CSV uses simple, plain-English column headers and includes a header row that any spreadsheet tool can read directly. Field meanings and recommended conventions are documented in `recordkeeping-guide.md`.

---

## 📄 Documentation (Markdown)

| Document | Purpose |
|---|---|
| `system-overview.md` | High-level overview of an industrial HVAC system and its components. |
| `inspection-methodology.md` | How to inspect each component type — what to look at, in what order, and what to record. |
| `preventive-maintenance-planning.md` | How to build a PM schedule from the templates, including frequency selection and prioritization. |
| `recordkeeping-guide.md` | The reference for every column in every CSV — mandatory vs. optional, units, and example values. |
| `control-logic-and-wiring.md` | Notes on BMS / control logic and wiring conventions, including the diagrams below. |
| `safety-and-compliance.md` | Safety reminders, refrigerant handling rules, and confidentiality guidance. |

---

## 🖼 Diagrams (PNG)

| Diagram | What it shows |
|---|---|
| `refrigerant-cycle.png` | Standard vapor-compression refrigerant cycle with labeled components. |
| `chilled-water-system.png` | Chilled water loop: chiller, pumps, AHU coils, expansion tank. |
| `bms-network-architecture.png` | BMS / BACnet network architecture with controllers and field devices. |
| `control-wiring-block.png` | Block diagram of typical HVAC control wiring conventions. |
| `pm-schedule-gantt.png` | Gantt-style representation of a typical annual PM schedule. |

---

## 🚀 Getting Started

### As a maintenance team

1. **Clone or download** the repository:
   ```bash
   git clone https://github.com/nicksananton0923-create/Industrial-hvac-maintenance.git
   ```
2. **Pick the templates you need** — most teams start with `hvac-asset-register.csv`, `ahu-inspection-checklist.csv`, and `fault-corrective-log.csv`.
3. **Open the CSVs in Excel, Google Sheets, or LibreOffice Calc** — the headers are plain English and the templates are immediately usable.
4. **Read `recordkeeping-guide.md`** to understand every column before you start filling the templates with real data.
5. **Replace sample data carefully** — never commit confidential facility readings to a public fork. Use anonymized copies for any shared version.

### As a student

1. Read `system-overview.md` first for the big picture.
2. Read `inspection-methodology.md` to understand how each component is actually inspected.
3. Open the CSVs to see how records are structured.
4. Read `recordkeeping-guide.md` to understand why each field exists.
5. Try filling in `sample-records.csv` with hypothetical data to test your understanding.

---

## 🎯 How to Use the Templates

- **One row per record.** Each CSV is structured so one row represents one event (one inspection, one service, one fault). This keeps the data tidy and easy to filter or chart.
- **Asset IDs are sacred.** Use the same asset ID across every CSV so records can be joined. The asset register is the source of truth for what each ID means.
- **Units must be explicit.** Every numeric field has a documented unit in `recordkeeping-guide.md`. Never write a number without a unit, and never mix units in the same column.
- **Dates are ISO 8601.** Use `YYYY-MM-DD` for all dates so the CSVs sort and filter correctly across locales.
- **Technician and signature.** Every record has a `technician` column and (where applicable) a `reviewed_by` column. Accountability is the whole point.

---

## ⚠️ Safety &amp; Accuracy

- **Refrigerant handling must be done by certified personnel.** The `refrigerant-service-log.csv` template is for record-keeping only — filling it in does not authorize anyone to handle refrigerants.
- **HVAC electrical work is hazardous.** Any template that includes electrical readings assumes the work is being done by a qualified electrician or technician.
- **OEM manuals override these templates.** Equipment-specific limits, service intervals, and procedures from the OEM manual always take precedence. These templates are vendor-neutral; they cannot capture every model-specific requirement.
- **Confidentiality.** Never commit real facility readings, customer names, or proprietary information to a public repository. Use the `sample-records.csv` pattern as a guide for anonymizing your own data.

---

## 🗺 Roadmap

- [ ] Add `boiler-inspection-checklist.csv` for heating-side coverage.
- [ ] Add `iaq-monitoring-log.csv` for indoor air-quality readings.
- [ ] Add a dashboard build guide (Excel / Power BI) that visualizes the CSVs.
- [ ] Add a Python utility script to validate CSVs against the schema in `recordkeeping-guide.md`.
- [ ] Translate key documentation into Tamil for bilingual use.

---

## 📄 License

This project is released under the MIT License unless the `LICENSE` file in the repository root specifies otherwise. The templates and documentation are free to use, modify, and distribute — including for commercial facility maintenance work — provided the license terms are respected.

---

<div align="center">

**🏭 Document it once. Find it forever. 🏭**

<sub>Project by [@nicksananton0923-create](https://github.com/nicksananton0923-create) · Electrical Technology · Renewable Energy · Automation</sub>

</div>
