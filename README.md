# 1. Git Fundamentals

Git es un sistema de control de versiones distribuido que permite 
rastrear y gestionar cambios en el código fuente durante el 
desarrollo de software.

> No uses Git con interfaces graficas de usuario (GUI), úsalo por
consola (CLI)
> <samp> *~ Paz Valderrama*</samp>

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
| `git status`           | Muestra el estado completo del repositorio.      |
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
    git diff <archivo>
    ~~~

-   Muestra los cambios que **ya están preparados** (staged) y se
incluirán en el próximo commit.

    ~~~
    git diff --staged <archivo>   # --staged is an alias for --cached
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
    git clean -fd   # Elimina files (-f) y directories (-d) no rastreados
    ~~~

---

## 1.8. Trabajar con remotos

-   Muestra los repositorios remotos configurados en tu maquina
local. La opcion `-v` muestra las URLs.

    ~~~
    git remote
    git remote -v
    ~~~

### 1.8.1. Añadir repositorios remotos 

Añadir un repositorio remoto significa **establecer una conexion
entre tu repositorio local y cualquier otro repositorio u
repositorios remotos.** 

> [!IMPORTANT]
> El comando `git clone` realiza muchas tareas por ti, lo
> importante es que **hace automaticamente la relación entre el
> repositorio remoto y tu repositorio local**. El repositorio
> local que el comando lo crea automaticamente, por defecto, añade el
> repositorio remoto con el nombre de `origin`, este nombre no es
> mas que **una referencia** legible en lugar de la URL completa.

> [!NOTE]
> Cuando inicializas un seguimiento, git crea automaticamente la
> rama `master` donde se almacena todo tu proyecto. Solo cuando
> realizas tu primer commit, git realiza esta acción.

> [!WARNING]
> No pienses erroneamente que solo puedes tener una rama local para
> seguir un único repositorio remoto. En un repositorio local puedes
> tener referencias a varios servidores (repositorios remotos). 

> *Los repositorios remotos son solo referencias en los repositorios locales.*

---

-   Añade un nuevo remoto con un nombre corto que puedas
referenciar fácilmente.
    ~~~
    git remote add [short-name] [url]
    ~~~
    > Por convención, el repositorio original del que clonaste se
    llama `origin`.

> [!IMPORTANT]
> El `short-name` no necesariamente debe ser igual al nombre del
> repositorio remoto. Si entendiste el concepto de referencias, me 
> responderas que efectivamente no debe ser igual porque ea solo
> una referencia para que simplifique manipular el repositorio.

### 1.8.2. Traer y combinar remotos
Ya tienes la referencia, ahora puedes realizar operaciones usando esta referencia.
- Trae toda la informacion que aun no tienes del repositorio remoto. Luego de hacer esto, tendrás referencias a todas las ramas del remoto, las cuales puedes combinar e inspeccionar cuando quieras.
~~~
git fetch [remote-name]
~~~
> Es importante destacar que el comando solo trae datos a tu repositorio local, ni lo combina automaticamente con tu trabajo ni modifica el trabajo que llevas hecho. La combinación con tu trabajo debes hacerla manualmente, creando y configurando ramas locales para que rastreen ramas remotas especificas y finalmente ejecutando el comando `git pull`. Si relacionas esto con lo aprendido te daras cuenta que desde ahora todo es igual.

### 1.8.3. Enviar a tus remotos
- Envia la informacion de una rama local al servidor (repositorio remoto) que este referenciado en el repositorio local.
~~~
git push [remote-name] [branch-name]
~~~
> Si has configurado que una rama local siga a una rama remota especifica, no es necesario realizar esta especificacion, basta con `git push`. De hecho, cuando clonamos un repositorio, git hace automaticamente esta configuracion, por ello solo ejecutamos comandos como `git pull` y `git push`

### 1.8.4. Inspeccionar un remoto
- Muestra informacion acerca de un remoto en particular.
~~~
git remote show [remote-name]
~~~

### 1.8.5. Eliminar y renombrar remotos
- Renombra y/o elimina el repositorio remoto respectivamente.
~~~
git remote rename [back-name] [new-name]
git remote rm [remote-name]
~~~

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
