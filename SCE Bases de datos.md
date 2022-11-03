# Bases de datos

Trabajaremos en la carpeta `D01/`. Hay tres archivos en Canvas:
1. `derby_affablebean_schema_creation_v03_afbv03.sql`
2. `Instr_creaBD.txt`
3. `SecuenciasParaFoliadores.txt`
Descargar los tres archivos y guardarlos en la carpeta `D01/`

Podemos ver los datos de las bases de datos en: `C:\Users\sdist\AppData\Roaming\Netbeans\Derby`. Para ello, verificar que se están mostrando los elementos ocultos.

## Creación de la base de datos
1. Levantar Glassfish
2. Expandir "Databases", click derecho en Java DB, create database.
	- Database Name: `afbv03`
	- User Name: `app`
	- Password: `app`
3. Levantar ij: ir a `C:\Users\sdist\SCE\glassfish-4.1.1\javadb\bin`, ahí abrir una consola `cmd` y escribir `ij` en la consola. Debería decirnos que la versión es 10.10.
4. Ejecutar el script de creación de tablas
	```bash
	run 'C:\SCEBeto\EJB\D01\derby_affablebean_schema_creation_v03_afbv03.sql';
	```

## Prueba con WebService
**Notas**: 
- Esta manera de hacer las cosas no es la mejor.
- El paso 7 no es necesario porque no nos sirvió de nada.
1. Crear un nuevo proyecto "Java Web" > "Web Application", nombrarlo `WS_afbv03_01` y guardarlo en la carpeta `D01\`. Verificar que se utilice Java EE 6 Web.
2. Click derecho en el nombre del proyecto > New > Entity Classes from Database...
3. Hacer click en el menú de Data Source y luego en New Data Source...
	- JNDI Name: jdbc/afbv03
	- Database Connection: seleccionar la que termina en /afbv03.
4. Click en "Add All >>" y luego en "Next >"
5. Escribir `entidades` en "Package:".
6. Finish.
7. Crear un nuevo proyecto "Java Class Library"
	- Nombre: `D01_afbv_03_Lib`
8. Click derecho en el nombre del proyecto (`WS_afbv03_01`) > New > Session Beans for Entity Classes
9. Click en "Add All >>" y luego en "Next >"
10. Cambiar el nombre del paquete a `fachadas_entidades`.
11. Activar el checkbox de "Local"
12. Finish.
13. Click derecho en el nombre del proyecto (`WS_afbv03_01`) > New > Web Service
	- Name: `WS_D01_Customer`
	- Package: `ws_d01_customer`
	- Seleccionar "Create Web Service from Existing Session Bean", dar click en "Browse..." y seleccionar "CustomerFacade"
14. Finish.
15. Darle "Clean and Build" al proyecto (`WS_afbv03_01`) y luego "Deploy"
16. Expandir "Web Services", click derecho en "WS_D01_Customer" y darle en "Test Web Service". Podemos probar cualquier método.

## El POJO
1. Crear un nuevo proyecto simple llamado `D01_CltePOJOWS_01`.
2. Copiar el URL del WSDL (del paso 16 en la sección anterior).
3. Click derecho en el nombre del proyecto (`D01_CltePOJOWS_01`) > New > Web Service Client y pegar el link en "WSDL URL". El nombre del paquete es `ws_d01_customer`.
4. Click derecho en `D01_CltePojoWS_01.java` (el archivo de texto) > Insert Code... > Call Web Service Operation... > count > OK.
5. Escribir en el main:
```java
System.out.prinln("Son " + count() + " clientes");
```
6. Darle run al proyecto. Debería decir que son 0 clientes.
7. Repetir el paso 4 con "findAll" y "create" para crear los métodos.
8. Copiar el siguiente código en `main`:
```java
ws_d01_customer.Customer elClte = new ws_d01_customer.Customer();
elClte.setAddress("Río Hondo 1");
elClte.setCcNumber("1");
elClte.setCityRegion("CD");
elClte.setEmail("correo@itam.mx");
elClte.setName("Ditirambo Farfulla");
elClte.setPhone("5512345678");

