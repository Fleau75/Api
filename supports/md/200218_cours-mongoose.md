# Recevoir les données

## Recevoir des données

Nous distinguerons deux manières pour le client d’envoyer des données au serveur :
1. dans l’url, avec par exemple la méthode `GET`, ce qui n’est pas la meilleure façon de faire,
1. dans le corps de la méthode `POST`, ce qui est la méthode appropriée.

### Transmettre des données dans l’URL

En **HTTP**, les requêtes `GET` peuvent transmettre des propriétés au serveur :
1. en ajoutant un point d’interrogation `?` à la fin de la requête,
1. puis en définissant les paramètres avec leurs noms séparant leurs valeurs par des égals `=`,
1. et en séparant chaque propriété par une esperluette `&`,

Par exemple avec l’URL <http://localhost:3000/login?user=vincent&password=azerty1234> :
1. nous demandons au serveur la page `/login`,
1. et nous lui transmettons les paramètres `"user"` et `"password"`, valant respectivement `"vincent"` et `"azerty1234"`.

**Express** nous délivre ces paramètres dans la propriété `req.query` :
1. `"user"` est stocké dans `req.query.user`,
1. et `"password"` est stocké dans `req.query.password`.

Ainsi, le server pourrait traiter l’URL <http://localhost:3000/login?user=vincent&password=azerty1234> comme ceci :
```js
app.get('/login', (req, res) => {
	console.log('user: ', req.query.user)
	console.log('password: ', req.query.password)
	res.send(`Bienvenue ${req.query.user} !`)
})
```

Le serveur a bien intercepté les deux paramètres de l’URL, à la place des deux `console.log`, il pourrait effectuer des requête auprès d’une base de données pour authentifier l’utilisateur.

Cependant, les paramètres d’une URL peuvent être interceptés, même avec une connexion sécurisée. Ce n’est donc pas la façon que nous privilégerions pour transmettre des données au serveur, surtout si ces données sont sensibles (par exemple : identifiants de connexion, numéro de carte bancaire, etc.).

### Transmettre des données dans le corps d’une reqête POST

Il est possible de crypter les données transmises dans le corps d’une requête `POST` avec une connexions sécurisée en **HTTPS** et on ne peut alors pas les intercepter, contrairement à des données qui sont pas tansmises dans l’URL qui circule toujours en clair.

Les pages **HTML** envoient les données de leurs formulaires au serveur au format `x-www-form-urlencoded`. 

Pour lire le corps d’une requête en `x-www-form-urlencoded` avec **Express**, nous allons :
1. installer le module **body-parser** avec `npm install --save body-parser`,
1. charger ce module avec `const bodyParser = require('body-parser')`,
1. créer un *middleware* pour lire les données au format `x-www-form-urlencoded` avec `const urlencodedParser = bodyParser.urlencoded({ extended: false })`,
1. utiliser ce *middleware* dans les routes qui traitent les requêtes `POST`,
1. les requêtes de ces routes auront alors la propriété `.body` contenant les données postées par le formulaire.

```js
app.post('/login', urlencodedParser, (req, res) => {
	console.log('user: ', req.body.user)
	console.log('password: ', req.body.password)
	res.send(`Bienvenue ${req.body.user} !`)
})
```

Pour simuler l’envoi d’un formulaire dans le **Terminal**, nous tapons `curl -X POST -d 'user=vincent&password=azerty1234' http://localhost:3000/login`.

## Persister les données avec MongoDB

### Créer un nouveau projet

Nous étudierons la persistance des données avec MongoDB dans un nouveau projet :
1. nous créons un nouveau dossier `3-mongoose`,
1. nous y lançons la commande du Terminal `npm init -y`, un fichier `package.json` est alors ajouté au projet par **NPM**,
1. nous modifions `package.json` pour l’adapter à notre projet :
	1. nous indiquons dans la `"description"` avec quelle version de **Node.js** nous réalisons notre application aujourd’hui,
	1. nous indiquons que le point d’entrée de notre application sera le fichier `server.js` et pour cela nous modifions la propriété `"main"` en conséquence,
	1. nous ajoutons les scripts `"start": "node server.js"` et `"dev": "nodemon server.js"` à la propriété `"scripts"`.
