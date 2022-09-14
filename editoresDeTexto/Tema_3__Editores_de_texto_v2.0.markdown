# Tema 3 - Editores de texto (nano y vim)

En este tema se ven muy brevemente los editores de texto `nano` y `vim`. Se han elegido estos dos porque son muy comunes, y suelen venir preinstalados en las distribuciones más populares (por ejemplo, *nano* es el editor por defecto en distribuciones Debian o derivadas, como Ubuntu) y vim lo es en las distribuciones Red Hut y derivadas, como CentoS. La idea de este tema es tener unas nociones básicas que nos permitan editar nuestros propios *scripts*.

Si tecleamos vim en Ubuntu, nos devuelve un mensaje de que no está instalado, y nos indica como instalarlo !!:

````
No se ha encontrado la orden «vim», pero se puede instalar con:

sudo apt install vim         # version 2:8.1.2269-1ubuntu5, or
sudo apt install vim-tiny    # version 2:8.1.2269-1ubuntu5
sudo apt install neovim      # version 0.4.3-3
sudo apt install vim-athena  # version 2:8.1.2269-1ubuntu5
sudo apt install vim-gtk3    # version 2:8.1.2269-1ubuntu5
sudo apt install vim-nox     # version 2:8.1.2269-1ubuntu5

````


Para instalar vim en Ubuntu hay que ejecutar:

```bash
sudo apt install vim
```

## 3.1 nano

`nano` es un sencillo editor de texto. Se puede iniciar *nano* con un nuevo fichero o se puede editar un fichero ya existente:

```bash
# Inicia nano con un nuevo fichero (inicialmente vacío)
nano

# Edita un fichero ya existente
nano /etc/crontab

# Edita un fichero y muestra los números de línea
nano -l /etc/crontab

Cuidado al editar este fichero. Al final de este Tema crearemos un script y practicaremos los editores. 
Con `Ctrl + X` nos salimos 

man nano
```

A continuación se especifican algunos de sus atajos de teclado (*shortcuts*) comunes, algunos de ellos aparecen en la parte inferior de la pantalla:

- `Ctrl + O`: Guarda el fichero que se está editando. Si no se especificó ningún fichero a editar, preguntará por el nombre de fichero que se quiere guardar.
- `Ctrl + X`: Sale de *nano*. Cuando hay cambios sin guardar, preguntará al usuario si desea guardar los cambios en el fichero. Y si no se especificó ningún fichero, preguntará por el nombre de fichero que se quiere guardar.
- `Ctrl + K`: Corta una línea, y la copia en el portapapeles.
- `Ctrl + U`: Pega el contenido del portapapeles.
- `Ctrl + W`: Busca un texto en el documento.q
- `Ctrl + G`: Muestra la ayuda básica del programa.
- `Ctrl + _`: Ir a la línea (pregunta por el número de línea). 
- `Alt + U`: Deshace la última edición.
- `Alt + 6`: Copia texto selecciondo
- `Ctrl + W` y sobre todo `Ctrl + _` son importantes para buscar lineas de script con fallos

## 3.2 vim

`vim` es un potente y completo editor de textos, que además permite ser ampliado mediante *plugins*. 

```bash
# Inicia vim con un nuevo fichero (inicialmente vacío)
vim

# Edita un fichero ya existente
Nota. ~ se obtiene en Windows se obtiene con Alt + 126 y en Linux con Alt Gr + ñ. Tambien se obtiene en ambos con  AltGr + 4

$ vim ~/.bashrc
Nota: El fichero .bashrc incluye la definicion de alias (por ejemplo la ='ls -A') y los colores usado en el Terminal de Ubuntu. 

man vim
```

El manejo de *vim* es más complejo que el de *nano*. *Vim* tiene dos modos de funcionamiento:

