# Tema 9. Bash Scripting. Anexo I - Algunas consieraciones y consejos importantes.   





## Extracción de datos de date

Date contiene la información de la fecha (d m Y) y del valor de la hora (H M S). Para asignar a una variable un valor se emplea la siguiente sintaxis, en este caso particularizada para minutos

```bash
minutos=$( date '+%M' )
```
%d %m %y %H %M %S 

Nota. Atención al espacio que separa date de la primera comilla simple. Los valores menores de 10, devuelve 0x, el 0 inicial puede ocasionar errores en operaciones matematicas, para solucionarlos, eliminando el cero incial  +%-M

## Algo más sobre awk
Con **awk** también se pueden realizar cálculos con numero reales

```bash
termino1=5.1
termino2=2
resultado=$(awk "BEGIN{print $termino1 + $termino2}")
echo $resultado

```


BEGIN es un patrón de awk, que suministra información antes de  ser procesada y el patron END al acabar de ser procesada.


## Comando útiles
Cuando se trabaja con rutas, hay dos comandos muy útiles: `basename` y `dirname`.

```bash
basename '/home/elenagg/tortilla.txt' # tortilla.txt
dirname '/home/elenagg/tortilla.txt' # /home/elenagg
```


## Recomendación para Examenes, dirección IP y blackboard
Siempre tener 2 copias de Ubuntu activas, por si falla una de ellas
Para realizar los exámenes y **subir información al blackboard desde Ubuntu**, lo primero es comprobar si tenemos dirección IP. 
- Con "ip a" podemos ver si tenemos dirección IP. En el bloque 2:enp0s3, tiene que aparecer la linea inet con el valor de una dirección IP valida.
- Otra forma de verlo es haciendo un ping a una dirección IP, por ejemplo: ping abc.es. Tenemos que obtener unos valores de ping validos.   

Si no tenemos IP, en primer lugar intentamos establecerla con sudo dhclient, y comprobamos con ip a si ya la tenemos (mirar en>> 2:enp0s3)   

Si seguimos sin dirección IP apagamos y volvemos a encender Ubuntu. Si lo anterior no funciona comprobar si Windows tiene una dirección IP valida con "cmd ipconfig"   

- Si Error por certificados del navegador en Ubuntu, solución apagar Ubuntu y volver a encender.
- Si Bloqueo de acceso al blackboard, navegación en modo oculto  o borrar cookies navegador. Como última opción repl.it

Hay veces que, después de haber salvado el estado de Ubuntu, al recuperarlo no se realiza correctamente. En ese caso VirtualBox ofrece la opción de borrar el estado salvado, con la opción Descartar (Flecha verdde hacia abajo), situada al lado de Mostrar (Flecha verde), que es la que habitualmente usamos para lanzar Ubuntu virtualizado.


## Pasar ficheros de Windows a Linux
Si se edita un fichero (script) en Windows y después se edita en Linux puede ser que se produzcan errores por los retornos de línea y el EOF, si es así usar el comando "dos2unix" para transformar el fichero. 

OJO si se usa Word como editor porque transforma las dobles comillas normales en dobles comillas "inclinadas": “

## Como crear alias
El comando alias sirve para ver los alias actuales en uso, y también para crear nuevos alias, de forma temporal o permanente. Los alias se usan para hacer atajos a comandos usados frecuentemente

**Sintaxis**:   
Crear un alias:  

```alias [nombre del alias]='[comandos del alias]'```

Listar los alias definidos   
```alias```

```bash
# ls -a lista ficheros ocultos incluyendo . y ..
# ls -A no lista . y ..
# ls -C listado en columnas
# ls -F clasifica (por tipo)
# ls -l formato largo
alias 
>alias grep='grep --color=auto'
>alias l='la -CF'
>alias la='ls -A'
>alias ll='ls -alF'
>alias ls='ls --color=auto'
````
Vamos a crear un alias temporal
```
 
#un ejemplo
alias chu='chmod u+x'
````
La próxima vez que listemos los alias, con el comando alias, aparecerá este nuevo elemento-

Si queremos establecer un alias permanente tenemos que editarlo en el archivo del perfil de configuración de usuario **nano .bashrc**, que está en nuestra carpeta de trabajo por defecto /home/nombre_usuario/.bashrc
Después de la sección de alias podemos añadir los alias que deseemos

````
# some more ls aliases
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
# nuevos alias personales
alias chu='chmod u+x'

````
También es posible guardar los nuevos alias en un archivo aparte (.bash_aliases), ya que en .bashrc, si existe este archivo, se va a leer. El código de lectura de .bashrc es el siguiente:
`````
if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi

`````

## Algunos recursos útiles
https://www.tecmint.com/working-with-arrays-in-linux-shell-scripting/
The Linux command line. Capitulo de arrays