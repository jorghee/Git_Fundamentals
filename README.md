# 1. Git Fundamentals

Git es un sistema de control de versiones distribuido que permite 
rastrear y gestionar cambios en el código fuente durante el 
desarrollo de software.

> No uses Git con interfaces graficas de usuario (GUI), úsalo por
consola (CLI)
> <samp> ~ Paz Valderrama</samp>

## 1.1. Los tres estados de Git

Para entender Git, es crucial conocer los tres estados principales 
en los que puede encontrarse un archivo:

1.  **Modified (Modificado):** Significa que haz modificado el
archivo, pero aún no has confirmado los cambios en la base de
datos de Git.
2.  **Staged (Preparado):** Significa que haz marcado un archivo
modificado en su version actual para que se incluya en tu proxima
confirmacion (commit).
3.  **Committed (Confirmado):** Significa que los cambios estan
almacenados de forma segura en tu base de datos local de Git.

## 1.2. Las Tres Secciones Principales

Estos estados se corresponden con tres secciones de trabajo en un
proyecto de Git:

1.  **Working Directory (Directorio de Trabajo):** Es una copia
local de una versión del proyecto. Aquí es donde modificas los
archivos directamente.
2.  **Staging Area (Área de Preparación):** Es un archivo,
comúnmente llamado "index" contenido en tu Git Directory, que
almacena la información sobre lo que incluirá tu próxima
confirmación. Es un borrador de tu próximo commit.
3.  **Git Directory (Directorio `.git`):** Es donde Git almacena
los metadatos y la base de datos de objetos de tu proyecto. Es el
corazón de Git, y es lo que se copia cuando clonas un repositorio.

### 1.2.1. Relacion entre los tres estados y secciones de Git

La relación es simple: modificas archivos en tu **Directorio de
Trabajo**, los preparas (añades) al **Área de Preparación**, y
finalmente los confirmas, lo que mueve la instantánea de los
archivos del Área de Preparación a tu **Directorio de Git** de
forma permanente.

---

## 1.3. Configurando Git por primera vez

Antes de empezar a usar Git, debes configurar tu identidad.

-   **Configuración de sistema** Lee y escribe especificamente en
el archivo `/etc/gitconfig`. Valores para todos los usuarios del
sistema.

    ~~~
    git config --system user.name "jorghee"
    git config --system user.email "jorgeehuarsaya@gmail.com"
    ~~~

-   **Configuración global** Lee y escribe especificamente en el
archivo `~/.gitconfig` o `~/.config/git/config`. Valores para tu
usuario.

    ~~~
    git config --global user.name "jorghee"
    git config --global user.email "jorgeehuarsaya@gmail.com"
    ~~~

-   **Configuración especifica del repositorio** Lee y escribe
especificamente en el archivo `config` (es decir `.git/config`).
Es especifico del directorio actual (Repositorio actual).

    ~~~
    git config user.name "jorghee"
    git config user.email "jorgeehuarsaya@gmail.com"
    ~~~

-   Elige el editor de texto por defecto que se utilizara cuando
Git necesite que introduzcas un mensaje.

    ~~~
    git config --global core.editor nvim
    ~~~

-   Muestra todas las propiedades que Git ha configurado.

    ~~~
    git config --list
    ~~~

## 1.4. Creando y Obteniendo Repositorios

Puedes empezar a trabajar con Git de dos maneras principales:
inicializando un nuevo repositorio o clonando uno existente.

### 1.4.1. Inicializando un repositorio en un directorio existente

Si ya tienes un proyecto y quieres empezar a controlarlo con Git.

-   Crea el esqueleto del archivo `.git` en el directorio actual.
Ten en cuenta que aún no hay nada que este bajo seguimiento en el
directorio.

    ~~~
    git init 
    ~~~

- Comienza el seguimiento de los archivos dentro del directorio y
luego realiza tu primer `commit` (confirmación).

    ~~~
    git add .
    git commit -m "Initial project version"
    ~~~

El comando `add` tiene varios funcionalidades, por ejemplo, lo
usamos para empezar a **rastrear archivos nuevos** (como en este
caso); tambien para **preparar archivos** y **marcar archivos
en conflicto como resueltos**.

### 1.4.2. Clonando un repositorio existente

