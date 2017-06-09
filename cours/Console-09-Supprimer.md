Supprimer : rm et rmdir
=======================

Document écrit par Nicolas Kovacs <info@microlinux.fr>

Gare aux armes de destruction massive
-------------------------------------

La commande `rm` (comme *remove*) sert à supprimer des fichiers et des
arborescences de répertoires. Accessoirement, elle vous permet de vous tirer
dans le pied, car elle est capable d'anéantir des dizaines de sites web, des
années de courriels archivés, voire un serveur entier en un tournemain. Dans la
panoplie des outils Unix, `rm` fait partie des instruments affûtés et
tranchants qu'il convient de manier avec précaution. 

Pour supprimer un fichier, il suffit de spécifier son nom en argument.

```
$ rm bonjour.txt
```

Vous n'obtiendrez pas de demande de confirmation du genre *Êtes-vous sûr de...*
ou autres *Voulez-vous vraiment...* Votre système Linux n'a rien d'une nounou
qui vous prend par la main. Vous lui avez ordonné de supprimer le fichier
`bonjour.txt` et c'est sans broncher qu'il s'est exécuté pour l'envoyer au
paradis des octets. Ici, vous ne trouvez pas de `Corbeille` non plus, où vous
auriez pu repêcher vos données malencontreusement supprimées.

Notez au passage que sur un poste de travail Linux, les environnements de
bureau comme KDE, GNOME, Xfce ou MATE disposent bien d'une `Corbeille` qui
permet de repêcher des fichiers supprimés en mode graphique par le biais du
navigateur de fichiers. En revanche, un fichier supprimé en ligne de commande
sera perdu à jamais. 


Travailler avec ou sans filet ?
-------------------------------

Si ces manières expéditives vous mettent mal à l'aise, vous pouvez toujours
invoquer `rm` avec l'option `-i` (comme *interactive*), ce qui produira une
demande de confirmation avant chaque destruction de fichier. Tapez **O** pour
répondre "oui".

```
$ rm -i bonjour2.txt 
rm : supprimer fichier « bonjour2.txt » ? o
```

Ce fonctionnement peut être implémenté par défaut par ce qu'on appelle les
alias de commande. 


Les alias de commande
---------------------

Invoquez la commande suivante.

```
$ alias
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'
```

Ici, chaque entrée commençant par `alias` correspond à la définition d'un
raccourci de commandes, le but du jeu étant visiblement d'ajouter un peu de
couleur ou de vous simplifier l'utilisation de ces quelques commandes. Vous
vous apercevez que le simple `ls` que vous avez pu invoquer jusqu'ici est
*enrichi* par défaut avec `--color=auto`.

Pour la plupart, les distributions Linux grand public prédéfinissent plusieurs
alias de commandes, afin de rendre l'utilisation du *shell* un peu plus
agréable. Essayons d'en définir un nous-même.

```
$ alias rm='rm -i'
$ touch fichierbidon
$ rm fichierbidon 
rm : supprimer fichier vide « fichierbidon » ? o
```

Il se peut que vous soyez confronté au cas de figure inverse, c'est-à-dire un
alias de commande `rm -i` qui serait défini pour `rm` dans votre environnement
de travail, alors que vous souhaitiez supprimer des fichiers directement, sans
avoir à passer par la confirmation de suppression. Dans ce cas, utilisez `rm`
avec l'option `-f` comme *force*.

```
$ alias rm
alias rm='rm -i'
$ touch fichierbidon
$ rm -f fichierbidon
```

Notons que notre définition d'alias n'est pas persistante. Autrement dit,
lorsque vous démarrerez une nouvelle session, en vous déconnectant et en vous
reconnectant par exemple, votre alias individualisé aura disparu. La
personnalisation persistante du *shell* sera abordée un peu plus loin.


Supprimer des répertoires avec rmdir
------------------------------------

De façon analogue à la commande `rm`, `rmdir` (*remove directory*) sert à
supprimer des répertoires du système de fichiers.

```
$ mkdir repertoirebidon
$ ls -ld repertoirebidon/
drwxrwxr-x. 2 kikinovak kikinovak 6  9 juin  09:58 repertoirebidon/
$ rmdir repertoirebidon/
```

Le répertoire que vous souhaitez supprimer doit impérativement être vide. Dans
le cas contraire, `rmdir` refuse de s'exécuter et vous obtenez un message
d'erreur.

```
$ mkdir repertoirebidon
$ touch repertoirebidon/fichierbidon
$ rmdir repertoirebidon/
rmdir: échec de suppression de « repertoirebidon/ »: Le dossier n'est pas vide
```

Dans ce cas, c'est-à-dire si l'on souhaite supprimer un répertoire ainsi que
tout son contenu, on peut avoir recours à la commande `rm` suivie de l'option
`-r` (comme *recursive*). Dans l'exemple suivant, j'ajoute l'option `-i` pour
bien expliciter chaque opération de suppression.

```
$ mkdir repertoirebidon
$ touch repertoirebidon/fichierbidon
$ rm -ri repertoirebidon/
rm : descendre dans le répertoire « repertoirebidon/ » ? o
rm : supprimer fichier vide « repertoirebidon/fichierbidon » ? o
rm : supprimer répertoire « repertoirebidon/ » ? o
```

Dans la pratique quotidienne, c'est plutôt l'inverse que l'on souhaite faire.
Il est souvent fastidieux de confirmer chaque suppression dans une arborescence
de répertoires. Dans certains cas de figure, c'est même impossible. Lorsque
vous décidez par exemple de nettoyer vos anciens répertoires de sources du
noyau, il vous faudrait confirmer quelques dizaines de milliers d'opérations de
suppression. C'est là que l'option `-f` (*force*) intervient. 

```
$ alias rm='rm -i'
$ mkdir repertoirebidon
$ touch repertoirebidon/fichierbidon
$ rm -rf repertoirebidon/
```

Comme l'exemple précédent vous le montre, `rm -rf` ne vous demande pas votre
avis et anéantit joyeusement tout ce que vous spécifiez, à condition que vous
en ayez le droit, bien sûr. Invoquée en tant que `root`, la commande `rm -rf`
peut même vous faire commettre l'équivalent numérique d'un Seppuku. 


Un peu de ménage
----------------

Pour vous exercer avec `rm` et `rmdir`, faites un peu de nettoyage dans votre
répertoire d'utilisateur. Effacez tous les résidus des exercices que nous avons
pu faire jusqu'ici. 


Un coup d'essuie-glace avec clear
---------------------------------

J'en profite pour vous montrer l'équivalent d'un coup d'essuie-glace dans votre
terminal. Remplissez ce dernier avec n'importe quelle commande susceptible de
bien l'encombrer (par exemple `ls /etc`), puis essayez ceci.

```
$ clear
```

Pour aller plus vite, utilisez simplement le raccourci clavier **Ctrl**+**D**,
ce qui revient au même.



