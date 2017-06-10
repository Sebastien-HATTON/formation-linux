Serveur mail avec Postfix
=========================

Document écrit par Nicolas Kovacs <info@microlinux.fr>


Le MTA Postfix
--------------

Postfix est un serveur mail, et plus exactement un MTA (*Mail Transfer Agent*).
Il gère l'envoi et la réception de mail par Internet en utilisant le protocole
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

  * slackbox.fr

  * unixbox.fr

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
# yum install postfix
```

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
smtpd_banner = $myhostname ESMTP 

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



