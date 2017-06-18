Serveur de bases de données MySQL
=================================

Document écrit par Nicolas Kovacs <info@microlinux.fr>

MySQL est le système de bases de données le plus populaire du monde de
l'Open Source. Un grand nombre d'applications web (wikis, moteurs de blogs et
de forums, systèmes de gestion de contenu, etc.) utilisent MySQL comme moteur
de bases de données.

On entend parfois dire que MySQL n'est pas un "vrai" système de bases de
données en comparaison à des systèmes comme Oracle ou DB/2. Contentons-nous de
savoir que MySQL est utilisé, entre autres, par Facebook, Google et Yahoo!

CentOS 7 a remplacé MySQL par le fork communautaire MariaDB, suite au rachat de
MySQL par Sun Microsystems et Oracle. La gouvernance du projet MariaDB est
assurée par la fondation MariaDB. Elle confère au logiciel l'assurance de
rester libre.


Installation et configuration
-----------------------------

Installer MySQL/MariaDB.

<pre>
# <strong>yum install mariadb-server mariadb</strong> 
</pre>

Le client `mariadb` est automatiquement installé par le paquet
`mariadb-server`.

Activer et démarrer le service.

<pre>
# <strong>systemctl enable mariadb</strong> 
# <strong>systemctl start mariadb</strong> 
</pre>


Sécuriser le serveur MySQL
--------------------------

MySQL/MariaDB dispose de l'utilitaire `mysql_secure_installation` pour assurer
la sécurité d'une installation fraîche sur une machine de production. Ce
programme permet d'effectuer quelques démarches de sécurisation essentielles.

  1. Définir un mot de passe `root` MySQL (ne pas confondre avec le compte
     `root` Linux).

  2. Supprimer les comptes `root` MySQL accessibles de l'extérieur.

  3. Supprimer les connexions anonymes.

  4. Supprimer la base de données de `test`.

Lancer la sécurisation.

<pre>
# <strong>mysql_secure_installation</strong> 

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL 
      MariaDB SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP 
      CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none): <strong>[Entrée]</strong> 
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the 
MariaDB root user without the proper authorisation.

Set root password? [Y/n] <strong>y</strong> 
New password: ******
Re-enter new password: ******
Password updated successfully!
Reloading privilege tables..
 ... Success!

By default, a MariaDB installation has an anonymous user, allowing 
anyone to log into MariaDB without having to have a user account 
created for them.  This is intended only for testing, and to make 
the installation go a bit smoother.  You should remove them before 
moving into a production environment.

Remove anonymous users? [Y/n] <strong>y</strong> 
 ... Success!

Normally, root should only be allowed to connect from 'localhost'. 
This ensures that someone cannot guess at the root password from 
the network.

Disallow root login remotely? [Y/n] <strong>y</strong> 
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone 
can access.  This is also intended only for testing, and should be 
removed before moving into a production environment.

Remove test database and access to it? [Y/n] <strong>y</strong> 
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so 
far will take effect immediately.

Reload privilege tables now? [Y/n] <strong>y</strong> 
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!</pre>

Dorénavant, il faut se connecter au moniteur MySQL/MariaDB avec le mot de passe
que l'on vient de définir un peu plus haut.

<pre>
$ <strong>mysql -u root -p</strong> 
Enter password:
Welcome to the MariaDB monitor.
...
MariaDB [(none)]>
</pre>

Afficher les bases de données.

<pre>
MariaDB [(none)]> <strong>show databases;</strong> 
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
3 rows in set (0.00 sec)
</pre>

Utiliser la base de données `mysql`.

<pre>
MariaDB [(none)]> <strong>use mysql;</strong> 
Database changed
</pre>

Afficher les utilisateurs.

<pre>
MariaDB [mysql]> <strong>select user, host, password from user;</strong> 
+------+-----------+-------------------------------------------+
| user | host      | password                                  |
+------+-----------+-------------------------------------------+
| root | localhost | *6883419C147A759B04D78A2D1E4E0C5BB0CDD1B4 |
| root | 127.0.0.1 | *6883419C147A759B04D78A2D1E4E0C5BB0CDD1B4 |
| root | ::1       | *6883419C147A759B04D78A2D1E4E0C5BB0CDD1B4 |
+------+-----------+-------------------------------------------+
3 rows in set (0.00 sec)
</pre>

On va garder la seule entrée pour `root@localhost` et supprimer les deux
autres.

<pre>
MariaDB [mysql]> <strong>delete from user where host!='localhost';</strong> 
Query OK, 2 rows affected (0.00 sec)
</pre>

Vérifier le résultat de l'opération.

<pre>
MariaDB [mysql]> <strong>select user, host, password from user;</strong> 
+------+-----------+-------------------------------------------+
| user | host      | password                                  |
+------+-----------+-------------------------------------------+
| root | localhost | *6883419C147A759B04D78A2D1E4E0C5BB0CDD1B4 |
+------+-----------+-------------------------------------------+
1 row in set (0.00 sec)
</pre>

