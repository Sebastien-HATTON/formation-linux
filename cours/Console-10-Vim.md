Éditer des fichiers texte : Vi
==============================

Document écrit par Nicolas Kovacs <info@microlinux.fr>

Une réputation problématique
----------------------------

L'éditeur Vi (prononcé "vie aïe") est une partie intégrante de tout système
Unix, de la même manière qu'un château médiéval qui se respecte ne serait rien
sans un solide chevalet de torture dans son sous-sol. Gare à l'imprudent qui
s'aventure dans l'édition d'un texte avec Vi sans avoir respecté
l'avertissement : "Vous qui éditez un texte avec cet utilitaire, abandonnez
tout espoir d'arriver à vos fins !" Jetez un oeil sur ce qui se dit dans les
forums d'utilisateurs Linux novices au sujet de Vi et vous verrez que, si l'on
considère la moyenne arithmétique des opinions, il s'agit de toute évidence
d'un logiciel conçu par un vénusien halluciné à la suite d'un abus conséquent
de substances illicites diverses, synthétiques et puissantes.  Bref, de quoi
s'inquiéter. 


L'éditeur de texte installé sur tous les systèmes Linux
-------------------------------------------------------

Laissons donc de côté les chimères mythiques qui font la grimace à Vi et
essayons de voir la chose plus sobrement. Vi, ce n'est pas simplement un
éditeur de texte, c'est tout d'abord *le* programme d'édition de texte standard
présent sur *tous* les systèmes unixoïdes. En d'autres termes, que vous
installiez la dernière Red Hat ou l'avant-dernière SUSE, que vous travailliez
avec un Live CD comme Knoppix, Slax ou Slitaz, ou que vous essayiez de
récupérer des données après un crash avec SystemRescueCd, vous aurez à votre
disposition la panoplie d'éditeurs sélectionnée par le distributeur, mais Vi
s'y trouvera toujours, dans tous les cas, invariablement. Il est réellement
incontournable.On le trouve même sur un Mac, installé par défaut sur Mac OS X. 


Vi amélioré : Vim
-----------------

Vi existe en plusieurs versions ou incarnations, les plus répandues étant
l'ancêtre `vi`, la version améliorée `vim` (ou *Vi Improved*) et GVim, une
version graphique qui s'installe sur les postes de travail. CentOS fournit Vi
dans trois moutures différentes.

  * l'ancêtre Vi sous forme du paquet `vim-minimal` ;

  * Vim : `vim-enhanced` ;

  * GVim pour les environnements graphiques : `vim-X11`.

Notre système minimal ne comprend pour l'instant que le paquet `vim-minimal`.
Nous allons installer la version améliorée comme ceci.

```
# yum install vim
```

Cette opération récupère et installe automatiquement l'éditeur Vim et toutes
ses dépendances. Le téléchargement ne pèse pas très lourd, un peu moins de 20
Mo au total. Dorénavant, lorsque je mentionnerai "Vi", je parlerai en fait de
la version améliorée invoquée par la commande `vim`. Une fois que vous l'aurez
maîtrisée, vous ne serez pas dépaysé en utilisant une version plus
rudimentaire. 

L'apprentissage de Vi se déroule généralement en deux temps. Tout d'abord, il
s'agit d'apprendre à survivre, c'est-à-dire réaliser des opérations simples
d'édition de texte. Ensuite, une fois que les manipulations de base sont à peu
près maîtrisées, on découvre peu à peu le potentiel de cet éditeur. Il se
révèle alors être un outil de travail extrêmement puissant au quotidien, qui
peut servir aussi bien à administrer des serveurs distants et confectionner des
sites web qu'à élaborer des scripts ou des programmes.

Parmi les nombreuses fonctionnalités que présente Vim, on trouve la coloration
syntaxique, le formatage automatique, des fonctions de recherche et de
remplacement, l'utilisation de macros, l'intégration du *shell* et beaucoup
d'autres choses encore. Autant d'atouts qui en font l'outil de prédilection des
programmeurs, des webmasters ou des administrateurs système. 

Commençons par les fonctions simples.


Vimtutor
--------

