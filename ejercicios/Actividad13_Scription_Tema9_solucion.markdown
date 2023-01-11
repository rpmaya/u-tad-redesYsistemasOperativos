#Actividad 13 – Scripting - Tema 9

 
## Ejercicio 1
Crear un script, en bucle infinito, que realice las siguientes operaciones sobre los arrays de las ventas por cuatrimestre por vendedor de la empresa FOAM (ver enunciado completo en Scripting)

 
```bash
#!/bin/bash
vendor1=(5 6 7 8)
vendor2=(2 3 4 5)
vendor3=(10 9 8 6)
vendor4=(7 2 9 9)

valorMedio()
{
	c=$(($1 - 1))
	media=$(( (vendor1[$c] + vendor2[$c] + vendor3[$c] + vendor4[$c]) / 4))
	echo "Media cuatrimestre $c: $media"
}

valorAnual()
{
	vendor=$1
	sum=0
	case $vendor
	in
		1) 
		for i in {0..3}; do
			sum=$((sum + vendor1[$i]))		
		done
		;;
		2)
		for i in {0..3}; do
            sum=$((sum + vendor2[$i]))
        done
		;;
		3)
		for i in {0..3}; do
            sum=$((sum + vendor3[$i]))
        done
		;;

		4)
		for i in {0..3}; do
            sum=$((sum + vendor4[$i]))
        done
		;;

		*)
		echo "Vendor no existente"
		;;
	esac
	echo "Vendedor $vendor: $sum"
}

listarCuatrimestre()
{
	cuatr=$(($1 - 1))
	echo "Vendor 1: ${vendor1[$cuatr-1]}"
	echo "Vendor 2: ${vendor2[$cuatr-1]}"
	echo "Vendor 3: ${vendor3[$cuatr-1]}"
	echo "Vendor 4: ${vendor4[$cuatr-1]}"
}

detector()
{
        for i in {0..3}; do
            if [[ vendor1[$i] -lt 3 ]]; then
                echo "vendor1: cuatrimestre $(($i+1))"
            fi
            if [[ vendor2[$i] -lt 3 ]]; then
                echo "vendor2: cuatrimestre $(($i+1))"
            fi

            if [[ vendor3[$i] -lt 3 ]]; then
                echo "vendor3: cuatrimestre $(($i+1))"
            fi

            if [[ vendor4[$i] -lt 3 ]]; then

                echo "vendor4: cuatrimestre $(($i+1))"
            fi
	    done

}

while true; do
	    echo ""
        echo "1. Listado completo de las ventas"
        echo "2. Valor total anual por vendedor"
        echo "3. Valor medio por cuatrimestre"
        echo "4. Detección de venderdores con ventas menor a 3k €"
        echo "5. Salir"	
	    read -p "Introduce una opción: " opc
	
    case $opc 
	in
		1) 
		echo "Listado completo"
		for i in {1..4}; do
			echo "Cutrimestre $i"
		    listarCuatrimestre $i	
		done
		;;

		2)
		echo "Valor anual por vendedor"
		read -p "Introduce vendedor: " vend
		valorAnual $vend
		;;

		3) 
		echo "Valor medio por cuatrimestre"
		read -p "Introduce cuatrimestre: " cuat
		valorMedio $cuat
		;;

		4) 
		echo "Detección de vendedores"
		detector
		;;

		5) 
        exit 0
		;;

		*) echo "opción no válida"

    esac  
done
```