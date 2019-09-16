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
La vera chicca di mongoose sono gli schemi e i modelli. <br  />
Con i primi andiamo a definire la struttura dei documenti di una determinata collezione. <br />
Questo significa che strutturiamo un minimo i nostri dati per avere una maggiore consistenza. <br />
I Modelli invece non sono altro che delle funzioni (costruttori)  che permettono di costruire oggetti (basandoci su un determinato schema definito in precedenza) e di memorizzarli nella rispettiva collezione del database. <br />
Una volta definito uno schema e creato un modello possiamo utilizzare quest'ultimo per:
* Effettuare query nel database eg `<model>.find({})`
* Creare un nuovo oggetto eg `<model>.create({obj})`
* Rimuovere oggetti eg `<model>.remove({})`

Ecco per esempio come ho definito lo schema per i miei utenti su EasyRestaurant:
```javascript
const mongoose = require('mongoose');
const crypto = require('crypto');
const jwt = require('jsonwebtoken');

// This obj describes the various roles the employees can assume
const Role = Object.freeze({
  Waiter: 'waiter',
  Bar: 'bar',
  Cook: 'cook',
  Cashier: 'cashier',
});

const userSchema = mongoose.Schema({
  username: {
    type: String,
    required: true,
    unique: true,
    lowercase: true,
    minLength: 1,
    maxLenght: 50,
  },
  name: {
    type: String,
    required: true,
  },
  surname: {
    type: String,
    required: true,
  },
  role: {
    type: String,
    enum: Object.values(Role),
    required: true,
  },
  hash: {
    type: String,
    required: true,
  },
  salt: {
    type: String,
    required: true,
  },
  admin: {
    type: Boolean,
    default: false,
    validate: function(value) {
      return ( (this.role == Role.Cashier && value == true) || (this.role != Role.Cashier && value == false) );
    },
    required: true,
  },
  hired: {
    type: Date,
    default: Date.now,
  },
  wage: {
    type: Number,
    min: 0,
  },
});

userSchema.methods.setPassword = function(password) {
  this.salt = crypto.randomBytes(16).toString('hex');
  this.hash = crypto.pbkdf2Sync(password, this.salt, 1000, 64, 'sha512').toString('hex');
};

userSchema.methods.validatePassword = function(password) {
  const hash = crypto.pbkdf2Sync(password, this.salt, 1000, 64, 'sha512').toString('hex');
  return this.hash === hash;
};

Object.assign(userSchema.statics, {
  Role,
});

mongoose.model('User', userSchema);
```

A questo punto sarà evidente come definire uno schema non è molto di più che definire la tipologia di dati accettati con qualche eventuale ulteriore validazione e/o valore di default. <br />
Ci tengo a far notare come uno schema possa possedere dei metodi e dei valori statici. <br />
Un chiaro esempio sono i metodi set e validePassword che per altro come da buona abitudine non salvano le password in chiaro ma una loro versione criptata. <br />
L'ultima riga ci permette di esportare il modello assegnandovi lo schema appena definito. <br />
Ecco quindi che potremmo utilizzarlo nella nostra applicazione in maniera semplice:
 
 ```javascript
const mongoose = require('mongoose');
const User = mongoose.model('User');

  const user = new User();
  user.username = "Johndoe";
  user.setPassword("secret");
  user.name = "John";
  user.surname = "Doe";
  user.role = "waiter";

  user.save((error) => {
    if (error) {
     // Handle the error
    } else {
     // The user has been created in our database!
    }
  });
};
 ```

Oppure possiamo effettuare delle query:
```javascript
const mongoose = require('mongoose');
const User = mongoose.model('User');

  try {
    const user = await User
      .findOne({username: "johndoe"})
      .exec();

    if (!user) {
      // The user was not found in our database
    }
		
		// user will now contain the required data
		console.log(user.username) // johndoe
  } catch (error) {
    console.log(error);
  }
};
```

L'articolo di oggi è stato un pò più lungo del solito ma finalmente abbiamo visto qualche riga di codice per capire come funzionano i vari strumenti che possono farci penare un pochino meno in fase di sviluppo. <br />
La prossima volta daremo un occhiata a Socket.io per gestire i websocket e alla soluzione che ho utilizzato nello specifico nel mio progetto. <br />

[index]:https://lucabozzetto.github.io/realizzare-una-single-page-application-con-angular-e-nodejs/
[mongodb]:https://www.mongodb.com
[mongoose]:https://mongoosejs.com