- Modo de comandos (**ESC**). Las teclas que pulsamos, en lugar de aparecer escritas en el documento, son interpretadas por Vim como comandos y nos permiten realizar acciones como grabar, salir, copiar, pegar, etc 
- Modo de inserción (**i**). Nos permite introducir caracteres en el fichero, en la posición actual del cursor, al estilo de los editores básicos a los que estamos acostumbrados

Al iniciar *vim*, se accede al modo de comandos. Para empezar a editar un fichero (y pasar al modo de inserción), se pueden pulsar diferentes teclas, como por ejemplo:

- **i**: **Insert** - Entra al modo de inserción (en el punto en el que esté el cursor).
- **a**: **Append** - Entra al modo de inserción (en el carácter siguiente al punto en el que esté el cursor).
- **s**: **Supress** - Entra al modo de inserción pero para suprimir (elimina el carácter en el que esté el cursor).

En la terminología de *vim*, lo que se ve en pantalla se denomina *buffer*. Por lo que cuando se guarda un fichero, en realidad se guarda el *buffer* en el fichero.

**Desde el modo de inserción se puede editar el fichero, y cuando se quiera realizar alguna acción, se debe volver al modo de comandos** (pulsando la tecla **ESC**).

Algunas de las operaciones básicas que se pueden hacer desde e**l modo de comando son**:

- **:w**: Guarda el fichero. Si no se especificó ningún fichero a editar, preguntará por el nombre de fichero que se quiere guardar.
- **:q**: Sale de *vim*. Cuando hay cambios sin guardar, preguntará al usuario si desea guardar los cambios en el fichero. Y si no se especificó ningún fichero, preguntará por el nombre de fichero que se quiere guardar.
- **:q!**: Sale de *vim*, forzando la salida, de tal forma que se perderán los cambios no guardados.
- **:númeroDeLínea**: Ir a la línea *númeroDeLínea*. Por ejemplo: *:4*, va a la línea número 4 del fichero, y sitúa el cursor en el primer carácter del fichero.
- **dd**: Corta la línea actual (y la copia en el portapapeles).
- **p**: Pega el contenido que está en el portapapeles, en la línea siguiente a la actual (desplaza el texto existente hacia abajo).
- **P**: Pega el contenido que está en el portapapeles, en la línea actual (desplaza el texto existente hacia abajo).
- **Y**: Copia la línea actual en el portapapeles.
- **/**: Se usa la barra */* seguida de un término de búsqueda, para buscarlo en el fichero.
- **n**: Mientras se está en una búsqueda, se desplaza a la siguiente ocurrencia del término de búsuqeda en el fichero.
- **N**: Mientras se está en una búsqueda, se desplaza a la anterior ocurrencia del término de búsuqeda en el fichero.
- **:%s/uno/otro/g**: Reemplaza el texto *uno* por el texto *otro* en todo el fichero.
- **u**: Deshace la última edición.

#### .vimrc

El fichero `$HOME/.vimrc` almacena la configuración de *vim* para un usuario en particular. En este fichero se puede, por ejemplo, activar el resaltado de código, especificar a cuántos espacios se traduce un tabulador, mostrar los números de línea, etc.

Si no encuentras el fichero .vimrc en tu directorio "home" puedes crearlo para particularizar y confgurar el uso de `vim` para tu usuario.

```bash 

# Activa la sintaxis coloreada según el tipo de fichero de texto.
syntax on

# muestra las coincidencias en los resultados de la búsqueda
set showmatch

# búsqueda sin importar mayúsculas y minúsculas
set ignorecase

# para ver marcados los resultados de una búsqueda
set hlsearch

# para ver los primeros resultados de la búsqueda mientras la estás escribiendo
set incsearch

# cambia los tabs por espacios
set expandtab

# un tab son 4 espacios
set tabstop=4

# Auto indenta, es decir, cuando se pulsa intro se pone el cursor debajo del principio de la línea de arriba.
set autoindent

# muestra el número de línea, útil para edición de código.
set number

#muestra el número fila y de columna en la línea de estado.
set ruler

```

