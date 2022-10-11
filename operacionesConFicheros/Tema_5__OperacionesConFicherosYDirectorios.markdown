
# Tema 5 - Operaciones con ficheros y directorios


## `date`  y  `cal` : La fecha, hora y el calendario


### date 
```bash
# Mostrar la fecha y hora


#Muestra la fecha y la hora en formato CET
date

# Nuestra la fecha y la hora en formato UTC
date -u

# Muestra el año 
date +"%Y"

# Muestra el mes
date +"%m"

# Muestra el día, mes y año en el formato indicado
#El carácter %Y se reemplazará con el año, %m con el mes y %d con el día del mes.
date +"%d-%m-%Y"
date +"%d-%m-%Y %H:%M:%S"
date +"Año: %Y, Mes: %m, Día: %d"

# Establecer la fecha y la hora (necesita permisos de root y no tener la sincronización NTP activada)
sudo date --set 2020-10-02
sudo date --set 19:00:07
sudo date --set="20190701 14:30"
```

### cal
```bash
# Mostrar el calendario
cal    (mes actual)
cal -3 (muestra el mes anterior, mes actual, mes posterior)
cal -m 12 (calendario de diciembre o mes 12 del año actual)
cal -m 3 2019 (calendario del mes 3 del año 2019)
cal -y (calendario anual)
cal -y 2021  (calendario del 2021)
ncal   (formato vertical)
ncal -e (fecha de semana santa, -o semana santa ortodoxa)
ncal -w -3 (muestra el numero de la semana del mes anterior, el actual y el siguiente)

```
### timedatectl

```bash 
# Gestión de la fecha y hora

#El comando timedatectl muestra la hora actual del hardware (RTC) y la hora del reloj del sistema 
timedatectl

# timedatectl: establecer la fecha y la hora (sólo se puede hacer si no está establecida sincronización automática)
timedatectl set-time 22:53:48
timedatectl set-time "2019-10-02 19:00:07"
timedatectl set-time "2019-10-02"

# timedatectl: listar las zonas horarias
timedatectl list-timezones
timedatectl list-timezones | grep -i madrid
timedatectl list-timezones | grep -i europe

# timedatectl: establecer la zona horaria
timedatectl set-timezone Europe/Madrid

# Des/Activar la sincronización de la fecha y hora por NTP
timedatectl set-ntp no
timedatectl set-ntp yes
```

NTP (Network Time Protocol) es un protocolo que se basa en el envio de señales de sincronizacion horaria através de la red IP. Es el método más común para sincronizar el reloj del software de un sistema GNU/Linux con los servidores horarios de Internet.

###Otros comandos relacionados con el tiempo

```bash
#Comienzo,Estado, ficheros, PID, del servidor NTP 
systemctl status systemd-timesyncd.service

#Informacion sobre el servidor NTP
timedatectl timesync-status

man date
man cal
man timedatectl


```


## Recordamos algunos comandos sobre ficheros que también podemos usar a la salida de un comando

### `tail` : Mostrar las últimas 10 líneas de un fichero o de la salida de un comando
- -n muestra las últimas n líneas del fichero
- -f muestra las últimas líneas del fichero en tiempo real (para visualiazción de cambios en el fichero)

```bash

tail /etc/rsyslog.conf
tail -n 5 /etc/rsyslog.conf
tail -f /var/log/messages

# Últimas 2 líneas del comando ls -l
ls -l | tail -n 2


man tail

```

### `head` : Mostrar las primeras líneas 10 de un fichero (cabecera)o de un comando

```bash

head /etc/rsyslog.conf
head -n 5 /etc/rsyslog.conf

# Primeras 2 líneas del comando ls -l
ls -l | head -n 2

man head

```


### `truncate` : Como vaciar un fichero

```bash
# Con la opción -s establecemos el tamaño del fichero
history > mi_fichero.txt
ls -l 
truncate -s 0 mi_fichero.txt
ls -l 

# Y hay más formas...

#Redirigir "nada" a un fichero
> mi_fichero.txt

# echo sin ningún parametro y sin nueva linea al final (-n)
echo -n > mi_fichero.txt


#true  no  hace  nada  excepto  devolver  un valor de salida de 0, significando "éxito".
true > mi_fichero.txt


#/dev/null o «null device», es un archivo especial que descarta toda la información que se escribe en o se redirige hacia él. A su vez, proporciona ningún dato a cualquier proceso que intente leer de él, devolviendo simplemente un EOF o fin de archivo.
cp /dev/null mi_fichero.txt


man truncate
```

### `grep` : Búsqueda de textos en ficheros y a la salida de comandos

