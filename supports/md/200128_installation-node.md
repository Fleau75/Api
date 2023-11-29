# Installation de Node

## Installation de Node Windows

<https://github.com/coreybutler/nvm-windows>

## Installation de Node Mac

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