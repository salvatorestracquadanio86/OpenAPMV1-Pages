---
layout: post
title: "Come ho strutturato i moduli di OpenAPM — e perché [IT]"
date: 2026-06-18
author: Salvatore Stracquadanio
tags: [Architettura, Moduli, APM, Manutenzione]
excerpt: "Dietro ogni piattaforma APM c'è una domanda fondamentale: da dove si comincia? In questo articolo racconto come ho strutturato i moduli di OpenAPM, le scelte che ho fatto e i valori che le guidano."
---

Nel [primo articolo]({{ '/blog/2026/06/17/introducing-openapm/' | relative_url }}) ho parlato del perché esiste OpenAPM: il divario tra gli strumenti disponibili per le grandi organizzazioni e quelli accessibili alle PMI. Ho descritto la filosofia — apertura, semplicità, gradualità — senza ancora entrare nel merito di cosa il sistema farà concretamente.

Oggi voglio fare un passo avanti. Voglio raccontare come ho strutturato i moduli di OpenAPM, quali processi coprono, e soprattutto perché ho fatto certe scelte piuttosto che altre.

---

## Tutto parte dall'asset

La prima domanda che mi sono posto quando ho iniziato a progettare OpenAPM è stata: **qual è il centro di gravità del sistema?**

In molte piattaforme APM che ho utilizzato negli anni, la risposta varia: per alcune è il work order, per altre è l'ispezione, per altre ancora è la gerarchia di impianto. Ognuna di queste scelte porta a una visione del mondo leggermente diversa.

La mia risposta è stata netta: il centro è l'**asset**.

Tutto in OpenAPM ruota intorno all'asset fisico — la pompa, il motore, il quadro elettrico, il server, il macchinario di produzione. Ogni evento, ogni intervento, ogni documento, ogni misurazione deve essere riconducibile a un asset specifico, collocato in un luogo preciso, con una storia propria.

Da questa convinzione è nato il primo modulo.

---

## OAH — Operational Asset Hub

L'**OAH** è il cuore del sistema. È il modulo incluso in ogni configurazione di OpenAPM, perché senza un censimento strutturato degli asset tutto il resto non ha senso.

OAH gestisce tre cose fondamentali:

**La gerarchia dei luoghi.** Ogni impianto ha una struttura: stabilimenti, reparti, aree, unità. OAH permette di modellarla liberamente, senza imporre uno schema fisso. Un'industria manifatturiera organizzerà i propri siti in modo diverso da un ospedale o da un datacenter. Il sistema si adatta, non il contrario.

**Il censimento degli equipment.** Ogni asset ha un codice, una posizione nella gerarchia, un tipo, una categoria, un livello di criticità. Ma ha anche caratteristiche tecniche specifiche — la portata di una pompa, la potenza di un motore, la capacità di un UPS — che in OAH sono gestite in modo flessibile per tipo di asset, senza richiedere di creare entità separate per ogni variante.

**La timeline degli eventi.** Ogni cosa che succede su un asset — un guasto, un intervento, un'ispezione, un'anomalia osservata, una misurazione — viene registrata in una timeline cronologica. Questo registro è la memoria dell'asset: permette di capire come si è comportato nel tempo, quali problemi si sono ripetuti, chi ha fatto cosa e quando.

OAH è anche il punto di arrivo delle informazioni prodotte dagli altri moduli. Quando un work order viene chiuso, quando un'ispezione viene completata, quando viene rilevata un'anomalia — tutto confluisce nella timeline dell'asset. È un registro vivo, non un archivio statico.

---

## WOM — Work Order Management

Il secondo modulo in ordine di implementazione è il **WOM**, che copre l'intero ciclo operativo della manutenzione.

Ho voluto che questo modulo rappresentasse fedelmente come funziona la manutenzione nella realtà, senza semplificare troppo né complicare inutilmente.

Il flusso tipico inizia con una **segnalazione di campo**: un operatore osserva un'anomalia, una perdita, un rumore anomalo. Non apre immediatamente un work order — segnala quello che ha visto, descrive dove si trova, propone un'azione. Questa segnalazione viene poi esaminata da un analista o da un responsabile, che decide se aprire un intervento formale o se risolvere la questione direttamente.

Se si apre un **work order**, il sistema guida l'utente attraverso tutto il ciclo: pianificazione, approvazione, assegnazione al tecnico (interno o ditta esterna), esecuzione, documentazione del lavoro svolto, chiusura. Ogni transizione di stato ha effetti precisi sull'asset: quando un intervento inizia, lo stato operativo dell'equipment si aggiorna; quando termina, l'evento viene registrato nella timeline OAH.

Il WOM gestisce anche la **documentazione dell'intervento** — non solo un campo note, ma un rapporto strutturato con sezioni dedicate: lavoro eseguito, ricambi utilizzati, misurazioni effettuate, raccomandazioni per il futuro. E traccia i **fermi macchina**, che sono la fonte dati primaria per calcolare disponibilità e affidabilità.

