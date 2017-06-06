La structure des répertoires sous Linux
=======================================

Document écrit par Nicolas Kovacs <info@microlinux.fr>


Où suis-je ?
------------

Parmi les questions que peuvent se poser les utilisateurs de Windows qui
viennent de passer à Linux, voici les plus fréquentes :

  * "Lorsque je viens de me connecter au système, où est-ce que je me
    retrouve ?"

  * "Où sont mes fichiers et mes documents ?"

  * "Où est mon lecteur `C:` ?"


Une structure en arborescence
-----------------------------

Comme la plupart des systèmes d'exploitation modernes, Linux enregistre tous
ses fichiers dans une structure organisée de façon hiérarchique, en
arborescence. 

Imaginez votre système Linux comme un de ces grands classeurs de
dossiers qu'on voit dans les bureaux. Ce genre de meuble se subdivise en
tiroirs, chaque tiroir pouvant contenir une série de classeurs qui renferment
des documents ou d'autres classeurs à leur tour. La métaphore est rudimentaire,
mais elle vous permettra de vous faire une première idée.

Une métaphore autrement parlante pourra venir compléter votre vision du système
de fichiers, celle des poupées russes. Vous connaissez certainement le principe
des poupées gigognes qui s'emboîtent les unes dans les autres. Dans un système
Linux comme dans d'autres systèmes, chaque répertoire peut ainsi contenir
d'autres répertoires, contenant eux-mêmes des sous-répertoires, jusqu'à ce
qu'on arrive au bout de la hiérarchie.


Visite guidée du système en dix minutes
---------------------------------------

Vous avez peut-être déjà vu ces cars de touristes japonais, ornés du sigle
publicitaire "*Visit Europe in six days*", "Visitez l'Europe en six jours".  Le
but du jeu de cette formule touristique consiste apparemment à parcourir une
moyenne de mille kilomètres par jour pour avoir tout juste le temps de prendre
en photo le sourire exténué des compatriotes devant Big Ben, la Tour Eiffel, le
Vésuve et la maison natale de Mozart.

C'est un peu dans le même esprit que nous allons entreprendre une première
visite sommaire du système, maintenant que nous savons nous déplacer dans les
répertoires. Un système Linux comme celui que nous venons d'installer est
composé de milliers de répertoires et de sous-répertoires, contenant des
dizaines, voire une centaine de milliers de fichiers. Si cette idée vous est
insupportable, songez à la découverte d'une grande ville. Mettez-vous dans la
peau d'un touriste qui vient passer une semaine à Marseille. La ville est
constituée de centaines de rues avec des milliers d'immeubles et des centaines
de milliers de gens. Est-ce que cela vous empêchera d'aller vous balader sur la
Canebière ou de boire un café au Vieux Port ? Non ? Bon.


Home sweet home
---------------

En partant de votre répertoire utilisateur, montez d'un cran (`cd ..`) dans le
répertoire `/home`. Vous y verrez le répertoire correspondant à l'utilisateur
créé lors de l'installation. Si nous avions créé les comptes des trois
utilisateurs `kikinovak`, `glagaffe` et `jktartempion` sur le même système,
chacun de ces trois utilisateurs disposerait ici de son propre répertoire
d'utilisateur. Kiki Novak rangerait sa documentation technique et ses articles
dans `/home/kikinovak`, Gaston Lagaffe organiserait ses blagues et vidéos
marrantes dans `/home/glagaffe` et Jean-Kevin pourrait stocker ses albums et
ses films piratés dans les sous-répertoires de `/home/jktartempion`.

Nous verrons la gestion des utilisateurs un peu plus loin.


Remonter à la racine : /
------------------------

Partant du répertoire `/home`, si vous montez encore d'un cran (`cd ..`), vous
vous retrouvez à la racine du système de fichiers, symbolisée par une barre
oblique `/`. Pour notre première visite, faites comme les touristes tout juste
sortis du car. Regardez tout, prenez des photos, mais ne touchez à rien. Nous
reviendrons bientôt pour revoir tout cela en détail.


Les répertoires /bin et /boot
-----------------------------