```bash
# Buscar una expresión en un fichero
grep "elenagg" /etc/group
grep "elenagg" /etc/passwd
grep $(whoami) /etc/passwd
```

#### sintaxis grep
```bash
grep -r busca recursiva
grep -n muestra numeros de línea
grep -i case insensitive (mayusculas o minusculas)
grep -c cuenta las veces que aparece una palabra dentro de un archivo
grep -v excluir una expresión de busqueda
grep -e -e busqueda de varias expresiones
grep "^cadena" buscar expresiones que empiecen por cadena
grep "cadena$" buscar expresiones que acaben en cadena


# Buscar una expresión en múltiples ficheros, de forma recursiva (-r)
cd /etc
grep -r "elenagg" *


# Buscar una expresión en un fichero mostrando los números de línea (-n)
grep -rn "elenagg" *
cat /etc/group | grep -n "elenagg"


# Buscar una expresión a la salida de otro comando
date --help
date --help | less

date --help | grep "%" (muestra los modificadores  de date %xxx )


# Nota previa: 
# Comando ps: muestra procesos en ejecución. Opciones: 
# a: muestra los procesos para todos los usuarios (todos los terminales)
# u: muestra información adicional
# x: muestra información de procesos sin terminal (demonios)

ps (muestra los procesos de usuario)
ps u (informacion de usuario extendida)
ps aux (a: todos los usuarios, x: procesos sin terminal -demonios-, u: extendida )

# Buscar sin considerar las MASYÚSCULAS/minúsculas (-i)
ps aux
ps aux | grep -i "Cron"

cat /etc/group | grep -ni "elenagg" | less


# Excluir una expresión en una búsqueda (-v)
# Ejecutaremos primero
ps aux | grep -i "cron"

# Y ahora: 
ps aux | grep -i "cron" | grep -v "grep"

#Con la opción -i ignora MAY/Mins, es decir, ignora la cadena a pesar de escribir "Grep" 
ps aux | grep -i "cron" | grep -vi "Grep"

# Excluir varias palabras de una búsqueda ( -v: exlcuye, -e: expresion)
cat /etc/passwd

grep -v -e "elenagg" -e "nologin" /etc/passwd
grep -vE "elenagg|nologin" /etc/passwd

history | grep "ls" | grep -v -e "-l" -e "-a" -e "~"

# Ver que pasa si usamos este formato:
history | grep "ls" | grep -vE "-l|-a|~"

# Hay que 'escapar' el primer signo -
history | grep "ls" | grep -vE "\-l|-a|~"

#---

cat /etc/passwd | grep "elenagg"

# Buscar líneas que empiecen con un patrón (^)
cat /etc/passwd | grep "^elenagg"

# Buscar líneas que terminen con un patrón ($)
cat /etc/passwd | grep "elenagg$"


man grep

```

**EJERCICIO**:

`ip neigh` nos da la tabla arp de la interfaz Ubuntu. Seleccionar las interfaces que estan en estado STALE y mostrar solo las últimas 2 líneas del resultado.  



### `find` : Buscando ***ficheros*** dentro de un directorio (y subdirectorios)

**Sintaxis básica**

find directorio_de_busqueda opciones termino_a_buscar

- **directorio_de_busqueda**: es el punto de origen de donde deseas iniciar la búsquedaopciones. Puede ser el directorio raíz "/", el directorio actual ".", el directorio de trabajo "~" o cualquier otro diectorio.
- **opciones**: es el filtro a utilizar para buscar el archivo. Podría ser el nombre, tipo, fecha de creación o de modificación del archivo, etc.
- **termino_a_buscar**: especificará el término de búsqueda relevante

.

**Buscar un fichero por su nombre, opción: -name**

```bash
cd

# Buscar un nombre de **fichero** por su nombre (-name)
find . -name "doc1.txt"
find . -name "doc*.txt"
find . -name "doc*.txt"

# Buscar un nombre de fichero sin diferenciar entre MAYÚSCULAS/minúsculas (-iname)
find . -iname "DOC*.txt"

# Buscar todos los nombres de fichero excepto los que se indiquen (-not)
find . -not -name "doc*.txt"

# Buscar y borrar (¡CUIDADO!)
find . -name "mi_fichero.txt" -delete

```
**Buscar un fichero por el tipo, opción: -type**

```bash
# Buscar por tipo de fichero (-type [f (fichero normal) d(directorio) l(enlace simbólico)])
# Muestra todos los directorios del sistema
find / -type d

# Muestra los directorios del home del usuario
find ~ -type d

# Se pueden combinar opciones 
# Busca ficheros que cumplen un patron y excluye directorios y enlaces
find ~ -type f -name "doc*"

# Busca directorios que cumplen un patrón excluyendo ficheros y enlaces
find ~ -type d -name "doc*"

```