Si quieres obtener una copia de un proyecto que ya existe en un
servidor remoto (como GitHub).

-   El siguiente comando, **obtiene una copia** de un repositorio
Git existente. **crea** un directorio llamado `libgit2`,
**inicializa** un directorio `.git` en su interior, **descarga**
toda la información del repositorio y **saca una copia de trabajo**
de la última versión. 

    ~~~
    git clone https://github.com/libgit2/libgit2
    git clone https://github.com/libgit2/libgit2 <local_name>
    ~~~

> [!NOTE]
> Git te permite usar distintos protocolos  de transferencia. En
> este caso se usa `http://`, pero tambien puedes utilizar
> `git://` o `usuario@servidor:path/to/repository.git` que
> utiliza el **protocolo SSH**.

#### Useful commands

| Command | Description |
| -------------- | --------------- |
| `git clone --recurse-submodules` | Clona un repositorio junto con todos sus *submódulos*, inicializándolos y descargando su contenido. |
| `git clone --depth <number> <url>` | Clona un repositorio con un historial reducido (*shallow clone*) |

## 1.5. Basic workflow

Una vez que tu repositorio está configurado, tu día a día
consistirá en un ciclo de modificación, preparación y confirmación
de cambios.

### 1.5.1 Revisando el Estado de tus archivos

El comando más importante para saber qué está pasando en tu
repositorio.

~~~
git status
~~~

#### Useful commands

| Command       | Description                                      |
| :--------------------- | :----------------------------------------------- |
| `git status -s`        | Muestra una salida breve y compacta del estado.  |
| `git status -sb`       | Salida breve que también muestra la rama actual. |

### 1.5.2. Ignorar archivos: el archivo `.gitignore`

