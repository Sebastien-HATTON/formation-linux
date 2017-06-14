Travailler moins pour taper plus
================================

Document écrit par Nicolas Kovacs <info@microlinux.fr>

Si vous avez patiemment suivi les exemples et les exercices jusqu'ici, il y a
des chances pour que vous soyez quelque peu dépité par un certain manque de
confort de la console.

  1. L'interface en ligne de commande requiert de saisir des commandes
  interminables, leurs options, les noms de fichiers, les chemins d'accès
  complets, etc.

  2. Saisir tout ce texte représente une certaine quantité de travail.

  3. Personne n'aime travailler.

Il en résulte que personne n'aime travailler en ligne de commande. Le moment
est donc venu de se soucier du confort de l'utilisateur. La notion de confort
d'utilisation peut également s'appliquer à la ligne de commande, car nous
allons aborder une fonctionnalité extrêmement puissante du *shell*.


La complétion automatique
-------------------------

Commençons par une commande simple.

```
$ /usr/sbin/getenforce
```

L'invocation de la commande `getenforce` avec son chemin complet représente un
certain travail. En tout et pour tout, il faut saisir vingt caractères et
confirmer par **Entrée**. Pfouh.

Essayons donc de faire plus court. Tapez ceci, sans confirmer par **Entrée**.

```
$ /u
```

Appuyez sur la touche **Tab**. Pour ceux qui ne savent pas ce que c'est : la
touche Tabulation (**Tab** pour les intimes) se situe au-dessus de la touche de
verrouillage des majuscules, en haut à gauche du clavier. Elle est ornée de
deux flèches pointant en sens inverse, une vers la droite, une vers la gauche.

Vous constatez que le *shell* a complété ce que vous avez tapé.

```
$ /usr/
```

Continuez. Ajoutez un `s` et un `b`.

```
$ /usr/sb
```

Appuyez à nouveau sur **Tab**.

```
$ /usr/sbin/
```

Ensuite, saisissez `g`, `e`, `t`, `e`.

```
$ /usr/sbin/gete
```

Et pour finir, appuyez une dernière fois sur **Tab**.

```
$ /usr/sbin/getenforce
```

Prenons un autre exemple. Imaginons que vous souhaitiez vous rendre dans le
répertoire `/boot`. Vous devriez donc taper la commande suivante.

```
$ cd /boot
```

Essayons de faire comme tout à l'heure.

```
$ cd /b
```

Nous appuyons sur **Tab** et... il ne se passe rien. Tout au plus, le terminal
a émis un *bip* et c'est tout. Appuyons une seconde fois sur **Tab**.

```
$ cd /b
bin/  boot/ 
```

Ici, le *shell* est confronté à une ambiguïté. Il ne sait pas s'il doit se
rendre dans `/bin` ou dans `/boot`. Lorsque vous avez appuyé une seconde fois
sur **Tab**, il vous a montré toutes les possibilités qui s'offrent à lui. Pour
lever l'ambiguïté, il suffit de fournir un caractère supplémentaire...

```
$ cd /bo
```

... suivi de **Tab**.

```
$ cd /boot/
```

Voyons un autre exemple pour illustrer ce fonctionnement. Je souhaite afficher
le contenu du répertoire `/sbin`. J'invoque donc la commande suivante.

```
$ ls /sbin
```

Si je me contente de saisir `ls /s` suivi de **Tab**, j'entends un simple *bip*
et rien ne se passe. Une seconde pression sur la touche **Tab** me montre alors
toutes les possibilités qui s'offrent à moi. 

```
$ ls /s
sbin/ srv/  sys/  
```

Il suffit donc de saisir le `b` de `/sbin` pour lever l'ambiguïté et permettre
au *shell* de compléter le nom du répertoire.

Continuons avec un peu de pratique. Dans votre répertoire d'utilisateur, créez
une série de cinq fichiers vides.

```
$ touch fichier1.txt fichier2.txt fichier3.txt
$ touch fichier3.png fichier3.avi
```

Admettons que vous souhaitiez afficher les propriétés détaillées de chacun
d'entre eux, un par un. Commencez par le premier.

```
$ ls -l f
```

**Tab** :

```
$ ls -l fichier
```

`1`, puis **Tab** :

```
$ ls -l fichier1.txt
```

