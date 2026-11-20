# Guion detallado para exponer: Árboles y veredas

Guion de apoyo para explicar **qué se hizo, por qué se hizo, qué muestran los resultados y qué no se puede concluir**. La presentación visual no muestra tiempos ni notas; este documento es para preparar la exposición y fundamentar las respuestas.

El texto marcado como **Guion oral** se puede decir en la exposición. Los apartados **Fundamento** y **Si preguntan** sirven para ampliar, sin necesidad de leerlos todos. La secuencia propuesta dura aproximadamente 10 minutos.

## Preparación

1. Abrir `Presentaciones/Proyecto_Final.html` en un navegador.
2. Avanzar con `→` o **Siguiente**; retroceder con `←`; usar **F** para pantalla completa.
3. Si se va a mostrar la API, iniciar el servidor antes de comenzar. Si no está listo, explicar la diapositiva 8 sin hacer la demostración en vivo.
4. Recordar la idea que mantiene unido todo el relato: **el proyecto describe una observación de Nueva York de 2015; no predice el futuro ni determina qué intervención realizar**.

## Guion por diapositiva

### 1. Árboles y veredas — 0:30

**Propósito:** presentar el tema y adelantar el alcance.

**Guion oral**

> Hola. Nuestro proyecto parte de una pregunta sencilla: ¿qué características de los árboles aparecen junto a veredas que fueron registradas con daño? Para explorarlo usamos un censo público de Nueva York. Vamos a mostrar qué datos usamos, qué patrones encontramos y cómo evaluamos una demostración de clasificación. Desde el comienzo es importante aclarar que trabajamos con el estado anotado en 2015, no con daños futuros.

**Fundamento:** el nombre del proyecto comunica los dos elementos del análisis. No presentar “VeredaSegura” como una aplicación municipal ya desplegada: es un proyecto académico reproducible.

**Transición:** “Primero, ¿por qué pensamos que esta pregunta era útil para explorar?”

### 2. ¿Por qué elegimos este tema? — 1:00

**Propósito:** explicar la motivación y definir la pregunta de análisis.

**Guion oral**

> Una vereda dañada puede dificultar el paso de peatones y requerir mantenimiento. Nos interesó saber si un conjunto de datos podía mostrar relaciones entre el arbolado y el estado de la vereda anotado junto a cada árbol. Esto nos permitió practicar el recorrido completo de un proyecto de datos: revisar información, representarla, entrenar y evaluar un modelo y explicar sus límites. La pregunta no es “¿qué árbol va a romper la vereda?”, sino “¿qué características coinciden con los daños que se registraron en el censo?”.

**Fundamento:** esta precisión define el tipo de problema: una clasificación entre dos etiquetas observadas. La fila representa un árbol con sus características y el estado registrado de la vereda asociada. No tenemos mediciones sucesivas que permitan establecer cuándo ocurrió el daño.

**Si preguntan si los árboles causan el daño:** responder que este análisis no permite afirmar causalidad. Sólo busca asociaciones en datos observacionales.

**Transición:** “Para estudiar esa pregunta, seleccionamos y preparamos datos del censo”.

### 3. Una muestra de 100.000 árboles — 1:05

**Propósito:** explicar de dónde salen los datos y cómo se distribuyen las etiquetas.

**Guion oral**

> Usamos el Censo de Árboles de Calle de Nueva York de 2015, publicado en NYC Open Data. Para que el análisis fuera manejable, generamos una muestra reproducible de 100.000 árboles. Conservamos árboles vivos, con el estado de vereda conocido y con diámetros de entre 1 y 60 pulgadas. En la muestra, 71.223 registros indican que no se anotó daño y 28.777 indican que sí. Por eso las dos clases no están representadas en la misma proporción.

**Cómo leer el gráfico:** la barra verde representa el 71,2% sin daño anotado y la amarilla el 28,8% con daño anotado. La longitud es la proporción de registros, no el nivel de gravedad.

