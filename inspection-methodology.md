# Inspection Methodology

This document defines the inspection routines, the data to capture at each routine, and the acceptance criteria for each equipment class. It is the procedural reference for the templates in `templates/`. Every entry in the `preventive-maintenance-schedule.csv` template references a procedure section in this document.

Inspection routines are organized by frequency: daily operator rounds, weekly visual inspections, monthly inspections, quarterly inspections, annual services, and multi-year special inspections. Each routine has a defined scope, a defined duration, and a defined acceptance criteria set. Deviation from acceptance criteria triggers a fault entry in `fault-corrective-log.csv`.

---

## 1. General Principles

### 1.1 Safety First

Every inspection starts with a safety assessment. The technician must confirm: (a) that the equipment is in a safe state for the inspection being performed — for a visual inspection, that means running normally; for a contact inspection, that may require isolation under LOTO; (b) that the technician has the required PPE for the task — hearing protection in plant rooms, refrigerant-resistant gloves when opening the chiller panel, arc-flash PPE when opening energized electrical compartments; (c) that the work area is safe — no liquid spills underfoot, no hot uninsulated surfaces within reach, no confined space concerns.

If any of these conditions is not met, the inspection is halted and a fault entry is logged. Continuing an inspection in unsafe conditions to "complete the round" is unacceptable and is itself a reportable incident. The technician's certification and authorization for the task must be current; if not, the task is reassigned.

### 1.2 Capture the Actual Reading, Not the Expected Reading

Inspection data is only useful if it reflects reality. A technician who records the expected value rather than the observed value — because the observed value "looks wrong" or "must be a sensor error" — defeats the purpose of the inspection. The expected value is established by OEM manuals and site baselines; the inspection record captures the observed value; a deviation triggers investigation, not normalization. If a sensor is suspected to be wrong, the inspection record notes the observed (wrong-looking) value and adds a note "sensor suspected — to be verified"; a follow-up task verifies the sensor.

This discipline is what makes trend analysis possible. A bearing temperature that creeps from 52 °C to 58 °C over six months is detectable only if the actual values were recorded each time. A record that has been "corrected" to the expected value hides the trend and ensures the failure is detected only when the bearing seizes.

### 1.3 Use OEM Limits, Not Generic Limits

This document mentions illustrative limit values for orientation. Operational limits are equipment-specific and are set by the OEM in the equipment manual. The maintenance office must hold the current revision of each OEM manual and the limits in the manual take precedence over any limit mentioned here. If the OEM has not specified a limit, the site engineering authority sets a documented limit based on engineering judgement and recorded in the asset register.

### 1.4 Defect Versus Fault

A defect is a condition that does not meet specification but does not prevent operation — for example, a bearing temperature of 65 °C where the OEM limit is 80 °C but the historical baseline is 55 °C. A defect is recorded in the inspection record `notes` field and a follow-up task is created in the PM schedule. A fault is a condition that prevents operation or violates a safety limit — for example, a bearing temperature of 85 °C, or a refrigerant leak detected by electronic leak detector. A fault is recorded in `fault-corrective-log.csv` and immediate corrective action is taken.

The distinction matters because defects accumulate and need to be tracked as a backlog; faults require immediate response. Confusing the two leads either to unnecessary downtime (treating every defect as a fault) or to missed early warnings (treating every fault as a defect).

### 1.5 Inspection Sequence

For any equipment, the inspection sequence is: observe from a distance (sight, sound, smell, thermal) before approaching; observe from the operating position; observe the BMS screen and confirm it matches the local instrument; only then touch the equipment or open a panel. This sequence avoids the technician's presence affecting the readings and reduces the risk of contact with energized or hot surfaces before the situation is understood.

---

## 2. Daily Operator Round

**Frequency:** Once per shift, or as defined by site protocol.
**Duration:** 15–30 minutes for a small plant room; up to 60 minutes for a large multi-room plant.
**Scope:** Visual and auditory scan of all running equipment; record BMS readings on the chiller plant; confirm cooling tower fan operation and water level; check for any active alarms.

### 2.1 Chiller Daily Round

Walk around the chiller. Listen for unusual sounds — bearing whine, compressor surge, refrigerant gurgle. Smell for oil burning or refrigerant leaks (R-134a and R-514A are nearly odourless; oil contamination often produces a detectable odour before refrigerant is detectable). Look for oil spots under the chiller, particularly at the compressor seal, oil pump, and oil cooler.

