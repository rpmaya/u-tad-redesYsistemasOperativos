# Tema 7 - Bash Scripting

Los *scripts*, de forma similar a los programas, permiten automatizar tareas y agrupar comandos que, normalmente, se escribirían en la *shell*. De hecho, cualquier comando o expresión que se puede ejecutar en la *shell*, puede ser ejecutado en un *script*, y viceversa.

Este tema es una introducción a la programación de *scripts de shell*, usando la shell de *Bash*. El contenido es eminentemente práctico.


## Estructura de un script

### Shebang: #!

La primera línea de un *script* hace referencia al intérprete que ejecutará el contenido del fichero (el código del *script*). Por ejemplo, para el anterior *script* `hola.sh` se compone de:

- **#!** : A estos dos caracteres en la primera línea de un *script* se les denomina **shebang**.
- **/bin/bash** : Junto al *shebang* se especifica el **intérprete** del *script* (en este caso, es **/bin/bash**).

normalmente usaremos el intérprete */bin/bash*, pero podrían usarse diferentes intérpretes de *shell*, como: */bin/csh*, */bin/zsh*, etc.

También se puede usar un *shebang* para ejecutar código escrito en algún lenguaje de *scripting*, como Python o Ruby.

Conviene especificar siempre el *shebang* para cada script, y siempre debe estar en la primera línea, y escribirse tal cual, sin espacios entre sus partes.

```bash
 #!/usr/bin/python
 #!/usr/bin/node (java)
 #!/usr/bin/ruby
```

**La sintaxis de los shell-scripts se caracteriza por ser bastante estricta en su escritura**, especialmente en lo que se refiere a la inserción u omisión de espacios en blanco entre las palabras especiales. Ten esto muy en cuenta a la hora de escribir los scripts que se proponen.

### ¿Cómo se ejecutan los scripts en Bash?

Básicamente, cuando se ejecuta un *script* en una *shell*, esta creará un sub-proceso, dentro del cual se ejecuta el *script*. El *script* necesitará tener permisos de ejecución.

No obstante, la utilización del shebang y la creación de este sub-proceso está condicionada por la forma en que sea invocado el shell-script, existiendo 3 opciones:

- **Explícita:** escribiendo explícitamente qué shell se desea invocar y pasando como argumento el nombre del script. Se crea un sub-proceso nuevo (se ignora el shebang). 

```bash
/bin/bash script_ejemplo.sh
```

- **Implícita:** invocando al script como si fuera un ejecutable, lo que requiere asignar permisos de ejecución al script.  Se ejecuta con "./" porque el directorio local no está en la variable PATH. Se crea un sub-proceso nuevo.   

```bash
./script_ejemplo.sh
```

- **Implícita con "."** (equivale a importar): el script será interpretado por el mismo proceso del shell responsable de la línea de comandos desde la que se invoca el script. Es decir, en este caso no se crea un sub-proceso. 

```bash
. script_ejemplo.sh
```
**NOTA**: Nosotros usaremos la forma "Implícita" de ejecución. 


Vamos a preparar un script y probaremos las tres formas aprendidas de ejecucución para observar los resultados.  

Crear el script: script_ejempl.sh. Utilizar cualquiera de los editores `vim` o `nano` pada editar el fichero. 

```bash
vim script_ejemplo.sh
```

Escribir en el editor el siguiente contenido: 
```bash
#!/bin/bash

echo Hola
# ps w: devuelve los procesos en ejecución en formato largo (wide)
ps w

# "$$" devuelve el id del proceso que se está ejecutando
echo "Proceso que ejecuta el script: $$"
```

Aseguramos de que el script tenga permisos de ejecución y ya podemos hacer las pruebas

```bash
chmod a+x script_ejemplo.sh
```


Vamos seguir practicando, ahora usaremos un here-document para crear un script......

```bash
cd

# Crea el script 'hola.sh', mediante el método "here-document": 
cat <<SCRIPT >hola.sh
#!/bin/bash

echo "Hola, Bash"
cd
pwd
ls -la
SCRIPT

# Otorga permiso de ejecución al script:
chmod a+x hola.sh

# Ejecuta el script (se usa ./ porque el directorio actual no está en el $PATH):
./hola.sh

```


## Variables (I)

Además de las variables de entorno, se pueden definir y usar variables internas en un *script* (variables de *shell*). 

Antes de definir nuestras propias variables recordemos algunas variables de entorno estudiadas ya en el Tema 2. Las variables de entorno, vienen definidas por defecto en Bash, se refieren al entorno en el que trabajas y recordemos que puedes ver su valor con el comando `env`. 

Algunas variables interesantes como:

- $SHELL que indica el shell que estás ejecutando
- $HOME la ruta del usuario
- $USERNAME el nombre del usuario
- $HOSTNAME nombre de la máquina
- $PATH la ruta por defecto donde encontrar binarios
- etc.

No obstante podemos crear nuestras variables que podemos usar en nuestros scripts.
Veamos la sintaxis básica de manejo de variables. 

| Operacion                                        |  Sintaxis                   | 
:--------------------------------------------------|-----------------------------| 
| Sólo Definición                                  |  ****VAR=" "   o VAR=****   |
| Definición y/o Inicialización/Modificación       |  ****VAR=valor****          | 
| Expansión (Acceso a Valor)                       |  $VAR o  ${VAR}    |
| Eliminación de la variable                       |  ****unset VAR****          |

-

 
```bash
# Crea la variable MSG y le asigna un valor (signo = sin espacios)
MSG='Hola caracola'

# Hacer la prueba de dejar espacios. Ver los mensajes de error
MSG ='Hola caracola' 
MSG= 'Hola caracola'

# Para usar una variable, se usa el carácter $ antes de su nombre:
echo "El mensaje es: $MSG"

# Es buena práctica rodear la variable con {}
echo "El mensaje es el mismo: ${MSG}"

# Como se ha visto, cuando se usan comillas dobles ("), se sustituye la variable
# por su contenido (para eso se usa el símbolo $).

# Cuando se usan comillas simples ('), no hay susitución de variables, ya que las comillas simples construyen textos literales. 
# Las comillas simples no expanden el valor de las variables. Veamos:
echo 'El mensaje es: $MSG'

# Los nombres de las variables distinguen mayúsculas y minúsculas:
msg='A message to you, Trudy'
msG='Mrs. Gullible'
echo "msg = ${msg}"
echo "msG = ${msG}"

# Se puede usar la sustitución de comandos para almacenar la salida de un 
# comando en una variable (command-substitution):
CURR_DIR=$( pwd )
echo "El directorio actual es: ${CURR_DIR}"

# En este caso los espacios no importan
CURR_DIR=$( pwd)
CURR_DIR=$(pwd )

# El resultado es el mismo si la sustitución de comandos se rodea con comillas
# dobles:
CURR_DIR="$( pwd )"
echo "El directorio actual sigue siendo: ${CURR_DIR}"

```

#### Un poco más acerca de las comillas dobles y simples:

```bash

# Comillas dobles dentro de comillas dobles. Se interpreta todo y desaparecen las comillas dobles
echo " Aquí hay "comillas dobles" dentro de comillas dobles (escape)..."
>Se interpretan y desaparecen todas las comillas doble

# Para mantener "comillas dobles" dentro de comillas dobles - Se escapan comillas dobles 
echo " Aquí hay \"comillas dobles\" dentro de comillas dobles (escape)..."
> Se mantienen las comillas dobles, pero \ desaparece, \" = "

# Comillas simples dentro de comillas dobles.Se imprime literalmente el texto 
echo " Aquí hay 'comillas simples' dentro de comillas dobles..."
> Las comillas simples se mantienen
 
# Comillas dobles dentro de comillas simples. Se imprime literalmente el texto 
echo ' Aquí hay "comillas dobles" dentro de comillas simples...'
> Las comillas dobles se mantiene

# No puede haber comillas simples dentro de comillas simples, si quieres que se mantengan 
se escapan las comillas simples con '\' o se concatenan caracteres y se usa "'"
echo ' Aquí hay '\''comillas simples'\'' dentro de comillas simples...'
echo ' Aquí hay' "'"comillas simples"'" 'dentro de comillas simples....'
>Para usar comillas simples dentro de comillas simples debemos usar la concatenación de cadenas de caracteres y escape. 

 
```

## Argumentos de invocación de un script

En Bash, hay algunas variables especiales y que están definidas por defecto, y que se utilizan para pasarlas como argumentos o parámetros de entrada a la hora de ejecutar los scripts. Hay otras que refieren al propio script, al proceso que ha ejecutado el script, o a la máquina en la que se ha ejecutado el script. Así, algunas de ellas son las siguientes. 
 
- $0 representa el nombre del script
- $1 – $9 los primeros nueve argumentos que se pasan a un script en Bash
- $# el número de argumentos que se pasan a un script
- $@ Lista de todos los argumentos que se han pasado al script
- $* Cadena con el contenido completo de los parámetros pasados al script
- $? código de salida del último proceso/comando que se ha ejecutado
- $$ el ID del proceso del script


```bash
cd

cat <<'SCRIPT' >args.sh
#!/bin/bash

echo "-----------------------------------"
echo "El nombre de este script es: $0"
echo "El primer argumento es: $1"
echo "El segundo argumento es: $2"
echo "El número de argumentos es: $#"
echo "Todos los argumentos son: $@"

# Puedo hacer copias de los argumentos:
FIRST_ARG=$1
echo "El primer argumento es = ${FIRST_ARG}"
echo
SCRIPT

chmod a+x args.sh

./args.sh
./args.sh "Esto es Bash!"
./args.sh "¿Cuál es la respuesta?" 42
./args.sh "Tortilla" "Cebolla" "Patata" 5

```