**Fundamento**

- El censo original contiene aproximadamente 683.788 filas y 45 campos; el archivo de trabajo incluido en el proyecto es una muestra filtrada de 100.000 filas y 20 campos.
- El descargador usa una semilla fija para poder repetir la selección. “Reproducible” significa que, con los mismos datos disponibles y la misma semilla, el procedimiento puede repetirse.
- El desbalance importa: si la mayoría de los registros corresponde a “sin daño”, un método podría acertar muchas veces respondiendo siempre esa clase y aun así no encontrar ningún caso con daño.
- La muestra representa ese conjunto filtrado de NYC, no a todas las ciudades ni la situación actual.

**Calidad y faltantes:** en la muestra hubo celdas vacías, sobre todo en la información de protectores (`guards`), cuidado humano (`steward`) y problemas reportados. Un campo vacío no prueba que “no exista” esa característica. En el modelado se reemplazan algunas ausencias con valores como `None` o `Unknown` para procesar los casos; es una decisión práctica y supone que la ausencia puede representar falta de información o ausencia del elemento.

En números: el 11,39% de las celdas de la muestra estaba vacío; `guards` tenía 87,78% de valores vacíos, `steward` 74,73% y `problems` 65,37%. `problems` se exploró y completó para documentar el tratamiento, pero no fue una variable de entrada del clasificador final.

**Transición:** “Una vez entendido el conjunto, exploramos cómo cambiaba el daño registrado según algunas características”.

### 4. El tamaño del árbol y el daño anotado — 1:20

**Propósito:** enseñar a leer un patrón descriptivo sin presentarlo como causa.

**Guion oral**

> Agrupamos los árboles por el diámetro de su tronco y calculamos qué porcentaje de las veredas de cada grupo aparece anotado con daño. En el grupo de hasta 6 pulgadas es aproximadamente 15%; en el grupo de 36 a 60 pulgadas es aproximadamente 49%. La tendencia sube a medida que avanzamos hacia los grupos de mayor diámetro. Es un patrón que observamos en esta muestra, pero no demuestra que el tamaño del árbol sea la causa del daño.

**Cómo leer el gráfico**

1. Cada fila es un rango de diámetro.
2. El número al final de la barra es el porcentaje de registros con daño anotado dentro de ese rango.
3. Las barras más largas indican una proporción mayor, no que el modelo tenga más certeza.
4. El último grupo contiene 984 árboles, muchos menos que algunos grupos anteriores; por eso es importante mirar también el tamaño de cada grupo.

**Fundamento:** es un análisis exploratorio, no una comparación experimental. La ubicación, especie, mantenimiento, condiciones de la vereda y calidad del registro podrían estar relacionadas con las diferencias. No aislamos todos esos factores ni demostramos cuál explica el patrón.

Además, se crearon tres conteos: cuántos problemas de raíz, tronco y ramas fueron anotados para cada árbol. Esto resume nueve indicadores de “Sí/No” en tres cantidades fáciles de comparar. Por ejemplo, un puntaje de raíz igual a 2 significa que se registraron dos de los tres tipos de conflicto de raíz incluidos.

Otro patrón exploratorio fue la tasa por conflictos de raíz: aproximadamente 19,7% con cero conflictos, 55,3% con uno y 63,3% con dos. Sólo ocho árboles tenían tres conflictos, así que ese grupo es demasiado pequeño para generalizar. Estos porcentajes no identifican qué factor causa el daño.

**Si preguntan por la palabra “probabilidad” en este gráfico:** hablar de “proporción observada en la muestra”; no es un pronóstico individual.

**Transición:** “Además de estos gráficos, usamos herramientas para ordenar la información y evaluar los modelos”.

### 5. ¿Qué tecnologías usamos y para qué? — 1:15

**Propósito:** relacionar cada tecnología con una tarea concreta.

**Guion oral**

