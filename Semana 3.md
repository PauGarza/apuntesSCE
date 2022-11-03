# Semana 3
23 y 25 de agosto de 2022

Repaso (ver foto que tomó Sonia):
- El rmi registry (*Paso 0*: una sola vez) tiene dos tipos de solicitudes: el servidor y el cliente.
- También tenemos el **servidor de disparo** (*Paso 2*). Éste tiene los siguientes APIs: 
	- 'QuienSoy'
	- '$\Delta$ t_fiesta': tiempo de inicio de la fiesta
	- acum$\Sigma$: las estadísticas de cómo le fue a los clientes en la fiesta.
	- $\Sigma$
	- reset, $\Delta$ t: para resetear
- Por otro lado, están los clientes. Se comunican con el servidor de disparo (y, obviamente, con el rmi registry). ¿Quién siembra los clientes? El **estresador**.
- El estresador (*paso 4*) avienta a todos los clientes. Luego cada uno de esos clientes hace lo que tiene que hacer y, después, cada cliente se avienta contra el **servicio**.
- El servicio (server; *paso 1*) es el sujeto que queremos revisar cómo anda. Al final, regresan sus estadísticas.
- Por último, está el **master** (*paso 3*) que se encarga de resetear el servidor de disparo y solicitar las estadísticas.
- También está el reporte (*paso 5*) que se onecta a $\Sigma$.

## Lo que hicimos hoy
1. Descargar `tstRMI` de Canvas y descomprimir los archivos en la carpeta `SCEBeto`. 
2. En Netbeans, abrir el proyecto (`Ctrl+Mayús+O`) que se llama `tstRMI`.
3. Revisar en File > Project Properties (tstRMI) > Run que en Main Class esté `example.hello.Distribuidor`.
4. Arrancar el Glassfish Server.
5. Crear un nuevo proyecto Web llamado `WSSUMA`. Asegurarse de que el servidor es glassfish y crear todo en default.
6. Click derecho en el nombre del proyecto > new > crear un nuevo Web service (simple). Guardar con el nombre de 'suma' y en el paquete `wssuma`. Crear desde cero.
7. Escribir el siguiente código debajo del método `hello` y guardar cambios:
```java
@WebMethod(operationName = "suma")
public int suma(@WebParam(name = "a") int a, @WebParam(name = "b") int b) {
	return a + b;
}
```
7. Click derecho en el nombre del proyecto y darle en "Deploy".
8. Expandir la carpeta de Web Services, click derecho en `suma` y "Test Web Service". Debería abrirse una página web en la que hay dos botones: "Hello" y "Suma". Probar.

Notas:
- `Class.forName`. Recordemos que Java, al invocar una clase, la levanta y la echa a andar. Busca la clase en el class path y la pone en l amemoria. Todavía no sabe si va a tener instancias de esa clase o no, pero se supone que ahí hay un main y a éste le transfiere el control. Por lo tanto, cada vez que uno quiera usar una clase, la busca en la memoria; si no la encuentra, lanza una excepción. Esto es lo que hace `Class.forName`: busca la clase en la memoria.
- En `ServidorDeDisparo.java`, los métodos son `synchronized` para que no vaya a entrar otro hilo cuando el método sea sacado de la memoria, y evitar que *se haga bolas el engrudo*; que las variables almacenadas en el Heap se mantengengan como estaban.

- Tarea: crear otro testRMI.jar para que haga sumas en vez de solamente decir "hola". Tenemos que modificar la interfaz (hello), la implementación de la interfaz y luego el cliente.
---
Notas:
- Cuando ponemos `@WebService` o `@WebMethod` estamos haciendo **anotaciones**, que sirven para inyectar código cuando se mande llamar el método.
- No enviar objetos por protocolos en los que no controlamos lo que sucede al otro lado. Solamente conviene enviar los escalares (que sí sean `final`) necesarios para reconstruir al objeto.
- **In-place deployment**: cuando glassfish no quiere copiar todo y en vez de eso refiere todo al `.war`.
- Cuando probamos la aplicación, glassfish nos deja ver el WSDL. Esto sirve para comunicarse con clientes que tienen otra tecnología. El URL que se abre al hacer click ahí también es muy importante. Podemos cambiar `localhost` por la IP de nuestra máquina. 
- Proxy: lo que está en una punta para hacer creer al servicio que está cerca.

1. Levantar el servidor.
2. Click derecho en `SumaWeb` en la parte de Projects y darle en "Clean and build". El output debería decir que el Building jar está en `C:\SCE\SumaWeb\SumaWeb\dist\SumaWeb.war`.
3. Click derecho en `WSSUMA` y darle en deploy.
4. Probar el método `hello`: escribir algo en la entrada de texto y hacer click en el botón que dice hello.
5. Podemos, con F12, abrir la consola y ver qué hay detrás. Igual podemos abrir la pestaña de Red y hacernos los interesantes.

## Para el cliente
1. Crear un nuevo proyecto (Java Application) y nombrarlo `PojoTestWSuma`.
2. File > New > New Web Service Client. Seleccionar "WSDL URL" y pegar el URL que conseguimos al cambiar el `localhost` por nuestra IP en el WSDL. 
	- Obsérvese que, al expandir la carpeta de Web Service References, ahora está el archivo `suma.wsdl`.
3. En PojoTestWSuma.java, hacer click derecho y luego en "Insert Code..." > "Call Web Service Operation...". Expandir las carpetas hasta llegar a `sumaPort` y seleccionar `suma`. Click en OK. Se debería generar el siguiente código debajo (**no** dentro) del método principal:
```java
private static int suma(int a, int b) {
	wssuma.Suma_Service service = new wssuma.Suma_Service();
	wssuma.Suma port = service.getSumaPort();
	return port.suma(a, b);
}
```

> "Esto es como una llamda al rmi, pero con Web Service."
> — Alexander

4. Dentro del método principal, escribir:
```java
int a, b, c;
a = 3;
b = 5;
c = suma(a, b);
System.out.println("La suma de " + a + " y " + b + " es " + c);
```
- Nota: como en el paso 3 tenemos un `new`, cada vez que queramos hacer una suma tendríamos que instanciar un nuevo servicio; esto no es bueno, por lo que nos convendría más sacar eso del método y escribirla en el principal, antes de hacer todas las sumas. Hagamos eso. Why not.
5. Modificar el método principal para que tenga:
```java
wssuma.Suma_Service service = new wssuma.Suma_Service();
wssuma.Suma port = service.getSumaPort();

int a, b, c;
a = 3;
b = 5;
c = port.suma(a, b);
System.out.println("La suma de " + a + " y " + b + " es " + c);
```
6. Comentar el método `private static int suma(int a, int b)` para que no haga nada.
7. Correr el proyecto (PojoTestWSuma) y verificar que salga la suma en la consola.

**Lo que va a venir en el examen:** copiar este código en el del RMI para que, en vez de conectarse al servidor del RMI, se conecte al otro y que podamos estresarlo (con lo del paso 0, 1, etc.).