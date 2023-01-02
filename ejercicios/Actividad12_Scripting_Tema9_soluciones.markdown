#Actividad 12 – Scripting - Tema 9

 
## Ejercicio 1
Un numero perfecto es un entero, que es igual a la suma de todos los enteros positivos (excluido el mismo) que son divisores del número. El primer número perfecto es el 6 (1+2+3=6)

Escribir un programa que pida un numero entero positivo y determine si es perfecto

 
```bash
#!/bin/bash


echo
echo "Vamos a calcular si el número introducido es un número perfecto!!!" 
echo

while true; do
        read -p "Introduce un número: " numero
        echo

es_perfecto="n"
suma_divisores=0
        for ((i=1; i<$numero; i++)); do
                mod=$(( $numero%$i ))
                if [ $mod -eq 0 ] 
                        then
                                echo "El número $numero es divisible por $i"
                                suma_divisores=$(( $suma_divisores+$i ))
                                echo "La suma de los divisores vale: $suma_divisores"
                                if [ $suma_divisores -eq $numero ]
                                  then
                                           es_perfecto="s"
                                  elif [ $suma_divisores -gt $numero ]
                                        then
                                                break
                                fi
                fi
        done
        if [ $es_perfecto = "s" ]
        then
			echo ">>>>> El número:  $numero ES un número PERFECTO"
    	else
			echo "Ohhhhh El número:  $numero NO ES un número PERFECTO"
  fi	
done

 

```

## Ejercicio 2
Escribir un script que determine los 6 primeros números perfectos. 

```bash

```


## Ejercicio 3

Escribir un script que pida dos valores por pantalla y determinar si dichos números son "números amigos". Dos números son amigos cuando la suma de los divisores propios de uno de ellos es igual al otro y viceversa. Los divisores propios de un número son aquellos valores que dividen el número en partes exactas exceptuando el propio número.  

Los números 220 y 284 son Números Amigos porque: 


- Los divisores de 220 son 1, 2, 4, 5, 10, 11, 20, 22, 44, 55, 110.

Si hacemos la suma 1+2+4+5+10+11+20+22+44+55+110 = 284. ¡Se cumple!

- Al igual, los divisores de 284 son 1, 2, 4, 71, 142.

Al sumarlos 1+2+4+71+142 = 220 ¡También se cumple!!


```bash
#!/bin/bash

echo
echo "LOS NÚMEROS TAMBIÉN PUEDEN SER AMIGOS.... VAMOS A COMPRBARLO!!!"

while true; do
        echo
        echo
        read -p "Introduce el primer número: " numero1
        read -p "Introduce el segundo número: " numero2
        echo

        son_amigos="n"
        num1_es_perfecto="n"
        num2_es_perfecto="n"
        suma_divisores1=0
        suma_divisores2=0

        for ((i=1; i<$numero1; i++)); do
                mod1=$(( $numero1%$i ))
                if [ $mod1 -eq 0 ] 
                        then
                                echo "El número1 $numero1 es divisible por $i"
                                suma_divisores1=$(( $suma_divisores1+$i ))
                fi
        done
        echo "La suma de los divisores de $numero1 vale: $suma_divisores1"


        for ((i=1; i<$numero2; i++)); do
                mod2=$(( $numero2%$i ))
                if [ $mod2 -eq 0 ] 
                        then
                                echo "El número2 $numero2 es divisible por $i"
                                suma_divisores2=$(( $suma_divisores2+$i ))
                fi
        done
        echo "La suma de los divisores de $numero2 vale: $suma_divisores2"


if [ $suma_divisores1 -eq $numero2 ] && [ $suma_divisores2 -eq $numero1 ]
then 
        echo ">>>> LOS NÚMEROS $numero1 y $numero2 SON AMIGOS!!!"
        echo ">>>> LA SUMA DE LOS DIVISORES PROPIOS DE UNO ES IGUAL AL OTRO NÚMERO Y VICEVERSA"
else
        echo "Ohhhhh LOS NÚMEROS $numero1 y $numero2 NO SON AMIGOS....." 
fi

done


```

## Ejercicio 4

Sabiendo que el factorial de un número n, (n!) es:
factorial = n*(n-1)*(n-2)* … * 1

Haz un script que lea por pantalla un número un número entero y calcule su factorial. 

```bash

#!/bin/bash
echo
echo "Calculemos el FACTORIAL de un número"
echo

while true; do
        read -p "Introduce el número: " numero
        echo
factorial=1
        for ((i=1; i<=$numero; i++)); do
                factorial=$(( $factorial*$i ))
        done
        echo ">>>>> El Factorial de $numero: es $factorial"
done


```



## Ejercicio 5

Haz un script que indique, para los números de 1 a 100, si es primo o no.  

```bash

#!/bin/bash


echo
echo "Calculando los primeros 100 números primos...."



# La variable i representa cada uno de los números de 1 a 100
    for ((i=1; i<=100; i++)); do
      echo
      echo "Veremos si el número $i es primo, pulsa ENTER para continuar:"
      read cont
      es_primo="s"

    # La variable j son los posibles divisores de i. 
    # Dividimos cada número (i) entre todos sus posibles divisores (j) 
      for ((j=1; j<=i; j++)); do

          modulo=$(( $i%$j ))
          # echo "El modulo de $i entre $j es $modulo"
          # Si el modulo o resto es 0 y el divisor es distinito de 1 y distinto
          # del propio número enotnces en número NO ES PRIMO
          if [ $modulo -eq 0 ] && [ $j -ne 1 ] && [ $j -ne $i ] 
             then 
                echo "El número $i es divisible entre $j"
                es_primo="n"
                break
          fi      
      done
                
    # echo "es_primmo vale $es_primo"     
    if [ $es_primo = "s" ] 
       then
             echo    
             echo ">>>>> $i es NUMERO PRIMO" 
       else 
             echo
             echo "OHhhhhh el número $i NO ES PRIMO"
    fi
done


```


Otra versión:
Esta versión imprimie los primeros 100 números seguidos y te dice si el número es primo o no. 

```bash

#!/bin/bash

echo
echo "Calculando los primeros 100 números primos...."
echo "Pulsa ENTER para continuar:"
read cont


# La variable i representa cada uno de los números de 1 a 100
    for ((i=1; i<=100; i++)); do
    echo
    #echo "Veremos si el número $i es primo, pulsa ENTER para continuar:"
    #read cont
    es_primo="s"
    
    # La variable j son los posibles divisores de i. 
    # Dividimos cada número (i) entre todos sus posibles divisores (j) 
    for ((j=1; j<=i; j++)); do
       modulo=$(( $i%$j ))
       # echo "El modulo de $i entre $j es $modulo"
       # Si el modulo o resto es 0 y el divisor es distinito de 1 y del propio
       # número entonces el número NO ES PRIMO
       if [ $modulo -eq 0 ] && [ $j -ne 1 ] && [ $j -ne $i ] 
          then 
             #echo "El número $i es divisible entre $j"
             es_primo="n"
             break
       fi      
    done
  
    if [ $es_primo = "s" ] 
       then
          echo    
          echo ">>>>> $i es NUMERO PRIMO" 
       else 
          echo
          echo "OHhhhhh >>>>> $i NO ES PRIMO"
    fi
done


```