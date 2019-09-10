---
title: Realizzare una Single Page Application con Angular e NodeJS
layout: post
date: '2019-05-01 08:30:00'
comments: true
tags:
- Italiano
- Angular
- Guide
image: angular.jpg
excerpt: |-
  Angular (da con confondere con AngularJS) è una delle tecnologie più calde del momento insieme ai suoi cugini React e VueJS per quanto riguarda lo sviluppo web frontend. <br />
  Grazie ad esso possiamo sviluppare applicazioni web anche molto complesse in maniera modulare e utilizzando strumenti di terze parti o scrivendoci interamente da zero tutto il codice ed è per questo che ho deciso di realizzare una piccola e breve guida al suo utilizzo.
---

Introduzione
----------
[Angular][angular] (da con confondere con AngularJS) è una delle tecnologie più calde del momento insieme ai suoi cugini React e VueJS per quanto riguarda lo sviluppo web frontend. <br />
Grazie ad esso possiamo sviluppare applicazioni web anche molto complesse in maniera modulare e utilizzando strumenti di terze parti o scrivendoci interamente da zero tutto il codice ed è per questo che ho deciso di realizzare una piccola e breve guida al suo utilizzo.

Angular supporta e anzi invita caldamente all'utilizzo di Typescript, un superset di Javascript che introduce numerose funzionalità aggiuntive tra cui come il nome suggerisce un maggiore controllo sui tipi.<br />
Il framework stesso è  scritto in Typescript!<br />
Imparare ad utilizzare Angular può inizialmente richiedere un pochino di tempo, soprattutto se non si conoscono Typescript o l'approccio MVC ma posso assicurarvi che vi darà non poche soddisfazioni una volta appreso.

La guida
-------
Pubblicherò quindi in queste prossime settimane una breve guida su come realizzare una SPA (Single Page Application) con Angular. Il backend verrà realizzato invece con NodeJS in modo tale da rimanere sempre in casa Javascript.<br />
Questa guida è più una sorta di introduzione alle varie tecnologie ed è scritta da uno studente per chi desidera approcciarsi per la prima volta al mondo di Angular e Node!<br />
Ci tengo inoltre a sottolineare che non scriverò una guida passo passo ma che darò qualche consiglio su come approcciarsi e su come ho personalmente risolto i vari problemi che mi sono trovato ad affrontare. <br />
Se state cercando una guida dettagliata, il web ne è già pieno e non ho minimamente la presunzione di poter scrivere qualcosa di altrettanto valido.<br />
> Tutto il codice del progetto è disponibile nella seguente [repository Github][repo-easyrestaurant] mentre se desiderate testare l'applicazione senza dover scaricare nulla potete dare un occhiata alla [versione online][online-app] hostata su Heroku.

Qui di seguito lascio la lista con i link agli articoli finora pubblicati in merito alla guida:<br />
* [L'applicazione: gestione di un ristorante][consegna]
* [Mockup interfacce e definizione API][api]

[angular]: https://angular.io/
[consegna]: https://lucabozzetto.github.io/lapplicazione-easyrestaurant/
[api]:https://lucabozzetto.github.io//mockup-interfacce-e-definizione-api/
[repo-easyrestaurant]:https://github.com/LucaBozzetto/EasyRestaurant
[online-app]:https://easyrestaurant-frontend.herokuapp.com/