**Buscar por tamaño (-size)**
```bash 
# El tamaño debe ir acompñado de la "magnitud" -> (c:caracter/byte, k:kilobytes, M:megabytes, G:gigabytes, b:bloques de 512 bytes)

# Buscar por tamaño (exacto) (-size)
# Usando bloques de 512 y redondeando
find / -size 10M

## Otras búsquedas por tamaño
find ~ -size +5G
find ~ -size -1k

```
**Buscar por propietario o grupo (-user y -group)**

```bash
ls -l

# Buscar por propietario o grupo (-user y -group)
find /tmp -user elenagg
find /tmp -group elenagg

```

**Buscar por permisos**
```bash
# Búsqueda por permisos (exacta).  
find ~ -perm 644

# Búsqueda por permisos (como mínimo 644).  
find ~ -perm -644

```

**Otras opciones de búsqueda**
```bash
# Algunas otras opciones útiles
## Buscar ficheros y directorios vacíos
find ~ -empty

## Buscar ejecutables
find / -executable

## Buscar legibles
find / -readable


man find
```

### `awk` : Una herramienta avanzada para el tratamiento de textos

`awk` es en realidad un lenguaje de programación interpretado. *awk* lee una
línea de la entrada y la va procesando (procesa línea a línea). Todas las 
instruciones que hacemos con *awk* se van aplicando secuencialmente a la
entrada (a la línea leída).

Es una herramienta muy potente, pero con cierta complejidad. Aquí sólo se 
muestran unos pocos usos comunes.

**Su utilización estándar es la de filtrar ficheros o salida de comandos de Linux, tratando las líneas para, por ejemplo, mostrar una determinada información sobre las mismas**

Se puede considerar que AWK tiene 2 partes principales: **el patrón y la acción**. Para ver como funciona lo veremos con un ejemplo.

**Sintaxis básica**

awk [opcion] [patron] {accion} nombre_fichero

```bash
# Mostrar el contenido de un fichero
cd 
echo "1) John       Maths         6.54" > notes.txt
echo "2) Paul       Physics       8.23" >> notes.txt
echo "3) Michael    Biology       7.98" >> notes.txt
echo "4) Steve      Physics       5.10" >> notes.txt
echo "5) Jack       History       4.68" >> notes.txt
echo "6) Richard    Programming   9.05" >> notes.txt
echo "7) John       Programming   8.42 - this note should be reviewed" >> notes.txt

ls -l
cat notes.txt


# En este caso sólo hay una acción "print" de todas las líneas de un fichero
awk '{print}' notes.txt

#En este caso se imprimen las líneas del fichero que cumplen el patrón:
awk '/Phy/ {print}' notes.txt

# Imprimir columnas (o campos)
# \t representa un carácter de tabulador, y se usa para igualar los límites de la línea de salida.
# Los números precedidios del caracter "$" son similares al uso de argumentos de los Shell Scripts , pero en este caso representan las posiciones de las columnas de la línea de entrada.
# $1 es para el primer campo, $2 es para el segundo campo, etc
# E $0 es para toda la línea

awk '{print $1}' notes.txt
awk '{print $2"\t"$3}' notes.txt
awk '{print $2"\t\t"$3}' notes.txt

# Imprimir las filas que cumplan un patrón
awk '/Phy/ {print}' notes.txt

# Imprimir las columnas que cumplan un patrón
awk '/Phy/ {print $1}' notes.txt
awk '/Phy/ {print $2"\t\t"$3}' notes.txt

# Contar e imprimir el número de filas que cumplen un patrón
awk '/Phy/{++counter} END {print "Cuenta = ", counter}' notes.txt

# Imprimir las filas que superan los 35 caracteres
awk 'length($0) > 35' notes.txt

# Se puede especificar otro carácter como separador de campos (opción -F)
# El separador por defeco es el espacio o el TAB
echo "10;20;30;40;50;60;70" > scale.csv
cat scale.csv | awk -F ";" '{print $1}'
cat scale.csv | awk -F ";" '{print $1"\t"$2"\t"$7}'

# Es muy útil para tratar la salida de otros comandos
Para imprimir tamaño de ficheros:
ls -la | awk '{print $5 "\t" $9}'

man awk
info awk
```

### `sed` : Edición de textos desde el terminal

`sed` es un editor de flujos (**s**tream **ed**itor). Al igual que *awk*
trata cada línea de su entrada. Por eso se suele usar para editar ficheros
o leer la salida de algún comando, sustituyendo un texto (o patrón) por
otro.

