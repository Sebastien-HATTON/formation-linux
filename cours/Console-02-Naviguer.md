Naviguer : ls, pwd et cd
========================

Document écrit par Nicolas Kovacs <info@microlinux.fr>


Afficher le contenu d'un répertoire avec ls
-------------------------------------------

La commande `ls` (comme *list*) affiche la liste des fichiers d'un répertoire.
Invoquée sans arguments, elle montre le contenu du répertoire courant.

```
[root@centosbox ~]# ls
anaconda-ks.cfg
```

Comment lire ce résultat ? La commande `ls` nous a retourné un élément situé
dans notre répertoire d'utilisateur. Notez que je me suis connecté en tant que
`root` pour avoir quelque chose à me mettre sous la dent, étant donné que le
répertoire d'utilisateur de `kikinovak` est encore vide. Pour le reste des
opérations, nous allons travailler en tant qu'utilisateur "commun mortel". Je
vérifie, mon répertoire d'utilisateur est encore vide, effectivement.

```
[kikinovak@centosbox ~]$ ls
```

Le répertoire courant est celui dans lequel vous vous trouvez au moment où vous
saisissez la commande. Pour afficher le contenu de la racine du système de
fichiers, saisissez ceci :

```
[kikinovak@centosbox ~]$ ls /
```

Résultat :

```
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var                     
boot  etc  lib   media  opt  root  sbin  sys  usr     
```

Et pour voir ce qu'il y a dans `/usr`, il suffit d'invoquer la commande
suivante :

```
[kikinovak@centosbox ~]$ ls /usr
bin  games    lib    libexec  sbin   src                                   
etc  include  lib64  local    share  tmp
```

Les habitués de Windows et de DOS l'auront deviné : `ls`sous Linux équivaut à
la commande `dir`.


Décrypter les résultats de votre ordinateur
-------------------------------------------

Les différentes couleurs de l'affichage nous suggèrent qu'il ne s'agit
peut-être pas d'éléments du même type. Essayez :

```
[kikinovak@centosbox ~]$ ls /etc
```

Le résultat de cette commande dépassera probablement la taille d'un écran. Pour
revenir en arrière, maintenez la touche **Maj** enfoncée et utilisez les
touches **PageHaut** et **PageBas** pour faire défiler l'affichage en arrière
et en avant. 

Certains éléments apparaissent en bleu, d'autres en turquoise, d'autres en noir
et blanc, et il y a même un peu de rouge. Pour en avoir le coeur net, il va
falloir utiliser `ls` avec l'option `-F`, qui donne des détails :

```
[kikinovak@centosbox ~]$ ls -F /etc
adjtime                  hosts                     rc0.d@
aliases                  hosts.allow               rc1.d@
aliases.db               hosts.deny                rc2.d@
alternatives/            init.d@                   rc3.d@
anacrontab               inittab                   rc4.d@
asound.conf              inputrc                   rc5.d@
audisp/                  iproute2/                 rc6.d@
...
```

Réinvoquez `ls -F` pour afficher le contenu de `/etc/ppp` :

```
[kikinovak@centosbox ~]$ ls -F /etc/ppp
chap-secrets   ip-down*          ip-up.ipv6to4*  options
eaptls-client  ip-down.ipv6to4*  ipv6-down*      pap-secrets
eaptls-server  ip-up*            ipv6-up*        peers/
```

Vous constatez que certains éléments sont suivis d'une barre oblique `/`,
d'autres d'une arobase `@` ou d'une astérisque `*`; le reste des éléments ne
contient aucun suffixe. 

  * La barre oblique `/` (couleur par défaut : bleu) désigne un répertoire.

  * L'absence de suffixe (couleur par défaut : noir, blanc ou gris, selon votre
    terminal) indique qu'il s'agit d'un fichier régulier non exécutable.

  * L'arobase `@` (couleur par défaut : turquoise) nous montre qu'il s'agit
    d'un lien symbolique, ce qui constitue l'équivalent d'un raccourci sous
    Windows. Nous verrons les liens symboliques un peu plus loin.

  * L'astérisque `*` (couleur par défaut : vert) nous indique qu'il s'agit d'un
    fichier régulier exécutable.