Quitter la console.

<pre>
MariaDB [mysql]> <strong>quit;</strong> 
Bye
</pre>


Exemple de base de données
--------------------------

Se connecter au moniteur MySQL.

<pre>
$ <strong>mysql -u root -p</strong> 
mysql>
</pre>

Créer une base de données `webapp`.

<pre>
mysql> <strong>create database webapp;</strong> 
</pre>

Afficher les bases de données.

<pre>
mysql> <strong>show databases;</strong> 
</pre>

Créer un utilisateur `webappuser` qui aura tous les droits sur `webapp`.

<pre>
mysql> <strong>grant all on webapp.* to webappuser@localhost</strong> 
   - > <strong>identified by 'motdepasse';</strong> 
</pre>

Attention, le mot de passe apparaît en clair dans le moniteur MySQL.

<pre>
mysql> <strong>quit;</strong> 
</pre>

Si l'on souhaite supprimer cette base de données.

<pre>
mysql> <strong>drop database webapp;</strong> 
</pre>


Sauvegarde
----------

La commande `mysqldump` permet de sauvegarder une base de données MySQL sous
forme d'une série d'instructions SQL.

L'exemple suivant effectue une sauvegarde complète de la base de données
`cmsms`.

<pre>
$ <strong>mysqldump -u root -p cmsms > sauvegarde_cmsms.sql</strong> 
</pre>

L'option `--all-databases` permet de sauvegarder l'ensemble des bases de
données d'un serveur.

<pre>
$ <strong>mysqldump -u root -p --all-databases > sauvegarde.sql</strong> 
</pre>

Les fichiers SQL résultants sont parfois très volumineux, étant donné qu'il
s'agit d'un format texte simple. Dans ce cas, on peut les compresser durant la
sauvegarde même. Voici ce que cela donne pour les deux exemples précédents.

<pre>
$ <strong>mysqldump -u root -p cmsms | gzip -c > sauvegarde_cmsms.sql.gz</strong> 
$ <strong>mysqldump -u root -p --all-databases | gzip -c > sauvegarde.sql.gz</strong> 
</pre>


Restauration
------------

Dans l'exemple suivant, on va restaurer la base de données `cmsms` depuis le
fichier de sauvegarde créé ci-dessus.

<pre>
$ <strong>mysqladmin -u root -p create cmsms</strong> 
$ <strong>mysql -u root -p cmsms < sauvegarde_cmsms.sql</strong> 
</pre>

Pour restaurer "à la louche" une sauvegarde globale, il suffit d'invoquer la
commande suivante qui gère également la création des bases de données.

<pre>
$ <strong>mysql -u root -p < sauvegarde.sql</strong> 
</pre>

La restauration à partir d'une sauvegarde compressée peut s'effectuer comme
ceci.

<pre>
$ <strong>mysqladmin -u root -p create cmsms</strong> 
$ <strong>gunzip -c sauvegarde_cmsms.sql.gz | mysql -u root -p cmsms</strong> 
</pre>

Et pour une sauvegarde globale.

<pre>
$ <strong>gunzip -c sauvegarde.sql.gz | mysql -u root -p</strong> 
</pre>

Si l'on obtient des erreurs de connexion aux bases restaurées, il faudra se
connecter au moniteur MySQL, puis...

<pre>
mysql> <strong>flush privileges;</strong> 
</pre>


Sauvegardes automatiques
------------------------

Mon dépôt Github `centos` fournit un script Bash `sqldump.sh` dans le
répertoire `el7/backup`. Ce script permet d'effectuer des sauvegardes
automatiques de l'ensemble des bases MySQL d'un serveur, individuellement et "à
la louche".  Les sauvegardes se présentent sous forme d'une série de fichiers
compressés `*.sql.gz` rangés dans le répertoire `/sqldump`. Par la suite, ces
fichiers pourront être récupérés par un serveur de sauvegardes.

<pre>
# <strong>cd</strong> 
# <strong>git clone https://github.com/kikinovak/centos</strong> 
# <strong>cd centos/el7/backup/</strong> 
# <strong>cp sqldump.sh /usr/local/sbin/</strong> 
# <strong>chmod 0700 /usr/local/sbin/sqldump.sh</strong> 
# <strong>vim /usr/local/sbin/sqldump.sh</strong> 
</pre>

Une fois qu'on a édité le script à sa convenance, on peut définir une tâche automatique.

<pre>
# <strong>crontab -e</strong> 
# Backup MySQL databases
05 10 * * * /usr/local/sbin/sqldump.sh 1> /dev/null
</pre>

Dans l'exemple, on définit une sauvegarde quotidienne à 10h05.

