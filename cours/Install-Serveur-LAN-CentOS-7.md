Installer un serveur de réseau local CentOS 7
=============================================

Document écrit par Nicolas Kovacs <info@microlinux.fr>

Cette page décrit de manière succincte l’installation et la configuration de
CentOS 7 sur un serveur de réseau local (*Local Area Network*).  CentOS 7 est
officiellement supporté jusqu’au 30 juin 2024. On choisira cette branche sur du
matériel raisonnablement récent qui supporte un OS 64-bits.  L’installateur de
CentOS 7 requiert au moins 1 Go de RAM. Sur une machine disposant de moins de
mémoire vive ou dotée d’un processeur 32-bits, on pourra opter pour CentOS 6.


Support d’installation
----------------------

On choisira le CD minimal, mais rien n’empêche d’utiliser le DVD.

  * `CentOS-7-x86_64-Minimal-1611.iso`

  * `CentOS-7-x86_64-DVD-1611.iso`

Graver le CD ou le DVD à partir de l’ISO téléchargé.

Sur les machines dépourvues de lecteur optique comme par exemple les serveurs
HP de la gamme Proliant Microserver, il faudra confectionner une clé USB
d’installation. L’image ISO est hybride et peut s’écrire directement sur une
clé USB.

```
# dd if=CentOS-7-x86_64-Minimal-1611.iso of=/dev/sdX
```


Démarrage
---------

Débrancher clés USB, disques externes et autres périphériques amovibles.
Autrement l’installateur les proposera au formatage.


Langue et clavier
-----------------

Dans l’écran de bienvenue, sélectionner la langue (Français) et la localisation
(Français – France). La disposition du clavier sera définie par le biais de
l’écran principal de l’installateur.


Interfaces réseau
-----------------

Le réseau n’est pas activé par défaut, il faut donc songer à l’activer
explicitement.

Les noms des interfaces réseau ont changé avec cette nouvelle version.
Désormais, on n’a plus affaire à `eth0`, `eth1`, `eth2`, `wlan0`, etc. Le
nouveau schéma de nommage est moins arbitraire et offre davantage de
consistance en se basant sur l’emplacement physique de la carte dans la
machine.

  * `enp2s0`

  * `enp3s0`

  * `enp3s1`

  * etc.


Date et heure
-------------

Vérifier si le fuseau horaire (*Europe/Paris*) est correctement configuré.
Éventuellement, activer *Heure du réseau* et vérifier les serveurs NTP.


Désactivation de Kdump
----------------------

Kdump est un mécanisme de capture lors du plantage d’un noyau. Il peut être
désactivé.


Partitionnement manuel
----------------------

L’outil de partitionnement graphique de CentOS n’est pas très intuitif. Voici
un exemple de schéma de partitionnement courant.


  * un disque RAID pour `/boot` de 500 MiB, formaté en `ext2`

  * un disque RAID pour la partition `swap`, équivalent à la RAM disponible

  * un disque RAID pour la partition principale, formaté en `ext4`

Avec deux disques, on optera pour le RAID 1. Si l’on dispose d’au moins trois
disques, on pourra choisir le RAID 5 pour la partition principale et le RAID 1
pour `/boot` et `swap`.

  1. Cliquer sur *Destination de l’installation*.

  2. Vérifier si le ou les disques durs sont bien sélectionnés.

  3. Cocher *Je vais configurer le partitionnement* et cliquer sur *Terminé*.

  4. Dans le menu déroulant, sélectionner *Partition standard* au lieu de *LVM*.


Partition /boot
---------------

La taille de la partition `/boot` sera relativement réduite. Il faudra veiller
à ne pas laisser s’entasser les vieux kernels sous peine de la remplir assez
rapidement.

  1. Cliquer sur le bouton **+** pour créer un nouveau point de montage.

  2. Créer le point de montage */boot* avec une capacité de 500 MiB.

  3. Définir le *type de périphérique RAID* avec un niveau *1*.

  4. Choisir le système de fichiers *ext2* et l’étiquette *boot*.

  5. Confirmer *Mise à jour des paramètres*.

Partition swap
--------------

