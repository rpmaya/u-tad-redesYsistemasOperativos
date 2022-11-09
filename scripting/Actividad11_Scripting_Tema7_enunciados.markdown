#Actividad 11 – Scripting - Tema 9

 
## Ejercicio 1
Basándonos en los resultados de ejecutar el comando ip a, realizar un script que imprima:
- La dirección MAC, situada a la derecha de link/ether, excluyendo cualquier otro texto asociado. El script empleará, entre otros, el comando de manejo de ficheros awk.
- La dirección IP, situado a la derecha de inet, excluyendo cualquier otro texto asociado. El script usará, entre otros, los comandos de manejo de ficheros awk y tr.
Los comandos tienen que ser universales, válidos para cualquier dirección IP y MAC posibles.


```bash


```
## Ejercicio 2
Realizar un *script* que sea una calculadora, en bucle infinito, con dos operaciones: Potenciación y conversor de grados Centígrado a Farenheit. 

La selección de la operación se realizará mediante una sentencia case. Se presentará un menú de selección de las dos operaciones:  
- 1: Potenciación 
- 2: Conversor de grados Centígrados a Farenhei

A continuación, mediante los comandos echo / read se pedirán los parámetros necesarios, en el caso de potenciación la base y el exponente (base^exp), y en el caso del conversor de temperatura, el valor de los grados Centigrados. 

La función exponenciación se realizará mediante un bucle FOR, no siendo válida la sentencia que utilice la operación aritmética: base**exponente.

La fórmula de conversión de ºC a ºF es; F = [18*C + 320] /10
Nota. 10 grados Centígrados son 50º grados Farenheit
La puntuación máxima se obtendrá con detección de parámetros introducidos erróneos.



```bash


```
## Ejercicio 3

Realizar el ejercicio 2, pero la selección de la operación será mediante sentencias if

```bash

```


## Ejercicio 4

Realizar el ejercicio 2, pero la selección de la operación será mediante case, y cada una de las operaciones será una función.

```bash

```

## Ejercicio 5

Imprimir "n" números aleatorios entre 1 y 1000. Partiendo de un numero generado por $RANDOM y hacer el módulo de la división. 

EL número n se pasará mediante el comando read. 

```bash

```

## Ejercicio 6
Determinar si existe un fichero, que se introduce por teclado, en tu directorio de trabajo por defecto ($HOME), si no existe crearlo.

```bash 

```