**Sintaxis básica**

- sed  's/cadena_original/cadena_nueva/'  fichero_de_entrada
- sed  's/cadena_original/cadena_nueva/' < fichero_de_entrada
- sed  's/cadena_original/cadena_nueva/' < fichero_de_entrada > fichero_salida
- resultado_comando | sed 's/cadena_original/cadena_nueva/'


```bash
# Sustitución de un patrón por otro patrón
echo "Es un poema de Antonio" | sed 's/Antonio/Federico/'
echo "Es un poema de Antonio" | sed 's/nton/urel/'

# ¡Atención! Por defecto, sed sólo modifica la primera ocurrencia del 
# patrón:
echo "one two three, one two three" > example.txt
echo "four three two one" >> example.txt
echo "one hundred" >> example.txt
cat example.txt

cat example.txt | sed 's/one/ONE/'

# Para reemplazar TODAS las ocurrencias, se usa la opción de reemplazo global:
cat example.txt | sed 's/one/ONE/g'

# Se puede especificar otro delimitador diferente a '/'
echo 'PATH=$PATH:/usr/local/bin' > wrong_path.txt
cat wrong_path.txt

# Si la cadena a reemplazar contiene '/', tenemos que escapar cada  slash'/' mediante un backslash '\' para que sed no las confunda con los separdores que necesita
#Vamos a cambiar en wrong_path.txt  /usr/local/bin por /opt/bin:
cat wrong_path.txt | sed 's/\/usr\/local\/bin/\/opt\/bin/'

# Esto se puede evitar cambiando el símbolo delimitador:
cat wrong_path.txt | sed 's_/usr/local/bin_/opt/bin_'
cat wrong_path.txt | sed 's:/usr/local/bin:/opt/bin:'
cat wrong_path.txt | sed 's|/usr/local/bin|/opt/bin|'
cat wrong_path.txt | sed 's+/usr/local/bin+/opt/bin+'

# sed se usa mucho para modificar ficheros ya existentes
## Generando un nuevo fichero con los cambios:
sed 's/\/usr\/local\/bin/\/opt\/bin/' < wrong_path.txt > right_path.txt
cat wrong_path.txt
cat right_path.txt

## Sobreescribiendo el fichero original, pero con los nuevos cambios: (-i)
cat wrong_path.txt
sed -i 's/\/usr\/local\/bin/\/opt\/bin/' wrong_path.txt
cat wrong_path.txt

# Se puede usar sed para modificar varias expresiones simultáneamente (opción -e):
cat example.txt | sed -e 's/one/ONE/' -e 's/two/TwO/'
cat example.txt | sed -e 's/one/ONE/g' -e 's/two/TwO/'
cat example.txt | sed -e 's/one/ONE/' -e 's/two/TwO/g'
cat example.txt | sed -e 's/one/ONE/g' -e 's/two/TwO/g'

# También se pueden modificar sólo las líneas que además contengan otro
# patrón:
cat example.txt | sed '/hundred/s/one/ONE/'

# Se pueden buscar patrones para reemplazar, independientemente de las
# MAYÚSCULAS/minúsculas:
cat example.txt | sed 's/oNe/ONE/I'

# Por último, se pueden agrupar varias opciones a la vez:
cat example.txt | sed -e 's/oNe/ONE/Ig'

# Como sustituir todos los espacios en blanco por una ','
echo "esto es un texto de prueba" > fichero.txt
sed 's/\s/,/g' fichero.txt

# Y si hay varios espacion en blanco.....
# \s: "espacio en blanco"
# \+: más de un espacio (es necesario "escapar" el caracter +)
echo "esto     es    un     texto     de     prueba" > fichero2.txt
sed 's/\s\+/,/g' fichero2.txt

man sed
info sed
```

### `tr` : Traductor (reemplazo o eliminación de caracteres). 

El comando `tr` es un "traductor": sirve para **reemplazar o eliminar** un conjunto 
de caracteres por otro conjunto de caracteres.

**Sintaxis básica**

tr [parametros]... conjunto1 [conjunto2]

Donde CONJUNTO1 Y CONJUNTO2 son ambos secuencias de caracteres definidas explícitamente o conjuntos predefinidos por este comando. 

Parámetros: 
- "-c": Usa el complemento del CONJUNTO1. Esto significa que define al CONJUNTO1 como todos los caracteres que no se encuentran en la definición dada por el Usuario. Este parámetro es útil para indicar caracteres que no queremos que se vean afectados.
- "-d": Borra los caracteres de definidos en CONJUNTO1.
- "-s": (squeeze-repeats): Elimina la secuencia continua de caracteres repetidos, definidos en el CONJUNTO1.