Dans certains cas, la partition `swap` pourra être reléguée à la fin du disque
par l’installateur pour une utilisation optimale.

  1. Cliquer sur le bouton **+** pour créer un autre point de montage.

  2. Créer le point de montage *swap* en spécifiant sa capacité en GiB.

  3. Définir le *type de périphérique RAID* avec un niveau *1*.

  4. Choisir l’étiquette *swap*.

  5. Confirmer *Mise à jour des paramètres*.


Partition principale
--------------------

La partition principale occupera tout l’espace disque restant.

  1. Cliquer sur le bouton **+** pour créer un dernier point de montage.

  2. Créer le point de montage / sans spécifier la capacité souhaitée.

  3. Définir le *type de périphérique RAID* et le niveau de RAID *1* ou *5*.

  4. Si l’on utilise le RAID 5, il faudra revoir la capacité souhaitée à la
     hausse.  Pour ce faire, on peut se servir de la valeur *Espace total* en
     bas à gauche de l’écran et spécifier cette valeur (voire un peu plus) dans
     le champ *Capacité souhaitée*. L’installateur se chargera de recalculer
     l’espace disponible en fonction de la capacité des disques et du niveau de
     RAID.

  5. Choisir le système de fichiers *ext4* et l’étiquette *root*.

  6. Confirmer *Mise à jour des paramètres*, puis *Terminé*.


Choix des paquets
-----------------

Dans l’écran de sélection des logiciels du DVD, on optera pour le groupe de
paquets *Installation minimale* proposé par défaut. Le CD minimal ne laisse pas
le choix de toute façon.


Utilisateur initial
-------------------

Créer un utilisateur non privilégié `microlinux`. Cocher l’option *Faire de cet
utilisateur un administrateur* pour l’ajouter au groupe *wheel* et lui
permettre d’utiliser `sudo`.


Démarrer le service RNGD
------------------------

Il se peut que tous les services du système n’aient pas démarré comme prévu.

```
# systemctl status
● amandine
  State: degraded
   Jobs: 0 queued
 Failed: 1 units
 ...
```

Afficher le service fautif.

```
# systemctl --failed
  UNIT         LOAD   ACTIVE SUB    DESCRIPTION
● rngd.service loaded failed failed Hardware RNG 
  Entropy Gatherer Daemon
```

RNGD, c’est le générateur d’entropie du système. Dans sa configuration par
défaut, il se base sur un périphérique `/dev/hwrandom` qui n’existe pas sur
notre machine. Pour corriger ce comportement, il faut éditer le fichier de
configuration du service.

```
# /usr/lib/systemd/system/rngd.service
[Unit]
Description=Hardware RNG Entropy Gatherer Daemon

[Service]
ExecStart=/sbin/rngd -f -r /dev/urandom

[Install]
WantedBy=multi-user.target
```

Recharger la configuration.

```
# systemctl daemon-reload
```

Démarrer le service et vérifier son statut.

```
# systemctl start rngd.service
# systemctl status rngd.service
● rngd.service - Hardware RNG Entropy Gatherer Daemon
   Loaded: loaded (/usr/lib/systemd/system/rngd.service; ...
```


Synchronisation de la grappe RAID
---------------------------------

La synchronisation initiale d’une grappe RAID peut être assez longue. L’astuce
suivante permet d’accélérer le processus de façon significative.

```
# echo 50000 > /proc/sys/dev/raid/speed_limit_min
```


Configuration provisoire du réseau
----------------------------------

L’installation par défaut ne fournit pas la commande `ifconfig`, qui fait
partie du paquet `net-tools`. Dans un premier temps, il faudra afficher la
configuration réseau en utilisant la commande `ip` fournie par le paquet
`iproute2`.

```
# ip address
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN 
 ...
2: enp2s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 
   qdisc mq state UP qlen 1000
 link/ether 2c:27:d7:15:54:a1 brd ff:ff:ff:ff:ff:ff
 inet 192.168.1.2/24 brd 192.168.1.255 scope global dynamic enp2s0
 ...
3: enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 
 ...
# ip route
default via 192.168.1.1 dev enp2s0
```


Mise à jour initiale
--------------------

Installer le paquet `deltarpm`, qui permet d’accélérer la procédure de mise à
jour en téléchargeant la différence binaire entre un paquet et son correctif.

```
# yum install deltarpm
```

Procéder à la mise à jour initiale de l’installation.

