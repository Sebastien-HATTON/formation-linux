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



