
# Tema 2 - Instalación, primeros pasos y operaciones con ficheros y directorios (Ubuntu)


## 2.1 Instalación y Conceptos básicos

Vamos a instalar [Ubuntu 22.04 ](https://ubuntu.com/download/desktop) en una máquina virtual. 
El software de virtualización que vamos a usar es [VirtualBox](https://www.virtualbox.org).

Realizaremos el proceso de instalación en clase utilizando las guías de instalación proporcionadas en "Tema2 Instalación": 
- **"Intalación de virtualBox"** 
- **"Instalación de Ubuntu 22.04"**

Cuando la instalación termine, accederemos al sistema mediante nuestras credenciales de acceso (**usuario y contraseña**) y si las credenciales son correctas, el sistema nos proporcionará una ***shell***. 

### Shell o interprete de comandos

Una ***shell*** es un *intérprete de comandos*, es decir, es un programa que permite a los usuarios interactuar con el Sistema Operativo, procesando las órdenes que se le indican. 

La shell puede interpretar comandos internos (corresponden en realidad a órdenes interpretadas por la propio shell) o comandos externos (correspondientes a ficheros ejecutables externos al shell). Además, las shells ofrecen otros elementos para mejorar su funcionalidad, tales como variables, funciones o estructuras de control. El conjunto de comandos internos y elementos disponibles, así como su sintaxis, dependerá del shell concreto empleado.

### Bourne shell (sh)
En los S.O.’s basados en unix **existen múltiples implementaciones de shell** (en Windows, el equivalente serían los programas "command.com" o "cmd.exe"). **Entre las shells de Unix destacaremos "sh"** que fue usado desde las primeras versiones y de la que han partido diferentes versiones que podriamos identificar como de la misma familia. 

**sh** (Bourne Shell): recibe ese nombre por su desarrollador, Stephen Bourne, de los Laboratorios Bell de AT&T. A raíz de él han surgido múltiples shells, tales como zsh (Z shell), ash (almquist shell), bash (Bourne again shell), dash (Debian almquist shell) o ksh (Korn shell). 

Bourne Shell **ha llegado a convertirse en un estándar de facto** de tal modo que todos los sistemas Unix tienen, al menos, una implementación del Bourne Shell (o un shell compatible con él), ubicada en `/bin/sh`. 

En el caso concreto de **los S.O.’s Linux, no existe ninguna implementación del Bourne Shell, manteniéndose la entrada  `/bin/sh` (así como su manual man sh) como un enlace simbólico a una implementación de shell compatible**.

### bash
bash: fue desarrollado para ser un superconjunto de la funcionalidad del Bourne Shell (en la que incluye funcionalidades de ksh y csh), **siendo el intérprete de comandos asignado por defecto a los usuarios en las distribuciones de Linux**, por lo que es el shell empleado en la mayoría de las consolas de comandos de Linux. Se caracteriza por una gran funcionalidad adicional a la del Bourne Shell. Como ficheros personales de los usuarios emplea `$HOME/.bashrc` y `$HOME/.profile`.


### Prompt
La shell **puede utilizarse desde la línea de comandos**, basada en el ***prompt*** como la indicación del shell para anunciar que espera una orden del usuario, **o puede emplearse para la interpretación de shell-scripts**. Un shell-script es un fichero de texto que contiene un conjunto de comandos y órdenes interpretables por el shell.

Un prompt puede tener un aspecto como este:

`rpalacios@localhost ~ $ `

    - **rpalacios**: es el nombre del usuario
    - **localhost**: es el nombre de la máquina.
    - **~** : hace referencia al directorio de trabajo del usuario (también llamado "home"), es decir, el directorio en el que nos encontramos por defecto.
    - **$**: es el final del prompt. A partir de aquí escribiremos nuestros comandos. Para el usuario **root**, el símbolo es __#__ en lugar de __$__.

Si queremos terminar nuestra sesión ejecutamos:

```bash
exit
```

**Una forma rápida de acceder al Terminal desde Ubuntu Desktop es mediante la combinación de teclas:**

 ```bash
Ctrl + Alt + T
```


## 2.2 Primeros pasos con ficheros y directorios

### Sistema de Ficheros

Una característica muy importante de todos los sistemas operativos basados en UNIX es que **todos los ficheros y directorios así como dispositivos del sistema se pueden tratar como si fueran ficheros.** Igualmente, cuando queramos acceder al contenido de un CD, disquete o cualquier otro dispositivo de almacenamiento, deberemos montarlo en un directorio ya existente en el sistema y navegaremos por él como si se tratara de una carpeta más.

En los SO basado en UNIX, y por tanto en Linux, todo el sistema de ficheros **parte de una misma raíz, a la cual nos referiremos con el carácter “/ ”**. 

Es el origen de todo el sistema de ficheros y sólo existe una. Para organizar los ficheros adecuadamente, el sistema proporciona lo que llamaremos directorios (o carpetas), dentro de las cuales podemos poner archivos y más directorios. De este modo conseguimos una organización jerárquica en modo de árblo como la que vemos en la siguiente figura:


![Arbol de Directorios](https://github.com/rpmaya/u-tad-redesYsistemasOperativos/blob/main/img/Arbol_de_Directorios.jpg)



Cuando entramos en el sistema, el login nos sitúa en nuestro directorio home, **este directorio se referencia con el carácter "~".** 

En todos los directorios **existe una entrada “.” y otra “..”** 

**El punto "." es la referencia al directorio actual**, mientras que **los dos puntos seguidos hacen referencia al directorio inmediatamente superior** (en el árbol de jerarquías).  Cuando estamos situados en la raíz del sistema de ficheros, la entrada “..” no existirá porque nos encontramos en el nivel superior. 



### Nuestros primeros comandos

#### ls: Veamos cómo ver el contenido de un directorio:

```bash
ls

rpalacios@rpalacios-VirtualBoxUb1:~$ ls
Descargas   Escritorio  Música      Público  Vídeos
Documentos  Imágenes    Plantillas  snap

# Y para ver ficheros ocultos:
ls -a

rpalacios@rpalacios-VirtualBoxUb1:~$ ls -a
.              .config     .local      .sudo_as_admin_successful
..             Descargas   Música      .vboxclient-clipboard.pid
.bash_history  Documentos  Plantillas  .vboxclient-display.pid
.bash_logout   Escritorio  .profile    .vboxclient-draganddrop.pid
.bashrc        .gnupg      Público     .vboxclient-seamless.pid
.cache         Imágenes    snap        Vídeos


```
Veamos otro ejemplo:

```bash
ls -l /home/rpalacios

total 40
drwxr-xr-x 2 rpalacios rpalacios 4096 sep  8 19:48 Descargas
-rw-rw-r-- 1 rpalacios rpalacios    0 sep 21 18:55 doc_rpalacios
drwxr-xr-x 2 rpalacios rpalacios 4096 sep  8 19:48 Documentos
drwxr-xr-x 2 rpalacios rpalacios 4096 sep  8 19:48 Escritorio
drwxr-xr-x 2 rpalacios rpalacios 4096 sep  8 19:48 Imágenes
-rw-rw-r-- 1 rpalacios rpalacios    0 sep 21 18:56 lab_SSOO
lrwxrwxrwx 1 rpalacios rpalacios   14 sep 21 18:55 manual.txt -> muydificil.txt
drwxrwxr-x 2 rpalacios rpalacios 4096 sep 21 18:54 MisDocumentos
drwxr-xr-x 2 rpalacios rpalacios 4096 sep  8 19:48 Música
-rw-rw-r-- 1 rpalacios rpalacios    0 sep 21 18:53 paraleer.txt
drwxr-xr-x 2 rpalacios rpalacios 4096 sep  8 19:48 Plantillas
-rw-rw-r-- 1 rpalacios rpalacios    0 sep 21 18:52 prueba
drwxr-xr-x 2 rpalacios rpalacios 4096 sep  8 19:48 Público
drwxr-xr-x 3 rpalacios rpalacios 4096 sep  9 18:35 snap
drwxr-xr-x 2 rpalacios rpalacios 4096 sep  8 19:48 Vídeos


___________ ___ ____  _____   ____ ____________ ______________
     |       |    |     |       |       |              |
	 |       |    |     |       |       |              -> Nombre
	 |       |    |     |       |       |
	 |       |    |     |       |       -> Fecha de modificación
	 |       |    |     |       |
	 |       |    |     |       -> Tamaño en bytes
	 |       |    |     |
	 |       |    |     -> Nombre del grupo con permisos sobre el fichero
	 |       |    |
	 |       |    -> Nombre del Usuario propietario del fichero
	 |       |
	 |       -> Número de enlaces duros al archivo/directorio
	 |
	 -> Permisos del fichero:
	   - Tipo de fichero (regular, directorio, enlace, etc).
	   - Permisos del propietario ( r w x )
	   - Permisos del grupo ( r - x )
	   - Permisos para el resto de usuarios ( r - - )


Nota: La segunda columna es el número de enlaces duros al archivo. Para un directorio, el número de enlaces duros es el número de subdirectorios inmediatos que tiene más su directorio padre y él mismo.

```

En general, los comandos pueden tener: 
- Argumentos
- Opciones

Y en función de eso generan una salida.
 
En general, las opciones pueden escribirse de dos formas: una forma larga, y una forma abreviada. Por ejemplo, estos dos comandos hacen lo mismo:

```bash
ls -a

ls --all
```

Las sigientes opciones nos listan todos los detalles de los ficheros (long list) o de forma recursiva los ficheros que existen en los directorios

```
ls -l

ls -R

```

Usando la notación abreviada, podemos agrupar varias opciones de forma cómoda:

```bash
ls -laR

ls -la
```

#### history: Comando muy útil que nos muestra el historial de comandos:

```bash
history
```

Para recuperar un comando del Historial de comandos --> En el terminal, pulsar las teclas UP y DOWN

 

#### pwd: Nos permite saber en qué directorio estamos ahora mismo

```bash
pwd

/home/rpalacios

```

**Nota**: Como se observa, las rutas en Linux se van componiendo usando la barra '/' (*slash*). En sistemas Windows, las rutas se expresan usando la barra invertida '\\' (*backslash*). Por eso en Windows las rutas son del tipo 'C:\\Usuarios\\Jacinto Flores'.

**Ejercicio**: 
Jugar con *ls* para ver qué hay en mi directorio y en mis directorios ascendentes.


#### cd: Comando utilizado para cambiar de directorio 

**Importante** para el uso de `cd`:

- El directorio raíz `/` (*root directory*)
- ~ referencia al directorio home
- . referencia al directorio actual
- .. referencia al directorio padre

Asi como tener en cuenta las Rutas absolutas y Rutas relativas

```bash
cd /etc

# Vamos a comprobarlo:
pwd

/etc
```

**Algunas formas de usar del comando cd:**

Para volver al directorio home (el inicial) hay dos opciones
````
cd 
cd ~
````
*Nota*: Para acceder a la virgulilla en Linux, pulsar Alt + ñ o Alt + 4 (esto último tambien sirve para Windows)

Ir al directorio raiz
````
cd /
````


Volver al directorio anterior
````
cd -
````

Subir un nivel en la jerarquía de directorios
````
cd ../
cd ..
````

Subir dos niveles en la jerarquía de directorios mediante cd ../ ../
````
rpalacios@Ubuntudesk01:~$ pwd
/home/rpalacios
rpalacios@Ubuntudesk01:~$ cd ../../
rpalacios@Ubuntudesk01:/$ pwd
/
rpalacios@Ubuntudesk01:/$ 
````

### Tipos de Ficheros 

Es un sistema sensible a mayúsculas y minúsculas (*case-sensitive*).

¡Cuidado con los espacios y las tildes en los nombres de los ficheros y directorios!

  - Comillas dobles y simples: las comillas simples, se utilizan para obtener un valor literal; en cambio las comillas dobles se utilizan para obtener un valor interpretado.
  - Escape de caracteres \

.

Ya hemos visto que todos los directorios y ficheros en Linux son ficheros, pero entonces, ¿todos los ficheros son iguales? La respuesta es que no. Hay diferentes tipos de ficheros. Estos son algunos comandos que nos ayudaran a distinguirlos: 

- **file**:  muestra el tipo de fichero en cuanto a su categoría en el sistema de archivos (es decir, tipo de archivo y formato)
- **type** indica que tipo de "comando" es el que estamos utilizando: alias, programa incluidos en la shell...
- **which**: indica donde se encuentra el archivo binario de un ejecutable o prgrama. En el modo estándar, which responde con el primer archivo que encuentra. Si se desea, la opción -a muestra todos los archivos que cumplen con el criterio de búsqueda.

```bash
file /etc
file /etc/hosts
file /bin/cp

type /etc
type ls
type cp
type type

which cp
```


## 2.3 Variables de entorno

Veamos otro comando, un tanto peculiar:

```bash
echo $PATH

/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin

```

Mmm, probemos de nuevo:

```bash
echo $SHELL

/bin/bash
```

El comando **echo** nos permite ver el contenido de las `variables de entorno`.

### Que son las variables de entorno
Las variables de entorno son valores dinámicos que afectan los programas o procesos que se ejecutan en un servidor. Existen en todos los sistemas operativos y su tipo puede variar. Las variables de entorno se pueden crear, editar, guardar y eliminar.

En Linux, **las variables de entorno permiten al sistema pasar datos a los programas iniciados en shells (intérpretes de comando) o sub-shells.**

`printenv` comando linux que muestra el valor de todas las variables de entorno (en el ejemplo siguiente no se muestran todas).

```
rpalacios@rpalacios-VirtualBoxUb1:~$ printenv
SHELL=/bin/bash
SESSION_MANAGER=local/rpalacios-VirtualBoxUb1:@/tmp/.ICE-unix/1239,unix/rpalacios-VirtualBoxUb1:/tmp/.ICE-unix/1239
QT_ACCESSIBILITY=1
COLORTERM=truecolor
XDG_CONFIG_DIRS=/etc/xdg/xdg-ubuntu:/etc/xdg
XDG_MENU_PREFIX=gnome-
GNOME_DESKTOP_SESSION_ID=this-is-deprecated
GTK_IM_MODULE=ibus
QT4_IM_MODULE=ibus
GNOME_SHELL_SESSION_MODE=ubuntu
SSH_AUTH_SOCK=/run/user/1000/keyring/ssh
XMODIFIERS=@im=ibus
DESKTOP_SESSION=ubuntu
SSH_AGENT_PID=1116
GTK_MODULES=gail:atk-bridge
PWD=/home/rpalacios
LOGNAME=rpalacios
XDG_SESSION_DESKTOP=ubuntu
XDG_SESSION_TYPE=x11
GPG_AGENT_INFO=/run/user/1000/gnupg/S.gpg-agent:0:1
XAUTHORITY=/run/user/1000/gdm/Xauthority
GJS_DEBUG_TOPICS=JS ERROR;JS LOG
WINDOWPATH=2
HOME=/home/rpalacios
USERNAME=rpalacios
IM_CONFIG_PHASE=1
LANG=es_ES.UTF-8
```

Cada línea contiene el nombre de la variable de entorno seguido de = y el valor. Por ejemplo:

```bash
HOME=/home/rpalacios
```

### Comandos para ver el valor de una variable de entorno
Para ver el valor de una variable de entorno podemor usar `printenv` pasando el nombre de esa variable como argumento a dicho comando o usar el comando `echo` visto anteriormente. EL resultado es el mismo

```bash
printenv HOME
echo $HOME

rpalacios@rpalacios-VirtualBoxUb1:~$ printenv HOME
/home/rpalacios
rpalacios@rpalacios-VirtualBoxUb1:~$ echo $HOME
/home/rpalacios
rpalacios@rpalacios-VirtualBoxUb1:~$ 

```

### Cómo crear una variable de entorno
La sintaxis básica de este comando se vería así:

```bash
export VAR="value"
``` 

- export: el comando utilizado para crear la variable.
- VAR: el nombre de la variable.
- = indica que la siguiente sección es el valor.
- «value»: el valor real

### Cómo revertir el valor de una variable

Usaremos el comando `unset`. La sintaxis del comando es la siguiente:

```bash
unset VAR
```

Las partes del comando son:

- unset: el comando en sí
- VAR: la variable cuyo valor queremos revertir

### Variable PATH
La variable PATH es una variable de entorno que contiene el listado de rutas o "paths" donde Linux busca los ejecutables de los comandos o programas a ejecutar.

```bash
echo $PATH

Retornará algo como esto:

/usr/local/bin:/usr/bin:/bin:/usr/games/bin:/usr/local/sbin:/usr/sbin:/sbin:/usr/lib/qt/bin
```

Cada ruta de directorio aparece separada por dos puntos (:). 

En caso de que queramos agregar directorios a nuestra variable PATH podemos hacerlo de dos formas: 
- La primera los hace válidos por el tiempo que dura tu sesión 
- La segunda los hace permanentes.

Para el primer caso basta escribir en el terminal:

```bash
PATH="$PATH:<ruta nueva1>:<ruta nueva2>:...<ruta nuevaN>"
```

**Supongamos que tenemos un usuario quiere agregar dos carpetas a su PATH**, ambas contenidas en ~/ (directorio home): la carpeta "Scripts" y la carpeta "Compilados". Tendría que escribir en el terminal:

```
PATH="$PATH:/home/nombreusuario/Scripts:/home/nombreusuario/Compilados” 
```

De esa manera **ambos directorios se agregan a su variable PATH. Sin embargo, estos cambios no son permanentes y la próxima vez que el usuario acceda a su cuenta todas las modificaciones se habrán perdido**.


Para que los cambios **sean permanentes es necesario editar un par de archivos: ~/.bash_profile y ~/.bashsrc**. Son los ficheros de configuración de `bash`y los veremos más adelante. 

 

# 2.4 Ayuda para usar la terminal de comandos

### Algunos trucos:

  - TAB Completion --> Escribir en el terminal "ls /et" y pulsar la tecla TAB (tabulador).
  - Historial de comandos --> En el terminal, pulsar las teclas UP y DOWN, e intentar recuperar un comando anterior del historial de comandos (incluso se puede modificar).
  - CTRL + R --> Búsqueda de comandos anteriores en el historial.
  - CTRL + L --> Borra la pantalla (equivale al comando `clear`).

Practiquemos un poco...

### Páginas del manual

Como en Linux hay muchos comandos, y es difícil recordar todas las opciones y argumentos de cada uno, existen unas páginas de ayuda, llamadas **manual** del sistema:

```bash
man ls

# Para salir de una página del manual, pulsamos la tecla 'q' (quit).
```

Dentro de una página del manual se pueden hacer búsquedas:
- / --> busca una cadena 
- n --> va a la siguiente aparición de la cadena

También se puede buscar una palabra en todas las páginas del manual:

```bash
man -k inode

# Busca en todas las páginas del manual la palabra "inode".
```

Podemos buscar más información sobre todos los comandos que hemos usado hasta ahora:

```bash
man exit

man ls

man echo

man history

man pwd

man cd

man file

man which

# :-)
man man
```



## 2.5 Más sobre Ficheros y Directorios

### Directorios

Un directorio en linux es un fichero especial, que puede contener otros ficheros y sub-directorios.

El comando file nos indicará si un fichero es un directorio

```bash
file /etc
```

### Directorios importantes
Ya lo hemos visto al ver el comando `ls`, lo volvemos a recordar: 
- El directorio raíz `/` (*root directory*)
- El directorio de trabajo del usuario con el que nos hemos registrado "~"
Rutas relativas: 
- "." referencia al directorio actual en el que me encuentro 
- ".." referencia al directorio padre al actual o directorio inmediatamente superior al directorio en el que me encuentro


### `cd` : Cambiar de directorios
Cuando usamos el comando cd le podemos pasar como parámetro la ruta a dónde queremos movernos en el árbol de directorios, y dicha ruta se le puede indicar de forma absoluta o de forma relativa. 

De **forma relativa significa que partiremos del directorio donde estamos** en el momento de ejecutar el comando y de forma **absoluta siempre partiremos de la raíz “/”.**

**Por ejemplo:** si estamos en el directorio /usr/bin/ y queremos ir al /etc/, deberíamos introducir el siguiente comando: 

```
#Forma relativa: 
cd ../../etc
los dos primeros puntos indican /usr/ y los siguientes la raíz “/” del sistema, a partir de la cual ya podemos acceder a /etc/
 
#Forma absoluta: 
cd /etc

#Para saber en qué directorio estamos, podemos utilizar el comando 
pwd

```

```bash
cd /home/rpalacios
pwd
man cd
```

### `mkdir` : Creación de directorios

```bash
cd ~
mkdir projects
ls -la projects

# ¿Que pasa si hacemos:?
mkdir projects/games/super_pang

# La opción -p crea los padres (ancestros) si no existen
mkdir -p projects/games/super_pang
mkdir -p projects/games/counter_strike
ls -laR projects

# Practiquemos:

cd projects/games/
pwd
cd ../
pwd
cd ~
pwd
cd projects/games
pwd
cd ../..

man mkdir
```


### `rmdir` : Borrado de directorios

```bash
cd ~

# Probemos el siguiente comando
rmdir projects/games

# rmdir SOLO FUNCIONA SI EL DIRECTORIO ESTÁ VACIO
rmdir projects/games/counter_strike
rmdir projects/games

# Para borrar directorios que no estén vacios se utiliza `rm` 
mkdir projects/games/counter_strike
rm -rf projects


man rmdir
```


### Directorios del Sistema

En la mayoría de distribuciones basadas en GNU/Linux se siguen unas recomendaciones para la distribución de directorios que son propios del sistema operativo, encontrando los siguientes directorios principales: 

| Directorio     | Descripción                                        |
| :------------- | ------------------------------------------------ |
|/             | Es el directorio raíz, todos los demás directorios cuelgan de este directrio. Suele ser el directorio donde se monta el directorio principal. |
|/root/        | Directorio home para el root del sistema. |
|/home/nombre_usuario/        | Directorio para poner las carpetas de los usuarios. |
|/boot/        | Archivos estáticos necesarios para el arranque del sistema. | 
|/bin/         | Aquí están localizados los ficheros binarios ejecutables del sistema y comandos básicos para todos los usuarios del sistema. |
|/sbin/        | Aquí se guardan solo los ejecutables del sistema que debe ejecutar únicamente el usuario root. |
|/etc/         | Archivos de configuración de las aplicaciones y servicios instalados en el sistema. |
|/lib/         | Librerías esenciales para el núcleo del sistema y módulos del mismo. |
|/dev/         | Este directorio no existe realmente en el disco duro, contiene todos los dispositivos detectados desde que el sistema está arrancado.  |
|/mnt/ y /media/ | Puntos de montaje temporal para dispositivos. |
|/proc/        | No existe realmente en el disco y se monta desde la memoria RAM. Contiene información del HW donde esta corriendo el sistema e información sobre los procesos y variables del núcleo del sistema. |
|/opt/  |Directorio en el que se pueden instalar aplicaciones. |
|/tmp/         | Archivos temporales. Según la distribución utilizada (o la configuración que utilicemos) se borran al arrancar el sistema o cada cierto período de tiempo. Es un directorio cargado en memoria. |
|/usr/         | Segunda estructura jerárquica, utilizada para almacenar todo el software instalado en el sistema al que pueden acceder la mayoría de los usuarios. Dentro tiene directorios como "bin", "lib", etc. en los que se guardan ficheros y librerías que no son del sistema. |
|/var/         | Directorio para ficheros variables como los trabajos de impresión, ficheros de log, etc. Es muy recomendable conservar y no eliminar. |
|/sys          | Archivos del sistema y de dispositivios HW. Muy parecido a /proc. Sólo existe en memoria. |
|/srv          | Puede contener ficheros y directorios para servir a otros sistemas. |
|/lost+foud    | "perdido y encontrado". Se guardan ficheros recuperados debajo de /. | 

### Ficheros 

### `touch` : Creación y actualización de ficheros

```bash
cd ~
touch projects/description.txt
touch projects/Description.txt


man touch
```
### `cat` : Mostrar el contenido de un fichero

```bash
cat /etc/profile


man cat
```

### `less` : Mostrar el contenido de un fichero, permitiendo interacción dinámica
Less muestra el fichero en modo interactivo de forma que se puede interactuar pero no editar ni modificar.

```bash
less /etc/profile

- ESPACIO (ir al final)
- ENTER (avanza una linea)
- b o Ctrl-b Scroll backward (hacia atras) el tamaño de una ventana
- d o Ctrl-D Scroll forward (hacia adelante) el tamaño de la ventana
- G (ir al final)
- r o Ctrl-R refresca la pantalla y descarta lo que tuiera en el buffer.  Útil si el fichero puede estar cambiando mientras se visualiza
-  /palabra (destacar plalabra)
- n (repite la última búsqueda)
- q (salir)

man less
```

### `tail` : Mostrar las últimas 10 líneas de un fichero
- -n muestra las últimas n líneas del fichero
- -f muestra las últimas líneas del fichero en tiempo real (para visualiazción de cambios en el fichero)

```bash
tail /etc/rsyslog.conf
tail -n 5 /etc/rsyslog.conf
tail -f /var/log/syslog


man tail
```

### `head` : Mostrar las primeras líneas 10 de un fichero (cabecera)

```bash
head /etc/rsyslog.conf
head -n 5 /etc/rsyslog.conf


man head
```

### `echo` : Imprimir en la consola y en ficheros

```bash
cd

# Imprimir un texto en la salida estándar (típicamente la consola):
# opción -e: habilita la interpretación de los saltos backslah '\'

echo "aprendemos Linux!"
echo "saltos de línea \n\n\n"
echo -e "saltos de línea \n\n\n Bien!"

# Imprimir un texto en un fichero (sobreescribe el fichero, 
# y lo crea si no existía):
echo "nuevo contenido" > mi_fichero.txt

# Por defecto, echo incluye un carácter de final de línea, que en Linux es '\n'.
# Se puede omitir la inserción de un carácter '\n' al final de la líne (-n)
echo -n "El prompt aparecerá justo a la derecha de este texto"


man echo
```

### `ln` : Enlaces simbólicos

**Son parecidos a los accesos directos de Windows (los que solemos tener en nuestro escritorio).**

Un link simbólico es un “enlace o puente” a un archivo o directorio perteneciente al sistema; una referencia que podemos poner en cualquier sitio y que actúa como un acceso directo a cualquier otro.  

Este mecanismo nos permite acceder a carpetas o archivos de forma más rápida y cómoda, sin tener que desplazarnos por la jerarquía de directorios o evitar la necesidad de tener archivos duplicados cuando queremos utilizarlos desde varios directorios. **Si borrásemos el fichero destino, el enlace no apuntaría a ninguna parte.**


```bash
# Veamos si tenemos algun enlace simbólico:
ls -l /boot

ls -l /lib
```

Veamos cómo crear enlaces (sintaxis):

**ln -s \<destino> \<enlace_simbolico>**

- Siendo \<destino> el nombre del fichero existente al que se crea el enlace 
- Siendo \<enlace_simbolico> nombre del enlace simbólico


```bash
cd ~

touch document.txt
mkdir softlinks
cd softlinks

#Crea un enlace simbolico al fichero que se le pasa como primer argumento. 
ln -s ../document.txt

# Crea un enlace simbólico(arg2) al fichero que se pasa en como arg1
ln -s /home/rpalacios/document.txt ./my_document.txt


# NOTA: no se pueden usar rutas relativas en el segundo argumento. Como norma general y para no cometer errores se aconseja ejecutar el comando ln -s siempre en el directorio en el que se quiere crear el enlace. 

ls -l
man ln

# AVISO: No se pueden crear enlaces simbólicos desde el S.O. invitado en las carpetas compartidas creadas con VirtualBox. 
# La opción está deshabilitada por defecto por motivos de seguridad

```

### Wildcards 
Los **_wildcards_** (o comodines) nos permiten seleccionar nombres de fichero basado en patrones de caracteres.

| Wildcard       | Significado                                        |
| :------------- | :------------------------------------------------ |
|  *             |  Selecciona cualquier conjunto de caracteres.      |
|  ?             |  Selecciona un carácter cualquiera.                |
|  [caracteres]  |  Selecciona cualquier carácter que esté incluido en un conjunto de caracteres específicos.<br>Pueden expresarse como una *clase de caracteres POSIX* |
|  [!caracteres] |  Selecciona cualquier carácter que **NO** esté incluido en un conjunto de caracteres específicos. También pueden expresarse como una *clase de caracteres POSIX* |

- **Nota: POSIX** (Portable Operating System Interface) define un conjunto de estándares especificados por el IEEE para varias interfaces de herramientas, comandos y API para garantizar la compatibilidad entre sistemas operativos.

.

Estas **Clases de caracteres POSIX** son las siguientes:


| Clase          | Significado                                         |
| :------------- | :------------------------------------------------- |
|  [:alnum:]     |  Selecciona caracteres alfanuméricos.               |
|  [:alpha:]     |  Selecciona caracteres alfabéticos.                 |
|  [:digit:]     |  Selecciona caracteres numéricos.                   |
|  [:upper:]     |  Selecciona caracteres alfabéticos en *MAYÚSCULAS*. |
|  [:lower:]     |  Selecciona caracteres alfabéticos en *minúsculas*. |
|  [:xdigit:]    |  Selecciona dígitos hexadecimales [0-9A-Fa-f].      |
|  [:space:]     |  Selecciona espacio, tabulador, tabulador vertical, retorno de carro, salto de línea. |
|  [:blank:]     |  Selecciona espacio y tabulador.                    |
|  [:print:]     |  Selecciona caracteres imprimibles.                 |
|  [:punct:]     |  Selecciona símbolos de puntuación: ! ' # S % & ' ( ) * + , - . / : ; < = > ? @ [ / ] ^ _ { | } ~ |
|  [:word:]      |  Selecciona cadenas de caracteres alfanuméricas, incluyendo los guiones bajos (*underscores*) |
|  [:ascii:]     |  Selecciona cualquier caracter ASCII (rango 0-127). |
|  [:cntrl:]     |  Selecciona caracteres de control, es decir, cualquier carácter que no pertenenzca a ninguna de las clases de caracteres POSIX que aparecen en esta tabla. |
|  [:graph:]     |  Selecciona cualquier caracter imprimible que no pertenezca a la clase [:space:]. |

.
 
Veamos algunos ejemplos para seleccionar nombres usando patrones de búsqueda y *wildcards*:

| Patrón                          | Significado                                         |
| :------------------------------ | :------------------------------------------------- |
|  *                              |  Selecciona todos los nombres de fichero.                                                                   |
|  g*                             |  Selecciona los que empiezan con el carácter 'g'.                                  |
|  b*.txt                         |  Selecciona los que empiezan con el carácter 'b' y terminan con la cadena ".txt".  |
|  Data???                        |  Selecciona los que empiezan con la cadena "Data" y tienen exactamente 3 caracteres más.  |
|  [abc]*                         |  Selecciona los que empiezan con el carácter '**a**', o con el '**b**' o con el '**c**', seguidos de cualquier otra cadena.  |
|  [[:upper:]]*                   |  Selecciona los que empiezan con una MAYÚSCULA seguida de cualquier otra cadena.  |
|  BACKUP.[[:digit:]][[:digit:]]  |  Selecciona los que empiezan con la cadena "BACKUP." seguida por exactamente dos números.  |
|  *[![:lower:]]                  |  Selecciona cualquier fichero que no termine en una letra en minúscula.  |



### `cp` : Copiar ficheros (y directorios)

```bash
# Copia simple de ficheros (orgien -> destino):
cd ~
touch flower.txt

cp flower.txt flower_ORIG.txt
cp /etc/rsyslog.conf ~/rsyslog_FOR_REVIEW.conf
cp /etc/rsyslog.conf ~
ls -l
cat syslog_FOR_REVIEW.conf

# ¡CUIDADO! cp sobreescribe el fichero si ya existía:
cp /etc/fstab ~/rsyslog_FOR_REVIEW.conf
ls -l
cat syslog_FOR_REVIEW.conf

# Podemos usar la opción -i para pedir confirmación al usuario:
cp -i /etc/fstab ~/rsyslog_FOR_REVIEW.conf

# Copia un fichero a un directorio destino. El fichero en el destino mantiene el nombre del fichero original:
mkdir copias
cp /etc/rsyslog.conf ./copias/

# También podemos especificar un nombre para el fichero destino:
cp /etc/rsyslog.conf ./copias/rsyslog_COPY.conf

ls -l

# Copia un directorio (y todo su contenido) en otro directorio:
cp -R copias copias_2

ls -laR copias_2

# Podemos copiar muchos ficheros cuyos nombres sigan un patrón (usando wildcards):
touch presupuesto_1.txt
touch presupuesto_2.txt
touch presupuesto_3.txt

mkdir documentos_de_texto

cp *.txt documentos_de_texto


man cp
```

### `mv` : Mover/Renombrar ficheros y directorios

```bash
# Como Renombrar ficheros:
cd
touch plato1.txt

# Renombro un fichero
mv plato1.txt plato2.txt

# Como Mover ficheros o directorios a otro directorio:
touch tipos_de_cubiertos.txt
mkdir recetas
mkdir cosas_cocina

ls -l

#Muevo un fichero a un directorio
mv tipos_de_cubiertos.txt cosas_cocina

#Muevo un directorio a otro directorio
mv recetas cosas_cocina

ls -l
ls -l cosas_cocina

# Mover varios ficheros/directorios a la vez:
touch tipos_de_sartenes.txt
touch tipos_de_cazuelas.txt
touch tipos_de_vajilla.txt

ls -l

mv tipos_de_sartenes.txt tipos_de_cazuelas.txt tipos_de_vajilla.txt cosas_cocina

ls -l
ls -l cosas


man mv
```

### `rm` : Eliminar ficheros (y directorios)

```bash

# Borrar ficheros:
touch document_01.txt
touch document_02.txt
touch document_03.txt
touch document_04.txt

ls -l

rm document_01.txt

ls -l

rm document_0?.txt

ls -l

# Borrar directorios:
mkdir rpalacios

# Para directorios vacios:
rmdir rpalacios

mkdir rpalacios
touch rpalacios/datos.txt

ls -l rpalacios

# Para directorios que tienen ficheros debemos usar -r:
rm -r rpalacios

ls -l rpalacios
ls -l

# ¡CUIDADO! Cuando borramos algo con rm, se borra para siempre.


man rm
```

## 2.6 Comandos para Empaquetar y Comprimir ficheros y directorios


- **Empaquetar**: Es agrupar en un solo fichero varios ficheros y/o directorios.
- **Comprimir**: Es reducir el tamaño de un fichero a través del uso de un algoritmo de compresión.


#### Empaquetar (.tar)
Con el comando **tar**, (tape archive) se pueden empaquetar y desempaquetar archivos. Para que haya compresión es necesario utilizarlo en combinación con programas tales como gzip o bzip. Este comando se puede utilizar con múltiples opciones, aunque hay algunas que quizá son importantes recordar.

| Opción    | Significado                                        |
| :-------: | :---------------------------------------------- |
|-c         |Crear un nuevo archivo .tar                       |
|-v	        |(verbose) Muestra una descripción detallada del progreso de la compresión |
|-f         |Nombre del archivo |
|-z         |Compresión gzip |
|-j	        |Compresión bzip2 |
|-C	        |Extrae archivos en un directorio diferente |
|-x	        |Extraer el archivo |
|-r	        |Actualizar o agregar un archivo o directorio en un archivo .tar existente |


Se pueden utilizar el comando .tar tanto para un archivo como para directorios.

```bash
cd
echo "Mi primer fichero" > file_1.txt
echo "Mi segundo fichero" > file_2.txt
echo "Mi tercer fichero" > file_3.txt

# Archiva o empaqueta varios ficheros/directorios en un fichero .tar
tar cvf mis_ficheros.tar file*.txt

ls -l
```

#### Comprimir (.tgz, .tar.gz, .bz2)

 
Si lo que queremos además es comprimir archivos tenemos que utilizar `**tar**` con un método de compresión (gzip o bzip)


**gzip**: Comprime varios ficheros/directorios en un fichero con extensión .tgz o .tar.gz
La compresión GZIP es la compresión de archivos estándar en sistemas Unix/Linux y es un proceso de dos pasos: primero se usa el programa tar para unir todos los archivos a comprimir en uno solo, sobre el que luego se usa el compresor gzip. Esta secuencia es tan común que el comando tar incluye una opción para comprimir directamente el archivo al finalizar. La opción es -z con el comando tar. 

```bash 
tar cvzf mis_ficheros_comprimidos.tgz file*.txt

Donde: 
-z Comprime el archivo usando gzip
-c Crea un archivo
-v Verbose, escribe en pantalla información sobre el proceso de compresión
-f Nombre del archivo

ls -l

# También se puede comprimir un directorio en un fichero .tgz o .tar.gz
mkdir mi_directorio
cp file*.txt mi_directorio

tar cvzf mi_directorio_comprimido1.tgz mi_directorio

ls -l
```

**bzip**: Comprime varios ficheros en un fichero .bz2
.bz2 proporciona más compresión en comparación con  gzip. Sin embargo, esta alternativa tomará mas tiempo para comprimir y descomprimir. Para usarla, debemos usar la opción -j con el comando tar

```bash 
tar cvjf mi_directorio_comprimido2.bz2 mi_directorio

ls -l
```


#### Desempaquetar y descomprimir con el comando .tar

```bash
# Extraer el contenido de un fichero .tar
tar xvf mis_ficheros.tar

# Extraer el contenido de un fichero .tgz o .tar.gz (GZIP)
tar xvzf mis_ficheros_comprimidos.tgz

# Extraer el contenido de un fichero .bz2 (BZIP2)
tar xvjf mi_directorio_comprimido2.bz2

# Listar el contenido de un fichero .tar
tar tvf mis_ficheros.tar

man tar
info tar
```

### Más sobre compresión y descompresión de ficheros (`zip`, `gzip`, `bzip2`, `tar`)

```bash
# Preparamos unos ficheros de prueba
cd
mkdir -p "$HOME/docus"
cd "$HOME/docus"
echo "Este es el documento 1" > doc1.txt
echo "Este es el documento 2" > doc2.txt
echo "Este es el documento 3" > doc3.txt
echo "Este es el documento 4" > doc4.txt
echo "Este es el documento 5" > doc5.txt

cat doc1.txt doc2.txt doc3.txt doc4.txt doc5.txt > big_doc.txt
cp big_doc.txt  big_doc_to_gzip.txt
cp big_doc.txt  big_doc_to_bzip2.txt

pwd
ls -l

```

### Comandos para comprimir `zip`, `gzip`, `bzip2`, `tar` 
#### zip
Algunas veces es necesario que el archivo sea descomprimido en otros sistemas operativos, en ese caso es útil usar el formato zip que tiene mayor compatibilidad (el más usado en Windows)

```bash
# Sintaxis: zip <archivo comprimdo> <archivo(s) a comprimir>
zip docus.zip doc[[:digit:]].txt
ls -l
```

#### gzip
Es uno de los métodos más utilizados para comprimir en Linux, pero puede utilizarse también en sistemas Windows y macOS.

```bash
# Sintaxis: gzip [opciones] <archivo(s) a comprimir>
# La opción -k mantiene el fichero original 
# gzip doc[[:digit:]].txt --> si se le pasan varios ficheros comprime cada uno de los archivos

gzip big_doc_to_gzip.txt

# Vemos que se ha generado el archivo: big_doc_to_gzip.txt.gz
ls -l

```


#### bzip2
Permite comprimir archivos en Linux sin pérdidas con una gran calidad. Los archivos empaquetados con bzip2 obtienen la extensión .bz2.

```bash
# Sintaxis: bzip2 [opciones] <archivo(s) a comprimir>
# La opción -k mantiene los archivos despues del proceso de compresión 
# bzip2 doc[[:digit:]].txt --> si se le pasan varios ficheros comprime cada uno de los archivos

bzip2 -k doc[[:digit:]].txt
# Vemos que se ha generado un fichero .bz2 por cada archivo y que se han mantenido los originales
ls -l 

bzip2 big_doc_to_bzip2.txt
# Vemos que se ha generado el archivo big_doc_to_bzip2.txt.bz2
ls -l

```


#### tar
```bash
# El comando tar se ha visto antes, pero se deja aquí una referencia rápida.

# tar usando gzip (opción z)
tar zcvf docus.tgz doc[[:digit:]].txt 
tar zcvf docus.tar.gz doc[[:digit:]].txt 
ls -l

# tar usando bzip2 (opción j)
tar jcvf docus.tar.bz2 doc[[:digit:]].txt
ls -l

```


### Comandos para descomprimir `unzip`, `gunzip`, `bunzip2`, `tar` 

```bash
# Usaremos los ficheros de prueba empleados al comprimir.
cd "$HOME/docus"
```

#### unzip
```bash
unzip docus.zip 
ls -l
```

#### gunzip (equivalente a: gzip -d)
```bash
gzip -d big_doc_to_gzip.txt.gz
gunzip big_doc_to_gzip.txt.gz
ls -l
```

#### bunzip2 (equivalente a bzip -d)
```bash
bzip2 -d big_doc_to_bzip2.txt.bz2
bunzip2 big_doc_to_bzip2.txt.bz2
ls -l
```

#### tar 
```bash
# El comando tar para descomprimir también se ha visto antes, pero se deja aquí una referencia rápida.

# tar usando gunzip 
tar zxvf docus.tgz 
tar zxvf docus.tar.gz 
ls -l

# tar usando bzip2 
tar jxvf docus.bz2
ls -l


man unzip
man gunzip
man bunzip2
man tar
```

#### Listar el contenido de un fichero comprimido, se usan estos comandos.
```bash 
unzip -l docus.zip
gzip -l doc1.gz
tar ztvf docus.tgz
tar ztvf docus.tar.gz
tar jtvf docus.tar.bz2
```

