Consulter l'aide en ligne : man et info
=======================================

Document écrit par Nicolas Kovacs <info@microlinux.fr>

Unix est long et la vie est brève
---------------------------------

Je ne sais pas si vous avez déjà eu l'occasion de jeter un coup d'oeil à un
manuel de référence Unix. Vous savez, ces ouvrages constitués de centaines,
voire de milliers de pages, présentant une myriade de commandes susceptibles
d'accepter chacune une ribambelle d'options, égrenées par ordre alphabétique,
le tout saupoudré de quelques tableaux indigestes, mais sans la moindre image.
En un mot, le genre de pavé sur lequel on peut faire asseoir le petit à table
pour les repas de famille et qui suscite chez tout lecteur normalement
constitué une envie violente d'aller habiter une île dépourvue d'électricité
pour y entamer une carrière de réparateur de pirogues au sein d'une
micro-économie basée sur le troc. 

*Ars longa vita brevis*, dit un proverbe latin, que l'on pourrait traduire par 
"Unix est long et la vie est brève". Même si vous ne connaissez qu'une poignée
de commandes avec une poignée d'options, il peut arriver que vous ne vous
rappeliez plus la syntaxe exacte de ce que vous souhaitez taper. Cela arrive
même souvent, pour ne pas dire tout le temps, aussi bien aux débutants qu'aux
experts. Quelle était donc l'option pour la copie récursive d'un répertoire ?
`cp -r`ou `-R` ? Et comment fallait-il s'y prendre pour voir les propriétés
détaillées d'un répertoire sans en afficher le contenu ? `ls` suivi de `-d`,
`-e` ou `-f` ? Essayer toutes les lettres de l'alphabet ? Parcourir les cours
de Nicolas Kovacs à la recherche de l'option perdue ?

Sous Linux, notre toute première réaction consistera à chercher l'aide
directement sur la machine, sous nos doigts pour ainsi dire.


Le bonheur est dans le PC
-------------------------

Tapez une commande, n'importe laquelle, pourvu que son utilisation nécessite
l'invocation d'un ou plusieurs argument(s). Invoquez-la sans arguments. Par
exemple :

```
$ cp
cp: opérande de fichier manquant
Saisissez « cp --help » pour plus d'informations.
```

Prenons notre machine au pied de la lettre et faisons exactement ce qu'elle
nous suggère de faire.

```
$ cp --help
Utilisation : cp [OPTION]... [-T] SOURCE DEST
         ou : cp [OPTION]... SOURCE... DIRECTORY
         ou : cp [OPTION]... -t DIRECTORY SOURCE...
Copier la SOURCE vers DEST ou plusieurs SOURCEs vers DIRECTORY.

Les arguments obligatoires pour les options longues le sont aussi pour les
options courtes.
  -a, --archive                identique à -dR --preserve=all
      --attributes-only        ne pas copier les données du fichier, seulement
                                 les attributs
      --backup[=CONTROL]       archiver chaque fichier de destination
  -b                           comme --backup mais n'accepte pas d'argument
      --copy-contents          copier le contenu des fichiers spéciaux en mode
                                 récursif
  -d                           identique à --no-dereference --preserve=links
  -f, --force                  si un fichier de destination existe et ne peut
                                 être ouvert alors le supprimer et réessayer
                                 (cette option est ignorée si l'option -n est
                                 aussi utilisée)
  -i, --interactive            demander confirmation avant d'écraser (surcharge
                                 une précédente option -n)
...
```

Le *shell* nous affiche une liste assez longue d'options applicables à la
commande `cp`, ainsi qu'une série d'explications sur son fonctionnement.


Afficher le manuel en ligne : man
---------------------------------

