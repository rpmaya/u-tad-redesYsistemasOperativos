# Tema 6 - Gestión básica de usuarios y permisos

Este tema trata sobre la gestión de usuarios y grupos de usuarios en sistemas
Ubuntu. Pero la teoría y filosofía de trabajo es aplicable a cualquier sistema
Linux. También describe quién es el usuario *root*.

En los sistemas Linux, todo lo que un usuario puede o no puede hacer con un
fichero está determinado por permisos y grupos. Si se dispone de los permisos 
necesarios, o si se pertenece al grupo con permisos necesarios, se podrá
acceder a un fichero o directorio.

## Permisos de ficheros y directorios

```bash
ls -l /etc/passwd
-rw-r--r-- 1 root root 1395 sep 22 01:14 /etc/passwd

ls -ld /etc/
drwxr-xr-x 102 root root 8192 oct 13 05:21 /etc/

```

Los listados anteriores muestran información sobre el propietario y los permisos
de los ficheros. Hagamos un repaso:

```texte

drwxr-xr-x 102 root  root    8192 oct 13 05:21  /etc/
__________     ____  _____                      
     |           |     |                           
     |           |     -> Grupo con permisos sobre el fichero                           
     |           |     
     |           -> Usuario propietario del fichero
     |           
     |
     -> Permisos del fichero:
       - Tipo de fichero (regular, directorio, enlace, etc).
       - Permisos del usuario/propietario ( r w x )
       - Permisos del grupo ( r - x )
       - Permisos para el resto de usuarios ( r - x )

```


Así que los permisos se agrupan para Usuario, Grupo y Otros usuarios:

- **owner/user** (propietario/usuario): Es el usuario que creó y posee un archivo/directorio.
- **group** (grupo): Todos los usuarios que son miembros del mismo grupo.
- **others** (otros): Todos los demás usuarios del sistema que no son propietarios ni miembros del grupo.

```text

-  r w x     r - x     r - x
   _____     _____     _____
     |         |         |
     |         |          -> Permisos para Otros usuarios
     |         |          
     |         -> Permisos para el Grupo propietario
     |
     -> Permisos para el Usuario propietario
     
```

Normalmente, estos permisos se representan como 3 números en base octal. Para el
ejemplo anterior, los permisos serían 755:

```text
-  r w x     r - x     r - x
   1 1 1     1 0 1     1 0 1
   \___/     \___/     \___/
     7         5         5

Otro ejemplo:

-  r w -     r - -     r - -
   1 1 0     1 0 0     1 0 0
   \___/     \___/     \___/
     6         4         4
```

A modo de recordatorio, la siguiente tabla muestra la equivalencia entre valores
expresados en base 2 (binarios) y valores expresados en base 8 (octal):

| Octal  | Binario   | Permisos   |
| :----- | :-------: | :--------: |
|  0     |  0 0 0    |  - - -     |
|  1     |  0 0 1    |  - - x     |
|  2     |  0 1 0    |  - w -     |
|  3     |  0 1 1    |  - w x     |
|  4     |  1 0 0    |  r - -     |
|  5     |  1 0 1    |  r - x     |
|  6     |  1 1 0    |  r w -     |
|  7     |  1 1 1    |  r w x     | 

-
Hay una pequeña diferencia entre el significado de los permisos para ficheros y
directorios:

- **Ficheros**
  - **r**: El fichero puede ser leído (se lee su contenido).
  - **w**: El fichero puede ser escrito/modificado (se escribe/modifica su 
    contenido).
  - **x**: El fichero puede ser ejecutado (el fichero es un programa o *script*).

- **Directorios**
  - **r**: Se puede listar el contenido del directorio (hacer un *ls*, por 
    ejemplo).
  - **w**: Se pueden crear o eliminar ficheros dentro del directorio.
  - **x**: Se puede acceder al directorio, y a sus subdirectorios (hacer un *cd*).

Hay que tener en cuenta que, cuando no se tiene permiso de lectura de un 
directorio, no se pueden mostrar (listar) sus contenidos, pero sí se puede, por
ejemplo, leer un fichero que esté dentro: tendríamos que usar la ruta absoluta
para leer un fichero dentro de un diretcorio que no podemos listar. Esto implica
que se debe conocer a priori el nombre completo del contenido de un directorio.

Además, aunque no se pueda leer (**r**) o escribir (**w**) en un directorio, 
siempre que se pueda acceder a él (**x**), se podrá trabajar con los ficheros
que contiene. Dicho de otra manera, los permisos de un directorio no son los
permisos de los ficheros (y sub-directorios) que contiene.

