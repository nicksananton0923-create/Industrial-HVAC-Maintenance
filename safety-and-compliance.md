# Safety and Compliance

This document defines the safety rules, regulatory framework, and compliance evidence requirements for the inspection, service, and maintenance activities documented in this repository. It is the procedural reference for the safety notices that appear throughout the templates and documentation.

Safety in industrial HVAC maintenance is non-negotiable. The equipment operates at pressures that can rupture pipes, temperatures that can scald skin, voltages that can electrocute, and refrigerants that can displace oxygen, ignite, or expose personnel to toxicity. The maintenance activities that the templates in this repository support — including refrigerant handling, electrical work, mechanical work at height, and confined space entry — are each regulated activities that require training, certification, authorization, and adherence to specific procedures.

This document is a summary and a pointer to the standards that govern the work. It does not replace those standards; it structures the site's compliance with them.

---

## 1. Regulatory Framework

The templates and procedures in this repository are aligned with the following standards. Each standard is the responsibility of a different regulatory body; site operators must confirm which standards apply to their jurisdiction and operations.

### 1.1 ASHRAE Standards (International)

ASHRAE (American Society of Heating, Refrigerating and Air-Conditioning Engineers) publishes consensus standards that are widely adopted internationally. The standards relevant to this repository:

- **ASHRAE 15** — *Safety Standard for Refrigeration Systems.* Defines refrigerant classification, charge limits, machinery room requirements, and safety devices. Cited by most international building codes.
- **ASHRAE 34** — *Designation and Safety Classification of Refrigerants.* Assigns refrigerant designations (e.g. R-134a, R-514A) and safety groups (A1, A2, A3, B1, B2, B3) based on toxicity and flammability.
- **ASHRAE 62.1** — *Ventilation for Acceptable Indoor Air Quality.* Defines minimum outdoor air rates by occupancy; drives the outdoor air damper position setpoints in AHU control sequences.
- **ASHRAE 90.1** — *Energy Standard for Buildings.* Sets minimum efficiency and minimum control requirements (e.g. deadband, economizer, lighting). Drives the PM program for energy-consuming equipment.
- **ASHRAE 185** — *Method of Testing Microbiocidal Devices in Air.* Reference for UV-C and other biological control devices in AHUs.
- **ASHRAE 188** — *Legionellosis: Risk Management for Building Water Systems.* Mandates a water management plan for buildings with cooling towers; drives the cooling tower water treatment PM.

### 1.2 EU F-Gas Regulation 517/2014 and EN 378

The EU F-Gas Regulation (Regulation (EU) No 517/2014) governs fluorinated greenhouse gases in the European Union. Key requirements relevant to this repository:

- **Leak checking** frequency scales with charge (in CO2-equivalent tonnes): ≥ 5 t CO2-eq requires 24-month checks, ≥ 50 t requires 12-month checks, ≥ 500 t requires 6-month checks. Chillers with R-134a charge above ~350 kg fall into the 12-month category; charge above ~2,500 kg falls into the 6-month category.
- **Certification** of technicians and companies. Only certified personnel may carry out installation, servicing, maintenance, repair, or decommissioning of equipment containing F-gases.
- **Refrigerant recovery** — refrigerant must be recovered during servicing and at end of life; venting is prohibited.
- **Logbook** — each piece of equipment ≥ 5 t CO2-eq must have a logbook recording the quantity and type of refrigerant, the quantity added during servicing, the quantity recovered, the leak checks performed, and the identity of the servicing company and technician.

The `refrigerant-service-log.csv` template is the structure for this logbook. The technician's `technician_cert_id` field documents certification; the `cylinder_id_recovered` field documents the recovery chain; the `leak_check_method` and `leak_check_result` fields document the leak check.

EN 378 — *Refrigerating systems and heat pumps — Safety and environmental requirements* — is the European harmonized standard that gives technical presumption of conformity with the F-Gas Regulation's safety requirements. It defines pressure equipment classification, machinery room requirements, and refrigerant charge limits by occupancy.

### 1.3 AS/NZS Standards (Australia / New Zealand)