Confirm against the BMS: CHW supply temperature (target within ±0.5 °C of setpoint), CHW return temperature (indicates load), condenser water supply and return temperatures, compressor amps (against FLA), suction and discharge pressures. Record in `operating-readings.csv` if not automatically logged.

### 2.2 Cooling Tower Daily Round

Walk to the tower. Listen for fan motor noise, gearbox whine, water distribution noise. Look at the drift eliminator for excessive drift (a visible plume of droplets indicates drift eliminator failure). Confirm water level in the sump (mid-range on the sight glass). Confirm that makeup water is not running continuously (continuous makeup indicates a leak in the system or a stuck blowdown valve).

Take a water sample. Test pH (target per water treatment program, typically 7.5–8.5), conductivity (target per program, typically 1000–2000 µS/cm). Confirm biocide dosing pump operation. Record in `cooling-tower-log.csv`.

### 2.3 Pump and Fan Daily Round

Walk past each pump and fan. Listen for bearing noise, cavitation (pumps), belt slap (belt-driven fans). Feel the motor housing briefly with the back of the hand (motor housing temperature should be warm but not hot — under 60 °C typically). Look at the motor for oil leaks at the bearings, oil ring rotation (on ring-oiled bearings), and VFD alarm indicators. Confirm against the BMS: motor amps (against FLA), VFD output frequency (if applicable).

### 2.4 AHU Daily Round

For each AHU, confirm supply fan operation, supply air temperature against setpoint, filter differential pressure (against limit), and outdoor air damper position. Listen for fan bearing noise, belt slap, coil freeze-up (a characteristic hissing noise on chilled water coils that are below dewpoint with insufficient airflow). Smell for biological growth (musty odour indicates coil or drain pan contamination).

### 2.5 BMS Daily Round

Open the BMS supervisor. Review the alarm log since the last round. Investigate any active alarms; resolve or escalate. Review trend charts of CHW supply temperature, chiller amps, condenser approach, tower approach, and any high-criticality zone temperatures. Confirm that no trend is drifting abnormally — a slow drift in condenser approach over weeks indicates tube fouling; a sudden change in condenser approach indicates a flow problem.

---

## 3. Weekly Visual Inspection

**Frequency:** Weekly.
**Duration:** 30–60 minutes per major equipment item.
**Scope:** More detailed visual inspection than the daily round, with specific attention to consumable items (filters, belts) and to the exterior condition of equipment.

### 3.1 Chiller Weekly Visual

Inspect the chiller exterior for oil seepage at flanges, the oil pump seal, and the oil filter housing. Inspect the refrigerant sight glass (where fitted) for bubble indication of low charge or moisture indicator colour change (blue = dry, yellow = wet; a yellow indicator requires immediate attention to the purge unit). Inspect the compressor motor terminal box for any sign of overheating (discolouration, smell). Inspect the chiller control panel for alarm indicators and confirm the chiller's local run hours counter.

Record observations in the chiller service record's `notes` field (no full service record is created for a weekly visual inspection; the next monthly or quarterly inspection creates the formal record).

### 3.2 AHU Weekly Visual

Inspect each AHU's filter bank: confirm filter differential pressure is below the changeout threshold, confirm filters are not buckled or bypassing, confirm filter frame seals are intact. Inspect the belt guard (if fitted) for damage. Inspect the condensate drain trap — confirm water is flowing (a dry trap indicates either that the coil is not dehumidifying or that the trap has lost its seal). Inspect the coil fins for damage or fouling.

### 3.3 Cooling Tower Weekly Visual

Inspect the tower fill from the access hatch — confirm even water distribution, no dry spots, no biological growth (slime or algae). Inspect the drift eliminator — confirm it is intact, properly seated, and not clogged with debris. Inspect the hot water distribution basin or spray nozzles — confirm even flow, no clogged nozzles. Inspect the sump — confirm no sediment accumulation above the design level.

### 3.4 Pump and Fan Weekly Visual

Inspect each pump and fan motor housing for cleanliness (dust on the motor housing reduces cooling and increases winding temperature). Inspect the motor terminal box for any sign of overheating. Inspect the coupling guard for damage. Inspect the seal (pump) or shaft seal (fan) for leakage — a slow drip from a pump mechanical seal is acceptable; a stream of water is not.

---

## 4. Monthly Inspection

**Frequency:** Monthly.
**Duration:** 60–120 minutes per major equipment item.
**Scope:** Detailed inspection with measurements; refrigerant leak check (F-Gas / equivalent requirement for some equipment categories); formal record created in the relevant template.

### 4.1 Monthly Refrigerant Leak Check

