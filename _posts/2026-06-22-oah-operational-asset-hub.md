---
layout: post
title: "OAH — Operational Asset Hub: the foundation of OpenAPM [EN]"
date: 2026-06-22
author: Salvatore Stracquadanio
tags: [OAH, Modules, AssetManagement, APM]
excerpt: "Before planning maintenance, opening work orders, or analysing risks, you need to know what you have, where it is, and what has happened to it. OAH is the module designed to answer that question — and here is how I designed it."
---

<img src="{{ '/assets/oah.png' | relative_url }}" alt="OAH — Operational Asset Hub" style="width:100%;border-radius:12px;margin-bottom:2rem;">

In my previous posts I talked about the why behind OpenAPM and how I imagined structuring its modules.
Today I want to tell you about the first one — the foundation, the one everything else rests on: **OAH, Operational Asset Hub**.

It is the base module included in every configuration. Without it, no other module makes sense, because everything in OpenAPM revolves around the physical asset: the pump, the motor, the electrical panel, the production machine. Before planning maintenance, opening work orders, or analysing risks, you need to know what you have, where it is, and what has happened to it.

Seems obvious. It is not.

In many organisations this information simply does not exist in any structured form, or it is completely fragmented — one Excel sheet for department A, another for department B, a list in some management system that nobody has updated in years.

---

## What does the plant look like? The location hierarchy.

A manufacturing plant has facilities, departments, lines, units. A hospital has buildings, floors, wards, rooms. A photovoltaic plant has sites, fields, strings. A data centre has rooms, racks, slots.

OAH will allow this structure to be modelled freely: each node has a type, a code, a name, and a link to the node above it. No fixed schema imposed by the system — the organisation builds its own map as it actually is, with minimal constraints that nonetheless ensure coherence and navigability over time.

The structure can be built through a standard form or through an interactive tree view, where nodes are added directly from the tree without opening separate forms.

But the feature I am most excited about is the one I am designing as the next step: a graphical mode for drawing the asset map on the plant. You load a floor plan or a photograph of the area, and drag and drop assets into their respective positions. The result is a navigable visual representation — something that today exists only in high-end systems, and that I want to make available even for those who, more often than you might think, struggle to give a structured shape to their plant reality.

---

## Assets: a record that adapts to the equipment type.

Each asset is registered with the basic information — code, position, type, category, criticality — but also with the technical specifications specific to that type of equipment. A pump has different characteristics from a motor, which has different ones from a UPS.

The system will recognise the type of asset being registered and show only the relevant fields. Whoever registers a pump sees flow rate, pressure, power. Whoever works on a UPS sees capacity, autonomy, voltage. No unnecessary fields, no generic form that covers everything for everyone.

This is not just a usability detail: it means that meaningful searches will be possible later — *"all motors with power above 30 kW in the production area"* — because technical data will have been collected in a structured way from the very beginning.

---

## The event timeline: the asset's memory.

This is the feature I consider the true heart of the module.

Every asset will have a history: a chronological sequence of everything that has happened to it — failures, interventions, inspections, anomalies, measurements, operational notes. During the initial setup phase it will be possible to manually enter past events with general information. Once up and running, every OpenAPM module will automatically update the timeline whenever something relevant occurs. The user does not need to remember to log it: the system does it for them.

The result is a living memory that answers a question that is deceptively simple but often incredibly hard: *"what happened to this pump over the last two years?"* Today that answer lives with the senior technician who was there. When that person leaves, the answer leaves with them.

---

## Documents: always tied to the asset.

I chose not to create a separate document management module. Manuals, certificates, drawings, datasheets, procedures — every document is always linked to a specific asset. It makes sense for it to live inside OAH.

Document management will be organised on two levels: operational attachments (photos of a failure, intervention reports — immutable, tied to the moment they were produced) and a technical document library (versioned, with tracked expiry dates). For this second category, the system will flag which mandatory documents are missing or expired for each asset type — a concrete feature for anyone operating in environments subject to audits or certifications.

---

## How do you bring in the existing asset register?

The most immediate answer is the one everyone knows: an Excel file. The plan is to provide a downloadable template for locations and equipment, with guided fields and validation on import.

It is convenient. It is quick. Everyone knows how to use Excel. But it is also the riskiest path: one wrong field, a poorly defined hierarchy, a duplicate code — and you end up with a register to fix record by record. Most of these errors are not discovered on day one, but three months later, when someone looks for something and cannot find it where it should be.

This is why my recommendation will be to start with the guided mode: a minimal set of fields, structure built step by step, progressively enriched. For large plants, I know this is not realistic — and there, Excel remains the only practical option.

And this is exactly where I want to open a conversation: how have you managed the initial asset census when you introduced a new system? Have you found alternatives to Excel? Are there integrations with ERP or management systems you consider more reliable? Any real-world experience is welcome.

---

OAH is not an asset database. It is a structured answer to a problem every operational organisation knows: information about assets exists, but it is scattered, informal, and often locked inside the memory of a few people.

The free hierarchy, the adaptive record, the visual map, the automatic timeline — these are not isolated features. They are the same idea expressed in different ways: the system must speak the language of the people who work on the plant floor, not the other way around.

OAH will be the first releasable MVP of OpenAPM. Future posts will track its development progress.

*Follow the project: [Blog]({{ '/blog/' | relative_url }}) · [Modules]({{ '/modules/' | relative_url }})*
