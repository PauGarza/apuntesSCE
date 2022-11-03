# Práctica 1
Objetivos:
1. Manejo de los elementos básicos de los servlets.
2. Manejo del estresador en varias computadoras.
3. Ejecución dinámica de código en java.

## 1. Elaboración de un servlet
1. Inicie el servidor de aplicación (glassfish o equivalente)
2. Cree un proyecto de tipo Web $\rightarrow$ Aplicación Web
3. Modifique el `index.html` para que solicite dos números e invoque al servlet indicado:

```html
<html>
    <head>
        <title>Pagina para sumas con servlet</title>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
    </head>
    <body>
        <form action ="Servlet_00" method = "Post">
        <br>operando A: <input type="text" value="5" name="opA"/>
        <br>operando A: <input type="text" value="8" name="opB"/>
        <br><input type="submit" value="Click para sumar"/>
        </form>
    </body>
</html>
```

Cree un servlet, llámelo `Servlet_00` y agregue el siguiente código:
```java
String deDonde = "";
protected void processRequest(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	int a,b,c;
	a = Integer.parseInt(request.getParameter("opA"));
	b = Integer.parseInt(request.getParameter("opB"));
	c = a + b;
	response.setContentType("text/html;charset=UTF-8");
	
	try (PrintWriter out = response.getWriter()) {		
		out.println("<!DOCTYPE html>");
		out.println("<html>");
		out.println("<head>");
		out.println("<title>Servlet Servlet_00</title>");           
		out.println("</head>");
		out.println("<body>");
		out.println("<h1>Servlet Servlet_00 at " + request.getContextPath() + "</h1>");
		out.println("<br><h1>El resultado de la suma de " + a + " y " + b + " es " + c + "</h1>");
		out.println("<br><h1>Proviene de " + deDonde + "</h1>");
		out.println("</body>");
		out.println("</html>");
	}
}
```

Modifique asimismo los métodos de acción `doGet` y `doPost` para definir valor a la variable `String deDonde` y que indiquen cómo se recibió el procesamiento:
```java
@Override
protected void doGet(HttpServletRequest request, HttpServletResponse response)
		throws ServletException, IOException {
	deDonde = "Get";
	processRequest(request, response);
}

@Override
protected void doPost(HttpServletRequest request, HttpServletResponse response)
		throws ServletException, IOException {
	deDonde = "Post";
	processRequest(request, response);
}
```

## 2.  Manejo del estresador en varias computadoras
### 2.1. Estresar servicico en RMI
Propósito: estresar el servicio `sayHello` de la interfaz remota `Hello` que se encuentra en la distribución `SimpleRMIAutonomo` El directorio con los `.bats` / `.sh` y `.jar` se encuentra configurado para ser instalado en computadoras Windows, Linux o OSX.

