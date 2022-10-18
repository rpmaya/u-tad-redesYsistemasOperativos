#Actividad 8 – Usuarios y Grupo -  Tema 6

Crea un directorio de prueba para practicar el Tema 6. 
Crea también los ficheros y directorios que se indican. 
 
```
mkdir actividades_tema6
cd actividades_tema6
echo "Ejercicios del Tema 6" > tema6.txt
echo "Ejercicios de permisos" > permisos.txt
echo "Ejercicios de Usuarios" > usuarios.txt
echo "Ejercicios de Grupos" > grupos.ttx
mkdir dir_permisos
echo "prueba permisos" > dir_permisos/prueba.txt
```
 
## Ejercicio 1
Observa los permisos de cada fichero y ejecuta los siguientes cambios: 

- a) Da permisos de ejecución para el owner (user), el grupo (group) y para el resto de usuarios (others) al fichero permisos.txt 
- b) Elimina (revoca o quita) el permiso de lectura para "others" del fichero grupos.txt
- c) Dale permisos de escritura a todos los ficheros para todos (usuario, grupo y others)
- d) Otorga los permisos "400" al directorio "dir_permisos". Observa si puedes acceder a dicho direcorio y si puedes listar su contenido. Prueba también a hacer un cat del fichero prueba.txt.  ¿que observas?
- e) Otorga ahora los permisos "100" a dicho directorio. Repite lo del apartado anterior y observa lo que sucede.


## Ejercicio 2.	 
 
Crea un grupo llamado "developers" y comprueba que dicho grupo se ha almacenado en el fichero correspondiente. 

```
 

```
 
 
## Ejercicio 3.	 
Cambia el grupo propietario del fichero "tema6.txt" y manten su usuario actual. 
  
```bash
 
``` 

## Ejercicio 4.  
 
- a) Crea un usuario en el sistema que se llame "lola". No utilices ninguna opción. Comprueba que se ha creado correctamente. 
- b) Crea un usuario en el sistema que se llame "nemo". Utiliza la opción correspondiente para que su directorio de trabajo sea /home/nemo_pez/. Comprueba que se ha creado correctamente. 
- c) Crea un uuario que se llame "lolo" y evita que se cree su directorio home. Demuestra que lo has hecho correctamente.
- d) Observa que cambios se han producido en el fichero de grupos.  

```bash
 

``` 


## Ejercicio 5.  
Cambia el uuario (owner) del fichero "tema6.txt" al usuario "lola" y manten su grupo actual. 

```bash
 

```

## Ejercicio 6  

Cambia la contraseña del usuario "lolo"

 ```bash


```


## Ejercicio 7  

Elimina el usuario nemo y elimina además su directorio de trabajo 

```bash


```



## Ejercicio 8  
Crea dos nuevos grupos en el sistema: "testers" "secgroup"

```bash


```


## Ejercicio 9 
- a) Añade el usuario "lola" a los grupos creados en el apartado anterior.

```bash

```

- b) Intenta borrar el grupo testers y observa si puedes

```bash

```


## Ejercicio 10

Añade el usuario "lola" al grupo "sudoers"
```bash


```