> Python fue el lenguaje con el que construimos el proyecto. Pandas nos permitió trabajar con las tablas; Jupyter, dejar el análisis explicado paso a paso; y Matplotlib y Seaborn, crear gráficos. Para preparar datos y comparar métodos usamos scikit-learn. El clasificador elegido, Random Forest, reúne muchas decisiones sencillas para producir una respuesta. Finalmente, FastAPI permite enviarle datos de prueba al modelo y recibir su resultado. Elegimos estas herramientas porque se pueden combinar y permiten repetir el análisis.

**Fundamento por herramienta**

| Herramienta | Qué hizo en el proyecto | Por qué fue útil |
|---|---|---|
| **Python** | Ejecutó los notebooks, descargador, entrenamiento y API. | Un mismo lenguaje sirve para el análisis y la demostración. |
| **Pandas y NumPy** | Leyeron, filtraron, resumieron y transformaron tablas. | Facilitan inspeccionar muchas filas y crear variables. |
| **Jupyter** | Organizó código, gráficos, resultados y explicaciones. | Permite mostrar el razonamiento de manera revisable. |
| **Matplotlib y Seaborn** | Generaron gráficos exploratorios y de resultados. | Ayudan a comunicar patrones y errores visualmente. |
| **scikit-learn** | Separó datos, preparó categorías, entrenó y evaluó modelos. | Proporciona piezas compatibles que se pueden combinar y repetir. |
| **Random Forest** | Clasificó los casos combinando decisiones de varios árboles. | Se probó como una opción adecuada para una mezcla de variables numéricas y categóricas. |
| **FastAPI** | Expuso una consulta local de prueba. | Permite ver cómo un sistema podría recibir datos y devolver una respuesta. |

**Cómo se conectan los pasos**

1. La separación reserva 20.000 casos para una prueba final y deja 80.000 para aprender y comparar.
2. Se usa una separación estratificada: conserva proporciones parecidas de casos con y sin daño anotado en ambos conjuntos.
3. La codificación convierte categorías escritas —por ejemplo, especie o borough— en columnas numéricas que el modelo puede procesar.
4. El `Pipeline` agrupa esas transformaciones y el clasificador. Así, en las cinco rondas de validación, cada ronda prepara los datos sin mirar la parte que está usando para validar.
5. Los campos ordinales, como salud, se codifican con un orden; las categorías sin orden, como borough, se transforman en indicadores. Las especies muy poco frecuentes se agrupan para evitar muchas columnas casi vacías.
6. Se asigna mayor peso a la clase menos frecuente durante el entrenamiento (`class_weight="balanced"`). Esto intenta que el modelo no ignore “con daño”, pero no elimina los errores ni corrige los sesgos de origen.

**Por qué no se escalan los números del modelo de árboles:** los árboles separan valores con reglas como “mayor o menor que cierto valor”; por eso no necesitan poner todas las variables en una misma escala. En el agrupamiento sí se estandarizan las variables numéricas porque K-Means compara distancias.

**Transición:** “Veamos ahora cómo respondió y qué errores cometió”.

### 6. ¿Qué tan bien funcionó? — 1:50

**Propósito:** explicar el balance entre casos encontrados y avisos equivocados.

**Guion oral**

> Apartamos 20.000 casos para probar el modelo después del entrenamiento. De cada 100 casos que el censo tenía anotados con daño, el modelo encontró alrededor de 62 y no encontró cerca de 38. Entre todos los casos que señaló como dañados, aproximadamente 49 de cada 100 sí tenían daño anotado. Esto muestra que detecta parte de los casos, pero también produce avisos equivocados.
>
> En la tabla, las filas muestran lo que decía el censo y las columnas lo que respondió el modelo. Los valores de la diagonal verde son respuestas correctas: 10.623 casos sin daño y 3.539 casos con daño. Las otras dos celdas son errores: 3.622 avisos falsos y 2.216 daños no detectados.
>
> El acierto general fue 70,8%. Una regla que siempre dijera “sin daño” llega a 71,2%, pero no encuentra ninguno de los daños. Por eso no evaluamos el sistema sólo con un porcentaje total: también preguntamos qué clase detecta y cuántas alertas son correctas.

