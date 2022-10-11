#Actividad 7 – Comandos Linux Ficheros y Directorios -  Tema 5



## Ejercicio 1.	(grep)
El comando `netstat` nos permite listar todas las conexiones TCP y UDP en un sistema Linux. 
Para listar todas las conexiones TCP y UDP en todos los estados posibles podemos utiliar `netstat -tun`. Si además se desea conocer el proceso con el que está establecida cada conexión, se agrega la opción -p.  
Teniendo en cuenta eso muestra usando dicho comando y la herramienta `grep` **solo aquellas conexiones que estan en estado ESTABLISH**.  

Antes de hacerlo abre un navegador en tu mauna y conectate a un par de páginas web. 

```bash
 

```

## Ejercicio 2.	(wc)

Usando el comando anterior muestra ahora por pantalla **sólo el número** de conexiones en estado ESTABLISH (ESTABLCIDO).   

```bash
  

```

## Ejercicio 3. (tr + cut)
Uitiliza de nuevo los comandos `netstat` y `grep` para obtener las conexsiones en estado "establecidas". 

Elimina los espacios en blanco repetidos y luego haz los cambios necesarios para obtener solo la IP remota (Foreign Address)con el puerto. 

```bash
 
 
```

## Ejercicio 4. (awk)
Utilizando el comando awk obten del fichero de usuarios (/etc/passwd) el nombre de cada usuario y su UID separados por un espacio en blanco.  El nombre de usuario debe ir precedido de "user:" y el UID de la cadena "UID:"

```bash
 

```

## Ejercicio 5 (sed)
Utilizando la salida del comando anterior, sustituye la cadena user por "login de usuario"

 ```bash
 

```



