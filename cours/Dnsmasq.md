Gérer le DNS et le DHCP avec Dnsmasq
====================================

Document écrit par Nicolas Kovacs <info@microlinux.fr>

Dnsmasq est un serveur léger qui fournit les services DHCP et DNS pour des
réseaux locaux, même de taille importante. Il est extrêmement facile à
configurer, et l’on pourra l’utiliser pour remplacer DHCPD et Bind. Ce dernier
n’est pas très adapté pour les DNS de réseaux locaux, notamment à cause de sa
syntaxe farfelue.


Prérequis
---------

Ouvrir les ports suivants dans le pare-feu.

  * 53 en TCP et UDP (requêtes DNS)

  * 67 et 68 en UDP (requêtes DHCP)

Le fichier `/etc/hosts` du serveur local doit comporter au moins les deux
lignes suivantes.

<pre>
# /etc/hosts
127.0.0.1     localhost.localdomain localhost
192.168.3.1   amandine.sandbox.lan  amandine
</pre>

C’est surtout la deuxième ligne qui est d’une importance capitale. Elle fait
correspondre le nom de la machine locale avec l’adresse IP dans le réseau
local.


Installation
------------

Vérifier si Dnsmasq est installé :

<pre>
# <strong>rpm -qa | grep dnsmasq</strong> 
dnsmasq-utils-2.66-12.el7.x86_64
dnsmasq-2.66-12.el7.x86_64
</pre>


Configuration de base
---------------------

La configuration de Dnsmasq s’effectue par le biais du fichier de configuration
`/etc/dnsmasq.conf`. Le fichier fourni par défaut comporte près de 600 lignes de
commentaires et sert également de documentation. On pourrait très bien activer
l’une ou l’autre option en la décommentant. Dans le cas présent, il vaut mieux
effectuer une copie de sauvegarde et commencer par un fichier vide.

<pre>
# <strong>cd /etc</strong> 
# <strong>mv dnsmasq.conf dnsmasq.conf.orig</strong> 
# <strong>touch dnsmasq.conf</strong> 
</pre>

Éditer une configuration minimale, par exemple :

<pre>
# /etc/dnsmasq.conf
domain-needed
bogus-priv
interface=enp3s1                             
dhcp-range=192.168.3.100,192.168.3.200,24h
local=/sandbox.lan/
domain=sandbox.lan
expand-hosts
server=192.168.2.1
</pre>

  * Les deux premières options `domain-needed` et `bogus-priv` évitent que
    Dnsmasq ne relaie les noms d’hôtes locaux à un ou plusieurs serveurs DNS en
    amont.

  * L’option `interface` spécifie l’interface réseau que l’on souhaite
    utiliser.

  * L’option `dhcp-range` définit la plage d’adresses dynamiques utilisée parle
    serveur DHCP. Dans le cas présent, les adresses attribuées auront une durée
    de validité de 24 heures. Passé ce délai, elles devront être renouvelées
    par les clients.

  * L’option `local` indique que les réponses aux requêtes pour le domaine
    spécifié doivent être fournies directement par Dnsmasq, et non pas par un
    serveur DNS en amont.

  * L’option `domain` attribue le nom de domaine spécifié aux clients. Pour des
    raisons évidentes, il doit coïncider avec le domaine défini dans l’option
    `local`.

  * L’option `expand-hosts` concerne les requêtes DNS sans le domaine et se
    charge d’ajouter celui-ci automatiquement. Concrètement, lorsqu’on essaie
    d’envoyer un `ping` sur `bernadette`, Dnsmasq retournera automatiquement
    l’adresse IP de `bernadette.sandbox.lan`.

  * L’option `server` spécifie l’adresse IP d’un ou plusieurs serveurs DNS en
    amont.


Démarrage et utilisation
------------------------

Activer Dnsmasq.

<pre>
# <strong>systemctl enable dnsmasq</strong> 
</pre>

Gérer le lancement et l’arrêt :

<pre>
# <strong>systemctl start|stop|restart dnsmasq</strong>
</pre>


Attribuer des adresses statiques
--------------------------------

On pourra attribuer une adresse IP et un nom d’hôte fixe en fonction de
l’adresse MAC des interfaces réseau respectives, en ajoutant une série
d’entrées comme ceci.

<pre>
# /etc/dnsmasq.conf
...
dhcp-host=00:1E:C9:43:A7:BF,bernadette,192.168.3.2
dhcp-host=00:1D:09:15:4A:D8,raymonde,192.168.3.3
...
</pre>

On choisira les adresses IP en-dehors de la plage d’adresses dynamiques.

Si l’on souhaite attribuer une adresse IP et un nom d’hôte fixe à un portable
que l’on connecte aussi bien par le wifi que par une connexion filaire, on peut
utiliser la syntaxe suivante.

<pre>
# /etc/dnsmasq.conf
...
dhcp-host=44:1E:A1:E6:FA:93,E4:D5:3D:BD:EA:05,buzz,192.168.3.6
dhcp-host=00:27:19:F1:BC:3A,00:19:E0:83:3A:C1,bebette,192.168.3.7
...
</pre>


Ajouter des hôtes statiques
---------------------------

L’ajout d’hôtes statiques est extrêmement simple avec Dnsmasq. Il suffit
d’ajouter l’entrée correspondante dans le fichier `/etc/hosts` du serveur, et
celui-ci se chargera de relayer l’info.

<pre>
# /etc/hosts 
...
192.168.3.254   wifi
...
</pre>

Relancer Dnsmasq pour prendre en compte les modifications.

<pre>
# <strong>systemctl restart dnsmasq</strong> 
</pre>

On peut également ajouter un raccourci pour une adresse IP externe.

<pre>
# /etc/hosts
...
88.191.189.120  dedibox
...
</pre>

Si le serveur héberge une série de sites web sous formes d’hôtes virtuels, on
peut ajouter les entrées correspondantes comme ceci.

<pre>
# /etc/hosts 
...
192.168.3.1   mirror.amandine.sandbox.lan mirror.amandine
192.168.3.1   cmsms.amandine.sandbox.lan cmsms.amandine
...
</pre>


Résolution des noms d’hôtes
---------------------------

Les postes clients sur le réseau utilisent les informations sur les noms
d’hôtes fournies par Dnsmasq. Pour que le serveur lui-même puisse les prendre
en compte aussi, il faudra éditer `/etc/resolv.conf` comme ceci.

<pre>
# /etc/resolv.conf
nameserver 127.0.0.1
</pre>

Vérifions.

<pre>
[root@amandine:~] # <strong>host bernadette</strong> 
bernadette has address 192.168.3.2
[root@amandine:~] # <strong>host raymonde</strong> 
raymonde has address 192.168.3.3
</pre>


Afficher en direct l’attribution des baux DHCP
----------------------------------------------

Sur le serveur, on peut suivre en direct l’attribution des baux DHCP en
affichant en continu le journal `/var/log/messages`.

<pre>
# <strong>tail -f /var/log/messages</strong> 
... DHCPREQUEST(enp3s1) 192.168.3.2 00:1e:c9:43:a7:bf
... DHCPACK(enp3s1) 192.168.3.2 00:1e:c9:43:a7:bf bernadette
... DHCPREQUEST(enp3s1) 192.168.3.3 00:1d:09:15:4a:d8
... DHCPACK(enp3s1) 192.168.3.3 00:1d:09:15:4a:d8 raymonde
</pre>

