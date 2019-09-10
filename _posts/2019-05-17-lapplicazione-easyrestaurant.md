---
title: 'L''applicazione: EasyRestaurant'
layout: post
date: '2019-05-05 09:30:00'
comments: true
tags:
- Italiano
- Angular
- Guide
image: restaurant.jpg
excerpt: |-
  In questo articolo non andremo ancora a toccare con mano Angular e mi limiterò nello specifico a spiegare che progetto andremo a realizzare. <br />
  Anche se è vero che il progetto che svilupperò non è particolarmente complesso, ritengo non sia quasi mai raccomandabile buttarsi a capofitto nello scrivere codice. Un'attenta analisi ci risparmierà di dover riscrivere tutto da capo in seguito!
---

> Questo articolo fa parte della guida allo sviluppo di una SPA con Angular. Se ti sei perso gli articoli prima [clicca qui][index]

In questo articolo non andremo ancora a toccare con mano Angular e mi limiterò nello specifico a spiegare che progetto andremo a realizzare. <br />
Anche se è vero che il progetto che svilupperò non è particolarmente complesso, ritengo non sia quasi mai raccomandabile buttarsi a capofitto nello scrivere codice. Un'attenta analisi ci risparmierà di dover riscrivere tutto da capo in seguito!

L'idea base è quella di creare un'app per la gestione di un ristorante. Man mano che procederò con lo sviluppo dell'applicazione illusterò i vari concetti chiave utilizzati.<br />
L'applicazione gestisce sostanzialmente le attività di 4 diversi ruoli di dipendenti: Camerieri, Cuochi, Baristi e Cassieri.

**Camerieri**
Ciascun cameriere porta con sé un dispositivo mobile (smartphone o tablet) con cui può prendere ordinazioni ai tavoli.

**Cuochi**
I cuochi o pizzaioli vengono notificati in tempo reale sul cibo ordinato da ciascun tavolo mediante i camerieri.<br />
Gli ordini sono inseriti in una coda e raggruppati per tavolo e per tempo di realizzazione di modo tale che la preparazione dei piatti per uno stesso tavolo possano essere serviti contemporaneamente. <br />
I cuochi, attraverso la loro interfaccia, possono notificare il sistema dell’inizio e la fine di una preparazione.<br />
Quando le preparazioni di un determinato tavolo sono ultimate, il cameriere associato a quel tavolo viene notificato di tale evento e può quindi procedere al servizio.

**Baristi**
I baristi sono concettualmente simili ai cuochi ma si occupano solo delle bibite.<br />
I baristi ricevono notifica delle ordinazioni di ciascun tavolo e inviano notifica di fine preparazione ai camerieri quando il “vassoio” di bibite di un determinato tavolo è pronto per il servizio

**Cassa**
L’addetto alla cassa può “chiudere il conto” di un tavolo producendo uno scontrino con il totale da pagare.<br />
La cassa ha inoltre visione delle statistiche sullo stato degli ordini, in particolare:
* Quali tavoli hanno ordini in fase di preparazione
* Quale cameriere è associato a ciascun tavolo
* Quali tavoli sono liberi/occupati
* Quanto è lunga la coda di preparazioni della cucina
Infine, la cassa può conoscere il guadagno totale di una giornata.

Il sistema dovrà implementare le seguenti funzionalità:
1. **Gestione degli utenti**
* Registrazione di nuovi utenti con i seguenti ruoli: camerieri, cuochi, baristi e cassa. Il ruolo “cassa” gode del ruolo di amministratore
* Possibilità di cancellazione di utenti esistenti da parte degli amministratori
* Login degli utenti con le proprie credenziali di accesso. Tutte le funzionalità del sistema sono accessibili previo login obbligatorio
2. **Gestione dei tavoli e ordini** (solo per camerieri)
* Modifica dello stato di un tavolo da libero a occupato da un certo numero di clienti. Il numero di clienti deve essere compatibile con i posti a sedere al tavolo
* Aggiunta di una o più ordinazioni di piatti per un determinato tavolo. Quando le ordinazioni sono ultimate queste vengono notificate ai cuochi
* Aggiunta di una o più ordinazioni di bevande per un determinato tavolo. Quando le ordinazioni sono ultimate queste vengono notificate ai camerieri
* Notifica al cameriere addetto ad un tavolo dei piatti e bevande pronte da servire per quel tavolo
3. **Gestione della coda di preparazione dei cibi** (solo cuochi)
* Visualizzazione in tempo reale della coda di preparazione dei cibi per ciascun tavolo. 
Si assuma per semplicità che vi sia un’unica coda di preparazione e che ciascun tavolo venga servito con logica FIFO. 
I cibi da realizzare vanno ordinati per tempo di preparazione, dal più lungo al più breve. 
Il cuoco, per ciascun cibo, può notificare al sistema l’inizio e la fine della preparazione. 
Quando tutti i cibi di un determinato tavolo sono stati preparati, il cameriere addetto a quel tavolo viene notificato che può procedere al servizio.
4. **Gestione della coda di preparazione delle bevande** (solo baristi)
* Visualizzazione in tempo reale della coda di preparazione bevande per ciascun tavolo. 
Quando il vassoio delle bevande è pronto il barista notifica tale evento al sistema che provvederà ad inoltrarlo al cameriere addetto a quel determinato tavolo (il funzionamento è analogo a quanto avviene per i cuochi)
5. **Gestione cassa** (solo per cassieri):
* Calcolo del conto totale di un tavolo e creazione dello scontrino con la lista delle ordinazioni consumate. Il tavolo passa quindi dallo stato occupato allo stato libero e può essere riutilizzato dai camerieri
* Visualizzazione dei tavoli liberi e occupati
* Visualizzazione degli ordini effettuati e in attesa di preparazione per ciascun tavolo
* Visualizzazione statistiche su ciascun cameriere e cuoco (numero clienti serviti, piatti realizzati, etc.)


Per oggi direi che è tutto, nel prossimo articolo inizieremo con un veloce design delle interfacce e con la definizione di un set di API in stile RESTful.

[index]:https://lucabozzetto.github.io/realizzare-una-single-page-application-con-angular-e-nodejs/