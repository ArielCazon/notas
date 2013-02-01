# Anexo A: Introducción a git
git es un sistema de control de versiones. Se convertirá en nuestro DeLorean personal que nos permitirá viajar por la historia de los proyectos y nos proveerá grandes facilidades para el trabajo en equipo.

Será la herramienta principal con la que administraremos el trabajo, y su incumbencia excede este ámbito: hoy por hoy se ha convertido en el estándar de la industria a nivel global.

## ¿Qué es git?
> git es un sistema de control de versiones distribuído de código abierto, diseñado para la velocidad y eficiencia

### Distribuído
Cada copia de un repositorio git es tan válida como las otras, y puede comportarse como servidor para un equipo de trabajo o simplemente como reservorio del proyecto a modo local.

Esto presenta como ventaja, por ejemplo, la posibilidad de trabajar individualmente desconectado de todo tipo de redes, o en grupo, sin necesidad de tener un servidor centralizado.
*Cualquier nodo aislado es independiente*.

> Otros sistemas de control de versiones necesitan de la presencia de una red y de un servidor centralizado para poder trabajar, ver diferencias en archivos, acceder a la historia del proyecto y generar ramas de desarrollo.

### Código abierto
Si fuera de nuestro interés, podríamos acceder al [repositorio git](http://www.github.com/git/git) donde se encuentra el código de git (sí, git está administrado con git).

El proyecto fue iniciado por [Linus Torvalds](http://www.google.com/search?q=Linus+Torvalds) en 2005 dado su hartazgo por los sistemas de control de versiones de la época, y debido a que su proveedor de preferencia había discontinuado sus servicios.

Hoy en día tiene una legión de programadores alrededor del mundo y cualquiera con una buena experiencia en C y algo de tiempo libre puede mejorar el proyecto.

### Velocidad y eficiencia
Los repositorios de git son inmutables, por lo que (idealmente) nunca se borran datos. Adicionalmente, lo que se resguarda en el repositorio son snapshots de los archivos (y no diferencias a la línea base, como otros sistemas de control de versiones). Y ésto es lo que realmente le brinda mucha velocidad: cada versión guardada de nuestro proyecto es una captura completa (no 100% completa, pero ya veremos) del estado en ese momento.

> **Nota:** la explicación excede los objetivos de estas notas. Sin embargo se pude recurrir a la bibliografía para profundizar en el tema. El libro *Pro git*, de Scott Chacon es excelente.

Para darnos una idea del funcionamiento de git, imaginémonos una [Polaroid](http://polaroidstore.com/products/instant-cameras/14-megapixel-instant-print-digital-camera-z340e-black.htm) tomando fotos de nuestro trabajo, y archivándolas en un álbum bien ordenado cada vez que nosotros ejecutamos un simple comando.

## Manos a la obra: git básico
Para comenzar a trabajar con git necesitamos conocer unos pocos comandos, y una vez adquirida cierta práctica se convertirá en una tarea natural al momento de desarrollar un proyecto.

> **Nota:** se asume que git se encuentra correctamente instalado y configurado, tareas que no son objetivo de este apartado.

### Iniciando un repositorio
La tarea que menos veces haremos es la de crear repositorios, pero es esencial ya que sin ella no hay forma de que git comience a recolectar el estado por el que atravieza el proyecto a cada momento: no sabe que debe empezar a tomar las "fotos".

#### Desde cero
Una de las opciones es comenzar un repositorio vacío. Cuando trabajamos en un proyecto que creamos nosotros, generalmente es la forma de iniciar el repo git.  
Simplemente debemos posicionarnos en el directorio del proyecto y escribir `git init`.

    lucas@falcon:~/notas/demo-git$ pwd
    /home/lucas/notas/demo-git
    lucas@falcon:~/notas/demo-git$ git init
    Initialized empty Git repository in /home/lucas/notas/demo-git/.git/

Cuando hayamos obtenido la confirmación por parte de git de que se ha podido generar el repositorio, estamos listos para comenzar a trabajar.

#### Desde un repositorio ya existente
La otra forma es trayéndonos un repositorio existente, que hayan compartido con nosotros o que hayamos subido a algún servidor desde otra computadora y deseemos descargar en nuestra terminal de trabajo. En regla general, siempre que ya exista el repo, debemos importarlo (la primera vez) para poder trabajar en él.  
Esta vez necesitamos el comando `git clone`, pero deberemos especificarle cuál será la ruta del repositorio desde el cual clonarlo.

    lucas@falcon:~/notas$ git clone git@github.com:tallerweb/notas.git
    Cloning into 'notas'...
    remote: Counting objects: 69, done.
    remote: Compressing objects: 100% (60/60), done.
    remote: Total 69 (delta 21), reused 55 (delta 7)
    Receiving objects: 100% (69/69), 17.29 KiB, done.
    Resolving deltas: 100% (21/21), done.

En el ejemplo anterior, vemos que dentro de nuestro espacio de trabajo escribimos el comando adecuado, indicando la ruta del repo de origen y conseguimos una réplica del mismo en esa ubicación, dentro de un directorio llamado como el repo original.

> **Nota:** no debe crearse el directorio para el repositorio, ya que el comando clone lo hará automáticamente. Se puede especificar otra ruta al final del comando, pero esto es opcional.

### Sondeando el repo
Estamos listos para comenzar a tomar fotos del estado de nuestro proyecto. Sin embargo, para poder hacerlo, necesitamos empezar a trabajar y realizar modificaciones de archivos.  
Podemos dar cuenta de esto si ejecutamos el comando `git status`.

    lucas@falcon:~/notas/demo-git$ git status
    # On branch master
    #
    # Initial commit
    #
    nothing to commit (create/copy files and use "git add" to track)

El mensaje es muy elocuente: "nothing to commit". Dado que no hemos hecho modificaciones en el proyecto no habrá nada a lo que sacar nuevas fotos.

> **Nota:** el comando `git status` será invocado en forma frecuente, ya que nos presenta un reporte del estado actual del directorio de trabajo y de aquello que queremos commitear.

No nos queda más que hacer alguna modificación al repo, para poder comenzar a experimentar con git.

### Sacando fotos
Una vez que comenzamos a trabajar sobre el proyecto, y tenemos un conjunto de cambios que deseamos resguardar, podemos hacer un commit (análogo a "sacar una foto", pero empecemos a tomar el vocabulario). En todo proyecto es buena idea tener un archivo `README.md` que especifique indicaciones generales del mismo, y toda información relevante para futuros usuarios/desarrolladores del mismo. Nosotros ya hemos escrito ese README, por lo que el estado del proyecto será diferente.

    lucas@falcon:~/notas/demo-git$ git status
    # On branch master
    #
    # Initial commit
    #
    # Untracked files:
    #   (use "git add <file>..." to include in what will be committed)
    #
    #	README.md
    nothing added to commit but untracked files present (use "git add" to track)

La clave para comenzar a entender los reportes de estado de git es leer cuidadosamente los mensajes: siempre tienen una ayuda dentro. En este caso, nos avisa que hay archivos no trazados ("untracked"): son aquellos que git aún no conoce, ni ha guardado previamente en ningún commit.

Nuestro primer commit será simple, y utilizaremos la ayuda que nos proveyó el comando `git status`: recurriremos al comando `git add`.

    lucas@falcon:~/notas/demo-git$ git add README.md

Aún no recibiendo respuesta alguna por parte de este comando, podemos intuír que "si nada estalló por los aires, al menos no ha fallado". Veamos el estado de nuestro proyecto ahora mismo:

    lucas@falcon:~/notas/demo-git$ git status
    # On branch master
    #
    # Initial commit
    #
    # Changes to be committed:
    #   (use "git rm --cached <file>..." to unstage)
    #
    #	new file:   README.md
    #

Ahora es diferente: el mismo archivo aparece como "cambios a ser commiteados", es decir, aquello que saldrá en la próxima foto.

> **¡Un momento!** ¿No habíamos sacado ya la foto con el comando `git add`? No. El proceso de tomar fotos lleva dos pasos necesarios. Primero, deben acomodarse las personas que saldrán en la misma y separarse las que no. Luego, puede sacarse la foto.

Muy bien, procedamos a commitear y terminemos con esta primera etapa:

    lucas@falcon:~/notas/demo-git$ git commit -m "Commit inicial"
    [master (root-commit) 4a8a534] Commit inicial
    1 file changed, 3 insertions(+)
    create mode 100644 README.md

Lo que acabamos de ejecutar se compone del comando básico (`git commit`) y un argumento que será el mensaje asociado a ese commit (`-m "Commit inicial"`). Es buena práctica utilizar mensajes descriptivos. En este caso, y al ser el inicio del proyecto, es lo mejor que tenemos.

Repitamos la operación una vez más para obtener algo de práctica con la herramienta. Supongamos que ahora hemos **modificado** el *README.md*, y **agregado** un nuevo archivo llamado *index.html*. Nos encontramos ante uno de los casos más comunes: cada vez que trabajamos en una nueva funcionalidad o mejora, es áltamente probable que agreguemos y modifiquemos. Empecemos con `git status`

    # On branch master
    # Changes not staged for commit:
    #   (use "git add <file>..." to update what will be committed)
    #   (use "git checkout -- <file>..." to discard changes in working directory)
    #
    #	modified:   README.md
    #
    # Untracked files:
    #   (use "git add <file>..." to include in what will be committed)
    #
    #	index.html
    no changes added to commit (use "git add" and/or "git commit -a")

Obtenemos un mensaje más largo que lo habitual, pero muy completo. Básicamente dice que hay cambios en el archivo `README.md`, y que existe uno nuevo que aún no fue registrado en ningún commit anterior llamado `index.html`. Una nueva combinación de `git add` + `git commit` hará su trabajo.

    lucas@falcon:~/notas/demo-git$ git add README.md index.html
    lucas @falcon:~/notas/demo-git$ git commit -m "Se agrega página de inicio"
    [master 40dcdd8] Se agrega página de inicio
    2 files changed, 9 insertions(+)
    create  mode 100644 index.html
    lucas @falcon:~/notas/demo-git$ git status
    # On branch master
    nothing to commit (working directory clean)

Adicionalmente hicimos un `git status` para verificar el estado del proyecto una vez commiteados los archivos.

> **Nota:** No siempre deberemos agregar los archivos de uno en uno. Podemos utilizar el comodín "." para agregar todos los archivos (no recomendado) o el comando `git add -i` para tener un menú interactivo que nos permita elegir qué archivos agregar, actualizar o incluso qué fragmentos de archivo deseamos commitear (áltamente recomendado).

Ya hemos tomado práctica y podemos detenernos a mirar los avances.

### Mirando el álbum
Si deseáramos ver la historia de commits, podríamos recurrir a un comando simple:

    lucas@falcon:~/notas/demo-git$ git log
    commit  40dcdd852ed7904109afe3ee2a5b51781f6b9422
    Author : Lucas Videla <lucas@uno21.com.ar>
    Date :   Fri Feb 1 18:13:21 2013 -0300
     
         Se agrega página de inicio
     
    commit  4a8a534ed145f6f6c187a75af957be9e3479be17
    Author : Lucas Videla <lucas@uno21.com.ar>
    Date :   Thu Jan 31 18:53:20 2013 -0300
     
         Commit inicial

Podemos observar que cada commit tiene una estructura similar: un código alfanumérico (el SHA del commit, su identificador único), quién es el autor, la fecha en que se "tomó esa foto" y el comentario agregado por el autor.

> **Nota:** Ese código alfanumérico extenso tiene un nivel de colisión insignificante. Tal es así que si tomamos sus primeros 7 caracteres podemos prácticamente asegurar a qué commit nos referimos dentro del proyecto.  
> Con ese código indicaremos, en un futuro, un commit en particular.

Una forma alternativa es utilizar el comando `gitk`, que es un inspector gráfico de los commits. Se recomienda su uso cuando se desea ir viendo la evolución del proyecto, y no sólamente la información de los commits.

### Volviendo atrás
Muchas veces trabajamos en algo que no rinde sus frutos o que por diversas razones deseamos volver atrás. El comando `git reset` nos ayudará en esta tarea.

Empecemos viendo el estado del proyecto (luego de un cambio que no está especificado en este documento, y utilizando un [comando de log más sintético](http://uberblo.gs/2010/12/git-lol-the-other-git-log)):

    lucas@falcon:~/notas/demo-git$ git lol
    * ac2bb29 (HEAD, master) Un cambio que descartaremos
    * 40dcdd8 Se agrega página de inicio
    * 4a8a534 Commit inicial

En este caso, descartaremos el cambio que así lo indica. Para eso debemos comprender que nuestro repositorio especifica que el último commit realizado es el `ac2bb29` (podemos afirmarlo ya que junto a ese identificador se encuentra la palabra `master`, que no es más que un *puntero al último commit* de esa **rama**).  
Simplemente recurriremos al comando mencionado:

    lucas@falcon:~/notas/demo-git$ git reset --hard 40dcdd8
    HEAD is now at 40dcdd8 Se agrega página de inicio

En este momento HEAD, nuestro *puntero de trabajo actual*, nos dice que nos reposicionó en el commit deseado. Cuando hagamos el próximo commit, éste se ubicará por sobre el indicado, volviendo atrás el cambio descartable que introdujimos al principio del apartado. Por supuesto, de este modo se perderá ese commit para todos fines prácticos.

> **Nota:** la opción `--hard` del comando `git reset` nos indica que quedemos volver atrás el puntero HEAD, descartando los cambios de los archivos definitivamente.  
Si en su lugar hubiésemos empleado la opción `--soft`, descartaría el último commit pero los cambios se encontrarían presentes en el directorio de trabajo por si deseamos modificarlos o emplear una de ellos en el próximo commit.


### Ramas... ramas... ¡ramas!


### Mezclando


### Etiquetando


### Trabajando en equipo


## Resumen de comandos
* `git init`, para inicializar un repositorio nuevo en el directorio actual.
* `git clone ruta_repo`, para copiar un repositorio existente dentro del directorio actual. Creará un directorio nuevo para el repo.
* `git status`, para ver el estado de nuestro directorio de trabajo, y el stash. Nos permite ver qué se está por commitear, o qué falta agregar a stash.
* `git add nombre_de_archivos`, para agregar uno o más archivos al próximo commit. Se dice que luego del add se han pasado a stash.
* `git commit -m un_comentario`, para realizar efectivamente un commit con todo lo guardado en el área de stash. Dejará el stash limpio.
* `git log`, para tener un listado de los commits, ordenados en forma inversa según la fecha de captura.
* `gitk`, para acceder a un inspector gráfico de la evolución del proyecto.
* `git lol`, un alias para el comando `git lol` con muchas opciones y argumentos. Es necesario [configurarlo](http://uberblo.gs/2010/12/git-lol-the-other-git-log).
* `git reset --hard id_commit`, para volver atrás descartando los cambios realizados luego del commit especificado, eliminándolos definitivamente.
* `git reset --soft id_commit`, para volver atrás descartando los cambios realizados luego del commit especificado, dejándolos en el directorio de trabajo.

## Bibliografía
* **Chacon, Scott.** *Pro git.* Berkeley, CA: Apress, 2009
* **Chacon, Scott.** [Pro git](http://github.com/progit/progit/tree/master/es). Libro gratuito y en español.