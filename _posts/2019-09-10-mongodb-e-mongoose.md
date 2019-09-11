---
title: MongoDB e Mongoose
layout: post
date: '2019-07-05 15:00:00'
comments: true
tags:
- Italiano
- Angular
- Guide
image: mongodb.png
excerpt: |-
  Dopo aver progettato le nostre API è arrivato finalmente il momento di sviluppare il nostro backend. <br />
  Una rapida occhiata a MongoDB e MongooseJS ci aiuterà a gestire in maniera rapida le nostre collezioni di dati e le interazioni con un database non relazionale.
---

> Questo articolo fa parte della guida allo sviluppo di una SPA con Angular. Se ti sei perso gli articoli prima [clicca qui][index]

Dopo aver progettato le nostre API è arrivato finalmente il momento di sviluppare il nostro backend. <br />
Avremo sicuramente bisogno di memorizzare i nostri dati in un database e [MongoDB][mongodb] può fare proprio al caso nostro! <br />
MongoDB infatti è un database non relazione che si intregra particolarmente bene con i linguaggi dinamici come javascript e non richiede una progettazione a priori del database. <br />
Il database è infatti orientato ai documenti e non alle relazioni tra i dati. Inoltre lo schema dinamico (schema-less) ci permette di salvare dati anche non omogenei (con ad esempio un numero di colonne diverso tra loro) nella stessa collezione!<br />
Tuttavia utilizzare MongoDB direttamente insieme a Node e Express può rivelarsi un'operazione lunga e poco simpatica. Ecco quindi che in nostro aiuto viene [Mongoose][mongoose].<br />
Mongoose è una libreria di Object Document Mapping ( ODM ) che permette di definire lo schema dei documenti attraverso oggetti Javascript per effettuare il mapping automatico da e verso il database.<br />
Grazie a questa comoda libreria andremo a lavorare su oggetti Javascript come siamo abituati ma quest'ultimi ci metterano a disposizione numerosi metodi per salvare e/o effetuare altre operazioni sul nostro database.<br />
Per essere più chiaro eccovi qualche esempio su come lavorare con mongoose.<br />

Collegamento al database
--------------------
Il mio consiglio è quello di realizzare nella vostra cartella dei modelli un file chiamato db.js dove andrete a definire il setup della connessione al database e importerete i vari schemi di Mongoose di cui a breve di parlerò. <br />
Un esempio di come ho realizzato il mio è il seguente:
```javascript
const mongoose = require('mongoose');

let dbURI = 'localhost';
if (process.env.NODE_ENV === 'production') {
  dbURI = process.env.MONGODB_URI;
}
mongoose.connect(dbURI, {
  useNewUrlParser: true,
});
mongoose.set('useCreateIndex', true);

mongoose.connection.on('connected', () => {
  console.log(`Mongoose connected to ${dbURI}`);
});
mongoose.connection.on('error', (err) => {
  console.log('Mongoose connection error:', err);
});
mongoose.connection.on('disconnected', () => {
  console.log('Mongoose disconnected');
});

const gracefulShutdown = (msg, callback) => {
  mongoose.connection.close( () => {
    console.log(`Mongoose disconnected through ${msg}`);
    callback();
  });
};

// For nodemon restarts
process.once('SIGUSR2', () => {
  gracefulShutdown('nodemon restart', () => {
    process.kill(process.pid, 'SIGUSR2');
  });
});
// For app termination
process.on('SIGINT', () => {
  gracefulShutdown('app termination', () => {
    process.exit(0);
  });
});
// For Heroku app termination
process.on('SIGTERM', () => {
  gracefulShutdown('Heroku app shutdown', () => {
    process.exit(0);
  });
	require('./users');
});
```
Come potete vedere questo file si limita in sostanza a stabilire una connessione con il nostro database e a terminare correttamente quest'ultima in caso di arresto dell'applicazione in maniera non prevista. <br />
L'ultima riga ossia `require('./users');` ci lascia intendere che stiamo effettuando l'import di un ulteriore modello. <br />
In questo modello come vedremo tra poco ho definito il mio schema di Mongoose.

Schemas, Models e loro utilizzo
-----------------------



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
[mongodb]:https://www.mongodb.com
[mongoose]:https://mongoosejs.com