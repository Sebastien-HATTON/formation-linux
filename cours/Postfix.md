Serveur mail avec Postfix
=========================

Document écrit par Nicolas Kovacs <info@microlinux.fr>


Le MTA Postfix
--------------

Postfix est un serveur mail, et plus exactement un MTA (*Mail Transfer Agent*).
Il gère l'envoi et la réception de mails par Internet en utilisant le protocole
SMTP.

Le monde de l'Open Source offre toute une panoplie de MTA, parmi lesquels on
trouve Postfix, Exim, Qmail et Sendmail. 


Prérequis
---------

Dans le pare-feu, il faudra ouvrir le port 25 en TCP.

Vérifier si le serveur n'est pas blacklisté quelque part.

  * [http://www.mxtoolbox.com/blacklists.aspx](http://www.mxtoolbox.com/blacklists.aspx)

Il faut impérativement disposer d'un ou de plusieurs noms de domaines
enregistrés et valides. 

  * `slackbox.fr`

  * `unixbox.fr`

  * etc.

Sur une machine externe, vérifier la configuration DNS des domaines pour
lesquels on souhaite gérer le courrier, comme ceci.

```
$ host -t MX slackbox.fr
slackbox.fr mail is handled by 10 mail.slackbox.fr.
$ host mail.slackbox.fr
mail.slackbox.fr has address 195.154.65.130
$ host slackbox.fr
slackbox.fr has address 195.154.65.130
slackbox.fr mail is handled by 10 mail.slackbox.fr.
$ host 195.154.65.130
130.65.154.195.in-addr.arpa domain name pointer sd-41893.dedibox.fr.
```


Installation
------------

Postfix est inclus dans une installation minimale de CentOS. S'il n'est pas
présent sur le système, on peut l'installer comme ceci.

```
# yum install postfix mailx

```

Le paquet `mailx` fournit la commande `/bin/mail` qui nous sera utile pour
tester l'envoi de mails.


Configuration initiale
----------------------

Les fichiers de configuration utilisés par Postfix se situent dans
`/etc/postfix`.

  * Le fichier `master.cf` gère la configuration du démon `master` de Postfix.
    Dans la plupart des configurations de base, on n'aura pas à intervenir sur
    ce fichier.

  * Le fichier `main.cf` contient les paramètres de contrôle des démons de
    Postfix. C'est celui que l'on modifiera le plus souvent.

Le fichier `main.cf` fourni par défaut fait près de 680 lignes, la plupart
étant des commentaires. On peut commencer par aérer ce fichier pour ne garder
que les directives.

```
# cd /etc/postfix
# mv main.cf main.cf.orig
# grep -h -v '^[[:space:]]*\#' main.cf.orig | grep -v '^[[:space:]]*$' > main.cf
```

On obtient un fichier beaucoup plus lisible.

```
# /etc/postfix/main.cf
queue_directory = /var/spool/postfix
command_directory = /usr/sbin
daemon_directory = /usr/libexec/postfix
data_directory = /var/lib/postfix
mail_owner = postfix
inet_interfaces = localhost
inet_protocols = all
mydestination = $myhostname, localhost.$mydomain, localhost
unknown_local_recipient_reject_code = 550
alias_maps = hash:/etc/aliases
alias_database = hash:/etc/aliases
debug_peer_level = 2
debugger_command =
   PATH=/bin:/usr/bin:/usr/local/bin:/usr/X11R6/bin
   ddd $daemon_directory/$process_name $process_id & sleep 5
sendmail_path = /usr/sbin/sendmail.postfix
newaliases_path = /usr/bin/newaliases.postfix
mailq_path = /usr/bin/mailq.postfix
setgid_group = postdrop
html_directory = no
manpage_directory = /usr/share/man
sample_directory = /usr/share/doc/postfix-2.10.1/samples
readme_directory = /usr/share/doc/postfix-2.10.1/README_FILES
```

Si un paramètre n'est pas présent dans `main.cf`, Postfix utilisera sa valeur
par défaut. Pour la plupart, ces valeurs sont définies "en dur" dans le code
source de Postfix, tandis que certaines sont initialisées à la compilation et
quelques-unes au moment du lancement du programme.

Le programme `postconf` est très utile pour examiner les valeurs courantes et
par défaut du fichier `main.cf`. Pour afficher la valeurs de certains
paramètres de configuration, il suffit de les fournir en argument. 

```
# postconf inet_interfaces
inet_interfaces = localhost
```

L'option `-d` affichera la valeur par défaut des paramètres demandés.

```
# postconf -d inet_interfaces
inet_interfaces = all
```

Nous allons supprimer la plupart des paramètres redondants ou autrement
inutiles, pour commencer avec quelques directives de base.

```
# /etc/postfix/main.cf

# Désactiver l'IPv6
inet_protocols = ipv4

# Identification
smtpd_banner = $myhostname ESMTP $mail_name (CentOS)

# Nom d'hôte pleinement qualifié du serveur
myhostname = sd-41893.dedibox.fr

# Domaine du serveur
mydomain = dedibox.fr

# Domaine pour qualifier les adresses sans partie domaine
myorigin = $myhostname

# Domaines locaux
mydestination = $myhostname, localhost.$mydomain, localhost

# Envoi de mails sans authentification
mynetworks = 127.0.0.0/8

# Tables de correspondance
alias_maps = hash:/etc/aliases
alias_database = hash:/etc/aliases

# Commande de débogage
debugger_command =
         PATH=/bin:/usr/bin:/usr/local/bin:/usr/X11R6/bin
         ddd $daemon_directory/$process_name $process_id & sleep 5

# Chemins des commandes
sendmail_path = /usr/sbin/sendmail.postfix
newaliases_path = /usr/bin/newaliases.postfix
mailq_path = /usr/bin/mailq.postfix

# Documentation
manpage_directory = /usr/share/man
sample_directory = /usr/share/doc/postfix-2.10.1/samples
readme_directory = /usr/share/doc/postfix-2.10.1/README_FILES
```

Quelques remarques :

  * Si l'IPv6 est désactivé au niveau du système, il faudra également le faire
    ici grâce à la directive `inet_protocols`.

  * `smtpd_banner` définit la chaîne de caractères avec laquelle Postfix
    s'identifie auprès d'un autre MTA.

  * `myhostname` est censé contenir le nom d'hôte pleinement qualifié du
    serveur, c'est-à-dire le résultat de la commande `hostname --fqdn`.

  * `myorigin` définit le domaine auquel sont associés des mails envoyés
    localement. Par défaut, `myorigin` a la même valeur que `myhostname`.

  * `mydestination` fournit la liste des domaines pour lesquels les messages
    reçus doivent être stockés dans une boîte mail locale. Même si Postfix gère
    plusieurs domaines, `mydestination` ne doit spécifier que le domaine
    principal. Les domaines virtuels seront gérés par la directive
    `virtual_alias_domains`, que nous verrons plus loin.

  * `mynetworks` définit les adresses depuis lesquelles Postfix accepte les
    mails sans authentification via SMTP. Les plages d'adresses fournies ici
    désignent donc toutes les machines auxquelles Postfix fait confiance, si
    l'on peut dire. Sur un serveur dédié public, il est impératif de définir
    uniquement l'hôte local pour `mynetworks`, sous peine de se retrouver avec
    une "pompe à merde", le terme communément utilisé pour les serveurs mails
    mal configurés qui sont utilisés par des tiers malintentionnés pour l'envoi
    massif de spams sans authentification. Les spammeurs du monde entier
    adorent ce genre de machines.

  * `alias_maps` définit l'emplacement de la table de correspondance, et
    `alias_database` la base de données correspondante. Certaines informations
    ne peuvent pas être facilement représentées dans `main.cf`. Les tables de
    correspondance permettent de les stocker dans des fichiers externes.
    Postfix n'utilise pas directement les fichiers texte, ce serait trop lent.
    Au lieu de cela, les tables de correspondance de type *hash* (ou "tables de
    hachage) servent pour construire des fichiers indexés, grâce à la
    bibliothèque Berkeley DB. Le programme `postmap` est utilisé pour
    construire les fichiers indexés. Pour mettre à jour les alias, on utilisera
    la commande `newaliases`.
