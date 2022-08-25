# Semana 2
## Prueba simple RMI
**Paso 0**. Inicializar el rmi registry. Por default queda en el puerto 1099.
**Paso 1**. Inicializar el servicio. 
**Paso 2**. (Cliente).
-	2.1 Localizar registro
-	2.2 Buscar el "server" en el registro.
-	2.3 Crear el proxy  del "server" del lado del cliente
-	2.4 Usar el servicio
Ver la foto del diagrama. Sonia la tiene.

1. Descargar el archivo `pruebaSimpleRMI.7z` en Canvas. 
2. Guardarlo y descomprimirlo en la carmeta `\SCEBeto`.
3. Abrir una ventana de la consola en esa dirección y ejecutar `java -version` para revisar que la versión de java sea 1.8.0_321.
4. Crear un archivo ejecutando  `notepad cj111.bat` en la terminal y, en el archivo, escribir lo siguiente:
```bat
@echo off
set path=C:\Program Files\Java\jdk1.8.0_111\bin;%path%
set classpath=.;C:\Program Files\Java\jdk1.8.0_111\bin;%classpath%
```
5. Ejecutar `cj111.bat`.
6. Volver  a correr `java -version` para revisar que ahora la versión sea 1.8.0_111.
7. Borrar los archivos `.class` que están en `\example\hello`.
8. Ejecutar `javac *.java`.
9. Mover los archivos `.class` a `\example\hello`.
10. Ahora hacemos el **paso 0**: ejecutar `start rmiregistry`.
11. Echamos a andar el servidor: `java example.hello.Server`. Debería decirnos `Server ready`. 
12. Creamos una nueva ventana en el directorio `\pruebaSimpleRMI` y volvemos a ejecutar `cj111.bat` para que sea la misma versión de java. 
13. Ejecutar `java example.hello.Client`.
14. Podemos alterar los parámetros ejecutando, por ejemplo, `java example.hello.Client localhost 10` para que use el servidor localhost y haga 10 sumas nada más.

Notas:
- Nota: en Java se interopera por medio de las **interfaces**. No sé qué quiere decir esto, pero parece importante.
- La interfaz que estamos usando aquí está en  `Hola.java`. Tiene dos funciones: una regresa un `String` y la otra regresa un `long`.
- En `Server.java` vemos que, en vez de trabajar con `Object`, como estamos acostumbrados, ahora usamos `UnicastRemoteObject`. Lo terminamos casteando como una interfaz `Hola`.
- "Bind" es atar; `rebind` nos permite poner una cosa y luego cambiarla por otra.
- En `Client.java` podemos ver que hay dos argumentos: el host y cuántas vueltas queremos que haga.
- wtf es un stub??? Tiene que ver con proxy y API.
- En `Server.java` podemos ver métodos que tienen `synchronized`.  Esto es para que no se haga un cagadero.

Para usar otro cliente:
1. Averiguar la dirección IP a la que queremos enviar todo. Podemos usar `ipconfig`.
2. Ejecutar `java example.hello.Client 148.205.133.134` (con la dirección que queremos). 
---
## Estresador
- Dos diagramas. Sonia tiene las fotos otra vez.

1. Descargar la carpeta de SimpleRMIAutonomo y guardarla en `C:\SCE`. 
2. En `batsDeInicio\cjk.bat` escribir: `cd C:\SCE\SimpleRMIAutonomo` y guardar el archivo. Verificar que tenemos la versión 8 de java; si no, modificar con lo de `set path...` que ya hicimos antes.
3. Copiar tanto `cjk.bat` como `gen.bat` y pegar en `C:\Users\psdist`. 
4. Ejecutar `gen.bat`. Se deberían abrir cuatro ventanas del cmd en `C:\SCE\SimpleRMIAutonomo`.
5. En la ventana de "Proceso 1", ejecutar `0_unaSolaVez.bat`. Tip: podemos escribir simplemente `0` y darle tab y se debería llenar solito. Minimizar (**no cerrar**) las ventanas que salgan.
6. En el "Proceso 1", ejecutar `1_ejes.bat`.  (Podemos usar lo mismo de escribir 1 y tab). Luego, en la ventana 2, con `2_ejesd.bat`; en ventana 3, `3_ejem.bat`; en ventana 4, `4_estresa.bat`. 
7. Una vez que termine, ejecutar `5_ejer.bat` en la ventana 3.

Bats de inicio:
- `cjk.bat`: define versión de java en PATH y cambia al directorio de trabajo.
- `gen.bat`: lanza 4 ventanas de cmd.