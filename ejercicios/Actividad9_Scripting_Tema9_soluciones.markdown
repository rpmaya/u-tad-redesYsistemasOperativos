#Actividad 9 – Scripting - Tema 9

 
## Ejercicio 1
El seno hiperbólico de x, es una función real de variable real (x),  que se designa con sinh(x). Sabiendo que su fórmula está definida mediante la siguiente ecuación:  sinh(x)=(e^x-e^(-x))/2

Utiliza bc para calcuar el seno hiperbólico de 2 y guarda su resultado en una variable. 

```bash
echo "scale=10; shx=((e(2)-e(-2))/2); shx " | bc -l

#O paso a paso

res=$( echo "scale=10; var=e(2); var=var-e(-2); var=var/2; var" | bc -l) 
echo $res

```
 

## Ejercicio 2

Imprime la lista de los colores primarios.

```bash
#!/bin/bash

for color in rojo amarillo verde
do
    echo "Este es el color $color"
done

```

## Ejercicio 3.	 

Imprime, mediante un bucle for, los números impares que haya del 1 al 100. 
 
```
#!/bin/bash
for i in {1..100..2}
do
    echo $i
done

```
 
 
## Ejercicio 4.	 
Imprime, utilizando un bucle for, el listado de todos los ficheros que hay en tu directorio de trabajo
  
```bash
#!/bin/bash

for i in $(ls)
do
    echo $i
done

``` 


## Ejercicio 5.  
 
Ejecuta el siguiente comando en tu espacio de trabajo: 
`touch README.md`
Haz un script que haga utilizado un bucle for haga un listado de todos los ficheros de tu directorio de trabajo, e imprima solo el que se llama README.md

```bash
#!/bin/bash
for i in `ls ~/`
do
        if [ $i == "README.md" ]
        then
                echo $i
        fi
done

``` 


## Ejercicio 6.  
Haz un script que reciba como único parámetro el nombre de un pais. El script debe admitir al menos los siguientes países: España, Francia, Alemania, Portugal e Italia y en función de dicho pais debe escribir el nombre de su capital. 

```bash
#!/bin/bash

case $1 in

        'España')
        echo "Capital de $1: Madrid";;
        'Francia')
        echo "Capital de $1: Paris";;
        'Italia')
        echo "Capital de $1: Roma";;
        'Portugal')
        echo "Capital de Portugal: Lisboa";;
        'Alemania')
        echo "Capital de $1: Berlin";;
        *)
        echo "Mmmmm....";;
esac


``` 


