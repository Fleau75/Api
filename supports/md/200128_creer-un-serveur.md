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