Pour la plupart, les commandes Unix acceptent ainsi une option `--help`
(parfois aussi tout simplement `-h`) qui affiche un écran d'aide succinct. En
revanche, toutes (à très peu d'exceptions près) disposent d'un véritable manuel
en ligne, que l'on peut afficher grâce à `man` suivi du nom de la commande sur
laquelle on souhaite se renseigner.

```
$ man cp

CP(1)                            User Commands                          CP(1)

NAME
       cp - copy files and directories

SYNOPSIS
       cp [OPTION]... [-T] SOURCE DEST
       cp [OPTION]... SOURCE... DIRECTORY
       cp [OPTION]... -t DIRECTORY SOURCE...

DESCRIPTION
       Copy SOURCE to DEST, or multiple SOURCE(s) to DIRECTORY.

       Mandatory arguments to long options are mandatory for short options too.

       -a, --archive
              same as -dR --preserve=all

       --attributes-only
              don't copy the file data, just the attributes

       --backup[=CONTROL]
              make a backup of each existing destination file

       -b     like --backup but does not accept an argument

       --copy-contents
              copy contents of special files when recursive

       -d     same as --no-dereference --preserve=links

       -f, --force
              if  an existing destination file cannot be opened, remove it and
              try again (this option is ignored when the -n option is also
              used)

       -i, --interactive
              prompt before overwrite (overrides a previous -n option)
      
...
```

L'affichage des pages de manuel s'effectue par le biais du visualiseur `less`,
et ce sont les raccourcis clavier de ce dernier qui servent à naviguer :
**Espace** pour avancer d'un écran et **Q** pour quitter. Alternativement, vous
pouvez également utiliser les touches directionnelles (**PageHaut**,
**PageBas**, **FlècheHaut**, **FlècheBas**) pour avancer et reculer.


Linux, Shakespeare et Molière
-----------------------------

Actuellement, une bonne partie des distributions Linux courantes et moins
courantes sont à peu près intégralement traduites en français. Autrement dit,
vous choisissez votre langue au moment de lancer l'installation, et à partir de
là, la procédure s'effectue entièrement en français, comme nous avons pu le
voir lors de l'installation de CentOS. 

Si le simple **utilisateur** d'un poste de travail sous Linux peut légitimement
s'attendre à ce que son environnement graphique s'affiche entièrement dans sa
propre langue, cela n'est pas tout à fait vrai pour l'**administrateur** d'un
système Linux, pour plusieurs raisons.

D'abord, les manuels en ligne ne sont que partiellement disponibles dans
d'autres langues que l'anglais. Leur traduction dans toutes les langues est une
entreprise d'une envergure pharaonique. 

Ensuite, la *lingua franca* d'un grand nombre de projets du monde du libre
(comme CentOS) reste l'anglais. Il existe certes des projets localisés pour
chaque pays (comme le forum francophone
[https://fr.centos.org/](https://fr.centos.org/), mais très souvent, le gros de
la communication passe par les forums, les listes de diffusion et les canaux
IRC anglophones. Dans le cas de CentOS, c'est la *mailing list*
`centos@centos.org`. Même s'il est théoriquement possible de survivre en tant
qu'administrateur Linux sans parler un seul mot d'anglais, attendez-vous à
quelques complications. Autant vouloir entamer une carrière au Vatican sans
comprendre un mot de latin. 

Si malgré tous ces avertissements, vous préférez afficher vos pages `man`
traduites en français, installez le paquet correspondant.

```
# yum install man-pages-fr
```

À partir de là, la page de manuel de `cp` s'affichera en français.

```
$ man cp

CP(1)                     Manuel de l'utilisateur Linux                     CP(1)

NOM
       cp - Copier des fichiers et des répertoires

SYNOPSIS
       cp [options] fichier chemin
       cp [options] fichier... répertoire

       Options POSIX : [-fiprR] [--]

       Options POSIX.1-2001 supplémentaires : [-HLP]

       Options GNU file-utils 4.0 (forme courte) :
       [-abdfilprsuvxPR]  [-S  SUFFIXE]  [-V {numbered,existing,simple}]
       [--backup=CONTROL] [--sparse=QUAND] [--help] [--version] [--]

       Options GNU file-utils 4.1 supplémentaires (forme courte) :
       [-HLP] [--copy-contents] [--no-preserve] [--reply=COMMENT]
       [--remove-destination]  [--strip-trailing-slashes] [--target-directory=RÉP]

DESCRIPTION
       cp sert à copier des fichiers (et éventuellement des répertoires).  On
       peut aussi bien copier un fichier donné vers une destination précise que
       copier un ensemble de fichiers dans un répertoire.

       Si le dernier argument correspond à un nom de répertoire, cp copie dans
       ce répertoire chaque  fichier  indiqué en  conservant  le même nom.
       Sinon, s'il n'y a que deux fichiers indiqués, il copie le premier sur le
       second.  Une erreur se produit si le dernier argument n'est pas un
       répertoire,  et  si  plus  de  deux  fichiers  sont indiqués. Par
       défaut, on n'effectue pas la copie de répertoires.

       Ainsi,  si  /a  est un répertoire, alors « cp -r /a /b » copiera /a dans
       /b/a et /a/x dans /b/a/x au cas où /b existe déjà, mais il copiera /a
       sur /b et /a/x dans /b/x si /b n'existait pas encore. Enfin, si  /b
       était  un fichier ordinaire, la copie échouera.
...
```


