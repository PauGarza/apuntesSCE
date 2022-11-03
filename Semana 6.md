- J2EE --> EE
- EJB's

# Access
Crear -> Diseño de consulta -> Agregar tablas necesarias -> seleccionar campos relevantes -> agregar criterios -> ejecutar. También se puede "Ver" -> Vista SQL

# EscBaileWeb
- El que en realidad hace toda la chamba es `ClsConexion.java`.
- `obtenRS` (obtén Result Set) recibe como parámetros el nombre de la tabla o la ventana y luego un TreeMap con los campos que va a trabajar.
- Data Description Language. Son los metadatos.

1. En Services, hacer click derecho en Databases y:
	- Database Name: EscuelaDeBaile
	- User Name: app
	- Password: app
2. Seguir las instrucciones de `Inst_creaBD.txt`. El archivo está en Canvas. (¿hicimos eso?)
3. Abrir con el bloc de notas el archivo `derby_affabebean_scema_creation_v03_afbv03.sql`. 
4. Lo que tenemos que hacer nosotros es poner los nombres de las tablas de la Escuela de baile y de las llaves. También tenemos que construir la de la escuela de baile y meterle los datos.