Premisas:
1. Se tendrán al menos dos computadoras con java instalado o copiado.
2. Se habrá "instalado" y probado de manera exitosa el estresador de manera individual en cada una de las computadoras (con experiencia suficiente no se requiere ejecutar solo el paso 4 en la(s) máquina(s) de los “clientes”.
3. Se tiene conexión de red entre las máquinas participantes.

#### Desarrollo: máquina servidora
1. Obtenga la dirección ip de la computadora. Pruebe con `ping` desde la computadora de los clientes.
2. Ejecute el generador de ventanas en la computadora que tendrá el servicio y la infraestructura de estresamiento (servidor de disparo).
3. Inicie el rmiregistry (paso 0) si aún no lo ha levantado.
4. Inicie los pasos 1 y 2
5. Prepare los pasos 3 y 4 y ejecute el estresamiento para verificar el funcionamiento.
6. Ejecute el paso 5 para limpiar las estadísticas.
7. Prepare el paso 3: `3_ejem 30` (30 segundos por los delays de la red y el factor humano)

#### Desarrollo: máquina donde se ejecutarán los clientes
En esta computadora solamente se requiere la terminal en el directorio donde residen los .bat o .sh y el .jar correspondiente.
1. Prepare la ventana de la terminal en la carpeta de la distribución del estresador.
2. Prepare el paso 4 con la modalidad completa (sustituyendo la ip por la de la computadora servidora rmi):
```shell
4_estresa 10 100 192.168.68.126
```
3. Ejecute el paso (`3_ejem 30`) en la computadora servidora.
4. Ejecute el paso (`4_estresa 10 100 etc.`) que preparó previamente.
5. Tome las estadísticas.

En caso de falla verifique los firewalls de ambas computadoras y ajústelos.

### 2.2. Estresar el servicio en Web Service
Propósito: estresar el servicio web desde una computadora con clientes pojo del web service.

Premisas:
1. El cliente pojo del ws se elaborará con la ip de la computadora donde se ejecuta el servicio web.
2. Se tendrán al menos dos computadoras con java instalado o copiado.
3. Copiar la distribución del estresador con los clientes pojo del WS. En este caso, la computadora donde se ejecutarán los clientesWSPojo no requiere sino la copia de la distribución del tstRMI_WS_Pojo o equivalente con el .jar correspondiente.
4. Ejecutar el estresamiento en la computadora de servicio para verificar el correcto funcionamiento del servicio y de la mecánica del estresamiento.
5. Se tiene conexión entre ambas computadoras.

#### Desarrollo: máquina servidora
1. Obtenga la dirección ip de la computadora (con `ipconfig`) y pruebe con `ping` desde la computadora de los clientes.
2. Levante Glassfish.
3. Mecánica de estrés habilitada (pasos 0, 2 y preparación del 3 con al menos 30 segundos: `3_ejem 30`).

#### Desarrollo: máquina con los clientes pojo del Web Service
1. Tener la copia de la distribución del estresador del WS.
2. En esta computadora solamente se ejecuta el paso 4, pero dirigiendo la solicitud de identificación de servicios a la ip de la computadora donde está el servicio rmi del servidor de disparo (cambie la ip hacia su propia máquina de servidor rmi):
```shell
4_estresa 10 100 192.168.68.126
```
3. En la computadora servidor ejecute el `paso 3_ejem 30`.
4. En la computadora de los clientes ws pojo ejecute el paso 4 previamente preparado.
5. Tome las estadísticas.

En caso de falla verifique los firewalls de ambas computadoras y ajústelos.

## 3. Ejecución de código dinámico en java
Propósito: comprender el acceso a las clases dentro de los .jars y la ejecución dinámica proporcionando la secuencia de paquetes donde se encuentra la clase a ser invocada para ejecución.

### 3.1 Modificación del distribuidor
Copie el proyecto para el estresamiento de los clientes pojo del WS y modifique el código del distribuidor eliminando del código la cadena `example.hello.` de la instrucción
```java
Class cl = Class.forName("example.hello." + args[0]);
```
Para dejarlo como
```java
Class cl = Class.forName(args[0]);
```
Ello implica la modificación de los .bat o .sh para que la clase a ser ejecutada por el distribuidor sea incluida como el `args[0]`. Por ejemplo, en el paso 2, para iniciar el servidor de disparo las instrucciones de ejecución deben cambiarse de
```shell
rem  ============================== Código original ===========================
echo off
echo inicializa el servidor de disparo
echo uso:
echo 2_ejesd HOSTNAME (en caso de omitirlo se usa localhost)
echo on
set cb=%cd%\tstRMI_WSPojo.jar
if [%1] NEQ [] goto conHost
java -Djava.rmi.server.codebase=file:%cb% -jar %cb% ServidorDeDisparo
goto fin
:conHost
java -Djava.rmi.server.codebase=file:///%cb% -jar %cb% ServidorDeDisparo %1
:fin
rem ======================================================================
```
a
```shell
rem  ============================== Nuevo código ===========================
echo off
echo inicializa el servidor de disparo
echo uso:
echo 2_ejesd HOSTNAME (en caso de omitirlo se usa localhost)
echo on
set cb=%cd%\tstRMI_WSPojoEjecDin.jar
if [%1] NEQ [] goto conHost
java -Djava.rmi.server.codebase=file:%cb% -jar %cb% example.hello.ServidorDeDisparo
goto fin
:conHost
java -Djava.rmi.server.codebase=file:///%cb% -jar %cb% example.hello.ServidorDeDisparo %1
:fin
```
y ejecutar los mismos cambios en el resto de los .bat o .sh.

### 3.2 Creación y nuevo proyecto
Para la segunda parte usted debe crear un nuevo proyecto que, si bien tiene las partes similares al proyecto completo de la parte 1, tiene otro nombre (no modifique los paquetes). Al cambiar el nombre del proyecto se genera otro .jar con el nuevo nombre.

1. Agregue un nuevo paquete al proyecto y ahí mueva (usando refactor) el ClienteWSPojo.
2. Copie los .bat o .sh de la parte 1 a otro directorio y agregue el nuevo .jar con el cliente “desacoplado”.
3. Modifique los .bat o .sh de la nueva distribución para que se ejecute el estresamiento de manera consistente.
4. Pruebe el mecanismo desarrollado estresando el ws con el cliente pojo "desacoplado".