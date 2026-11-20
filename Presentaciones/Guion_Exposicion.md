# Guion para exponer: Árboles y veredas

**Duración aproximada: 10 minutos · 9 diapositivas.**  
El guion explica los gráficos y propone frases para contar el proyecto con lenguaje sencillo. No hace falta leerlo palabra por palabra: sirve como apoyo para hablar con naturalidad.

## Antes de empezar

- Abrir `Presentaciones/Proyecto_Final.html` en el navegador.
- Usar `→` o **Siguiente** para avanzar; `←` para volver y **F** para pantalla completa.
- Las diapositivas muestran el tiempo sugerido y el recorrido total de 10 minutos.
- Si se muestra la demostración de la API, dejarla abierta antes de iniciar. Si no está disponible, explicar el ejemplo en la diapositiva 8 sin interrumpir la exposición.

## Guion por diapositiva

### 1. Árboles y veredas — 0:30

**Qué decir**

> Hola. Nuestro proyecto analiza si aparecen relaciones entre las características de los árboles de calle y el estado de la vereda que quedó anotado junto a ellos. Para hacerlo usamos un censo público de Nueva York. Primero vamos a contar qué pregunta hicimos, qué datos usamos y qué aprendimos al analizarlos.

**Transición:** “Empecemos por explicar por qué elegimos este tema”.

### 2. ¿Por qué elegimos este tema? — 1:00

**Qué decir**

> Una vereda dañada puede dificultar el paso de las personas y necesitar mantenimiento. Nos preguntamos si los datos públicos podían ayudarnos a describir qué características aparecen junto a los casos con daño anotado. El objetivo fue aprender a recorrer todo un proyecto de datos, desde revisar la información hasta probar una demostración. No buscamos predecir cuándo se romperá una vereda ni indicar qué obra hacer.

**Idea importante:** decir **“aparece relacionado”**, no **“lo causa”**.

**Transición:** “Para responder esa pregunta, primero tuvimos que conocer el origen y el contenido de los datos”.

### 3. Una muestra de 100.000 árboles — 1:05

**Qué decir**

> Los datos vienen del censo de árboles de calle de Nueva York realizado en 2015. Para trabajar con un tamaño manejable usamos una muestra reproducible de 100.000 árboles. Conservamos árboles vivos, con estado de vereda anotado y un diámetro dentro del rango elegido para el análisis. En la muestra, 71.223 veredas aparecen anotadas sin daño y 28.777 con daño.
>
> La barra muestra esa diferencia: aproximadamente siete de cada diez registros no tienen daño anotado y tres de cada diez sí. Por eso, si sólo contamos el porcentaje general de aciertos, podríamos llevarnos una impresión equivocada del modelo. Además, son datos de Nueva York de 2015; no describen otras ciudades ni la situación actual.

**Cómo explicar el gráfico:** verde = sin daño anotado; amarillo = con daño anotado. La longitud representa cuántos casos hay de cada tipo.

**Transición:** “Después de revisar los datos, miramos qué patrones descriptivos aparecían”.

### 4. El tamaño del árbol y el daño anotado — 1:20

**Qué decir**

> En este gráfico se agrupan los árboles según el diámetro del tronco. Cada barra indica qué porcentaje de las veredas de ese grupo quedó anotado con daño. En los árboles de hasta seis pulgadas es aproximadamente 15%; en el grupo de 36 a 60 pulgadas, cerca de 49%.
>
> En esta muestra, los grupos de árboles más gruesos tienen una proporción mayor de daño registrado. Esto no demuestra que el diámetro sea la causa: puede haber otras diferencias, como el lugar, la especie, el mantenimiento o la forma en que se registró la información. El último grupo, además, es más pequeño: contiene 984 árboles.

**Cómo explicar el gráfico:** leer de izquierda a derecha; las barras más largas representan una proporción observada mayor, no una probabilidad futura.

**Transición:** “Para pasar de la exploración a la evaluación, usamos varias herramientas; cada una resolvió una parte distinta”.

### 5. ¿Qué tecnologías usamos y para qué? — 1:15

**Qué decir**

> Usamos Python como lenguaje de trabajo. Con Pandas organizamos y filtramos las tablas; con Jupyter documentamos el análisis junto con sus gráficos. Matplotlib y Seaborn nos ayudaron a representar los datos. Para comparar métodos y entrenar el clasificador usamos scikit-learn. El modelo elegido, Random Forest, combina las respuestas de muchos árboles de decisión sencillos. Finalmente, FastAPI permite consultar una demostración del modelo.
>
> La ventaja de organizar los pasos junto al modelo es que podemos repetir la misma preparación de datos cuando evaluamos casos nuevos. Eso ayuda a que la comparación sea más justa y reproducible.

**Si preguntan por K-Means o DBSCAN:** son dos métodos que agrupan observaciones parecidas sin indicarles de antemano cuál es la respuesta correcta; los mostramos en la siguiente diapositiva.

**Transición:** “Ahora veamos qué tan bien respondió el clasificador y, sobre todo, dónde se equivocó”.

### 6. ¿Qué tan bien funcionó? — 1:50

**Qué decir**