Recordatorio: al hacer un `ls -l` podemos ver de qué tipo es un fichero:

| Código   | Tipo de objeto                        |
| :------- | :-----------------------------------: |
|  -       |  Fichero regular.                     |
|  d       |  Directorio.                          |
|  l       |  Enlace simbólico.                    |
|  c       |  Dispositivo (especial) de carácter.  |
|  b       |  Dispositivo (especial) de bloque.    |
|  p       |  FIFO.                                |
|  s       |  Socket.                              |


### Cambiar los permisos de ficheros y directorios

Se usa la herramienta **`chmod`** (*change file mode bits*) para cambiar los
permisos. Los argumentos con los que se invoca a *chmod* son: 

  - ¿Para quién se cambian los permisos?: usuario, grupo, otros, todos. [**ugoa**].
  - ¿Se conceden permisos **[+]** o se revocan permisos **[-]**?
  - ¿Qué permiso se configura?: lectura, escritura y/o ejecución [**rwx**]

A continuación se muestran algunos ejemplos de permisos para ficheros:

```bash
cd
mkdir permisos
cd permisos
echo "Este es un fichero para jugar con chmod" > permisos.txt

# Vamos a cambiar algunos permisos

ls -l
# -rw-rw-r-- 1 elenagg elenagg 40 nov 10 11:26 permisos.txt

chmod o-r permisos.txt
ls -l
# -rw-rw---- 1 elenagg elenagg 40 nov 10 11:26 permisos.txt

chmod g+x permisos.txt
ls -l
# -rw-rwx--- 1 elenagg elenagg 40 nov 10 11:26 permisos.txt

chmod u-r permisos.txt
ls -l
# --w-rwx--- 1 elenagg elenagg 40 nov 10 11:26 permisos.txt


# También podemos cambiar varios permischmod os a la vez

chmod u+rx permisos.txt
ls -l
# -rwxrwx--- 1 elenagg elenagg 40 nov 10 11:26 permisos.txt

chmod go-wx permisos.txt
ls -l
# -rwxr----- 1 elenagg elenagg 40 nov 10 11:26 permisos.txt

chmod a-x permisos.txt
ls -l
# -rw-r----- 1 elenagg elenagg 40 nov 10 11:26 permisos.txt

chmod a+rw permisos.txt
ls -l
# -rw-rw-rw- 1 elenagg elenagg 40 nov 10 11:26 permisos.txt

```

También es común usar los valores expresados en base octal para cambiar los
los permisos:

```bash
chmod 755 permisos.txt
ls -l
# -rwxr-xr-x 1 elenagg elenagg 40 nov 10 11:26 permisos.txt

chmod 750 permisos.txt
ls -l
# -rwxr-x--- 1 elenagg elenagg 40 nov 10 11:26 permisos.txt

chmod 644 permisos.txt
ls -l
# -rw-r--r-- 1 elenagg elenagg 40 nov 10 11:26 permisos.txt

chmod 650 permisos.txt
ls -l
# -rw-r-x--- 1 elenagg elenagg 40 nov 10 11:26 permisos.txt

chmod 666 permisos.txt
ls -l
# -rw-rw-rw- 1 elenagg elenagg 40 nov 10 11:26 permisos.txt

```

Y para comprender mejor cómo funcionan los permisos en los directorios, veamos algunos ejemplos:

```bash
cd
ls -l permisos
# -rwxr-xr-x 1 elenagg elenagg 40 nov 10 11:26 permisos.txt

ls -ld permisos
# drwxrwxr-x 2 elenagg elenagg 4096 nov 11 20:34 permisos/

chmod 400 permisos
ls -ld permisos
# Modificamos el permiso del directorio ("r") y vemos que podemos hacer
# dr-------- 2 elenagg elenagg 4096 nov 11 20:34 permisos/

cd permisos/
# No puedo acceder al directorio (no hay permiso "x" para el usuario)
# bash: cd: permisos/: Permiso denegado

ls permisos/
# Sí Puedo listar su contenido
# ls: no se puede acceder a permisos/permisos.txt: Permiso denegado
# permisos.txt

chmod 100 permisos
ls -ld permisos
# Modificamos ahora el directorio y le damos permiso "x". Vemos que pasa...
# d--x------ 2 elenagg elenagg 4096 nov 11 20:34 permisos/

ls permisos/
# No puedo listar su contenido
# ls: no se puede abrir el directorio permisos/: Permiso denegado

cd permisos
# Sí puedo acceder al directorio pero no puedo listar el contenido

ls
# ls: no se puede abrir el directorio .: Permiso denegado

pwd
#/home/elenagg/MisPruebas/permisos
# Puedo accder a sus ficheros siempre que conozca el nombre completo
cat permisos.txt
# Este es un fichero para jugar con chmod


man chmod
```