Conjuntos de caracteres POSIX (algunos de los más comunes): 
- [:alnum:] : Las letras y Dígitos.
- [:alpha:] : Letras.
- [:digit:] : Dígitos.
- [:graph:] : Caracteres imprimibles, sin incluir el Espacio en Blanco.
- [:print:] : Caracteres imprimibles, incluyendo el Espacio en Blanco.
- [:lower:] : Letras minúsculas.
- [:upper:] : Letras mayúsculas.
- [:punct:] : Signos de puntuación.
- [:space:] : Los Espacios en Blanco (horizonatales y verticales).

```bash

echo "Cambiemos espacios por tabuladores" | tr [:space:] '\t' 

# Como se puede ver, se pueden usar clases de caracteres POSIX
echo "mi nombre es Elena" | tr [:lower:] [:upper:]

# Y es útil, porque aunque se puede especificar un conjunto completo de caracteres, no siempre es elegante
echo "mi nombre es Elena" | tr abcdefghijklmnopqrstuvwxyz ABCDEFGHIJKLMNOPQRSTUVWXYZ

#Incluso codificar mensajes secretos
echo "mi nombre es elena" | tr abcdefghijklmnopqrstuvwxyz   ZYWVUTSRQPONMLKJIHGFEDCBA > secreto1.txt
cat secreto1.txt
cat secreto1.txt | tr ZYWVUTSRQPONMLKJIHGFEDCBA abcdefghijklmnopqrstuvwxyz 

# Se pueden usar intervalos. En este ejemplo, además, se ejecuta tr en modo interactivo. Usar ^C para salir. 
tr a-z A-Z 
mi nombre es Elena 


# Se puede utilizar tr de fomra encadenada 
echo "mi nombre es Elena, profesora de la asignatura" |tr ' ' z |tr a-z b-z |  tr b-z a-z | tr y ' '

# ¿Y si hay muchos espacios entre palabras?
echo "Cambiemos  espacios   por tabuladores" | tr [:space:] '\t'

# Podemos ignorar las repeticiones de los caracteres que son iguales y van seguidos y dejar uno solo (-s)
echo 'Este es un simple ejemplo!!!.... Algo práctico tal vez???' | tr -s '!¡.?¿'

# Podemos sustituir las repeticiones de los caracteres que son iguales y van seguidos (-s)
# El flag -s es útil para "comprimir" los caracteres repetidos seguidos
echo "Cambiemos  espacios         por tabuladores" | tr -s [:space:] '\t'
echo "ssss BBB ss BBBBB sssss" | tr -s 's' 'a'

# Más ejemplos
echo "Comprimimos        los      espacios    :)" | tr -s [:space:]
echo "Comprimimos        los      espacios    :)" | tr -s [:space:] ' '

# En lugar del comodín [:space:] también puedo usar el carácter espacio directamente
echo "Comprimimos        los      espacios    :)" | tr -s ' '

# También permite suprimir o borrar ciertos caracteres (-d)
echo "Borrremos todas las errrres" | tr -d 'r'

# Eiminemos números
echo "Mi DNI es 555424242A" | tr -d [:digit:]`

# De nuevo, se pueden usar intervalos
echo "Mi DNI es 555424242A" | tr -d 0-9

# Se puede usar el complemento de un conjunto (-c), por ejmeplo para borrar 
# Imprimir solamente los números (borro todo lo que no son digitos)
echo "Mi DNI es 555424242A" | tr -cd [:digit:]	
# Borro lo que no son números ni espacios
echo "Mi DNI es 555424242A" | tr -cd [:digit:][:space:]

# Para eliminar todos los caracteres repetidos se hace uso de -s con el complemento (-c) del Conjunto "":
echo 'Esteeee es ooootro simple ejemplo!!!.... ' | tr -c -s ""

# El complemento (-c)  se puede utilizar para eliminar todos los caracteres no imprimibles de un fichero (ej: saltos de línea \n, tabuladores: \t, etc)
# Utilizamos primero el comando `echo`con la opción -e para interpretar los saltos de línea y los tabuladores: 	
echo -e "Hola,\n\n\t\t mundo"
echo -e "Hola,\n\n\t\t mundo" | tr -cd [:print:]

# Otra forma enviando antes la información a un fichero
echo -e "Hola,\n\n\t\t mundo" > non_printable.txt
cat non_printable.txt

# Paso el fichero como entrada del cmando tr
tr -cd [:print:] < non_printable.txt

# Podría guardarse el fichero modificado en un nuevo fichero
tr -cd [:print:] < non_printable.txt > printable.txt


