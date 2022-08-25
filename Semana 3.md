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
2. Revisar en File > Project Properties (tstRMI) > Run que en Main Class esté `example.hello.Distribuidor`.
3. Arrancar el Glassfish Server.
4. Crear un nuevo proyecto Web llamado `WSSUMA`. Asegurarse de que el servidor es glassfish y crear todo en default.
5. Click derecho en el nombre del proyecto > new > crear un nuevo Web service (simple). Guardar con el nombre de 'suma' y en el paquete `wssuma`. Crear desde cero.
6. Escribir el siguiente código debajo del método `hello` y guardar cambios:
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

- Tarea: crear otro testRMI.jar para que haga sumas en vez de solamente decir "hola". Tenemos que modificar la interfaz (hello), la implementación de la intrfaz y luego el cliente.