### Propietarios de un fichero o un directorio

**Un fichero pertenece a un usuario** y a un **grupo**. Los usuarios y los 
grupos tienen asociados un identificador numérico, llamados *Id. de usuario* (**UID**) e *Id. de grupo* (**GID**).

**Los usuarios del sistema están definidos en los ficheros** `/etc/passwd` y `/etc/shadow`. En este último estan encriptadas las contraseñas de los usuarios y su periodo de vigencia y sólo tiene acceso el usuario root. 

**Los grupos están definidos en los ficheros** `/etc/group` y `/etc/gshadow`. En este último se guardan los usuarios que están dados de alta dentro de cada grupo de usuarios asi como los admisnitradores de cada grupo y sólo tiene permiso de acceso el usuario root. 

Veamos algun ejemplo: 

```bash
cat /etc/passwd | grep elenagg 
# El usuario elenagg aparece en una línea

elenagg:x:1000:1000:Elena GG,,,:/home/elenagg:/bin/bash

```

```bash
cat /etc/group | grep elenagg
# El usuario elenagg aparece en varias líneas:

adm:x:4:syslog,elenagg
cdrom:x:24:elenagg
sudo:x:27:elenagg
dip:x:30:elenagg
plugdev:x:46:elenagg
lpadmin:x:120:elenagg
lxd:x:131:elenagg
elenagg:x:1000:
sambashare:x:132:elenagg
vboxsf:x:998:elenagg

```

- En el primero ejemplo, se puede ver que el usuario *elenagg* tiene *UID = 1000* y *GID = 1000*. 

- En el segundo ejemplo se puede ver que el grupo *vboxsf* tiene *GID = 998* y que el usuario *elenagg* pertenece al dicho grupo *vboxsf*.


### Cambiar el propietario y el grupo de un fichero o directorio

Comando `chown`

**Sintaxis**: 
chown [owner][:group] [file name]

```bash
su -

# Crearemos antes un grupo para poder ver los ejemplos (el comando de creación de grupos se ve más adelante)
groupadd developers

# Cambia el usuario y el grupo de un fichero o directorio
chown elenagg:developers /home/elenagg/license.txt
chown elenagg:developers /home/elenagg/projects/

# Cambia solo el grupo
chown :developers /home/elenagg/license.txt

# Cambia solo el usuario
chown lola /home/elenagg/license.txt

# Cambia el usuario y el grupo de un directorio y todo su contenido
chown -R elenagg:developers /home/elenagg/projects

```

## Algunos comandos previos sobre el "usuario"

Antes de nada, como usuario de un sistema Linux: ¿quién soy yo? ¿a qué grupos pertenezco? Ambas preguntas se responden con estos comandos:

```bash
# ¿Qué usuario soy yo?
whoami

# ¿A qué grupos pertenezco?
groups

# Obteniendo toda la información a la vez
id

# UID
id -u

# Nombre del usuario (loguin)
id -un

# GID
id -g

# Nombre del grupo
id -gn

man id
```

### Ficheros de Usuarios: /etc/passwd y /etc/shadow

### /etc/passwd
Cuando se crea un usuario en el sistema, se alamcena la siguiente información en el fichero `/etc/passwd`. Esta información son 7 campos, separados por caracteres (`:`). 
 
```bash
username:x:UID:GID:comment:home_directory:login_shell
    1    2  3   4     5        6               7

root:x:0:0:root:/root:/bin/bash
elenagg:x:1000:1000:Elena GG,,,:/home/elenagg:/bin/bash

```

Los campos son:

1. **Id. de usuario** (se almacena en las variables $USER o $LOGNAME de la *shell*.)
2. **Contraseña** cifrada (o una x que indica que se usa `/etc/shadow`)
3. **UID** (código numérico del usuario)
4. **GID** (código numérico del grupo primario) - Aunque un usuario puede pertenecer a más de
   un grupo.
