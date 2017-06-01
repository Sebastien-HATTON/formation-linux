CentOS 
======

Document écrit par Nicolas Kovacs <info@microlinux.fr>


La foire aux distributions
--------------------------

L'utilisateur novice de Linux se retrouve d'abord confronté à un choix qui peut
s'avérer déroutant : Linux, oui, mais lequel ? Ubuntu ? Debian ? Red Hat ?
Fedora ? SUSE ? En effet, il ne s'agit pas d'un seul "système Linux", mais de
toute une ribambelle de "distributions".

Le portail Distrowatch.com recense actuellement plusieurs centaines de
distributions Linux activement maintenues. Cette liste s'allonge toutes les
semaines, et c'est sans compter les milliers de projets privés ou autrement
confidentiels dans le monde entier.


Qu'est-ce qu'une distribution Linux ?
-------------------------------------

Dans le cas d'une des grandes distributions courantes comme Red Hat Enterprise
Linux, Debian, Ubuntu, CentOS, Fedora ou SUSE, une distribution Linux est un ensemble
cohérent, composé en règle générale :

* du système de base

* d'une série d'outils d'administration

* d'une panoplie logicielle

* d'un installateur


Red Hat Enterprise Linux
------------------------

La société Red Hat a été fondée en 1993. C'est actuellement la marque la plus
reconnue dans le monde de l'Open Source. Red Hat est le leader de Linux en
entreprise. 

Red Hat Enterprise Linux est une distribution commerciale de qualité
*entreprise*. Nous verrons un peu plus loin ce que cela signifie concrètement.
Le modèle économique de Red Hat, c'est la souscription. Autrement dit, le
client paye pour avoir accès au support technique de Red Hat. En dehors de
cela, Red Hat respecte scrupuleusement les règles du logiciel libre en publiant
l'ensemble des codes source de ses systèmes. 


CentOS
------

CentOS (*Community Enterprise Operating System*) est un clone parfait de la
distribution Red Hat Enterprise Linux. Ses développeurs ont utilisé les sources
de Red Hat et les ont recompilées à leur propre sauce, en prenant soin
d'enlever tous les logos et icônes spécifiques à Red Hat. Cette opération est
tout à fait légale, car les sources de tous les composants des systèmes Red Hat
sont placées sous licence libre. Notons que cette démarche est d'ailleurs non
seulement tolérée, mais aussi encouragée par la société, étant donné que les
rapports d'erreurs des utilisateurs de CentOS sont à leur tour utilisés par Red
Hat.  Notons également que depuis la sortie de CentOS 7, Red Hat a décidé de
salarier l'équipe de dévelopeurs bénévoles de cette distribution. 

CentOS est donc un système techniquement identique et binairement compatible à
Red Hat Enterprise Linux. La seule différence réside dans le fait que Red Hat
fournit un support technique payant pour ses clients. En dehors de cela, c'est
une distribution de qualité *entreprise*, solide et éprouvée, et qui ne réserve
pas de mauvaises surprises. 


La qualité *entreprise*
-----------------------

En règle générale, vous pouvez utiliser votre système Linux de façon sûre tant
que vous disposez de mises à jour. Une fois que la période de support de votre
version a expiré, vous devez mettre à jour l'ensemble de la distribution vers
une version plus récente. 

Imaginons maintenant que votre entpreprise héberge son site de e-commerce sur
un serveur Linux. Une faille de sécurité importante vient d'être découverte sur
un des composants, et l'administrateur décide de mettre à jour le serveur.
Malheureusement, l'application de e-commerce ne semble plus compatible avec
certains des nouveaux composants. Le site ne fonctionne plus correctement, et
il faut songer à revoir d'urgence l'intégralité du code pour l'adapter à la
nouvelle version. C'est le scénario catastrophe. 

Et c'est là où les distributions de qualité *entreprise* entrent en jeu. Leur
ambition est de fournir une plate-forme robuste, stable et pérenne pour faire
tourner des applications sans causer de problèmes de compatibilité. Les deux
principes de base d'une telle distribution sont donc :

1. l'extension de la durée de support

2. la mise à disposition de mises à jour peu risquées

En pratique, pendant une période d'au moins cinq ans, parfois même beaucoup
plus, un tel système bénéficiera de mises à jour de sécurité sans que celles-ci
introduisent de nouvelles fonctionnalités susceptibles de causer des mauvaises
surprises. 

Les "grandes" distributions commerciales affichent cette qualité *entreprise*
dans leur nom même :

* Red Hat Enterprise Linux

* Red Hat Enterprise Workstation

* SUSE Linux Enterprise Server

* SUSE Linux Enterprise Desktop

Chacun de ces produits bénéficie en effet d'une période de support étendu
comprise entre sept et dix ans. En comparaison, la durée du système
communautaire Fedora est limitée à dix-huit mois, ce qui est bien trop court
pour un usage en entreprise.

Tout comme Red Hat Enterprise Linux en amont, chaque version de CentOS
bénéficie d'un cycle de support de dix ans pour chaque version.

* CentOS 6 est maintenu jusqu'au 30 novembre 2020.

* CentOS 7 est maintenu jusqu'au 30 juin 2024.


