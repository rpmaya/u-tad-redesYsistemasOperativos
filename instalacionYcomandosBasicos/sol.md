# NOMBRE APELLIDOS
## Ejercicio 1

```bash
mkdir MisPruebas
cd MisPruebas
mkdir Mis_Documentos
cd /home/luismarquina/MisPruebas/Mis_Documentos/
cd ~
cd MisPruebas/Mis_Documentos
cd -
mkdir MisPruebas/Mis_Documentos/Mis_enlaces
cd MisPruebas/Mis_Documentos
touch doc_1.txt
ln -s home/luismarquina/MisPruebas/Mis_Documentos/doc_1.txt ./Mis_enlaces
ls -l 
cp /var/log/syslog mi_syslog
cd Mis_enlacces
ln -s ~/MisPruebas/Mis_Documentos/mi_syslog mi_link_a_syslog
cd ../../
ls -a
ls -l
ls -a
ls -al
```

El punto "." es la referencia al directorio actual, mientras que los dos puntos
seguidos hacen referencia al directorio inmediatamente superior (en el árbol
de jerarquías).

	lectura / escritura / ejecución | para los permisos
	usuario / grupo / otros | para los grupos
	
	Directorio .
	La primera "d" nos ndica que es un directorio, que el usuario  y el grupo tienen todos los permisos, y que  
	otros solo los de lectura y ejecucion.

    Directorio ..
    La primera "d" nos ndica que es un directorio, que el usuario tiene todos los permisos, que el grupo solo tiene los permisos de lectura y ejecucion y 
	otros también solo los de lectura y ejecucion.

    Archivo Mis_Documentos
	La primera "d" nos ndica que es un directorio, que el usuario  y el grupo tienen todos los permisos, y que  
	otros solo los de lectura y ejecucion.

	A continuación:
			->Nombre
| | | | | |
| | | | | -> Fecha de modificación
| | | | |
| | | | -> Tamaño en bytes
| | | |
| | | -> Nombre del grupo con permisos sobre el fichero
| | |
| | -> Nombre del Usuario propietario del fichero
| |
| -> Número de enlaces duros al archivo/directorio

```bash		
cd ~
mkdir MisFicheros
cd MisFicheros
echo "Hola Mundo" > Fichero_1.txt
echo "estoy usando Linux" > Fichero_22.txt
echo "practicando los primeros comandos" > Fichero_333.txt
cat Fichero_1
cat Fichero_22
cat Fichero_333
cat *
cp Fichero_1.txt fichero1.txt
mv Fichero_22.txt Mi_fichero22.txt
touch BACKUP.5 BACKUP.66 BACKUP.777
ls -l
ls [[:upper:]]*
ls [[:lower:]]*
ls [M,F]*
ls Fichero??.*
ls BACKUP.[[:digit:]][[:digit:]]*
cd ~
mv MisFicheros ~/MisPruebas
cd MisPruebas/MisFicheros
tar -cf Backups.tar BACKUP*
tar -czf Backups_compress1.tgz BACKUP*
tar -cjf Backups_compress2.bz2 BACKUP*
```