Il nous en manque encore quelques-uns, mais nous nous contenterons des éléments
que nous avons pour l'instant. 


Mais encore ?
-------------

Ces informations peuvent nous paraître un peu maigres. Nous pouvons en afficher
davantage en utilisant l'option `-l` (comme *long*) :

```
[kikinovak@centosbox ~]$ ls -l /etc/sysconfig
total 80
-rw-r--r--. 1 root root  429 23 mai   20:30 authconfig
drwxr-xr-x. 2 root root   43 23 mai   20:20 cbq
drwxr-xr-x. 2 root root    6  6 nov.   2016 console
-rw-r--r--. 1 root root  150 22 nov.   2016 cpupower
-rw-------. 1 root root  110 31 mars   2016 crond
-rw-------. 1 root root 1390  5 nov.   2016 ebtables-config
-rw-r--r--. 1 root root   73 11 nov.   2016 firewalld
...
```

L'utilisateur non averti trouvera cet affichage quelque peu énigmatique. En
fait, il est facile à lire une fois que l'on sait à quoi correspond chaque
terme. 

Tout à fait à gauche, vous avez une série de dix caractères. Le tout premier
vous indique s'il s'agit d'un fichier (tiret `-`) ou d'un répertoire (`d` comme
*directory*). Ensuite, la série de neuf caractères (en fait, trois fois trois)
indique les droits d'accès au fichier ou au répertoire. Nous traiterons la
question des droits d'accès un peu plus loin. Les caractères `r`, `w`, `x` et
`-` décrivent ce que l'on a le droit de faire avec le fichier ou le répertoire :

  * lire : `r` comme *read* ;

  * écrire (modifier) : `w` comme *write* ;

  * exécuter : `x` pour *e[x]ecute* ;

  * rien du tout : `-`.

Je disais : ce que l'*on* a le droit de faire. Ce *on* est en fait assez bien
spécifié : les trois premiers caractères de la série concernent le propriétaire
du fichier, les trois suivants le groupe et les trois derniers le reste du
monde. Nous y reviendrons bientôt en détail.

Le chiffre qui suit (ici : la série de `1` et de `2` dans la deuxième colonne)
n'est pas d'une grande importance pour nous. Précisons tout de même qu'il
indique le nombre de liens pointant vers le fichier ou le répertoire.

Les deux indications immédiatement après nous montrent le propriétaire du
fichier, ainsi que le groupe auquel il appartient. Dans l'exemple, fichiers et
répertoires appartiennent à l'utilisateur `root` et au groupe `root`. 


Humain, pas trop humain ?
-------------------------

La prochaine indication correspond à la taille du fichier. Ici, l'astuce est
d'invoquer `ls` avec l'option supplémentaire `-h` ou `--human-readable`.
Essayez :

```
[kikinovak@centosbox ~]$ ls -lh /etc/sysconfig
total 80K
-rw-r--r--. 1 root root  429 23 mai   20:30 authconfig
drwxr-xr-x. 2 root root   43 23 mai   20:20 cbq
drwxr-xr-x. 2 root root    6  6 nov.   2016 console
-rw-r--r--. 1 root root  150 22 nov.   2016 cpupower
-rw-------. 1 root root  110 31 mars   2016 crond
-rw-------. 1 root root 1,4K  5 nov.   2016 ebtables-config
-rw-r--r--. 1 root root   73 11 nov.   2016 firewalld
...
```

La taille des fichiers est tout de suite beaucoup plus lisible, car le système
l'indique en kilo-octets (`K`), mégaoctets (`M`) ou gigaoctets (`G`). 

En passant, nous faisons également deux autres constats. D'une part, les
options des commandes peuvent se combiner. Nous aurions donc très bien pu
invoquer la commande comme ceci :

```
[kikinovak@centosbox ~]$ ls -l -h /etc/sysconfig
```

D'autre part, bon nombre d'options de commande ont une version courte et une
version longue. L'option `-h` (qui signifie "lisible par un humain", comme si
les informaticiens ne faisaient pas vraiment partie de l'espèce) peut donc très
bien s'écrire comme dans l'exemple qui suit. Je vous conseille cette option
uniquement si vous avez un penchant prononcé pour la dactylographie :

```
[kikinovak@centosbox ~]$ ls -l --human-readable /etc/sysconfig
```