C'è anche una predisposizione per l'integrazione con sistemi ERP esterni. Non tutti i clienti ne avranno bisogno, ma chi utilizza già un sistema gestionale per gli acquisti o la contabilità potrà sincronizzare i work order senza doppie registrazioni.

---

## INS — Inspection Module

Le ispezioni sono un processo distinto dalla manutenzione, anche se spesso finiscono nello stesso "cassetto". OpenAPM le tratta separatamente, perché hanno logiche proprie: un'ispezione ha un obiettivo definito, un metodo, uno standard di riferimento, e un risultato formale.

Il modulo **INS** gestisce le **ispezioni one-shot** su un singolo asset: visive, con checklist strutturate, termografiche, vibrazionali, di spessore ultrasonico, di pressione, di conformità normativa, e molte altre.

La caratteristica che mi sta più a cuore in questo modulo è la **guida adattiva**. Il form di ispezione cambia in base al tipo di ispezione che si sta eseguendo. Un'ispezione visiva mostra campi per le osservazioni e le fotografie. Un'analisi vibrazioni mostra una griglia per i punti di misura con valori, unità e soglie ISO di riferimento. Una spessimetria mostra una mappa di punti con misurazioni e confronto con lo spessore minimo ammissibile.

Non è necessario essere esperti di NDT per capire cosa compilare: il sistema guida, spiega cosa inserire, e fornisce il contesto normativo di riferimento per chi lo vuole.

Se il risultato è negativo, il modulo può generare automaticamente un work order in WOM. La catena ispezione → anomalia → intervento non richiede nessuna azione manuale aggiuntiva.

---

## SCHED — Scheduling & Planning

Fare manutenzione e ispezioni "quando si ricorda" non è una strategia. Eppure è quello che accade in molte organizzazioni che non hanno un sistema strutturato.

Il modulo **SCHED** introduce la pianificazione ricorrente: piani di manutenzione preventiva e piani ispettivi, con regole di ricorrenza configurabili per frequenza, tipo di trigger e asset coinvolti.

La scelta progettuale che mi preme sottolineare è quella dei **template**. OpenAPM viene pre-fornito con piani standard per i tipi di asset più comuni — pompe, motori, compressori, UPS, trasformatori, server. Chi parte da zero non deve costruire da zero: trova già una base sensata su cui lavorare, calibrata su best practice di settore, e la personalizza in base alla propria realtà.

I trigger di ricorrenza non sono solo temporali. Oltre all'intervallo fisso ("ogni 30 giorni"), SCHED supporta trigger basati su contatori di esercizio ("ogni 500 ore di funzionamento"), su condizioni di salute rilevate dal modulo AIM ("quando la vibrazione supera la soglia"), o su eventi OAH ("dopo ogni guasto, esegui un'ispezione").

Questa flessibilità permette di avvicinarsi alla manutenzione condition-based in modo graduale, senza richiedere fin dall'inizio un'infrastruttura IoT completa.

Il modulo include anche la previsione di budget: sapere in anticipo quante attività sono pianificate nei prossimi mesi, e quanto costano stimativamente, è un'informazione preziosa per chi gestisce risorse limitate.

---

## AIM — Asset Integrity Monitoring

L'**AIM** è il modulo che trasforma i dati in conoscenza.

Si articola su tre pilastri.

Il primo è la **matrice di rischio**. Per ogni asset, il sistema guida l'utente attraverso una valutazione strutturata: quanto è probabile che questo asset si guasti? Quali sarebbero le conseguenze — per la sicurezza delle persone, per l'ambiente, per la continuità operativa, per i costi, per la conformità normativa?

L'output è un punteggio di rischio che classifica ogni asset in quattro categorie: basso, medio, alto, critico. Questa classificazione aggiorna automaticamente la criticità dell'asset in OAH, e può influenzare le frequenze di manutenzione in SCHED.

La novità rispetto a molti approcci che ho visto è il **wizard guidato**: il sistema non chiede all'utente di inserire direttamente i punteggi P e C, ma pone domande contestuali calibrate per il tipo di asset e il dominio aziendale. Chi non è un esperto di risk assessment può comunque produrre una valutazione strutturata e ripetibile.

Il secondo pilastro è il **monitoraggio delle condizioni operative** tramite KPI: portata, pressione, temperatura, vibrazioni, corrente assorbita, fattore di prestazione. Ogni tipo di asset ha i propri KPI di riferimento, con soglie configurabili su tre livelli — normale, warning, critico. Quando un valore supera una soglia, lo stato di salute dell'asset si aggiorna automaticamente in OAH.

Il terzo pilastro sono le **statistiche di affidabilità**: MTBF, MTTR, disponibilità, frequenza di guasto. Calcolate automaticamente dai dati già presenti nel sistema — gli eventi OAH, i work order completati — senza nessuna registrazione aggiuntiva. Questi indicatori alimentano il wizard di risk assessment e forniscono la base per ottimizzare le frequenze dei piani di manutenzione.

---

## LOG — Digital Logbook