**Cómo leer la matriz**

| Lo que anotó el censo | Respuesta del modelo | Nombre sencillo |
|---|---|---|
| Sin daño | Sin daño | Acierto |
| Sin daño | Con daño | Aviso falso |
| Con daño | Sin daño | Daño no detectado |
| Con daño | Con daño | Acierto |

**Fundamento cuantitativo**

- Hubo 5.755 casos con daño anotado en el test; el modelo encontró 3.539: `3.539 / 5.755 ≈ 61,5%`.
- El modelo generó 7.161 respuestas “con daño”; 3.539 eran correctas: `3.539 / 7.161 ≈ 49,4%`.
- Por eso la precisión no es alta: más de la mitad de sus avisos fueron falsos en este test.
- La medida resumen F1 fue 0,548; combina la proporción de daños encontrados con la proporción de avisos correctos. El valor sirve para comparar modelos, no debe interpretarse como “54,8% de probabilidad”.
- El modelo superó al método trivial en la detección de la clase minoritaria, pero no tuvo mayor acierto global que ese método. La elección de qué errores son más costosos depende del uso, y aquí no se estableció ningún umbral operativo.

**Cómo se eligieron los ajustes:** se compararon varias configuraciones del modelo mediante cinco rondas de validación sobre los datos de entrenamiento. El ajuste con mejor promedio de F1 fue Random Forest con 100 árboles, profundidad máxima 16 y mínimo de 10 casos por hoja. La prueba final se dejó aparte para medir el resultado después de seleccionar esos valores. Esto reduce el riesgo de elegir los ajustes mirando el test.

Antes del ajuste se compararon cuatro opciones: una respuesta base que siempre elige la clase mayoritaria, regresión logística, un árbol de decisión y Random Forest. Sus resultados de validación fueron parecidos y el ajuste del bosque obtuvo F1 promedio 0,544. En la evaluación final el F1 fue 0,548.

**Si preguntan por el umbral:** la demostración usa 0,5 como regla simple para convertir la probabilidad del modelo en una clase. No se optimizó para una decisión municipal ni para un costo real de inspección.

**Transición:** “Además de predecir una clase conocida, exploramos si había grupos de árboles parecidos”.

### 7. Agrupar casos parecidos — 1:05

**Propósito:** diferenciar la búsqueda de grupos de la clasificación supervisada.

**Guion oral**

> En otra parte del proyecto usamos 10.000 árboles y buscamos grupos con características parecidas, sin mostrarle al método si la vereda tenía daño. K-Means formó tres grupos: uno con 670 árboles, otro con 7.004 y otro con 2.326. El grupo de 2.326 tuvo una mediana de diámetro de 16 pulgadas. Probamos también DBSCAN, que puede dejar algunos casos apartados; encontró cinco grupos y marcó como ruido al 1,2% de los casos.
>
> Estos grupos dependen de qué características incluimos y de cómo las representamos. No son niveles de peligro ni categorías oficiales.

**Cómo leer el gráfico:** la longitud compara tamaños de grupos. No indica qué grupo es mejor o más riesgoso. Los nombres “Grupo 1, 2 y 3” son sólo etiquetas para distinguirlos.

**Fundamento:** en esta exploración no se usaron ni la etiqueta de daño ni el estado de vereda para formar grupos. Primero se estandarizaron los números —para que el diámetro no domine sólo por su escala— y las categorías se transformaron en indicadores numéricos. K-Means se probó con distintas cantidades de grupos, de 2 a 7; el mejor resultado de separación fue aproximadamente 0,282 con 3 grupos. Las medianas de diámetro fueron 11, 8 y 16 pulgadas; las medianas de conflictos de raíz fueron 0, 0 y 1. La separación fue modesta, así que no se deben sobreinterpretar las agrupaciones. DBSCAN encontró cinco grupos; 1,2% de los casos quedó como ruido. Su puntuación de separación fue 0,333, calculada sólo con los casos no marcados como ruido. La gran cantidad de categorías también limita la interpretación de las distancias.

