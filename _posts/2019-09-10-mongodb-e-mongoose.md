---
title: MongoDB e Mongoose
layout: post
date: '2019-07-05 15:00:00'
comments: true
tags:
- Italiano
- Angular
- Guide
image: RESTfil-API.png
excerpt: |-
  Scopriamo insieme cosa significa progettare un set di API in stile RESTful e come approcciare al meglio lo sviluppo di un backend. <br />
  Gli strumenti che ci possono facilitare il compito e cosa dobbiamo tenere in considerazione durante la fase di progettazione e definizione degli endpoints.
---

> Questo articolo fa parte della guida allo sviluppo di una SPA con Angular. Se ti sei perso gli articoli prima [clicca qui][index]

Dopo aver progettato le nostre API è arrivato finalmente il momento di sviluppare il nostro backend. <br />

API in stile RESTful
---------------
Con l’acronimo API si intende Application Programming Interface e non è altro che la creazione di una serie di endpoint (delle URL) che rispondono alle richieste fatte da uno sviluppatore.
Praticamente una raccolta di metodi che ci permettono di collegare librerie o applicazioni presenti su diversi sistemi. <br />
Il significato dell’acronimo REST è invece REpresentational State Transfer, ovvero una rappresentazione del trasferimento di stato di un determinato dato. REST non è una tecnologia, piuttosto descrive una serie di linee guida e di approcci che definiscono lo stile con cui i dati vengono trasmessi.<br />
Ora che abbiamo finito con gli acronimi possiamo andare a vedere davvero cosa significa realizzare un set di API RESTful.<br />
Oramai saper definire API RESTful è un prerequisito per chiunque voglia lavorare su un backend web.<br />
Tuttavia non esiste uno standard per la realizzazione di una API ma solamente una seria di __linee guida__ che se rispettate garantiscono un buon risultato finale:
* Consistenza all'interno dell'intera API
* Stateless ovvero non esistono sessioni nel nostro server e per tanto non salviamo informazioni tra una richiesta e l'altra. Di fatto ogni richiesta è una comunicazione a se stante e il server non conserva memoria di eventuali richiesta precedenti arrivando a non tenere traccia nemmeno delle informazioni di login.
* Utilizzo degli status code HTTP per velocizzare e facilitare le risposte e la loro leggibilità.
* Definizione di URL endpoint che seguono una determinata logica 

Accopiati agli endpoint si utilizzano i metodi HTTP che hanno lo scopo di rappresentare le azioni che intendiamo compiere su una determinata risorsa:
* __GET__: utililizzato come il nome suggerisce quando desideriamo ottenere una risorsa o set di dati.
*  __POST__: da usare quando si aggiungono nuovi elementi di un determinato tipo di risorsa.
*  __DELETE__: neanche a dirlo questo metodo lo si utilizza per eliminare dei dati.
*  __PUT__: Da utilizzare quando si desidera effetuare l'update sostituendo del tutto una risorsa. Si può anche utilizzare per sostituire solo in parte un dato ma per questo scopo è più indicato il prossimo metodo.
*  __PATCH__: Come appena spiegato PATCH è da utilizzare quando si vuole effetuare l'update di una parte di una risorsa, modificandone alcune delle informazioni.

Fatta questa breve parentesi di introduzione c'è da dire che realizzare una API in stile RESTful richiede un minimo di pratica.<br />
Per sperimentare facilmente senza dover per forza scrivere nel frattempo l'intero codice per il vostro backend vi suggerisco di utilizzare [Postman][postman], una magnifica applicazione che vi permetterà di realizzare e testare le vostre API in maniera semplice e veloce.<br />
Io l'ho inoltre usato per testare le API man mano che scrivevo il codice per il mio server, velocizzando di molto i miei tempi di sviluppo e test.<br />
Tra le numerose funzionalità che Postman offre vi è inoltre la possibilità di documentare in maniera elegamente il proprio set di API.  Una buona documentazione è infatti importante quasi tanto quanto un buon set di API e in questo Postman vi faciliterà non poco il compito, fornendovi anche qualche utile spunto.<br />
Potete dare un occhiata alle API e alla loro documentazione per EasyRestaurant [direttamente qui][easyrestaurant-api] ma vi suggerisco di provare prima a realizzare un vostro set di endpoints e di sperimentare con Postman prima di sbirciare come ho risolto io il tutto.

Nel prossimo articolo daremo un'occhiata veloce a come approcciare lo sviluppo di un backend con NodeJS e le tecnologie che ho utilizzato per rendere lo sviluppo un pochino più veloce e semplice.

[index]:https://lucabozzetto.github.io/realizzare-una-single-page-application-con-angular-e-nodejs/
[mockup]:https://ibb.co/sCN1gKk
[postman]:https://www.getpostman.com/
[easyrestaurant-api]:https://documenter.getpostman.com/view/6803722/SVYxnF9P?version=latest