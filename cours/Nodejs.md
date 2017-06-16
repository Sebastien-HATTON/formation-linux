Installation de Node.js sous CentOS 7
=====================================

Document écrit par Nicolas Kovacs <info@microlinux.fr>

[Node.js](https://nodejs.org/) est une plate-forme de programmation JavaScript.
Elle permet d'exécuter du JavaScript côté serveur. Sur un système CentOS, elle
pourra nous servir pour mettre en place un environnement d'édition pour
Markdown, par exemple.

Prérequis
---------

Le site [NodeSource](https://nodesource.com/) met à disposition un script pour
tester les différents prérequis pour l'installation de Node.js. Télécharger et
exécuter ce script.

```
# curl --silent --location https://rpm.nodesource.com/setup_6.x | bash -

## Inspecting system...

...

## Run `yum install -y nodejs` (as root) to install Node.js 6.x and npm. 
## You may also need development tools to build native addons: 
##   `yum install -y gcc-c++ make`
```

On va suivre la suggestion du script et installer les deux outils de
développement.

```
# yum install gcc-c++ make
```

Les paquets `nodejs` et `npm` sont fournis par le dépôt
[EPEL](https://fedoraproject.org/wiki/EPEL), qu'il faudra avoir configuré au
préalable.

```
# yum install nodejs npm 
```

Tester l'installation
---------------------

Afficher la version de Node.js.

```
$ node --version
v6.10.3
```

Faire de même avec NPM.

```
$ npm --version
3.10.10
```

Pour tester Node.js, on va éditer un fichier `~/demo_server.js` comme ceci.

```
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Welcome Node.js');
}).listen(3001, "127.0.0.1");
console.log('Server running at http://127.0.0.1:3001/');
```

Ce code contient un petit serveur web censé afficher du texte. Démarrons-le.

```
$ node --debug demo_server.js 
Debugger listening on [::]:5858
Server running at http://127.0.0.1:3001/
```

À présent, nous pouvons afficher le résultat avec un navigateur web.

```
# links http://127.0.0.1:3001/

        ===============
        
        Welcome Node.js
        
        ===============
```
