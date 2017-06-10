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
# yum install postfix
```

On installera également la commande `mail` (paquet `mailx`) et le client `mutt`
pour pouvoir tester et gérer les mails en ligne de commande directement sur le
serveur.

```
# yum install mailx mutt
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

# Relais
relayhost =

# Format de stockage
home_mailbox = Maildir/

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

  * `relayhost` définit le MTA auquel on est censé transférer les mails qui ne
    doivent pas être acheminés localement. Dans notre configuration, cette
    directive doit rester vide. On l'utilisera sur un serveur de réseau local
    pour transférer les mails à un MTA public sur Internet.

  * Le format de stockage par défaut de Postfix, c'est `mbox`. On préférera le
    format `Maildir/`, bien plus adapté pour une configuration IMAP.

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

Éditer la table de correspondance.

```
# /etc/postfix/aliases
postmaster: root
root      : microlinux
```

Construire le fichier indexé.

```
# newaliases
```

Premier test
------------

Activer et démarrer Postfix.

```
# systemctl enable postfix
# systemctl start postfix
```

Vérifier si Postfix tourne correctement.

```
# systemctl status postfix
● postfix.service - Postfix Mail Transport Agent
   Loaded: loaded (/usr/lib/systemd/system/postfix.service; enabled; vendor preset: disabled)
   Active: active (running) since sam. 2017-06-10 12:51:19 CEST; 47s ago
   ...
# cat /var/log/maillog
... sd-41893 postfix/postfix-script[10097]: starting the Postfix mail system
... sd-41893 postfix/master[10099]: daemon started -- version 2.10.1, configuration /etc/postfix
```

Basculer vers un compte utilisateur normal (`microlinux` dans l'exemple) et
envoyer un mail vers un compte Webmail externe. Un point `.` sur une ligne à
part marque la fin du message.

```
# su - microlinux
Dernière connexion : samedi 10 juin 2017 à 10:18:37 CEST sur pts/0
$ mail info@microlinux.fr
Subject: Test Postfix
Ceci est un test.
.
EOT
```

Se connecter au compte Webmail et vérifier si le message a bien été envoyé,
puis répondre à ce message. Si tout se passe bien, le répertoire utilisateur
contient un nouveau répertoire `~/Maildir`, qui ressemble à ceci.

```
$ tree Maildir/
Maildir/
├── cur
├── new
│   └── 1497093622.V802I1480009M893901.sd-41893.dedibox.fr
└── tmp

3 directories, 1 file
```

Le nouveau mail est un simple fichier texte, que l'on peut afficher avec `less`
par exemple.

```
$ less Maildir/new/1497093622.V802I1480009M893901.sd-41893.dedibox.fr
Return-Path: <info@microlinux.fr> 
X-Original-To: microlinux@sd-41893.dedibox.fr
Delivered-To: microlinux@sd-41893.dedibox.fr
Received: from smtp.nfrance.com (smtp-6.nfrance.com [80.247.225.6])
        by sd-41893.dedibox.fr (Postfix) with ESMTP id C59246405C8
        for <microlinux@sd-41893.dedibox.fr>; Sat, 10 Jun 2017 13:20:22 +0200 (CEST)
Received: from [192.168.2.2] (nikikovacs.pck.nerim.net [62.212.104.80])
        (using TLSv1.2 with cipher ECDHE-RSA-AES256-GCM-SHA384 (256/256 bits))
        (No client certificate requested)
        by smtp.nfrance.com (Postfix) with ESMTPSA id 2B1A5112818
        for <microlinux@sd-41893.dedibox.fr>; Sat, 10 Jun 2017 13:20:22 +0200 (CEST)
Reply-To: info@microlinux.fr
Subject: Re: Test Postfix
To: microlinux <microlinux@sd-41893.dedibox.fr>
References: <20170610111955.15A726407E1@sd-41893.dedibox.fr>
From: Nicolas Kovacs <info@microlinux.fr>
Organization: Microlinux
Message-ID: <a7153484-3cec-d2d0-5ec7-e1543f788646@microlinux.fr>
Date: Sat, 10 Jun 2017 13:20:21 +0200
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:52.0) Gecko/20100101
 Thunderbird/52.1.0
MIME-Version: 1.0
In-Reply-To: <20170610111955.15A726407E1@sd-41893.dedibox.fr>
Content-Type: text/plain; charset=utf-8
Content-Language: en-US
Content-Transfer-Encoding: 8bit
X-Scanned-By: MIMEDefang 2.78 on 80.247.225.6

Le 10/06/2017 à 13:19, microlinux a écrit :
> Ceci est un test.
> 

Et voici la réponse.

-- 
Microlinux - Solutions informatiques durables
7, place de l'église - 30730 Montpezat
Web  : http://www.microlinux.fr
Mail : info@microlinux.fr
Tél. : 04 66 63 10 32
```

Gérer les mails en ligne de commande avec Mutt
----------------------------------------------

Mutt est un MUA (*Mail User Agent*) en ligne de commande. On peut l'utiliser
sur des machines dépourvues d'interface graphique.

Avant de lancer Mutt, éditer le fichier de configuration `~/.muttrc`.

```
# ~/.muttrc 
set mbox_type=Maildir
set folder="~/Maildir"
set spoolfile="~/Maildir"
set mbox="+Mailbox"
my_hdr From: microlinux@sd-41893.dedibox.fr (Microlinux)
my_hdr Reply-To: microlinux@sd-41893.dedibox.fr (Microlinux)
```

Lancer Mutt :

```
$ mutt
```

La fenêtre principale de Mutt affiche la boite de réception. Les nouveaux mails
sont marqués par un `N`. Une barre d'état en haut de l'écran affiche les
principaux raccourcis. En règle générale, Mutt fonctionne avec les mêmes
raccourcis que Vim. Pour lire un message, il suffit de le sélectionner et
d'appuyer sur *Entrée*.






