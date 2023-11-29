# Express

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