create(elClte);

System.out.prinln("Son " + count() + " clientes");
List<ws_d01_customer.Customer> cltes = findAll();
cltes.forEach(clte->{System.out.prinln(clte.getId() + " ... " + clte.getName());});
```

## 27 de octubre de 2022
1. Descargar el archivo `Instrucciones_AltaDeEntidades.txt`. Ahí estará la línea que vamos a escribir. Alternativamente, ir a `WS_D01_Customer.java` y, en el método `create`, después de `ejbRef.create(customer)`, agregar la siguiente línea:
```java
Logger.getLogger(Customer.class.getName()).log(Level.SEVERE, "El id del nuevo cliente es {0}", customer.toString());
```
Nota: puede que sea necesario importar `java.util.logging.Level` y `java.util.logging.Logger`.
2. Deployear y probar que funcione. No hice esto xd.
3. Ahora vamos a modificar el método `create` en el Web Service para que nos regrese el id. El método debería quedar así:
```java
@WebMethod(operationName = "create")
//@Oneway
public long create(@WebParam(name = "customer") Customer customer) {
	ejbRef.create(customer);
	long lngId = customer.getId();
	Logger.getLogger(Customer.class.getName()).log(Level.SEVERE, "El id del nuevo cliente es {0}", customer.toString());
	return lngId;
}
```
**Ojo** con comentar `@Oneway`.
4. En la ventana de proyectos, en `D01_CltePOJOWS_01`, ir a "Web Service References", expandir, hacer click derecho en "WS_D01_Customer" y darle en "Refresh...". Esto debería actualizar lo que hicimos en el paso anterior.
5. En `D01_CltePOJOWS_01.java`, modificar el método `create` para que quede así:
```java
private static long create(ws_d01_customer.Customer customer) {
	ws_d01_customer.WSD01Customer_Service service = new ws_d01_customer.WSD01Customer_Service();
	ws_d01_customer.WSD01Customer_Service service port = service.getWSD01CustomerPort();
	return port.create(customer);
}
```
6. En el mismo archivo, pero ahora en el método `main`, agregar la siguiente línea:
```java
long elID = create(elClte);
```
y asegurarse de que no está ningún `setId`.
7. Ir a `persisence.xml` (está en `WS_afbv03_01` > `Configuration Files`) y desactivar el checkbox de `Include All Entity Classes in "WS_afbv03_01" Module`. 
8. Click en "Add Class...", seleccionar todas las clases y darle click en "OK".
9. En la sección de Properties, hacer click en "Add...", seleccionar `eclipseling.logging.level` y asignarle el valor de `FINEST`. Darle click en "OK".

## Asignación automática del ID
1. Ir a "Services" > "Databases" > "jdbc:derby://localhost:1527/afbv03 \[app on APP\]" > "Other scemas" > "SYS" > "Tables" > "SYSSEQUENCES". Click derecho y "Execute Command". Ahí escribiremos lo que nos va pidiendo el paso 1 del archivo `DefiniendoYOperandoUnFoliadorParaElCampoIdDeEntidadesEJB.txt` en Canvas.
2.  Levantar ij: ir a `C:\Users\sdist\SCE\glassfish-4.1.1\javadb\bin`, ahí abrir una consola `cmd` y escribir `ij` en la consola. Debería decirnos que la versión es 10.10.
3. Escribir `connect 'jdbc:derby://localhost:1527/afbv03;user=app;password=app';` en la consola
4. Escribir `show tables;`
5. Escribir `describe app.customer;`
6. Mejor seguir el tutorial del `.txt`.

## 1 de noviembre de 2022
- Actualizar la base de datos afbv03 para que no sea autonumérico el número de orden. Esto se hace en el archivo `derby_affablebean_scema_creation_v03_afbv03.sql`. Tendría qeu quedar: `customer_order_id INT NOT NULL` y ya.
- Usar el ij de glassfish (asegurarse de que está la versión 8 de java) y hacer lo del `run`.
- 