Il existe un grand nombre de méthodes, de livres (imprimés ou en ligne) et de
tutoriels pour apprendre Vi. Tous ont un point en commun. Ils vous dressent tôt
ou tard une liste impressionnante de raccourcis clavier plus rébarbatifs les
uns que les autres, de façon à ce que seuls les utilisateurs très têtus ou les
compulsifs obsessionnels puissent espérer arriver au bout du tutoriel. Sachez
donc que le meilleur pédagogue pour enseigner Vi, c'est... Vi lui-même !

Il existe en effet un petit logiciel incorporé, nommé `vimtutor`, qui vous
permet de vous entraîner à souhait sur cet outil. C'est aussi ce que je vous
conseille de faire, en faisant fi de toutes les méthodes imprimées ou autres,
car Vi est un outil de travail qui doit avant tout vous rentrer dans les
doigts. Ne cherchez pas à mémoriser directement ses fonctions, mais
apprenez-les en les utilisant, par l'entraînement et par la répétition. 

```
$ vimtutor

===============================================================================
=    B i e n v e n u e  dans  l e  T u t o r i e l  de  V I M  -  Version 1.7 =
===============================================================================

     Vim est un éditeur très puissant qui a trop de commandes pour pouvoir
     toutes les expliquer dans un cours comme celui-ci, qui est conçu pour en
     décrire suffisamment afin de vous permettre d'utiliser simplement Vim.

     Le temps requis pour suivre ce cours est d'environ 25 à 30 minutes, selon
     le temps que vous passerez à expérimenter.

     ATTENTION :
     Les commandes utilisées dans les leçons modifieront le texte. Faites une
     copie de ce fichier afin de vous entraîner dessus (si vous avez lancé
     "vimtutor" ceci est déjà une copie).

     Il est important de garder en tête que ce cours est conçu pour apprendre
     par la pratique. Cela signifie que vous devez exécuter les commandes
     pour les apprendre correctement. Si vous vous contentez de lire le texte,
     vous oublierez les commandes !

     Maintenant, vérifiez que votre clavier n'est PAS verrouillé en
     majuscules, et appuyez la touche  j  le nombre de fois suffisant pour
     que la Leçon 1.1 remplisse complètement l'écran.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
                      Leçon 1.1 : DÉPLACEMENT DU CURSEUR


  ** Pour déplacer le curseur, appuyez les touches h,j,k,l comme indiqué. **
          ^
          k        Astuce :  La touche h est à gauche et déplace à gauche.
    < h       l >            La touche l est à droite et déplace à droite.
          j                  La touche j ressemble à une flèche vers le bas.
          v
  1. Déplacez le curseur sur l'écran jusqu'à vous sentir à l'aise.

  2. Maintenez la touche Bas (j) enfoncée jusqu'à ce qu'elle se répète.
     Maintenant vous êtes capable de vous déplacer jusqu'à la leçon suivante.

  3. En utilisant la touche Bas, allez à la Leçon 1.2.

NOTE : Si jamais vous doutez de ce que vous venez de taper, appuyez <Échap>
       pour revenir en mode Normal. Puis retapez la commande que vous vouliez.
```

À partir de maintenant, il suffit de lire attentivement les instructions qui
vous sont données et de les mettre en pratique. Vimtutor estime qu'il faut
"entre 20 et 30 minutes" pour apprendre les bases. Comptez plutôt entre trois
quarts d'heure et une bonne heure si vous débutez. Si vous prévoyez d'effectuer
un passage complet par jour, il y a fort à parier qu'au bout de trois ou quatre
jours, vous soyez raisonnablement à l'aise dans l'édition de fichiers de
configuration avec Vi. 

Les raccourcis clavier peuvent vous paraître pour le moins biscornus, notamment
les déplacements du curseur, mais cela s'explique très simplement. Vim a été
conçu spécialement pour les utilisateurs qui ont l'habitude de dactylographier.
Si vous faites partie des gens qui utilisent leurs dix doigts pour taper sans
regarder le clavier, vous serez très vite agréablement surpris par la
redoutable efficacité de cet outil de travail.



