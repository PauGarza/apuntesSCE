# Práctica 1
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
        <form **action ="Servlet_00" method = "Post"**>
        <br>operando A: <input type="text" value="5" name="opA"/>
        <br>operando A: <input type="text" value="8" name="opB"/>
        <br><input type="submit" value="Click para sumar"/>
        </form>
    </body>
</html>
```

Cree un servlet, llámelo `servlet_00` y agregue el siguiente código:
```java
String deDonde = "";
protected void processRequest(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	int a,b,c;
	a = Integer.parseInt(request.getParameter("opA"));
	b = Integer.parseInt(request.getParameter("opB"));
	c = a + b;
	response.setContentType("text/html;charset=UTF-8");
	
	try (PrintWriter out = response.getWriter()) {
		/* TODO output your page here. You may use following sample code. */
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

Modifique asimismo los métodos de acción `doGet` y `doPost` para definir valor a la variable `String deDonde` y que indiquen ccómo se reccibió el procesamiento:
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
Propósito: Estresar el servicio `sayHello` de la interfaz remota `Hello` que se encuentra en la distribución `SimpleRMIAutonomo` El directorio con los `.bats` / `.sh` y `.jar` se encuentra configurado para ser instalado en computadoras Windows, Linux o OSX.

Premisas:
1. Se tendrán al menos dos computadoras con java instalado o copiado.
2. Se habrá “instalado” y probado de manera exitosa el estresador de manera individual en cada una de las computadoras (con experiencia suficiente no se requiere ejecutar solo el paso 4 en la(s) máquina(s) de los “clientes”.
3. Se tiene conexión de red entre las máquinas participantes.