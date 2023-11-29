# Transmettre des valeurs à Node

Contrairement au JS dans les navigateurs, Node n’a pas :

1. de DOM,
1. d’objet `window` (l’onglet du navigateur),
1. d’objet `document` (le document HTML).

En Node, l’objet qui serait l’équivalent de `window` est l’objet `process`. C’est lui qui exécute le code d’une application Node;

Cet objet fondamental de Node a deux propriétés que nous utiliserons pour transmettre des valeurs à notre application :

1. l’objet `process.env` qui contient les variables d’environnement, celles du système d’exploitation qui exécute notre application,
1. et le tableau `process.argv` qui contient les arguments d’exécution, ceux qui ont été passés à notre application au lancement de son exécution.

Nous créons le fichier `index.js` contenant :

```js
console.log(process.env)
console.log(process.argv)
```

Puis nous lançons cette commande dans le Terminal pour afficher `process.env` et `process.argv` :

```
$ node index.js
``` 

## Les variables d’environnement

Il est fréquent de définir le mode d’exécution des applications Node en déclarant une variable d’environnement :

1. qu’on nomme `NODE_ENV` par convention,
1. et à qui, également par convention, nous donnons les valeurs `dev` ou `production`.

Dans notre fichier `index.js`, nous déterminons quel est le mode d’exécution :

```js
const { NODE_ENV } = process.env
if (NODE_ENV === 'production') {
    console.log('L’application est en mode production')
}
if (NODE_ENV === 'dev') {
    console.log('L’application est en mode développement')
}
console.log('Mode :', NODE_ENV)
```

### Déclarer une variable d’environnement éphèmère

Pour initialiser une variable d’environnement :

1. nous déclarons son nom (ici `NODE_ENV`),
1. puis nous y accolons un égal `=` suivi de sa valeur.

```
$ NODE_ENV=production node index.js
$ echo $NODE_ENV
```

1. Cette variable d’environnement existe bien dans le fichier `index.js` exécuter par la ligne de commande,
1. mais quand nous souhaitons afficher cette variable avec la commande `echo` à la ligne suivante, celle-ci n’existe plus.

### Déclarer une variable d’environnement permanente

En précédant l’initialisation de `NODE_ENV` par la commande `export`, la variable d’environnement existe maintenant aux lignes suivantes :

```
$ export NODE_ENV=production
$ node index.js
$ echo $NODE_ENV
```

1. La variable d’environnement `NODE_ENV` est initialisée à la première ligne,
1. puis elle existe encore à la deuxième ligne, quand nous exécutons notre fichier `index.js`,
1. et elle existe toujours à la troisième ligne quand nous l’affichons avec la commande `echo`, en,précédant le nom de la variable d’un dollar `$`,
1. en fait la variable d’environnement déclarée avec `export` existera jusqu’à la fin session de la session du Terminal (la fermeture de la fenêtre du Terminal).

Pour supprimer une variable d’environnement initialisée avec la commande `export`, nous utilisons la commande `unset` :

```
$ export NODE_ENV=production
$ echo $NODE_ENV
$ unset NODE_ENV
$ echo $NODE_ENV
```

## Les arguments d’exécution

`process.argv` est un tableau de chaînes de caractères dont les deux premières valeurs sont :

1. l’emplacement de l’éxécutable Node,
1. l’emplacement du script.

```js
[
  '/Users/vincentcaronnet/.nvm/versions/node/v12.14.1/bin/node',
  '/Users/vincentcaronnet/Documents/process-sandbox/index.js'
]
```

Les arguments suivants sont accessibles à partir de l’index `2` du tableau, dans l’ordre de leurs déclarations :

```js
const [,,my_argument] = process.argv
console.log(my_argument)
```

Les arguments d’exécution sont passés au script en les déclarant après son nom.

Nous lançons ces deux commandes dans le Terminal :

```
$ node index.js hello node
$ node index.js "hello node"
```

1. La première ligne n’affiche que `hello` :
    1. parce qu’il y a une espace entre `hello` et `node`,
    1. il y a donc deux arguments (`node` et `hello`) et `node` est le premier de ces deux arguments.
1. la deuxième ligne affiche `hello node`,
1. ce qui veut dire qu’il faut entourer de guillemets droits `""` les chaînes de caractères qui contiennent des espaces.

## Le module dotenv

Le module `dotenv` permet d’initialiser nos variables d’environnement :

1. sans le faire dans le Terminal,
1. mais dans un fichier de notre projet.

Nous pouvons ainsi déclarer dans ce fichier :

1. le port d’une application Express,
1. les identifiants de connexion d’une base de données,
1. ou toute donnée sensible, puisqu’il suffit de ne pas commiter ce fichier pour rendre public sans risque le dépôt GitHub de notre application.

### Installation de dotenv

```
$ npm init -y
$ npm install --save dotenv
```

### Déclaration et initialisation des variables d’environnement avec dotenv

Nous créons dans le projet le fichier `.env` et y écrivons nos variables :

```
NODE_ENV=dev
APP_DOMAIN=localhost
APP_PORT=3000
DB_DOMAIN=mongodb.net
DB_PORT=27017
DB_USER=vincent
DB_PASSWORD=azerty1234
DB_NAME=blog
DB_TABLE=users
```

### Accès aux variables d’environnement initialisées avec dotenv

Dans `index.js` :

1. nous chargeons les variables du fichier `.env` avec la méthode `require('dotenv').config()`
1. nos variables d’environnement sont accessible comme d’habitude, dans l’objet `process.env`.s

```js
require('dotenv').config()
const { NODE_ENV,
    APP_DOMAIN,
    APP_PORT,
    DB_DOMAIN,
    DB_PORT,
    DB_USER,
    DB_PASSWORD,
    DB_NAME } = process.env
// Ces constantes sont alors utilisables dans le reste du code
```