```
# yum update
```

Redémarrer.

```
# systemctl reboot
```


Installer l’éditeur Vim
-----------------------

Pour faciliter l’édition des fichiers de configuration, on peut déjà installer
l’éditeur de texte Vim.

```
# yum install vim-enhanced
```


Faire le ménage dans les kernels
--------------------------------

Le paquet `yum-utils` nous facilitera la tâche.

```
# yum install yum-utils
```

Afficher les kernels installés.

```
# rpm -q kernel
kernel-3.10.0-327.el7.x86_64
kernel-3.10.0-327.13.1.el7.x86_64
```

Supprimer l’ancien kernel.

```
# package-cleanup --oldkernels --count=1
```

L’option `--count=x` spécifie le nombre de kernels que l’on souhaite garder.

Éditer `/etc/yum.conf` et définir le nombre de kernels à préserver.

```
# /etc/yum.conf
...
installonly_limit=2
...
```


Chargeur de démarrage
---------------------

En fonction du nombre de disques durs, il faudra installer le chargeur de
démarrage manuellement sur le MBR de chaque disque. En cas de défaillance d’un
des disques, on pourra toujours démarrer sur le ou les disques restants.

```
# grub2-install /dev/sda
# grub2-install /dev/sdb
# grub2-install /dev/sdc
# grub2-install /dev/sdd
```

Éditer les options dans `/etc/default/grub` pour afficher les messages de
démarrage et rendre la console plus lisible.

```
# /etc/default/grub 
...
GRUB_CMDLINE_LINUX="video=1024x768 quiet"
...
```

Alternativement.

```
# /etc/default/grub 
...
GRUB_CMDLINE_LINUX="nomodeset quiet vga=791"
...
```

Prendre en compte les modifications.

```
# grub2-mkconfig -o /boot/grub2/grub.cfg
```


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

Dans certains cas, il est souhaitable d’élaguer une installation existante pour
revenir à un système de base plus épuré. Pour ce faire, je fournis le script
`00-elaguer-paquets.sh` dans le répertoire `centos/el7/install/`. Ce script se
charge de supprimer tous les paquets qui ne font pas partie du système de base
à proprement parler, c’est-à-dire l’équivalent de ce que l’on obtient lorsqu’on
effectue une installation minimale.

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
# systemctl stop chronyd
# yum remove chrony
```

Pour la synchronisation, on préférera NTP à Chrony.


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


Configurer le dépôt tiers ELRepo
--------------------------------

Le dépôt [ELRepo](http://elrepo.org/) est un autre dépôt tiers pour CentOS, qui
se concentre sur les drivers.

  * systèmes de fichiers

  * cartes réseau

  * etc.

Aller sur le site du projet, télécharger et installer le paquet
`elrepo-release` pour les versions 7.x.

```
# yum localinstall elrepo-release-*.rpm
```

Désactiver l’ensemble des dépôts `[elrepo]`.

```
# /etc/yum.repos.d/elrepo.org 
[elrepo]
enabled=0
...
[elrepo-testing]
enabled=0
...
[elrepo-kernel]
enabled=0
...
[elrepo-extras]
enabled=0
...
```

On activera ce dépôt ponctuellement en cas de besoin.

```
# yum --enablerepo=elrepo-kernel install kernel-lt
```


Installer les outils de base
----------------------------

Le script `01-installer-outils.sh` installe une poignée d’outils en ligne de
commande qui ne sont pas fournis par l’installation par défaut.

```
# ./01-installer-outils.sh
```


Agrémenter la console
---------------------

Le script `02-configurer-base.sh` agrémente la console pour `root` et les
futurs utilisateurs.

```
# ./02-configurer-base.sh
```

Outre la personnalisation du *shell*, le script se charge également de
peaufiner la  configuration de l’éditeur Vim.

Prendre en compte la personnalisation du *shell* pour `root`.

```
# source ~/.bashrc
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

La désactivation de l’IPv6 peut entraîner des problèmes avec le service SSH.
Il faut donc adapter sa configuration en conséquence.

Éditer `/etc/ssh/sshd_config` et spécifier les directives suivantes.

```
# /etc/ssh/sshd_config 
...
#Port 22
AddressFamily inet
ListenAddress 0.0.0.0
#ListenAddress ::
...
```

