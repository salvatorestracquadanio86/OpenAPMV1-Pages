---
layout: post
title: "WOM — Work Order Management: first look [EN]"
date: 2026-07-14
author: Salvatore Stracquadanio
tags: [WOM, Modules, MaintenanceOperations, WorkOrders, APM]
excerpt: "The second OpenAPM module is live. WOM gives you what you actually do with assets every day — work orders, maintenance tracking, downtime, and reports."
---

After shipping the Operational Asset Hub, the second module of OpenAPM is live: **WOM — Work Order Management**.

OAH gave you the asset registry. WOM gives you what you actually do with those assets every day — work orders, maintenance tracking, downtime, and reports. A modern, composable replacement for the maintenance side of legacy CMMS/EAM systems, without the implementation pain.

---

### Work Orders Dashboard

The central command center for the maintenance team. At a glance: how many work orders are active, how many are awaiting approval, in progress, or on hold. Date-based KPIs surface what's overdue, what's due this week, and what's planned for the month.

Two distribution charts — by status and by priority — give the supervisor an immediate health picture of the backlog. Quick Views let you drill directly into work orders by location or by equipment with a single click.

---

### Work Order Management

Creating a work order in WOM is a structured process, not a free-text ticket. You select the equipment from the asset registry (OAH integration out of the box), assign a type (corrective, preventive, inspection, project), set priority, schedule the intervention, and assign a team.

The lifecycle — from draft to approval to execution to closure — is tracked at every step. Intervention notes, measurements, and outcomes are captured on the same record.

---

### Maintenance Notes

Not every field activity deserves a full work order. Maintenance Notes are the lightweight counterpart: quick observations, lubrication rounds, visual checks, minor adjustments. They feed the same event timeline on the equipment record and appear in their own dashboard with the same KPI structure. The signal-to-noise ratio stays under control.

---

### Downtime Events

When a piece of equipment stops, someone needs to log it immediately and accurately. Downtime Events capture start time, end time, cause, and the affected production line. The data flows back to the equipment record and contributes to OEE and availability calculations in future dashboard iterations.

---

### Intervention Reports

Formal closure of a maintenance activity produces an Intervention Report: what was found, what was done, parts used, time spent, outcome. This is the document that feeds audits, insurance reviews, and the asset's maintenance history.

---

### Functional Documentation

Inline contextual documentation is available directly from the application menu — no external wiki needed. The same navigation panel pattern introduced in OAH is used here, so the team always has the operating manual one click away.

---

**What's next:** the Inspection module (INS) — checklist-based inspections, NDT campaigns, safety & compliance audits, and a dedicated finding catalog, all linked to the same asset registry.

OpenAPM is open-core and self-hostable.

*Follow the project: [Blog]({{ '/blog/' | relative_url }}) · [Modules]({{ '/modules/' | relative_url }})*
