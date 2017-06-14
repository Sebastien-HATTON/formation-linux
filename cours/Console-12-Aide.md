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
(comme [CentOS](https://www.centos.org/)) reste l'anglais. Il existe certes des
projets localisés pour chaque pays (comme [le forum francophone de
CentOS](https://fr.centos.org/)), mais très souvent, le gros de la
communication passe par les forums, les listes de diffusion et les canaux IRC
anglophones. Dans le cas de CentOS, la *mailing list*
[centos@centos.org](https://lists.centos.org/mailman/listinfo/centos) constitue
le principal canal de communication du projet. Même s'il est théoriquement
possible de survivre en tant qu'administrateur Linux sans parler un seul mot
d'anglais, attendez-vous à quelques complications.  Autant vouloir entamer une
carrière au Vatican sans comprendre un mot de latin. 

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

Comment lire une page man ?
---------------------------

Les pages de manuel en ligne (ou *pages man*) sont toutes plus ou moins
organisées de la même façon.

  * Tout en haut de la page se trouve la commande, avec son numéro de chapitre,
    par exemple `CP(1)`. Traditionnellement, les pages de manuel sont
    organisées en huit sections distinctes, que nous n'allons pas toutes
    énumérer ici. Retenez seulement que certaines commandes intéressent les
    utilisateurs du système, alors que d'autres seront réservées à
    l'administrateur. Pour comprendre cette distinction, affichez la page du
    manuel de `cfdisk`, une commande qui sert à partitionner les disques durs.
    En haut de la page, `CFDISK(8)` vous indique qu'il s'agit d'une page de
    manuel de la section 8 et donc d'une commande réservée à l'administrateur
    du système. Les commandes des simples utilisateurs sont toutes regroupées
    dans la section 1. Certaines commandes disposent de leur page de manuel
    dans chaque section. Dans ce cas, il est nécessaire de spécifier le numéro
    de section pour les afficher séparément. À titre d'exemple, essayez
    successivement `man 1 printf` et `man 3 printf`.
    
  * L'en-tête intitulé `NAME` (ou `NOM`) fournit une description succincte de
    la commande.

  * La section `SYNOPSIS` désigne la syntaxe de la commande, c'est-à-dire la
    façon dont il faut invoquer les options et les arguments. Ceux-ci peuvent
    être facultatifs ou obligatoires.

  * `DESCRIPTION` fournit une explication détaillée du fonctionnement de la
    commande.

  * La section `OPTIONS` affiche une liste exhaustive de toutes les options
    applicables à la commande, en les détaillant une par une. Pour comprendre
    de quoi il s'agit, affichez par exemple la page de manuel de la commande
    `ls` (`man ls`) et essayez de retrouver les options qui vous sont déjà
    familières.

  * Plus loin, la section `BUGS` (ou `BOGUES`) est quelque chose que vous
    chercherez en vain chez un éditeur de logiciels propriétaires. Si la
    commande a pu présenter une quelconque anomalie ou un quelconque
    dysfonctionnement dans le passé, cette section vous en informe. Voyez par
    exemple la page de manuel de `fdisk` pour une telle section.

  * `SEE ALSO` (`VOIR AUSSI`) vous renvoie d'une part vers une documentation
    plus détaillée (les pages `info`, que nous verrons tout de suite), d'autre
    part vers des commandes "cousines", c'est-à-dire en relation étroite. La
    page de manuel de `fdisk` vous renverra ainsi vers `cfdisk`, `parted` et
    `sfdisk`, trois autres commandes pour manipuler les tables de partitions
    sous Linux.

  * Les pages de manuel en ligne comportent également souvent une section
    `AUTHORS` (`AUTEURS`) avec des informations de contact sous forme d'adresse
    web ou de courrier électronique, ce qui permet de signaler d'éventuels
    bogues. Ne vous sentez pas trop concerné par ceci, du moins pas pour
    l'instant. 


Recherche dans les pages de manuel
----------------------------------

J'ai dit plus haut que les pages de manuel en ligne s'affichaient par le biais
du visualiseur `less`. Cela signifie que nous pouvons également nous servir des
fonctions de recherche intégrées dans `less`. Pour essayer ceci, cherchons par
exemple toutes les occurrences du mot "modification" dans la page de manuel de
`ls`. Une fois que la page s'affiche, invoquez la fonctionnalité de recherche
grâce à la barre oblique `/` suivie de la chaîne de caractères (`modification`)
que vous souhaitez trouver dans le texte. Certaines pages de manuel sont assez
longues, et la fonction de recherche pourra s'avérer utile pour trouver
rapidement le brin d'information qu'il vous faut. 
    

Afficher le manuel en ligne : info
----------------------------------

Dans certains cas, les renseignements fournis par la commande `man` s'avèrent
insuffisants. Essayez par exemple d'obtenir des informations sur l'interpréteur
de commandes Bash en tapant `man bash`. Vous obtenez alors une série de pages
pour le moins cryptiques, qui ne vous sembleront probablement pas très
parlantes. 

Voici comment afficher un manuel bien plus complet.

```
$ info bash

File: bash.info,  Node: Top,  Next: Introduction,  Prev: (dir),  Up: (dir)

Bash Features
*************

This text is a brief description of the features that are present in the
Bash shell (version 4.2, 28 December 2010).

   This is Edition 4.2, last updated 28 December 2010, of 'The GNU Bash
Reference Manual', for 'Bash', Version 4.2.

   Bash contains features that appear in other popular shells, and some
features that only appear in Bash.  Some of the shells that Bash has
borrowed concepts from are the Bourne Shell ('sh'), the Korn Shell
('ksh'), and the C-shell ('csh' and its successor, 'tcsh').  The
following menu breaks the features up into categories based upon which
one of these other shells inspired the feature.

   This manual is meant as a brief introduction to features found in
Bash.  The Bash manual page should be used as the definitive reference
on shell behavior.

* Menu:

* Introduction::                An introduction to the shell.
* Definitions::                 Some definitions used in the rest of this
                                manual.
* Basic Shell Features::        The shell "building blocks".
* Shell Builtin Commands::      Commands that are a part of the shell.
* Shell Variables::             Variables used or set by Bash.
* Bash Features::               Features found only in Bash.
* Job Control::                 What job control is and how Bash allows you
                                to use it.
* Command Line Editing::        Chapter describing the command line
                                editing features.
* Using History Interactively:: Command History Expansion
* Installing Bash::             How to build and install Bash on your system.
...
```

À la différence d'une simple page `man`, vous disposez ici d'un curseur.
Celui-ci vous permet de naviguer de page en page, en suivant les liens. Une
page `info` est en fait une véritable arborescence de pages organisées de façon
hiérarchique. Pour naviguer dans cette arborescence, il suffit de placer le
curseur sur les bouts de texte compris entre une étoile `*` et un deux-points
`:`. La touche **Entrée** vous permet alors de vous rendre dans le noeud
(*node*) correspondant. Pour revenir en arrière, utilisez la touche **U**
(comme *up*, c'est-à-dire "remonter"). Là aussi, servez-vous du raccourci **Q**
pour quitter la page `info`.

L'organisation d'une page `info` est comparable au fonctionnement d'un site
web. Les pages y sont organisées hiérarchiquement, le passage d'une page à
l'autre se faisant par le biais d'hyperliens. Le bouton *Page précédente* du
navigateur permet de revenir en arrière. 

À en juger par les commentaires des utilisateurs chevronnés de systèmes Unix
dans les forums ou sur les listes de diffusion, les pages `info` ont moins
bonne presse que les pages `man`, en raison de leur système de navigation
quelque peu désuet. 

La formule magique à retenir
----------------------------

Votre mémoire est une véritable passoire et vous oubliez sans cesse les
commandes les plus basiques, au point de vous retrouver incapable d'utiliser
l'aide en ligne ? Ce n'est pas bien grave. Retenez juste ceci.

```
$ man man
```

La même chose vaut pour l'utilisation des pages `info`.

```
$ info info
```

Bien évidemment, il existe une quantité de façons d'obtenir de l'aide pour
votre système Linux : les sites de documentation, les blogs, les forums, les
listes de diffusion, les *newsgroups* (ou groupes de discussion Usenet), les
canaux IRC spécialisés, sans parler de la documentation spécifique à chaque
distribution. Dans le monde Linux, ce n'est certainement pas la documentation
qui fait défaut, mais il est parfois difficile de s'y retrouver. Pour
l'instant, n'oubliez pas que l'aide la plus immédiate lorsque vous utilisez la
ligne de commande se trouve à portée de doigts.


La liste de diffusion de CentOS
-------------------------------

La meilleure adresse pour trouver de l'aide avec un système CentOS, c'est sans
doute [la liste de diffusion
anglophone](https://lists.centos.org/mailman/listinfo/centos). Vous y trouverez
non seulement toute l'équipe de CentOS, mais également une communauté
sympathique d'administrateurs système, de développeurs et autres professionnels
compétents, qui apporteront des réponses aux questions les plus pointues que
vous pourrez leur poser. Je suis moi-même un utilisateur régulier de cette
*mailing list*. 


La communauté francophone de CentOS
-----------------------------------

Si vous préférez trouver de l'aide en français, visitez le [forum de la
communauté francophone de CentOS](https://fr.centos.org/forums/).
