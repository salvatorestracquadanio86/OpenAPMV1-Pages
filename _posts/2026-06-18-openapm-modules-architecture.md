---
layout: post
title: "How I structured the OpenAPM modules — and why"
date: 2026-06-18
author: Salvatore Stracquadanio
tags: [Architecture, Modules, APM, Maintenance]
excerpt: "Behind every APM platform there is a fundamental question: where do you start? In this post I share how I structured the OpenAPM modules, the choices I made, and the values that guided them."
---

In my first post I talked about the why behind OpenAPM: the gap between the tools available to large organizations and those accessible to SMEs. I described the philosophy — openness, simplicity, gradual adoption — without yet going into the specifics of what the system will actually do.

Today I want to take a step forward and share how I have thought about structuring the application modules, what processes they cover, and above all why I made certain choices rather than others.

## Everything starts from the asset

The first question I asked myself was: what is the centre of gravity of the system?

The answer varies depending on the application focus and the level of vertical specialisation: for some platforms it is the work order, for others a solid inspection system, for others still the plant hierarchy. In my vision, the centre is the asset.

OpenAPM will be an "asset-centric" system. Every event, every intervention, every document, every measurement must be traceable back to a specific asset, located in a precise place, with its own history.

From this conviction I designed the first module: **OAH — Operational Asset Hub**.

OAH is the heart of the system, included in every configuration. It manages three fundamental things: the location hierarchy (plants, departments, areas, units — freely modellable for any industry), the equipment registry with its technical characteristics, and the event timeline. Everything that happens on an asset (a failure, an intervention, an inspection, an anomaly) is recorded chronologically. This register is the memory of the asset: it allows you to understand how it has behaved over time, which problems have recurred, who did what and when.

## WOM — Work Order Management

The second module in development order will be **WOM — Work Order Management**.

I wanted this module to faithfully represent how maintenance actually works in practice. The typical flow starts either from scheduled activities or from a field observation: an operator notices an anomaly and records it in the system, describing what they saw and proposing an action. This report is reviewed by a designated person (maintenance engineers, inspectors, etc.), who decides whether to open a formal intervention. If a work order is opened, the system guides the user through the entire cycle: planning, approval, assignment, execution, documentation, closure. Each transition automatically updates the status of the asset. The module also tracks machine downtime, which is the primary data source for calculating availability and reliability.

## INS — Inspection Module

The third module will be **INS — Inspection Module**.

Inspections are a distinct process from maintenance and OpenAPM treats them separately, because they have their own logic. INS manages one-shot inspections on individual assets: visual, structured checklist, thermographic, vibration analysis, ultrasonic thickness measurement, pressure testing, regulatory compliance, and more.

The feature I care most about is adaptive guidance. The inspection form changes based on the type of inspection being performed. A vibration analysis shows a grid for measurement points with values and ISO reference thresholds. An ultrasonic thickness inspection shows a point map with comparison against the minimum acceptable wall thickness. You do not need to be an expert to understand what to fill in: the system guides and provides context. If the result is negative, the module can automatically generate a work order in WOM.

## SCHED — Scheduling & Planning

The fourth module will be **SCHED — Scheduling & Planning**.

Performing maintenance "when you remember" is not a strategy. SCHED introduces recurring planning: preventive maintenance plans and inspection plans, with configurable recurrence rules. The design choice I want to highlight is that of adaptive templates: OpenAPM will come pre-loaded with standard plans for the most common asset types — pumps, motors, compressors, UPS, transformers. Someone starting from scratch will find a sensible foundation already in place, calibrated on industry best practices that are by now well-established literature. Recurrence triggers are not only time-based: the system will also support triggers based on runtime counters (e.g. every 500 operating hours) or on asset health conditions (e.g. when vibration exceeds a threshold).

## AIM — Asset Integrity Monitoring

The fifth module is **AIM — Asset Integrity Monitoring**.

AIM transforms data into knowledge, built on three pillars. The first is the risk matrix: for each asset, a guided wizard leads the user through an assessment of failure probability and consequences — for safety, the environment, operational continuity, costs, and regulatory compliance. The output is a criticality classification that automatically updates the asset in OAH. The second pillar is the monitoring of operating conditions through KPIs (flow rate, pressure, temperature, vibrations, etc.) with configurable thresholds and automatic updates to the asset health status. The third pillar is reliability statistics — MTBF, MTTR, availability — calculated automatically from data already present in the system, with no additional data entry required.

## LOG — Digital Logbook

The sixth module is **LOG — Digital Logbook**.

It is the simplest, but it was born from a real problem I have seen in many plants: the handover between operator shifts in the field. In environments with continuous shift rotation, information that is not correctly transmitted generates delays, duplicated work, and sometimes incidents. The most common solution? A paper sheet or a WhatsApp group. LOG introduces a structured digital register, contextualised to the asset or plant, with a shift handover workflow: the outgoing shift leader selects the notes to pass on and writes a summary; the incoming shift leader receives them and with a single click can turn any note into a maintenance report in WOM. LOG is included in the baseline tier because it represents one of the most immediate entry points towards a culture of operational traceability.

Once this foundation is in place, it will be possible to think about integration with existing ERP systems and more advanced modules addressing topics with more vertical-specific characteristics.

## How they connect with each other

The real strength of these modules emerges from their integration. An anomaly detected by a sensor (AIM) updates the asset health status (OAH) and can generate a work order (WOM), which upon completion feeds the reliability statistics (AIM). An operator notices something during a shift, writes it in the logbook (LOG), the shift leader escalates it to WOM, the technician intervenes, and the asset timeline updates automatically. These flows do not require complex configurations: they are the natural behaviour of modules designed to work together from the very beginning.

## Three recurring principles behind every choice

**Guide, do not just collect data.** At every critical point there is a mechanism that helps the user: the risk assessment wizard, the adaptive inspection forms, the plan templates, the checklists. The goal is not to have a database of records, but to help people do things well even without being experts in the process.

**Start from what already exists.** No organisation starts from zero. OpenAPM is designed to gradually incorporate the existing reality, not to replace it with a system imposed from above.

**Traceability as a value, not as bureaucracy.** Every action in the system generates a trace. Not to monitor people, but to build the memory of the plant — a memory that over time becomes the most valuable asset of any operational organisation.

---

I will begin implementing the Operational Asset Hub module tomorrow. For each completed module, you will be able to download and install the application via Docker.

If you manage assets, maintenance, inspections, or operational shifts — in any industry — I would love to understand:

- Which of these processes is most problematic in your organisation today?
- Do you already use a dedicated tool, or do you work mainly with spreadsheets and informal procedures?
- Is there something you have always wanted from an APM tool and never found?

I am available via the [contact page]({{ '/contact/' | relative_url }}) or directly on LinkedIn.