For chillers with a refrigerant charge above the threshold defined by the applicable regulation (e.g. EU F-Gas 517/2014 requires monthly leak checks for charge ≥ 5 t CO2-eq, with frequency scaling up to 1-month intervals for ≥ 500 t CO2-eq), perform a documented leak check.

Method: electronic leak detector (most sensitive for HFCs and HFOs), bubble solution (for confirmation of a located leak), or ultraviolet dye (for slow leaks that are not immediately detectable). Inspect all flanges, schrader valves, rupture discs, relief valve discharge, oil filter housing, oil pump seal, compressor seal, and any welded joints that have been reworked.

Record the result (no leak found, leak located and repaired, leak located and not yet repaired) in `refrigerant-service-log.csv` even if no refrigerant was added — the leak check itself is a regulated event and must be documented.

### 4.2 Monthly AHU Inspection

Open the AHU access panel. Inspect filters visually (replace if at changeout threshold, record in `filter-replacement-log.csv`). Inspect the fan belt for tension (deflection 10–15 mm at 10 N force for typical belt sections; consult OEM for specific belt), condition (no glazing, no cracking, no oil contamination), and alignment (laser alignment preferred; straight-edge acceptable). Inspect fan bearings — DE and NDE bearing temperature (contact thermometer or infrared at bearing housing), vibration (contact or non-contact per ISO 10816). Inspect the coil — clean if visible fouling. Inspect the drain pan — clean if contaminated, confirm trap is primed. Inspect the mixing box dampers — confirm damper actuators operate smoothly, damper blades seal against the frame.

Record in `ahu-inspection-checklist.csv`.

### 4.3 Monthly Cooling Tower Inspection

Open the tower access hatch. Inspect the fill — clean if fouled. Inspect the drift eliminator — clean if fouled, replace if damaged. Inspect the hot water distribution — clean nozzles if clogged. Inspect the fan motor and bearings (DE and NDE bearing temperature, vibration). Inspect the gearbox (where fitted) — oil level, oil sample for water contamination (Karl Fischer or visual).

Confirm water chemistry against the water treatment program: pH, conductivity, total dissolved solids, hardness, alkalinity, biocide residual. Adjust the blowdown cycle if conductivity is drifting. Confirm biocide dosing pump operation and biocide tank level.

Record in `cooling-tower-log.csv`.

### 4.4 Monthly Pump and Fan Vibration Inspection

Take a vibration reading at each pump and fan bearing in three axes (horizontal, vertical, axial) using an accelerometer. Compare to ISO 10816 zone criteria for the equipment class. A reading in zone C (restrict) triggers a follow-up vibration spectrum analysis to identify the dominant frequency and hence the likely failure mode (1× rotational frequency = imbalance; 2× = misalignment; high frequency = bearing; vane pass frequency = impeller or fan issue).

Take a bearing temperature reading at the DE and NDE bearings. Compare to OEM limit and to historical baseline. A 5 °C increase over baseline, even within OEM limit, triggers a follow-up.

Record in `pump-fan-bearing-inspection.csv`.

---

## 5. Quarterly Inspection

**Frequency:** Quarterly (every 90 days).
**Duration:** 2–4 hours per major equipment item.
**Scope:** Detailed inspection requiring partial disassembly or specialized instruments.

### 5.1 Quarterly Chiller Inspection

Sample the compressor oil. Send to laboratory for analysis: moisture content (Karl Fischer, target < 50 ppm for screw chillers, < 30 ppm for centrifugal), acidity (TAN, target < 0.1 mg KOH/g), metal content (wear metals: Fe, Cu, Sn, Pb; additive metals: P, Zn, Ca), particle count (ISO 4406 cleanliness code). Trend results quarter-over-quarter; a step change in any parameter warrants investigation.

Inspect the purge unit operation (centrifugal chillers). A purge unit that runs more frequently than baseline indicates an air ingress path that must be located and sealed. Inspect the oil filter differential pressure (where fitted); replace if at changeout threshold. Inspect the chiller's refrigerant moisture indicator sight glass (where fitted); a colour change indicates the dryer is saturated and must be replaced.

Inspect the condenser water flow switch (where fitted) — confirm it trips at the design low-flow setpoint. Inspect the chiller's safety controls: low-pressure cutout (test and confirm cutout point), high-pressure cutout, oil pressure differential cutout, motor overload. Confirm each cutout operates by simulating the trip condition under controlled conditions.

Record in `chiller-service-record.csv` with `service_type` = `quarterly_inspection`.

