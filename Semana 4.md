# Semana 4
## Distribuiones estadísticas
**Poisson**: cantidad de eventos en una unidad de tiempo. 
- Los eventos ocurren de manera independiente con una media $\lambda$.
$$\mathbb{P}(X = k) = e^{-\lambda} \frac{\lambda^k}{k!}$$
Nótese que
$$\mathbb{P}(X = k+1) = \mathbb{P}(X=k) \frac{\lambda}{k+1}$$
**Tiempos exponenciales**: la duración de tiempo ($\Delta t$) que hay entre un evento y otro (eventos que tienen la misma naturaleza que los de la Poisson).
- El primer evento es al tiempo $t$ y el segundo al tiempo $t+\Delta t$.
Es una distribución exponencial:
$$F(\Delta t) = 1 - e^{-\lambda \Delta t}$$
es la probabilida de que ocurra un evento antes o al tiempo de $\Delta t$. 

Lo que vamos a hacer nosotros es escoger $u \in (0, 1)$ de manera que $F(\Delta t) = u$. Así pues, dada la $u$, tenemos que 
$$ \Delta t = - \frac{1}{\lambda} \ln(1-u)$$
Notemos que, si generamos muchas instancias de esto, el promedio será el inverso de $\lambda$. Además, si cambiamos la fórmula y ponemos $\Delta t = \lambda \ln(1-u)$, entonces tendremos el número de eventos en ese lapso de tiempo; una instancia de la Poisson.

## Tips
Tip para rellenar en Excel:
- Ir a Inicio > Edición > Rellenar > Series... > Columnas > Seleccionar incremento > Aceptar.

Sobre lo que viene en el examen
- En Netbeans hay que generar el cliente del WS
- No es necesario el paso 1
- Hay que modificar todo lo de "quienSoy"
- Para ver si se echa a andar
