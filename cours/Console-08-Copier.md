Copier, déplacer et renommer : cp et mv
=======================================

Document écrit par Nicolas Kovacs <info@microlinux.fr>

Copier des fichiers et répertoires avec cp
------------------------------------------

La commande `cp` (*copy*) sert à copier des fichiers. Dans son utilisation la
plus basique, `cp` duplique un fichier d'un endroit à un endroit. Prenons par
exemple un fichier de notre répertoire d'utilisateur et copions-le dans le
répertoire `/tmp`.

```
$ ls -l bonjour.txt 
-rw-rw-r--. 1 kikinovak kikinovak 19  7 juin  13:13 bonjour.txt
$ cp bonjour.txt /tmp/
$ ls -l /tmp/bonjour.txt 
-rw-rw-r--. 1 kikinovak kikinovak 19  8 juin  09:19 /tmp/bonjour.txt
```

Pour copier des répertoires entiers avec leur contenu, il faudra invoquer `cp`
avec l'option `-R` (comme *recursive*, "récursif"). Dans l'exemple, j'utilise
en plus l'option `-v` qui explicite bien chaque détail de l'opération.

```
$ tree Fichiers
Fichiers
├── Documents
│   └── texte.txt
├── Films
│   └── film.avi
└── Images
    └── photo.jpg

3 directories, 3 files
$ cp -Rv Fichiers /tmp/
« Fichiers » -> « /tmp/Fichiers »
« Fichiers/Documents » -> « /tmp/Fichiers/Documents »
« Fichiers/Documents/texte.txt » -> « /tmp/Fichiers/Documents/texte.txt »
« Fichiers/Images » -> « /tmp/Fichiers/Images »
« Fichiers/Images/photo.jpg » -> « /tmp/Fichiers/Images/photo.jpg »
« Fichiers/Films » -> « /tmp/Fichiers/Films »
« Fichiers/Films/film.avi » -> « /tmp/Fichiers/Films/film.avi »
$ tree /tmp/Fichiers
/tmp/Fichiers
├── Documents
│   └── texte.txt
├── Films
│   └── film.avi
└── Images
    └── photo.jpg

3 directories, 3 files
```

Voici maintenant une utilisation de `cp` qui peut ressembler (de loin) au
quotidien réel d'un administrateur système. Créez un fichier de configuration
`config` dans votre répertoire d'utilisateur. Effectuez-en ensuite une copie de
sauvegarde `config.orig`, qui représentera en quelque sorte l'état initial de
votre fichier de configuration. Maintenant, modifiez `config` en ajoutant une
ligne, par exemple. Vos fichiers ne sont désormais plus les mêmes. 

```
$ cat > config << EOF
> Option 1
> Option 2
> Option 3
> EOF
$ cp -v config config.orig
« config » -> « config.orig »
$ ls -l config*
-rw-rw-r--. 1 kikinovak kikinovak 27  8 juin  09:27 config
-rw-rw-r--. 1 kikinovak kikinovak 27  8 juin  09:27 config.orig
$ echo Option 4 >> config
$ ls -l config*
-rw-rw-r--. 1 kikinovak kikinovak 36  8 juin  09:27 config
-rw-rw-r--. 1 kikinovak kikinovak 27  8 juin  09:27 config.orig
```


Utiliser le joker *
-------------------

Dans ce dernier exemple, j'ai introduit une petite nouveauté qui semblera
familière aux utilisateurs de MS-DOS, même si son fonctionnement diffère
quelque peu : le joker `*`. J'évite d'utiliser les traductions françaises comme
"caractère générique" ou "métacaractère", tout juste aptes à séduire les
penseurs postmodernes. L'astérisque signifie "n'importe quelle chaîne de
caractères". Si cela ne vous paraît pas très parlant, pensez aux jeux de
cartes, où le joker signifie "n'importe quelle carte", ou encore aux petites
lettrines immaculées du Scrabble, celles qui endossent le rôle de n'importe
quelle lettre salvatrice au milieu de votre "WGTHRX". Là encore, prenons un
exemple.

Depuis que nous avons entrepris notre initiation à la ligne de commande, les
fichiers et les répertoires s'entassent dans notre répertoire d'utilisateur. La
prochaine leçon sera d'ailleurs consacrée aux commandes de suppression, ce qui
nous permettra d'envisager un brin de ménage. Pour l'instant, vous devez faire
avec tout ce fatras. Admettons que vous ne vouliez afficher des renseignements
précis que sur vos seuls fichiers dont le nom commence par `bonjour`. Vous
pourriez très bien expliciter à la suite les quatre fichiers en argument.

```
$ ls -l bonjour.txt bonjour2.txt bonjour3.txt bonjourtous.txt 
-rw-rw-r--. 1 kikinovak kikinovak 17 26 mai   10:50 bonjour2.txt
-rw-rw-r--. 1 kikinovak kikinovak 22 26 mai   10:50 bonjour3.txt
-rw-rw-r--. 1 kikinovak kikinovak 58 26 mai   10:54 bonjourtous.txt
-rw-rw-r--. 1 kikinovak kikinovak 19  7 juin  13:13 bonjour.txt
```

Toutefois, il y a moyen de faire plus court.

