Synchroniser un réseau local avec NTP
=====================================

Document écrit par Nicolas Kovacs <info@microlinux.fr>

Lorsque plusieurs personnes manipulent des données partagées sur des postes
clients différents, il est essentiel que ces postes soient tous à la même
heure. Malheureusement, l'horloge intégrée dans les machines n'est pas
suffisamment exacte.

Le protocole NTP (*Network Time Protocol*) permet aux machines d'un réseau de
mettre leurs pendules à l'heure. Il permet la synchronisation des machines
entre elles. Les serveurs de temps public sur Internet permettent de recevoir
le temps exact. À partir de là, on a plusieurs possibilités d'utiliser NTP.

  * La commande `ntpdate` procède à un ajustement ponctuel de l'horloge du
    BIOS.

  * L'ajustement ponctuel ne suffit pas pour un serveur qui est censé tourner
    sans discontinuer. L'horloge du serveur risque de dévier de plus en plus de
    l'heure exacte. Dans ce cas, il faudra configurer le démon `ntpd` contenu
    dans le paquet `ntp`, qui se charge de contacter les serveurs de temps
    publics à intervalles réguliers pour procéder ensuite à une série de
    corrections de l'heure locale.

  * Le démon `ntpd` peut à son tour être configuré comme serveur de temps pour
    les machines locales.

  * Dans la pratique quotidienne, on utilisera `ntpdate` pour l'ajustement
    initial de l'heure locale, et `ntpd` pour la synchronisation régulière.


Prérequis
---------

Le démon `ntpd` utilise le port 123 en UDP. Il faut donc ouvrir ce port sur le
serveur pour permettre aux postes clients de se connecter.


Installation
------------

Voici les paquets relatifs à NTP.

<pre>
# <strong>rpm -qa | grep ntp</strong> 
ntp-4.2.6p5-19.el7.centos.x86_64
ntpdate-4.2.6p5-19.el7.centos.x86_64
</pre>

Si ce n'est pas déjà fait, supprimer le serveur Chrony installé par défaut pour
éviter les conflits.

<pre>
# <strong>systemctl stop chronyd</strong> 
# <strong>yum remove chrony</strong> 
</pre>

Basculer SELinux en mode permissif.

<pre>
# <strong>setenforce 0</strong> 
</pre>


Synchronisation avec un serveur NTP public
------------------------------------------

Éventuellement, aller sur http://www.pool.ntp.org et choisir la liste des
serveurs en fonction du pays.

Sauvegarder le fichier de configuration existant.

<pre>
# <strong>cd /etc</strong> 
# <strong>mv ntp.conf ntp.conf.orig</strong> 
</pre>

Configurer le service.

<pre>
# /etc/ntp.conf

driftfile /var/lib/ntp/drift
logfile /var/log/ntp.log

server 0.fr.pool.ntp.org
server 1.fr.pool.ntp.org
server 2.fr.pool.ntp.org
server 3.fr.pool.ntp.org

server 127.127.1.0
fudge 127.127.1.0 stratum 10

restrict default nomodify nopeer notrap
restrict 127.0.0.1 mask 255.0.0.0
</pre>

Quelques remarques :

  * Le fichier journal `/var/log/ntp.log` sera créé à la volée au démarrage du
    service. Ce n'est donc pas la peine de le créer manuellement.

  * La directive `fudge 127.127.1.10 stratum 10` constitue un serveur "bidon"
    en guise d'IP *fallback*, au cas où la source de temps extérieure
    deviendrait momentanément indisponible. En cas d'indisponibilité du serveur
    distant, NTP continuera à tourner en se basant sur ce fonctionnement-là.

  * NTP offre une panoplie de règles pour contrôler l'accès au service, que
    l'on pourra utiliser en-dehors des règles de pare-feu. Ici, les directives
    `restrict` signifient qu'on empêche les machines distantes de modifier la
    configuration du serveur (première ligne) et qu'on fait confiance à la
    machine elle-même (deuxième ligne). La directive `restrict` sans option
    derrière, mais suivie du seul nom d'hôte, équivaut à un `allow all`.


Gestion et utilisation
----------------------

Éventuellement, effectuer l'ajustement initial de l'horloge.

<pre>
# <strong>ntpdate fr.pool.ntp.org</strong> 
17 Feb 07:04:05 ntpdate[1943]: adjust time server 212.83.131.33 
  offset 0.010439 sec
</pre>

La commande `ntpdate` est normalement considérée comme obsolète, mais elle sert
toujours à effectuer des corrections importantes. Théoriquement, c'est la
commande `ntpd -g` qui est censée remplacer `ntpdate`, mais son utilisation
s'avère problématique sur des systèmes déréglés de plus d'une heure.

Activer le service.

<pre>
# <strong>systemctl enable ntpd</strong> 
</pre>

Gérer le service :

<pre>
# <strong>systemctl start|stop|restart|status ntpd</strong> 
</pre>

Afficher la liste des serveurs auxquels on est connecté.

<pre>
# <strong>ntpq -p</strong> 
   remote     refid     st t when poll reach delay offset jitter
=================================================================
+tidore.ordimati 91.121.122.16  3 u 36 64 1  31.353 -2.014 0.000
+176.31.53.204   193.190.230.65 2 u 35 64 1 114.250 36.854 0.000
*ntp-2.arkena.ne 193.190.230.65 2 u 34 64 1  30.266 -2.454 0.000
 fr3.tomhek.net  195.154.216.35 3 u 33 64 1  34.424 -3.169 0.000
 LOCAL(0)        .LOCL.        10 l  - 64 0   0.000  0.000 0.000
</pre>

  * Le petit astérisque `*` en début de ligne signifie que la machine est
    correctement synchronisée avec le serveur distant. La première
    synchronisation peut nécessiter quelques minutes, parfois même une
    demi-heure.

  * Pour guetter la première synchronisation, on peut invoquer `watch ntpq -p`.


NTP et SELinux
--------------

Rien de particulier à signaler avec notre configuration.

<pre>
# <strong>sealert -a /var/log/audit/audit.log</strong> 
100% done
found 0 alerts in /var/log/audit/audit.log
</pre>

Rebasculer en mode strict.

<pre>
# <strong>setenforce 1</strong> 
</pre>