L'ultimo modulo del baseline è il più semplice, ma nasce da un problema reale che ho visto in molti impianti: il passaggio di consegne tra turni.

In ambienti con turnazione continua — industria, ospedali, datacenter, facility 24/7 — le informazioni che non vengono trasmesse correttamente da un turno all'altro generano ritardi, doppioni di lavoro, e talvolta incidenti. La soluzione più comune? Un foglio cartaceo sul bancone o un gruppo WhatsApp. Nessuna tracciabilità, nessun collegamento agli asset, nessuna possibilità di ricerca.

Il modulo **LOG** introduce un registro digitale strutturato. Ogni nota è contestualizzata a un asset o a un'area, ha una categoria (anomalia, lavoro in corso, nota operativa, segnalazione di sicurezza), e uno stato nel workflow di passaggio consegne.

Il capoturno uscente seleziona le note da trasmettere al turno successivo e scrive un summary. Il capoturno entrante le riceve, le legge, e con un click può scalare qualunque nota a WOM — creando automaticamente una segnalazione di manutenzione pre-compilata.

Il LOG è incluso nel tier gratuito perché rappresenta uno dei punti di ingresso più semplici verso una cultura di tracciabilità operativa. Non richiede processi formali preesistenti: si inizia a usarlo domani mattina.

---

## Come si collegano tra loro

Ho descritto i moduli uno alla volta, ma la loro forza reale emerge dall'integrazione.

Un'anomalia rilevata da un sensore (AIM) aggiorna la salute dell'asset (OAH) e può generare un'occorrenza anticipata nel piano di manutenzione (SCHED), che genera un work order (WOM), che al completamento alimenta le statistiche di affidabilità (AIM).

Un operatore nota qualcosa di strano durante il turno, scrive una nota nel logbook (LOG). Il capoturno la scala a una segnalazione WOM. L'analista apre un work order. Il tecnico esegue l'intervento e chiude il work order, documentando causa e azioni. La timeline dell'asset (OAH) si aggiorna automaticamente.

Un piano ispettivo annuale (SCHED) genera un'ispezione (INS) su tutti i serbatoi in pressione di un impianto. L'ispettore esegue le spessimetrie, registra i valori, e su uno dei serbatoi trova un valore sotto la soglia minima. Il sistema genera automaticamente un work order correttivo (WOM) e aggiorna il risk assessment del serbatoio (AIM).

Questi flussi non richiedono configurazioni complesse o integrazioni custom. Sono il comportamento naturale di moduli progettati per lavorare insieme fin dall'inizio.

---

## La filosofia dietro le scelte

Rileggendo quello che ho scritto, mi rendo conto che alcune scelte ricorrono in tutti i moduli. Vale la pena esplicitarle.

**Guidare, non solo raccogliere dati.** In ogni punto critico del sistema c'è un meccanismo che guida l'utente: il wizard di risk assessment, i form adattativi delle ispezioni, i template dei piani di manutenzione, le checklist. L'obiettivo non è avere un database di record compilati, ma aiutare le persone a fare le cose bene, anche quando non sono esperti del processo.

**Partire da ciò che esiste.** Non esiste un'organizzazione che parte da zero. Ci sono già asset da qualche parte, già interventi in corso, già conoscenza nelle teste delle persone. OpenAPM è progettato per inglobare gradualmente questa realtà, non per sostituirla con un sistema calato dall'alto.

**La tracciabilità come valore, non come burocrazia.** Ogni azione nel sistema genera una traccia. Non per controllare le persone, ma per costruire la memoria dell'impianto. Quella memoria, nel tempo, diventa il patrimonio più prezioso di un'organizzazione operativa.

---

## Dove siamo e cosa viene dopo

OAH e la parte core di WOM sono già implementati e funzionanti. I moduli INS, SCHED, AIM e LOG sono in fase di design e saranno sviluppati nei prossimi mesi, nell'ordine che le conversazioni con chi usa questi processi ogni giorno mi aiuterà a definire.

Ecco perché il vostro contributo in questa fase è prezioso.

Se gestite asset, manutenzione, ispezioni o turni operativi — in qualunque settore — mi interessa capire:

- Quale di questi processi è oggi più problematico nella vostra realtà?
- Usate già uno strumento, o lavorate principalmente con spreadsheet e procedure informali?
- C'è qualcosa che avete sempre voluto da uno strumento APM e che non avete mai trovato?

Sono disponibile nei commenti o in direct message. E se conoscete colleghi che si occupano di questi temi — responsabili di manutenzione, reliability engineer, HSE manager, responsabili di impianto — sarò grato se vorrete condividere questo post con loro.

Il valore di OpenAPM si costruisce insieme.

---

*Seguire l'avanzamento del progetto: [pagina Progress]({{ '/blog/' | relative_url }}) · [pagina Moduli]({{ '/modules/' | relative_url }})*

#OpenAPM #AssetManagement #Manutenzione #APM #ReliabilityEngineering #MaintenanceManagement #Industria40 #DigitalTransformation #CMMS #PMI #Engineering
