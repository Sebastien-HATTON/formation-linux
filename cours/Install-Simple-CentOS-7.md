Installation simple de CentOS 7
===============================

Document écrit par Nicolas Kovacs (info@microlinux.fr)

Cette page décrit de manière succincte l’installation et la configuration d'un
système CentOS 7. Nous optons ici pour une installation minimale dépourvue
d'interface graphique et réduite au minimum syndical de services. Toutes les
applications - comme par exemple le serveur web Apache, le serveur de fichiers
Samba ou une panoplie d'outils en ligne de commande - devront être installées
par la suite selon les besoins individuels. 

L’installateur de CentOS 7 requiert au moins 1 Go de mémoire vive ainsi
qu'un processeur capable de faire tourner un OS 64-bits. Pour travailler plus
sereinment, on préférera une machine dotée d'au moins 2 Go de RAM. 

Notons que c'est surtout l'installateur qui se montre relativement gourmand en
ressources. Une fois que CentOS est installé sur le disque, le système consomme
moins de 100 Mo de RAM au repos. 


Support d'installation
----------------------

On choisira le CD minimal, mais rien n’empêche d’utiliser le DVD.

* `CentOS-7-x86_64-Minimal-1611.iso`

* `CentOS-7-x86_64-DVD-1611.iso`

Graver le CD ou le DVD à partir de l’ISO téléchargé.

Sur les machines dépourvues de lecteur optique comme par exemple les serveurs
HP de la gamme Proliant Microserver, il faudra confectionner une clé USB
d’installation. L’image ISO est hybride et peut s’écrire directement sur une
clé USB.

```
# dd if=CentOS-7-x86_64-Minimal-1611.iso of=/dev/sdX
```

Démarrage
---------

Débrancher clés USB, disques externes et autres périphériques amovibles.
Autrement l’installateur les proposera au formatage. Évidemment, cela ne vaut
pas pour la clé USB d'installation. 

L'option par défaut *Test this media & install CentOS Linux 7* permet de tester
l'intégrité du support d'installation, ce qui n'est pas une mauvaise idée. Si
l'on souhaite passer cette étape, on optera directement pour *Install CentOS
Linux 7*

Enfin, l'option *Run a memory test* accessible via l'entrée de menu
*Troubleshooting* permet d'effectuer un test minutieux de la mémoire vive du
serveur, ce qui est recommandé pour une nouvelle installation. 

Pour l'instant, nous laissons de côté les options *Rescue a CentOS Linux
system* et *Boot from local drive*.


Langue et clavier
-----------------

Dans l’écran de bienvenue, sélectionner la langue (*Français*) et la
localisation (*Français – France*). La disposition du clavier sera définie par
le biais de l’écran principal de l’installateur.


Nom d'hôte et réseau
--------------------

Le réseau n’est pas activé par défaut, il faut donc songer à l’activer
explicitement en cliquant sur le bouton en forme d'interrupteur, ce qui fait
passer sa valeur de **0** à **1**.

Notons au passage que les noms des interfaces réseau ont changé avec cette
nouvelle version.  Désormais, on n’a plus affaire à `eth0`, `eth1`, `eth2`,
`wlan0`, etc. Le nouveau schéma de nommage est moins arbitraire et offre
davantage de consistance en se basant sur l’emplacement physique de la carte
dans la machine.

* `enp2s0`

* `enp3s0`

* `enp3s1`

* etc.

Pour le nom d'hôte de votre machine, choisissez-en un à votre convenance et
notez-le en minuscules, en remplacement de `localhost.localdomain` par défaut.
Voici quelques exemples pour vous donner une idée.

* `centosbox`

* `serveur-linux`

* `alphamule`

* `grossebertha`

* etc.


Date et heure
-------------

Vérifier si le fuseau horaire (*Europe/Paris*) est correctement configuré.
Éventuellement, activer *Heure du réseau* et vérifier les serveurs NTP.


Disposition du clavier
----------------------

Si l'on souhaite remplacer le clavier AZERTY par un autre clavier, il faut
mettre *Français (variante)* en surbrillance, cliquer sur le bouton **-** pour
le supprimer et sélectionner le clavier souhaité dans la liste des agencements. 


Désactivation de Kdump
----------------------

Kdump est un mécanisme de capture lors du plantage d’un noyau. Il peut être
désactivé.


Partitionnement
---------------

Pour une première installation, on choisira le partitionnement automatique. 

1. Cliquez sur *Destination de l'installation*.

2. Vérifiez si le disque dur est bien sélectionné.

3. Gardez l'option *Configurer automatiquement le partitionnement*.

4. Éventuellement, cochez *Je voudrais libérer de l'espace* pour faire de la
place.

5. Cliquez sur *Terminé*.

6. Dans l'écran subséquent, supprimez toutes les partitions existantes le cas
échéant. Cliquez sur *Tout supprimer*, puis sur *Récupérer de l'espace*, ce qui
vous fait revenir à l'écran principal. L'installateur se chargera de calculer
automatiquement le schéma de partitionnement et choisira les systèmes de
fichiers adaptés. 


Sélection de logiciels
----------------------

Dans l’écran de sélection des logiciels du DVD, on optera pour le groupe de
paquets *Installation minimale* proposé par défaut. Le CD minimal ne laisse pas
le choix de toute façon.

