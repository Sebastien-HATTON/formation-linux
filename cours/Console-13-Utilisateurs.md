Gérer les utilisateurs
======================

Document écrit par Nicolas Kovacs <info@microlinux.fr>

Tout comme Unix, Linux a été conçu dès le départ comme un vrai système
multi-utilisateur. Gérer les utilisateurs revient à définir qui a accès à quoi
dans un système Linux.

Systèmes mono-utilisateurs et systèmes multi-utilisateurs
---------------------------------------------------------

Linux est un vrai système mulit-utilisateur, tout comme son ancêtre Unix. Pour
comprendre la portée de cette assertion, imaginez un poste de travail comme on
peut en trouver dans la salle informatique d'une grande université, fréquentée
par une bonne dizaine de milliers d'étudiants. Chaque étudiant inscrit a le
droit d'utiliser les machines de la salle informatique. Il possède donc son
identifiant personnel et son mot de passe, qui lui permettent de se connecter à
une machine de la salle informatique pour y travailler, c'est-à-dire effectuer
ses recherches, écrire ses devoirs, rédiger son mémoire ou sa thèse, etc. Une
telle installation doit répondre à quelques exigences.

  * Chaque utilisateur du système doit disposer de son répertoire personnel,
    c'est-à-dire d'un endroit pour lui seul, utilisable par lui seul, où il
    peut stocker toutes ses données.

  * La confidentialité doit être assurée, c'est-à-dire qu'un étudiant connecté
    ne pourra pas aller fouiner librement dans les données de ses collègues.

  * Il ne faut pas non plus qu'un utilisateur puisse effacer *par mégarde* (ou
    même intentionnellement) les données qui ne lui appartiennent pas.

  * Enfin, l'intégrité du système ne doit en aucun cas être mise en péril par
    les utilisateurs.

Techniquement parlant, une telle installation dans une université
se différencie d'une installation domestique d'un poste de travail par la
configuration *itinérante* des profils d'utilisateurs. Dans une telle
configuration, l'ensemble des données, les identifiants de connexion et les
mots de passe sont stockés de façon centralisée sur le serveur. À partir de là,
chaque étudiant peut se connecter sur n'importe quelle machine de la salle
informatique et retrouver son environnement, alors que sur un poste de travail
à la maison, chaque compte d'utilisateur reste lié à la machine locale. Il
existe plusieurs manières de mettre en place les profils itinérants dans un
réseau local, et nous les aborderons en temps et en heure. Pour l'instant, nous
nous concentrons sur la gestion des utilisateurs sur un serveur Linux.


Ajouter de nouveaux utilisateurs : useradd
------------------------------------------

Lors de la configuration post-installation de notre système, j'ai défini un
premier utilisateur du "commun des mortels" pour la machine. Cela signifie que
ma machine connaît déjà deux comptes : l'administrateur `root` et l'utilisateur
en question (`kikinovak`). 

En dehors de mon utilisateur initial, je vais créer quelques comptes
supplémentaires.

  * Agnès Debuf (`adebuf`)

  * Jean Mortreux (`jmortreux`)

  * Fanny Banester (`fbanester`)

  * Franck Teyssier (`fteyssier`)

Chacun des utilisateurs sera créé à l'aide de la commande `useradd`.
L'invocation de cette commande requiert des droits d'administrateur. Dans un
premier temps, nous allons acquérir ces droits de façon peu élégante, en nous
déconnectant et en nous reconnectant en tant que `root`.

Je lance la création de ma première utilisatrice.

```
# useradd -c "Agnès Debuf" adebuf
# passwd adebuf
Changement de mot de passe pour l'utilisateur adebuf.
Nouveau mot de passe : 
Retapez le nouveau mot de passe : 
passwd : mise à jour réussie de tous les jetons d'authentification.
```

Un coup d'oeil rapide dans la page `man` de `useradd(8)` vous renseigne sur la
signification exacte de l'option `-c`, que vous avez probablement devinée dans
le contexte.

```
-c, --comment COMMENTAIRE
    Toute chaîne de texte. C'est généralement une description courte du compte,
    elle est actuellement utilisée comme champ pour le nom complet de
    l'utilisateur.
```

