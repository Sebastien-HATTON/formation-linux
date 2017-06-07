Créer : touch et mkdir
======================

Document écrit par Nicolas Kovacs <info@microlinux.fr>

L'affichage détaillé de `ls` avec l'option `-l` nous montre que chaque fichier
est horodaté. Prenons par exemple le fichier `bonjour.txt` que nous avons créé
précédemment.

```
$ ls -l bonjour.txt 
-rw-rw-r--. 1 kikinovak kikinovak 19 26 mai   10:46 bonjour.txt
```


Modifier l'horodatage d'un fichier avec touch
---------------------------------------------

En l'occurrence, ce fichier a été créé (ou modifié pour la dernière fois) le 26
mai à 10h46. Attention à ne pas confondre les indications. `19` indique ici la
taille du fichier, c'est-à-dire dix-neuf octets. Maintenant, essayons ceci :

```
$ touch bonjour.txt 
$ ls -l bonjour.txt 
-rw-rw-r--. 1 kikinovak kikinovak 19  7 juin  13:13 bonjour.txt
```

Nous constatons que l'horodatage du fichier indique maintenant le 7 juin à
13h13. En effet, cela correspond à la date et à l'heure à laquelle j'écris ces
lignes. 


Créer un fichier vide avec touch
--------------------------------

Si le fichier spécifié n'existe pas, `touch` prendra soin de le créer. Essayons
avec un nom de fichier qui n'existe pas dans le répertoire courant.

```
$ touch yatahongaga.txt
$ ls -l yatahongaga.txt 
-rw-rw-r--. 1 kikinovak kikinovak 0  7 juin  13:16 yatahongaga.txt
```


Créer des fichiers texte sans éditeur de texte
----------------------------------------------

Tant que nous y sommes, je vous montre une méthode pour créer des fichiers
texte simples, à l'aide de la seule commande `cat`.

```
$ cat > ~/livres.txt << EOF
> Alice au pays des merveilles
> La montagne magique
> Faust
> EOF
$ ls -l livres.txt 
-rw-rw-r--. 1 kikinovak kikinovak 55  7 juin  13:19 livres.txt
$ cat livres.txt 
Alice au pays des merveilles
La montagne magique
Faust
```

Nous avons écrit trois lignes de texte dans un fichier `~/livres.txt`.
N'oubliez pas que le symbole *tilde* ~ représente ici le répertoire
d'utilisateur, dans mon cas `/home/kikinovak`. La suite de caractères `EOF`
(comme *End Of File*) définit la fin du fichier.

Aurions-nous pu obtenir quelque chose de comparable avec la commande `echo` ?
Essayons.

```
$ echo Beethoven > compositeurs.txt
$ cat compositeurs.txt 
Beethoven
```

La commande `echo` a créé ici un nouveau fichier `compositeurs.txt` en y
écrivant une ligne `Beethoven`. Jusqu'ici, cela ressemble beaucoup à ce que
nous avons fait plus haut avec `bonjour.txt`. Maintenant, essayons ceci.

```
$ echo Bach > compositeurs.txt 
$ cat compositeurs.txt 
Bach
```

Ce n'était donc pas la bonne méthode pour ajouter une ligne à notre fichier. Le
dernier contenu en date a tout simplement écrasé l'ancien contenu. Nous allons
donc nous y prendre autrement.

```
$ echo Bartok >> compositeurs.txt 
$ cat compositeurs.txt 
Bach
Bartok
```

Voilà qui est mieux. L'utilisation du double chevron `>>` au lieu du simple `>`
a provoqué l'ajout de la chaîne de caractères à la fin du fichier, en évitant
la substitution du contenu précédent. Si nous souhaitons ajouter un troisième
nom à la liste, il devrait donc suffire de répéter la dernière commande en
insérant un autre nom. Essayons. 

```
$ echo Schubert >> compositeurs.txt 
$ cat compositeurs.txt 
Bach
Bartok
Schubert
```

Effectivement, c'est bien cela. Soit dit en passant, nous en avons profité pour
avoir un autre petit aperçu de la redirection sous Linux. Passons maintenant à
la création de répertoires. 


Créer des répertoires avec mkdir
--------------------------------

La commande `mkdir` (comme *make directory*, vous aurez remarqué que les
informaticiens ont un problème avec les voyelles) sert à créer un nouveau
répertoire. Il suffit de spécifier le nom de ce dernier lorsqu'on invoque
`mkdir`. Créons un répertoire `Documents` dans notre répertoire d'utilisateur.

```
$ mkdir Documents
$ ls -ld Documents
drwxrwxr-x. 2 kikinovak kikinovak 6  7 juin  13:54 Documents
```

Il est également possible de spécifier le chemin complet du répertoire à créer.
Pour créer un répertoire `Images` à l'intérieur de mon répertoire d'utilisateur
`/home/kikinovak`, je pourrais le faire comme ceci.