1. nous sauvegardons et nous fermons le fichier `package.json` avant de passer à la suite.

### Structure de notre projet

**Après avoir sauvegardé et fermé le fichier `package.json`,** nous installons les packages nécessaires au projet avec la commande :
```
npm install --save body-parser express helmet mongoose pug
```

Nous créons à la racine du projet :
1. un fichier `server.js`,
1. un dossier `views/` qui contient lui-même :
    1. un fichier `signup.pug`,
    1. un fichier `signin.pug`.

Notre projet est alors structuré comme suit :
```
node_modules/
views/
├── signin.pug
└── signup.pug
package-lock.json
package.json
server.js
```

Le fichier `views/signin.pug` contient :
```pug
doctype html
html(lang="fr-FR")
    head
        title Se connecter
        meta(charset="utf-8")
    body
        h1 Se connecter
        form(action="/signin", method="post")
            input(type="text", name="name")
            input(type="password", name="password")
            input(type="submit", value="Sig in")
```

Le fichier `views/signup.pug` contient :
```pug
doctype html
html(lang="fr-FR")
    head
        title S’inscrire
        meta(charset="utf-8")
    body
        h1 S’inscrire
        form(action="/signup", method="post")
            input(type="text", name="name")
            input(type="password", name="password")
            input(type="submit", value="Login")
```

Dans `server.js`, nous commençons par charger les dépendances du projet, créer l’application **Express** et la lancer :
```js
const express = require('express')
const app = express()
const helmet = require('helmet')
const bodyParser = require('body-parser')
const urlencodedParser = bodyParser.urlencoded({ extended: false })
const mongoose = require('mongoose')

app.get('/signin', (req, res) => {
    res.render('signing.pug')
})
app.get('/signup', (req, res) => {
    res.render('signing.pug')
})

app.listen(3000, () => console.log('SERVEUR LANCÉ SUR LE PORT 3000'))
```

À ce stade, quand nous lançons `npm run dev` dans le Terminal :
1. la console nous indique que le serveur est lancé,
1. nous accédons au pages <http://localhost.3000/signin> et <http://localhost.3000/signup> dans le navigateur qui nous affiche un formulaire de connexion et de création de compte.

### Équivalences entre MySQL, MongoDB, JS et Mongoose

|MySQL|MongoDB|JS|Exemple JS|Mongoose|
|:---:|:-----:|:--:|:-------:|:-------:|
|Base de données|Base de données|—|—|—|
|Table|Collection|Liste d’objet|`[{prenom:"Jean",nom:"Dupont"}, {prenom:"Marie",nom:"Durand"}, {prenom:"Jacques",nom:"Martin")]`|Modèle (ex.&nbsp;: `User`)|
|Ligne|Document|Objet|`{prenom:"Jean",nom:"Dupont"}`|Instance du modèle (ex.&nbsp;: `user`)|
|Colonne|Champ|Propriété|`prenom:"Jean"`|Propriété de l’instance du modèle (ex.&nbsp;: `user.prenom`)|

Il est à noter que tous les documents **MongoDB** possèdent la propriété `_id` dont la valeur est unique, elle correspond à la clé primaire en **SQL**.

### Piloter MongoDB avec l’ODM Mongoose

Pour assurer la stabilité de notre base de données **NoSQL** **MongoDB**, nous la piloterons avec l’ODM (Object Data Model) **Mongoose** :
1. nous l’installons avec `npm install --save mongoose`,
1. puis nous le chargeons dans `server.js` avec `const mongoose = require('mongoose')`.

Pour nous connecter à la base de données, la documentation de **Mongoose** nous indique d’utiliser ce code :
```js
const mongoose = require('mongoose')
mongoose.connect('<taper-ici-l-adresse-et-le-mot-de-passe-de-la-base-de-données>', 
    {useNewUrlParser: true, useUnifiedTopology: true })
const db = mongoose.connection
db.on('error', console.error.bind(console, 'ERROR: CANNOT CONNECT TO MONGO-DB'))
db.once('open', () => {
	console.log('SUCCESS: CONNECTED TO MONGO-DB')
})
```