**Si preguntan por los nombres:** K-Means y DBSCAN son algoritmos de agrupamiento. Para la exposición alcanza con decir que buscan semejanzas usando distintos criterios.

**Transición:** “También preparamos una manera sencilla de consultar el clasificador”.

### 8. Del análisis a una demostración — 1:10

**Propósito:** mostrar cómo se conecta un modelo entrenado con una consulta, sin afirmar que está en producción.

**Guion oral**

> Preparamos una demostración local con FastAPI. Se ingresan características básicas de un árbol y el sistema muestra cómo lo clasifica el modelo. La página `/docs` describe los campos y permite enviar una consulta de prueba. Para iniciarla hay que entrenar primero el modelo y después ejecutar el comando que aparece en pantalla.
>
> La demostración sirve para enseñar el recorrido desde los datos hasta una respuesta. No está publicada como servicio, no fue validada para una ciudad local y no debe usarse para decidir inspecciones reales.

**Fundamento técnico**

- Una API es un acuerdo sobre cómo enviar datos a un programa y cómo recibir una respuesta.
- FastAPI crea las rutas; Pydantic valida que los datos tengan el formato y los rangos esperados.
- `/health` informa si el servicio está respondiendo y si cargó el modelo; `/predict` recibe la observación; `/docs` muestra una pantalla de prueba.
- El archivo del modelo se genera localmente al ejecutar `python Final/train.py`; no es necesario versionar el binario para reproducir el proyecto.
- La probabilidad que devuelve el modelo no fue calibrada para uso operativo y no representa una certeza.

**Si la demostración falla:** explicar el flujo con la diapositiva. No perder tiempo reinstalando ni presentar la API como una aplicación municipal terminada.

**Transición:** “Para cerrar, resumimos qué aporta el trabajo y qué haría falta para ir más lejos”.

### 9. ¿Qué nos llevamos? — 0:45

**Propósito:** terminar con aprendizaje, límite y posible siguiente paso.

**Guion oral**

> Aprendimos a recorrer un proyecto completo: obtener datos, analizarlos, comparar modelos y comunicar tanto sus aciertos como sus errores. Pero los datos son de Nueva York y de 2015, y el modelo todavía comete errores. Antes de plantear un uso real harían falta datos actuales del lugar, validaciones adicionales y revisión humana. Nuestra conclusión es sencilla: este análisis describe lo registrado en un censo histórico; no predice el futuro ni reemplaza una inspección. Gracias, ¿qué preguntas tienen?

**Fundamento y cierre:** la principal contribución es didáctica y exploratoria. Una posible continuación sería contrastar el procedimiento con datos recientes de otra ciudad, con autorización y participación de quienes conocen el problema. No presentar esa continuación como un trabajo ya realizado.

## Conceptos clave para fundamentar preguntas

