---
layout: post
title: "INS — Inspection Module: from the field to the report [EN]"
date: 2026-07-20
author: Salvatore Stracquadanio
tags: [INS, Modules, Inspection, NDT, APM]
excerpt: "The INS module structures the inspection process from start to finish: planning, field data collection, anomaly classification, and integration with the asset event timeline."
---

In the lifecycle of an industrial asset, inspection is the moment when theoretical knowledge of the plant meets the physical reality of the machine. If carried out poorly — or left untracked — the risk of unexpected failures, regulatory non-compliance, and out-of-control maintenance costs grows silently.

**INS** structures this process from start to finish: from planning to the signed report, through field data collection, anomaly management, and integration with the rest of the platform.

---

## A clear lifecycle

Every inspection follows a precise status workflow:

**Draft → Planned → In Progress → On Hold → Completed → Closed**

The overall result is encoded in four values: **Pass**, **Fail**, **Conditional Pass**, **Not Applicable**.

---

## Eleven inspection types

Each type has its own specialized form, tailored to the information the inspector needs to collect in that specific context.

**1. Visual Inspection** — Surface condition assessment: corrosion, leaks, mechanical damage, guard integrity.

**2. Safety Compliance** — Verification against applicable safety regulations: protective devices, signage, PED or machinery directive compliance.

**3. Functional Test** — Operational test of primary functions: startup, work cycle, command response, nominal parameter achievement.

**4. Electrical Test** — Insulation measurements, earth resistance, circuit continuity, panel condition. Typical for motors, transformers, UPS systems.

**5. Vibration Analysis** — RMS values per axis (X, Y, Z) and dominant frequency on rotating machines, compared against alarm thresholds. Early warning for imbalances, bearing wear, alignment issues.

**6. Pressure Test** — Leak testing for pressurized systems. Records test pressure, nominal pressure, duration, and reference standard (EN 13480, ASME B31.3, PED).

**7. Environmental Inspection** — Temperature, humidity, dust, noise levels, corrosive agents. Relevant for ESG requirements and environmental audits.

**8. Thermographic Inspection** — Hot spot detection, thermal dispersion, insulation defects. Supports thermographic image uploads and temperature recording.

**9. Magnetic Particle Inspection (MPI)** — NDT for surface and sub-surface discontinuities in ferromagnetic materials. Includes NDT standard, magnetization type, lighting conditions.

**10. Dye Penetrant Inspection (DPI)** — NDT for open surface discontinuities on non-porous materials. Includes reference standard, penetrant type, indication classification.

**11. Checklist Inspection** — Structured control points compiled item by item in the field (OK / Not OK / Warning / N/A / Not Verified). Templates are defined per asset type — the system auto-populates the checklist based on the selected equipment.

---

## The finding catalog

Anomalies are classified by category (leaks, corrosion, mechanical issues, insulation deterioration, electrical anomalies, alignment problems) and severity (Low / Medium / High / Critical), each linked to a recommended action. The inspection closes not just with a numeric result, but with immediately actionable operational information.

---

## From the field to maintenance

When an inspection closes with a **Fail**, the system prompts the creation of a Maintenance Note — one click, from the inspector's report to the work order.

Every completed inspection feeds automatically into the **asset event timeline**. Over time, this trace becomes a tool for analyzing operating condition drift, planning preventive maintenance, and supporting replacement decisions.

---

## Inspection triggers

OpenAPM tracks the origin of every inspection:

- **Manual** — created by the operator or manager
- **From Work Order** — arising from WO completion
- **From Maintenance Plan** — scheduled by SCHED
- **From Alarm** — requested after a condition monitoring alert
- **Regulatory** — mandatory periodic verification

Tracking the trigger reveals which anomalies emerge from active monitoring versus which are only discovered during scheduled rounds — a data point that guides instrumentation decisions over time.

---

## Reports

Every inspection produces a PDF report directly from the platform: asset details, inspector, dates, measurements, findings, overall result. Attached to the record for full traceability.

---

The INS module was built for people who perform inspections in the field every day and need tools that reduce compilation time, structure information usefully, and don't leave it scattered across Excel sheets or PDFs disconnected from the asset's history.

Because a well-executed inspection that leaves no structured trace is worth almost nothing.

*Follow the project: [Blog]({{ '/blog/' | relative_url }}) · [Modules]({{ '/modules/' | relative_url }}) · [Install]({{ '/install/' | relative_url }})*
