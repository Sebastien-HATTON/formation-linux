Visualiser : more et less
=========================

Document écrit par Nicolas Kovacs <info@microlinux.fr>

Gérer l'affichage de fichiers longs
-----------------------------------

L'utilitaire `cat` convient parfaitement pour visualiser des fichiers courts. À
titre d'exemple, utilisez-le pour visualiser le contenu de fichiers de
configuration comme `/etc/hostname`, `/etc/hosts`, `/etc/resolv.conf` ou
`/etc/fstab`. Ne tenez pour l'instant pas compte du fait que vous ne comprenez
pas grand-chose au contenu quelque peu énigmatique de ces fichiers, nous en
éluciderons la signification en temps et en heure. 

Pour des fichiers dont l'affichage dépasse la taille d'un écran, comme dans
l'exemple avec `/proc/cpuinfo`, on peut très bien revenir en arrière avec la
combinaison de touches **Maj**+**PageHaut** ou **Maj**+**PageBas**. Si l'on
s'est connecté à distance depuis un terminal graphique, on peut éventuellement
utiliser la barre de défilement ou la molette de la souris. 

Dans votre terminal, affichez le fichier `/etc/DIR_COLORS` à l'aide de `cat`.
L'affichage dépassera la taille de votre fenêtre. Revenez en arrière en
utilisant la combinaison de touches indiquée. 

Pour un fichier comme `/etc/DIR_COLORS`, cette manière de procéder est tout à
fait valable. Dans d'autres cas, il faudra procéder différemment. Voyez
vous-même pourquoi. 

```
$ cat /etc/services
...
isnetserv       48128/tcp               # Image Systems Network Services
isnetserv       48128/udp               # Image Systems Network Services
blp5            48129/tcp               # Bloomberg locator
blp5            48129/udp               # Bloomberg locator
com-bardac-dw   48556/tcp               # com-bardac-dw
com-bardac-dw   48556/udp               # com-bardac-dw
iqobject        48619/tcp               # iqobject
iqobject        48619/udp               # iqobject
matahari        49000/tcp               # Matahari Broker
```

Essayez de remonter au début du fichier, jusqu'à ce que vous aperceviez la
ligne contenant l'invite de commande. Vous n'en voyez pas la fin (ou plutôt :
le début) ? C'est normal : le fichier en question compte plus de 11.000 lignes.
Sa longueur pose un problème dans le sens où nous arrivons aux limites de :

  * la patience de l'utilisateur qui n'en peut plus d'appuyer sur
    **Maj**+**PageHaut** ;

  * la mémoire d'affichage de la console, qui ne conserve qu'un nombre limité
    de caractères : au-delà, il n'est plus possible de remonter davantage pour
    voir le début du fichier.


Visualiser avec more
--------------------

Bien sûr, nous pourrions décider d'ouvrir tous ces fichiers dont l'affichage
dépasse la taille d'un écran avec un éditeur de texte simple. Avec un système
Linux, si nous souhaitons seulement voir le contenu d'un certain fichier de
configuration sans toutefois l'éditer, nous aurons d'abord le réflexe
d'utiliser un *pager*. Le terme anglais a été francisé en "logiciel de
pagination", "logiciel de visualisation" ou "pageur" pour faire plus court. En
voici un.

```
$ more /etc/DIR_COLORS
```

Le visualiseur `more` affiche le fichier spécifié en argument en remplissant
exactement un écran, puis il s'arrête. Pour voir le reste du fichier, vous avez
le choix entre :

  * appuyer sur **Entrée** pour avancer ligne par ligne ;

  * utiliser la touche **Espace** pour progresser page par page ;

  * ou appuyer sur **Q** (comme *quit* ou "quitter") pour sortir du mode de
    visualisation.

Dès que `more` est arrivé à la fin du fichier, il considère qu'il a terminé son
travail. L'invite de commande réapparaît et le clignotement du curseur vous
indique que vous pouvez continuer de travailler normalement dans la console. 

Essayez `more` sur un fichier plus long :

```
$ more /etc/services
```

Les anciennes versions de `more` ne permettaient pas de revenir en arrière, ce
qui ne facilitait pas la recherche dans un fichier un peu plus volumineux. Les
versions plus récentes - comme celle incluse dans notre installation de CentOS
- ont ajouté cette fonctionnalité sous forme de la touche **B** (comme *back*),
qui permet de "feuilleter" le fichier page par page en sens inverse. Toutefois,
il faudra bien se résoudre à admettre que `more` fait partie de la poignée
d'utilitaires un peu obtus du monde Linux et qu'il n'est pas vraiment
confortable à utiliser.


Less is more : moins, c'est plus !
----------------------------------

C'est là que `less` entre en jeu. C'est un autre logiciel de pagination, dont
le but déclaré est de fournir un remplaçant confortable à `more`. Son nom est
un clin d'oeil ironique à la devise *less is more*, la version anglaise de la
tournure "ce n'est pas la peine d'en rajouter". Prenez cette boutade au pied de
la lettre et vous devinerez qu'effectivement, *less = more*. 

```
$ less /etc/services
```

Vous constatez que `less` vous laisse naviguer exactement comme `more`. La
touche **Entrée** sert à avancer d'une ligne, les touches **Espace** et **B**
permettent de feuilleter le fichier dans un sens et dans l'autre, **Q**
interrompt la pagination et fait réapparaître l'invite de commande. Cependant
ce n'est pas tout.

  * À la différence de `more`, `less` ne quitte pas le mode de pagination
    lorsqu'il arrive à la fin du fichier, ce qui évite les manipulations
    énervantes du style "retour à la case départ". 

  * Les touches directionnelles du clavier **FlècheHaut** et **FlècheBas**
    permettent également de naviguer dans le fichier. Non content de cela,
    **FlècheGauche** et **FlècheDroite** vous déplacent latéralement dans un
    fichier dont la largeur dépasse celle de l'écran. Essayez.

  * Il arrive très souvent que l'on ouvre un fichier de configuration à la
    recherche d'une certaine chaîne de caractères. Lorsque le fichier compte
    plusieurs milliers de lignes, cela revient à chercher une aiguille dans une
    botte de foin. Pour remédier à cela, `less` inclut une fonction de
    recherche simple. Pour exemple, ouvrez le fichier `/etc/passwd` avec
    `less`, appuyez sur la barre oblique **/** et faites-la suivre de la chaîne
    de caractères que vous cherchez, par exemple `nologin` ou votre nom
    d'utilisateur. Vous remarquez que `less` vous affiche toutes les
    occurrences trouvées en surbrillance. Utilisez la touche **N** (*next* =
    prochain) pour sauter d'occurrence en occurrence et **Maj**+**N** pour
    faire la même chose en sens inverse.

Enfin, précisons que `less` et `more` sont deux logiciels de visualisation non
destructifs, c'est-à-dire qu'ils ne modifient en aucune manière le contenu des
fichiers sur lesquels vous les appliquez. Leur utilisation se limite au seul
format texte simple. Étant donné que dans un système Linux, toute la
configuration est contenue dans des fichiers de ce format, nous constaterons
bientôt qu'il s'agit d'outils fort pratiques pour l'administration de notre
machine.