Procédez de même avec `fichier2.txt` et `fichier3.txt`. Vous constaterez
qu'avec `fichier3.txt`, vous aurez deux ambiguïtés à lever.

```
$ ls -l f
```

**Tab** :

```
$ ls -l fichier
```

`3`, **Tab** :

```
$ ls -l fichier3.
```

**Tab**, **Tab** :

```
$ ls -l fichier3.
fichier3.avi  fichier3.png  fichier3.txt
```

`t`, **Tab** :

```
$ ls -l fichier3.txt
```

Résultat des courses :

  * moins de touches à actionner ;

  * autant d'heures de loisirs de gagnées, que l'on pourra mettre à
    contribution pour aller à la plage ou faire de l'escalade ;

  * plus aucune raison pour détester le travail en ligne de commande.


Reculer pour mieux sauter
-------------------------

Comme tous les outils puissants, la complétion automatique requiert un certain
entraînement avant d'être efficace. Il se peut même qu'au début, cette façon de
procéder vous ralentisse. Faites fi de cette frustration initiale et
accrochez-vous, car cela en vaut vraiment la peine. L'utilisation systématique
de la complétion automatique vous fera gagner un temps considérable. 

J'en profite pour prodiguer un autre conseil : apprenez à dactylographier,
c'est-à-dire utiliser vos dix doigts sans regarder le clavier. Là aussi, les
bénéfices que vous en tirerez dépasseront de loin l'investissement que cela
nécessite au départ. Pensez en termes de productivité accrue et surtout de
migraines évitées. Vos yeux n'auront plus à effectuer des va-et-vient
incessants entre les touches.


La paresse devient un gage de qualité
-------------------------------------

La complétion automatique ne sert pas seulement à satisfaire le paresseux qui
sommeille en nous tous. Elle joue un autre rôle pour le moins aussi important
que celui de vous faire gagner du temps. Voyons un autre exemple pratique. 

Le répertoire `/etc/X11/xorg.conf.d`est censé contenir la configuration du
serveur graphique. Ne vous inquiétez pas si vous ne savez pas ce que c'est.
Pour l'instant, nous aimerions seulement afficher le contenu de ce répertoire,
sans trop nous soucier des détails techniques. Nous invoquons donc la commande
suivante, sans utiliser la complétion automatique.

```
$ ls /etc/x11/xorg.conf.d
```

Et nous nous retrouvons face au message d'erreur suivant.

```
ls: impossible d'accéder à /etc/x11/xorg.conf.d: Aucun fichier ou dossier de ce type
```

Là, nous restons quelque peu perplexes. La documentation du serveur graphique a
pourtant insisté sur l'utilisation de ce répertoire, et voilà qu'il n'existe
pas. Que se passe-t-il donc ?

Regardez bien : le sous-répertoire de `/etc` s'appelle `X11` (avec un `X`
majuscule) et non `x11`, ce qui n'est pas la même chose. Maintenant, invoquez
cette même commande, mais en utilisant la complétion automatique, c'est-à-dire
en tapant...

```
$ ls /e
```

**Tab** :

```
$ ls /etc/
```

`X`, **Tab** :

```
$ ls /etc/X11/
```

`x`, **Tab** :

```
$ ls /etc/X11/xorg.conf.d/
```

Ici, le *shell* a complété le nom du répertoire correctement. Si nous avions
essayé de taper `x` au lieu de `X` pour le répertoire `X11`, nous nous serions
tout de suite aperçu de notre erreur, suite à l'absence pour le moins suspecte
de `X11` dans la liste des répertoires proposés.

Cette petite expérience nous permet de tirer la conclusion suivante. Outre
l'avantage d'accélérer la saisie de façon considérable, la complétion
automatique offre également un contrôle de qualité, en jouant un rôle non
négligeable de correcteur.


Répéter une commande
--------------------

Dans certains cas, le passage par la case départ est inévitable. Une fois que
nous avons invoqué notre commande erronée et qu'elle nous a gratifié d'un
message d'erreur, il va bien falloir se résoudre à la ressasir, en prenant soin
de ne pas commettre de faute de frappe. Est-il donc vraiment nécessaire de tout
recommencer depuis le début, juste à cause d'une petite coquille ? Non, comme
vous le montre cet autre exemple. 