man tr
```

### `cut` : Cortar textos

El comando `cut` permite seleccionar y cortar campos de cada línea de un fichero o de su entrada estándar para extraer extraer las partes seleccionadas. Para diferenciar un campo de otro, usa delimitadores. El delimitador por defecto es el tabulador

**Sintaxs básica**

cut OPCIONES... [ARCHIVO]...

- "-c": Esta opción  selecciona los **caracteres** de cada línea en base al archivo de entrada
- "-d": Permite indicar el **delimitador** entre los campos
- "-f": Esta opción permite  seleccionar las **columnas** separadas por un delimitador en particular

 
```bash
cd
echo "Lee y aprende, aprende y haz, haz y evoluciona." > meme.txt
echo "Learn and code, code and share, share and evolve." >> meme.txt

# Selecciona unos caracteres a partir de su posición en la línea (-c)
cut -c 1-3 meme.txt
cut -c 7-14 meme.txt

# -f: Selecciona campos, -d: separados por un delimitador

# Seleccionar campos (separados por ',')  o selecciona un rango de campos (separados por '-'),  sseparados por un delimitador (-d) (-f)
cut -d "," -f 1,2 meme.txt 
cut -d "," -f 1,3 meme.txt 
cut -d "," -f 1-3 meme.txt 

cut -d " " -f 1,2 meme.txt 
cut -d " " -f 1,3 meme.txt 
cut -d " " -f 1-3 meme.txt 

# ¡Cuidado con los límites de los campos! (nuestro fichero solo tiene 3 campos separados por ',')
cut -d "," -f 1,4 meme.txt 
cut -d "," -f 1,5 meme.txt 

# Ejemplo: Como ver la memoria RAM (total)
free
free | grep Mem
free | grep Mem | tr -s ' ' ','
free | grep Mem | tr -s ' ' ',' | cut -d "," -f 2

# Otra forma
free | grep Mem | tr -s [:space:] | sed 's/ /,/g' | cut -d , -f 2


# Otra forma: usando sed con una Expresión Regular (entre dos slash)
# \s --> space
# \+ --> más de un espacio. Es necesario "escapar" (con backslash) el caracter "+" 
free | grep Mem | sed 's/\s\+/,/g'
free | grep Mem | sed 's/\s\+/,/g' | cut -d , -f2

#Usando tr
free | grep Mem |  tr -s ' ' ',' | cut -d , -f2

# Para especificar bytes en lugar de caracteres (útil con ficheros binarios),
# se puede usar la opción para especificar el número de bytes
 (-b).


man cut
info cut
```

### `wc` : Word Count - Calculadora en el terminal

`wc`es una utilidad que permite contar el número de líneas, el número de 
palabras y el número de bytes de un fichero, o de las líneas que reciba 
por su entrada.

```bash
# Número de líneas, número de palabras, y número de bytes
wc /etc/bash.bashrc

# Para contar el número de líneas
wc -l /etc/bash.bashrc

# Para contar el número de bytes
wc -c /etc/bash.bashrc

# Para contar el número de caracteres
wc -m /etc/bash.bashrc 

# Para contar el número de palabras
wc -w /etc/bash.bashrc 

# Tamaño de la línea más larga
wc -L /etc/bash.bashrc 

# Se puede usar en combinación con otros comandos
ls -l /etc | wc -l


man wc
```

## Redirecciones: Entrada estándar, Salida estándar, Salida de error

Una *shell* como Bash, ejecuta comandos y programas que toman unos datos de
entrada y producen unos datos de salida. Normalmente estos datos de E/S son
bytes o caracteres. 

Los datos de entrada de un comando se pueden obtener, por ejemplo, de un 
fichero, desde la terminal (el teclado), o desde la salida de otro comando.

La salida de un comando puede dirigirse a un fichero, a la terminal (pantalla), 
o a la entrada de otro comando. Es decir, se pueden hacer redirecciones de la
salida de un comando hacia otro descriptor de fichero destino.

Las *shell* en Linux, manejan tres flujos de E/S (*I/O streams*):

- `0 - stdin ` - **Entrada estándar**: Proporciona la entrada a los comandos.
  Tiene el descriptor de fichero **0**.
- `1 - stdout` - **Salida estándar**: Muestra la salida de los comandos.
  Tiene el descriptor de fichero **1**.
- `2 - stderr` - **Salida de error estándar**: Muestra la salida de los errores
de los comandos. Tiene el descriptor de fichero **2**.

Vamos a preparar unos ficheros para poder practicar las redirecciones:

```bash
cd
mkdir redir
cd redir
echo -e "1 Programación\n2 Sistemas Operativos\n3 Estadística" > x_text1
echo -e "9\tredes\n3\testadística\n10\tprogramación" > y_text2
echo -e "4\tPatatas\n5\tHuevos\n10\tCebolla" > text3