For sites in Australia and New Zealand, the AS/NZS standards apply:

- **AS/NZS 1677.1** — *Refrigerating systems — Refrigerant classification.* Equivalent to ASHRAE 34 for the Australian / NZ market.
- **AS/NZS 1677.2** — *Refrigerating systems — Safety requirements for fixed applications.* Equivalent to ASHRAE 15.
- **AS/NZS 3666.1** — *Air-handling and water systems of buildings — Microbial control — Design, installation and commissioning.*
- **AS/NZS 3666.2** — *Air-handling and water systems of buildings — Microbial control — Operation and maintenance.* Drives the cooling tower PM program in Australia / NZ.
- **AS/NZS 3666.3** — *Performance-based maintenance of cooling water systems.*
- **AS/NZS 5149** — *Refrigerating systems and heat pumps — Safety and environmental requirements* (multi-part, harmonized with EN 378).

The Australian Refrigeration Council (ARC) administers the refrigerant handling licensing scheme (ARCtick). The `technician_cert_id` field in `refrigerant-service-log.csv` should record the technician's ARCtick licence number for Australian / NZ sites.

### 1.4 US EPA Section 608

For sites in the United States, EPA Section 608 of the Clean Air Act governs refrigerant handling. Technicians must hold a Section 608 certification (Type I for small appliances, Type II for high-pressure systems, Type III for low-pressure systems, Universal for all). The `technician_cert_id` field records the technician's certification number.

EPA also imposes leak repair requirements under 40 CFR 82.157: commercial refrigeration and comfort cooling equipment with charges above 50 lb (22.7 kg) must be repaired within 30 days of leak detection if the leak rate exceeds thresholds (30% for commercial refrigeration, 10% for comfort cooling, on an annualized basis).

### 1.5 Pressure Equipment Directives

Chiller condensers, evaporators, and oil coolers are pressure vessels. They are subject to pressure equipment regulations: PED 2014/68/EU in the EU, ASME Boiler and Pressure Vessel Code Section VIII in the US, AS 1210 in Australia. Periodic inspection (every 5–10 years depending on jurisdiction) is mandatory and is recorded in `chiller-service-record.csv` with a reference to the inspection certificate.

---

## 2. Refrigerant Safety

### 2.1 Refrigerant Properties

The refrigerants in scope for this repository (R-134a, R-123, R-514A) are widely used in centrifugal and screw chillers in industrial applications. Each has specific hazards:

| Refrigerant | ASHRAE 34 Group | Toxicity | Flammability | GWP (AR4) | Notes |
|-------------|-----------------|----------|--------------|-----------|-------|
| R-134a | A1 | Low (TLV 1,000 ppm) | None | 1,430 | Common HFC; phase-down under EU F-Gas |
| R-123 | B1 | Low (TLV 50 ppm) | None | 240 | HCFC; global phaseout in progress |
| R-514A | B1 | Low (TLV 323 ppm OEL) | None | 2 | HFO blend; R-123 replacement |

Group A refrigerants have lower toxicity (TLV ≥ 400 ppm); Group B have higher toxicity (TLV < 400 ppm). R-123 and R-514A are Group B, which means ASHRAE 15 imposes additional requirements: machinery room refrigerant detectors, alarms, and ventilation interlocks; restricted occupant access to machinery rooms; and trained personnel for any work in the machinery room.

### 2.2 Asphyxiation Risk

All refrigerants are heavier than air and can displace oxygen in a confined space. A refrigerant leak in a machinery room can reduce oxygen below the 19.5% minimum for safe occupancy. The symptoms of oxygen deficiency (lightheadedness, confusion, fatigue) are subtle and progress to unconsciousness without warning.

Mitigations:

- Machinery rooms with charges above the threshold defined in ASHRAE 15 (Section 8) must have refrigerant detectors with audible and visual alarms, set to alarm at the TLV-TWA or lower.
- Machinery room ventilation must activate automatically when refrigerant concentration exceeds the setpoint.
- Personnel entering a machinery room after an alarm must use a calibrated atmospheric monitor and confirm oxygen concentration above 19.5% and refrigerant concentration below the TLV before entry without respiratory protection.
- Confined space entry procedures apply to any work in a refrigerant-contaminated atmosphere; self-contained breathing apparatus (SCBA) is required.