Quant aux deux dernières colonnes, elles nous indiquent respectivement la date
de la création ou de la dernière modification et, pour finir, le nom du fichier
ou du répertoire. 


Splendeur et misère des fichiers cachés
---------------------------------------

Une autre option fréquemment utilisée est `-a` (ou `--all` : tout).
Appliquez-la sur votre répertoire utilisateur, et vous serez probablement
surpris. 

```
[kikinovak@centosbox ~]$ ls -a
.  ..  .bash_history  .bash_logout  .bash_profile  .bashrc  .lesshst
```

Cette option sert à afficher les fichiers et répertoires cachés. Dans un
système Linux, les fichiers et répertoires dont le nom commence par un point ne
s'affichent pas lorsque `ls` est invoqué normalement. Vous ne les verrez donc
qu'en utilisant l'option `-a`.


Cachez cette configuration que je ne saurais voir
-------------------------------------------------

À quoi peuvent bien servir ces fichiers, et quel intérêt de les dissimuler ?
Les fichiers cachés ou *dotfiles* (de l'anglais *dot* : point) contiennent la
configuration personnalisée de vos applications. Concrètement, les fichiers
`~/.bash_profile`, `~/.bashrc` et `~/.bash_logout` contiennent la configuration
de votre shell Bash (qui est une application). Quant au fichier
`~/.bash_history`, il renferme l'historique des commandes précédemment
invoquées. 

À la différence des fichiers situés dans `/etc`, qui définissent une
configuration globale, c'est-à-dire valable pour tous les utilisateurs, les
fichiers et répertoires cachés que nous rencontrons ici ne sont valables que
pour vous seul. Nous aborderons le rôle du répertoire `/etc` un peu plus loin.

Vous vous demandez certainement ce que signifie le *tilde* `~` que j'ai
utilisé à plusieurs reprises. Sur les systèmes Linux (tout comme dans Unix), ce
symbole désigne votre répertoire utilisateur. Le fichier `~/.bashrc` de
l'utilisateur `kikinovak` sera donc `/home/kikinovak/.bashrc` dans sa notation
explicite, tandis que le fichier `~/.bashrc` de `glagaffe` correspondra à
`/home/glagaffe/.bashrc`. Étant donné que chaque utilisateur est libre de
configurer le shell Bash à sa guise (ce que nous verrons également plus loin),
tout le monde aura donc son propre fichier `.bashrc`. 

Notons que certains d'entre vous auront probablement déjà remarqué le tilde `~`
dans l'invite de commande :

```
[kikinovak@centosbox ~]$
```

Comprendre l'invite de commande
-------------------------------

Jetons un oeil sur l'invite de commande dans sa configuration par défaut. Elle
est très simple à décrypter.

  * La première indication, c'est le nom de l'utilisateur (ex : `kikinovak`).

  * Il est séparé du nom de la machine (ex : `centosbox`) par une arobase `@`.

  * La troisième indication, c'est le répertoire courant (ex : `~` à savoir
    `/home/kikinovak`, puisque c'est l'utilisateur `kikinovak`)

  * Et enfin, le `$` signifie par convention qu'il s'agit d'un utilisateur du
    "commun des mortels". S'il s'agissait de l'utilisateur `root`, nous
    verrions ici un dièse `#` à la place du `$`.

Et ne partez pas en courant si je vous dis que l'aspect même de l'invite peut
être paramétré à souhait. 


Afficher les informations détaillées d'un répertoire
----------------------------------------------------

Il nous reste à voir une dernière option importante pour `ls`. Admettons que
vous souhaitiez afficher les informations détaillées pour le répertoire `/etc`
: les droits d'accès, le propriétaire, le groupe, etc. Vous invoquez donc
hardiment `ls` suivi de l'option `-l` et de l'argument `/etc` ; et vous
voyez... les informations détaillées de tout le contenu du répertoire, mais pas
du répertoire lui-même. 

