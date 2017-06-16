Éditer les fichiers Markdown avec Vim
=====================================

Document écrit par Nicolas Kovacs <info@microlinux.fr>

Ce document décrit l'installation du plug-in `vim-instant-markdown` pour Vim,
qui permet de prévisualiser instantanément les fichiers Markdown.


Prérequis
---------

Les paquets suivants doivent être installés sur le système.

  * `curl`

  * `xdg-utils`

  * `nodejs`

  * `npm`


Installation
------------

Installer le plug-in en tant que `root`.

```
# npm -g install instant-markdown-d
```

Récupérer le contenu du dépôt Github.

```
$ git clone https://github.com/suan/vim-instant-markdown
```

Créer l'arborescence suivante.

```
$ mkdir -pv .vim/after/ftplugin/markdown
mkdir: création du répertoire « .vim »
mkdir: création du répertoire « .vim/after »
mkdir: création du répertoire « .vim/after/ftplugin »
mkdir: création du répertoire « .vim/after/ftplugin/markdown »
```

Ranger le fichier `instant-markdown.vim` à l'endroit approprié.

```
$ cp vim-instant-markdown/after/ftplugin/markdown/instant-markdown.vim \
  .vim/after/ftplugin/markdown/
```

Faire un peu de ménage.

```
$ rm -rf vim-instant-markdown/
```

Vérifier si `/etc/vimrc` contient bien l'option suivante. 

```
filetype plugin on
```

Créer ou éditer `~/.vimrc`pour l'utilisateur qui souhaite éditer des fichiers
Markdown.

```
autocmd BufNewFile,BufReadPost *.md set filetype=markdown
let g:markdown_fenced_languages = ['html', 'python', 'bash=sh']
let g:markdown_syntax_conceal = 0
let g:markdown_minlines = 100
```