### 5.2 Quarterly Cooling Tower Descale

Where the water treatment program indicates scale formation, perform a descale. Add acid descaler (or organic acid alternative) per the water treatment program. Circulate per the descaler manufacturer's instructions. Neutralize and flush. Inspect the fill after descale — replace any sections that have been damaged by the descaler or that show structural degradation.

Record the descale event in `cooling-tower-log.csv` with a `notes` entry describing the descaler used, the duration, and the post-descale water chemistry.

### 5.3 Quarterly Belt Tension Inspection

Inspect belt tension on every belt-driven fan and pump. Measure deflection at the midpoint of the longest belt span under a defined force (per OEM — typically 10 N for A-section belts, 25 N for B-section). Compare to OEM specification; adjust if outside ±10% of specification. Inspect belt condition — replace any belt with glazing, cracking, or oil contamination; replace belts in matched sets, not individually. Inspect sheave condition — groove wear reduces belt life and indicates misalignment; replace sheaves with measurable groove wear. Confirm laser alignment of sheaves within OEM angular and parallel tolerance.

Record in `pump-fan-bearing-inspection.csv`.

---

## 6. Annual Service

**Frequency:** Annually (every 365 days or per OEM run-hour interval, whichever is sooner).
**Duration:** 4–8 hours for an AHU; 8–24 hours for a chiller; typically performed by OEM service personnel or by an authorized third party.
**Scope:** Full disassembly-level inspection; replacement of consumables; full functional test of safety controls; oil change on chiller compressor.

### 6.1 Annual Chiller Service

The annual chiller service is the most significant PM event of the year. It is typically performed by the OEM service organization or by an authorized third party under a service contract. The site's role is to provide access, isolate the chiller under LOTO, provide a refrigerant recovery cylinder if any refrigerant is to be recovered, and witness the work.

The annual service scope (per typical OEM procedure for centrifugal chillers) includes: oil change (drain, flush, refill with the OEM-specified oil, replace oil filter, record oil quantity in litres and quarts in `chiller-service-record.csv`); eddy-current or ultrasonic leak detection on the entire refrigerant circuit (record leak check method and result in `chiller-service-record.csv` and create an entry in `refrigerant-service-log.csv` even if no refrigerant was added); refrigerant moisture and acidity test (where the OEM procedure specifies sampling); condenser and evaporator tube inspection (where the OEM procedure specifies, including eddy-current tube test at multi-year intervals — typically every 5 years for critical chillers, recorded in `chiller-service-record.csv` with the inspection result); electrical inspection of the motor terminal box, motor winding resistance (IR test, polarization index), and starter or VFD; functional test of every safety cutout (low-pressure, high-pressure, oil pressure differential, motor overload, freeze protection, surge protection); and a full controls test through the BMS interface (confirm every alarm that the BMS should receive is actually received).

### 6.2 Annual AHU Service

The annual AHU service is typically performed by site maintenance personnel or a service contractor. Scope includes: full filter replacement (record in `filter-replacement-log.csv`); coil cleaning (chemical cleaner per coil manufacturer, brushed if biological growth is present, then rinsed — confirm drain pan clears before reassembly); fan belt replacement (matched set, not individual belts); fan bearing grease (per OEM — over-greasing is as damaging as under-greasing; typical interval 6–12 months for ball bearings); fan motor electrical inspection (winding resistance, IR test); damper actuator inspection (confirm full stroke, confirm torque); coil freeze protection test (where fitted); and a full controls test through the BMS interface.

### 6.3 Annual Cooling Tower Service

The annual cooling tower service is typically performed by site maintenance personnel with a water treatment contractor. Scope includes: full fill inspection (clean or replace as needed); full drift eliminator inspection (clean or replace as needed); fan motor and bearing service (grease or oil per OEM); gearbox oil change (where fitted); gearbox inspection (gear wear, oil contamination); fan blade inspection (cracks, leading-edge erosion, balance); fan pitch adjustment (where adjustable); and water treatment system service (clean biocide day tank, calibrate dosing pumps, replace conductivity sensor if drifted).

The annual service also triggers a review of the water management plan under ASHRAE 188 / AS/NZS 3666 — confirm the plan is current, confirm the sampling and culture regime has been followed, confirm the biocide program is effective based on the previous 12 months of culture results.

### 6.4 Annual Pump and Fan Service

