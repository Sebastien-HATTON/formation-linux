Installer CentOS dans VirtualBox
================================

Document écrit par Nicolas Kovacs (info@microlinux.fr)

Présentation
------------

[VirtualBox](https://www.virtualbox.org/) ("machine virtuelle") est un logiciel
de virtualisation de systèmes d’exploitation. En utilisant les ressources
matérielles de l’ordinateur (*système hôte*), VirtualBox permet la création
d’un ou de plusieurs ordinateurs virtuels dans lesquels s’installent d’autres
systèmes d’exploitation (*systèmes invités*). Les systèmes invités fonctionnent
en même temps que le système hôte, mais seul ce dernier a accès directement au
véritable matériel de l’ordinateur.

VirtualBox est un logiciel libre développé par la société Oracle. Il est
disponible pour toutes les plateformes :

  * Microsoft Windows

  * Mac OS X

  * Linux

  * Solaris

Créer la machine virtuelle
--------------------------

Dans l'exemple qui suit, nous allons installer CentOS 7 dans une machine
virtuelle.

Démarrer VirtualBox et cliquer sur *Nouvelle* pour créer une nouvelle machine
virtuelle.

  * Nom : CentOS 7

  * Type : Linux

  * Version : Red Hat (64-bit)

Définir la quantité de mémoire vive que l'on souhaite allouer à la machine
virtuelle. L'assistant suggère 1 gigaoctet, ce qui est le minimum requis pour
que l'installateur fonctionne correctement. Rien n'empêche d'allouer davantage
de mémoire en fonction de la RAM disponible sur le système hôte. 

La prochaine étape consiste à créer un disque dur virtuel. Garder l'option par
défaut *Créer un disque dur virtuel maintenant* et cliquer sur *Créer*. 

  * Type de fichier de disque dur : VDI (Image Disque VirtualBox)

  * Dynamiquement alloué

  * Taille du disque : 8 gigaoctets (minimum prédéfini)

Configurer la machine virtuelle
-------------------------------

L'icône de la nouvelle machine virtuelle apparaît dans l'écran principal de
VirtualBox. Vérifier si elle est bien en surbrillance et cliquer sur
*Configuration*. 

L'onglet *Système* nous permet éventuellement de rectifier le tir pour la
quantité de mémoire vive allouée à la machine virtuelle. 

Le sous-menu *Système > Processeur* permet d'augmenter le nombre de processeurs
disponibles pour la machine virtuelle, en fonction de la machine. Sur un
système Intel Core i7 muni de 8 coeurs, on pourra en utiliser 4 pour une
machine virtuelle. 

L'onglet *Affichage* ne nous concerne pas vraiment, étant donné que nous
installons un système en mode console, c'est-à-dire dépourvu d'interface
graphique. Notons quand-même que pour un système invité muni d'une telle
interface, il faut généralement augmenter la mémoire vidéo au maximum
disponible. C'est également une bonne idée de cocher l'option *Activer
l'accélération 3D*. 

L'onglet *Stockage* nous affiche notre disque dur virtuel `CentOS 7.vdi` relié
au *Contrôleur SATA*. Pour le support d'installation, nous avons le choix.

1. Si nous utilisons un CD-Rom ou un DVD pour installer CentOS, nous devons
cliquer sur le champ *Vide* relié au *Contrôleur IDE*, et dans le champ à
droite du  menu déroulant *Lecteur optique* - symbolisé par un CD-Rom - nous
pouvons sélectionner *Lecteur de l'hôte* et cocher *Mode direct*. 

2. Si nous avons téléchargé le fichier ISO de CentOS et que nous ne l'avons pas
encore gravé, nous pouvons directement fournir l'ISO à VirtualBox, ce qui nous
permettra éventuellement d'économiser un CD vierge. Dans ce cas, au lieu de
choisir *Lecteur de l'hôte*, nous sélectionnons l'option *Choisissez un fichier
de disque optique virtuel*, ce qui lance un navigateur de fichiers qui nous
permet de retrouver l'ISO en question, par exemple
`CentOS-7-x86_64-Minimal-1611.iso`. 

Il ne nous reste plus que l'onglet *Réseau* à configurer. Dans la configuration
par défaut, la machine virtuelle est en accès NAT (*Network Address
Translation*). Autrement dit, elle se situera dans son propre sous-réseau, elle
aura accès à Internet, mais elle ne pourra pas communiquer directement avec les
machines qui sont dans le même réseau que l'hôte. 

Dans le menu déroulant *Mode accès réseau*, sélectionnez *Accès par pont*, ce
qui aura pour effet de créer une machine virtuelle accessible dans la même
plage d'adresses que l'hôte et les autres machines du réseau local.
Éventuellement, dans le sous-menu *Avancé*, vous pouvez prédéfinir une adresse
MAC personnalisée pour la carte réseau de la machine virtuelle, par exemple
`080027ABCDEF`, ce qui vous permet d'emblée de configurer votre serveur DHCP
local en attribuant une adresse IP statique et un nom d'hôte correspondant à
votre machine virtuelle. 

Confirmez par *OK* et revenez dans l'écran principal. Il ne reste plus qu'à
*Démarrer* la machine virtuelle. 

Pour aller plus loin
--------------------

VirtualBox fait partie des applications du genre "usine à gaz" qui peuvent
intimider par une myriade de fonctionnalités et d'options. Si vous souhaitez
aller plus loin, n'hésitez pas à jeter un oeil sur l'excellente [documentation
du projet](https://www.virtualbox.org/wiki/Documentation), qui existe également
en traduction française. Elle a peut-être une ou deux version de retard, mais
ce n'est pas bien grave. 
