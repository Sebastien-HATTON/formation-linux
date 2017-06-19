Organiser les hôtes virtuels Apache
===================================

Document écrit par Nicolas Kovacs <info@microlinux.fr>

<img align="right" src="../images/apache-vhosts.jpg">

Comme la plupart des distributions Linux, Red Hat et CentOS fournissent un
répertoire comprenant une série de fichiers de configuration modulaires pour
Apache, en l’occurrence `/etc/httpd/conf.d`. Ces fichiers sont pris en compte
grâce à la directive `Include` qui figure dans le fichier de configuration
principal `/etc/httpd/conf/httpd.conf`.

<pre>
# Load config files in the "/etc/httpd/conf.d" directory, if any.
IncludeOptional conf.d/*.conf
</pre>

Le répertoire `/etc/httpd/conf.d` contient un fichier `README` qui nous
renseigne un peu plus sur le fonctionnement particulier de cette configuration
modulaire.

<pre>
This directory holds configuration files for the Apache HTTP Server;
any files in this directory which have the ".conf" extension will be
processed as httpd configuration files.  The directory is used in
addition to the directory /etc/httpd/conf.modules.d/, which contains
configuration files necessary to load modules.

Files are processed in alphabetical order.
</pre>

L’info à retenir ici, c’est que les bouts de fichiers de configuration pour
Apache sont traités par ordre alphabétique. Partant de là, on peut répartir la
configuration des hôtes virtuels sur plusieurs fichiers pour plus de
lisibilité, avec un `VirtualHost` par fichier, et en nommant chaque fichier en
correspondance avec le nom d’hôte. Voici la solution que j’ai adoptée sur mes
serveurs.

Dans un premier temps, les fichiers contenant la configuration globale d’Apache
sont préfixés `00-*` comme ceci.

<pre>
# <strong>cd /etc/httpd/conf.d</strong> 
# <strong>ls</strong> 
autoindex.conf  php.conf  README  ssl.conf  userdir.conf  
welcome.conf
# <strong>for FILE in $(ls *.conf); do</strong> 
> <strong>mv $FILE 00-$FILE</strong> 
> <strong>done</strong> 
# <strong>ls</strong> 
00-autoindex.conf  00-php.conf  00-ssl.conf  00-userdir.conf  
00-welcome.conf README
</pre>

Ensuite, j’ai deux hôtes virtuels qui pointent vers la page par défaut, un pour
le HTTP, l’autre pour le HTTPS. Les deux sont préfixés `10-*` comme ceci.

  * `10-default.conf`

  * `10-default-ssl.conf`

Enfin, les hébergements à proprement parler sont tous préfixés `20-*` et nommés
en fonction du nom d’hôte. Les hébergements sécurisés portent tous le suffixe
`-ssl.conf`. Voici ce que l’on obtient au total.

<pre>
# <strong>ls -1</strong> 
00-autoindex.conf
00-php.conf
00-ssl.conf
00-userdir.conf
00-welcome.conf
10-default.conf
10-default-ssl.conf
20-freebsd.slackbox.fr.conf
20-mail.slackbox.fr.conf
20-mail.slackbox.fr-ssl.conf
20-mail.unixbox.fr.conf
20-mail.unixbox.fr-ssl.conf
20-nextcloud.slackbox.fr.conf
20-nextcloud.slackbox.fr-ssl.conf
20-nextcloud.unixbox.fr.conf
20-nextcloud.unixbox.fr-ssl.conf
20-slackware.slackbox.fr.conf
20-www.slackbox.fr.conf
20-www.unixbox.fr.conf
README
</pre>

Pour un hébergement sécurisé comme `20-mail.slackbox.fr-ssl.conf`, le fichier
`20-mail.slackbox.fr.conf` pourra contenir une redirection HTTP > HTTPS.