### 2.3 Toxicity

R-123 and R-514A have TLV-TWA values lower than R-134a, reflecting their higher toxicity. The TLV-TWA is the time-weighted average concentration to which a worker may be exposed for 8 hours per day, 5 days per week, without adverse effect. Exceeding the TLV requires respiratory protection.

The primary toxic effect of R-123 and R-514A exposure is on the cardiovascular system — these refrigerants can produce cardiac sensitization at concentrations above the TLV, leading to arrhythmia under physical stress. Personnel working with these refrigerants must be trained on the symptoms and on the appropriate response.

### 2.4 Burns and Frostbite

Liquid refrigerant released to atmosphere is at saturation temperature, which for R-134a at atmospheric pressure is -26 °C (-15 °F); for R-123 it is 27 °C (81 °F); for R-514A it is 35 °C (95 °F). Contact with liquid R-134a causes frostbite. Contact with liquid R-123 or R-514A is less severe thermally but the refrigerant dissolves skin oils and causes dermatitis on prolonged contact.

Mitigations:

- Wear refrigerant-resistant gloves (nitrile for R-134a and R-514A, butyl rubber for R-123 — confirm glove material compatibility before use).
- Wear safety glasses or face shield when there is any risk of refrigerant splashing (e.g. when opening a system, when connecting recovery cylinders).
- Wear long-sleeved clothing to protect skin from splashes.
- Have eyewash and emergency shower available in the machinery room.

### 2.5 Pressure Hazards

Refrigeration systems operate at pressures from sub-atmospheric (R-123 chillers operate with low-side pressure below atmospheric) to several hundred psig. Opening a pressurized system releases refrigerant at high velocity, which can propel debris, refrigerant oil, and metal fragments.

Mitigations:

- Before opening any refrigeration system, recover the refrigerant to a recovery cylinder until the system pressure is at or below atmospheric (0 kPa gauge). Confirm with a calibrated pressure gauge.
- Verify that the system is at zero pressure before opening any fitting. Do not rely on the system's own pressure gauge — it may be faulty.
- Wear eye protection when opening any fitting on a refrigeration system, even after pressure verification.
- Never apply heat to a refrigeration system that contains refrigerant — heating raises pressure and can rupture the system or cause a relief valve to lift.

---

## 3. Electrical Safety

### 3.1 LOTO (Lockout / Tagout)

Every electrical maintenance task on HVAC equipment requires lockout/tagout (LOTO) per the site's LOTO procedure, which is typically aligned with OSHA 1910.147 (US) or equivalent international standard. LOTO isolates energy sources (electrical, pneumatic, hydraulic, thermal, gravitational) and prevents accidental re-energization while personnel are working.

The LOTO sequence:

1. Notify affected personnel that LOTO is about to be applied.
2. Shut down the equipment by normal means (BMS stop command).
3. Open the disconnect switch for the equipment. Confirm open with a voltmeter.
4. Apply the lock and tag at the disconnect. The lock is keyed to the applying technician; the tag identifies the technician, the date, and the reason.
5. Verify isolation by attempting to start the equipment (it should not start) and by testing for voltage at the equipment's terminals.
6. Perform the work.
7. On completion, remove tools and materials from the work area.
8. Verify personnel are clear.
9. Remove the lock and tag.
10. Notify affected personnel that LOTO is removed and the equipment is being returned to service.

Group LOTO (multiple technicians applying their own locks to a hasp) is required when multiple technicians are working on the same equipment. Each technician's lock prevents the disconnect from being closed until all locks are removed.

### 3.2 Arc Flash

Arc flash is the explosive release of energy caused by an electrical arc. The arc flash energy depends on the available fault current, the voltage, and the duration of the arc. The arc flash boundary is the distance at which the arc flash energy is sufficient to cause second-degree burns (1.2 cal/cm²).