| Concepto | Explicación en lenguaje sencillo | Cómo se aplicó |
|---|---|---|
| **Clasificación** | Elegir una categoría para cada caso. | “Daño anotado” o “sin daño anotado”. |
| **Variable objetivo** | La respuesta que queremos comparar o estimar. | El estado `sidewalk` registrado en el censo. |
| **Variables de entrada** | Datos disponibles para describir cada caso. | Tamaño, especie, salud y características del entorno. |
| **Random Forest** | Un método que combina muchas decisiones sencillas para producir una respuesta. | Clasificación supervisada de los casos. |
| **Búsqueda de ajustes** | Probar configuraciones distintas con los datos de entrenamiento y elegir la que mejor valida. | Se usó para elegir los parámetros del bosque de árboles. |
| **Pipeline** | Una secuencia guardada de preparación de datos y modelo, para repetir los mismos pasos. | Mantiene juntas la codificación y la clasificación. |
| **Entrenamiento** | Ajustar el modelo usando ejemplos conocidos. | 80.000 observaciones de la muestra. |
| **Prueba** | Revisar el modelo en ejemplos reservados. | 20.000 observaciones que no se usaron para ajustarlo. |
| **Estratificar** | Mantener proporciones de clases parecidas al separar los datos. | Se conserva aproximadamente el 28,8% de la clase “con daño” en train y test. |
| **Validación cruzada** | Repetir la evaluación en varias particiones del entrenamiento. | Se usaron cinco particiones para comparar ajustes. |
| **Sobreajuste** | Aprender demasiado los ejemplos de entrenamiento y rendir peor en casos nuevos. | Se compara el resultado del entrenamiento con la validación y el test. |
| **Precisión de avisos** | Entre los casos que el modelo señaló, cuántos sí tenían daño anotado. | 49,4% en el test. |
| **Detección de daños (recall)** | Entre los casos con daño anotado, cuántos encontró el modelo. | 61,5% en el test. |
| **F1** | Una medida que resume precisión de avisos y detección de daños. | 0,548 para “con daño” en el test. |
| **ROC-AUC** | Una medida de cuánto distingue el modelo los dos grupos al variar el umbral. | 0,741; no significa que el modelo “acierte 74,1%”. |
| **Agrupamiento** | Juntar observaciones parecidas sin usar una respuesta conocida. | K-Means y DBSCAN sobre 10.000 árboles. |
| **API** | Una forma definida para que un programa reciba datos y devuelva una respuesta. | FastAPI ofrece `/health`, `/predict` y `/docs`. |
| **Causalidad** | Demostrar que una variable produce otra. | Este análisis no la demuestra porque los datos son observacionales. |

## Preguntas frecuentes y respuestas cuidadosas

**¿El tamaño del árbol causa el daño?**

No lo sabemos con este análisis. El gráfico muestra que el daño anotado fue más frecuente en los grupos de mayor diámetro, pero otras características podrían explicar parte de la diferencia.

**¿El modelo predice qué veredas se van a romper?**

No. Clasifica el estado que quedó anotado en el censo de 2015. No hay observaciones a lo largo del tiempo que permitan hacer un pronóstico futuro.

**¿Por qué el método de siempre decir “sin daño” tiene más acierto general?**

Porque “sin daño” es la clase más numerosa. Aun así, ese método no detecta ninguno de los casos con daño. Por eso miramos también la detección y los avisos falsos.

**¿Entonces Random Forest sirve o no sirve?**

En este ejercicio detectó alrededor de seis de cada diez casos con daño, pero generó muchos avisos falsos y dejó daños sin detectar. El resultado puede ser útil para aprender y comparar, no para decidir por sí solo.

**¿Se puede usar en Argentina?**

No directamente. Harían falta datos locales actuales, revisar cómo se registran las veredas y evaluar el modelo con personas responsables del mantenimiento y de la movilidad urbana.

**¿Qué significa que haya tres grupos de árboles?**

Que el método encontró tres conjuntos parecidos según las variables usadas. No significa que existan tres tipos oficiales de árbol o tres niveles de riesgo.

**¿Por qué se incluyó una API si el modelo no es de producción?**

Para demostrar cómo se podría consultar un modelo desde una interfaz. Es un componente educativo, no un despliegue ni una autorización de uso real.

## Reproducción por si preguntan

Desde la carpeta raíz del repositorio, con las dependencias instaladas:

```bash
python Final/download_dataset.py --rows 100000
python Final/train.py
uvicorn Final.app:app --reload
```

La presentación corresponde al proyecto local de la carpeta `YPF-curso-Data-Science`. Las métricas de referencia están guardadas en `Final/artifacts/metrics.json`; pueden cambiar si se regenera la fuente o se modifican las opciones de entrenamiento.
