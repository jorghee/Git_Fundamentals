# Fundamentos de Git
Git es un sistema de control de versiones distribuido que permite 
rastrear y gestionar cambios en el código fuente durante el 
desarrollo de software.

> *No uses Git con interfaces graficas de usuario (GUI), úsalo por 
consola (CLI)*
> - Paz Valderrama

## Los tres estados de Git

Para entender Git, es crucial conocer los tres estados principales 
en los que puede encontrarse un archivo:

### Modified (modificado)
Significa que haz modificado el archivo, pero aún no has confirmado 
los cambios en la base de datos de Git.

### Staged (preparado)
Significa que haz marcado un archivo modificado en su version 
actual para que se incluya en tu proxima confirmacion (commit).

### Committed (confirmado)
Significa que los cambios estan almacenados de forma segura en tu
base de datos local de Git.

## Las Tres Secciones Principales

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

### Relacion entre los tres estados y secciones de Git

La relación es simple: modificas archivos en tu **Directorio de Trabajo**, los preparas (añades) al **Área de Preparación**, y finalmente los confirmas, lo que mueve la instantánea de los archivos del Área de Preparación a tu **Directorio de Git** de forma permanente.

---

## Configurando Git por primera vez

Antes de empezar a usar Git, debes configurar tu identidad.

-   **Configuración de sistema** Lee y escribe especificamente en 
el archivo `/etc/gitconfig`. Valores para todos los usuarios del 
sistema.
~~~
git config --system user.name "jorghee"
git config --system user.email "jorgeehuarsaya@gmail.com"
~~~

-   **Configuración global** Lee y escribe especificamente en el 
archivo `~/.gitconfig` o `~/.config/git/config`. Valores para tu usuario.
~~~
git config --global user.name "jorghee"
git config --global user.email "jorgeehuarsaya@gmail.com"
~~~

-   **Configuración especifica del repositorio** Lee y escribe 
especificamente en el archivo `config` (es decir `.git/config`). Es 
especifico del directorio actual (Repositorio actual).
~~~
git config user.name "jorghee"
git config user.email "jorgeehuarsaya@gmail.com"
~~~

-   Elige el editor de texto por defecto que se utilizara cuando 
Git necesite que introduzcas un mensaje.
~~~
git config --global core.editor nvim
~~~

-   Muestra todas las propiedades que Git ha configurado
~~~
git config --list
~~~

## Creando y Obteniendo Repositorios

Puedes empezar a trabajar con Git de dos maneras principales: 
inicializando un nuevo repositorio o clonando uno existente.

### Inicializando un repositorio en un directorio existente

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

## Clonando un repositorio existente

Si quieres obtener una copia de un proyecto que ya existe en un servidor remoto (como GitHub).

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
> esta ocasion se uso `http://`, pero tambien puedes utilizar 
> `git://` o `usuario@servidor:ruta/del/repositorio.git` que 
> utiliza el protocolo SSH.

### Comandos útiles

| Command | Description |
| -------------- | --------------- |
| `git clone --recurse-submodules` | Clona un repositorio junto con todos sus *submódulos*, inicializándolos y descargando su contenido. |
| `git clone --depth <number> <url>` | Clona un repositorio con un historial reducido (*shallow clone*) |

## Revisando el Estado de tus archivos

El comando más importante para saber qué está pasando en tu 
repositorio.

~~~
git status
~~~

### Comandos Útiles y Alias

| Comando Completo       | Descripción                                      |
| :--------------------- | :----------------------------------------------- |
| `git status`           | Muestra el estado completo del repositorio.      |
| `git status -s`        | Muestra una salida breve y compacta del estado.  |
| `git status -sb`       | Salida breve que también muestra la rama actual. |

## Ignorar archivos: el archivo `.gitignore`