Se deja aquí un ejemplo básico de fichero *.vimrc*:

```text
syntax on
set showmatch
set tabstop=2
set hlsearch
set ruler

```

### 3.3 Conceptos básicos para creacion del primer script

### Estructura básica de un Shell Script


Un shell-script puede ser un simple fichero de texto que contenga uno o varios comandos. Para ayudar a la identificación del contenido a partir del nombre del archivo, es habitual que los shell scripts tengan la extensión ".sh", por lo que seguiremos siempre este criterio.

```
mi_scrip.sh
```

La estructura básica de un shell-script es la siguiente:

```
#!/bin/bash                                       <-- Shebang
# Línea de Comentario: líneas no interpretables    <-- Comentarios
echo "Hola Mundo"                                  <-- Contenido del script
ls -l ~          
```

El "**shebang**" permite especificar el intérprete de comandos con el que deseamos que sea interpretado el resto del script. 
**La sintaxis de esta línea es la secuencia #! seguida del ejecutable del shell deseado** (ruta completa al ejecutable de la Shell). 

Importante:
- Es imprescindible que sea la primera línea del script, ya que, en caso contrario, sería interpretado como un comentario (comienza con el carácter #).
- Puede haber espacios entre #! y el ejecutable del "shell".
- El shebang no es obligatorio (en caso de que no aparezca se intentará usar el mismo tipo de shell desde el que se ha invocado el script). 


### Importante para poder ejecutar un script: 
Debe tener permisos de ejecución generales, si no los tuviese, para asignárselos bastaría ejecutar: 

```
chmod +x script.sh
```


### Vamos a crear nuestro primer script. Por simplicidad usamos nano. 

*Nota*: Todos los comandos que se teclean manualmente se pueden incluir en un script, para automatizar tareas repetitivas o realizar funciones.

No vamos a crear primero el "soso" script Hola_mundo.sh. Vamos a crear algo más potente y vistoso

`nano PCinfo_y_todos_los_ficheros_de_Linux.sh`

En nano escribimos:
````
#!/bin/bash
lscpu
#lscpu presenta la arquitectura de tu CPU, arquitectura (32/64 bits), modelo, número de procesadores, compatibilidad con virtualización y memoria caché L1, L2 y L3·
sleep 5
cat /proc/cpuinfo
# cpuinfo presenta toda la informacion de tu procesador
sleep 5
ls -lR \
````
Salvamos el fichero `Ctrl + O` y salimos de nano `Ctrl + X`

Para hacer que el script sea ejecutable (cambiar el modo a 'x') tecleamos:

``chmod a+x PCinfo_y_todos_los_ficheros_de_Linux.sh``

para ejecutarlo, necesario teclear delante del scrip "./":

./PCinfo_y_todos_los_ficheros_de_Linux.sh

**!!Importante, no asustarse para parar el script teclear Ctrl + C ** ;-)

Ahora escribimos:

````
echo "Hola Mundo" >Hola.txt
ls
````
Podemos ver como, en el listado, Hola.txt aparece en color blanco, porque no es un ejecutable y como  PCinfo_y_todos_los_ficheros_de_Linux.sh aparece en verde, al ser un fichero con permiso de ejecución. Los directorios aparecen en azul, como ya sabiamos.


#####Ejercicio en clase

Realizar el script ejecutable Hola_Mundo.sh que imprima  ```Hola Mundo``` y despues de 5 segundos imprima la configuracion IP de Ubuntu, mediante el comando ``ip a``



## Para ampliar

Nano es un editor sencillo, y vim es mucho más complejo, y dado que este tema es sólo una pincelada de ambas herramientas, se recomienda buscar información sobre ellos para sacarles todo el partido.

Opcionalmente, se puede investigar acerca de los (numerosísimos) *plugins* que existen para *vim*, o también buscar más información sobre cómo configurar el fichero *.vimrc*.
