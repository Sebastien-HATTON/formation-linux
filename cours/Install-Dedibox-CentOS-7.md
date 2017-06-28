Installer un serveur dédié CentOS 7 chez Online
===============================================

Document écrit par Nicolas Kovacs <info@microlinux.fr>

Cette page décrit l’installation et la configuration de CentOS 7 sur un serveur
dédié Dedibox SC de chez [Online](https://www.online.net/fr). Un serveur dédié,
c’est une machine généralement située dans la salle blanche d’un datacenter. Si
vous louez un serveur dédié, une interface web correspondante vous permettra
d’installer à distance l’OS de votre choix, à condition bien sûr que celui-ci
figure dans la panoplie de systèmes proposés. Vous serez ensuite seul maître à
bord de cette machine, et vous pourrez en faire ce que vous voudrez.

  ![Serveur dédié Dedibox](../images/splash-online.png)

Depuis quelques années, la société Online propose une gamme de serveurs dédiés
à des prix extrêmement intéressants, notamment la Dedibox SC, qui vous permet
de disposer d’un accès `root` sur votre propre machine à moins de dix euros par
mois.

Installation initiale de CentOS
-------------------------------

Dans un premier temps, il faut procéder au choix de la machine et du système
d’exploitation.

  1. Se connecter à la console d’Online :
  [https://console.online.net](https://console.online.net).

  2. Ouvrir le menu *Serveur* > *Liste des serveurs*.

  3. Sélectionner la machine > *Administrer* > *Installer*.

  4. *Distributions serveur* > *CentOS 7.x 64bits* > *Installer CentOS*.

Online propose un schéma de partitionnement par défaut, que nous allons modifier.

  1. Réduire la taille de la partition principale pour avoir un peu de marge.

  2. Augmenter la taille de la partition `/boot` : 500 Mo.

  3. La partition `/boot` sera formatée en `ext2`.

  4. Augmenter la taille de la partition d’échange : 4096 Mo.

  5. Remplir l’espace disponible pour la partition principale.

Voici ce que l’on obtient.

  ![Partitionnement Dedibox](../images/dedibox-partitionnement.png)

L’écran subséquent permet de choisir le mot de passe `root`, de définir un
utilisateur "commun mortel" et de choisir un mot de passe pour cet utilisateur.

Ensuite, l’interface affiche un récapitulatif des paramètres réseau de la
machine : nom d’hôte, adresse IP, masque de sous-réseau, IP de la passerelle,
DNS primaire et secondaire. Noter ces paramètres pour les avoir sous la main.

Il ne reste plus qu’à cliquer sur *Effacer l’intégralité de mes disques durs*
pour procéder à l’installation.


Connexion initiale
------------------

L’installation du système initial dure un peu plus d’une heure. La première
connexion SSH montre qu’il y a déjà pas mal d’activité autour de notre serveur.

```
Last failed login: Mon Nov 14 07:10:04 CET 2016 from 116.31.116.39
There were 87 failed login attempts since the last successful login.
```

On vérifie si tous les paquets du système initial sont bien à jour.

```
[root@sd-106630 ~]# yum check-update
Loaded plugins: fastestmirror, langpacks
Loading mirror speeds from cached hostfile
 * base: centos.serverspace.co.uk
 * extras: centos.serverspace.co.uk
 * updates: mirror.econdc.com
```

Apparemment, l’installateur de chez Online s’est chargé d’effectuer la mise à
jour initiale, ce qui semble logique.


Récupérer les scripts d’installation
------------------------------------

Installer Git.

```
# yum install git
```

Récupérer mes scripts et mes fichiers de configuration.

```
# cd
# git clone https://github.com/kikinovak/centos
```

Le répertoire `centos/el7/install/` contient une série de scripts numérotés qui
facilitent la configuration post-installation.


Élaguer l’installation initiale
-------------------------------

La prochaine étape consiste à épurer le système installé par Online pour
revenir au strict minimum, c’est-à-dire l’équivalent de ce que l’on obtient sur
une machine locale lorsqu’on opte pour une installation minimale. Pour
faciliter cette tâche, je fournis le script `00-elaguer-paquets.sh` dans le
répertoire `centos/el7/install/`. Ce script se charge de supprimer tous les
paquets qui ne font pas partie du système de base à proprement parler. Notons
que l’opération d’élagage supprime près de 200 paquets.On passe de 490 paquets
dans la configuration d’origine à un système minimal constitué de 288 paquets
et occupant un peu plus d’un gigaoctet d’espace disque.

```
# cd centos/el7/install
# ./00-elaguer-paquets.sh
```

Le script se sert de la liste de paquets `centos/el7/pkglists/minimal` qui a
été établie auparavant moyennant la commande suivante.

```
# rpm -qa --queryformat '%{NAME}\n' | sort > minimal
```

Afficher la vue d’ensemble sur les groupes de paquets.

```
# yum group list hidden | less
```

Il faudra rectifier à la main le statut des groupes installés.

```
# yum group mark remove "Core"
# yum group mark remove "Base"
```

Redémarrer.

```
# systemctl reboot && exit
```

À l’issue du redémarrage, on réinstallera l’éditeur Vim pour un peu plus de
confort.

```
# yum install vim-enhanced
```


Limiter le nombre de kernels installés
--------------------------------------

L’installation fournie par Online s’est chargée de supprimer les anciens
kernels.

```
# rpm -q kernel
kernel-3.10.0-327.36.3.el7.x86_64
```

Pour éviter qu’à l’avenir, les kernels s’entassent sur le système et saturent
la partition `/boot`, on va éditer `/etc/yum.conf` pour limiter le nombre de
kernels à préserver.

```
# /etc/yum.conf
...
installonly_limit=2
...
```

Supprimer les services inutiles
-------------------------------

L’installation minimale comporte une poignée de services inutiles, que nous
pouvons supprimer. Notons au passage que contrairement à ce qui se dit dans des
blogs un peu partout sur le Web, NetworkManager n’est **pas** nécessaire pour
la gestion du réseau. C’est juste une couche d’abstraction et de complexité
supplémentaire, et dont on peut aisément se passer.

```
# systemctl stop NetworkManager
# yum remove NetworkManager*
# systemctl stop postfix
# yum remove postfix
```

Pour la synchronisation, on préférera NTP à Chrony. Quant à Postfix, il sera
installé plus loin en cas de besoin.


Peaufiner la configuration réseau
---------------------------------

La prochaine étape consiste à mettre un peu plus d’ordre et de clarté dans les
fichiers de configuration réseau. Le répertoire
`/etc/sysconfig/network-scripts/` contient un fichier `ifcfg-eth0` que nous
pouvons rendre beaucoup plus lisible.

```
# /etc/sysconfig/network-scripts/ifcfg-eth0
DEVICE=eth0
TYPE=Ethernet
ONBOOT=yes
BOOTPROTO=static
IPADDR=195.154.65.130
NETMASK=255.255.255.0
```

La passerelle sera définie dans `/etc/sysconfig/network`.

```
# /etc/sysconfig/network
GATEWAY=195.154.65.1
```

Les deux serveurs DNS seront renseignés dans `/etc/resolv.conf`.

```
# /etc/resolv.conf
nameserver 62.210.16.6
nameserver 62.210.16.7
```

Le fichier `/etc/hosts` ressemblera à ceci.

```
# /etc/hosts
127.0.0.1      localhost.localdomain localhost
195.154.65.130 sd-41893.dedibox.fr
```

Quant à `/etc/hostname`, il est censé contenir le nom d’hôte entièrement
qualifié, et c'est tout. Veillez à ne surtout pas ajouter de commentaires dans
ce fichier, sous peine de provoquer toute une série d'erreurs bizarres.

```
sd-41893.dedibox.fr
```


Configurer les dépôts de paquets officiels
------------------------------------------

Éditer `/etc/yum.repos.d/CentOS-Base.repo` et activer les dépôts `[base]`,
`[updates]` et `[extras]` avec une priorité maximale.

```
# /etc/yum.repos.d/CentOS-Base.repo
[base]
enabled=1
priority=1
name=CentOS-$releasever - Base
...
[updates]
enabled=1
priority=1
name=CentOS-$releasever - Updates
...
[extras]
enabled=1
priority=1
name=CentOS-$releasever - Extras
...
```

Laisser le dépôt `[centosplus]` désactivé.

```
# /etc/yum.repos.d/CentOS-Base.repo
...
[centosplus]
enabled=0
name=CentOS-$releasever - Plus
...
```


Configurer le dépôt tiers EPEL
------------------------------

Le dépôt tiers [EPEL](https://fedoraproject.org/wiki/EPEL) (*Extra Packages for
Enterprise Linux*) fournit des paquets qui ne sont pas inclus dans la
distribution CentOS. Une fois que le dépôt officiel `[extras]` est configuré,
le dépôt EPEL peut se configurer très simplement à l’aide du paquet
correspondant.

```
# yum install epel-release
```

Définir les priorités du dépôt EPEL.

```
# /etc/yum.repos.d/epel.repo
[epel]
enabled=1
priority=10
name=Extra Packages for Enterprise Linux 7 - $basearch
...

[epel-debuginfo]
enabled=0
name=Extra Packages for Enterprise Linux 7 - $basearch - Debug
...

[epel-source]
enabled=0
name=Extra Packages for Enterprise Linux 7 - $basearch - Source
...
```


Activer les priorités
---------------------

Installer le plug-in `yum-plugin-priorities`.

```
# yum install yum-plugin-priorities
```

Vérifier s’il fonctionne correctement.

```
# yum check-update
Loaded plugins: fastestmirror, priorities
Loading mirror speeds from cached hostfile
 * base: mirror.plusserver.com
 * epel: mirrors.neterra.net
 * extras: centos.mirror.ate.info
 * updates: mirrors.ircam.fr
125 packages excluded due to repository priority protections
```


Installer les outils de base
----------------------------

Le script `01-installer-outils.sh` installe une poignée d’outils en ligne de
commande qui ne sont pas fournis par l’installation par défaut.

```
# cd /root/centos/el7/install/
# ./01-installer-outils.sh
```


Agrémenter la console
---------------------

Le script `02-configurer-base.sh` agrémente la console pour `root` et les
utilisateurs.

```
# ./02-configurer-base.sh
```

Outre la personnalisation du shell, le script se charge également de peaufiner
la configuration de l’éditeur Vim.

Prendre en compte la personnalisation du *shell* pour `root`.

```
# source ~/.bashrc
```

Récupérer la personnalisation du *shell* pour l’utilisateur initial.

```
# su - microlinux
$ cp -v /etc/skel/.bash* .
‘/etc/skel/.bash_logout’ -> ‘./.bash_logout’
‘/etc/skel/.bash_profile’ -> ‘./.bash_profile’
‘/etc/skel/.bashrc’ -> ‘./.bashrc’
$ source ~/.bashrc
$ exit
```


Activer SELinux
---------------

Dans la configuration par défaut, SELinux est désactivé.

```
# getenforce
Disabled
```

Éditer `/etc/selinux/config` pour activer SELinux.

```
SELINUX=permissive
SELINUXTYPE=targeted
```

Créer un fichier `/.autorelabel` pour lancer le réétiquetage du système au
prochain démarrage. Le cas échéant, vérifier s'il existe déjà. 

```
# touch /.autorelabel
```


Désactiver l’IPv6
-----------------

Créer un fichier `/etc/sysctl.d/disable-ipv6.conf` et l’éditer comme ceci.

```
# /etc/sysctl.d/disable-ipv6.conf
# Disable IPv6
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
```

La désactivation de l’IPv6 peut entraîner des problèmes avec le service SSH. Il
faut donc adapter sa configuration en conséquence.

Éditer `/etc/ssh/sshd_config` et spécifier les directives suivantes.

```
# /etc/ssh/sshd_config 
...
AddressFamily inet
ListenAddress 0.0.0.0
...
```

La directive `inet` désigne l’IPv4 et `inet6` l’IPv6. Les modifications seront
prises en compte au prochain redémarrage.


Installer un pare-feu personnalisé
----------------------------------

Vérifier si les paquets `iptables` et `iptables-services` sont installés.

```
# rpm -qa | grep iptables
iptables-1.4.21-13.el7.x86_64
iptables-services-1.4.21-13.el7.x86_64
```

Activer le service correspondant.

```
# systemctl enable iptables
# systemctl start iptables
```

Sous CentOS, la meilleure solution consiste à éditer un simple script Bash pour
iptables, en enregistrant la configuration à la fin du script.

```
# /usr/sbin/service iptables save
```

Copier le script `el7/firewall/firewall-dedibox.sh` dans un endroit approprié
et en le renommant, par exemple `/usr/local/sbin/firewall.sh`. Adapter le
script à la configuration réseau de la machine et aux services que l’on compte
héberger.

```
# firewall.sh
```

Afficher la configuration du pare-feu.

```
# iptables -L -v -n
```


Régler les problèmes relatifs à SELinux
---------------------------------------

Afficher les alertes SELinux.

```
# sealert -a /var/log/audit/audit.log
100% done
found 2 alerts in /var/log/audit/audit.log
```

J’obtiens deux alertes. Je m’occupe d’abord de celle qui me semble le plus
facilement gérable.

```
SELinux is preventing audispd from open access on the file 
/etc/ld.so.cache.
*****  Plugin restorecon (94.8 confidence) suggests   ******
If you want to fix the label. 
/etc/ld.so.cache default label should be ld_so_cache_t.
Then you can run restorecon. 
Do
# /sbin/restorecon -v /etc/ld.so.cache
```

Le contexte de sécurité a l’air correct.

```
# ls -Z /etc/ld.so.cache 
-rw-r--r--. root root unconfined_u:object_r:ld_so_cache_t:s0 
/etc/ld.so.cache
```

Je restaure le contexte par défaut comme indiqué.

```
# restorecon -v /etc/ld.so.cache 
```

Je vérifie si ça règle le problème.

```
# echo > /var/log/audit/audit.log
# systemctl reboot && exit
```

Après redémarrage, je relance un audit.

```
# sealert -a /var/log/audit/audit.log 
100% done
found 0 alerts in /var/log/audit/audit.log
```

Tout est en ordre, et je peux dorénavant passer en mode `Strict`.

```
# setenforce 1
```

