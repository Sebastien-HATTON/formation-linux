Deux commandes de sortie simples : echo et cat
==============================================

Document écrit par Nicolas Kovacs <info@microlinux.fr>

echo : afficher une ligne de texte
----------------------------------

La commande `echo` affiche sur l'écran le texte spécifié en argument :

```
[kikinovak@centosbox ~]$ echo Bonjour Monsieur !
Bonjour Monsieur !
```

Voilà un grand pas pour nous, un petit pas pour l'humanité. Continuons :

```
[kikinovak@centosbox ~]$ echo Bonjour Monsieur ! > bonjour.txt
```

Cette fois-ci, il n'y a aucun résultat immédiat. Regardons le contenu du
répertoire courant :

```
[kikinovak@centosbox ~]$ ls
bonjour.txt
```

Explication : la flèche `>` a redirigé la sortie standard vers un fichier,
comme le formulerait quelqu'un du métier. En d'autres termes, la chaîne de
caractères `Bonjour Monsieur !` a été écrite dans un fichier `bonjour.txt` au
lieu de s'afficher sur l'écran.


cat : afficher et concaténer
----------------------------

Affichons le contenu de ce fichier avec la commande `cat` :

```
[kikinovak@centosbox ~]$ cat bonjour.txt 
Bonjour Monsieur !
```

Maintenant, créez deux nouveaux fichiers `bonjour2.txt` et `bonjour3.txt`,
comme ceci :

```
[kikinovak@centosbox ~]$ echo Bonjour Madame ! > bonjour2.txt
[kikinovak@centosbox ~]$ echo Bonjour les enfants ! > bonjour3.txt
```

Affichez leur contenu en utilisant `cat` :

```
[kikinovak@centosbox ~]$ cat bonjour2.txt 
Bonjour Madame !
[kikinovak@centosbox ~]$ cat bonjour3.txt 
Bonjour les enfants !
```

Si nous mettons nos trois nouveaux fichiers en argument, `cat` affiche leurs
contenus respectifs l'un après l'autre :

```
$ cat bonjour.txt bonjour2.txt bonjour3.txt 
Bonjour Monsieur !
Bonjour Madame !
Bonjour les enfants !
```

Vous remarquerez que dorénavant, je réduis l'invite de commande à un simple
`$`, ce qui est une manière habituelle de procéder et permet de faire figurer
ma commande sur une seule ligne. 

Là aussi, nous pouvons rediriger la sortie standard. Le résultat s'écrira dans
un fichier au lieu de s'afficher à l'écran :

```
$ cat bonjour.txt bonjour2.txt bonjour3.txt >
bonjourtous.txt
$ cat bonjourtous.txt 
Bonjour Monsieur !
Bonjour Madame !
Bonjour les enfants !
```

Ici, j'ai mis la charrue de la pratique avant les boeufs de la théorie pour
vous faire comprendre le fonctionnement de `cat`, qui remplit essentiellement
deux fonctions. D'une part, cette commande sert à la *concaténation* de
fichiers, d'où son nom. C'est le fait de rassembler le contenu de ces fichiers
en un seul gros :

```
$ cat fichier1 fichier2 fichier3 > grosfichier
```

D'autre part, l'autre rôle de `cat` est tout simplement d'afficher le contenu
de fichiers texte simples. 


Exemple pratique : tout savoir sur son processeur
-------------------------------------------------

Voici un cas d'utilisation de `cat` qu'on peut rencontrer dans le quotidien
d'un administrateur système. Il affiche des renseignements sur la machine : le
nombre de processeurs (processeur simple, biprocesseur, quad-core, etc.), leur
type (Intel, AMD, Celeron) et leur fréquence de travail (ou vitesse) exprimée
en MHz (mégahertz). 

```
$ cat /proc/cpuinfo
...
processor       : 1
vendor_id       : AuthenticAMD
cpu family      : 16
model           : 6
model name      : AMD Athlon(tm) II Neo N36L Dual-Core Processor
stepping        : 3
microcode       : 0x10000c8
cpu MHz         : 800.000
cache size      : 1024 KB
...
```

Cette simple utilisation de `cat` me montre donc que le serveur HP Proliant
Microserver sur lequel j'ai invoqué la commande dispose d'un biprocesseur
(processeur `0` et processeur `1`) AMD tournant à une fréquence de 800 MHz. 






