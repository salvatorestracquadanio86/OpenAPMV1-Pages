---
layout: post
title: "OAH — Operational Asset Hub: first look [EN]"
date: 2026-07-01
author: Salvatore Stracquadanio
tags: [OAH, Modules, APM, Screenshots]
excerpt: "The first OpenAPM module is done. OAH — Operational Asset Hub — is the system's foundation: plant hierarchy, asset registry, event timeline, document management. Here is what it looks like."
---

The first OpenAPM module is done.

**OAH — Operational Asset Hub** is the system's foundation: plant hierarchy, asset registry, event timeline, document management. All working.

In the coming weeks it will be available via Docker for anyone who wants to try it. Here is what it looks like.

---

### Location management

<img src="{{ '/assets/oah_screenshots/LocationsManagement.png' | relative_url }}" alt="OAH — Location Management" style="width:100%;border-radius:10px;margin:1rem 0;">

Every plant has its own structure — facilities, departments, areas, units. OAH models it freely, without imposing fixed schemas, while still guiding users towards good management practices.

---

### The tree view

<img src="{{ '/assets/oah_screenshots/LocatioNtree.png' | relative_url }}" alt="OAH — Location Tree" style="width:100%;border-radius:10px;margin:1rem 0;">

The plant hierarchy the way you actually think about it: expand, navigate, add new nodes directly from the tree.

---

### Visual plant map

<img src="{{ '/assets/oah_screenshots/PlantEquipmentMap.png' | relative_url }}" alt="OAH — Plant Equipment Map" style="width:100%;border-radius:10px;margin:1rem 0;">

This answers a question I've encountered everywhere: *"where exactly is this asset?"* Position equipment on the plant using floor plans or photographs. Drag and drop — nothing more.

---

### Equipment list

<img src="{{ '/assets/oah_screenshots/EquipmentList.png' | relative_url }}" alt="OAH — Equipment List" style="width:100%;border-radius:10px;margin:1rem 0;">

Your entire asset base at a glance, with operational status, criticality, and position in the hierarchy.

---

### Technical record

<img src="{{ '/assets/oah_screenshots/EquipmentDetails.png' | relative_url }}" alt="OAH — Equipment Details" style="width:100%;border-radius:10px;margin:1rem 0;">

General information valid for every asset, and type-specific fields that adapt to the equipment type. No generic form — but all the information you actually need.

---

### Functional documentation

<img src="{{ '/assets/oah_screenshots/Docs.png' | relative_url }}" alt="OAH — Module Documentation" style="width:100%;border-radius:10px;margin:1rem 0;">

Not just a manual: contextual guidance that accompanies the user while working — explaining what fields mean, the reasoning behind certain choices, and how to correctly read the displayed data. The goal is that anyone can use the system well, even without specific training.

---

### Event timeline

<img src="{{ '/assets/oah_screenshots/EquipmentEventTimeline.png' | relative_url }}" alt="OAH — Event Timeline" style="width:100%;border-radius:10px;margin:1rem 0;">

Failures, interventions, inspections, anomalies, notes — the full history of an asset in a single chronology. Built automatically by the connected modules, with the option to manually log events at any time.

---

All features are developed and working. This is not a prototype — it is the real module, the one everything else will be built on.

OAH is now in refinement. The next module is starting.

*Follow the project: [Blog]({{ '/blog/' | relative_url }}) · [Modules]({{ '/modules/' | relative_url }})*