5. **Comentarios**: por ejemplo el nombre completo, o el nombre de la empresa, etc.
6. **Directorio home** (ruta absoluta): normalmente */home/$USER*.
7. ***Shell*** del usuario cuando hace *login*: por ejemplo, */bin/bash*.

Normalmente, el fichero */etc/passwd* puede ser leido por todos los usuarios, pero sólo el usuario *root* puede modificar su contenido.

Si el usuario tiene una **x**, quiere decir que su contraseña cifrada se 
encuentra almacenada en el fichero */etc/shadow*. Este fichero sólo puede ser 
leído y modificado por *root*.


### Añadir usuarios `adduser`

Al añadir un usuario se crea su directorio personal en */home*. Por ejemplo, para
la usuaria *peppa*, se creará el directorio */home/peppa*. Dentro de */home/peppa*,
se copia una plantilla (que se encuentra en */etc/skel*).

`adduser` **Sintaxis básica:**

sudo adduser [--home DIR] [--shell INTÉRPRETE] [--no-create-home] [--uid ID][--ingroup GRUPO | --gid ID] USUARIO

```bash
# Crea el usuario y el grupo primario de dicho usuario, con la shell por defecto y con el directorio del usuario por defecto en /home/<usuario>/
sudo adduser peppa

#Crea un usuario y lo añade al grupo developers (tiene que existir)
# "ingroup" es el grupo por defecto que se crea cuando cras un usuario. Si no se especifica con esta opción el grupo que se crea toma el mismo nombre que el usuario
sudo adduser --ingroup developers peppa

# Crea un usuario indicando cual sera su directorio de trabajo
sudo adduser --home /home/usu_peppa peppa

# Añade un grupo existente a un usuario ya existente
# Por ejemplo, para un grupo llamado developers se haría:
sudo adduser peppa developers

# Para no crear un directorio de usuario en /home (--no-create-home)
sudo adduser --no-create-home peppa

# Para crear un usuario o cuenta de sistema (--system)
# Por omisión, los usuarios  del  sistema  se  añaden  al  grupo nogroup.
# Para  añadir el nuevo usuario del sistema a un grupo existente, usar las opciones --gid [GID] o --ingroup [GRUPO]. 
# Para añadir  el nuevo  usuario  del sistema a un grupo con su mismo ID, use la opción --group.
sudo adduser --system peppa


man adduser / adduser --help


```

### Cambiar la contraseña `passwd`

Un usuario puede **modificar su contraseña usando el comando** `passwd`.

 **Sintaxis**:

passwd [opciones] [USUARIO]

- "-d": borra la contraseña
- "-e": hace expirar la contraseña para que en el siguiente loguin el usuario solicite cambiar la password

```bash
# Para cambiar la contraseña de un usuario en particular, se ejecuta:
sudo passwd elenagg

# Se puede borrar la contraseña de un usuario
sudo passwd -d peppa

# Hace expirar la contraseña de un usuario
sudo passwd -e peppa

man passwd / passwd --help
man shadow
```


### Eliminar usuarios `userdel`

```bash
# Elimina un usuario, pero conserva sus ficheros en /home/peppa
sudo userdel peppa

# Elimina un usuario, y elimina además el directorio /home/peppa. También se elimina al usuario de los grupos a los que pertenecía.
sudo userdel -r peppa


man userdel / userdel --help
```

### Añadir un usuario a un grupo `usermod` 
**Sintaxis:** 

usermod [opciones] USUARIO

Normalmente, sólo un usuario con privilegios puede añadir  otro usuario a uno o más grupos

```bash
su -

# Añade un usuario a dos grupos
usermod -aG developers,testers elenagg

# También se puede usar el comando
gpasswd -a elenagg ungrupo

# Añadir varios usuarios a un grupo
gpasswd -M user1,user2,user3 ugrupo

# Elimina un usuario de un grupo
gpasswd -d user2 ungrupo


man usermod
man gpasswd
```

### Ficheros de Grupos: /etc/gpasswd y /etc/gshadow

Un grupo es un conjunto de usuarios que comparten los mismos permisos.

Hay dos tipos de grupos:

- Primario
- Secundarios

En Linux, la información relativa a los grupos está en los ficheros 
`/etc/group` y `/etc/gshadow`.

### /etc/group

Cada grupo está definido en una línea del fichero */etc/group*. Cada línea consta de 4 campos (separados por `:`)