Notez qu'il y a une vérification sur la robustesse du mot de passe défini. Pour
un utilisateur quelconque, le système refuserait tout simplement de créer le
mot de passe s'il est faible (puisque la commande `passwd` peut être invoquée
par un utilisateur pour changer son propre mot de passe). En revanche, `root` a
tous les droits, y compris celui ce forcer l'utilisation d'un mot de passe trop
simple (ce que je vous déconseille néanmoins). 

```
# useradd -c "Jean Mortreux" jmortreux
# passwd jmortreux
Changement de mot de passe pour l'utilisateur jmortreux.
Nouveau mot de passe : 
MOT DE PASSE INCORRECT : Le mot de passe comporte moins de 8 caractères
Retapez le nouveau mot de passe : 
passwd : mise à jour réussie de tous les jetons d'authentification.
```

Procédez de même pour créer les autres utilisateurs de la machine.


Utiliser n'est pas administrer
------------------------------

Tout au long de notre initiation à la ligne de commande, nous avons travaillé
en tant que simples utilisateurs (à quelques rares exceptions près) pour créer,
éditer, visualiser, déplacer, copier et effacer des fichiers. Ces tâches ne
mettaient pas en péril le fonctionnement du système ou les données des autres
utilisateurs et ne nécessitaient par conséquent aucun privilège spécifique. Il
n'en est plus de même pour la gestion des utilisateurs, qui comprend entre
autres choses :

  * la création d'un nouvel utilisateur ;

  * la définition de son mot de passe ;

  * la configuration de ses droits : à quoi aura-t-il accès dans le système ?

  * la suppression éventuelle de l'utilisateur ainsi que de toutes ses données.


Changer d'identité et devenir root
----------------------------------