La directive `inet` désigne l’IPv4 et `inet6` l’IPv6. Là aussi, les
modifications seront prises en compte au prochain redémarrage.


Configurer le réseau
--------------------

Dans l’exemple, l’interface `enp2s0` se situe côté Internet.

```
# /etc/sysconfig/network-scripts/ifcfg-enp2s0
DEVICE=enp2s0
TYPE=Ethernet
ONBOOT=yes
BOOTPROTO=static
IPADDR=192.168.2.5
NETMASK=255.255.255.0
```

Côté réseau local, c’est l’interface `enp3s1`.

```
# /etc/sysconfig/network-scripts/ifcfg-enp3s1
DEVICE=enp3s1
TYPE=Ethernet
ONBOOT=yes
BOOTPROTO=static
IPADDR=192.168.3.1
NETMASK=255.255.255.0
```

L’adresse IP de la passerelle sera notée dans `/etc/sysconfig/network`.

```
# /etc/sysconfig/network 
GATEWAY=192.168.2.1
```

Renseigner le ou les serveurs DNS.

```
# /etc/resolv.conf
nameserver 192.168.2.1
```

Corriger la configuration du nom d’hôte.

```
# /etc/hosts
127.0.0.1 localhost.localdomain localhost
192.168.3.1 amandine.sandbox.lan amandine
```

Quant à `/etc/hostname`, il est censé contenir le nom d’hôte entièrement
qualifié, et c'est tout. Veillez à ne surtout pas ajouter de commentaires dans
ce fichier, sous peine de provoquer toute une série d'erreurs bizarres.

```
amandine.sandbox.lan
```


Créer un utilisateur
--------------------

Pour éviter les connexions en `root` depuis l’extérieur, on peut éventuellement
créer un utilisateur non privilégié si cela n’a pas été fait durant
l’installation.

```
# useradd -c "Microlinux" microlinux
# passwd microlinux
```

Ajouter l’utilisateur au groupe `wheel`.

```
# usermod -G wheel microlinux
```

Éditer `/etc/pam.d/su` et décommenter la ligne qui requiert l’appartenance au
groupe `wheel` pour acquérir les droits de `root`.

```
# /etc/pam.d/su
...
# Uncomment the following line to require a user to be in 
# the "wheel" group.
auth required pam_wheel.so use_uid
```

Si l’utilisateur a déjà été créé, on peut récupérer le profil manuellement comme ceci.

```
# su - microlinux
$ cp -v /etc/skel/.bash* .
« /etc/skel/.bash_logout » -> « ./.bash_logout »
« /etc/skel/.bash_profile » -> « ./.bash_profile »
« /etc/skel/.bashrc » -> « ./.bashrc »
$ source ~/.bashrc
$ exit
```


Installer un pare-feu personnalisé
----------------------------------

Depuis la version 7.0, CentOS gère le pare-feu avec `firewalld`, qui est au
pare-feu ce que NetworkManager est au réseau. Une couche d’abstraction à la
sauce Red Hat, et qui ne nous servira pas à grand-chose. On préférera une
configuration traditionnelle avec `iptables`.

```
# systemctl stop firewalld
# yum remove firewalld
```

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

Copier le script `el7/firewall/firewall-lan.sh` dans un endroit approprié et en
le renommant, par exemple `/usr/local/sbin/firewall.sh`. Adapter le script à la
configuration réseau de la machine et aux services que l’on compte héberger
dessus.

```
# firewall.sh
```

Afficher la configuration du pare-feu.

```
# iptables -L -v -n
```

Au redémarrage du serveur, les règles `iptables` sont bien restaurées, mais le
relais des paquets est désactivé. Pour l’activer par défaut, on peut créer un
fichier `/etc/sysctl.d/enable-ip-forwarding.conf` comme ceci.

```
# /etc/sysctl.d/enable-ip-forwarding.conf 
# Enable IP forwarding
net.ipv4.ip_forward = 1
```


Vérifier les alertes SELinux
----------------------------

Pour finir, on pourra vérifier le bon fonctionnement du système de base avec
SELinux.

```
# sealert -a /var/log/audit/audit.log
100% done
found 0 alerts in /var/log/audit/audit.log
```



