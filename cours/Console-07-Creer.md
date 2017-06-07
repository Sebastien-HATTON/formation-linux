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