Within the arc flash boundary, personnel must wear arc flash PPE (face shield, gloves, flame-resistant clothing) appropriate to the incident energy at the working distance. The incident energy is calculated based on the equipment's available fault current and clearing time, typically by a power systems engineer using IEEE 1584 methods.

HVAC equipment's arc flash risk varies:

- 120 VAC control circuits: low risk; standard PPE.
- 240/480 VAC motor circuits: medium risk; category 1–2 PPE depending on fault current.
- 4160 VAC medium-voltage chiller motors: high risk; category 4 PPE; work only with power isolated.
- VFD output circuits: high frequency content increases arc flash energy; treat as medium-voltage even at 480 VAC.

### 3.3 Capacitor Discharge

VFDs and motor starters contain capacitors that retain charge after power is removed. The capacitor discharge time can be several minutes; some VFDs require up to 15 minutes for the DC bus to discharge to safe levels.

Mitigation:

- Wait the OEM-specified discharge time after isolating power before opening the VFD enclosure.
- Verify the DC bus voltage with a calibrated voltmeter before touching any internal component.
- Use the VFD's discharge function (if equipped) to actively discharge the bus before opening.
- Wear insulated gloves rated for the DC bus voltage.

### 3.4 Working at Height

HVAC maintenance often requires working at height — on cooling towers (roof), on AHUs (ceiling), on chiller piping (platforms). Fall protection per OSHA 1926 Subpart M (US) or equivalent is required above 1.8 m (6 ft) for general industry or above 1.2 m (4 ft) for construction.

Mitigation:

- Use a fixed platform with guardrails where available.
- Use a fall arrest system (harness, lanyard, anchor point) where fixed platforms are not available.
- Confirm the anchor point is rated for fall arrest (not just for restraint).
- Inspect fall arrest equipment before each use; retire after any fall.
- Two-person rule: at least one other person on site whenever work at height is performed.

---

## 4. Mechanical Safety

### 4.1 Pressure Testing

After opening a refrigeration system for service, the system is pressure-tested with an inert gas (nitrogen or nitrogen-helium blend for tracer gas leak detection) before evacuation and recharge. The pressure test is performed at the system's design pressure, typically 1.1× the maximum operating pressure per ASHRAE 15.

The pressure test is recorded in `refrigerant-service-log.csv` with `service_type` = `system_pressure_test`, the test pressure (in kPa and psig), the test duration (minimum 60 minutes; 24 hours for high-precision tests), and the result (no pressure drop, or pressure drop indicating a leak that must be located and repaired).

Never pressure-test a refrigeration system with air or oxygen — the mixture of refrigerant, oil, and oxygen at elevated pressure is explosive.

### 4.2 Evacuation

After pressure testing and before charging, the system is evacuated to remove non-condensables (air, nitrogen, water vapor). Evacuation is performed with a vacuum pump until the system pressure reaches the level specified by the OEM (typically 50–500 Pa / 500–5,000 microns, depending on system size and refrigerant).

The vacuum level is recorded in `refrigerant-service-log.csv` with `vacuum_level_pa` and `vacuum_level_microns` fields. The vacuum is held for the OEM-specified duration (minimum 30 minutes; typically 2–4 hours for large chillers) to confirm there is no moisture boiling off and no inboard leak.

### 4.3 Rotating Equipment

Pumps, fans, and compressors have rotating parts that can cause severe injury if contacted. Guards are required over couplings, belts, and fan inlets. Guards must be in place before the equipment is started and must not be removed while the equipment is running.

Mitigations:

- Inspect guards before each start. Replace damaged guards before operating.
- Lock out power before removing a guard for maintenance.
- Do not wear loose clothing, jewelry, or long hair that could be caught in rotating parts.
- Do not reach into a fan housing, pump volute, or compressor while the equipment is in motion. Wait for the rotating element to come to a complete stop.

### 4.4 Hot Surfaces

Refrigerant discharge lines, hot water pipes, and steam pipes can reach temperatures that cause instant burns on contact. Hot water above 60 °C / 140 °F can scald skin in less than 5 seconds. Refrigerant discharge above 80 °C / 176 °F is common during normal operation.

