# Doc

## Check version
`git --version`

## Clone doc
`git clone git://git.kernel.org/pub/scm/git/git-htmldocs.git git-doc`

## Get help
`git help`
`git help commit`

## Config
- `git config`: query, set, replace, unset options with this command.
  - `--replace-all`: default behavior is to replace at most one line.
  - `--add`: adds a new line to the option without altering any existing values.
	- `--get`: get the value for a given key. 
  - `--global`: write to global `~/.gitconfig` file rather than the repository `.git/config`.
	- `--system`: write to system-wide `$(prefix)/etc/gitconfig` rather than the repository `.git/config`.
  - `--local`: write to the repository `.git/config`. This is the default behavior.
	- `--worktree`: similar to `--local` except that `.git/config.worktree`.
- Initial config. This information is attached to commits.
  `git config --global user.name "Diego Garrido"`
  `git config --global user.email "diego.garrido.6568@gmail.com"`
- Check global config.
  `git config --global -e`
- Specified a text editor.
  `git config --global core.editor "atom --wait"`

## Create local repo
 - `git init`: initialize an empty git repository in the current directory. This command can receive more arguments.

## Ignore files
- `.gitignore`: specifies intentionally untracked files to ignore.
- directories
  - `<relative_path>/folder_name`: ignore all content in folder.
  - `doc/frotz`: match doc/frotz/ but not a/doc/frotz.
  - `frotz/`: match frotz and a/frotz.
   - `**/foo/bar`: matches file or directory "bar" anywhere that is directly under directory "foo".
- files
  - `<relative_path>/file_name`: ignore file.
  - `*.txt`: ignore all files that end with .txt. 

## Check status
- `git status`: show files that hava been updated since the last commit.
- `git status -s`: show the output in the short-format. M: modified, A: added, R: renamed, D: deleted, Green: in stage, Red: out of stage.
- `git status -sb`: also show the branch.

## Stage files 
- `git add -A`: add all files to stage.
- `git add .`: add all files to stage.
- `git add ./git add -A`: add all modified files since the last commit.
- `git add <file_name>`: add file to stage.
- `git add <list>`: add files to stage.
- `git add <folder_name/>`: add all files in the directory to stage.
- `git add <folder/*extension_name>`: add all files from the directory that match the extension_name to stage.
- `git add -f`: add ignored files.
- `git add -i`: get into interactive mode.
- `git add -p`: interactively choose hunks of patch between the index and the work tree and add them to the index.

## Unstage files
- `git reset HEAD <file_name>`: unstages any modifications made to the file since the last commit. This doesn't revert them in the filesystem.

## Diff
- `git diff`: show differences between commits, commit and working tree, etc.
- `git diff --staged/--cached`: show differences between Head and stage.
- `git diff <brabch1>..<branch2>`: show differences between branches. 

## Remove files
- `git rm <file_name>`: remove files from the working tree and from the index. Also staged the removal of the file.

## Move files
- `git mv <source> <destionation>`: move or rename file(s).

## Commit
- `git commit -m "<message>"`: record changes to the repository.
- `git commit --amend "<message>"`: create a new commit which fixes up the last commit.

## Tags
- `git tag`: list tags.
- `git tag <tag_name>`: add tag to last commit. 
- `git tag <tag_name> <commit>`: add tag to commit. 
- `git tag -a <tag_name> -m "<message>"`: add tag and a comment to it.
- `git tag -a <tag_name> <commit> -m "<message>"`
- `git tag -d <tag_name>`: delete tag. 
- `git show <tag_name>`: show tag details. 

## Branch
- `git branch`: list branches.
- `git branch <branch_name>`: create new branch.
- `git -m <old_name> <new_name>`: rename branch.
- `git branch -d <branch_name>`: delete branch. The branch must be fully merged in its upstrem branch.
- `git branch -D <branch_name>`: shortcut for --delete --force.
- `git branch -a`: list both remote-tracking branches and local branches.
- `git push <remote_name> -d <branch_name>`: delte remote branch. 

## Log 
- `git log`: show commit logs. 
- `git log --oneline`: make the logs look better. 
- `git log --oneline --decorate --all --graph`: make the logs look better. 

WIP
## Checkout
- `git checkout -- .`: vuelve al �ltimo commit todos los archivos
- `git chekout -- file_name`: vuelve al �ltimo commit el archivo
- `git checkout nombre_rama`: cambiar de rama en uso
- `git checkout -b nombre_rama`: crear y cambiar de rama