```bash
cat /etc/group | grep elenagg

# El grupo elenagg aparece en varias líneas:

adm:x:4:syslog,elenagg
cdrom:x:24:elenagg
sudo:x:27:elenagg
dip:x:30:elenagg
plugdev:x:46:elenagg
lpadmin:x:120:elenagg
lxd:x:131:elenagg
elenagg:x:1000:
sambashare:x:132:elenagg
vboxsf:x:998:elenagg

 1     2  3   4

```

1. **Nombre del grupo**.
2. **Contraseña cifrada** (**x** indica que la contraseña se guarda en /etc/gpaswd).
3. **GID**.
4. Lista de **miembros** (separados por `,`).

### Añadir grupos `groupadd`

```bash
groupadd developers

man groupadd

```

### Modificar grupos `groupmod`

```bash
# Se puede modificar el GID
groupmod -g 1003 developers


# Se puede modificar el nombre
group -n nuevo_nombre_gr antiguo_nombre_gr
groupmod -n desarrolladores developers

man groupmod

```

### Eliminar grupos `groupdel`

Un grupo no se podrá eliminar si tiene algún miembro.

```bash
groupdel developers

man groupdel

```

## El usuario root

El usuario ***root*** es un super-usuario que puede hacer todo lo que quiera en el 
sistema: es el usuario con más privilegios. Esto le permite poder mantener y
administrar el sistema.

Normalmente, sólo el dueño de un fichero/directorio puede modificar los permisos
del mismo. Y normalmente un usuario sólo tiene total libertad dentro de su *home*.

Sin embargo, el usuario *root* puede modificar los permisos de cualquier fichero
del sistema, incluso aunque no sea el propietario.

De igual forma que podemos acceder al sistema con nuestro usuario, también 
podemos acceder al sistema como *root*, para tener todos los privilegios.

Si ya hemos accedido como un usuario pero queremos convertirnos en *root*, 
podemos usar el comando **su**.

```bash
su -
```

También se usa *su* para convertirnos en otro usuario existente en el sistema:

```bash
su - lola
```

Si el usuario root no está activo ejecutar sudo -i (Esto nos permitirá acceder como root, y una vez hecho ejecutar el comando para cambiar su passwd: sudo passwd root)

```bash
sudo -i

sudo passwd root
```

### El comando sudo

**Permite ejecutar comandos con permisos de *root* o super usuario**. En Ubuntu, para que un usuario pueda ejecutar comandos con *sudo*, debe pertenecer al grupo *sudo*.

Sólo un usuario con privilegios puede añadir a otro usuario al grupo de *"sudoers"*.
Para gestionar los usuarios que pueden usar *sudo*, se usa el comando *visudo*.

Este comando abre un editor (vi) **para editar el fichero */etc/sudoers*.** En el caso de Ubuntu se abre nano.

```bash
su -
# Añadir un usuario al grupo "sudoers"
usermod -aG sudo lola
```


#### Gestion de usuarios "sudoers": `visudo`

 Como root, ejecutamos:
```bash
visudo
```

Al usar *visudo*, se debe buscar una línea similar a la siguiente:

```text
%sudo        ALL=(ALL)       ALL
```

Si la línea estuviese comentada (comienza por #), habrá que descomentarla.


Para que los cambios tengan efecto, se debe salir de la sesión y volver a
entrar, o también se puede abrir una nueva *shell* con el usuario.



### Otra forma alternativa de añadir un usuario a /etc/sudoers

En lugar de añadir el usuario al grupo *sudo* también se puede añadir el usuario usando visudo, e insertando una línea similar a esta:

```text
elenagg        ALL=(ALL)        ALL
```

## Ejercicios

- Haced completa la [Actividad 8](https://github.com/rpmaya/u-tad-redesYsistemasOperativos/blob/main/gestionUsuariosYPermisos/Actividad8_Comandos_Linux_Tema6_enunciados.markdown)

## Para ampliar

El tema de gestión de usuarios da para varias asignaturas, pero tras esta
primera introducción, se puede buscar información acerca de **suid**, **sgid**, **sticky bit**, **ficheros inmutables**.

Mucha de la configuración relativa a usuarios y grupos se guarda en el fichero `/etc/login.defs` :)

`umask` El comando umask, es la abreviatura de user file-creation mode mask, y sirve para establecer los permisos por defecto que tendrán los nuevos ficheros y directorios que creemos.

## Algunos recursos útiles

- [Permissions - Ryans Tutorials](https://ryanstutorials.net/linuxtutorial/permissions.php)
- [Permissions - IBM Developer](https://developer.ibm.com/tutorials/l-lpic1-104-5/)