GitHub mantiene una extensa lista de archivos
[.gitignore templates](https://github.com/github/gitignore)
adecuados a docenas de proyectos y lenguajes.

## Ver los cambios (Staged y Unstaged)

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

### Comandos útiles

| Command                                | Description                                                                 |
|----------------------------------------|-----------------------------------------------------------------------------|
| `git diff --cached --word-diff`        | Igual que `--cached`, pero resalta las diferencias a nivel de palabras en lugar de líneas. |
| `git diff-tree --no-commit-id --name-only -r <commit>` | Lista solo los nombres de archivos cambiados en un commit específico. |
| `git diff @{upstream}`                 | Muestra las diferencias entre la rama local y su rama remota (*upstream*). |
| `git diff --word-diff`                 | Muestra los cambios resaltando palabras modificadas en lugar de líneas completas. |

## Saltar el Staging Area
- Prepara automaticamente todos los archivos rastreados antes de confirmarlos, ahorrandote el paso de `git add`.
~~~
git commit -a -m "Added new benchmarks"
~~~

## Eliminar archivos
- Elimina tu archivo del Git Directory y del Working Directory, sim embargo esta a la espera de ser confirmado (commited). Pero, puedes **forzar** la eliminacion inmediata sin la necesidad de una confirmacion.
~~~
git rm <file>
git rm -f <file>       # -f de force
~~~

- Mantiene el archivo en el Working Directory pero lo elimina del Staging Area
~~~
git rm --cached <file>
~~~

## Ver el historial de commits (confirmaciones)
- Lista los commits hechos sobre ese repositorio en orden cronologico inverso.
~~~
git log
~~~

- Muestra las diferencias introducidas en cada confirmacion
~~~
git log -p
~~~

- Muestra una lista de archivos con un resumen de las lineas insertadas y eliminadas de los archivos confirmados
~~~
git log --stat
~~~

- Modifica el formato de salida
~~~
git log --pretty=<choice>        # choices: oneline, short, full, fuller, format
git log --pretty=format:"%h - %an, %ar : %s"        # Example
~~~

Opciones utiles de `git log --pretty=format

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

> Puede que te estés preguntando la diferencia entre autor (author) y confirmador (committer). **Imagina** que estas contribuyendo a un proyecto de software libre y mandas un "parche", entonces, yo, que soy colaborador lo revisare y como se que eres de la UNSA lo aplicare. Pasa que ambos hemos sido autores de ese "parche". **Tu como autor y yo como confirmador.**

## Limitar la salida del Historial
- Lista  aquellas confirmaciones hechas despues de la fecha especificada (since - desde). Filtra segun el autor.
~~~
git log --since=2.weeks
git log --since="2050-01-24"
git log --author=jorgels
~~~
- Lista solo aquellas confirmaciones que cumplen ciertos criterios.
~~~
git log --author=jorgels
~~~

Opciones para limitar la salida de `git log`

| Opción            | Descripción de la salida                                                                          |
|-------------------|---------------------------------------------------------------------------------------------------|
| -(n)              | Muestra solamente los ultimos n commits                                                           |
| --since, --after  |  Muestra los commits hechos despues de la fecha especificada                                      |
| --until, --before | Muestra los commits hechos antes de la fecha especificada                                         |
| --author          | Muestra solo los commits cuyo autor coincide con la cadena especificada                           |
| --committer       | Muestra solo los commits cuyo committer coincide con la cadena especificada                       |
| -S                | Muestra solo los commits que agregan o eliminan codigo que corresponda con la cadena especificada |

## Deshacer cosas
- Utiliza tu Staging Area para el nuevo commit, si no hay cambios preparados despues del commit que deseas reemplazar, entonces la instantanea lucira exactamente igual y lo unico que cambiaras sera el mensaje de confirmacion.
~~~
git commit --amend
~~~

### Deshacer un archivo preparado
- Deshace la preparacion del archivo especifico.
~~~
git reset HEAD <file>
~~~
> Ojito: Este comando `reset` es magico, te recomiendo investigar como usarlo para que haga cosas realmente interesantes.

### Deshacer un archivo modificado
- Deshace los cambios hechos en el directorio de trabajo
~~~
git restore <file>
~~~

---
## Trabajar con remotos
- Muestra los repositorios remotos configurados en tu maquina local, la opcion -v muestra las URLs
~~~
git remote
git remote -v
~~~

## Añadir repositorios remotos 
Es importante que domines esta teoria entretenida, es por ello que no debes quedarte solo con esta informacion, investiga a profundidad.
- Usa GitHub, es el programa mas usado para alojar tus repositorios remotos.
- Practica y prueba los siguiente comandos para analizar su funcionamiento, no esperes a que te digan que es lo que va a pasar, tu puedes probarlo.
- Te exigo tener creado un repositorio local y uno remoto. Nombralos como mas te guste.

Añadir un repositorio remoto **significa establecer una conexion entre tu repositorio local y cualquier otro repositorio remoto u repositorios remotos.** 

> Recuerda que el comando `git clone` realizaba muchas tareas por ti, lo que importa saber es que **hace automaticamente la relacion entre el repositorio remoto y tu repositorio local**, dicho repositorio local que tambien lo crea automaticamente. Por defecto añade el repositorio remoto con el nombre de `origin`, este nombre no es mas que **una referencia** para usarla en lugar de la URL completa.

Cuando inicializas un seguimiento, git crea automaticamente la rama `master` donde se almacena todo tu proyecto. Ten en cuenta que solo cuando realizas tu primera confirmacion (commit), git realiza esta accion.
No te quiero confundir pero no pienses erroneamente que solo puedes tener una rama local para seguir solo un repositorio remoto, de hecho, en un repositorio local puedes tener referencias a varios servidores (repositorios remotos). Me sentire bien conmigo mismo si es que entendiste que los repositorios remotos son solo referencias en los repositorios locales.

---
- Añade un remoto nuevo y asocia a un nombre que puedas referenciar facilmente.
~~~
git remote add [name] [url]
~~~

> El `name` no necesariamente debe ser igual al nombre del repositorio remoto, si entendiste eso de referencias, me diras que obviamente no debe ser igual ya que es solo una referencia para que me simplifique manipular dicho repositorio.

## Traer y combinar remotos
Ya tienes la referencia, ahora puedes realizar operaciones usando esta referencia.
- Trae toda la informacion que aun no tienes del repositorio remoto. Luego de hacer esto, tendrás referencias a todas las ramas del remoto, las cuales puedes combinar e inspeccionar cuando quieras.
~~~
git fetch [remote-name]
~~~
> Es importante destacar que el comando solo trae datos a tu repositorio local, ni lo combina automaticamente con tu trabajo ni modifica el trabajo que llevas hecho. La combinación con tu trabajo debes hacerla manualmente, creando y configurando ramas locales para que rastreen ramas remotas especificas y finalmente ejecutando el comando `git pull`. Si relacionas esto con lo aprendido te daras cuenta que desde ahora todo es igual.

## Enviar a tus remotos
- Envia la informacion de una rama local al servidor (repositorio remoto) que este referenciado en el repositorio local.
~~~
git push [remote-name] [branch-name]
~~~
> Si has configurado que una rama local siga a una rama remota especifica, no es necesario realizar esta especificacion, basta con `git push`. De hecho, cuando clonamos un repositorio, git hace automaticamente esta configuracion, por ello solo ejecutamos comandos como `git pull` y `git push`

## Inspeccionar un remoto
- Muestra informacion acerca de un remoto en particular.
~~~
git remote show [remote-name]
~~~

## Eliminar y renombrar remotos
- Renombra y/o elimina el repositorio remoto respectivamente.
~~~
git remote rename [back-name] [new-name]
git remote rm [remote-name]
~~~

## Dudas existenciales
- Que pasaria si en un contexto controlado, dos colaboradores intentan hacer exactamente los mismo movimientos y cambios en el codigo, es posible que el hash sea el mismo (?)