## Reset
git reset HEAD file_name: sacar archivo del staged, HEAD apunta al �ltimo commit
soft -> revierte a un determinado punto en nuestra historia
git reset --soft HEAD^: actualizar el �ltimo commit con el pr�ximo commit
git reset --soft commit_id: el proximo commit reemplazar� al commit post commit_id, es equivalente a HEAD^ si commit_id es el penultimo commit.
git reset --mixed commit_id: ir a un punto de la historia, imprime
los archivos que han cambiado (desde el �ltimo commit hasta commit_id)
y los saca del staged, no se han perdido los archivos y sus modificaciones. perdemos los commit entremedio.
git reset --hard commit_id: vuelve commit_id el HEAD y se pierden cambios
post commit_id.
git reflog: historial de movimiento, commits, resets, et.
=> git reset --hard commit_id => recuperamos la historia hasta el commit_id

recovery delted files?

## Merge
git merge nombre_rama: merge fast-forward entre el HEAD y nombre_rama

squash and merge: realiza merge entre rama y master y deja el �ltimo commit de la rama como commit del merge.

## Rebase
rebase: mueve los commit de la rama a un �rea temporal, luego pone el puntero de la rama en el �ltimo commit del master e inserta los commit de la rama a partir de ah�. Si se realiza un merge entre master y la rama ser� un merge fastforward
git rebase master: realiza un rebase entre la rama master y la rama actual de trabajo

git rebase -i HEAD/hash~3:rebase interactivo, sirve para:
1. ordenar commits
2. corregir mensajes de los commits
3. unir commit
4. separar commits

git rebase -i HEAD~4: rebase interactivo de los �ltimos 4 commits=>se abre un archivo de texto con los 4 commits.
squas/s: fusionar un commit con el anterior -> abre ventana de texto para renombrar el nuevo commit.
reword/r: renombrar un commit->abre ventana de texto para renombrar
edit/e: para volver editable un commit. Luego realizo un reset al commit anterior y realizo commits con los cambios, luego un rebase continue, los commits han reemplazado al commit editado.
git rebase --continue
git checkout --/hash /file_name/folder_name: regresar archivo al estado del �ltimo/hash commit

## Stash
git stash __/save "Mensaje Opcional": guardar proyecto con cambios realizados desde el �ltimo commit (WIP) en el stash y regresa al estado del proyecto del �ltimo commit
git stash list: muestra lista de stash
git stash list --stash: muestra m�s informaci�n de la lista de stash
git show stash/stash@{id}: muestra m�s informaci�n del �ltimo/id stash.
git stash pop: extrae el �ltimo ingreso del stash y lo eliminar� (apply+drop)
git stash apply __/stash@{id}: restaura al �ltimo/id stash
git stash drop __/stash@{id}: elimina el �ltima stash.
git stash save --keep-index: guarda en el stash todo menos los archivos en el stage.
git stash save --include-untracked: incluye todo los archivos a los que git no les da siguimiento.
git stash clear: borra todas las entradas en el stash

## Shorcuts
git config --global alias.alias_name "big expresion to alias"
ejemplos:
1. git config --global alias.lg "log --oneline --decorate --all --graph"
2. git config --global alias.s "status -s -b"
git config --global -e: revisar las configuraciones globales
git config --global -l: revisar las configuraciones globales en modo lectura

## Work with remote 
git remote add origin URL_destino: add->nuevo remote,
origin: El nombre de Nuestro remote, est�ndar es origin.
git remote -v: Leer fuentes remotas
git push -u origin master:
origin: nombre del repositorio  master: rama que deseamos enviar
-u: la pr�xima vez que queremos hacer un push no necesitamos especificar la rama ni el origen.
git push --tags: sube los tags, ya que por defecto no se suben tras el push
git pull:bajar los cambios del repositorio remoto al local e intenta un merge.
git fetch: no realiza el merge en forma automatica, actualiza localmente los cambios del repositorio remoto
git clone url/path folder_name(opcional): clonar un repositorio
git fork: contribuir a un repositorio - clonar repositorio que no somos due�os o colaboradores y crea nueva rama sobre un repositorio, con un pull request le mandamos nuestros cambios al due�o del repositorio para que ellos lo acepten.

Gitflow/Githubflow

## SSH
ls .ssh: verificar si ya existe
mkdir .ssh: crear directorio
cd .ssh/: entrar al directorio
ssh-keygen -t rsa -C "diego.garrido.6568@gmail.com": asociar al correo asociado a github
ssh -T git@github.com:conectar a github a trav�s de la clave ssh.
