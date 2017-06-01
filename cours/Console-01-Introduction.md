Introduction à la ligne de commande
===================================

Document écrit par Nicolas Kovacs <info@microlinux.fr>

Pour travailler en ligne de commande, vous devez tout d'abord vous retrouver
face à une "invite de commande" (*command prompt*), c'est-à-dire quelque chose
qui ressemble à ceci :

```
[kikinovak@centosbox ~]$ _
```

Si nous nous sommes connectés en tant que `root`, cette invite se présente
un peu différemment :

```
[root@centosbox ~]# _
```

Se connecter à un serveur Linux depuis Windows avec PuTTY
---------------------------------------------------------

PuTTY est un client SSH libre pour Windows, écrit par Simon Tatham et publié
sous licence MIT. Téléchargez-le sur la [page du projet](http://www.putty.org),
installez-le et lancez-le. 

Pour ouvrir une session distante avec PuTTY, je dois lui fournir quelques
paramètres de base comme le nom d'hôte ou l'adresse IP, le port et le type de
connexion.

  * Host Name : `centosbox`

  * IP address : `192.168.2.20` (si la machine n'est pas joignable par nom
    d'hôte)

  * Port : 22

  * Connection type : SSH

  * Cliquer sur *Open*

PuTTY affiche un avertissement quant à la clé publique du serveur distant. Nous
décidons de lui faire confiance en cliquant sur *Oui*. À partir de là, la
console de PuTTY nous demande de nous identifier.

```
login as: kikinovak
kikinovak@centosbox's password: ********
Last login: Wed May 24 08:14:24 2017 from alphamule.microlinux.lan
[kikinovak@centosbox ~]$ _
```

Se connecter à un serveur Linux depuis Mac OS X
-----------------------------------------------

Pour nous connecter au serveur Linux depuis un poste de travail tournant sous
Mac OS X, nous invoquerons SSH depuis un émulateur de terminal. 

Sous Mac OS X, rendez-vous dans le dossier `Applications/` et repérez
l'application `Terminal` dans le sous-dossier `Utilitaires/`. 

Pour me connecter en tant qu'utilisateur `kikinovak` sur le serveur, j'invoque
la commande suivante.

```
$ ssh kikinovak@centosbox
```

Pour fournir l'adresse IP plutôt que le nom d'hôte, la syntaxe sera la
suivante.

```
$ ssh kikinovak@192.168.2.20
```

Pour me connecter en tant qu'administrateur `root`, la commande ressemblera à
ceci.

```
$ ssh root@centosbox
```

Lors de la première connexion, SSH m'affiche un avertissement quant à
l'identité de la machine. Répondez par l'affirmative.


Basculer entre les consoles virtuelles
--------------------------------------

Si vous êtes connecté physiquement au serveur - autrement dit, si vous n'avez
pas ouvert une session distante - vous pouvez basculer entre les consoles
virtuelles. 

La commande `tty` affiche le nom du terminal associé à l'entrée standard. 

```
[kikinovak@centosbox ~]$ tty
/dev/tty1
```

Utilisez le raccourci clavier **Alt**+**F2** pour basculer vers la deuxième
console virtuelle. Connectez-vous et invoquez la commande `tty`.

```
CentOS Linux 7 (Core)
Kernel 3.10.0-514.el7.x86_64 on an x86_64

centosbox login: kikinovak
Password: ********
[kikinovak@centosbox ~]$ tty
/dev/tty2
```

Dans la configuration par défaut, CentOS dispose de six consoles virtuelles
auxquelles vous accédez par le raccourci clavier **Alt**+**F1**, **Alt**+**F2**
et ainsi de suite jusqu'à **Alt**+**F6**. 

Basculez vers la troisième console virtuelle (**Alt**+**F3**), connectez-vous
en tant que `root` et, là aussi, invoquez `tty`. Puis revenez vers la première
console avec **Alt**+**F1**. 


Quitter la console
------------------

Pour fermer la session et revenir à l'invite de connexion, invoquez la commande
suivante :

```
$ logout
```

Alternativement, vous pouvez également vous déconnecter comme ceci :

```
$ exit
```

Enfin, le raccourci clavier **Ctrl**+**D** permet de faire la même chose plus
rapidement.

Pour vous entraîner, connectez-vous successivement en tant que `root`, puis en
tant qu'utilisateur normal.


Premiers pas en ligne de commande
---------------------------------

Commencez par taper n'importe quoi et regardez la réaction de votre système.
Par exemple :

```
[kikinovak@centosbox ~]$ make love
make: *** Aucune règle pour fabriquer la cible « love ». Arrêt.
[kikinovak@centosbox ~]$ 
```

Vous venez de taper votre première commande (`tty` ne compte pas vraiment)...
et vous vous retrouvez face à votre premier message d'erreur. Mais que s'est-il
passé exactement ?

  * L'interpréteur de commandes vous a affiché une invite :
    `[kikinovak@centosbox ~]$`. Il vous a ainsi signifié qu'il était prêt à
    recevoir une ou plusieurs commandes.

  * Vous avez tapé une commande : `make`.

  * Vous avez fait suivre la commande `make` d'un argument : `love`.

  * L'interpréteur a essayé en vain d'exécuter ce que vous lui avez demandé de
    faire et vous a dit plus ou moins clairement ce qu'il en pense, en
    l'occurrence : `make: *** Aucune règle pour fabriquer la cible « love ».
    Arrêt.`

  * L'interpréteur de commandes vous affiche à nouveau l'invite, pour vous
    indiquer qu'il est prêt à recevoir une ou plusieurs commandes. 

À présent, nous n'avons qu'à essayer avec des commandes qui ont un peu plus de
sens pour votre machine. 


Le shell Bash et les autres
---------------------------

Le shell est un programme ayant pour fonction d'assurer l'interface entre
l'utilisateur et le système Linux. C'est un interpréteur de commandes. Les
systèmes Unix et Linux disposent de toute une panoplie de shells au choix.

  * le Bourne Shell (`sh`)

  * le C-Shell (`csh`)

  * le Korn Shell (`ksh`)

  * le Z Shell (`zsh`)

  * le Bourne Again Shell (`bash`)

Nous nous concentrerons sur le shell Bash, qui est devenu la norme sur les
systèmes Linux.