```

### Redireccionar la salida

Como se ha visto anteriormente, existen dos operadores para redireccionar una 
salida a un fichero:

- **`>`**  redirecciona la salida de un descriptor de fichero hacia otro fichero.
  de salida. Crea el fichero de salida si no existe. Si ya existe, los 
  contenidos del fichero de salida son sobreescritos, generalmente sin avisar.

- **`>>`**  redirecciona la salida de descriptor de fichero hacia otro fichero.
  de salida. Crea el fichero de salida si no existe. Si ya existe, los 
  contenidos se anexan al fichero de salida.

Con estos modos, se puede separar la salida estándar de la salida de errores del
un comando. Veamos algunos ejemplos básicos:

```bash

# Enviamos los datos de salida a un fichero y los errores a otro fichero:  
ls -laR /  1>fichero_datos_salida 2>fichero_errores

# LOs errores los podemos ignorar: redirigiendo la salida de error (stderr) a /dev/null
ls -laR /  2>/dev/null

# Si queremos ignorar la salida del comando: redirige la salida estándar (stdout) a /dev/null
ls -laR /  1>/dev/null

# Si queremos ignorar la salida y los errores: primero redirigir la salida estándar (stdout)
# a /dev/null, y después redirigir la salida de error (stderr) hacia donde
# apunte la salida estándar (que se había apuntado a /dev/null)
ls -laR / 1>/dev/null 2>&1

# Lo más común y úitl para guardar la salida de un comando (o programa) y los errores es utilizar:
ls -laR / 1>salida_y_errores.txt 2>&1

```

¿Qué es **/dev/null?** 
Es un fichero especial en Linux. Es donde se envía la
información para que sea descartada. Representa "la nada".

Sigamos con más ejemplos usando los ficheros que hemos preparado antes:

```bash

#mkdir redir
cd ~/redir
echo "Hola" > xprueba.txt

ls x* z*
ls x* z* 1>stdout.txt 2>stderr.txt
cat stdout.txt
cat stderr.txt

```

**Si se omite el descriptor de fichero,** se toma la salida estándar por defecto stdout. La salida estandar por defecto "stdout" tiene como descriptor de fichero: 1.

```bash 
ls x* z* >stdout.txt 2>stderr.txt

ls w* y*
ls w* y* >>stdout.txt 2>>stderr.txt

cat stdout.txt
cat stderr.txt

```

Se puede redirigir la salida estándar y la salida de error estándar al mismo
destino, y para ello se emplean los operadores **`&>`** y **`&>>`**.

```bash
ls x* z* &>stdout_and_stderr.txt
ls w* y* &>>stdout_and_stderr.txt
``` 

**El orden** en el que se redireccionan las salidas **es importante**. Por ejemplo:

```bash
[ls 2>&1 >salida.txt]
ls x* z* 2>&1 >salida.txt
cat salida.txt
#en el fichero no se incluye la salida de error

```

no es lo mismo que:

```bash
[ls >salida.txt 2>&1]
ls x* z* >salida.txt 2>&1
cat salida.txt
#en este caso salida.txt si está afectado por la redirección

```

- En el primer caso, *stderr* es redireccionada al sitio actual de *stdout* y luego 
*stdout* es redireccionada al fichero *salida.txt*. Pero esta segunda 
redirección afecta sólo a *stdout*, no a *stderr*. 

- En el segundo caso, *stderr* es redireccionada al sitio actual de *stdout* (que es *salida.txt*), por ello el error aparece en el fichero

Observar que en el primer comando que la salida estándar fue redireccionada después de que el error estándar, por lo tanto la salida del error estándar todavía va a la ventana de la terminal.

```bash

# Por defecto, las salidas stdout y stderr de un comando, están apuntando por defecto al terminal/pantalla 
#                   
#                   +----------+   (stdout)
#                   |          |----> 1 (terminal, pantalla)
#            0 >----|    ls    |
#         (stdin)   |          |----> 2 (terminal, pantalla)
#                   +----------+   (stderr)
#

```


```bash
# Redirige stdout y stderr hacia el fichero salida.txt
ls x* z* &>salida.txt
cat salida.txt
#en la pantalla NO se presenta la salida de stderr

# Redirige stdout al fichero salida.txt, y después redirige stderr hacia donde apunte stdout (que es salida.txt)
ls x* z* >salida.txt 2>&1
cat salida.txt
#en la pantalla NO se presenta la salida de stderr