> Reservamos 20.000 casos para probar el modelo con datos que no usó para aprender. De cada 100 casos que en el censo figuraban con daño, detectó alrededor de 62 y dejó pasar cerca de 38. Si miramos todos los avisos que generó, aproximadamente 49 de cada 100 correspondían a casos con daño anotado.
>
> La tabla cruza lo que decía el censo con la respuesta del modelo. La diagonal verde son los aciertos. Arriba a la derecha están 3.622 avisos de daño en casos que el censo no anotó con daño. Abajo a la izquierda hay 2.216 casos de daño que el modelo no detectó.
>
> Esto también explica por qué no alcanza con el porcentaje general de aciertos: el modelo tuvo 70,8%, pero una regla que siempre respondiera “sin daño” habría llegado a 71,2% y no habría detectado ningún caso dañado. Por eso miramos también cuántos daños logra encontrar y cuántos avisos son correctos.

**Cómo explicar la matriz:** filas = lo que estaba anotado; columnas = lo que respondió el modelo. Recorrer primero los dos aciertos de la diagonal y luego los dos tipos de error.

**Transición:** “Además de clasificar casos, hicimos una exploración diferente: agrupar árboles parecidos sin usar la etiqueta de daño”.

### 7. Agrupar casos parecidos — 1:05

**Qué decir**

> Tomamos 10.000 árboles y buscamos grupos con características parecidas, sin decirle al método si tenían daño anotado. El gráfico muestra el tamaño de los tres grupos que encontró K-Means: uno pequeño de 670 árboles, uno grande de 7.004 y otro de 2.326. En este último, el diámetro mediano fue de 16 pulgadas.
>
> También probamos DBSCAN, que forma grupos de otra manera y puede dejar algunos casos sin asignar a un grupo. Encontró cinco grupos y marcó como ruido al 1,2% de los casos. Estos resultados dependen de las variables y de cómo las preparamos; los grupos no son niveles de peligro ni categorías oficiales.

**Cómo explicar el gráfico:** la longitud y el número muestran cuántos árboles cayeron en cada grupo; no indican calidad, riesgo o importancia.

**Transición:** “Para mostrar cómo se podría consultar el modelo, preparamos una pequeña demostración”.

### 8. Del análisis a una demostración — 1:10

**Qué decir**

> Además de los cuadernos de análisis, preparamos una interfaz local con FastAPI. Se ingresan características básicas del árbol y la demostración devuelve la clasificación del modelo. La página `/docs` explica qué campos acepta y permite enviar una consulta de prueba.
>
> Esto sólo muestra cómo conectar el modelo con una interfaz. No es un servicio público, no está preparado para uso municipal y su resultado no debería decidir una intervención. Para levantarlo hay que entrenar primero el modelo y luego ejecutar el comando que aparece en pantalla.

**Si hacen la demostración en vivo:** usar un ejemplo preparado y explicar que el resultado depende de un modelo entrenado con el censo de Nueva York de 2015.

**Transición:** “Para cerrar, resumimos qué aprendimos y qué haría falta antes de pensar en un uso real”.

### 9. Cierre — 0:45

**Qué decir**

> Aprendimos a usar datos públicos para explorar una pregunta, hacer gráficos, comparar respuestas y comunicar errores, no sólo aciertos. Pero el censo es antiguo y corresponde a Nueva York. No comprobamos que los resultados se mantengan hoy o en otra ciudad.
>
> La conclusión es que este análisis describe lo que quedó registrado en 2015. No predice el futuro ni reemplaza una inspección humana. Gracias. ¿Qué preguntas tienen?

## Términos por si aparecen preguntas

| Término | Explicación sencilla |
|---|---|
| **Clasificar** | Elegir entre categorías; aquí, “daño anotado” o “sin daño anotado”. |
| **Random Forest** | Un método que combina muchas decisiones sencillas para producir una respuesta. |
| **Agrupar (clustering)** | Juntar casos que se parecen sin usar una respuesta conocida. |
| **API** | Una forma definida para que un programa reciba datos y devuelva una respuesta. |
| **Datos de prueba** | Casos reservados para revisar el modelo; no se usan para entrenarlo. |
| **Aviso falso** | El modelo indica daño, pero en el censo ese caso figura sin daño. |
| **Daño no detectado** | El censo indica daño, pero el modelo responde sin daño. |

## Preguntas que podrían hacer

**¿Entonces el árbol grueso rompe la vereda?**  
No podemos afirmarlo. El gráfico muestra una relación dentro de esta muestra; no prueba causa y efecto.

**¿El modelo predice qué veredas se van a romper?**  
No. Clasifica el estado anotado en el censo de 2015; no tiene una secuencia de datos en el tiempo para pronosticar el futuro.

**¿Por qué no alcanza con el porcentaje total de aciertos?**  
Porque la mayoría de los casos no tiene daño anotado. Una regla que siempre responde “sin daño” logra un porcentaje global parecido, pero no encuentra ninguno de los casos con daño.

**¿Se puede aplicar en una ciudad local?**  
No directamente. Harían falta datos actuales del lugar, pruebas de calidad y una evaluación con personas responsables de las decisiones.

**¿Qué significa que haya grupos de árboles?**  
Que, con las variables elegidas, algunas observaciones se parecen entre sí. No significa que sean categorías oficiales ni que indiquen riesgo.
