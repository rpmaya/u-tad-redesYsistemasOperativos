#Actividad 11 – Scripting - Tema 9

 
## Ejercicio 1
Basándonos en los resultados de ejecutar el comando ip a, realizar un script que imprima:
- La dirección MAC, situada a la derecha de link/ether, excluyendo cualquier otro texto asociado. El script empleará, entre otros, el comando de manejo de ficheros awk.
- La dirección IP, situado a la derecha de inet, excluyendo cualquier otro texto asociado. El script usará, entre otros, los comandos de manejo de ficheros awk y tr.
Los comandos tienen que ser universales, válidos para cualquier dirección IP y MAC posibles.


```bash
#!/bin/bash
echo "Tu dirección MAC es: "
ip a | grep link/ether | awk '{print $2}'

echo  "Tu dirección IPv6 es: "
ip a | grep inet | grep link  | tr '/' ' ' | awk '{print $2}'


echo "Dirección IP"
ip a |grep "scope global" | awk '/inet / {print$2}'
ip a |grep "scope global" | awk '/inet / {print$2}' |tr '/' ' '



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
#!/bin/bash
echo calculadora
while [ 1 ]; do
	echo "Elige la funcion de la calculadora"
	echo " 1> potenciacion 2> conversor a grados Farenheit "
		read opcion
		echo " Ha elegido $opcion"
		case "$opcion" in
			'1')
		echo 'Potenciacion'
		echo "introduzaca la base"
		read base
		echo "Introduzca el exponente"
		read exp
		if [ $exp -gt 0 ]; then
			res=1
			for ((i=0; i<=exp; i=i+1)); do
				if [ $i -gt 0 ]; then # Esto no es necesario…
					res=$(( res*base ))
					echo "Num_prov = $res $i"
				fi	 		
			done
			echo " Exponencial de $base elevado a $exp es $res"
		else
			echo "Exponente menor de cero"
		fi
		;;
		'2')
		echo 'Conversor a grados Farenheit'
		echo 'Introduzaca la temperatura en grados C'
			read tempC
			F1=$(( ((18*tempC + 320 )) /10 ))
			F2=$(( F1 / 10 ))
			echo "La temperatura en grados Farenheit es $F1"	
		;;
		*)
		echo 'eleccion erronea'	
		;;
	esac
done 


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

echo "introduce cuantos números aleatorios que desees obtener"
read x
for i in $(  seq 1 $x )
do 
        aleatorio=$(($RANDOM/33))
        echo " Este el número aleatorio $i; $aleatorio "
        # RANDOM genera  un número aleatorio entre 0 y 32768
        # Una solución más optima es aleatorio=$(($RANDOM%1000)) 

done

```

## Ejercicio 6
Determinar si existe un fichero, que se introduce por teclado, en tu directorio de trabajo por defecto ($HOME), si no existe crearlo.

```bash 

echo "Introduce un fichero que deseas comprobar si existe en tu directorio de trabajo"
read  fichero
echo $HOME
if [ -f $HOME/$fichero ]
        then
                echo "el fichero existe"
else
                echo " el fichero no existe"

                echo  "he llegado al else"
fi


```
Otra versión por parámetros: 

```
#!/bin/bash

if [ $# != 1 ]
then
        echo
        echo ">>> ERROR: El número de parámetros es incrrecto"
        echo "El script necesita para su ejecución el nombre de un fcihero"
        echo 
        exit 0
fi

DIR=$(pwd)
echo "Estamos en el directorio ${DIR}"
echo "Comprobaremos si existe el fichero $DIR/$1"
if [ -f $DIR/$1 ]
        then
               echo ">>> El ya fichero existe, ejecuta el programa con otro nombre"
        else
               echo ">>> El fichero no existe, lo creo"
               for (( i=1; i<=10; i++ ))
            do
                    echo "$i" >> $DIR/$1
            done
            cat $DIR/$1
fi
```