# Redirige stderr a donde apunte stdout (terminal, pantalla). Después redirige stdout hacia el fichero salida.txt. ¡Pero stderr se había quedado apuntando al terminal (pantalla)!
ls x* z* 2>&1 >salida.txt    # stderr no va hacia salida.txt
cat salida.txt
#en la pantalla SÍ se presenta la salida de stderr


# Ahora podemos ejecutar los siguientes comandos para ver la diferencia
find / -user elenagg | grep -vi denegado
find / -user elenagg 2>&1 | grep -vi denegado

```

### Redireccionar la entrada

Se puede redireccionar la entrada estándar (stdin) como entrada de un comando utilizando el operador **`<`**. 

Anteriormente se han visto varios ejemplos, similares al siguiente:

```bash
echo "Esto es una prueba" > text1
tr ' ' '\t' <text1
cat text1
cat < text1
```

Algunas *shells*, como Bash, incluyen una forma de redirección de entrada
llamada `here-document`, y que es habitualmente usada en scripts. 

Los *here-documents* también se usan con el operador **`<<`**.

A continuación se muestran algunos ejemplos de *here-documents*:

```bash
# Usamos un here-document para simular un fichero como entrada para el comando 'sort'. El here-document está delimitado por el identificador END.

# 'sort' es una utilidad que toma el archivo(s) que figuran en su lista de argumentos y ordena sus líneas según unos parametros. 

sort -k2 <<END
1 física
2 programación
3 estadística
END
# el documento se acaba al introducir END. sort -k2 ordena alfabeticamente por la segunda columna


# Es común crear el contenido de un fichero así:
cat <<TORT >tortilla.txt
patatas
huevo
CEBOLLA
sal
aceite

y amor :)
TORT

cat tortilla.txt
```

### Tuberías (pipes)

Ya se han usado tuberías anteriormente. Básicamente sirven para redirigir la
salida de un comando hacia la entrada de otro comando. Se pueden así encadenar
comandos usando tuberías. El operador tubería es **`|`** (AltGr + 1).

```bash
ls y* x* z* u* q*

ls y* x* z* u* q*  2>&1 | sort -r
# comando sort: ordena. Con opción -r invierte la ordenacion

```

### Para ampliar...

**Operador "-"**

Cada comando de la cadena puede tener sus opciones o argumentos. Algunos 
comandos usan el operador **`-`** (guión) en lugar de un nombre de fichero, 
cuando la entrada del comando debe provenir de un stdin (en lugar de hacerlo de un fichero).

```bash

# Preparamos el fichero para el ejemplo
echo  "Texto 1" > text1
echo  "Texto 2" > text2
echo  "Texto 3" > text3
cat text1 text2 text3  > tuberias.txt
cat tuberias.txt

tar cvf tuberias.tar tuberias.txt
ls -l
bzip2 tuberias.tar
ls -l

# Ahora podemos ver el operador - en acción:
 bunzip2 -c tuberias.tar.bz2 | tar -xvf -

```

En el caso de **usar tuberías para encadenar varios comandos**, y que algún comando necesite redireccionar su entrada (con el operador **<**), se pondrá esa redirección de la entrada en primer lugar dentro de la cadena de comandos.

Esto quiere decir, que será el primer comando el que tome como entrada un
fichero, por ejemplo. Así, el resto de comandos que estén encadenados harán
operaciones y tratamientos sobre el fichero leído por el primer comando.

### Para ampliar más aún...

Las *shells* en general, y Bash en particular, son un conjunto de herremientas
muy completas, pero que también revisten cierta complejidad. Dominar estas
herramientas es cuestión de práctica, estudio y uso. Si se desea ampliar lo
expuesto en este tema, se puede buscar información acerca de los siguientes
temas:
"
- Uso de la opción **-exec** para el comando **find**.
- La herramienta **xargs**.
- La herramienta **tee**.

## Algunos recursos útiles

- [Learn Linux 101](https://developer.ibm.com/tutorials/l-lpic1-map/)
- [comando superbasicos](https://ryanstutorials.net/linuxtutorial/cheatsheet.php)
- [Linux Tutorial](https://ryanstutorials.net/linuxtutorial/)
- [Linux Filesystem](https://www.linux.com/tutorials/linux-filesystem-explained/)
- [Tutorial de awk](https://www.grymoire.com/Unix/Awk.html)
- [Tutorial de sed](https://www.grymoire.com/Unix/Sed.html)
- [Developer Technologies - IBM](https://developer.ibm.com/technologies/)
- [Servidor TeamSpeak3](https://www.hostinger.es/tutoriales/crear-servidor-ts3-teamspeak-3)