```
$ ls -l bonjour*
-rw-rw-r--. 1 kikinovak kikinovak 17 26 mai   10:50 bonjour2.txt
-rw-rw-r--. 1 kikinovak kikinovak 22 26 mai   10:50 bonjour3.txt
-rw-rw-r--. 1 kikinovak kikinovak 58 26 mai   10:54 bonjourtous.txt
-rw-rw-r--. 1 kikinovak kikinovak 19  7 juin  13:13 bonjour.txt
```

Les habitués de MS-DOS verront tout de suite la différence. Sous Windows, le
joker aurait dû être invoqué sous la forme `BONJOUR*.*`, voire `BONJOUR*.TXT`.


Sauvegarder un répertoire
-------------------------

Pour en revenir à `cp`, je peux également copier l'intégralité d'un répertoire
vers un répertoire d'un autre nom.

```
$ cp -Rv Fichiers CopieFichiers
« Fichiers » -> « CopieFichiers »
« Fichiers/Documents » -> « CopieFichiers/Documents »
« Fichiers/Documents/texte.txt » -> « CopieFichiers/Documents/texte.txt »
« Fichiers/Images » -> « CopieFichiers/Images »
« Fichiers/Images/photo.jpg » -> « CopieFichiers/Images/photo.jpg »
« Fichiers/Films » -> « CopieFichiers/Films »
« Fichiers/Films/film.avi » -> « CopieFichiers/Films/film.avi »
```

Admettons maintenant que je souhaite effectuer une copie complète du répertoire
`Fichiers` et de tout son contenu vers un autre endroit du système, tout en
donnant un autre nom au répertoire copié, par exemple `Sauvegarde20170608`.
Dans ce cas, voici ce qu'il faut faire.

```
$ cp -Rv Fichiers/ /tmp/Sauvegarde20170608
« Fichiers/ » -> « /tmp/Sauvegarde20170608 »
« Fichiers/Documents » -> « /tmp/Sauvegarde20170608/Documents »
« Fichiers/Documents/texte.txt » ->
« /tmp/Sauvegarde20170608/Documents/texte.txt »
« Fichiers/Images » -> « /tmp/Sauvegarde20170608/Images »
« Fichiers/Images/photo.jpg » -> « /tmp/Sauvegarde20170608/Images/photo.jpg »
« Fichiers/Films » -> « /tmp/Sauvegarde20170608/Films »
« Fichiers/Films/film.avi » -> « /tmp/Sauvegarde20170608/Films/film.avi »
```

Le seul aspect peu réaliste de ce dernier exemple, c'est que `/tmp` n'est pas
un endroit approprié pour ranger des sauvegardes. De meilleurs endroits seront
à découvrir un peu plus loin. 

Certains parmi vous auront probablement remarqué une certaine inconsistance
dans l'utilisation de `/` à la fin des noms de répertoires. Concrètement,
lorsque je copie un répertoire `Fichiers`, je peux écrire `cp Fichiers/
CopieFichiers` ou bien `cp Fichiers CopieFichiers`. La barre oblique s'ajoute
automatiquement à la fin d'un nom de répertoire lorsque j'utilise la complétion
automatique, comme nous le verrons un peu plus loin. 


Déplacer des fichiers et des répertoires avec mv
------------------------------------------------

La commande `mv` (*move* comme "bouger") sert à déplacer des fichiers.

```
$ mv bonjour.txt /tmp/
```

Cette dernière commande a déplacé le fichier `~/bonjour.txt` vers le répertoire
`/tmp`.

`mv` ne s'applique pas seulement sur des fichiers, mais également sur des
répertoires entiers. Pour essayer, créez une autre copie du répertoire
`Fichiers` et déplacez-la vers `/tmp` en utilisant `mv`.

```
$ cp -R Fichiers/ AutreCopieFichiers
$ mv AutreCopieFichiers/ /tmp/
```

Question épineuse : comment déplacer à nouveau le fichier `/tmp/bonjour.txt`
vers mon répertoire d'utilisateur lorsque je me trouve dans ce dernier ? Voici
la réponse.

```
$ mv /tmp/bonjour.txt ./
```

Et je pourrais faire de même avec `/tmp/AutreCopieFichiers`.

```
$ mv /tmp/AutreCopieFichiers/ ./
```

Vous rappelez-vous ce que nous avons dit concernant le point `.` qui signifie
"ici" ? Dans ce cas, la première des deux commandes précédentes peut se lire
littéralement comme ceci : "déplace (`mv`) le fichier `bonjour.txt` qui se
situe dans le répertoire `/tmp` vers ici (`./`)".


Renommer des fichiers et des répertoires avec mv
------------------------------------------------

La commande `mv` ne sert pas seulement à déplacer, mais aussi à renommer des
fichiers et des répertoires. Cette double utilisation tourmente habituellement
les novices de la ligne de commande sous Linux, mais ce n'est qu'une simple
habitude à prendre. 

```
$ mv bonjour.txt hello.txt
```

Là, nous venons tout simplement de renommer le fichier `bonjour.txt` en
`hello.txt`. Pour déplacer ce fichier `hello.txt` vers `/tmp` tout en le
renommant en `bonjour.txt`, c'est très simple.

```
$ mv hello.txt /tmp/bonjour.txt
```









