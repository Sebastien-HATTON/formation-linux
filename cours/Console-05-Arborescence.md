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

Le répertoire `/bin` (pour *binaries* ou "binaires") contient des commandes
simples pour tous les utilisateurs, et plus précisément, toutes les commandes
dont le système a besoin pour démarrer correctement.

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











