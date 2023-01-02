#Actividad 10 – Scripting - Tema 9

 
## Ejercicio 1

Hacer un *script* que reciba dos parámetros numéricos de entrada y devuelva como resultado la suma de los dos números. 
El script debe hacer la comprobación de que tiene los dos valores para realizar la operación solicitada.  Por tanto: 
- Si el número de parametros que recibe el script es cero, entonces el script debe pedir al usuario los dos valores mediante el comando read. 
- Si el número de parametros que recibe es uno, entonces el script debe pedir al usuario el segundo de los valores que falta utilizando el comando read.
- Si el número de parametros es dos o más de dos, entonces el script utilizará los dos primeros valores recibidos para ejecutar la suma.

La comprobación del número de parámetros recibidos se hara mediante una sentencia case. 
El script debe devolver el resultado de la suma. 

```bash


#!/bin/bash
# File suma_valores.sh

clear
error="n"

case $#
in
        0) 
        echo "No ha introducido ningun parámetro..."
        read -p 'Introduca el valor de num1: ' num1
        read -p 'Introduca el valor de num2: ' num2
        if [ -z $num1 ] || [ -z $num2 ]
            then 
                error="s"
                echo "Alguno de los valores introducido no es correcto"
        fi
        ;;
  
        1)
        echo "Falta un parámetro..."
        num1=$1
        read -p 'Introduca el valor de num2: ' num2
        if [ -z $num2 ]
            then
                error="s"
                echo "El valor introducido no es correcto"
        fi
        ;;
        2)
        echo "Número de parámetros correcto"
        num1=$1
        num2=$2
        ;;
        *)
        num1=$1
        num2=$2
        echo "Demasiados parametros, utilizo los 2 primeros"
        ;;
esac


if [ $error = "s" ]
then 
        echo "Nos salimos, no podemos hacer la suma....."
        exit
else
        echo "Hago la suma de $num1 + $num2"
        echo $num1 + $num2 = $(($num1 + $num2))
fi


```
 Otra forma de hacerlo, comprobando que los valores recibidos son numéricos: 

```bash 

#!/bin/bash

# File: suma_valores2.sh
clear
error="n"
es_numero='^[0-9]$'

case $#
in
        0) 
        echo "No ha introducido ningun parámetro..."
        read -p 'Introduca el valor de num1: ' num1
        read -p 'Introduca el valor de num2: ' num2
        if [ -z $num1 ] || [ -z $num2 ]
            then 
                error="s"
                echo "Alguno de los valores introducido no es correcto"
        fi
        ;;
  
        1)
        echo "Falta un parámetro..."
        num1=$1
        read -p 'Introduca el valor de num2: ' num2
        if [ -z $num2 ]
            then
                error="s"
                echo "El valor introducido no es correcto"
        fi
        ;;
        2)
        echo "Número de parámetros correcto"
        num1=$1
        num2=$2
        ;;
        *)
        num1=$1
        num2=$2
        echo "Demasiados parametros, utilizo los 2 primeros"
        ;;
esac

if [ $error = "s" ]
then 
        echo "Nos salimos, no podemos hacer la suma....."
        exit

elif ! [[ $num1 =~ $es_numero ]] || ! [[ $num2 =~ $es_numero ]]
        then
                echo "Alguno de los valores no es un número"
        else
                echo "Hago la suma de $num1 + $num2"
                echo $num1 + $num2 = $(($num1 + $num2))

fi

```
## Ejercicio 2

Hacer un script que haga la suma de dos números recibidos como parámetros comprobando antes que  los valores recibidos son numéricos

```bash

#!/bin/bash

# File: suma_nueva.sh

error="n"

num1=$1
num2=$2
echo "Valor de num1: " $num1
echo "Valor de num2: " $num2

[ -n "$num1" ] && [ "$num1" -eq "$num1" ] 2>/dev/null
if [ $? -ne 0 ] 
        then 
                echo "$num1 no es un valor númerico"
                error="s"
else 
                [ -n "$num2" ] && [ "$num2" -eq "$num2" ] 2>/dev/null
                if [ $? -ne 0 ]  
                then 
                        echo "$num2 no es un valor numerico"
                        error="s"
                fi
fi


if [ $error = "s" ]
then 
        echo "Nos salimos.... hay un error en alguno de los valores"
        exit
else
        suma=$(( $num1 + $num2 ))
        #echo "resultado del comando anterior $?"
        echo "resultado de la suma $num1 + $num2: " $suma
fi


```