Lors de l'installation du système, nous avons défini un mot de passe pour
l'utilisateur `root`. Un peu plus haut, nous avons eu besoin des privilèges de
`root` pour créer quelques utilisateurs supplémentaires, que nous avons acquis
en nous déconnectant et en nous reconnectant. Or, il existe un moyen bien plus
simple grâce à la commande `su` (*switch user*, c'est-à-dire "changer
d'utilisateur"). Tapez ce qui suit, en saisissant votre mot de passe `root`
lorsque le système vous le demande.

```
[kikinovak@centosbox ~]$ su -
Mot de passe : 
Dernière connexion : jeudi 15 juin 2017 à 14:01:26 CEST sur pts/0
[root@centosbox ~]# 
```

Notez le tiret `-` qui suit la commande `su`. Il précise qu'il faut devenir
`root` en récupérant toutes les variables d'environnement de ce compte. Nous y
reviendrons. Contentez-vous pour l'instant de connaître la démarche.

Une mise en garde solennelle s'impose. En acquérant les droits de `root`, vous
voilà en quelque sorte détenteur du fameux bouton rouge. Cela ne veut pas dire
que vous allez forcément déclencher une guerre nucléaire, mais une simple
commande bien sentie suffirait à enclencher une apocalypse numérique sur votre
système. En un mot : prudence. Et gare aux fautes de frappe.

S'il est utile de savoir comment acquérir les pleins pouvoirs sur la machine,
il est tout aussi indispensable de savoir comment revenir en sens inverse pour
se débarrasser de tous ces super-pouvoirs lorsqu'on n'en a plus besoin. Dans ce
cas, c'est exactement la même commande que pour quitter une session dans la
console. Vous avez donc le choix entre les deux commandes `logout` et `exit`, à
moins que vous ne préfériez le raccourci clavier **Ctrl**+**D**. 

```
[root@centosbox ~]# exit
déconnexion
[kikinovak@centosbox ~]$ 
```

Savoir qui l'on est
-------------------

La commande `su` ne nous permet pas seulement de devenir `root`. Si le système
dispose d'un utilisateur `fteyssier`, je pourrais très bien *devenir*
`fteyssier` en invoquant la commande suivante (et en saisissant son mot de
passe).

```
[kikinovak@centosbox ~]$ su - fteyssier
Mot de passe : 
[fteyssier@centosbox ~]$ 
```

Là encore, notez l'utilisation du tiret `-` pour indiquer que vous souhaitez
devenir un autre utilisateur en utilisant ses variables d'environnement.
L'invite de commandes (`[fteyssier@centosbox ~]$`) nous indique qu'un
changement d'identité a eu lieu. Pour le vérifier, nous avons tout loisir de
demander à notre système qui nous sommes, grâce à la commande `whoami` (*Who am
I ?", "Qui suis-je ?"). Voici une petite démonstration pratique.

```
$ su - fteyssier
Mot de passe : 
[fteyssier@centosbox ~]$ whoami
fteyssier
[fteyssier@centosbox ~]$ exit
déconnexion
[kikinovak@centosbox ~]$ whoami
kikinovak
[kikinovak@centosbox ~]$ su -
Mot de passe : 
[root@centosbox ~]# whoami
root
[root@centosbox ~]# exit
déconnexion
[kikinovak@centosbox ~]$ whoami
kikinovak
```

Vous remarquerez que si j'invoque `su` sans autre argument que le tiret, cela
revient exactement à la même chose que `su - root`.

```
[kikinovak@centosbox ~]$ su -
Mot de passe : 
[root@centosbox ~]# 
```

En savoir un peu plus sur les utilisateurs : id, groups, finger
---------------------------------------------------------------

Chacun des utilisateurs que nous avons créés jusqu'ici possède un certain
nombre de caractéristiques : son UID unique, son GID, les groupes secondaires
auxquels il appartient, son répertoire d'utilisateur, son *shell* de connexion,
etc. Voyons maintenant comment afficher ces différentes informations.
Commençons par nous-mêmes, en utilisant la commande `id`.

```
$ id
uid=1000(kikinovak) gid=1000(kikinovak) groupes=1000(kikinovak),10(wheel)
contexte=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
```

Invoquée sans autre argument, la commande `id` nous affiche l'UID, le GID,
ainsi que la liste complète des groupes secondaires auxquels l'utilisateur est
affecté. Elle nous affiche également le contexte SELinux (*Security Enhanced
Linux*), que nous allons laisser de côté pour l'instant. SELinux est une
technologie relativement complexe, et nous l'aborderons plus loin. 

Afficher l'UID (*User Identification*) de l'utilisateur :

```
$ id -u
1000
```


Afficher le GID (Group Identification) :

```
$ id -g
1000
```

Afficher le nom du groupe :

```
$ id -gn
kikinovak
```

Afficher les groupes dont l'utilisateur est membre :

```
$ id -G
1000 10
```

Afficher les noms des groupes dont l'utilisateur est membre :

```
$ id -Gn
kikinovak wheel
$ groups
kikinovak wheel
```

Vous noterez que pour cette dernière commande, vous disposez de deux
alternatives possibles.

Évidemment, personne ne vous demande de retenir toutes ces options par coeur.
N'oubliez pas que vous avez la page du manuel pour cela. 

```
$ man id
```

Pour en savoir plus sur les autres utilisateurs du système, il suffit de
fournir leur nom en argument. Ces informations sont accessibles à tous les
utilisateurs non privilégiés du système.

```
$ id adebuf
uid=1001(adebuf) gid=1001(adebuf) groupes=1001(adebuf)
$ id jmortreux
uid=1002(jmortreux) gid=1002(jmortreux) groupes=1002(jmortreux)
```

Les arguments et les options peuvent évidemment être combinés à souhait, par
exemple pour afficher l'UID d'un autre utilisateur.

```
$ id -u fteyssier
1004
```

Enfin, la commande `finger` permet d'afficher quelques renseignements sur les
utilisateurs du système comme le nom, le répertoire utilisateur et le *shell*
de connexion utilisé. Elle ne fait pas partie du système minimal, mais nous
pouvons l'installer facilement.

```
# yum install finger
```

Une fois installée, la commande `finger` affiche ces informations.

```
$ finger fteyssier
Login: fteyssier                        Name: Franck Teyssier
Directory: /home/fteyssier              Shell: /bin/bash
Last login ven. juin 16 10:49 (CEST) on pts/0
...
```





 