Mitigations:

- Inspect insulation on all hot surfaces before each round; repair damaged insulation before the next round.
- Treat any uninsulated pipe in a plant room as potentially hot; confirm with a non-contact thermometer before touching.
- Wear long-sleeved clothing and gloves when working in a plant room with uninsulated hot pipes.
- Post hot-surface warning labels at access points to plant rooms with significant hot-surface exposure.

---

## 5. Confined Space

### 5.1 Definition

A confined space is large enough for personnel to enter, has limited means of entry and exit, and is not designed for continuous occupancy. HVAC confined spaces include: chiller internal vessels (only during major overhaul), AHU interiors (during service), cooling tower sumps (during descale), and pipe trenches.

### 5.2 Confined Space Entry Permit

Entry to a confined space requires a permit that documents: the space to be entered, the hazards present, the atmospheric test results (oxygen, flammable gases, toxic gases), the entry team and the attendant, the rescue plan, and the duration of the entry.

The permit is issued by a qualified person and signed by the entry team before entry. The permit is posted at the entry point. The permit is cancelled when the entry is complete and all personnel have exited.

### 5.3 Atmospheric Testing

Before entry, the confined space atmosphere is tested for: oxygen (19.5–23.5%), flammable gases (< 10% LEL), and toxic gases (per the space's hazard assessment, typically CO and H2S). The atmosphere is monitored continuously during the entry.

If the atmosphere is outside any limit, personnel exit immediately and do not re-enter until the atmosphere is corrected (typically by forced ventilation with a fan).

### 5.4 Cooling Tower Sump Entry

Cooling tower sump entry during descale is a particularly hazardous confined space. The sump may contain biocide residue, scale dust, and biological contamination (including potential Legionella). Entry requires:

- Isolation of the tower fan and water pump under LOTO.
- Draindown of the sump.
- Forced ventilation of the sump (typically 20 air changes per hour).
- Atmospheric testing (oxygen, CO, H2S, VOCs from biocide residue).
- Full-body harness with retrieval line, attended by a second person outside the sump.
- Respiratory protection (P100 or P3 cartridge minimum; SCBA if atmosphere cannot be maintained).
- Disinfection of PPE after exit.

---

## 6. Compliance Evidence

### 6.1 What Constitutes Compliance Evidence

Compliance evidence for HVAC maintenance is the documentation that demonstrates a regulated activity was performed in accordance with the applicable standard. The evidence consists of:

1. **The procedure** that was followed (this repository's `docs/`).
2. **The record** that the work was performed (the relevant template in `templates/`).
3. **The technician's certification** to perform the work (recorded in the template's `technician_cert_id` field).
4. **The equipment's documentation** (OEM manual, pressure vessel certificate).

All four elements must be present and must be consistent. A record without a corresponding procedure is suspect. A procedure without a record is evidence of nothing. A record signed by an uncertified technician is invalid.

### 6.2 F-Gas Compliance (EU)

For EU F-Gas compliance, the regulator may inspect:

- The refrigerant service log for each chiller above 5 t CO2-eq (this is `refrigerant-service-log.csv`).
- The leak check frequency for each chiller (this is `preventive-maintenance-schedule.csv` with `regulatory_ref` = `EU F-Gas 517/2014 Art.3`).
- The technician certification (recorded in the log).
- The company certification (held in the maintenance office).
- The refrigerant recovery and disposal records (recorded in the log with `cylinder_id_recovered`).

### 6.3 ASHRAE 188 Compliance (Legionella)

For ASHRAE 188 compliance in the US (or AS/NZS 3666 in Australia/NZ), the regulator or insurer may inspect:

- The water management plan (held in the maintenance office; referenced from `preventive-maintenance-schedule.csv`).
- The cooling tower water test records (this is `cooling-tower-log.csv`).
- The biocide dosing records (in `cooling-tower-log.csv` with `biocide_added_l` and `biocide_added_gal`).
- The annual service record for cooling towers (in `cooling-tower-log.csv` with `notes` indicating annual service).
- The training records for personnel performing water testing (held in HR; referenced from `cooling-tower-log.csv` via `technician_id`).

### 6.4 Pressure Equipment Compliance

For pressure equipment compliance (PED in EU, ASME in US, AS 1210 in Australia), the regulator may inspect:

- The pressure vessel certificate (held in the maintenance office; referenced from `chiller-service-record.csv`).
- The periodic inspection records (in `chiller-service-record.csv` with `service_type` = `pressure_vessel_inspection`).
- The pressure relief valve test records (in `chiller-service-record.csv` with `service_type` = `relief_valve_test`).
- The operating log (in `operating-readings.csv`) showing the equipment has not been operated above its design pressure.

---

## 7. Emergency Procedures

### 7.1 Refrigerant Leak in Machinery Room

If the refrigerant detector in a machinery room alarms (typically set at the TLV-TWA or below):

1. **Evacuate** the machinery room immediately. Do not delay to retrieve tools or belongings.
2. **Isolate** the chiller via the emergency stop button at the machinery room exit. The emergency stop closes the chiller's refrigerant isolation valves and stops the compressor.
3. **Activate** the machinery room ventilation (it should activate automatically on the alarm; verify manually).
4. **Notify** the site emergency response team and the refrigerant service contractor.
5. **Do not re-enter** the machinery room until the refrigerant concentration is confirmed below the TLV-TWA by a calibrated atmospheric monitor and the source of the leak has been identified and isolated.
6. **Record** the event in `fault-corrective-log.csv` with `severity` = `critical` and `regulatory_impact` = `yes` (F-Gas leak above threshold requires reporting to the regulator within the timeframe specified by the regulation — typically 30 days for EU F-Gas).

### 7.2 Cooling Tower Biological Incident

If routine sampling of a cooling tower returns a positive result for Legionella above the action threshold (typically 1,000 CFU/L for routine action, 10,000 CFU/L for emergency action):

1. **Notify** the site water treatment contractor immediately.
2. **Increase** biocide dosing per the water management plan's emergency protocol.
3. **Restrict** access to the tower drift zone (typically the roof and the downwind area).
4. **Re-sample** after 24–48 hours of emergency treatment. If still positive, consider emergency descale and disinfection.
5. **Notify** the public health authority per local regulation (mandatory in many jurisdictions for confirmed Legionella above the threshold).
6. **Record** the event in `fault-corrective-log.csv` with `severity` = `critical` and `regulatory_impact` = `yes`.
7. **Conduct** a root cause analysis: was the biocide program adequate? Was the dosing pump operating? Was the makeup water contaminated? Was the drift eliminator intact? Document the RCA findings in the maintenance office.

### 7.3 Electrical Incident

If an electrical incident occurs (arc flash, electrocution, electrical fire):

1. **De-energize** the affected circuit at the upstream disconnect. Do not approach the affected equipment until de-energized.
2. **Provide first aid** to any injured personnel. Call emergency medical services if injuries are more than minor.
3. **Fight** electrical fires only with Class C extinguishers (CO2 or clean agent). Never use water on an electrical fire.
4. **Do not touch** any person in contact with energized equipment — use a non-conductive object to separate them from the source, or de-energize the source first.
5. **Notify** the site emergency response team and the electrical service contractor.
6. **Preserve** the scene for investigation. Do not repair or modify the affected equipment until the investigation is complete.
7. **Record** the event in `fault-corrective-log.csv` with `severity` = `critical` and `regulatory_impact` = `yes` (electrical incidents are typically reportable to the safety regulator).

---

## 8. Training and Certification

### 8.1 Technician Certification Matrix

The table below lists the certifications that are typically required to perform the activities documented in this repository. The specific certifications required depend on the jurisdiction and the equipment; site operators must confirm the requirements with their regulator and insurer.

| Activity | US certification | EU certification | AU/NZ certification |
|----------|-----------------|------------------|---------------------|
| Refrigerant handling (comfort cooling) | EPA Section 608 Universal | F-Gas Company Certificate + Personnel Certificate (Cat I) | ARCtick Refrigerant Handling Licence (RHL) |
| Refrigerant handling (industrial) | EPA Section 608 Type III (low pressure) or Universal | F-Gas Personnel Certificate (Cat I or Cat V for recovery) | ARCtick RHL |
| Electrical work (low voltage) | State electrical licence | National electrical qualification | State electrical licence |
| Electrical work (medium voltage) | State electrical licence + MV qualification | National MV qualification | State electrical licence + HV switchyard |
| Pressure vessel inspection | ASNT / NBIC inspector | Recognized inspection body | AS/NZS inspector |
| Legionella water sampling | Site-specific training | Site-specific training | Site-specific training |
| Confined space entry | OSHA 1910.146 training | National confined space training | AS/NZS 2865 training |
| Lockout/Tagout | OSHA 1910.147 training | National LOTO training | National LOTO training |

### 8.2 Training Records

Training records for each technician are held in the maintenance office and in the site's HR system. The records include: the technician's name, the certification, the issuing body, the issue date, the expiry date, and the scope of work authorized. The records are reviewed annually; expiring certifications are renewed before expiry.

For entries in the templates' `technician_cert_id` field, the certification must be current at the time of the work. A record signed by an uncertified or expired-certification technician is invalid for compliance evidence; the work must be re-performed by a certified technician.

### 8.3 Site-Specific Training

Site-specific training covers topics not addressed by general certifications: the site's LOTO procedure, the site's confined space entry procedure, the site's emergency response plan, the location of emergency stops and exits, the location of safety equipment (eyewash, shower, fire extinguishers), and the site's specific equipment (chiller models, BMS interface, water treatment system). Site-specific training is delivered at induction and refreshed annually.

Site-specific training records are held in the maintenance office. The training is required before a technician works unsupervised on the site. A technician who has not completed site-specific training must be accompanied by a trained technician at all times in the plant room.

---

## 9. Audit and Continuous Improvement

### 9.1 Internal Audit

The maintenance manager conducts an internal audit of the safety and compliance records quarterly. The audit confirms:

- All `technician_cert_id` values in the templates reference current certifications.
- All safety-critical PM tasks (refrigerant leak checks, cooling tower water tests, pressure vessel inspections) have been completed on schedule.
- All fault records with `safety_critical` = `yes` have a documented root cause analysis.
- All refrigerant service records with `quantity_recovered_kg` > 0 have a corresponding disposal record.
- The CHANGELOG documents all template changes.

Audit findings are recorded in a separate audit log (not in this repository) and tracked to closure.

### 9.2 External Audit

External audits are conducted by the regulator (e.g. EU F-Gas authority), the insurer, or the client (for contracted maintenance). The external auditor may request:

- The repository's git history (showing when records were added and by whom).
- The OEM manuals referenced in the procedures.
- The technician certifications referenced in the records.
- The water management plan (for Legionella compliance).
- The pressure vessel certificates (for pressure equipment compliance).

The repository is structured to make external audit straightforward: the records are in `templates/` and `data/`; the procedures are in `docs/`; the CHANGELOG documents template evolution; the git history documents record evolution.

### 9.3 Continuous Improvement

Safety and compliance practices are continuously improved based on audit findings, incident investigations, and regulatory changes. Improvements are reflected in updates to:

- The templates (adding fields, refining values).
- The procedures in `docs/` (adding steps, refining acceptance criteria).
- The training program (adding topics, refreshing content).
- The CHANGELOG (documenting the change, the date, and the reason).

Continuous improvement is not optional. Regulations change (the EU F-Gas Regulation has been amended several times since 2014; ASHRAE 15 was significantly revised in 2019; AS/NZS 5149 was published in 2016 replacing AS/NZS 1677.2). Procedures that do not keep up with regulatory change become non-compliant. The annual review (Section 6 of `preventive-maintenance-planning.md`) includes a regulatory currency check — confirm that the standards referenced in `docs/safety-and-compliance.md` are current and update the references if they are not.
