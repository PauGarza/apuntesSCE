# Semana 7
- Entrar en `vdih.itam.mx`. Usar el acceso web. 
- Usar el correo del ITAM y la contraseña de Comunidad.
- Entrar en Sistemas de Comercio Electrónico.

Para el examen, hay que manejar el esquema de estresamiento al 100%. 

El EJB tiene APIs/Intrefaz local. 
El application server tiene APIs/Interfaz remota.  

---
1. Undeploy a todo lo que tenga EJB
2. Detener el servidor (click derecho > stop)
3. Clean and build: `EntAppEJB`.
4. Levantar el servidor nuevamente
5. Deploy `EntAppEJB`.
6. Clean and build: `EntAppClient`.
7. Darle run al main de `EntAppClient`. Debería salir la suma.