# Node JS

## Table des matières

1. [Installation de Node](#installation-de-node)
	1. [Installation de Node Windows](#installation-de-node-windows)
	1. [Installation de Node Mac](#installation-de-node-mac)
	1. [Installer la Visual Studio Code CLI](#installer-la-visual-studio-code-cli)
1. [Créer un serveur HTTP](#cr-er-un-serveur-http)
	1. [Afficher “Hello Node” dans le Terminal](#afficher-hello-node-dans-le-terminal)
	1. [Afficher “Hello Node” dans le navigateur](#afficher-hello-node-dans-le-navigateur)
	1. [Afficher “Hello Node” en HTML](#afficher-hello-node-en-html)
		1. [Les entêtes du protocole HTTP](#les-ent-tes-du-protocole-http)
	1. [Servir plusieurs pages](#servir-plusieurs-pages)
1. [Le module Nodemon](#le-module-nodemon)
1. [Le module Express](#le-module-express)
	1. [Inititialiser NPM](#inititialiser-npm)
	1. [Hello Node avec Express](#hello-node-avec-express)
	1. [Sécuriser notre application avec Helmet](#s-curiser-notre-application-avec-helmet)
	1. [Hello Node avec Pug](#hello-node-avec-pug)
	1. [Servir plusieurs pages avec Express](#servir-plusieurs-pages-avec-express)
	1. [Servir des fichiers statiques](#servir-des-fichiers-statiques)
	1. [Refactoriser les vues grâce aux inclusions de Pug](#refactoriser-les-vues-gr-ce-aux-inclusions-de-pug)
	1. [Les routes dynamiques](#les-routes-dynamiques)
	1. [Les middlewares du routeur](#les-middlewares-du-routeur)
	1. [L’approche modulaire de Node.js](#lapproche-modulaire-de-nodejs)
1. [Recevoir des données](#recevoir-des-donn-es)
	1. [Transmettre des données dans l’URL](#transmettre-des-donn-es-dans-l-url)
	1. [Transmettre des données dans le corps d’une reqête POST](#transmettre-des-donn-es-dans-le-corps-d-une-req-te-post)
1. [Persister les données avec MongoDB](#persister-les-donn-es-avec-mongodb)
	1. [Créer un nouveau projet](#cr-er-un-nouveau-projet)
	1. [Structure de notre projet](#structure-de-notre-projet)
	1. [Équivalences entre MySQL, MongoDB, JS et Mongoose](#-quivalences-entre-mysql-mongodb-js-et-mongoose)
	1. [Piloter MongoDB avec l’ODM Mongoose](#piloter-mongodb-avec-l-odm-mongoose)
		1. [Le modèle Mongoose](#le-mod-le-mongoose)
		1. [Créer le modèle](#cr-er-le-mod-le)
		1. [Poster des données avec Mongoose](#poster-des-donn-es-avec-mongoose)
		1. [Lire des données avec Mongoose](#lire-des-donn-es-avec-mongoose)
		1. [Modifier des données avec Mongoose](#modifier-des-donn-es-avec-mongoose)
		1. [Supprimer des données avec Mongoose](#supprimer-des-donn-es-avec-mongoose)
1. [Transformer une callback en promesse](#transformer-une-callback-en-promesse)
1. [Authentifier l’utilisateur avec Passport et express-session](#authentifier-l-utilisateur-avec-passport-et-express-session)
	1. [Installer et configurer les dépendances](#installer-et-configurer-les-d-pendances)
	1. [Login](#login)
	1. [Protéger une route](#prot-ger-une-route)
	1. [Logout](#logout)

## Installation de Node

### Installation de Node Windows

<https://github.com/coreybutler/nvm-windows>

### Installation de Node Mac

<https://github.com/creationix/nvm>

1. Ouvrir le **Terminal**
1. `touch ~/.bash_profile`
1. `curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash`
1. Ouvrir une nouvelle fenêtre du **Terminal**
	1. `command -v nvm`
	1. Le **Terminal** doit répondre `nvm`
1. `nvm install --lts`
	1. Cette commande installe la dernière version de Node JS
	1. La version installée sera celle par défaut
		1. `nvm ls`
		1. Réponse (entre autre) : `default -> lts/* (-> v10.16.0)`
		1. Pour changer la version par défaut :
			1. Choisir une version parmi `nvm ls-remote`
			1. `nvm install <version>`
			1. `nvm alias default <version>`

### Installer la Visual Studio Code CLI

1. Taper `cmd-shift-p` dans Visual Studio Code
1. Rechercher et lancer `Shell Command: Install 'code' command in PATH command`

## Créer un serveur HTTP

Nous créons un projet Node :

1. `mkdir 1-serveur-http`
1. `code 01-hello-node` ouvre le dossier `01-hello-node` Visual Studio Code

### Afficher “Hello Node” dans le Terminal

Nous créons le fichier `1-hello-terminal.js` et y écrivons :

```js
console.log('Hello Node');
```

Nous lançons `node 1-hello-terminal.js` dans le **Terminal** qui nous répond `Hello Node`.

### Afficher “Hello Node” dans le navigateur

Plus tard, nous installerons dans nos applications Node des modules développés par des tiers, puis nous les chargerons avec la fonction `require()`.

Node propose nativement des modules que nous n’avons pas besoin d’installer au préalable. Pour les utiliser, nous avons seulement besoin de les charger avec la fonction `require()`.

Pour afficher une page dans le navigateur, nous allons créer un serveur **HTTP** avec le module `http` inclus nativement dans Node et ce serveur HTPP servira cette page au navigateur.

Nous créons le fichier `2-hello-http.js` et y écrivons :

```js
const { createServer } = require('http') // Chargement du module http inclus dans Node

const server = createServer((request, response) => {  // Création d’un serveur HTTP
	response.end('Hello Node depuis un serveur HTTP')
});

server.listen(3000, () => console.log('Serveur lancé sur le port 3000')) // Lancement du serveur sur le port 3000 de l’ordinateur
```

Nous lançons `node hello-http.js` dans le **Terminal** qui nous répond `Serveur lancé sur le port 3000`

Quand le serveur est lancé, nous accédons à <http://localhost:3000/> dans un navigateur ou nous lançons dans le **Terminal** la commande `curl localhost:3000` ou la commande `curl -X GET localhost:3000`.

Dans cet exemple, nous avons arbitrairement choisi le port `3000`. Il est possible d’utiliser n’importe quel port à partir de `1000`. Si on ne spécifie pas le port sur lequel le serveur est accessible, ce port sera implicitement `80` pour un serveur **HTTP** et `443` pour un serveur **HTTPS**.

Nous stoppons le serveur Node avec la commande `ctrl-c` avant de passer à l’exemple suivant.

### Afficher “Hello Node” en HTML

Nous créons le fichier  `3-hello-html.js` et y écrivons :

```js
const { createServer } = require('http')

const server = createServer((req, res) => {
	res.end(`<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8"/>
		<title>Ma page HTML servie par Node JS</title>
		<style>
			html { font-family: sans-serif; }
			h1 { color: #f00; }
		<style>
	</head>
	<body>
		<h1>Hello Node !</h1>
		<p>Voici un paragraphe <strong>HTML</strong> !</p>
		<script>console.log("hello")</script>
	</body>
</html>
`
	)
})

server.listen(3000, () => console.log('Serveur lancé sur le port 3000'))
```

Nous lançons `node 3-hello-html.js`, puis nous allons à <http://localhost:3000/> dans le navigateur, il affiche une page **HTML**.

#### Les entêtes du protocole HTTP

Quand le navigateur envoie sa requête au serveur **HTTP**, sa requête contient un entête qui lui permet de se faire comprendre par le serveur.

Quant au serveur, sa réponse est composée :
1. elle aussi d’un entête,
1. ainsi que du corps de la réponse, qui est le document **HTML** que nous envoyons au client.

Pour observer aussi bien les entêtes de la requête que ceux de la réponse, lançons cette commande dans le **Terminal** : `curl -v http://localhost:3000/`.

Le **Terminal** affiche ceci en particulier :

```
> GET / HTTP/1.1
> Host: localhost:3000
> User-Agent: curl/7.54.0
> Accept: */*
> 
< HTTP/1.1 200 OK
< Date: Wed, 21 Aug 2019 09:25:54 GMT
< Connection: keep-alive
< Content-Length: 257
< 
<!DOCTYPE html>
<html>
        <head>
                <meta charset="utf-8"/>
                <title>Ma page HTML servie par Node JS</title>
				<style>
					html { font-family: sans-serif; }
					h1 { color: #f00; }
				<style>
        </head>
        <body>
                <h1>Hello Node !</h1>
                <p>Voici un paragraphe <strong>HTML</strong> !</p>
                <script>console.log("hello")</script>
        </body>
</html>
```

1. Les lignes précédées d’un `>` sont les entêtes de la reqête du client,
1. alors que les lignes précédées d’un `<` sont ceux de la requête du serveur, destinés à se faire comprendre par le client :
	- `HTTP/1.1 200 OK` signifie que :
		- le serveur utilie la version `1.1` du protocole **HTTP**,
		- et que son code de status est le `200`, a donc été traitée avec succès par le serveur.
	- les trois lignes suivantes sont les entêtes proprement dits, composé chacun :
		- du nom d’un entête suivi de deux points
		- et de la valeur de cet entête.
1. Ce qui suit les entêtes est le corps de la réponse, c’est-à-dire le **HTML** que nous avons rédigé.

Les entêtes du serveur ont été rédigés automatiquement par **Node.js**, mais nous aurions tout intérêt à les rédiger nous-même. Par exemple, il serait oportun d’indiquer au client que la ressources que nous lui envoyons est du **HTML**. Pour cela, nous utilisons la méthode `writeHead()` avant de retourner la réponse :

```js
const { createServer } = require('http')

const server = createServer((req, res) => {
	res.writeHead(200, {'Content-Type': 'text/html'}) // Nous écrivons nous-même ces entêtes
	res.end(`<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8"/>
		<title>Ma page HTML servie par Node JS</title>
	</head>
	<body>
		<h1>Hello Node !</h1>
		<p>Voici un paragraphe <strong>HTML</strong> !</p>
		<script>console.log("hello")</script>
	</body>
</html>
`
	)
})

server.listen(3000, () => console.log('Serveur lancé sur le port 3000'))
```

Nous stoppons le serveur Node avec la commande `ctrl-c` avant de passer à l’exemple suivant.

### Servir plusieurs pages

Nous avons appris à servir une page avec Node. Si nous voulons servir plusieurs pages, notre serveur doit parser la requête de l’utilisateur. Pour cela nous utiliserons le module `url` inclus nativement dans Node.

Nous créons le fichier `4-servir-plusieurs-pages.js` et y écrivons :

```js
const { createServer } = require('http')

const server = createServer((req, res) => {
	// Nous récupérons le chemin d’accès à la ressource demandée
	const page = req.url
	// Le serveur sert du texte brut et renvoie un status code 200 pour toutes les pages
	res.writeHead(200, {'Content-Type': 'text/plain; charset=utf-8'})
	switch (page) {
		case '/':
			res.write('Page d’accueil.')
			break
		case '/category':
			res.write('Page des catégories.')
			break
		case '/category/html':
			res.write('Page de la catégorie HTML.')
			break
		case '/category/css':
			res.write('Page de la catégorie CSS.')
			break
		case '/category/javascript':
			res.write('Page de la catégorie JavaScript.')
			break
		default:
			// Ici, le serveur renvoie un status code 404
			res.writeHead(404, {'Content-Type': 'text/plain; charset=utf-8'})
			res.write('Page non trouvée !')
	}
	console.log(res.statusCode)
	res.end();
})

server.listen(3000, () => console.log('Serveur lancé sur le port 3000'))
```

Pour la concision du code, notre serveur ne sert pas des pages **HTML** mais du texte brut. Cela est précisé dans les `headers` grâce à la propriété `'Content-Type'`.

Le `Status Code` est `200` quand la page est trouvée et est `400` quand la page n’existe pas.

Quand nous lançons le serveur avec `node 4-servir-plusieurs-pages.js`, nous accédons dans le navigateur à :
- <http://localhost:3000/> qui affiche l’accueil,
- <http://localhost:3000/category> qui affiche la page des catégories,
- <http://localhost:3000/category/html> qui affiche la page de la catégorie **HTML**,
- <http://localhost:3000/category/css> qui affiche la page de la catégorie **CSS**,
- <http://localhost:3000/category/javascript> qui affiche la page de la catégorie JavaScript,
- <http://localhost:3000/lol> qui affiche `Page non trouvée`.

La gestion des routes se complexifie et un routeur nous faciliterait la tâche. C’est pour cela que nous utiliserons le module **Express** qui intègre lui-même une multitude de modules, dont un routeur.

Avant cela, nous installerons le module **Nodemon** de manière globale sur notre ordinateur. Il nous fera gagner du temps en observant nos fichiers et en relançant lui-même le serveur à chaque nouvelle sauvegarde.

Nous stoppons le serveur Node avec la commande `ctrl-c` avant de passer à la suite.

## Le module Nodemon

À partir de maintenant nous développerons une application, plutôt que de devoir stopper le serveur Node et le relancer à chaque modification, nous utiliserons le module **Nodemon**.

Puisque **Nodemon** n’est pas un module natif de Node, nous devons l’installer avant de pouvoir nous en servir.

Concernant les modules de Node :
- ils sont appelés des *packages*
- les *packages* s’installent avec l’application **NPM** (*Node Package Manager*),
- **NPM** est intimement lié à Node et il a été automatiquement installé avec celui-ci.

Les modules sont presque toujours installés pour un projet particulier et nous verrons comment faire cela à la section suivante. Mais nous allons maintenant installer le module **Nodemon** de façon globale, de telle sorte qu’il sera disponible pour tous les projets Node que nous créerons sur notre ordinateur.

Pour installer **Nodemon** et le rendre disponible pour toutes les applications Node de notre ordinateur, nous lançons la commande `npm install nodemon -g`. C’est l’option `-g` qui fait que l’installation de **Nodemon** se fera *globalement*, pour tout l’ordinateur et pas seulement pour un projet en particulier.

Maintenant que **Nodemon** est installé, nous ne lancerons plus la commande `node`, mais `nodemon` pour lancer une application Node. Par exemple, au lieu de lancer `node 4-servir-plusieurs-pages.js` comme précédemment, nous pouvons lancer `nodemon 4-servir-plusieurs-pages.js`. Ainsi, à chaque chaque fois que nous sauvegardons le fichier `4-servir-plusieurs-pages.js`, le serveur est relancé automatiquement et prend en compte nos dernières modifications. Nous tapons `ctrl-c` pour stopper le serveur lancé par **Nodemon**.

## Le module Express

**Express** est un *framework* qui regroupe de nombreux modules très utiles pour traiter les tâches d’un serveur **HTTP**. Il simplifie le développement d’API, d’applications ou tout simplement de sites en **Node.js**.

Nous sommes encore dans le dossier `1-serveur-http/`. Nous allons créer un deuxième dossier à côté de celui-ci, puis nous placer dans ce nouveau dossier : `mkdir ../2-express && cd ../2-express`.

### Inititialiser NPM

À partir de maintenant, toutes nos applications utiliserons des modules, à commencer par le module **Express**. Pour gérer ces modules, nous utiliserons le **Node Package Manager**, il a été installé en même temps que nous avons installé **Node.js** sur notre ordinateur.

Au cœur de l’utilisation du **NPM**, il y a le fichier de configuration `package.json`. Nous pourrions le créer de toutes pièces, mais nous allons plutôt utiliser la ligne de commande `npm init -y`. Mais finalement, `package.json` n’est pas seulement le fichier de configuration de **NPM** et de ses modules, mais c’est aussi le fichier de configurationd de nos applications **Node.js**.

De ce fait, à chaque fois que nous créerons un nouveau projet en **Node.js** :
1. nous créerons un nouveau dossier,
1. nous nous placerons dedans avec le **Terminal**,
1. et nous lancerons `npm init -y`.

Si nous n’avions pas ajouté le paramètre `-y` à `npm init`, **NPM** nous aurait demandé de renseigner les informations du `package.json`, mais nous lui avons demandé de le faire lui-même avec le paramètre `-y`. Maintenant, nous modifions nous-même `package.json`, comme ceci :
1. pour la propriété `"description"`, nous indiquons la version de **Node.js** avec laquelle nous travaillons, cela nous sera très utile si nous rouvrons ce projet dans plusieurs mois ou plusieurs années, pour connaître notre version de **Node.js** nous tapons `node -v`,
1. pour la propriété `"main"`, nous indiquons le point d’entrée de notre application et nous le nommerons `"server.js"` plutôt qu’`"index.js"`,
1. la propriété `"script"` est un objet dont chaque clé que nous y indiquons est une nouvelle commande que nous ajoutons et dont chaque valeur est la commande qui sera lancée quand la commande sera invoquée :
	- il y a déjà une commande `"test"`, nous n’y touchons pas,
	- après `"test": "echo \"Error: no test specified\" && exit 1"`, nous ajoutons une virgule `,` et nous sautons une ligne,
	- nous ajoutons une deuxième propriété `"start": "node server.js"`, nous ajoutons une virgule `,` et nous sautons une ligne,,
	- nous ajoutons une troisième propriété `"dev": "nodemon server.js"`, puis nous sauvegardons `package.json`.

Voici comment nous lancerons les deux scripts que nous venons d’ajouter :
1. `"start"` est lancé par la commande `npm start`, c’est une bonne pratique d’écrire ce script parce que c’est la première commande que lancera un développeur qui ne connaît pas votre projet,
1. les autres scripts se lancent avec la commande `npm run` suivie du nom du script, la commande pour lancer `"dev"` est donc `npm run dev`.

Pour installer un module, nous utiliserons la commande `npm install` suivie du nom du module. Pour que ce module soit référencé dans notre `package.json`, nous ajouterons le paramètre `--save`, ce qui aura pour effet de renseigner automatiquement la propriété `"dependencies"` de `package.json`.

Lors de la première installation d’un module dans un projet :
1. la propriété `"dependencies"` est créée dans `package.json`,
1. le dossier `node_modules/` est créé dans le projet,
1. le nouveau module est ajouté à `node_modules/`.

Par la suite, à chaque nouvelle installation d’un module dans un projet, le nouveau module :
1. est référencé dans la propriété `"dependencies"` de `package.json`,
1. est ajouté au dossier `node_modules/`,

Maintenant, si nous supprimons le dossier `node_modules/` et que nous lançons la commande `npm install`, le dossier `node_modules/` est recréé. Ainsi, il est inutile de sauvegarder ce dossier puisqu’il est possible de le recréer à tout moment à partir du fichier `package.json` avec la commande `npm install`. Pour qu’il ne soit pas sauvegardé dans un dépôt **Git**, nous créerons un fichier nommé `.gitignore` et y écrirons la ligne `node_modules/`, il sera alors totalement ignoré par **Git**.

Par ailleurs, **NPM** a créé un fichier nommé `package-lock.json`. Il n’est pas indispensable mais il accèlère le fonctionnement de **NPM**, nous ne nous occuperons pas de ce fichier.

### Hello Node avec Express

Nous installons **Express** avec ```npm install --save express```, puis nous créons un fichier `server.js` dans lequel nous écrivons :

```js
const express = require('express')
const app = express()

app.get('/', (req, res) => {
	res.type('text/plain').send('Bonjour de Node')
});

app.listen(3000)
```

Plusieurs remarques à propos de ces quelques lignes :
1. dans les deux première lignes :
	1. nous chargeons le module **Express** avec la fonction `require()`, c’est comme cela que nous chargeons un module en **Node.js** et `require()` va chercher le module à charger dans le dossier `node_modules`,
	1. nous créons une instance d’**Express** que nous appelons `app`, `app` aura alors accès à toutes les propriétés et les méthodes d’**Express**,
1. le routeur d’**Express** est accessible avec les méthode `.get()`, `.post()`, `.put()`, `.delete()`, etc., qui se réfèrent au verbes **HTTP** `GET`, `POST`, `PUT`, `DELETE`, etc.,
1. ces méthodes prennent pour premier paramètre une chaîne de caractère qui est le chemin d’accès écouté par le serveur,
1. ces méthodes prennent pour dernier argument une fonction *callback*,
1. cette *callback* a deux arguments que nous nommons comme le souhaitons, le premier est la requête et le deuxième est la réponse,
1. nous traitons la réponse (et éventuellement la requête, si nécessaire) dans le corps de la *callback*,
1. entre la chaîne de caractères et la *callback*, nous pouvons passer d’autres arguments aux méthodes du routeur pour traiter la requête et la réponse, ces arguments sont eux aussi des fonctions *callbacks* et s’appelle des *middlewares*, nous les verrons en détail.

Ici :
1. nous avons utilisé la méthode `.get()`,
1. le routeur écoute les requêtes `GET` que le serveur reçoit à sa racine `/`,
1. et nous pouvons accéder à <http://localhost:3000/> dans le navigateur.

### Sécuriser notre application avec Helmet

Toute application **Express** doit être protégée avec le module **Helmet** avant sa mise en production. Il ajoute aux réponses du serveur des entêtes qui le protègent des vulnérabilités du protocole **HTTP**.

Son installation est très simple :
1. nous lançons `npm install --save helmet`,
1. puis nous le chargeons avec `const helmet = require('helmet')``
1. et enfin, nous demandons à notre application à l’utiliser avec `app.use(helmet())`.

Nous faisons adopter **Helmet** le plus tôt possible par notre application, ce qui nous donne ceci :
```js
const express = require('express')
const app = express()
const helmet = require('helmet')

app.use(helmet())
// La suite du code...
```

### Hello Node avec Pug

Nous allons :
1. séparer les vues de la logique,
1. et utiliser un moteur de vue pour afficher les vues.

Il existe de multiples moteurs de vue (**Pug**, **EJS**, **Handlebars**, **Nunjucks**, etc.).

Pour notre part, nous utiliserons **Pug** et nous créons un dossier `views/` qui contient lui-même un fichier `index.pug` :
```
node_modules/
views/
└── index.pug
package-lock.json
package.json
server.js
```

Ensuite, nous installons **Pug** ```npm install --save pug``` et dans ```index.pug``` nous rédigeons :

```pug
doctype html
html
	head
		title Page #{name}
		meta(charset="utf-8")
	body
		h1 Hello #{name} !
		p Page générée par le moteur de vue #{viewEngine}.
```

Les accolades précédées d’un *hashtag* `#{}` nous permettent d’écrire du **JS**. La documentation complète de **Pug** est disponible sur <https://pugjs.org/api/getting-started.html>, mais ce qu’il faut retenir en premier est l’indentation et les sauts à la ligne :
1. Quand nous écrivons :
```pug
body
	h1 Hello Pug
```
1. le moteur de vue le traduit par ce **HTML** :
```html
<body>
	<h1>Hello Pug</h1>
</body>
```

Nous avons choisi **Pug** pour sa consision.

Dans un modèle de conception Modèle/Vue/Contrôleur (MVC) :
1. le traitement de la base de données que nous verrons plus tard représentera le Modèle (les données à afficher dans l’interface graphique et leur logique).
1. les fichiers `.pug` représentent la Vue (l’interface graphique),
1. le fichier `server.js` représente le Contrôleur (la logique de la Vue, ainsi que la liaison entre les données et la Vue),

Du côté de `server.js` :
1. nous indiquons le chemin du dossier des vues avec la méthode `app.set('views', './views')`,
1. nous définissons le moteur de vue avec la méthode `app.set('view engine' 'pug')`,
1. nous fournissons à la vue les deux variables `title` et `viewEngine` qu’elle utilise dans `index.pug`.

```js
const express = require('express')
const app = express()
const helmet = require('helmet')

app.use(helmet())
app.set('views', './views')
app.set('view engine', 'pug')

app.get('/', (req, res) => {
	res.render('index.pug', {
		name: 'Node',
		viewEngine: 'Pug'
	});
});

app.listen(3000);
```

### Servir plusieurs pages avec Express

Pour servir d’autres pages, nous commençons par ajouter un fichier `category.pug` au le dossier `views/` :
```pug
doctype html
html
	head
		title Le langage #{name}
		meta(charset='utf-8')
	body
		h1 Page du #{name} !
```

Ensuite, il nous suffit d’ajouter les nouvelles routes dans le contrôleur :
```js
const express = require('express')
const app = express()
const helmet = require('helmet')

app.use(helmet())
app.set('views', './views')
app.set('view engine', 'pug')

app.get('/', (req, res) => {
	res.render('index.pug', { name: 'Node', viewEngine: 'Pug' })
})
app.get('/html', (req, res) => {
	res.render('category.pug', { name: 'HTML' })
})
app.get('/css', (req, res) => {
	res.render('category.pug', { name: 'CSS' })
})
app.get('/js', (req, res) => {
	res.render('category.pug', { name: 'JavaScript' })
})
app.get('*', (req, res) => {
	res.status(404).render('404.pug')
})

app.listen(3000)
```

Tout à la fin des routes, nous avons ajouté le traitement des pages qui n’existent pas. Quand le client souhaite accéder à une page qui n’existe pas :
1. nous lui renvoyons une réponse dont le code de statut est le `404`,
1. et nous lui servons une page spécifique,
1. pour ajoutons pour cela un nouveau fichier dans le dossier `views/`, nous la nommons `404.pug` et nous y écrivons :
```pug
doctype html
html
	head
		title Page non trouvée !
		meta(charset='utf-8')
	body
		h1 Page non trouvée !
		a(href='/') Retour à l’accueil
```

### Servir des fichiers statiques

Jusqu’à présent, les ressources que nous servons au client sont des fichiers dynamiques que nous construisons de toutes pièces. Mais certaines ressources réclamées par le client sont statiques :
- les images,
- les fichiers `.css`,
- les scripts `.js` utilisées par en *front-end*,
- etc.

Pour héberger une feuille de style `style.css` et quatre images, nous placerons ces ressources dans un dossier que nous nommons `public/` :
```
node_modules/
public/
├── css/
│	└── style.css
└── img/
	├── logo-css.jpg
	├── logo-html.jpg
	├── logo-js.jpg
	└── logo-pug.jpg
views/
package-lock.json
package.json
server.js
```

Dans `index.pug`, `styles.css` et `logo-pug.png` seront accessibles comme ceci :
```pug
doctype html
html
	head
		title Page #{name}
		meta(charset='utf-8')
		link(rel="stylesheet", href="/css/style.css")
	body
		h1 Hello #{name} !
		p Page générée par le moteur de vue #{viewEngine}.
		img(src="/img/logo-pug.png", alt="Logo de Pug")
```

Dans `category.pug`, nous lions la feuille de styles de la même manière que dans `index.pug`. Pour intégrer les images dans ces pages :
1. nous demanderons au contrôleur de leur donner le nom de ces images dans une variable `img`,
1. pour insérer ces variables dans l’attribut `src` de la balise `<img>`, nous utiliserons un littéral de gabarit,
1. pour renseigner l’attribut `alt` de la balise `<img>`, nous utiliserons aussi un littéral de gabarit.

```pug
doctype html
html
    head
        title Le langage #{name}
        meta(charset="utf-8")
        link(rel='stylesheet', href="/css/style.css")
    body
        h1 Page du #{name} !
        img(src=`/img/${img}`, alt=`Logo de ${name}`)
```

Dans le contrôleur `server.js` :
1. nous indiquons à **Express** que le dossier `public/` contient les fichiers statiques de notre application comme ceci :
```js
app.use(express.static('public'))
```
1. nous fournissons la variable `img` aux vues des catégories.

```js
const express = require('express')
const app = express()
const helmet = require('helmet')

app.use(helmet())
app.set('views', './views')
app.set('view engine', 'pug')
app.use(express.static('public'))

app.get('/', (req, res) => {
	res.render('index.pug', { name: 'Node', viewEngine: 'Pug' })
})
app.get('/html', (req, res) => {
	res.render('category.pug', { name: 'HTML', img: 'logo-html.png' })
})
app.get('/css', (req, res) => {
	res.render('category.pug', { name: 'CSS', img: 'logo-css.png' })
})
app.get('/js', (req, res) => {
	res.render('category.pug', { name: 'JavaScript', img: 'logo-js.jpg' })
})
app.get('*', (req, res) => {
	res.status(404).render('404.pug')
})

app.listen(3000)
```

### Refactoriser les vues grâce aux inclusions de Pug

Toutes nos pages ont un élément en commun :
1. leur `<head>` qui est quasiment identique,
1. pendant que nous y sommes, nous pourrions ajouter un menu de navigation,
1. et puis nous pourrions aussi ajouter un pied de pages.

Pour commencer, nous ajoutons deux nouveaux fichiers dans le dossier `views/` :
1. `header.pug` qui contiendra le `<head>` et la `<nav>` de toutes les pages :
```pug
doctype html
html
	head
		title Page #{name}
		meta(charset="utf-8")
		link(rel="stylesheet", href="/css/style.css")
	body
		nav
			a(href="/", class=name==='Node'?'active':'') Accueil
			|&nbsp;|&nbsp;
			a(href="/html", class=name==='HTML'?'active':'') HTML
			|&nbsp;|&nbsp;
			a(href="/css", class=name==='CSS'?'active':'') CSS
			|&nbsp;|&nbsp;
			a(href="/js", class=name==='JavaScript'?'active':'') JavaScript
```
Nous avons ajouté aux éléments de la `<nav>` une classe qui prend pour valeur `active` quand nous nous trouvons dans la page correspondant à cet élément. Dans `style.css`, nous pourrions définir :
```css
html {
    font-family: sans-serif;
}
.active {
    color: #000;
    text-decoration: none;
}
```
1. `footer.pug` qui contiendra le pied de page de toutes les pages :
```pug
hr
footer © 2019 Vincent Caronnet
```

Pour inclure ces morceaux de pages, nous utilisons `include` suivi du chemin vers le fichier à inclure.

Pour inclure `header.pug` et `footer.pug` dans `index.pug`, cela nous donne :
```pug
include includes/header.pug
h1 Hello #{name} !
p Page générée par le moteur de rendu #{viewEngine}.
img(src="/img/logo-pug.png", alt="Logo de Pug")
include includes/footer.pug
```

Et nous incluons `header.pug` et `footer.pug` dans `category.pug` de la même manière :
```pug
include includes/header.pug
h1 Page du #{name} !
img(src=`/img/${img}`, alt=`Logo de ${name}`)
include includes/footer.pug
```

### Les routes dynamiques

Mettons que nous souhaitions que :
1. transformer le chemin `http://localhost:3000/html` en `http://localhost:3000/category/html`,
1. transformer le chemin `http://localhost:3000/css` en `http://localhost:3000/category/css`,
1. et transformer le chemin `http://localhost:3000/js` en `http://localhost:3000/category/js`.

Pour cela :
1. il suffit de définir une variable dans le chemin en la précédant de deux points `:`,
1. et ensuite, cette variable est disponible en tant que propriété de la propriété `req.params`.

Ainsi, nous pourrions remplacer les trois routes `/html`, `/css` et `/js` par cette route :
```js
app.get('/category/:language', (req, res) => {
	switch (req.params.language) {
		case 'html':
			res.render('category.pug', { name: 'HTML', img: 'logo-html.png' })
			break
		case 'css':
			res.render('category.pug', { name: 'CSS', img: 'logo-css.png' })
			break
		case 'js':
			res.render('category.pug', { name: 'JavaScript', img: 'logo-js.jpg' })
			break
		default:
			res.status(404).render('404.pug')
	}
})
```

### Les middlewares du routeur

Sans le savoir, nous avons utilisé des *middlewares* à plusieurs reprises : avec la syntaxe `app.use()`. C’est une manière d’inclure un *middleware* dans notre application, ce *middleware* :
1. exécute une tâche sur l’application,
1. puis nous renvoie l’application pour la suite,
1. et nous pouvons utiliser autant de *middlewares* que nous souhaitons avec `app`, chaque *middleware* exécutera une tâche particulière et nous la redonnera transformée,
1. cela s’apparente au travail à la chaîne en usine.

Le traitement des routes dynamiques nous donnent l’occasion d’aborder les *middlewares* du routeur.

Nous avons vue que le routeur fonctionnait comme ceci :
1. nous avons utilisé la méthode `.get()`,
1. cette méthode accepte deux arguments,
1. le premier argument est la route à écouter, sous la forme d’une chaîne de caractères (ex. : `'/category/:language`),
1. le deuxième argument est une fonction *callback* qui accepte deux arguments :
	1. le premier argument de la *callback* est la requête du client reçue par le serveur,
	1. le deuxième argument de la *callback* est la réponse que le serveur enverra au client quand le routeur aura fini de traiter la route,
	1. ce qui nous donne cette *callback* : `(req, res) => { ... }`.

Jusqu’à présent, nous avons passé deux arguments à la fonction *callback* et nous avons choisi de les appeler `req` et `res`. Cette *callback* accepte un troisième argument facultatif que nous appellerons `next` par convention :
1. l’existence de `next` dans la fonction *callback* permet d’enchaîner les *middlewares*,
1. et que chaque *middleware* transmettent les objets `req` et `res` au *middleware* suivant.

Cela nous permet de passer autant de fonction *callback* que nous souhaitons à `.get()`. Chaque *callback*, que nous appellerons maintenant *middleware* :
1. accèdera aux objets `req` et `res` et à leurs propriétés,
1. pourra les modifier elle le souhaite,
1. et pourra transmettre `req` et `res` au *middleware* suivant avec la fonction `next()`.

Nous plaçons l’instruction `switch` qui traite la propriété `req.params.language` dans un middleware  :
```js
app.get('/category/:language', (req, res, next) => {
	switch (req.params.language) {
		case 'html':
			res.category = { name: 'HTML', img: 'logo-html.png' }
			next()
			break
		case 'css':
			res.category = { name: 'CSS', img: 'logo-css.png' }
			next()
			break
		case 'js':
			res.category = { name: 'JavaScript', img: 'logo-js.jpg' }
			next()
			break
		default:
			res.status(404).render('404.pug')
	}
}, (req, res) => {
	res.render('category.pug', res.category)
})
```

Ce nouveau *middleware* :
1. prend en arguments :
	1. la requête `req`,
	1. la réponse `res`,
	1. la fonction *callback* `next` qui sortira éventuellement `req` et `res` du *middleware* pour les transmettre au *middleware* suivant.
1. initialise une nouvelle propriété à la réponse `res`, nous avons choisi de nommer cette nouvelle propriété `category`,
1. `req.category` est un objet aux propriétés `.name` et `.img` qui seront passées à la vue par le *middleware* suivant,
1. une fois que la propriété `req.category` est intialisée, nous sortons du *middleware* avec la fonction *callback* `next()` qui transmet `req` et `res` au *middleware* suivant,
1. si `req.params.language` ne vaut ni `'html'`, ni `'css'`, ni `'js'`, alors nous envoyons une page `404` au client, le traitement de la route s’arrête là et le *middleware* suivant ne sera pas exécuté.

Le *middleware* suivant :
1. prend en arguments `req` et `res`,
1.`res` a la propriété `.category` ajoutée par le premier *middleware*
1. envoie à la vue cette propriété `.category`, un objet aux propriétés `.name` et `.img`.

Pour l’utiliser plus simplement dans `app.get()`, nous stockons le nouveau *middleware* dans une constante :
```js
const chooseCategory = (req, res, next) => {
	switch (req.params.language) {
		case 'html':
			res.category = { name: 'HTML', img: 'logo-html.png' }
			next()
			break
		case 'css':
			res.category = { name: 'CSS', img: 'logo-css.png' }
			next()
			break
		case 'js':
			res.category = { name: 'JavaScript', img: 'logo-js.jpg' }
			next()
			break
		default:
			res.status(404).render('404.pug')
	}
}
app.get('/category/:language', chooseCategory, (req, res) => {
	res.render('category.pug', res.category)
})
```

Cela nous permet d’alléger l’écriture du `app.get()` qui fait appel à ce *middleware*. Pour alléger tout le fichier `server.js`, nous pourrions consacrer un module aux *middlewares*.

### L’approche modulaire de Node.js

Créer un module dédié aux *middleware* nous permettra d’alléger le fichier `server.js` et de le rendre plus lisible.

Pour créer un module interne à notre application :
1. nous créons un fichier `routeur-middlewares.js`, ce qui signifie que notre module s’appellera `routeur-middlewares`,
1. dans `routeur-middlewares.js`, nous écrivons `module.exports = {}`,
1. le module `routeur-middlewares` est un objet `{}` auquel nous pourrons ajouter des propriétés et des méthodes,
1. les propriétés et les méthodes de `routeur-middlewares` seront alors utilisables par tout code qui importe ce module.

Nous écrivons dans `routeur-middlewares.js` :
```js
module.exports = {
	chooseCategory: (req, res, next) => {
		switch (req.params.language) {
			case 'html':
				res.category = { name: 'HTML', img: 'logo-html.png' }
				next()
				break
			case 'css':
				res.category = { name: 'CSS', img: 'logo-css.png' }
				next()
				break
			case 'js':
				res.category = { name: 'JavaScript', img: 'logo-js.jpg' }
				next()
				break
			default:
				res.status(404).render('404.pug')
		}
	}
}
```

Ceci fait, dans `server.js` :
1. nous importons le module avec la fonction `require(./routeur-middlewares)`, ou `require()` prend pour argument ici un chemin absolu vers le module,
1. nous pouvons alors utiliser la méthode `.chooseCategory()` quand nous le souhaitons.

```js
const express = require('express')
const app = express()
const helmet = require('helmet')
const routeurMiddlewares = require('./routeur-middlewares')

app.use(helmet())
app.set('views', './views')
app.set('view engine', 'pug')
app.use(express.static('public'))

app.get('/', (req, res) => {
	res.render('index.pug', { name: 'Node', viewEngine: 'Pug' })
})
app.get('/category/:language', routeurMiddlewares.chooseCategory, (req, res) => {
	res.render('category.pug', res.category)
})
app.get('*', (req, res) => {
	res.status(404).render('404.pug')
})

app.listen(3000)
```

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
mongoose.set('useFindAndModify', false)
mongoose.connect('<taper-ici-l-adresse-et-le-mot-de-passe-de-la-base-de-données>', {useNewUrlParser: true})
const db = mongoose.connection
db.on('error', console.error.bind(console, 'ERROR: CANNOT CONNECT TO MONGO-DB'))
db.once('open', () => {
	console.log('SUCCESS: CONNECTED TO MONGO-DB')
})
```

### Le modèle Mongoose

Une collection **MongoDB** représente un tableaux d’objets.

Mongoose traite les collections **MongoDB** sous forme de modèles :
1. un modèle est un constructeur,
1. lui-mêmme défini par un schéma qui définit les propriétés,
1. ce schéma décrit les propriétés que contient un objet de la collection,
1. ce schéma peut aussi définir des méthodes destinées à manipuler ces propriétés.

Par convention, si nous souhaitons traiter avec Mongoose une collection **MongoDB** nommée `users`, nous nommerons :
1. `userSchema` le schéma,
2. `User` le modèle,
3. `user` l’instance du modèle.

### Créer le modèle

Nous écrirons le modèle `User` dans un dossier `models/`, dans le fichier `user.js` :
```
models/
└── user.js
node_modules/
public/
views/
package-lock.json
package.json
server.js
```

Dans `user.js` :
1. nous commençons par charger **Mongoose** avec `const mongoose = require('mongoose')`,
1. puis nous créons le schéma `userSchema`,
1. ce schéma définit :
	1. le nom des propriété d’un `user`,
	1. le type de ces propriétés.
1. à l’avant-dernière ligne, nous créons le modèle `User` à partir du schéma `userSchema`,
1. à la dernière ligne, nous faisons de `user.js` un module qui représente le modèle `User` et nous pourrons charger ce module dans le contrôleur `server.js`.

```js
const mongoose = require('mongoose')
const Schema = mongoose.Schema

const userSchema = new Schema({
	name: String,
	password: String
}, { collection: 'users' })

const User = mongoose.model('User', userSchema)
module.exports = User
```

Il est à noter que nous avons donné au constructeur `Schema` l’argument `{ collection: 'users' }`. Ce deuxième, destiné à lier le schéma `userSchema` à la collection `users` n’était pas indispensable parce que :
1. quand nous nommons un schema `userSchema`,
1. il est implicite qu’il fait référence à la collection `users`.

Pour se servir du module dans `server.js`, nous l’importons :
```js
const User = require('./models/user')
```

Cela permet nous permet d’effectuer les quatre opérations CRUD :
1. créer des données,
1. lire des données,
1. mettre des données à jour,
1. supprimer des données.

### Poster des données avec Mongoose

Dans cette requête `POST`, nous utilisons le *middleware* `urlencodedParser` que nous avons créé précédemment. 

Avant d’enregistrer un nouvel utilisateur, nous interrogeons **MongoDB** avec la méthode `.findOne()` pour vérifier que le nom n’est pas déjà pris par un utilisateur. La méthode `.findOne()` prend en arguments :
1. le ou les paramètres de la recherche, ici `{ name: name }` que nous raccourcissons en `{ name }`,
1. une fonction *callback* dont les deux arguments sont une erreur éventuelle et l’objet éventuellement trouvé.

Si le nom n’est pas déjà pris, nous enregistrons le nouvel utilisateur avec la méthode `.save()`.
```js
app.post('/signup', urlencodedParser, (req, res) => {
	const { name, password } = req.body
	console.log(name, password)
	const newUser = new User({ name, password });
	User.findOne({ name }, (err, existingUser) => {
		if (err) {
			return res.status(500).send('Erreur du serveur.')
		}
		if (existingUser) {
			return res.status(500).send(`Le nom ${existingUser.name} est déjà utilisé`)
		}
		newUser.save((err, savedUser) => {
			if (err) {
				return res.status(500).send('Erreur du serveur.')
			} else {
				res.status(201).send(`${savedUser.name} enregistré avec succès !`)
			}
		})
	})
})
```

Les requêtes de **Mongoose** retournent toujours en premier une éventuelle erreur et nous commençons toujours par tester son éventuelle existence avec `if (err) {...}`.

### Lire des données avec Mongoose

Obtenir tous les `users` nécessite la méthode `.find()` :
```js
app.get('/user', (req, res) => {
	User.find({}, (err, users) => {
		if (err) {
			return res.status(500).send('Erreur du serveur')
		}
		return res.send(users)
	}).select('_id name')
})
```

Pour n’obtenir qu’un `user` en particulier :
1. nous récupérons son `_id` dans l’URL de la requête,
1. puis nous utilisons la méthode `.findOne()`.

```js
app.get('/user/:_id', (req, res) => {
	const { _id } = req.params
	User.findById(_id, (err, user) => {
		if (err) {
			return res.status(500).send('Erreur du serveur')
		}
		return res.send(user)
	}).select('_id name created')
})
```

### Modifier des données avec Mongoose

Nous modifions les données d’un utilisateur :
1. en obtenant son `_id` dans l’URL de la requête,
1. en utilisant la méthode `.findByIdAndUpdate()`.

```js
app.put('/user/:_id', urlencodedParser, (req, res) => {
	const { _id } = req.params
	const { name, password } = req.body
	User.findByIdAndUpdate(_id, { $set: { name, password } }, { new: true }, (err, user) => {
		if (err) {
			return res.status(500).send('Erreur du serveur')
		}
		if (!user)  {
			return res.status(404).send(`Il n’y a pas d’utilisateur ${_id}`)
		}
		return res.send(`Utilisateur ${user._id} modifié : ${user}`)
	})
})
```

### Supprimer des données avec Mongoose

La suppression d’un document **MongoDB** peut se faire avec la méthode `.findByIdAndDelete()`, toujours après avoir récupéré l’`_id` de ce document dans l’URL :
```js
app.delete('/user/:_id', (req, res) => {
	const { _id } = req.params
	User.findByIdAndDelete(_id, (err, user) => {
		if (err) {
			return res.status(500).send('Erreur du serveur')
		}
		if (!user) {
			return res.status(404).send(`Il n’y a pas d’utilisateur ${_id}`)
		}
		return res.send(`L’utilisateur ${user._id} a bien été supprimé`)
	})
})
```

## Transformer une callback en promesse

En **ES5**, l’asynchronicité était traitée avec les fonctions **callbacks**. Mais trop de *callbacks* qui font appel à des *callbacks* qui font appel à des *callbacks* font basculer notre code dans le *callback hell* (“l’enfer de la *callback*”).

Les promesses font partie d’**ES6** et permettent parfois d’écrire un code plus lisible parce que cela permet de lire du code asynchrone à la manière d’un code syncrhone.

Pour transformer une fonction *callback* en promesse, nous utiliserons `async...await` et `try...catch`. Voici par exemple notre reqête get :
```js
app.get('/user', (req, res) => {
	User.find({}, (err, users) => {
		if (err) {
			return res.status(500).send('Erreur du serveur')
		}
		return res.send(users)
	}).select('_id name')
})
```

Pour transformer la fonction *callback* en promesse, nous la précédons du mot-clé `async`. Dans cette promesse, nous écrivons :
1. une déclaration `try` qui traitera le résultat de la promesse,
1. une déclaration `catch` qui capturera une éventuelle erreur de la promesse.

Dans le `try` :
1. nous initialisons la constante `users` avec le mot-clé `await` indiquant que cette constante sera le résultat de la promesse,
1. le code qui suivra cette constante ne sera exécuté que quand le résultat de la promesse sera parvenu.

```js
app.get('/user', async (req, res) => {
	try {
		const users = await User.find({}).select('_id name')
		return res.send(users)
	} catch (err) {
		return res.status(500).send('Erreur du serveur')
	}
})
```

Voici la totalité de `server.js` après avoir remplacé les fonctions *callbacks* des routes en promesses :
```js
const express = require('express')
const app = express()
const helmet = require('helmet')
const bodyParser = require('body-parser')
const urlencodedParser = bodyParser.urlencoded({ extended: false })

const mongoose = require('mongoose')
mongoose.set('useFindAndModify', false)
mongoose.connect('<taper-ici-l-adresse-et-le-mot-de-passe-de-la-base-de-données>', 
	{useNewUrlParser: true})
const db = mongoose.connection
db.on('error', console.error.bind(console, 'ERROR: CANNOT CONNECT TO MONGO-DB'))
db.once('open', () => {
	console.log('SUCCESS: CONNECTED TO MONGO-DB')
})

const User = require('./models/user')

app.use(helmet())
app.use(express.static('public'))

app.post('/signup', urlencodedParser, async (req, res) => {
	const { name, password } = req.body
	const newUser = new User({ name, password })
	try {
		const existingUser = await User.findOne({ name })
		if (existingUser) {
			return res.status(400).send(`Le nom ${existingUser.name} est déjà utilisé`)
		}
	} catch (err) {
		return res.status(500).send('Erreur du serveur')
	}
	try {
		const savedUser = await newUser.save()
		res.status(201).send(`${savedUser.name} enregistré avec succès avec l’ID ${savedUser._id} !`)
	} catch (err) {
		return res.status(500).send('Erreur du serveur')
	}
})

app.get('/user', async (req, res) => {
	try {
		const users = await User.find({}).select('_id name')
		return res.send(users)
	} catch (err) {
		return res.status(500).send('Erreur du serveur')
	}
})

app.get('/user/:_id', async (req, res) => {
	const { _id } = req.params
	try {
		const user = await User.findById(_id).select('_id name created')
		return res.send(user)
	} catch (err) {
		console.log(err)
		return res.status(500).send('Erreur du serveur')
	}
})

app.put('/user/:_id', urlencodedParser, async (req, res) => {
	const { _id } = req.params
	const { name, password } = req.body
	try {
		const user = await User.findByIdAndUpdate(_id,  { $set: { name, password } }, { new: true })
		if (!user)  {
			return res.status(404).send(`Il n’y a pas d’utilisateur ${_id}`)
		}
		return res.send(`Utilisateur ${user._id} modifié`)
	} catch (err) {
		return res.status(500).send('Erreur du serveur')
	}
})

app.delete('/user/:_id', async (req, res) => {
	const { _id } = req.params
	try {
		const user = await User.findByIdAndDelete(_id)
		if (!user) {
			return res.status(404).send(`Il n’y a pas d’utilisateur ${_id}`)
		}
		return res.send(`L’utilisateur ${user._id} a bien été supprimé`)
	} catch (err) {
		return res.status(500).send('Erreur du serveur')
	}
})

app.get('*', (req, res) => {
	res.status(404).send('Cette page n’existe pas !')
})

app.listen(3000, () => console.log('Serveur lancé sur le port 3000'))
```

Nous allons maintenant restreindre l’accès à l’administration des `users`. L’utilisateur ne pourra accéder à <http://localhost:3000/user> que s’il s’est correctement authenfié à <http://localhost:3000/signin>.

## Authentifier l’utilisateur avec Passport et express-session

Pour protéger une route par une authentification :
1. nous utilserons le *middleware* **Passport.js**,
1. **Passport.js** aura une stratégie “locale”,
1. les sessions de l’utilisateur seront maintenues par le package `express-session`.

### Installer et configurer les dépendances

Pour cela, nous ajoutons trois modules au projet `3-mongoose`  :
```
npm install --save express-session passport passport-local
```

Dans `server.js` :
1. nous chargeons **Passport.js** avec `require('passport')` et `express-session` avec `require('express-session')`,
1. nous définissons la stratégie locale avec `require('passport-local').Strategy`,
1. nous passons `express-session` en *middleware* de l’application avec `app.use(session())` et nous lui fournissons un objet de configuration contenant notamment une phrase secrète,
1. nous passons **Passport.js** en *middleware* de l’application avec `app.use(passport.initialize())` et `app.use(passport.session())`,
1. nous configurons **Passport.js** avec `passport.serializeUser()` et `passport.deserializeUser()`.

```js
const passport = require('passport')
const session = require('express-session')
const LocalStrategy = require('passport-local').Strategy

app.use(session({ secret: '<taper-ici-une-phrase-secrete>', resave: false, 
	saveUninitialized: false }))
app.use(passport.initialize())
app.use(passport.session())

passport.serializeUser((user, done) => {
	done(null, user);
});
passport.deserializeUser((user, done) => {
	done(null, user)
})
```

Pour finir l’installation de **Passport.js**, nous configurons notre stratégie d’authentification, c’est-à-dire son déroulement :
```js
passport.use(new LocalStrategy({
		usernameField: 'name'
	},
	(username, password, done) => {
		User.findOne({ name: username }, (err, user) => {
			if (err) {
				return done(err)
			}
			// Return if user not found in database
			if (!user) {
				return done(null, false, {
					message: 'User not found'
				})
			}
			// Return if password is wrong
			if (!user.validPassword(password)) {
				return done(null, false, {
					message: 'Password is wrong'
				})
			}
			// If credentials are correct, return the user object
			return done(null, user)
		})
	}
))
```

Dans ce que nous venons d’écrire :
1. `{ usernameField: 'name' }` est nécessaire parce que l’identifiant de nos utilisateur est la propriété `name` du modèle Mongoose, cet objet de configuration n’aurait pas été nécessaire si cette propriété s’était appelée `username`,
1. avec `User.findOne()`, nous recherchons un utilisateur qui dont la propriété `name` a la même valeur que la valeur du nom fournie par le formulaire de connexion,
1. nous traitons les erreurs du serveur avec `if (err) {...}`,
1. les instruction de `if (!user) {...}` seront exécutées si l’utilisateur n’est pas trouvé,
1. les instruction de `if (!user.validPassword(password)) {...}` seront exécutées si le mot de passe n’est pas le bon.

### Login

Nous rendons la vue `login.pug` contenant le formulaire de connexion :
```js
app.get('/login', (req, res) => {
	res.render('login.pug')
})
```

Pour rappel, ce formulaire poste ses données à <http://localhost:3000/signin> :
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

C’est pour cela qu’il faut ajouter la route `app.post('/signin')` à `server.js`. Cette route utilse les *middlewares* :
1. `urlencodedParser` pour obtenir les données postées par l’utilisateur,
1. `passport.authenticate()` pour authentifier l’utilisateur.

```js
app.post('/login', urlencodedParser, passport.authenticate('local', { 
	successRedirect: '/user',
	failureRedirect: '/login'
}))
```

Une fois que `passport.authenticate()` a appliqué la stratégie locale (notre stratégie d’authentification), il redirige ici l’utilisateur :
1. vers la page <http://localhost:3000/user> si l’authentification a réussi,
1. vers la page de connexion <http://localhost:3000/login> pour qu’il tent à nouveau de se connecter si l’authentification a échoué.

Il nous reste à protéger la route `/user` pour n’autoriser son accès qu’aux utilisateurs authentifiés par **Passport**.

### Protéger une route

Quand **Passport** a auhtentifié un utilisateur, il a ajouté la propriété `.user` à la requête. Pour protéger l’accès à `/user`, il suffit donc au tout de la fonction *callback* d’`app.get()` de vérifier si la requête a la propriété `.user` et à rediriger l’utilisateur le cas échéant :
```js
app.get('/user', async (req, res) => {
	if (!req.user) return redirect('/login')
	try {
		const users = await User.find({}).select('_id name')
		return res.send(users)
	} catch (err) {
		return res.status(500).send('Erreur du serveur')
	}
})
```

### Logout

Pour déconnecter l’utilisateur, nous utilisons la méthode `req.logout()` proposée par **Passport** dans une route dédiée à la déconnexion, puis nous le redirigeons vers une autre page de notre choix :
```js
app.get('/logout', (req, res) => {
	req.logout();
	res.redirect('/login');
})
```