The annual pump and fan service includes: coupling inspection (replace coupling element if worn, laser align); mechanical seal inspection (replace if leaking); bearing inspection (replace if temperature or vibration trend indicates end-of-life); motor electrical inspection (winding resistance, IR test); motor bearing grease; and VFD inspection (clean cooling fan, inspect capacitors for swelling, confirm firmware version, back up parameters).

---

## 7. Multi-Year Special Inspections

### 7.1 Eddy Current Tube Inspection (Chillers)

Every 5 years (or per OEM specification), perform an eddy current tube inspection on the condenser and evaporator of every chiller. The test detects tube wall thinning, pitting, and cracking before failure. Each tube is tested; tubes with significant wall loss are plugged or replaced (depending on the number affected and the OEM guidance on plugging limits). The result is recorded in `chiller-service-record.csv` with the inspection report attached as a reference.

### 7.2 Vibration Spectrum Analysis (Pumps and Fans)

Every 2 years (or per OEM specification), perform a full vibration spectrum analysis on every pump and fan motor. The spectrum identifies the dominant vibration frequencies, which indicate the failure mode. A baseline spectrum taken at commissioning is the reference; significant deviation triggers a follow-up. The spectrum is stored as a PDF or CSV alongside the vibration readings.

### 7.3 Chiller Pressure Vessel Inspection

Every 5–10 years (depending on jurisdiction and pressure rating), the chiller's pressure vessels (condenser, evaporator, oil cooler) require a formal inspection per the local pressure equipment regulation. This inspection is typically performed by a third-party inspector and results in a certificate that must be retained for the life of the equipment. The certificate is filed in the maintenance office; an entry is made in `chiller-service-record.csv` referencing the certificate number and date.

---

## 8. Fault Response and Follow-Up

Any inspection finding that meets the fault criteria (Section 1.4) is recorded in `fault-corrective-log.csv` and immediate corrective action is taken. The fault record captures: the symptom (what was detected), the category (mechanical, electrical, control, refrigerant, water treatment), the severity (low/medium/high/critical), the root cause (if known at the time of detection; updated when the root cause analysis is complete), the immediate action (what was done to make the equipment safe), the corrective action (what was done to restore the equipment to service), and the parts used.

The fault record also captures the downtime hours (criticality for SLA reporting) and the recurrence count (the number of times the same fault has occurred on the same asset in the last 12 months — a recurrence count of 3 or more triggers a root cause analysis review). The `fdd_flag` field indicates whether the fault was detected by the BMS fault detection and diagnostics (FDD) routine or by a human — FDD-detected faults that recurred despite correction suggest the FDD logic is incomplete and should be reviewed.

---

## 9. Inspection Records Retention

Inspection records are retained per the site's recordkeeping policy. The minimum retention for regulatory evidence is typically 5 years (EU F-Gas), 5 years (ASHRAE 188 water management plan records), 7 years (most insurance audit requirements), or 10 years (some jurisdictions' pressure equipment records). See `recordkeeping-guide.md` for the recommended retention schedule.

Records should never be deleted to "clean up" the repository. The repository's git history is itself an audit trail; deletion of records destroys that trail. If a record was entered in error, it is corrected with a new entry that references the erroneous entry, not deleted.

---

## 10. Acceptance Criteria Summary

The acceptance criteria below are illustrative. Always defer to OEM manual and approved site procedure.

| Parameter | Class | Illustrative Limit | Source |
|-----------|-------|--------------------|--------|
| Condenser approach | Chiller | < 2.0 °C / 3.6 °F (water-cooled) | OEM manual |
| Evaporator approach | Chiller | < 1.5 °C / 2.7 °F | OEM manual |
| Compressor amps | Chiller | < 100% FLA | Nameplate |
| Bearing temperature | Pumps, fans | < 80 °C / 176 °F (DE & NDE) | OEM manual / ISO 10816 |
| Vibration (zone B/C boundary) | Pumps, fans | 2.8 mm/s rms (rigid mount) | ISO 10816 |
| Filter differential pressure | AHU | < 250 Pa / 1.0 in w.g. (typical changeout) | OEM manual |
| Cooling tower approach | CT | < 4 °C / 7.2 °F to wet bulb | OEM manual |
| Cooling tower conductivity | CT | Per water treatment program | Water treatment vendor |
| Cooling tower pH | CT | 7.5–8.5 (typical) | Water treatment program |
| CHW supply temperature | Chiller | Within ±0.5 °C of setpoint | Site procedure |
| Outdoor air damper position | AHU | Per ventilation design | ASHRAE 62.1 / site procedure |

These values are starting points; site-specific values must be established at commissioning and reviewed annually.
