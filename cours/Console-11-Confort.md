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

$ ls -l fichier

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

$ ls -l fichier

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
















