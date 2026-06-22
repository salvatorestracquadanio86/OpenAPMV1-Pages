---
layout: post
title: "OAH — Operational Asset Hub: le fondamenta di OpenAPM [IT]"
date: 2026-06-22
author: Salvatore Stracquadanio
tags: [OAH, Moduli, GestioneAsset, APM]
excerpt: "Prima di pianificare manutenzioni, aprire interventi o analizzare rischi, bisogna sapere cosa c'è, dove si trova, e cosa gli è successo. OAH è il modulo che risponde a questa esigenza — e in questo articolo racconto come l'ho progettato."
---

<img src="{{ '/assets/oah.png' | relative_url }}" alt="OAH — Operational Asset Hub" style="width:100%;border-radius:12px;margin-bottom:2rem;">

Nei post precedenti ho parlato del perché di OpenAPM e di come ho immaginato di strutturarne i moduli.
Oggi voglio raccontarvi il primo — le fondamenta, quello su cui si appoggia tutto il resto: **OAH, Operational Asset Hub**.

È il modulo base incluso in ogni configurazione. Senza di esso nessun altro modulo ha senso, perché tutto in OpenAPM ruota attorno all'asset fisico: la pompa, il motore, il quadro elettrico, il macchinario di linea. Prima di pianificare manutenzioni, aprire interventi o analizzare rischi, bisogna sapere cosa c'è, dove si trova, e cosa gli è successo.

Sembra ovvio. Non lo è.

In molte realtà questa informazione non esiste in forma strutturata, o è assolutamente frammentata — un foglio Excel per il reparto A, un altro per il reparto B, una lista in qualche gestionale che nessuno aggiorna più.

---

## Come è fatto l'impianto? La gerarchia dei luoghi.

Un'industria manifatturiera ha stabilimenti, reparti, linee, unità. Un ospedale ha edifici, piani, reparti, stanze. Un impianto fotovoltaico ha siti, campi, stringhe. Un datacenter ha sale, rack, slot.

OAH permetterà di modellare questa struttura liberamente: ogni nodo ha un tipo, un codice, un nome, e si collega al nodo superiore. Nessuno schema fisso imposto dal sistema — l'organizzazione costruisce la propria mappa così com'è nella realtà, con vincoli minimi che garantiscono però coerenza e navigabilità nel tempo.

La struttura potrà essere costruita tramite form classico o tramite una vista ad albero interattiva, in cui si aggiungono nodi direttamente dall'albero senza aprire form separati.

Ma la funzionalità che mi entusiasma di più è quella che sto progettando come passo successivo: una modalità grafica per disegnare la mappa degli asset sull'impianto. Si carica una planimetria o una fotografia dell'area, e si trascinano gli asset nelle rispettive posizioni con un drag & drop. Il risultato è una rappresentazione visuale navigabile — qualcosa che oggi esiste solo nei sistemi di fascia alta, e che voglio rendere disponibile anche per chi, più spesso di quanto si pensi, fatica a dare una forma strutturata alla propria realtà di impianto.

---

## Gli asset: una scheda che si adatta al tipo.

Ogni asset viene censito con le informazioni di base — codice, posizione, tipo, categoria, criticità — ma anche con le specifiche tecniche proprie di quel tipo di equipment. Una pompa ha caratteristiche diverse da un motore, che ne ha diverse da un UPS.

Il sistema riconoscerà il tipo di asset che si sta censendo e mostrerà solo i campi rilevanti. Chi censisce una pompa vede portata, pressione, potenza. Chi lavora su un UPS vede capacità, autonomia, tensione. Nessun campo inutile, nessun form generico che vale a 360 gradi per tutti.

Non è solo un dettaglio di usabilità: significa che sarà possibile fare ricerche significative — *"tutti i motori con potenza superiore a 30 kW in area produzione"* — perché i dati tecnici saranno stati raccolti in modo strutturato fin dall'inizio.

---

## La timeline degli eventi: la memoria dell'asset.

Questa è la funzionalità che considero il vero cuore del modulo.

Ogni asset avrà una storia: una sequenza cronologica di tutto quello che gli è capitato — guasti, interventi, ispezioni, anomalie, misurazioni, note operative. In fase di avvio sarà possibile inserire manualmente gli eventi pregressi con informazioni di carattere generale. A regime, ogni modulo di OpenAPM aggiornerà automaticamente la timeline quando accade qualcosa di rilevante. L'utente non deve ricordarsi di registrare: il sistema lo fa per lui.

Il risultato è una memoria viva che risponde a una domanda semplicissima ma spesso difficilissima: *"cosa è successo su questa pompa negli ultimi due anni?"* Oggi quella risposta la ha il tecnico senior che c'era. Quando quella persona va via, la risposta va via con lei.

---

## I documenti: sempre legati all'asset.

Ho scelto di non creare un modulo documentale separato. Manuali, certificati, schemi, datasheet, procedure — ogni documento è sempre legato a un asset specifico. Ha senso che viva dentro OAH.

La gestione sarà su due livelli: allegati operativi (foto di un guasto, report di un intervento — immutabili, legati al momento) e libreria documenti tecnici (versionata, con scadenze tracciate). Per questa seconda categoria, il sistema segnalerà quali documenti obbligatori mancano o sono scaduti per ogni tipo di asset — una funzionalità concreta per chi opera in contesti soggetti a verifiche e certificazioni.

---

## Come si porta dentro il censimento esistente?

La risposta più immediata è quella che tutti conoscono: un file Excel. Il piano è di fornire un template scaricabile con campi guidati e validazione all'importazione.

È comodo. È immediato. Chiunque sa usare Excel. Ma è anche la strada più rischiosa: un campo sbagliato, una gerarchia mal definita, un codice duplicato — e ci si ritrova con un censimento da correggere record per record. La maggior parte di questi errori non vengono scoperti il primo giorno, ma tre mesi dopo, quando qualcuno cerca qualcosa e non lo trova.

Per questo la mia raccomandazione sarà di partire dalla modalità guidata: set minimo di campi, struttura costruita passo dopo passo, arricchimento progressivo. Per impianti grandi, so bene che non è realistico — e lì Excel resta l'unica alternativa pratica.

Ed è qui che apro un confronto: come avete gestito il censimento iniziale quando avete introdotto un nuovo sistema? Avete trovato approcci alternativi? Ci sono integrazioni con gestionali o ERP che considerate più affidabili? Qualsiasi esperienza vissuta è benvenuta.

---

OAH non è un database di asset. È la risposta strutturata a un problema che ogni organizzazione operativa conosce: le informazioni sugli asset esistono, ma sono disperse, informali, spesso intrappolate nella memoria di poche persone.

La gerarchia libera, la scheda adattiva, la mappa visuale, la timeline automatica — non sono funzionalità isolate. Sono la stessa idea declinata in modi diversi: il sistema deve parlare il linguaggio di chi lavora in impianto, non il contrario.

OAH sarà la prima MVP rilasciabile di OpenAPM. Nei prossimi post aggiornerò lo stato di avanzamento dello sviluppo.

*Seguire l'avanzamento del progetto: [Blog]({{ '/blog/' | relative_url }}) · [Moduli]({{ '/modules/' | relative_url }})*