Comment faire ? Tout simplement en invoquant l'option supplémentaire `-d`
(comme *directory*, c'est-à-dire "répertoire"), qui affiche les répertoires
avec la même présentation que les fichiers, sans lister leur contenu :

```
[kikinovak@centosbox ~]$ ls -ld /etc
drwxr-xr-x. 76 root root 8192 25 mai   07:09 /etc
```


pwd : "Vous êtes ici !"
-----------------------

La commande `pwd` (*print working directory*) s'acquitte d'une seule tâche.
Elle vous affiche (*print*) le nom du répertoire courant (*working directory*),
c'est-à-dire le répertoire dans lequel vous vous situez actuellement.

```
[kikinovak@centosbox ~]$ pwd
/home/kikinovak
```

Lorsque vous vous promenez dans une grande ville, il vous arrive de vous
perdre. Avec un peu de chance, vous tombez sur un de ces grands plans de la
ville, avec une flèche et un petit rond bien visibles, qui vous indique : "VOUS
ÊTES ICI". C'est exactement ce que fait `pwd`. Et maintenant que nous savons
nous repérer, apprenons à nous déplacer. 


On bouge avec cd !
------------------

La commande `cd` (*change directory*) est utilisée pour changer de répertoire
courant. Il suffit de la faire suivre du chemin du répertoire dans lequel on
veut se placer. Dans l'exemple ci-après, l'invocation de la commande `pwd`
après `cd` montre que nous sommes bien dans le répertoire demandé.

```
[kikinovak@centosbox ~]$ cd /
[kikinovak@centosbox /]$ pwd
/
[kikinovak@centosbox /]$ cd bin
[kikinovak@centosbox bin]$ pwd
/bin
[kikinovak@centosbox bin]$ cd /etc
[kikinovak@centosbox etc]$ pwd
/etc
[kikinovak@centosbox etc]$ cd /usr/bin
[kikinovak@centosbox bin]$ pwd
/usr/bin
```

Chemin relatif ou absolu ?
--------------------------

Lorsque je me trouve dans le répertoire racine `/` et que je souhaite me
déplacer vers le répertoire `/bin`, je peux écrire `cd bin`. Cela correspond au
chemin *relatif*, c'est-à-dire le chemin indiqué à partir du répertoire dans
lequel je me situe, soit `/`. Quant à `cd /bin`, c'est le chemin *absolu*,
autrement dit l'emplacement à partir du répertoire racine.

En revanche, lorsque je me trouve dans le répertoire `/etc` et que je veux me
rendre dans `/bin`, je suis obligé - pour l'instant - d'utiliser un chemin
absolu. Pour saisir la distinction, je vous donne un exemple de ce qu'il ne
faut *pas* faire :

```
[kikinovak@centosbox bin]$ cd /etc
[kikinovak@centosbox etc]$ pwd
/etc
[kikinovak@centosbox etc]$ cd bin
-bash: cd: bin: Aucun fichier ou dossier de ce type
```

Ces deux exemples de la vie courante vous permettront peut-être de saisir la
nuance :

  * "Remontez la rue devant vous, tournez à gauche, continuez deux cents
    mètres, puis tournez à droite et encore à droite" (chemin relatif) ;

  * "Partez du Vieux Port, remontez la Canebière, puis prenez le boulevard
    Longchamp et arrêtez-vous au Palais Longchamp" (chemin absolu).

Dans l'exemple précédent, nous nous situons dans le répertoire `/etc`. Si nous
écrivons `cd bin` sans la barre oblique `/` qui précède, l'interpréteur de
commandes cherchera un répertoire inexistant `/etc/bin` et affichera une
erreur. 


À court d'arguments
-------------------

Pour revenir dans votre répertoire d'utilisateur, il suffit d'invoquer `cd`
tout court, sans arguments :

```
[kikinovak@centosbox etc]$ cd /etc
[kikinovak@centosbox etc]$ pwd
/etc
[kikinovak@centosbox etc]$ cd sysconfig
[kikinovak@centosbox sysconfig]$ pwd
/etc/sysconfig
[kikinovak@centosbox sysconfig]$ cd
[kikinovak@centosbox ~]$ pwd
/home/kikinovak
```


"Ici" et "à l'étage"
--------------------

Voyons maintenant deux répertoires un peu particuliers. Affichez la totalité du
contenu de votre répertoire d'utilisateur :

```
[kikinovak@centosbox ~]$ ls -aF
./  ../  .bash_history  .bash_logout  .bash_profile  .bashrc  .lesshst
```

Vous remarquez qu'en début de liste, vous avez un répertoire nommé `.` et un
autre nommé `..`. Affichez maintenant le contenu d'un autre répertoire, avec la
même option `-a` combinée avec l'option `-F` :

```
[kikinovak@centosbox ~]$ ls -aF /usr
./   bin/  games/    lib/    libexec/  sbin/   src/
../  etc/  include/  lib64/  local/    share/  tmp@
```

Si vous répétez l'opération sur d'autres répertoires au hasard, vous
constaterez que chaque liste débute invariablement par ces mêmes répertoires
`.` et `..` :

  * `.` est le répertoire courant.

  * `..` est le répertoire parent.

Là encore, la mise en pratique vous aidera à saisir le concept. Essayez ceci :

```
[kikinovak@centosbox ~]$ cd /etc/sysconfig/network-scripts
[kikinovak@centosbox network-scripts]$ pwd
/etc/sysconfig/network-scripts
[kikinovak@centosbox network-scripts]$ cd ..
[kikinovak@centosbox sysconfig]$ pwd
/etc/sysconfig
[kikinovak@centosbox sysconfig]$ cd ..
[kikinovak@centosbox etc]$ pwd
/etc
[kikinovak@centosbox etc]$ cd ..
[kikinovak@centosbox /]$ pwd
/
```

Chaque appel à `cd ..` nous fait ainsi remonter d'un cran dans l'arborescence,
jusqu'à ce que nous nous retrouvions à la racine. 

Quant au point `.`, il faut se le représenter comme le fameux "VOUS ÊTES ICI"
sur le plan de la ville. Admettons que je me situe dans le répertoire `/etc` et
que je veuille me rendre dans le sous-répertoire `sysconfig` ; je pourrais
utiliser indépendamment ces deux notations, qui reviendraient au même :

```
[kikinovak@centosbox /]$ cd /etc
[kikinovak@centosbox etc]$ cd sysconfig
[kikinovak@centosbox sysconfig]$ pwd
/etc/sysconfig
```

Ou alors :

```
[kikinovak@centosbox ~]$ cd /etc
[kikinovak@centosbox etc]$ cd ./sysconfig
[kikinovak@centosbox sysconfig]$ pwd
/etc/sysconfig
```

L'utilité de cette notation vous apparaîtra un peu plus loin. Pour l'instant,
retenez simplement que `.` signifie "ici".

Notez aussi que `..` peut très bien faire partie d'un chemin. Admettons que
vous soyez dans le répertoire `/etc/sysconfig` et que vous souhaitiez vous
rendre dans `/etc/ssh`. Vous pourriez vous y prendre comme ceci :

```
[kikinovak@centosbox ~]$ cd /etc/sysconfig
[kikinovak@centosbox sysconfig]$ pwd
/etc/sysconfig
[kikinovak@centosbox sysconfig]$ cd ..
[kikinovak@centosbox etc]$ cd ssh
[kikinovak@centosbox ssh]$ pwd
/etc/ssh
```

Il y a moyen de faire plus court et plus élégant :

```
[kikinovak@centosbox ~]$ cd /etc/sysconfig
[kikinovak@centosbox sysconfig]$ pwd
/etc/sysconfig
[kikinovak@centosbox sysconfig]$ cd ../ssh
[kikinovak@centosbox ssh]$ pwd
/etc/ssh
```

Si vous vous demandez pourquoi j'invoque `pwd` à chaque changement de
répertoire, c'est uniquement à des fins de démonstration, pour bien expliciter
le répertoire courant.

Vous pouvez également monter de plusieurs crans, si cela est nécessaire. Si
votre répertoire courant est `/etc/sysconfig/network-scripts` et si vous
souhaitez vous rendre dans `/etc/ssh`, il va falloir que vous montiez de deux
crans, pour ensuite entrer dans le répertoire `ssh`. En pratique, cela
ressemblerait à l'exemple suivant :

```
[kikinovak@centosbox ~]$ cd /etc/sysconfig/network-scripts
[kikinovak@centosbox network-scripts]$ pwd
/etc/sysconfig/network-scripts
[kikinovak@centosbox network-scripts]$ cd ../../ssh
[kikinovak@centosbox ssh]$ pwd
/etc/ssh
```



