Historiquement, le répertoire `/bin` (pour *binaries* ou "binaires") contient
des commandes simples pour tous les utilisateurs, et plus précisément, toutes
les commandes dont le système a besoin pour démarrer correctement. Sur notre
système CentOS, `/bin` est un lien symbolique pointant vers `/usr/bin`. Nous
verrons plus loin pourquoi. 

`/boot` est l'endroit où un système Linux range tout ce qu'il lui faut pour
démarrer (*to boot*, "démarrer"). Le fichier `vmlinuz-3.10.0-514.el7.x86_64`,
c'est le noyau de votre machine. Vous pouvez en avoir un seul ou toute une
collection dans ce répertoire (ce que nous verrons plus loin). En revanche,
vous n'en utiliserez jamais qu'un seul à la fois. 

Le noyau (ou *kernel*), c'est la partie du système d'exploitation qui est la
plus près de votre matériel. C'est précisément ce fichier qui est chargé, dès
que GRUB (*GRand Unified Bootloader*, le chargeur de démarrage) passe la main
au système. GRUB est en quelque sorte un logiciel ayant pour seule tâche de
démarrer le noyau. Les fichiers utilisés par GRUB se trouvent respectivement
dans `/boot/grub` et `/boot/grub2`. Si cela ne vous évoque pas grand-chose pour
l'instant, ne vous tracassez pas. Et oui, c'est normal que vous n'ayez pas
accès à `/boot/grub2` en tant que simple utilisateur. Le chargeur de démarrage
fait partie des composants vitaux de votre système, et les utilisateurs
"communs mortels" n'ont rien à y faire. Pour l'instant, contentez-vous de
savoir que ces fichiers existent et qu'ils sont rangés par ici. 


Les répertoires /dev et /etc
----------------------------

Le répertoire `/dev` (comme *device*, qui signifie "périphérique") est peuplé
d'une multitude de fichiers qui symbolisent chacun un périphérique de votre
machine. Repérez, par exemple, `/dev/cdrom` qui représente votre lecteur CD ou
DVD (s'il existe), `/dev/input/mouse0` qui désigne votre souris, ou encore
`/dev/sda` qui symbolise votre premier disque dur. Et si jamais vous vous
demandez ce que peut bien être `/dev/null` : c'est le nirvana numérique. 

L'étymologie de `/etc` est controversée. C'est un de ces cas de figure assez
fréquents dans la vie quotidienne où l'acceptation erronée a pris le dessus
pour devenir monnaie courante (un peu comme ces jardins "privatifs" qui les
agents immobiliers cherchent à vous vendre dans l'espoir d'être "rénumérés").
La tradition veut que ETC signifie *Editable Text Configuration*, c'est-à-dire
"configuration éditable en mode texte". Voyons ce que cela veut signifie
concrètement. Dans le répertoire `/etc`, repérez le fichier `passwd` et
affichez son contenu. 

```
[kikinovak@centosbox ~]$ cd /etc
[kikinovak@centosbox etc]$ ls passwd
passwd
[kikinovak@centosbox etc]$ cat passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
...
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
postfix:x:89:89::/var/spool/postfix:/sbin/nologin
chrony:x:997:995::/var/lib/chrony:/sbin/nologin
kikinovak:x:1000:1000:Kiki Novak:/home/kikinovak:/bin/bash
```

La fin du fichier contient manifestement la configuration du ou des
utilisateurs "communs mortels" du système. Et avant que vous n'émettiez des
réserves, ne vous inquiétez pas : les mots de passe des utilisateurs sont
stockés dans un endroit inaccessible pour lesdits utilisateurs. Quoi qu'il en
soit, il n'y a là rien de bien spectaculaire, objecterez-vous en réprimant un
baîllement d'ennui. Eh bien, si !

La configuration d'un système Linux - qu'il s'agisse d'un serveur comme le
nôtre, d'un poste de travail ou d'une machine intégrée dans une ferme de calcul
de trois mille machines de la NASA - est stockée dans de simples fichiers texte
humainement lisibles. Il est donc possible de configurer le système en
modifiant ces fichiers avec un éditeur de texte comme seul outil. En
comparaison, un système Windows n'offre pas cette transparence. Sur ce dernier,
les opérations du système sont retranscrites sous forme de graffitis écrits
dnas une obscure base de registre, illisible pour un humain. 

Si vous êtes curieux, jetez un oeil distrait à quelques autres fichiers de ce
répertoire, sans vous laisser intimider par leurs noms quelque peu barbares :
`DIR_COLORS`, `hostname`, `hosts`, `group`, `fstab` ou `profile`. Ne vous
inquiétez pas si vous n'y comprenez rien du tout. Retenez simplement que ce
sont des fichiers au format texte simple.

Et pour l'histoire : "*etc[etera] ( = all the other stuff: doesn't go into bin,
dev, tmp, usr, var).*" Traduction : "etc[etera] ( = tous les autres trucs : ne
vont pas dans bin, dev, tmp, usr, var)." C'est un extrait de la description de
l'arborescence d'Unix fournie par Dennis Ritchie, le fondateur en personne. Lui
devrait savoir. 