À partir de là, on peut *Démarrer l'installation*. 


Paramètres utilisateur
----------------------

L'écran suivant vous somme de choisir un mot de passe administrateur et vous
propose de créer un premier utilisateur "commun mortel". 

Un système Linux fait, en gros, la distinction entre deux types d'utilisateurs.

1. Les utilisateurs du "commun des mortels" ont accès à certaines zones du
système, si l'on peut dire. À condition que leur compte soit configuré
correctement - nous verrons cela plus loin - ils ont suffisamment de droits
pour travailler correctement mais, en aucun cas, une mauvaise manipulation de
leur part ne pourra porter atteinte à l'intégrité du système. On peut comparer
ce cas de figure à une entreprise où chaque employé possède son propre casier,
son propre bureau avec ses propres tiroirs qui ferment à clé. Il bénéficie de
l'infrastructure de l'entreprise et partage une partie de son travail s'il le
souhaite, mais personne - à l'exception de l'administrateur `root` - ne pourra
fouiner dans ses affaires personnelles.

2. L'administrateur `root`, quand à lui, possède tous les droits sur la
machine. C'est le vigile avec l'énorme trousseau de clés qui donne accès aux
moindres recoins du bâtiment. Comme il a tous les droits, on l'appelle aussi
parfois *super-utilisateur*. 


Bien choisir son mot de passe
-----------------------------

Linux a une préférence marquée pour les mots de passe compliqués, le genre de
chaîne de caractères que vous obtenez lorsque le chat marche sur le clavier.
`123456`, `654321` ou le nom dudit chat ne sont **pas** de bons mots de passe,
à moins que vous n'ayez l'habitude d'appeler votre animal domestique `GnLpF3th`
ou `Wgh8sTr5FgH`. Vous verrez d'ailleurs que l'installateur protestera si le
mot de passe que vous choisissez lui paraît trop simple. Dans ce cas, vous
devrez soit en choisir un autre plus compliqué, soit confirmer par deux fois. 


Créer un utilisateur
--------------------

L'écran de création de l'utilisateur initial vous pose une série de questions.
Rien ne vous oblige de respecter l'ordre Nom-Prénom dans le premier champ, et
vous pouvez très bien indiquer *Nicolas Kovacs*, *Gaston Lagaffe* ou
*Jean-Kevin Tartempion*. 

En fonction de votre saisie initiale, l'installateur vous fera une suggestion
pour le nom d'utilisateur, mais vous n'êtes pas obligé de la suivre. Il existe
une série de règles et de conventions sur les systèmes Linux en ce qui concerne
les noms d'utilisateur.

* Il est **interdit** d'utiliser les caractères spéciaux et les espaces.

* Préférez les lettres minuscules. C'est une convention, et rien ne vous
  empêche d'utiliser les majuscules.

* Un nom d'utilisateur est généralement composé de la première lettre
  (initiale) du prénom, suivie du nom de famille. Là aussi, c'est une
  recommandation, et vous n'êtes pas obligé de vous y tenir. 

Si nous respectons ces règles, l'utilisateur Gaston Lagaffe utilisera donc le
nom d'utilisateur `glagaffe`, Jacques Martin s'identifiera sur le système en
tant que `jmartin`, et le login de Jean-Kevin Tartempion ressemblera à quelque
chose comme `jktartempion`. 

Rien ne nous oblige pourtant à être aussi strict dans la définition du nom
d'utilisateur. Kiki Novak pourra préférer `kikinovak` à `knovak`, Jean-Kevin
Tartempion favorisera `warlordz` ou `nemesis`, Gaston Lagaffe utilisera un
simple `gaston`, et rien n'empêchera Jean-Philippe Smet de s'identifier en tant
que `johnny`, plus incisif que `jpsmet`. 

Cochez éventuellement la case *Faire de cet utilisateur un administrateur*,
mais c'est facultatif. Nous verrons plus loin ce que cela signifie. 

Enfin, choisissez un mot de passe pour cet utilisateur, en respectant les mêmes
règles que celles énoncées un peu plus haut pour le mot de passe `root`.

Il ne vous reste plus qu'à *Terminer la configuration* et à *Redémarrer* la
machine. N'oubliez pas de retirer le support d'installation.


Redémarrage initial
-------------------

L'ordinateur redémarre et vous affiche tout d'abord l'écran du chargeur de
démarrage GRUB (*GRand Unified Bootloader*). Le réglagle par défaut prévoit un
temps d'attente de cinq secondes avant le lancement automatique du système.
Appuyez sur *Entrée* pour écourter ce délai et démarrer directement.

L'initialisation du système minimal se fait assez rapidement. Au terme du
démarrage, vous vous retrouvez confronté à un message qui ressemble à peu de
choses près à ceci :

```
CentOS Linux 7 (Core)
Kernel 3.10.0-514.el7.x86_64 on an x86_64

centosbox login: _
```

À l'heure actuelle, nous disposons de deux comptes sur notre machine.

1. l'administrateur `root`

2. l'utilisateur commun mortel (`kikinovak` par exemple)

Connectez-vous en tant qu'utilisateur normal. Notez que le mot de passe ne
s'affiche pas sur l'écran.

```
centosbox login: kikinovak
Password: ********
[kikinovak@centosbox ~]$ _
```

Si tout s'est bien passé, vous vous retrouvez face à l'invite de commande.