```
$ mkdir /home/kikinovak/Images
$ ls -ld /home/kikinovak/Images
drwxrwxr-x. 2 kikinovak kikinovak 6  7 juin  13:57 /home/kikinovak/Images
```

Bien évidemment, dans cet exemple, il vous faudra remplacer `kikinovak` dans le
chemin par votre nom d'utilisateur. D'ailleurs, pour être sûr que c'est bien
dans notre répertoire d'utilisateur que l'on crée le dossier, nous aurions pu
écrire notre dernière commande comme ceci.

```
$ mkdir ~/Images
$ ls -ld ~/Images
drwxrwxr-x. 2 kikinovak kikinovak 6  7 juin  14:00 /home/kikinovak/Images
```


Créer une série de répertoires
------------------------------

Admettons qu'à l'intérieur du répertoire `~/Images`, nous souhaitions créer
trois sous-répertoires `Photos`, `Graphismes` et `Captures`. Nous pourrions le
faire de la façon suivante.

```
$ cd ~/Images
$ mkdir Photos Graphismes Captures
$ ls -l
total 0
drwxrwxr-x. 2 kikinovak kikinovak 6  7 juin  14:08 Captures
drwxrwxr-x. 2 kikinovak kikinovak 6  7 juin  14:08 Graphismes
drwxrwxr-x. 2 kikinovak kikinovak 6  7 juin  14:08 Photos
```

Ce dernier exemple appelle deux remarques. D'une part, il est tout à fait
possible de créer une série de répertoires *à la louche*. Il suffit de
spécifier leurs noms respectifs en argument, en les séparant d'un espace.
D'autre part, notez bien le `d` comme *directory* en tête des attributs
complets (`drwxrwxr-x`), qui signifie que nous avons affaire à des répertoires. 


Gare aux espaces !
------------------

N'oublions pas de dire deux mots sur un détail important qui constitue une
source d'erreur fréquente : les espaces dans les noms de fichiers et de
répertoires. Dans certains cas de figure (sur les serveurs, par exemple, ou
dans les réseaux hétérogènes, c'est-à-dire composés de machines dotées de
systèmes d'exploitation différents), il vaut mieux tout faire pour les éviter.
Dans d'autres cas, il est tout à fait possible de les utiliser, à condition
d'être sûr de ce que l'on fait. Je vous donne un exemple pour vous sensibiliser
à la problématique.

Retournez dans votre répertoire d'utilisateur (`cd` sans argument), créez un
répertoire `Test` et, à l'intérieur de ce dernier, créez un répertoire `Mes
Documents`, dont le nom vous semblera vaguement familier si vous venez d'un
autre système d'exploitation, du genre auquel on échappe difficilement. 

```
$ cd
$ mkdir Test
$ cd Test
$ mkdir Mes Documents
$ ls -l
total 0
drwxrwxr-x. 2 kikinovak kikinovak 6  7 juin  14:54 Documents
drwxrwxr-x. 2 kikinovak kikinovak 6  7 juin  14:54 Mes
```

Vous voyez le problème. La commande `mkdir` nous a créé deux répertoires
distincts, `Mes` et `Documents`. Ce n'est pas ce que nous voulions faire. 

Prenons un autre exemple pour voir comment nous aurions pu nous y prendre.
Revenons dans notre répertoire d'utilisateur, créons un répertoire `Test2` et,
à l'intérieur de ce dernier, essayons de créer trois répertoires distincts `Mes
Documents`, `Mes Images` et `Mes Films`. 

```
$ cd
$ mkdir Test2
$ cd Test2
$ mkdir "Mes Documents"
$ mkdir 'Mes Images'
$ mkdir Mes\ Films
$ ls -l
total 0
drwxrwxr-x. 2 kikinovak kikinovak 6  7 juin  14:57 Mes Documents
drwxrwxr-x. 2 kikinovak kikinovak 6  7 juin  14:58 Mes Films
drwxrwxr-x. 2 kikinovak kikinovak 6  7 juin  14:57 Mes Images
```

Cette fois-ci, nous avons bien obtenu le résultat escompté. Vous aurez
certainement remarqué que pour chacun des trois répertoires, je me suis servi
d'une syntaxe différente, en utilisant respectivement des guillemets doubles,
des guillemets simples et un caractère d'échappement devant l'espace. 


Exercice de révision
--------------------

Je vous propose de souffler un peu en faisant un petit exercice de révision.

1. Dans votre répertoire d'utilisateur, créez un dossier `Fichiers`.

2. À l'intérieur de ce dernier, créez trois sous-répertoires `Documents`,
`Images` et `Films`.

3. Créez-y trois fichiers vides nommés respectivement `texte.txt`, `photo.jpg`
et `film.avi`. 