Le répertoire /lib
------------------

Les bibliothèques partagées par les programmes de `/bin` et `/sbin` se trouvent
dans `/lib` (*libraries*, "bibliothèques"). 

Un programme n'est pas forcément un bloc monolithique. Il se sert souvent d'un
ensemble de fonctions qui se situent dans une bibliothèque partagée. Cette
dernière ne peut pas s'exécuter directement, mais contient du code que l'on
veut éviter de réécrire chaque fois qu'un programme doit exécuter une fonction
similaire (par exemple ouvrir une fenêtre, *Enregistrer sous* ou calculer un
cosinus). Sous Windows, ce sont tous les fichiers `.DLL` (*Dynamic Linked
Library* ou "bibliothèque de liens dynamiques" que vous trouverez dans le
répertoire `C:\WINDOWS\SYSTEM`. Sur votre système Linux, ce sont tous les
fichiers `.so` (comme *shared object* ou "objet partagé". 

Si vous vous rendez dans le répertoire `/lib/modules`, vous y trouverez un
répertoire `3.10.0-514.el7.x86_64`. Les lecteurs attentifs noteront un air de
parenté avec le nom du noyau `vmlinuz-3.10.0-514.el7.x86_64` que nous avons vu
un peu plus haut. Effectivement, tous les fichiers contenus dans ce répertoire
appartiennent au noyau. Ce sont là les "modules", l'équivalent de ce que les
utilisateurs de Windows ou Mac OS X connaissent sous la désignation de pilote
(*driver* ou "gestionnaire de périphérique") : des petits bouts de code que
l'on charge dans le noyau pour lui permettre de gérer tel ou tel périphérique.
Avec Linux, contrairement à Windows, on peut effectuer cette opération sur un
système en état de marche, sans que cela ne nécessite un redémarrage. De façon
analogue, on peut enlever un module, ce qui permet donc d'activer ou de
désactiver la prise en charge d'un certain matériel sur un système "à chaud". 

Pour vous faire une idée un petit peu moins vague, entrez dans le répertoire
`3.10.0-514.el7.x86_64`, puis continuez dans `kernel/drivers/net/ethernet`.
Dans la liste de répertoires qui s'affiche, vous reconnaîtrez peut-être
vaguement des noms de fabricants : `atheros`, `broadcom`, `intel`, `qlogic`,
`realtek`, etc. Jetez un oeil distrait dans quelques-uns de ces répertoires et
observez les différents fichiers `.ko` qu'ils contiennent. Chaque nom de
fichier correspond en effet à un certain type de matériel, plus précisément à
une série de cartes réseau (*net* signifie "réseau"). Ainsi, `8139cp.ko` et
`8139too.ko` dans le répertoire `realtek` correspondent à une carte réseau
équipée d'une puce (*chip*) Realtek 8139. De manière similaire, les fichiers
commençant par `al` et `atl` dans le répertoire `atheros` correspondent à des
cartes réseau Atheros, `e1000.ko` et `e1000e.ko` dans l'arborescence `intel`
gèrent les cartes réseau Intel, et ainsi de suite. Dans la plupart des cas, le
nom du module permet de deviner quel matériel lui correspond. Dans d'autres
cas, la relation n'est pas évidente, et il faut se renseigner. Nous verrons
plus loin où et comment faire. 


Les répertoires /mnt, /media et /run
------------------------------------

Les répertoires `/media` et `/mnt` constituent par convention les points de
montage de votre système. Le répertoire `/run` est un ajout récent à la
hiérarchie des répertoires sous Linux, dont un des rôles est de prendre la
relève de `/media`. C'est ici que se trouvent vos disques `C:`, `D:`, `E:`,
`F:`, etc. 

Dans un système Linux, lorsque vous insérez un périphérique amovible comme un
CD-Rom, un DVD, un disque dur externe ou une clé USB, il doit être "monté".
Cela signifie que le système de fichiers du périphérique doit être intégré à
l'arborescence du système. Les données sont ensuite accessibles en dessous du
répertoire qui constitue ce qu'on appelle le "point de montage". Avant
d'enlever le périphérique, celui-ci doit être "démonté", c'est-à-dire que l'on
indique au système de fichiers que les données du périphérique amovible ne
doivent plus être englobées. 

Sur un serveur Linux dépourvu d'environnement graphique, les opérations de
montage et de démontage s'effectuent de manière traditionnelle, à la main. Pas
avec un tournevis et une clé de douze, non, mais en tapant une série de
commandes. Les distributions "poste de travail" modernes gèrent les
périphériques amovibles de manière complètement transparente, c'est-à-dire que
le montage s'effectue automatiquement. 

Le montage et le démontage constituent un des concepts qui peuvent paraître
étrangers à un habitué des systèmes Windows. Pour l'instant, retenez simplement
que `/media` et `/run` vous permettent d'accéder aux données des périphériques
amovibles que le système gère automatiquement, par exemple sur un poste de
travail. Quant à `/mnt`, c'est le point de montage "historique" que l'on
conserve pour les systèmes de fichiers montés manuellement, comme c'est le cas
sur les serveurs. Nous éluciderons tout cela par la pratique, le moment venu.


Les répertoires /proc et /sys
-----------------------------

Les répertoires `/proc` et `/sys` contiennent un système de fichiers virtuel
qui documente à la volée le noyau et les différents processus du système. Avant
que vous ne partiez en courant, retenez juste que certains fichier contenus
dans ces répertoires nous fourniront des informations précieuses sur le système
: le modèle et la fréquence du processeur (`/proc/cpuinfo` que nous avons eu
l'occasion de voir), la mémoire vive et la quantité de mémoire vive utilisée
(`/proc/meminfo`), la synchronisation d'une grappe de disques (`/proc/mdstat`)
et beaucoup d'autres choses encore. Quant au "système de fichiers virtuel", on
peut le considérer comme un système de fichiers volatile, dont il ne reste pas
la moindre trace dès que vous éteignez la machine. 


Les répertoires /root et /sbin
------------------------------

`/root`, c'est le répertoire d'utilisateur de... l'utilisateur `root` ! Il
n'est donc pas étonnant que vous n'y ayez pas accès en tant qu'utilisateur
normal. "Et pourquoi pas `/home/root` ?" penserez-vous peut-être.
L'administrateur serait-il réticent de se retrouver ainsi à pied d'égalité avec
les basses castes des utilisateurs communs ? Oui, en quelque sorte, mais pour
une simple raison de sécurité.

Dans les installations de grande envergure, il arrive assez souvent qu'un
système Linux soit réparti sur plusieurs partitions d'un disque, voire sur
plusieurs disques et, dans certains cas, le système sur lequel vous travaillez
peut être "distribué" sur plusieurs machines, qui ne sont d'ailleurs pas
forcément dans la même pièce, ni dans le même bâtiment. Au démarrage, le
système se charge d'assembler les pièces pour vous présenter un tout cohérent.
Imaginez maintenant qu'il y ait un problème avec le disque ou la machine
contenant le répertoire `/home`. Si le répertoire d'utilisateur de `root` était
en dessous de `/home`, il serait inaccessible. `root` ne pourrait plus
s'identifier et, par conséquent, ne pourrait plus rien faire sur la machine. 

Une remarque en passant. La langue anglaise désigne la racine `/` du système de
fichiers par *root directory*. Gare à la confusion issue de l'homophonie avec
*/root directory* !

Le répertoire `/sbin` (*system binaries*, autrement dit "binaires système")
renferme une série d'exécutables pour l'administrateur. Ces outils servent à
partitionner et formater des disques, configurer des interfaces réseau et bien
d'autres choses encore. Une partie de ces commandes peut être invoquée par les
utilisateurs du "commun des mortels". À titre d'exemple, la commande suivante
vous permet d'afficher la configuration réseau de votre machine.

```
[kikinovak@centosbox ~]$ /sbin/ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether 52:54:00:ab:cd:ef brd ff:ff:ff:ff:ff:ff
    inet 192.168.2.20/24 brd 192.168.2.255 scope global dynamic eth0
       valid_lft 85369sec preferred_lft 85369sec
    inet6 fe80::a9aa:7f83:429:ff1e/64 scope link 
       valid_lft forever preferred_lft forever
```

Cependant, pour la plupart, ces utilitaires représentent l'équivalent numérique
d'une tronçonneuse. Dans les mains d'un expert, cela permet d'abattre de la
besogne en un tour de main. Mettez un utilisateur lambda aux commandes et
attendez-vous à un massacre. 

Tout comme `/bin`, `/sbin` est un lien symbolique qui pointe vers
l'arborescence `/usr`. Nous y venons, justement. 


Le répertoire /usr
------------------

L'arborescence sous `/usr` (*Unix System Resources* ou *Unix Specific
Resources*, aucun lien avec *us(e)r*), renferme précisément tout ce qui n'est
*pas* nécessaire au fonctionnement minimal du système. Vous serez d'ailleurs
peut-être surpris d'apprendre que cela représente la part du lion. Sur notre
installation serveur minimale, `/usr` contient près de 90 % de la totalité du
système. Vous constaterez que certains répertoires ou liens symboliques
rencontrés à la racine du système sont également présents ici : `/usr/bin`,
`/usr/lib` ou encore `/usr/sbin`. 

Même sur notre installation minimale, l'arborescence `/usr` compte déjà plus de
20.000 fichiers. Avec un système d'une telle complexité, il est important que
chaque chose ait une place bien définie pour que l'on s'y retrouve.

  * `/usr/sbin` comporte d'autres commandes pour l'administrateur.

  * `/usr/bin` renferme la majorité des exécutables pour les utilisateurs.

  * `/usr/lib` et `/usr/lib64` contiennent les bibliothèques partagées de ces
    derniers.

Le moment est venu pour dire deux mots des liens symboliques (ou raccourcis)
`/bin`, `/lib`, `/lib64` et `/sbin` à la racine du système. Traditionnellement,
les systèmes Linux opéraient la distinction et rangeaient dans ces répertoires
le nombre relativement limité d'applications et de bibliothèques qui étaient
nécessaires au démarrage ou au dépannage du système. Tout ce qui n'était pas
vital *stricto sensu* pour le démarrage avait sa place dans `/usr`. Or, depuis
quelques années, on observe une tendance croissante à faire fi de cette
distinction et à fusionner `/bin` et `/usr/bin`, `/lib` et `/usr/lib`, et ainsi
de suite. D'où la série de liens symboliques.

Notez que sous Linux, `/usr/bin` représente à peu de choses près l'équivalent
du répertoire `C:\Program Files` de Windows. 

Étant donné que la question revient souvent, je me permets d'anticiper un peu
pour vous montrer comment je m'y suis pris pour compter le nombre de fichiers
en dessous de `/usr`, ou pour afficher l'espace disque occupé par les
arborescences respectives. Invoquez les deux commandes suivantes sans vous
préoccuper des détails pour l'instant.

```
[kikinovak@centosbox /]$ find /usr -type f 2> /dev/null | wc -l
20058
[kikinovak@centosbox /]$ du -sh /* 2> /dev/null
0       /bin
81M     /boot
0       /dev
17M     /etc
36K     /home
0       /lib
0       /lib64
0       /media
0       /mnt
0       /opt
0       /proc
0       /root
8,4M    /run
0       /sbin
0       /srv
0       /sys
0       /tmp
836M    /usr
41M     /var
```