```
$ ls /etc/X11/xorg.donf.d
ls: impossible d'accéder à /etc/X11/xorg.donf.d: Aucun fichier ou dossier de ce type
```

Après la petite surprise initiale, vous vous apercevez tout de suite de
l'erreur. Vous avez tapé `xorg.donf.d` au lieu de `xorg.conf.d`. Pour corriger
votre erreur, il vous suffirait de ressaisir la commande en prenant soin, cette
fois-ci, d'écrire correctement le nom du répertoire. Avant de faire cela,
appuyez simplement sur la touche **FlècheHaut** et voyez ce qui se passe.

```
$ ls /etc/X11/xorg.donf.d
```

En effet, votre *shell* a gardé la dernière commande en mémoire. Dans ce cas,
il sera plus simple de remplacer le `d` par un `c` que de retaper l'intégralité
de la commande. 

Ce n'est pas tout. Actionnez plusieurs fois de suite la touche **FlècheHaut**
et observez ce qui se passe. Apparemment, le *shell* n'a pas mémorisé seulement
la dernière commande, mais toutes celles que vous avez pu invoquer depuis belle
lurette. Ajoutez à cela la touche **FlècheBas** et vous voilà capable de
naviguer dans l'historique de toutes les commandes saisies jusque-là. Enfin,
pas toutes, il y a une limite quand-même. 


Utiliser l'historique des commandes
-----------------------------------

Il existe un moyen très simple d'afficher la liste de toutes les commandes que
vous avez pu saisir.

```
$ history
...
558  alias rm='rm -i'
559  mkdir repertoirebidon
560  touch repertoirebidon/fichierbidon
561  rm -rf repertoirebidon/
562  vimtutor
...
```

Pour répéter l'une des commandes dans la liste, remontez dans l'historique en
appuyant autant de fois que nécessaire sur la touche **FlècheHaut**. Dans
certains cas, ce sera un exercice fastidieux et il vaut mieux afficher
l'historique complet, puis sélectionner la commande directement. Pour ce faire,
il suffit de taper un point d'exclamation `!` suivi du numéro de la commande.
Admettons que je veuille réinvoquer la commande `mkdir repertoirebidon` de
l'exemple précédent, il me suffirait de faire ceci.

```
$ !559
mkdir repertoirebidon
```

Soyez tout de même vigilant en utilisant cette fonctionnalité du *shell*. La
commande est exécutée directement, sans attendre une quelconque confirmation
avec la touche **Entrée**. Ne l'utilisez donc pas avec des commandes
destructives comme `rm`.


Invoquer une commande en utilisant la recherche inversée
--------------------------------------------------------

Dans le rayon "historique du shell", je vous montre une dernière fonctionnalité
très utile au quotidien : la recherche inversée ou *reverse search*. Admettons
que la dernière fois que vous ayez invoqué la commande `mkdir`, c'était pour
créer `repertoirebidon`. Admettons encore que vous ayez effacé
`repertoirebidon` par la suite et que vous souhaitiez le recréer maintenant.
Pour ce faire, vous avez plusieurs possibilités.

  * invoquer `mkdir repertoirebidon` tout simplement, en saisissant tous les
    caractères de la commande ;

  * actionner la touche **FlècheHaut** plusieurs fois de suite, jusqu'à ce que
    vous finissiez par tomber sur la commande souhaitée ;

  * afficher l'historique (`history`), chercher la commande que vous souhaitez,
    puis l'exécuter par le biais du point d'exclamation `!` suivi du numéro
    dans l'historique.

Il existe une autre solution, beaucoup plus simple. Tapez simplement
**Ctrl**+**R** et vous verrez que votre invite de commande change.

```
(reverse-i-search)`': 
```

À présent, dès que vous tapez les premiers caractères de la commande, le
*shell* complète instantanément avec ce qu'il trouve dans l'historique. En
l'occurrence, il me suffit ici de saisir `m` et `k` pour obtenir ce que je
veux.

```
(reverse-i-search)`mk': mkdir repertoirebidon
```

Il ne me reste qu'à confirmer par **Entrée** pour exécuter la commande.
Alternativement, je peux appuyez une deuxième fois sur **Ctrl**+**R** pour
afficher la prochaine commande dans l'historique qui commence par `mk`.