Un detalle importante acerca de este *script*, es que usa un *here-document* y **el primer campo de terminación aparece entre comillas simples (')**. **Esto se hace para evitar que se sustituyan las variables (como *$1*, *$2*, ...)** por su contenido en el momento de generar el fichero *args.sh*.

El contenido del fichero *args.sh* será:

```bash
#!/bin/bash

echo "-----------------------------------"
echo "El nombre de este script es: $0"
echo "El primer argumento es: $1"
echo "El segundo argumento es: $2"
echo "El número de argumentos es: $#"
echo "Todos los argumentos son: $@"

# Puedo hacer copias de los argumentos:
FIRST_ARG=$1
echo "El primer argumento es = ${FIRST_ARG}"
echo
```

## Sustitución de comandos (command-substitution)

Sirve para ejecutar un comando *in-situ* y utilizar su salida. Para realizar una sustitución de comandos, se usa la construcción **$( )**. Para ejecutar un comando también se puede utilizar el acentro invertido **"`"**
Hemos visto más arriba como almacenar el resultado en una variables. 

```bash
echo "Estamos a $( date )."

CURR_DIR=$( pwd )
ls -la ${CURR_DIR}

MI_DIR=`pwd`
echo $MI_DIR


```

### Exportar variables (visibilidad, ámbito o scope)

Una variable declarada dentro de un *script* sólo es visible por ese *script*: es una **variable de _shell_**. Si se desea que los hijos de ese *script* tengan acceso a las variables, hay que "exportarlas". Cuando se exporta una variable de *shell*, se convierte en una **variable de entorno**, que es visible desde los procesos hijos.

Por ejemplo, un *script* (*sh1.sh*) llama a otro *script* (*sh2.sh*), y se desea que una variable declarada dentro de *sh1.sh* sea visible desde *sh2.sh*: hay que **exportar** la variable desde *sh1.sh*. El comando o palabra para exportar una varriable es "**export VAR**".

Creamos sh1.sh 

```bash
cat <<'SH1' >sh1.sh
#!/bin/bash

# File: sh1.sh

echo "Inicio de ejecucion de sh1"
var1='Galaxia'
var2=44444

# Exportamos la variable que contiene el directorio actual
export CURR_DIR=$( pwd )
echo "[$0]: ${CURR_DIR}"

echo -e "[$0]: var1 = ${var1} \t var2 = ${var2}"

# Exportamos var1
export var1

# Ejecutamos sh2 desde sh1
./sh2.sh

echo -e "[$0]: var1 = ${var1} \t var2 = ${var2}"
SH1

```
Creamos sh2.sh

```bash
cat <<'SH2' >sh2.sh
#!/bin/bash

# File: sh2.sh
echo -e "\n Inicio de ejecucion de sh2"

echo "[$0]: ${CURR_DIR}"

echo -e "[$0]: var1 = ${var1} \t var2 = ${var2}"

var1='Constelación'
export var1

echo -e "[$0]: var1 = ${var1}"

echo -e "Fin de sh2.... \n"

SH2

chmod a+x sh1.sh
chmod a+x sh2.sh

# Ejecutamos sh1.1 (siempre de forma implícita)
./sh1.sh

```

El contenido de los ficheros *sh1.sh* y *sh2.sh* sería:

- **sh1.sh**:

```bash
#!/bin/bash

# File: sh1.sh

echo "Inicio de ejecucion de sh1"
var1='Galaxia'
var2=44444

# Exportamos la variable que contiene el directorio actual
export CURR_DIR=$( pwd )
echo "[$0]: ${CURR_DIR}"

echo -e "[$0]: var1 = ${var1} \t var2 = ${var2}"

# Exportamos var1
export var1

# Ejecutamos sh2 desde sh1
./sh2.sh

echo -e "[$0]: var1 = ${var1} \t var2 = ${var2}"

```

- **sh2.sh**:

```bash
#!/bin/bash

# File: sh2.sh
echo "Inicio de ejecucion de sh2"

echo "[$0]: ${CURR_DIR}"

echo -e "[${0}]: var1 = ${var1} \t var2 = ${var2}"

var1='Constelación'
export var1
echo -e "[$0]: var1 = ${var1}"

echo -e "Fin de sh2.... \n"

```

Además de exportar una variable, también puede eliminarse (su contenido pasa a estar indefinido, *null*). Para ello se usa el comando `unset`.

```bash
export myVar='Andrómeda'
echo "myVar = $myVar"


unset myVar
echo "myVar = $myVar"

```

### Longitud de una variable

La forma nativa de obtener la longitud de una variable en bash es **${#var}**


```bash
cat <<'LEN' >len.sh
#!/bin/bash

# File: len.sh

msg='Tortilla de patatas'
echo "La longitud de '${msg}' es: ${#msg}"

num=7251
echo "La longitud del número '${num}' es: ${#num}"

LEN

chmod a+x len.sh
./len.sh

```

El contenido del fichero *len.sh* es:

```bash
#!/bin/bash

# File: len.sh

msg='Tortilla de patatas'
echo "La longitud de '${msg}' es: ${#msg}"

num=7251
echo "La longitud del número '${num}' es: ${#num}"

```

### Más pruebas con algunas variables de entorno útiles

```bash
cat <<'ENVVARS' >env_vars_demo.sh
#!/bin/bash

# File: env_vars_demo.sh
echo "Directorios de la variable 'PATH': $PATH"

cd
echo "Accediendo al directorio: $( pwd ) ..."
echo "   Status = $?"


echo "Accediendo al directorio: /root ..."
cd /root
echo "   Status = $?"

echo "Este script tiene el PID: $$"
echo "Este script ha sido ejecutado por: $USER"
echo "Este script se ejecuta en el host: $HOSTNAME"

sleep 2
echo "Este script lleva ejecutándose ${SECONDS} segundos"

# La variable RANDOM genera un número pseudo-aleatorio entre 0 y 32767.
echo "Un número aleatorio: ${RANDOM}"
echo "Otro número aleatorio: ${RANDOM}"
echo "Y otro número aleatorio más: ${RANDOM}"

echo "Esta es la línea $LINENO del script."

ENVVARS

chmod a+x env_vars_demo.sh
./env_vars_demo.sh

```

El contenido del fichero *env_vars_demo.sh* sería:

```bash
#!/bin/bash

# File: env_vars_demo.sh
echo "Directorios de la variable 'PATH': $PATH"

cd
echo "Accediendo al directorio: $( pwd ) ..."
echo "   Status = $?"

cd /root
echo "   Status = $?"

echo "Este script tiene el PID: $$"
echo "Este script ha sido ejecutado por: $USER"
echo "Este script se ejecuta en el host: $HOSTNAME"

sleep 2
echo "Este script lleva ejecutándose ${SECONDS} segundos"

# La variable RANDOM genera un número pseudo-aleatorio entre 0 y 32767.
echo "Un número aleatorio: ${RANDOM}"
echo "Otro número aleatorio: ${RANDOM}"
echo "Y otro número aleatorio más: ${RANDOM}"

echo "Esta es la línea $LINENO del script."

```

### Como concatenar cadenas de texto usando VARIABLES

```bash
#!/bin/bash
cadena1='La unión hace '
cadena2='la fuerza'
cadena3=$cadena1$cadena2
echo "$cadena3"
```
>La unión hace la fuerza


## Operaciones aritméticas  

Por defecto, una variable en Bash, independientemente de su contenido, es tratada como una cadena de texto y no como un número. 
Sin embargo, y gracias a la evolución de bash podemos hacer operaciones aritméticas aunque es importante aclarar que **Bash sólo opera con enteros.** Para realizar operaciones matemáticas con números reales necesitaremos utilzar `bc`.

Así, *Bash* ofrece varias formas de realizar operaciones aritméticas sencillas:  **_let_**, **_expr_** , **la expansión con dobles paréntesis** y el uso de **bc** para números reales. 

Algunas de las operaciones matemáticas más comunes son:

| Operador  | Operación                            |
| :-------- | :----------------------------------: |
|  +        |  Suma                                |
|  -        |  Resta                               |
|  *        |  Multiplicación                      |
|  /        |  División                            |
|  %        |  Resto o Módulo                      |
|  ++       |  Pre/Post-incremento (incrementa 1)  |
|  --       |  Pre/Post-decremento (decrementa 1)  |
|  **       |  Potencia                            |

Para nuestras operaciones **usaremos la expansión mediante los dobles paréntesis**. 

### Expansión de una operación aritmética: $((expresion))  (Uso de dobles paréntesis)

A continuación se realiza un ejemplo con expansión (evaluación de dicha expresión), por considerarse una buena práctica. Se describen varias operaciones matemáticas sencillas. Para usar la sustitución, se rodea la expresión matemática con dobles paréntesis, precedidos del símbolo *$*: **$(( ))**.

```bash
cd
cat <<'MATH' >math_ops.sh
#!/bin/bash

# File: math_ops.sh

# Se usa expansión: con dobles paréntesis que rodean a una expresión matemática
a=$((3+4))
echo "a = ${a}"

# Es buena práctica usar espacios para mejorar la claridad y la legibilidad
a=$(( 40 + 2 ))
echo "Ahora, a = ${a}"

# Se pueden usar variables como operandos de las expresiones matemáticas.
b=$(( $a - 10 ))
echo "b = ${b}"

# Se puede omitir el símbolo $:
b=$(( 10 + a + 10 ))
echo "Ahora, b = ${b}"

# Se puede usar ++ para incrementar en 1.  
(( b++ ))
echo "Autoincremento: b = ${b}"

# Como en otros lenguajes, se puede sumar y asignar en una sentencia
(( b += 5 ))
echo "Suma y asigna: b = ${b}"

# Operaciones comunes
a=$(( 5 + 7 ))
echo "5 + 7 = ${a}"

a=$(( 5 - 7 ))
echo "5 - 7 = ${a}"

a=$(( 5 * 7 ))
echo "5 * 7 = ${a}"

a=$(( 5 / 7 ))
echo "5 / 7 = ${a}"

a=$(( 7 / 5 ))
echo "7 / 5 = ${a}"

a=$(( 7 % 5 ))
echo "7 % 5 = ${a}"

a=$(( 5 ** 7 ))
echo "5 ** 7 = ${a}"

MATH

chmod a+x math_ops.sh
./math_ops.sh

```

El contenido del fichero *math_ops.sh* es:

```bash
#!/bin/bash

# File: math_ops.sh

# Se usa sustitución: con dobles paréntesis que rodean a una expresión matemática
a=$((3+4))
echo "a = ${a}"

# Es buena práctica usar espacios para mejorar la claridad y la legibilidad
a=$(( 40 + 2 ))
echo "Ahora, a = ${a}"

# Se pueden usar variables como operandos de las expresiones matemáticas.
b=$(( $a - 10 ))
echo "b = ${b}"
# Se puede omitir el símbolo $:
b=$(( 10 + a + 10 ))
echo "Ahora, b = ${b}"

# Se puede usar ++ para incrementar en 1
(( b++ ))
echo "Autoincremento: b = ${b}"

# Como en otros lenguajes, se puede sumar y asignar en una sentencia
(( b += 5 ))
echo "Suma y asigna: b = ${b}"

# Operaciones comunes
a=$(( 5 + 7 ))
echo "5 + 7 = ${a}"

a=$(( 5 - 7 ))
echo "5 - 7 = ${a}"

a=$(( 5 * 7 ))
echo "5 * 7 = ${a}"

a=$(( 5 / 7 ))
echo "5 / 7 = ${a}"
a=$(( 7 / 5 ))
echo "7 / 5 = ${a}"

a=$(( 7 % 5 ))
echo "7 % 5 = ${a}"

a=$(( 5 ** 7 ))
echo "5 ** 7 = ${a}"
```

## Otras formas de hacer operaciones matemáticas (alternativas a los dobles paréntesis)

**expr**  
Comando antiguo de Unix. Esta en desuso. Hay que dejar espacios alrededor de los operadores y operandos  var=$(expr 1+1) da 1+1 (y no 2 como esperaríamos). Lo correcto es var=$(expr 1 + 1) 
el * hay que escaparlo var=$(expr 5 \* 3). 
Para usar con precaución, mejor usar las otras opciones.

**let**  
let z=14; let z++; let z=z+10; echo "$z"


## Operaciones usando la concatenación de cadenas de texto (en variables)


```bash
#!/bin/bash
# Operaciones usando la concatenación de cadenas de texto
op='+'
echo "op='+'"
a=5
echo "a=5"
b=6
echo "b=6"
res=$((${a}${op}${b}))
echo " $a $op $b = $res"
```
> 5 + 6 = 11



## Operador bc (basic calculator)
El comando **bc** se puede considerar **un lenguaje de programación en si mismo**, y como tal tiene muchas opciones y cierta complejdad al igual que el comando awk que vimos anteriormente. 

bc permite las  **operaciones con números con decimales, en punto flotante, y además incluye varias funciones matemáteticas** 

Al igual que awk soporta un modo interactivo:
- **bc -i**: para iniciar el modo interactivo
- **bc -l**: para iniciar bc y poder utilizzar las funciones de su librería matemática
- **quit**: para salir del modo interactivo
- **Se pueden crear programas en bc** (no es oblogatorio que la expetnsión sea .bc, es más un convenio): **bc -l script.bc**. La sintaxis del  lenguaje es sencillo pero ligeramente diferente a la sintaxis de bash. Se pueden utilizar funciones como print() y read() para entrada y salida de datos. No será objeto de estudio en este tema.
- Los comentarios en bc se escriben así: **/* esto es un comentario */**


Nosotros nos centraremos en el modo no interactivo, para poder **usar bc en nuestros bash scripts**

En Ubuntu está incluido, pero en otras distribuciones, por ejemplo en CentoS, no está incluido.

Soporta estructuras de control (bucles) y condicionales (if). "bc" incluye las siguientes funciones especiales:

#### Funciones Especiales (bc)

#####sqrt () 
Sirve para calcular raices cuadradas 

#####scale ()
Esta funcion determina el numero de decimales con los que se va a trabajar. Por defecto es 0. Para asignar un valor a scale haremos: **"scale=valor"**

#####length () 
Indica el número total de dígitos significativo con los que se va a trabajar: 

##### ibase y obase
Definen la base de los números de entrada y salida, por defecto es 10. Valores de base 2 a 36, soportan de 0-9 y de A -Z.

#### Funciones de las librería matemática 
Las funciones de la librería matematica se invocan con (-l), son las sigientes:
- s(x) seno. x en radianes
- c(x), coseno, x en radianes
- a(x), arco tangente, devuelve un valor en radianes
- l(x), logaritmo natural (base e)
- e(x), función exponencial
- j(n, x), función de Bessel de orden n en x, devuelve el orden n,de la función de Bessel

#### Expresiones básicas

- **- expr** negacion de la expresión
- **++ var, -- var**  Pre incremento/decremento de la variable en 1, y este es el valor de la expresión
- **var ++, var --** El resultado de la expresión es su valor, y despues se Pos incrementa/decrementa. 

 

**(expr)** El uso de los **paréntesis fuerza la evaluación de la expresión en primer lugar**. Es importante para asegurar el orden de ejecución de la expresión si no se conoce la prioridad de la precedencia de los operadores.

- expr + expr 
- expr - expr
- expr / exp
- exp % expr
- exp * exp
- expr ^ expr2 . Potencia de base "expr" y exponente "expr2". En este último caso el exponente (expr2) debe ser entero. 


#### Expresiones relacionales de comparación:  

Son: 
- expr1 < expr2, Menor: el restultado es 1 si expr1 es estrictamente menor que expr2
- expr1 > expr2, Mayor 
- expr1 <= expr2, Menor o igual
- expr1 >= expr2, Mayor o igual
- expr1 == expr2, Igual
- expr1 != expr2, Distinto


#### Operaciones boleanas 
- !exp, el resultado es 1 si la expresión es distita de cero
- exp && exp, AND, el resultado es 1, si ambas no son cero
- exp || exp, OR, el resultado es 1, si una no es cero

##### Soporta algunas estructuras de control:   
Soporta bucles y sentencias condicionales:  while, for, if  


### Como usar bc en scripts
**El comando "bc" funciona como una claculadora, es decir le podemos pasar una serie de operaciones y/o expresiones y bc evalua el resultado.**


#### Comando "echo"
La forma más sencilla de usar bc en scripts es **con un comando echo, que imprime una expresión (operaciones), y las pasa al comando bc a trvés de la tuberia (|)**.   

echo "expresion" | bc [-l]

La expresión puede contener las funciones y opreadores vistos más arriba. 

Veamos algunos ejemplos: 
````
echo '6.5 / 2.7' | bc
````

El resultado será 2, debido a que por defecto bc **no** funciona con números en coma flotante. Para que los soporte usamos la sentencia scale (número de decimales. Asignamos a scale el valor de décimales que queremos utilizar. 


#### Podemos separar distintas operaciones mediante ";"

````

# Podemos separar distintas operaciones mediante ";"

echo 'scale=3; 6.5 % 2.7' | bc
echo 'scale=3; 6.5 % 2.7' | bc
````

#### Calcular el número de decimales o el número de digitos de un número
Veamos como averiguar el número de décimales o el número de digitos de un valor.  
El número, .12345 tiene una longitud (length=) de 5 y una escala (scale=) de 5,  mientras que el número 12345.67890 tiene una longitud de 10 y una escala de 5

```bash
echo 'length(.12345)' | bc
echo 'scale(.12345)' | bc

echo 'length(12345.12345)' | bc
echo 'scale(12345.12345)' | bc
```

#### Y ejecutar comparaciones

````bash
echo '2 > 1' | bc                 # Devuelve 1
echo '1 != 2' | bc                # Devuelve 1
echo '2 > 1 && 2 != 1' | bc       # Devuelve 1
```

#### Asignar el resultado a variables

**Otra forma útil de usar bc** en scripts, **es asignar a una variable al resultado del valor obtenido de la calculadora** (echo | bc), como se hace en el siguiente ejemplo.

```bash

z=$(echo '4.1+5.2' | bc)
echo $z

# Es lo mismo que...
z=$(echo '4.1+5.2' | bc);echo $z
```

## Consejos y recordatorios sobre el uso de bc

**bc**  
A veces, no es intuitivo usar variable dentro de una sentencia que usa bc, ni como almacenar en una variable los resultados. Vamos a ver un ejemplo. 

Incluir la variable x mediante ($x) en una función. 

```bash
# Calculo del seno hiperbólico de 3
x=3
y=$(( -1*$x ))
echo " scale=3; shx=(( e($x)-e($y) )); shx " | bc -l

```

La forma más elegante y potente de usar bc dentro de scripts es almacenando los resultados en variables globales, que pueden ser usadas posteriormente. Los valores almacenados en 
variable internas de bc se pierden.

```bash
x=3
y=$(( -1*$x ))
senhx=$(echo " scale=3; shx=(( e($x)-e($y) )); shx " | bc -l)
echo $senhx
echo $shx
```


**Más ejemplos**: 

- 1. Calcular el seno (pi), con 1000 decimales, cuando pi tiene 5 decimales pi = 3.14159

````
# Ejecutamos primero para ver que obtenemos
echo "scale = 1000; s(3.14159)" | bc -l

# Guardamos el resultado en una variable
resultado=$(echo "scale = 1000; s(3.14159)" | bc -l)
echo $resultado
````

- 2. Calcular el número pi, con 1000 decimales, basandote en que el arcontangente de 1 es pi/4, es decir, **arcotangente de 1 = pi/4**.  Recuerda que en bc la función arcontangente(X) es, a(x).

````

pi=$(echo "scale=1000; 4*a(1)" | bc -l)
echo $pi

````

 - 3. Calcular 3.5 elevado a 4 con escala de 10.
Podemos ver como se pueden realizar una cadena de expresiones matemáticas, en una línea, separadas por ";"

````
# Calculamos el resultado de una potencia con: ^= (3.5 elevado a 4 y se lo asignamos a "var")
echo "scale= 10; var=3.5;var^=4;var" | bc

# Encadenamos mediante ";" y asignamos el resultado final a una variable "x"
x=$( echo "scale= 10; var=3.5;var^=4;var" | bc)
echo $x

````

- 4. Calcular 255 en hexadecimal

````
x=$(echo "obase=16;255" | bc) 
echo $x

````

##### OJO con el comportamiento de los post/pre incrementos/decrementos de las variables

```bash
echo 'var=10;++var' | bc   # Devuelve 11
echo 'var=10;var++' | bc   # Devuelve 10

```
   
   
.
## Comando test, `test`
 

- **Para evaluar expresiones  en *Bash*, se usa el comando** `test`. El comando *test* compara dos expresiones mediante un operador que nos permite obtener el resultado de la condición. Se pueden anidar condiciones mediante los operadores lógicos.    


 **Test devuelve valor 0 (cierto) y valor 1 (falso)**

```bash

# Comprueba si la variable $num es igual a 1
num=1
test $num -eq 1
echo "El resultado de del comando anterior es: $?"

# Aquí solo cambia la forma en la que mostramos el resultado (comillas '')
# Puedes probar...
num=1
test $num -eq 1
echo "El resultado de 'test \$num -eq 1' es: $?"

# Hacemos ahora esta comprobación
num=2
test $num -eq 1
echo "El resultado de del comando anterior es: $?"

# Mostramos el resultado de otra forma 
num=2
test $num -eq 1
echo "El resultado de 'test \$num -eq 1' es: $?"

man test
```

**IMPORTANTE**: **cuando se usan variables** con *test*, **deben inicializarse primero.** Si se usa *test* con una variable sin inicializar, se puede producir un error.


Todas la operaciones que puede hacer *test*, **se pueden usar en estructuras de control** (como *if*, o como los bucles). **Para ello se suele usar la notación `[ ]` o `[[ ]]`, que se mostrará más adelante**

En la página del manual de *test* se especifican todas los posibles operadores que acepta *test*. Se muestran a continuación algunos de los más comunes:

| Operador               | Operación (Compara ...)                                     |
| :--------------------- | :--------------------------------------------------------- |
|  ! EXPRESION           |  la EXPRESION es falsa (NOT).                               |
|  -n STRING             |  Devuelve true si la longitud de STRING es mayor que 0.                      |
|  -z STRING             |  Devuelve true si la longitud de STRING es 0 (está vacío).       |
|  STRING1 = STRING2     |  el contenido de STRING1 es igual al de STRING2.            |
|  STRING1 != STRING2    |  el contenido de STRING1 es distinto al de STRING2.         |
|  INTEGER1 -eq INTEGER2 |  INTEGER1 es igual a INTEGER2 (valor numérico).             |
|  INTEGER1 -ne INTEGER2 |  INTEGER1 es distinto a INTEGER2 (valor numérico).          |
|  INTEGER1 -gt INTEGER2 |  INTEGER1 es mayor que INTEGER2 (valor numérico).           |
|  INTEGER1 -ge INTEGER2 |  INTEGER1 es mayor o igual que INTEGER2 (valor numérico).   |
|  INTEGER1 -lt INTEGER2 |  INTEGER1 es menor que INTEGER2 (valor numérico).           |
|  INTEGER1 -le INTEGER2 |  INTEGER1 es menor o igual que INTEGER2 (valor numérico).   |
|  -e /path/to/file      |  existe '/path/to/file'.                                    |
|  -f /path/to/file      |  existe '/path/to/file' y es un fichero regular.            |
|  -d /path/to/file      |  existe '/path/to/file' y es un directorio.                 |
|  -h /path/to/file      |  existe '/path/to/file' y es un enlace simbólico.           |
|  -r /path/to/file      |  existe '/path/to/file' y puede ser leído.                  |
|  -w /path/to/file      |  existe '/path/to/file' y puede ser escrito.                |
|  -x /path/to/file      |  existe '/path/to/file' y puede ser ejecutado.              |
|  -s /path/to/file      |  existe '/path/to/file' y no está vacío (tamaño > 0 bytes). |
|  -O /path/to/file      |  existe '/path/to/file' y soy su propietario.               | 

 
.

**Ejemplos:** 
```bash
test 6 -gt 2
echo$?        # Devuelve 0 Cierto

test 9 -lt 8
echo $?       # Devuelve 1 Falso

test ! 9 -lt 8
echo $?       # Devuelve 0 Cierto

test -f /etc/passwd 
echo $?

test -f /etc 
echo $?

# Necesario el espacio en blanco entre el corchete de abrir [ y -f
[ -f /etc ] 
echo $?

# Necesario el espacio en blanco entre el signo ! y el corchete de abrir
! [ -f /etc ]
echo $?

```


### -eq vs =

Conviene destacar que *-eq* se comporta de forma diferente a *=*, como muestra el siguiente ejemplo:

```bash
test 007 -eq 7
echo "El resultado de 'test 007 -eq 7' es: $?"

test 007 = 7
echo "El resultado de 'test 007 -eq 7' es: $?"

```

Como puede apreciarse, **"=" realiza una comparación de strings**, mientras que **"-eq" realiza una comparación numérica**


## ESTRUCTURAS CONDICIONALES (if, case) 

### IF

Se usa una estructura `if` **para evaluar el resultado de una expresión booleana**.

También se utiliza para evaluar la salida de un comando.

Sintaxis:

```bash
if condicion_A then
    comando1
    comando2

elif condicion_B then
    comando3
    comando4
... (puede haber tantos else-if como sean necesarios)

else
  comandoN
fi
```


```bash
# grep -s (silent). Suprime mensajes de error sobre ficheros que no existen o no se pueden leer
if grep -s "pull request" ./README.md; then
    echo ">>> grep OK"
else
    echo "<<< grep KO"
fi

```

Los siguientes ejemplos muestran cómo usar las **comparaciones** (equivalente al uso del comando `test`) **mediante el uso de los corchetes (`[ ]`) con una estructura *if***.

NOTA: Los corchetes o dobles corchetes no forman parte de la sentencia "if" sino que son un operador equivalente a "test" para evaluar expresiones.

Si se va a usar bash como interprete, es mucho más fácil usar siempre el comando compuesto condicional de doble paréntesis [[ ... ]], en lugar de la versión de paréntesis único compatible con Posix [ ... ]. Eso es porque dentro de un [[ ... ]] compuesto, la expansión de nombres de variables no provocan errores si no tienen valor o si contienen espacios en blanco. 

```bash
num=5

if [ $num -gt 20 ]
then
    echo "Wow, el número $num es mayor que 20!! :D"
elif [ $num -gt 10 ]
then
    echo "Bien, el número $num es mayor que 10 :)"
else
    echo "Vaya, el número $num es menor o igual que 10 :("
fi

# Con dobles corchetes [[ .. ]]

num=25
if [[ $num -gt 25 ]]
then
    echo "Wow, el número $num es mayor que 25!! :D"
elif [[ $num -gt 15 ]]
then
    echo "Bien, el número $num es mayor que 15 :)"
else
    echo "Vaya, el número $num es menor o igual que 15 :("
fi



```

El ejemplo anterior también destaca la importancia del espaciado. Por una parte, hay un espacio entre la palabra *if* o *elif* y el correspondiente corchete de apertura *[*. También hay espacios entre los corchetes (*[ ]*) y su contenido (por ejemplo *$num -gt 10*).

Además, para una mayor claridad, se han indentado los bloques de código del *if* y del *else*. 

En lugar de poner *if* y *then* en líneas distintas, se pueden poner en la misma línea usando un ';'

```bash
num=5

if [ $num -gt 20 ]; then
    echo "Wow, el número $num es mayor que 20!! :D"
elif [ $num -gt 10 ]; then
    echo "Bien, el número $num es mayor que 10 :)"
else
    echo "Vaya, el número $num es menor o igual que 10 :("
fi

```

### Operaciones booleanas

Son aplicables a todas las estructuras de control. Los siguientes ejemplos muestran su uso con *if*, pero son extrapolables a cualquier estructura de control.

```bash
# AND: &&
num=12

if [ $num -gt 10 ] && [ $num -le 20 ]
then
    echo "El número $num es mayor que 10, y menor o igual que 20 :)"
else
    echo "Vaya, el número $num es menor o igual que 10, o mayor que 20 :("
fi

# OR: ||
nombre='Perry'
apellido='Meison'

if [ "$nombre" = 'Perry' ] || [ "$apellido" = 'Foster' ]
then
    echo 'O bien el nombre es Perry, o bien el apellido es Foster'
else
    echo 'Ni el nombre es Perry, ni el apellido es Foster'
fi

# NOT: !
file='/etc/tortilla'

if [ ! -f "$file" ]
then
    echo "Vale, el fichero '$file' no existe..."
else
    echo "¡Suerte! Existe el fichero '$file'."
fi

```

.
```

Existe una forma alternativa de referirse a los operadores *&&* (AND) y *||* (OR).

```bash
# AND: -a
num=12

if [ $num -gt 10 -a $num -le 20 ]
then
    echo "El número $num es mayor que 10, y menor o igual que 20 :)"
else
    echo "Vaya, el número $num es menor o igual que 10, o mayor que 20 :("
fi

# OR: -o
nombre='Perry'
apellido='Meison'

if [ "$nombre" = 'Perry' -o "$apellido" = 'Foster' ]
then
    echo 'O bien el nombre es Perry, o bien el apellido es Foster'
else
    echo 'Ni el nombre es Perry, ni el apellido es Foster'
fi

```

**Importante:**

```bash
[ $num -gt 10 ] && [ $num -le 20 ]  # CORRECTO !!!!  (Standar POSIX)
[[ $num -gt 10 &&  $num -le 20 ]]   # CORRECTO TAMBIÉN !!!!   (bash/ksh)


[ $num -gt 10 && $num -le 20 ]   # INCORRECCTO !!!!
[ $num -gt 10 -a $num -le 20 ]   # CORRECCTO !!!! Extensión de POSIX no portable. Evitar su uso!!!!


```


### CASE

La estructura `case` **se usa para comparar una variable con una serie de patrones**. Su comportamiento también puede realizarse con *if*, pero a veces un *case* es más elegante. Veamos un ejemplo típico:
Sintaxis: 

```bash 
case cadena_texto in
  patron1) lista-compuesta1;;
  patron2) lista-compuesta2;;
  ...
  patronN) lista-compuestaN;;
  * ) lista-defecto [;;] #coincide con todo
esac
```

```bash
cd
cat <<'CASE' >case.sh
#!/bin/bash

# File: case.sh

case "$1" in
    'start')
        echo 'start...'
        ;;
    'stop')
        echo 'stop...'
        ;;
   
    'restart')
        echo 'restart...'
        ;;
    *)
        echo 'huh?'
        ;;
esac

CASE

chmod a+x case.sh
./case.sh

```

El contenido del fichero `case.h` es:

```bash
#!/bin/bash

# File: case.sh

case "$1" in
    'start')
        echo 'start...'
        ;;
    'stop')
        echo 'stop...'
        ;;
    'restart')
        echo 'restart...'
        ;;
    *)
        echo 'huh?'
    ;;
esac

```

## BUCLES (for, while, until)

### FOR

Un bucle nos permite repetir un bloque de código tantas veces como queramos. En el caso de "for" se repetirá el código que está entre **do** y **done** tantas veces como elementos haya en la lista de valores que va después de **in**.  

**Sintaxis**:
```bash
for VAR in lista_valores
do
    comando
done


for VAR in lista_valores; do
    comando
done

```
El nombre de la variable **VAR** debe aparecer obligatoriamente junto con la palabra reservada **for** en la misma línea. Y **lista_valores** debe estar obligatoriamente en la misma línea que la palabra reservada **in**.

**El bucle `for`** es un poco diferente a otros lenguajes. En *Bash* **se usa para iterar sobre un conjunto de elementos de una lista**, tomándose cada valor de dicha lista como una cadena de caracteres que puede ser objeto de expansión.

Ejemplo: 
```bash
# Itera sobre el conjunto de números dado
for i in 1 2 3 4 5
do
    echo "Item: $i"
done

```

#### Comando seq
Suele ser habitual el uso del **comando** externo **seq** para generar una lista de valores. Si bien este comando no está recogido en el estándar POSIX, es habitual su presencia en la mayoría de los sistemas UNIX/Linux. El comando `seq` presenta la sintaxis:

**seq   valor_inicial   valor_final**

siendo ambos valores números enteros. La salida del comando es la secuencia de números enteros entre ambos valores extremos indicados.

```bash
seq 1 10
``` 

**Más ejemplos de uso de for:**

```bash
# Itera sobre los nombres de fichero recogidos con 'ls'
for i in $( ls ~ ); do
    echo "Item: $i"
done

# Itera sobre los nombres de fichero recogidos en un directorio
for i in ~/* ; do
    echo "Item: $i"
done

# Itera sobre el conjunto de números determinado
for i in 1 2 3 4 5
do
    echo "Item: $i"
done

# Itera sobre los números 1 al 10
for i in $( seq 1 10 );
do
    echo "Num: $i"
done

# Recordar que la sustitución de comandos, además de la notación anterior $(),también puede emplear las comillas invertidas ( ` ` ) (back-ticks)

for i in `seq 1 10`;
do
    echo "Num: $i"
done

# Itera sobre un RANGO
for i in {1..5}
do
    echo "Num: $i"
done

# Itera sobre un RANGO, con un PASO
for i in {1..10..2}
do
    echo "Num: $i"
done

# También se puede iterar sobre elementos de distinto tipo:
for i in tortilla 7 * 8 cebolla 
do
    echo "Item: $i"
done
```

Como se acaba de ver en el bloque de código anterior, **un bucle for es muy
apropiado para recorrer el contenido de un directorio**. Usando el "*" se expande el contenido del directorio en el que estamos: 

```bash
for i in * 
do
    echo "Item: $i"
done

for i in /etc/* 
do
    echo "Item: $i"
done
```

**Podemos probar más cosas:**
```bash
for i in tortilla 7 "*" 8 cebolla 
do
    echo "Item: $i"
done

for i in tortilla 7 \* 8 cebolla 
do
    echo "Item: $i"
done

for i in tortilla 7 8 "cows are going mad"
do
    echo "Item: $i"
done
```

**También se puede usar el formato del lenguaje C:**

```bash
for (( k=0; k<=7; k++ )); do
    echo "k = ${k}"
done

```

**Observa el siguiente script y analiza lo que hace:** 

```bash 
for d in poc dev qa pre prod; do
    today="$( date '+%d-%m-%Y' )"
    now="$( date '+%T' )"
    cat <<CONFIG >config-${d}.json
{
    "deployment": {
        "environment": "${d}",
        "date": "${today}",
        "hour": "${now}",
        "team": "Data Engineering",
        "servers": {
            "bastion": {
                "ip": "192.168.1.100",
                "port": 22333,
                "proto": "ssh"
            },
            "front": {
                "ip": "${d}.web-guay.com",
                "port": 443,
                "proto": "https"
            },
            "db": {
                "ip": "192.168.10.80",
                "port": 3306,
                "proto": "mariadb"
            }
        }
    }
}
CONFIG

done

ls -l
```

**Ejercicio 1:** 

Fijandote en el ejemplo anterior, crear un script que genere una serie de ficheros, uno para cada item de los contenidos en la siguiente lista: poc, dev, qa, pre, prod.  El nombre de cada fichero debe tener el siguiente formato: config-"item_de_la_lista".json. Por ejemplo: "config-poc.json"

Y el contenido de cada fichero debe ser: 
{
 "environment": "${d}",
 "date": "${today}",
 "hour": "${now}"
}

La solución que ves a continuación usa el método here-document. Puedes hacerlo editando el fichero y escirbiendo el codigo correspondiente.
 
```bash
#!/bin/bash

for d in poc dev qa pre prod; do
    today="$( date '+%d-%m-%Y' )"
    now="$( date '+%T' )"
    cat <<CONFIG >config-${d}.json
        {
        "environment": "${d}",
        "date": "${today}",
        "hour": "${now}"
        }
CONFIG
done

ls -l

```


### WHILE

**El bucle `while` ejecuta un bloque de código mientras se cumpla una condición (expresión de control)**. Termina su ejecución cuando la condición deja de ser cierta (o cuando se ejecuta *break*).

Sintaxis: 
```bash
while condicion
do
   cmd1
   cmd2
done


while condicion; do
   cmd1
   cmd2
done

```
**Ejemplo:**

```bash
COUNTER=0
while [ $COUNTER -lt 10 ]; do
    echo "El contador es: $COUNTER"
    (( COUNTER++ )) 
done
```

**Se puede hacer un *while* infinito de varias formas. A continuación se muestra una típica utilizando los ":" como condición del while**. 


```bash
cat <<'INF' >infinite_while.sh
#!/bin/bash

# File: infinite_while.sh
# El comando read espera leer algo desde la entrada estándar. 
# Los ":" como condición del while indican que siempre se cumple la condición. Equivalente a "true"

while :
do
    echo 'Dime algo, o pulsa CTRL + C para salir:'
    read STR
    echo ">>> $STR"
done

INF

chmod a+x infinite_while.sh
./infinite_while.sh

```

El contenido de *infinite_while.sh* es:

```bash
#!/bin/bash

# File: infinite_while.sh
# El comando read espera leer algo desde la entrada estándar. 
# Los ":" como condición del while indican que siempre se cumple la condición. Equivalente a "true"
while :
do
    echo 'Dime algo, o pulsa CTRL + C para salir:'
    read STR
    echo ">>> $STR"
done

```

####Comando READ. Como leer un archivo linea a línea en bash: 

Es común usar un bucle *while* para leer (y procesar) cada línea de un fichero y utilizar el comando  *read* para leer dicho fichero. 

**Esta es la forma de hacerlo:**

```bash
while read linea; do
	echo -e "Línea: $linea"
done < fichero
```

**El archivo de entrada (“fichero”) es el nombre del archivo que deseamos que esté abierto para su lectura mediante el comando “read”**. 

Dicho comando **lee el archivo línea por línea, asignando cada línea a la variable "línea"**.

Una vez que se procesan todas las líneas, el bucle “while” terminará. El separador de campo interno (IFS) se establece en la cadena nula para conservar los espacios en blanco iniciales y finales, que es el comportamiento predeterminado del comando de lectura (read).

**Veamos un ejemplo**:

- **Creamos un fichero de registros que usaremos de entrada para lectura** linea a linea mediante el comando read. En el primer campo de cada registro (línea) se indica el nombre de una banda de música. 

```bash
# Creamos un fichero de registros (tipo csv, cuyos campos estan separados por ";"). Este fichero sera el que usaremos de entrada para leer linea a linea mediante el comando read.

# El formato de cada línea de este fichero es:
# Nombre_de_banda_de_musica;año;nombre_disco1;discografica

cat <<'RECORDS' >records.csv
His Hero is Gone;1996;Fifteen Counts of Arson;Prank Records
His Hero is Gone;1997;Monuments to Thieves;Prank Records
His Hero is Gone;1998;The Plot Sickens;The Great American Steak Religion
Tragedy;2000;Tragedy;Tragedy Records
Tragedy;2001;Tragedy;Skuld Releases
Tragedy;2002;Can We Call This Life?;Tragedy Records
Tragedy;2002;Vengeance:Tragedy Records
Tragedy;2003;Vengeance:Skuld Releases
Tragedy;2003;Split 7" with Totalitär;Armageddon Label
Tragedy;2004;UK 2004 Tour EP;Tragedy Records
Tragedy;2006;Nerve Damage;Tragedy Records
Tragedy;2012;Darker Days Ahead;Tragedy Records
Tragedy;2018;Fury;Tragedy Records
RECORDS

```

- Ya tenemos el fichero que vamos a leer, **ahora crearemos un script (records_count.sh) que leerá y procesará cada una de las líneas del fichero de entrada** records.csv **y debe contar cuantos registros (líneas) hay de cada una de las bandas**. 
- Para ello utilizará el primer campo de cada línea y comprobara a que banda pertenece: "His Hero is Gone" o "Tragedy". 
- Cuando se cumpla un determinado patron, el script incrementerá la variable correspondiente que lleva la cuenta de las líneas que pertenecen a cada banda.
- Al final el script debe imprimir el total del número de líneas que pertenecen a cada banda.

**El contenido del script records_count.sh seria el siguiente**:

```bash 

#!/bin/bash

# Inicializamos los contadores en los que vamos a contar las líneas que cumplen cada patrón.

# Contador de líneas que cumplen el patron "His Hero is Gone"
HHIG=0

# #Contador de líneas que cumplen el patron "Tragedy"
TGDY=0

# Leemos cada línea del fichero "records.csv" en un bucle while y la almacenamos en la variable "f"
while read f; do
    band="$( echo "${f}" | cut -d';' -f1 )"
    case ${band} in
        'His Hero is Gone')
            (( HHIG++ ))
            echo ">>> $HHIG" ;;
        'Tragedy')
            (( TGDY++ ))
            echo ">>> $TGDY" ;;
        *)
            echo '>>> Banda desconocida... ($f)' ;;
    esac
done < records.csv

echo ">>> Total HHIG = ${HHIG}"
echo ">>> Total TGDY = ${TGDY}"

```

Asignamos permiso de ejcución y ejecutamos el script
```bash
chmod a+x records_count.sh
./records_count.sh

```


### UNTIL

El bucle `until` es muy similar al bucle *while*, pero *until* s**e ejecuta mientras la condición es falsa, y termina su ejecución cuando la condición se vuelve verdadera**.

```bash
COUNTER=20
until [ $COUNTER -lt 10 ]; do
    echo "La variable COUNTER = $COUNTER"
    (( COUNTER-=1 ))
done

```

Observar que el código anterior devuelve el mismo resultado que el siguiente usando el bucle `while`.  

```bash
COUNTER=20
while [ $COUNTER -gt 9 ]; do
    echo "La variable COUNTER = $COUNTER"
    (( COUNTER-=1 ))
    echo $COUNTER
done

```
## RUPTURA DE SENTENCIAS DE CONTROL (break, continue)

Se utilizan para romper el funcionamiento normal de las estructuras repetitivas `for`, `while` y `until`.

### BREAK

Como en otros lenguajes, en *Bash* la sentencia `break` **interrumpe la ejecución de un bucle**, ya sea *for*, *while* o *until*. **Break detiene todas las iteraciones restantes** de la estructura de control.

```bash
for i in {1..10..2}
do
    if [ ${i} -gt 5 ]; then
        break
    fi
    echo "Num: ${i}"
done

echo 'Hecho!'
```

### CONTINUE

*Bash* también incorpora una sentencia *continue*, que dentro de un bucle, **deja de ejecutar la iteración actual** y pasa a la siguiente iteración.

```bash
for i in {1..10..2}
do
    if [ ${i} -eq 5 ]; then
        continue
    fi
    echo "Num: ${i}"
done

echo 'Hecho!'
```

## Algunos consejos sobre if, while, until, for, corchetes simples y dobles
Recordar que los corchetes de la expresión de test, tienen que tener **espacio** a ambos lados, si no --> fallo
````
i=0
while [ $i -lt 10]; do  --> fallo [:missing ']'
	echo " $i"
	((++i))
done

while [$i -lt 10 ];do --> fallo [: comand no found

salir=0
while [ $salir -lt 1] --> fallo linea 7 [: -lt: se esperaba operador unario

````

Si se hace if con nada entre el then y el fi también se produce fallo, si se está haciendo un esqueleto insertar de forma provisional algo entre el then y el fi, ej un echo "". 

```bash
if [ $i -lt 10 ]; then  --> fallo
fi
```

Recordar la forma de incluir varias condiciones en un if (|| -o o  && -a):
```bash
if [ $nombre = "juan"] || [ $nombre = "pepe" ]; then .... 
```
--> Si nombre es juan o pepe se ejecuta el then

**Los corchetes dobles:**
- 1. Admiten la comparación de dos variables; ```bash if [[ $a > $b ]] ```
	En anteriores vesiones de bash los corchetes simples sólo admiten una variable (operador unario)  if [ $a -eq 1 ] 
- 2. Los dobles corchetes son mas potentes que los sencillos: 
    Se pueden usar operadores || && y con cadenas <>
    Se pueden usar comodines \*
- 3. Admite operaciones con doble paréntesis dentro del doble corchete:
```bash  if [[ $(($1 % 2 )) = 0 ]]```



## DATOS DE ENTRADA A UN SCRIPT

Además de los argumentos de entrada ya vistos anteriormente, o de las señales que se le pueden enviar (*signals*), un *script* puede tomar su entrada de otras fuentes:

- Utilizando **la estructura `while`** para leer las líneas de un fichero mediante el **comando `read`** (tal como ya hemos visto al estudiras la estructura "while"). 
- El usuario puede **introducir datos manualmente** al *script* (desde la línea de comandos, **haciendo uso** también del **comando `read`**.
- **La salida de otro comando** o programa se puede usar como entrada al *script*.

### Comando read

**Se usa `read` cuando se quiere que el usuario introduzca algún valor, por la entrada estandar**, típicamente por teclado.

```bash
cd
cat <<'READUSER' >read_user.sh
#!/bin/bash

# File: read_user.sh

echo "Hola, quién eres?"
read NOMBRE
echo "Encantada de conocerte, $NOMBRE"
READUSER

chmod a+x read_user.sh
./read_user.sh

```

El contenido del fichero *read_user.sh* sería:

```bash
#!/bin/bash

# File: read_user.sh

echo "Hola, quién eres?"
read NOMBRE
echo "Encantada de conocerte, $NOMBRE"
```


El comando *read* tiene **algunas opciones interesantes**, como **-p** o **-s**:

- (-p): no se imprime un salto de línea después del mensaje de prompt a la espera de recibir el valor de entrada
- (-s): no se hace echo de lo que se esta escribiendo en el terminal 
- Las dos opciones se pueden usar a la vez: read -sp

```bash
#!/bin/bash

# File: login.sh

read -p 'Username: ' LOGIN_USERNAME
read -sp 'Password: ' LOGIN_PASSWORD
echo
echo "Gracias $LOGIN_USERNAME, ahora puedes acceder al sistema"
echo "Aunque no hayas visto la contraseña, está aqui: $LOGIN_PASSWORD"

```

El comando *read* **permite leer varias variables a la vez**:

```bash
read var1 var2 ... varn
```

Veamos el siguiente ejemplo: 
```bash
#!/bin/bash

# File: planets.sh

echo 'Dime tres planetas: '
read planet1 planet2 planet3
echo "Primero visitaremos ${planet1}, para enterarnos de qué va esto."
echo "Si necesitamos seguridad, podemos pasar por ${planet2}."
echo "Nos reuniremos con el resto del equipo en ${planet3}."

```


La siguiente tabla muestra algunas de la **opciones más comunes** de `read`:

| Opción                 | Descripción                                                 |
| :--------------------- | :--------------------------------------------------------- |
|  -a ARR_NAME           |  Las palabras son asignadas en orden a los índices del *array* ARR_NAME (ojo, sobreescribe ARR_NAME). |
|  -d DELIM              |  Se usa el carácter DELIM para finalizar la operación de *read* (por defecto *read* termina cuando se pulsa *ENTER*, es decir, con */n*). |
|  -n NUMCHARS           |  El comando *read* finaliza después de haber leído NUMCHARS caracteres. |
|  -p PROMPT             |  Muestra el mensaje PROMPT delante de la entrada de texto. |
|  -s                    |  Modo silencioso: no se muestra el texto que se escribe (útil con contraseñas). |
|  -t TIMEOUT            |  Hace que *read* termine y devuelva un error, si no se completa la entrada antes de TIMEOUT segundos. |
|  -u FD                 |  Lee desde el descriptor de fichero FD. |

En la página de manual de *read* se pueden ver todas las posibilidades que ofrece.

```bash
man read
```

También se puede usar en bucles, para hacer programas en los que el *prompt* invita a escribir:

```bash
#!/bin/bash

# File: teclea.sh

STR='vamos'
while [ "$STR" != 'q' ]; do
    echo 'Dime algo, o teclea q para salir:'
    read STR
    echo ">>> $STR"
done

```

### Leer desde STDIN

**Cuando un *script* lee desde *STDIN*, se permite que otros comandos puedan enviarle su salida (usando un *pipe* o '|').**

Como ya se ha explicado, *Bash* usa unos (descriptores) ficheros especiales para la entrada estandar (STDIN), la salida estándar (STDOUT) y la salida de errores (STDERR). 

Cada proceso tiene sus ficheros *STDIN*, *STDOUT* y *STDERR*, y pueden ser relacionados con los ficheros *STDIN*, *STDOUT* y *STDERR* de otro proceso. Esto ocurre cuando usamos un *pipe* (|) o una redirección. Así, cada proceso tiene su propio conjunto de ficheros especiales:

- STDIN  - /proc/<PID>/fd/0
- STDOUT - /proc/<PID>/fd/1
- STDERR - /proc/<PID>/fd/2

Para simplificar el acceso a estos ficheros dede los procesos, cada proceso puede usar estos atajos:

- STDIN  - /dev/stdin  o /proc/self/fd/0
- STDOUT - /dev/stdout o /proc/self/fd/1
- STDERR - /dev/stderr o /proc/self/fd/2

Así que si un *script* necesita leer datos a través de un *pipe*, todo lo que debe hacer es leer el fichero correspondiente. Aunque estos ficheros son algo especiales, se comportan como cualquier otro fichero en Linux.

**Lo vemos con un ejemplo:**

Haremos dos scripts: 
- writer.sh escribe una serie de líneas en la pantalla (salida estándar)
- reader.sh lee lo que le llega por la entrada estándar

script writer.sh:
```bash
#!/bin/bash

# writer.sh

for i in {0..5}; do
     echo "line ${i}"
done
```

script: reader.sh
```bash
#!/bin/bash

# reader.sh

while read line; do
     echo "reading: ${line}"
done < /dev/stdin
```

Podemos hacer que el script writer.sh envíe su salida estandar a través de una tuberia a la entrada estandar de reader.sh  

```bash
chmod a+x reader.sh writer.sh
./writer.sh | ./reader.sh
```

Veamos otro ejemplo: 
- Crearemos un fichero de registros con el mismo formato que en el ejemplo visto anteriormente: Nombre_de_banda_de_musica;año;nombre_disco1;discografica

 
```bash

$ cat <<'RECORDS' >records.csv
His Hero is Gone;1996;Fifteen Counts of Arson;Prank Records
His Hero is Gone;1997;Monuments to Thieves;Prank Records
His Hero is Gone;1998;The Plot Sickens;The Great American Steak Religion
Tragedy;2000;Tragedy;Tragedy Records
Tragedy;2001;Tragedy;Skuld Releases
Tragedy;2002;Can We Call This Life?;Tragedy Records
Tragedy;2002;Vengeance:Tragedy Records
Tragedy;2003;Vengeance:Skuld Releases
Tragedy;2003;Split 7" with Totalitär;Armageddon Label
Tragedy;2004;UK 2004 Tour EP;Tragedy Records
Tragedy;2006;Nerve Damage;Tragedy Records
Tragedy;2012;Darker Days Ahead;Tragedy Records
Tragedy;2018;Fury;Tragedy Records
RECORDS

```

- Creamos ahora el script que recibe por su entrada estandar el fichero de registros, lee cada línea y la procesa para obtener los campos que hacen referencia al año y el nombres del disco (campos 2 y 3 de cada línea). 

Contenido del script: filter_csv.sh 
 
```bash
#!/bin/bash

# File: filter_csv.sh
echo '>>> Discos:'
echo
cat /dev/stdin | cut -d';' -f 2,3 | sort

```
```bash 
chmod a+x filter_csv.sh
cat records.csv | ./filter_csv.sh

```

## FUNCIONES

Como en otros lenguajes de programación, en *Bash* se pueden declarar funciones para agrupar bloques de código. Una cosa es declarar una función y otra ejecutar una función. Par usar una función o invocarla es preciso haberla declarado/definido antes. 

En Bash, **se puede definir una función en una sola línea**, en este caso es muy importantetener en cuenta los espacios después de la primera llave y antes del primer comando, y antes de la última llave. Es decir, hay que dejar espacio entre las llaves y los comandos para que de un error. Además entre comandos debes, utilizar ";" incluso después del último comando. Ejemplo:
 

```bash

mi_funcion() { echo "hola mundo"; echo "adios mundo"; }

```
**Otra opción para declarar una función es utilizando la palabra clave `function`.** De esta manera la declaración de tu función tendría el siguiente aspecto:

```bash
function mi_primera_funcion(){
    echo Hola Mundo
}
```
**Para hacer uso de una función, solo tenemos que invocarla por su nombre**.  
```bash
mi_primera_funcion
```

Veamos un ejemplo sencillo:

```bash
#!/bin/bash

# File: saludamos.sh
saluda() {
    echo "Hola, terrícola"
}

echo "Invocamos función saluda()...."
saluda

```

### Valor de retorno de una función. Comando return

Como el resto de comandos en *Bash*, una función devuelve un número entero indicando el estado de salida. Típicamente un valor de retorno de 0 implica una ejecución correcta. Un valor mayor que 0, implica que hubo errores durante la ejecución.

**Para devolver un valor se usa la sentencia `return`.**

```bash
#!/bin/bash

# File: saludamos2.sh

# Funcion saluda()
saluda() {
    echo "Hola, terrícola"
    return 42
}

echo "Invocamos funcion saluda()..."
saluda
echo ">>> saluda() ha devuelto: $?"


# Funcion saluda_mas()
saluda_mas() {
    echo "Hola terrícola, es un placer recibirte a bordo de nuestra nave"
}

echo "Invocamos funcion saluda_mas()..."
saluda_mas
echo ">>> saluda_mas() ha devuelto: $?"

```

### Valores de salida de una Función

En realidad, **aunque una función sólo devuelva un entero, puede producir salidas de varias formas**:

- Puede modificar variables externas.
- Puede invocar el comando *exit* para terminar el *script*.
- Puede devolver el resultado, utilizando el comando *echo* y cuyo valor se guarda en una variable usando el método command-substittion o sustitución de comandos: `miVar=$( miFuncion )`.

Veamos algunos ejemplos de **como devolver el resultado de una función:**

**Ejemplo 1**:

```bash
#!/bin/bash

# File: suma_valor.sh

suma_diez() {

        read -p "Introduce un valor: " value

# El resultado se devuelve mediante echo dentro de la función. 
        echo $(($value + 10))

}

# Invoca la función mediante el metodo de sustitución de comando. 
result=$(suma_diez)

echo "Resultado de la funcion: $result"

```

**Ejemplo 2**: 

```bash
#!/bin/bash

# File: datos_so.sh

# Define osname()
function osname() {
    local os="$( uname -s )"
    local ver="$( uname -r )"
    echo "${os} ${ver}"
    return 17
}

# Invoca a osname()
osname
echo ">>> osname() ha devuelto: $?"

# Podemos invocar la función y almacenar 
n=$( osname )

echo ">>> n = ${n}"


```

**Ejemplo 3**: 

```bash
#!/bin/bash

# File: saludando.sh

saluda() {
    echo "Hola, terrícola"
}

saluda
echo ">>> Invocar saluda() ha devuelto: $?"

s=$( saluda )
echo ">>> El resultado de invocar mediante sustitución de comando es = ${s}"

```

### Ambito de las variables
`VARIABLES`: Como se puede ver en los ejemplos siguientes, cuando se declara una variable en un script, por defecto su **ámbito es global**, y puede ser leída o modificada dentro de una función. Se pueden declarar variables de **ámbito local**, que sólo existen dentro de una función, con la expresión **local**.

```bash
#!/bin/bash

# File: program_demo.sh

# Ámbito de variables

# Inicializamos dos variables (globales)
num=10
val=5
echo -e ">>> num = ${num}\tval = ${val}\t\t[1]"

# Definimos funcion varDemo
function varDemo() {
    local num=42
    echo -e "varDemo(): num = ${num}\tval = ${val}\t[2]"
    
    # Modificamos el valor de "num" (local)
    num=51
    # Modificamos el valor de "val" (global)
    val=7
}

echo -e ">>> num = ${num}\tval = ${val}\t\t[3]"

# Invocamos Funcion
varDemo
echo ">>> varDemo() ha devuelto: $?"
echo -e ">>> num = ${num}\tval = ${val}\t\t[4]"

# Invocamos de nuevo la función mediante el método de sustitución de comando
v=$( varDemo )
echo ">>> Resultado invocación método de comando v = ${v}"


##-------------------------------------

function varDemo_other() {
    alpha=42
    beta=24
    echo -e "varDemo_other(): alpha = ${alpha}\tbeta = ${beta}\t[5]"
}

echo -e ">>> alpha = ${alpha}\tbeta = ${beta}\t[6]"

v=$( varDemo_other )
echo ">>> Resultado invocación método de comando v = ${v}"

```
### Paso de Parámetros especiales/posicionales

A la función se le pueden pasar los mismos parámetros que puede recibir un script: $0, $1-$9, $#, $?, $@, $$ 

**IMPORTANTE**: Consideraciones a tener en cuenta que el parámetro $0 siempre hace referencia al nombre del script (no de la función) y el parámetro $$ hace referencia al ID del proceso del script. 

Veamos un ejemplo:
```bash
#!/bin/bash

# File suma_parametros.sh
addnum() {
      
        if [ $# != 2 ]; then

           echo "Número de parámetros incorrecto"

        else
          # Los sumo
          echo $(($1 + $2))
        fi
}

echo -n "Sumando 10 y 15: "
value=$(addnum 10 15)
echo $value


echo -n "Sumando otros números: "
value=$(addnum 10)
echo $value

```


## Variables (II)

Como se ha visto, las variables, por defecto, tienen ámbito global y no tienen un tipo específico. Pero mediante el uso de expresiones como **local**, se puede restringir su ámbito. También hay formas de restringir su contenido, o si se prefiere, su tipo.

### declare

Se **puede especificar el tipo de valor que puede almacenar una variab**le con la expresión `declare`, como se muestra a continuación:

| Opción  | Descripción                                                        |
| :-------| :----------------------------------------------------------------- |
|  -a     |  La variable es un *array*.                                        |
|  -f     |  La variable sólo contiene nombres de funciones.                   |
|  -i     |  La variable será tratada como un número entero.                   |
|  -p     |  Muestra los atributos y valores de cada variable.                 |
|  -r     |  La variable es de sólo lectura (constante).                       |
|  -x     |  Marca la variable para ser exportada.                             |


Veamos algunos ejemplos con el uso de `declare`: 
```bash
#!/bin/bash

# declaro una variable de tipo entero
declare -i NUM=42

echo ">>> NUM = ${NUM}"

# Veamos que pasa si a una variable de tipo entero le asigno un string
NUM='tortilla'
echo ">>> NUM = ${NUM}"

# Muestra atributos y valores de la variable
declare -p NUM

```

### readonly

Otra forma de **especificar que una variable no se puede modificar**, es marcarla como de sólo lectura.

Veamos un ejemplo: 
```bash
#!/bin/bash

readonly NUM=42

echo ">>> NUM = ${NUM}"

# Arrojará un error
NUM=56
echo ">>> NUM = ${NUM}"

```

## ARRAYS

En *Bash*, como en otros lenguajes, los *arrays* son colecciones de elementos, cada uno de los culaes se alamacena en una de las posiciones de dicho array. En *Bash* los elementos pueden ser de diferentes tipos. A continuación se muestran algunas operaciones típicas con *arrays*:

### Declarando un "array"
En Bash existen diferentes formas de declarar un array,

- Asignando valores, por ejemplo: ```colores[0]='rojo'```
- Utilizando la palabra clave **declare**, de la forma: ```declare -a colores```
- O directamente con el siguiente formato  ```colores=('rojo' 'azul')```

Los elementos del array pueden ir sin comillas, salvo que alguno de los elemntos lleve un espacio en blanco, en cuyo caso las comillas son necesarias o bien es necesario escapar el espacio. 


```bash
#!/bin/bash

# 3 formas de crear un array:
colores[0]=rojo
miArr[7]=33

declare -a colores

colores('rojo' 'azul')
planetas=('Júpiter' 'Urano' 'Venus')

```
### Asignar un valor a una posición de un "array"
Los elementos de los arrays en Bash empiezan a contar en la posición 0, aunque al hacer una asiganión podemos cambiar los índices indicandolo explicitamente. 

```bash
#!/bien/bash
# Asignar un valor a una posición del array:
miArr[7]=33
frutas=(melon piña sandia)
frutas=([2]=melon [1]=piña [0]=sandia)

```
### Accediendo a un "array"

- **Para acceder de forma individual a cada una de los elementos de un array**, tienes que utilizar la siguiente sintaxis, ```${array[i]}```. Así por ejemplo para imprimir el primero de los colores, utilizarías echo ${colores[0]}.

- Si lo que utilizamos es solo el nombre del array, es decir,  ```echo $array``` **obtendremos el primer elemento del array**. En nuestro ejmplo echo $colores.

- Si en lugar de imprimir un solo elemento en concreto, **queremos imprimirlos todos en la misma línea**, entonces debemos ejecutar echo ```${array[@]}```. En nuestro caso ${colores[@]}.

- **Los índices especiales: @ y * funcionan igual cuando no se usan las comillas dobles**. Si usamos comillas, la @ devuelve los elementos del array separados por blancos mientras que el * devuelve los elementos del array separados por el caracter IFS (Internal Field Separator). 


```bash
#!/bin/bash

colores=('rojo' 'azul')
planetas=('Júpiter' 'Urano' 'Venus')
miArr[7]=33

# Acceder a los elementos de un array:
echo "colores[1]: ${colores[1]}"
echo '$colores:' "$colores"
echo "colores[@]: ${colores[@]}"
echo "miArr[7] = ${miArr[7]}"

# Para imprimir cada elemento del array en una línea es necesario utilizar un bucle
for i in "${colores[@]}"
do
    echo $i
done

# También se puede acceder a varios elementos del array. Este ejemplo extrae dos elementos empezando en la posición 1
echo ${planetas[@]:1:2}


# Ejemplos de uso de @ y *.  Ambos funcionan igual cuando no se usan las comillas dobles. Si usamos comillas, la @ devuelve los elementos del array separados por blancos mientras que el * devuelve los elementos del array separados por el caracter IFS. 
IFS=,
echo ${colores[@]}
echo ${colores[*]}
echo "${colores[@]}"
echo "${colores[*]}"


echo '-----------------------'

# Más ejemplos
echo "planetas[*] = ${planetas[*]}"
echo "planetas[2] = ${planetas[2]}"
planetas[3]='Saturno'

echo '-----------------------'

```
### Tamaño de un "array" y elementos no nulos de un "array"
- **Para saber cuantos elementos tienen un array** en Bash, debemos utilizar ```${#array[@]}```. Así, en el ejemplo que estamos utilizando de los colores, si quieres conocer cuantos elementos hay en dicho array tienes que utilizar echo ${#colores[@]}.

- **Si queremos saber la longitud de uno de los elementos del array**, entonces debemos referirnos a la posición de dicho elemento: ```${#array[0]}```. En nuestro ejemplo, si ejecutas echo ${#colores[0]} te devolverá 4, el número de letras de la palabra 'rojo'.

- **Si queremos saber que elementos son no nulos** debemos usar: ```${!array[@]}```. Nos devuelve los índices de los elementos no nulos.  

```bash
#!/bin/bash

planetas=('Júpiter' 'Urano' 'Venus')
planetas[3]='Saturno'


# Ejemplos de contenido, tamaño e índices no nulos de un array.  
echo "Contenido del array:   planetas[@] = ${planetas[@]}"
echo "Tamaño del arary:   planetas[@] = ${#planetas[@]}"
echo "Índices no nulos:    !planetas[@] = ${!planetas[@]}"

# Veamos que pasa ahora
planetas[9]='Neptuno'
echo "Contenido del array:   planetas[*] = ${planetas[*]}"
echo "Tamaño del array:      planetas[*] = ${#planetas[*]}"
echo "Índices no nulos del array:    !planetas[*] = ${!planetas[*]}"

# Tamaño de un array (número de elementos):
echo "El array 'planetas' tiene: ${#planetas[*]} elementos."

# Tamaño del contenido de una de sus posiciones:
echo "planetas[1] tiene: ${#planetas[1]} caracteres/dígitos."

```

### Eliminar un array o una posición del array

Se utiliza el comando **unset** seguido de la posi´ción del array que se desea eliminar o seguido del nombre del array.

```bash
#!/bin/bash
planetas=('Júpiter' 'Urano' 'Venus')

# Eliminar una posición
unset planetas[2]
echo "planetas[2] = ${planetas[2]}"

# Ha desparecido "Veus"
echo "planetas[@]= ${planetas[@]}"


# Eliminar un array
unset planetas
echo "planetas[*] = ${planetas[*]}"
```


### Un *array* puede generarse a partir de la salida de un comando:

```bash
#!/bin/bash

lista=( $( ls ~ ) )
echo "${lista[*]}"
echo "${#lista[*]}"

```

### Uso de bucles para recorrer un "array"

Es común recorrer los elementos de un *array* con un bucle *for*:

```bash
#!/bin/bash

# Creamos un array 
numeros=(1 2 3 4)

# Para iterar sobre todos los elementos del array hay que convertirlo primero en una lista mediante el uso de "@", es decir usano ${array[@]}. 

for num in ${numeros[@]}; do
    echo -e "num = ${num}"
done

```

### Arrays y operaciones con variables

```bash
#!/bin/bash
# Sintaxis para recuperar el valor de un array, siendo el indice una variable y guardando el resultado en otra variable
array[1]=5
indice=1
var=${array[indice]} 
echo "El valor de array[1] es $var"

```

```bash
#!/bin/bash
# Operaciones con elementos de un array
indice=2
array[indice]=10
array[3]=20
array[4]=30
suma=$((array[3] + array[4]+ array[indice]))
echo $suma

```

```bash
#!/bin/bash
# Añadir elementos a un array, después del último elemento,  incluso cuando hay indices vacíos

array[0]=1; array[3]=2
echo "1. Contenido array: ${array[@]}"; echo "1. Índices no nulos: ${!array[@]}"

# Añade un elemento (6) en la siguiente posición vacia a la última
array+=(6)
echo "2. Contenido array: ${array[@]}"; echo "2. Índices no nulos: ${!array[@]}"

```

```bash
#!/bin/bash
# Si se asigna "" a una posición de un array, dicha posición no es nula, es decir, el índice no desaparece

array[3]=""
echo "3. Contenido array: ${array[@]}"; echo "3. Índices no nulos: ${!array[@]}"

```



## Algunos recursos útiles

- [Bash conditional expressions](https://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html)
- [Operadores de comparación](https://www.tldp.org/LDP/abs/html/comparison-ops.html)
- [Operadores aritméticos](https://www.tldp.org/LDP/abs/html/ops.html)
- [RedHat Certified System Administration Notes](https://codingbee.net/rhcsa)
