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