GitHub mantiene una extensa lista de archivos
[.gitignore templates](https://github.com/github/gitignore)
adecuados a docenas de proyectos y lenguajes.

### 1.5.3. Ver los cambios (Staged y Unstaged)

Para saber exactamente qué has modificado.

-   Muestra los cambios en el directorio de trabajo que **aún no
han sido preparados** (unstaged).

    ~~~
    git diff <file>
    ~~~

-   Muestra los cambios que **ya están preparados** (staged) y se
incluirán en el próximo commit.

    ~~~
    git diff --staged <file>   # --staged is an alias for --cached
    ~~~

#### Useful commands

| Command                                | Description                                                                 |
|----------------------------------------|-----------------------------------------------------------------------------|
| `git diff --cached --word-diff`        | Igual que `--cached`, pero resalta las diferencias a nivel de palabras en lugar de líneas. |
| `git diff-tree --no-commit-id --name-only -r <commit>` | Lista solo los nombres de archivos cambiados en un commit específico. |
| `git diff @{upstream}`                 | Muestra las diferencias entre la rama local y su rama remota (*upstream*). |
| `git diff --word-diff`                 | Muestra los cambios resaltando palabras modificadas en lugar de líneas completas. |

### 1.5.4. Añadir cambios al Staging Area

Usa `git add` para mover tus cambios del Directorio de Trabajo al 
Área de Preparación.

#### Useful commands

| Command           | Description                                                                     |
|-------------------|---------------------------------------------------------------------------------|
| `git add <file>`  | Prepara los cambios del archivo especificado.                                   |
| `git add .`       | Prepara todos los cambios (nuevos, modificados, eliminados) en el directorio actual. |
| `git add --all` | Sube todo lo que cambió (repo/área indicada) |
| `git add --patch`      | Permite revisar cada cambio de forma interactiva y decidir si prepararlo o no.    |
| `git add --update`      | Prepara solo los archivos que ya están siendo rastreados (modificados y eliminados), pero no los nuevos. |

### 1.5.5. Confirmando cambios (Commit)

Una vez que tu área de preparación está como la quieres, puedes
confirmar tus cambios. Cada vez que confirmas, estás guardando una
instantánea de tu proyecto que puedes restaurar más tarde.

-   Realiza un commit abriendo tu editor de texto para escribir un
mensaje detallado.

    ~~~
    git commit
    ~~~

-   Prepara automaticamente todos los archivos rastreados antes de
confirmarlos, ahorrandote el paso de `git add`.

    ~~~
    git commit -a -m "Added new benchmarks"
    ~~~

#### Useful commands

| Command             | Description                                                              |
|---------------------|--------------------------------------------------------------------------|
| `git commit -m "msg"` | Confirma los cambios con un mensaje directamente desde la línea de comandos. |
| `git commit -v`     | Muestra los cambios que se están confirmando en el editor, para más contexto. |
| `git commit --amend`| Modifica el último commit. Útil para corregir el mensaje o añadir cambios. |
| `git commit -s`     | Añade una línea `Signed-off-by` al final del mensaje del commit.         |
| `git commit -S`     | Firma el commit con GPG para verificar su autoría.                       |

### 1.5.6. Eliminar archivos

Para eliminar un archivo en Git, tienes que eliminarlo de los
archivos rastreados y luego confirmar.

-   Elimina el archivos del Working Directory y prepara la
eliminación para el próximo commit. 

    ~~~
    git rm <file>
    ~~~

-   Para forzar la eliminación si el archivo tiene cambios
preparados.
    ~~~
    git rm -f <file>       # -f de force
    ~~~

-   Mantiene el archivo en el Working Directory pero lo elimina del Staging Area (deja de ser rastreado)

    ~~~
    git rm --cached <file>
    ~~~

## 1.6. Ver el historial de commits

- Lista los commits hechos sobre ese repositorio en orden
cronológico inverso.

    ~~~
    git log
    ~~~

#### Useful commands and formatting

| Command                               | Description                                                               |
|---------------------------------------|---------------------------------------------------------------------------|
| `git log -p`                          | Muestra las diferencias (patch) introducidas en cada commit.              |
| `git log --all`                       | Muestra el historial de commits de todas las ramas del repositorio.       |
| `git log --stat`                      | Muestra estadísticas de los archivos modificados en cada commit.          |
| `git log --oneline`                   | Muestra cada commit en una sola línea.                                    |
| `git log --graph`                     | Dibuja un gráfico ASCII de la historia de las ramas y fusiones.           |
| `git log --pretty=format:"%h %s"`     | Formatea la salida del log. Ver la tabla de opciones más abajo.           |
| `git log --decorate=short`            | Muestra referencias (HEAD, ramas, tags). Modos `short`, `full` y `auto`.  |
| `git show <commit-hash>`              | Muestra los metadatos y los cambios de un commit específico.              |
| `git blame <file>`                    | Muestra qué autor modificó por última vez cada línea de un archivo.       |

#### Useful choices for `git log --pretty=format`

| Opción  | Descripción de la salida                                |
|---------|---------------------------------------------------------|
| %H      | Hash de la confirmación                                 |
| %h      | Hash de la confirmación abreviado                       |
| %T      | Hash del árbol                                          |
| %t      | Hash del árbol abreviado                                |
| %P      | Hashes de las confirmaciones padre                      |
| %p      | Hashes de las confirmaciones padre abreviados           |
| %an     | Nombre del autor                                        |
| %ae     | Dirección de correo del autor                           |
| %ad     | Fecha de autoría (el formato respeta la opción -–date)  |
| %ar     | Fecha de autoría, relativa                              |
| %cn     | Nombre del confirmador                                  |
| %ce     | Dirección de correo del confirmador                     |
| %cd     | Fecha de confirmación                                   |
| %cr     | Fecha de confirmación, relativa                         |
| %s      | Asunto                                                  |

> [!IMPORTANT]
> Puede que te estés preguntando la diferencia entre *author* y
> *committer*. **Imagina** que estás contribuyendo a un proyecto de
> software libre y envias un `parche`. Entonces, yo, que soy
> colaborador lo revisaré y lo aplicaré. En este caso, **tú eres el
> *author* y yo soy el *committer*.**

### 1.6.1. Limitar la salida del Historial

-   Lista  aquellas confirmaciones hechas después (`since`) de la
fecha especificada.
    ~~~
    git log --since=2.weeks
    git log --since="2025-01-24"
    ~~~

-   Lista solo aquellas confirmaciones que cumplen ciertos criterios.

    ~~~
    git log --author=jorgels
    ~~~

Opciones para limitar la salida de `git log`

| Option            | Output description                                                                            |
|-------------------|-----------------------------------------------------------------------------------------------|
| -(n)              | Muestra solo los últimos `n` commits                                                          |
| --since, --after  | Muestra commits hechos después de la fecha especificada                                       |
| --until, --before | Muestra commits hechos antes de la fecha especificada                                         |
| --author          | Muestra solo commits cuyo autor coincide con la cadena especificada                           |
| --committer       | Muestra solo commits cuyo committer coincide con la cadena especificada                       |
| -S                | Muestra solo commits que agregan o eliminan codigo que corresponda con la cadena especificada |

## 1.7. Deshacer cosas

En cualquier punto, puede que quieras deshacer algo.

-   **Modificar el último commit:** Si has confirmado y te das
cuenta de que olvidaste añadir un archivo o escribiste mal el
mensaje. La instantánea del staging area reemplazará al commit
anterior.

    ~~~
    git add forgotten_file.txt
    git commit --amend
    ~~~

### 1.7.1. Deshacer un archivo preparado

-   Si preparaste un archivo por error, este comando lo devuelve
al estado "Modified".

    ~~~
    git reset HEAD <file>
    ~~~

> [!NOTE]
> El comando `reset` es mágico, se recomienda investigar cómo
> explotarlo para que haga cosas realmente interesantes.

### 1.7.2. Deshacer un archivo modificado
- Si quieres descartar los cambios hechos en tu directorio de
trabajo desde el último commit.

    ~~~
    git restore <file>
    ~~~

### 1.7.3. Deshacer commits

#### Los 3 árboles
Una manera más fácil de pensar sobre `reset` y `checkout` es
comprender a Git como un administrador de contenido de tres
árboles diferentes.

> [!IMPORTANT]
> Por “árbol”, aquí realmente queremos decir “colección de
> archivos”, no específicamente la estructura de
> datos. (Hay algunos casos donde el índice no funciona
> exactamente como un árbol, pero
> para nuestros propósitos es más fácil pensarlo de esta
> manera por ahora)

1. **HEAD** Es el puntero a la referencia de bifurcación
    actual, que es, a su vez, un puntero al último commit
    realizado en esa rama. Eso significa que HEAD será el
    padre del próximo commit que se cree. 

> [!IMPORTANT]
> En general, es más simple pensar en HEAD como un
> snapshot completo del proyecto en tu último commit.

> [!NOTE]
> Es bastante fácil ver cómo es el aspecto de ese snapshot
> (instantánea). Aquí hay un ejemplo de cómo obtener la **lista del
> directorio real** y **las sumas de comprobación SHA-256
> para cada archivo** en la instantánea de HEAD.

~~~
$ git cat-file -p HEAD
tree cfda3bf379e4f8dba8717dee55aab78aef7f4daf
author Scott Chacon 1301511835 -0700
committer Scott Chacon 1301511835 -0700

commit inicial

$ git ls-tree -r HEAD
100644 blob a906cb2a4a904a152... README
100644 blob 8f94139338f9404f2... Rakefile
040000 tree 99f1a6d12cb4b6f19... lib
~~~

2. **Índice** Es tu **siguiente commit propuesto**. También nos
    hemos estado refiriendo a este concepto como el "Área de
    Preparación" de Git ya que esto es lo que Git ve cuando
    ejecutas `git commit`.

> [!IMPORTANT]
> Git rellena este índice con una lista de los archivos tal como
> se veían la última vez que fueron extraídos (checkout) a tu
> directorio de trabajo. Luego, reemplaza algunos de esos archivos
> con nuevas versiones de ellos, y `git commit` los convierte en
> el árbol para un nuevo commit.

> [!CAUTION]
> El índice no es técnicamente una estructura de árbol, en realidad
> se implementa como un manifiesto aplanado, pero para nuestros
> propósitos, está lo suficientemente cerca.

~~~
$ git ls-files -s
100644 a906cb2a4a904a152e80877d4088654daad0c859 0 README
100644 8f94139338f9404f26296befa88755fc2598c289 0 Rakefile
100644 47c6340d6459e05787f644c2447d2595f5d3a54b 0 lib/simplegit.rb
~~~

3. **Working Directory** Descomprime los objetos del directorio `.git`
    para tratarlos como archivos reales listos para ser editados.

#### Useful commands

-   **`git reset`**: Mueve el puntero `HEAD` a un commit anterior,
afectando potencialmente el **Staging Area** y el **Working
Directory**. Es poderoso pero puede ser destructivo.

    | Command                   | Description                                                                                             |
    |---------------------------|---------------------------------------------------------------------------------------------------------|
    | `git reset --soft HEAD~1` | Deshace el último commit, pero mantiene los cambios en el Staging Area.                                  |
    | `git reset --mixed HEAD~1`| **(Por defecto)** Deshace el último commit y deja los cambios en el Working Directory (unstaged).         |
    | `git reset --hard HEAD~1` | **Danger** Deshace el último commit y descarta todos los cambios en el Staging Area y Working Directory. |

-   **`git revert`**: Crea un **nuevo commit** que invierte los
cambios introducidos por un commit anterior. Es la forma segura de
deshacer cambios en un historial público, ya que no reescribe la
historia.

    ~~~
    git revert <commit-hash>
    ~~~

-   **`git clean`**: Elimina archivos no rastreados del Working
Directory. Es útil para tener un espacio de trabajo limpio.

    ~~~
    git clean -fd  # Delete files (-f) and directories (-d) untracked
    ~~~

---

## 1.8. Ramas (Branching): El superpoder de Git

Una de las características más importantes de Git es su sistema de
ramas. Una rama es esencialmente un puntero móvil y ligero a uno de
tus commits.

### 1.8.1. Creación, listado y cambio de ramas

Las ramas te permiten divergir de la línea principal de desarrollo
y continuar trabajando de forma aislada sin afectar esa línea principal.

| Command                       | Description                                                     |
|-------------------------------|-----------------------------------------------------------------|
| `git branch`                  | Lista todas las ramas locales. La actual está marcada con `*`.    |
| `git branch <nombre-rama>`    | Crea una nueva rama.                                            |
| `git switch <nombre-rama>`    | Cambia a la rama especificada. Es el comando moderno y preferido. |
| `git switch -c <nombre-rama>` | Crea una nueva rama y cambia a ella inmediatamente.             |
| `git branch -d <nombre-rama>` | Elimina una rama que ya ha sido fusionada.                      |
| `git branch -D <nombre-rama>` | Fuerza la eliminación de una rama, incluso si no ha sido fusionada. |
| `git branch -a`               | Lista todas las ramas, tanto locales como remotas.              |

### 1.8.2. Fusión de ramas (Merging)

Una vez que has completado el trabajo en tu rama, puedes fusionar
tus cambios de nuevo en la rama principal.

~~~
git switch main
git merge <name-branch-to-merge>
~~~

![Ejemplo de `git merge`](./.github/git_merge.png)

> [!IMPORTANT]
> **Conflictos de fusión:** Si dos ramas modifican la misma parte
> del mismo archivo, Git no podrá fusionarlas automáticamente y se
> producirá un conflicto. Deberás resolver el conflicto manualmente
> editando el archivo, guardándolo, y luego usando `git add` y
> `git commit` para finalizar la fusión.

## 1.9. Reorganizando el trabajo

### 1.9.1. Rebase: La alternativa a la fusión

Rebase es otra forma de integrar cambios de una rama a otra. En lugar
de crear un commit de fusión, rebase **reaplica** los commits de tu
rama, uno por uno, sobre la rama de destino. Esto resulta en un
historial de proyecto más limpio y lineal.

~~~
git switch feature-branch
git rebase main
~~~

![Ejemplo de `git rebase`](./.github/git_rebase.svg)

> [!CAUTION]
> Nunca hagas rebase en commits que ya has empujado a un repositorio
> público y que otras personas pueden estar usando. Reescribir el
> historial compartido puede causar serios problemas.

#### Useful commands

| Command                   | Description                                                   |
|---------------------------|---------------------------------------------------------------|
| `git rebase --abort`      | Cancela el rebase en curso y restaura el estado anterior.     |
| `git rebase --continue`   | Continúa el rebase tras resolver conflictos manualmente.      |
| `git rebase --skip`       | Omite el commit en conflicto y prosigue con el rebase.        |

### 1.9.2. Rebase Interactivo

Una herramienta increíblemente poderosa para reescribir, combinar,
reordenar y eliminar commits antes de compartirlos.

~~~
git rebase -i HEAD~3   # Revisa y modifica los últimos 3 commits
~~~

### 1.9.3. Rebase Avanzado

El rebase avanzado permite mover un rango específico de commits
desde una rama y reaplicarlos sobre otra base diferente.
Esto es útil cuando deseas **reubicar solo una parte de la
historia** sin arrastrar commits innecesarios.

~~~
git rebase --onto <new-base> <upstream> <branch>
~~~

* `<new-base>`: Donde quieres “pegar” los commits.
* `<upstream>`: Hasta dónde cortar de la historia.
* `<branch>`: La rama que contiene los commits a mover.

### 1.9.4. Cherry-pick

Permite tomar un commit específico de una rama y aplicarlo sobre otra.

~~~
git cherry-pick <commit-hash>   # list of commits
~~~

#### Useful commands

| Command                       | Description                                                                   |
|-------------------------------|-------------------------------------------------------------------------------|
| `git cherry-pick --abort`     | Cancela el proceso de cherry-pick en curso y revierte los cambios aplicados.  |
| `git cherry-pick --continue`  | Reanuda un cherry-pick después de resolver conflictos manualmente.            |

## 1.10. Guardado temporal de cambios (Stashing)

A veces necesitas cambiar de rama, pero tienes trabajo a medio hacer
que no quieres confirmar todavía. El comando `stash` toma tu estado
de trabajo modificado, lo guarda en una pila de cambios sin terminar
y te devuelve a un directorio de trabajo limpio.

| Command                               | Description                                                               |
|---------------------------------------|---------------------------------------------------------------------------|
| `git stash push`                      | Guarda tus cambios locales no confirmados en el "stash".                  |
| `git stash push --include-untracked`  | Guarda tus cambios locales junto con archivos no rastreados.              |
| `git stash --all`                     | Guarda tus cambios locales incluyendo archivos ignorados y no rastreados. |
| `git stash list`                      | Muestra la lista de todos los cambios guardados en el stash.              |
| `git stash apply`                     | Aplica el último stash sin eliminarlo de la lista.                        |
| `git stash pop`                       | Aplica el último stash y lo elimina de la lista.                          |
| `git stash drop`                      | Elimina el último stash de la lista.                                      |
| `git stash clear`                     | Elimina todos los stashes.                                                |
| `git stash show --text`               | Muestra el contenido detallado del último stash en formato de texto.      |

## 1.11. Trabajar con remotos

-   Muestra los repositorios remotos configurados en tu maquina
local. La opcion `-v` muestra las URLs.

    ~~~
    git remote
    git remote -v
    ~~~

### 1.11.1. Añadir repositorios remotos 

Añadir un repositorio remoto significa **establecer una conexion
entre tu repositorio local y cualquier otro repositorio o
repositorios remotos.** 

> [!IMPORTANT]
> El comando `git clone` realiza muchas tareas por ti, una de ellas
> es **realizar automáticamente la relación entre el repositorio
> remoto y tu repositorio local**.
>
> El comando crea el repositorio local y  añade el repositorio
> remoto con el nombre de `origin` por defecto. Este nombre no es
> más que **una referencia** legible en lugar de la URL completa.

> [!NOTE]
> Cuando inicializas un repositorio, [Git](https://git-scm.com/)
> crea por defecto la rama `master`, pero esta solo toma forma
> cuando realizas el primer commit.

> [!WARNING]
> No creas que solo puedes tener una rama local para conectada a un
> único repositorio remoto. Un repositorio local puede referenciar a
> varios servidores (repositorios remotos). 

> *Un repositorio remoto no es más que una referencia guardada en 
tu repositorio local.*

---

-   Añade un nuevo remoto con un nombre corto que puedas
referenciar fácilmente.

    ~~~
    git remote add <remote-name> <url>
    ~~~

    > Por convención, el repositorio original del que clonaste se
    llama `origin`.

> [!IMPORTANT]
> El `remote-name` no tiene que coincidir con el nombre del
> repositorio remoto. Si ya entendiste el concepto de referencias,
> seguramente ya pensaste responder que efectivamente no debe ser 
> igual porque es solo un `alias` para simplificar las operaciones
> con el repositorio.

### 1.11.2. Traer, combinar y enviar a remotos

-   `fetch`: Trae toda la información que aún no tienes del
    repositorio remoto. **Solo descarga los datos**; no los
    fusiona con tu trabajo.

    ~~~
    git fetch <remote-name>
    ~~~

> [!IMPORTANT]
> La fusión con tu trabajo debes ser manual, creando y
> configurando ramas locales para que rastreen ramas remotas
> específicas.

-   `pull`: Es esencialmente un `git fetch` seguido de un
    `git merge`. Trae los cambios del remoto y los fusiona
    inmediatamente en tu rama local actual.

    ~~~
    git pull <remote-name> <branch-name>
    ~~~

-   `push`: Envia los commits de tu rama local al repositorio remoto.

    ~~~
    git push <remote-name> <branch-name>
    ~~~

> [!NOTE]
> Si una rama local está configurada para seguir a una rama remota,
> no es necesario especar nada al hacer `git push` o `git pull`.
> De hecho, al clonar un repositorio, Git realiza esta
> configuración automáticamente, por eso normalmente basta con
> usar los comandos sin argumentos.

#### Useful commands

| Command                       | Description                                                                                               |
|-------------------------------|-----------------------------------------------------------------------------------------------------------|
| `git fetch --all`             | Descarga actualizaciones de todos los remotos configurados.                                               |
| `git fetch --prune`           | Elimina referencias a ramas remotas que ya no existen en el servidor.                                     |
| `git fetch --jobs=10`         | Ejecuta hasta 10 tareas de `fetch` en paralelo para mayor velocidad.                                      |
| `git pull --rebase`           | Realiza un `fetch` y luego un `rebase` en lugar de un `merge`. Mantiene un historial lineal.              |
| `git pull --autostash`        | Guarda temporalmente cambios sin commitear (`stash`), realiza el `pull` y luego los restaura.             |
| `git push -u origin <branch>` | Empuja la rama al remoto y la configura para que se rastreen mutuamente (`-u` es `--set-upstream`).       |
| `git push --force-with-lease` | Fuerza un push solo si la rama remota no ha sido actualizada por otra persona. Más seguro que `--force`.  |
| `git push --dry-run`          | Simula un `push` mostrando qué cambios se enviarían, sin subir nada al remoto.                            |

### 1.11.4. Inspeccionar y gestionar remotos

-   Muestra informacion detallada sobre un remoto en particular.

    ~~~
    git remote show <remote-name>
    ~~~

-   Renombra y elimina un repositorio remoto, respectivamente.

    ~~~
    git remote rename <back-name> <new-name>
    git remote rm <remote-name>
    ~~~

#### Useful commands

| Command                | Description                                                                                                  |
|------------------------|--------------------------------------------------------------------------------------------------------------|
| `git remote update`    | Descarga actualizaciones de todos los remotos y sus ramas, pero sin fusionarlas ni modificarlas localmente.  |
| `git remote set-url`   | Cambia la URL asociada a un remoto existente (ej. pasar de HTTPS a SSH).                                     |

## 1.12. Etiquetado (Tagging)

Git puede etiquetar puntos específicos en el historial como
importantes. Típicamente, la gente usa esta funcionalidad para
marcar puntos de lanzamiento.

| Command                           | Description                                                                   |
|-----------------------------------|-------------------------------------------------------------------------------|
| `git tag`                         | Lista todas las etiquetas existentes.                                         |
| `git tag -s`                      | Crea un tag firmada con tu clave GPG, para autenticidad de la versión marcada.|
| `git tag -l "v1.8.5*"`            | Lista tags que coinciden con un patrón.                                       |
| `git tag -a v1.4 -m "version 1.4"`| Crea un tag "anotada", que se almacena como un objeto completo en Git.        |
| `git tag v1.4-lw`                 | Crea un tag "ligera", que es solo un puntero a un commit.                     |
| `git push origin --tags`          | Empuja todas tus etiquetas al servidor remoto.                                |

---

# Questions and research

> [!IMPORTANT]
> No te quedes solo con esta informacion, investiga a profundidad
> por tu cuenta.

> [!CAUTION]
> - Usa GitHub, es el programa mas usado para alojar tus
>   repositorios remotos.
> - Practica y prueba los siguiente comandos para analizar su
>   funcionamiento, no esperes a que te digan que es lo que va a
>   pasar, tu puedes probarlo.
> - Te exigo tener creado un repositorio local y uno remoto.
>   Nombralos como mas te guste.
