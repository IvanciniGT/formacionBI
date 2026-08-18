# Temario del curso: contenidos, sesiones y equivalencias

Material de referencia del curso **ADGG102PO · Business Intelligence** (Acción/Grupo 10/3, 30 horas en 8 sesiones de 3 h 45 min).

Este documento recorre los contenidos formativos oficiales y, para cada uno, indica:

- **Qué se ha impartido** y en qué sesión.
- **Qué ejemplo concreto** lo ilustra, para que podáis volver sobre él.
- En el caso de los conceptos que el programa oficial recoge pero que la industria ha dejado atrás: **qué eran, por qué dejaron de usarse y qué los ha sustituido**.

Las notas de cada sesión están en `notas/dia1.md` … `notas/dia8.md`.

---

## Índice

1. [El planteamiento del curso](#1-el-planteamiento-del-curso)
2. [Conceptos del temario que hoy hay que matizar](#2-conceptos-del-temario-que-hoy-hay-que-matizar)
3. [Tabla de equivalencias: ayer y hoy](#3-tabla-de-equivalencias-ayer-y-hoy)
4. [Recorrido por los contenidos formativos](#4-recorrido-por-los-contenidos-formativos)
5. [Índice de sesiones](#5-índice-de-sesiones)
6. [Trazabilidad completa del temario oficial](#6-trazabilidad-completa-del-temario-oficial)

---

## 1. El planteamiento del curso

El curso se articula sobre tres partes, que son las tres cosas que hacen falta para que un dato acabe cambiando una decisión:

```
ANÁLISIS DE DATOS (Estadística)     Qué significan los datos
INFORMÁTICO (Soporte + operaciones) Dónde viven y cómo se procesan
BUSINESS INTELLIGENCE               Cuadros de mando, indicadores, decisiones
```

La idea que sostiene todo lo demás se enuncia el primer día y no se abandona:

> **Tener los datos no es lo mismo que entender los datos.** Y cuantos más datos, menos se entiende. Toda la estadística —y por extensión todo el Business Intelligence— existe para cubrir esa distancia.

De ahí sale la definición operativa de BI que se usa en el curso: la estadística nos dice **cómo** resumir los datos para entenderlos; pero hacerlo a mano sobre millones de registros es inviable. Hacen falta además técnicas informáticas que permitan calcular eso de forma rápida y eficiente. **Esa suma —estadística más informática, al servicio de la decisión— es Business Intelligence.**

### Por qué el orden del curso no es el del programa oficial

El programa oficial arranca por el Datawarehouse y OLAP, y deja el análisis para el final. En el curso se ha invertido:

1. **Primero la estadística**, porque es lo que determina si un indicador significa algo. Un cuadro de mando construido sobre una media mal elegida es falso por muy bien que esté el almacén que hay debajo.
2. **Después la informática**: dónde se guardan los datos, en qué formato y con qué modelo.
3. **Y por último el BI propiamente dicho**: cuadros de mando, minería y proyecto.

La cobertura del temario es completa. Lo que cambia es la secuencia.

---

## 2. Conceptos del temario que hoy hay que matizar

Igual que ocurre en otros programas formativos, algunos puntos del temario describen el estado de la tecnología de hace veinte años. Conviene conocerlos —aparecen en sistemas heredados y en la documentación— pero también saber en qué han quedado.

### ROLAP, MOLAP y HOLAP

**Qué eran.** Tres formas de implementar un cubo multidimensional.

| Sigla | Cómo almacena | Ventaja | Inconveniente |
|---|---|---|---|
| **MOLAP** | Estructura multidimensional propia, con todas las agregaciones precalculadas | Consultas casi instantáneas | Procesamiento largo; límite de volumen; poca flexibilidad |
| **ROLAP** | No hay cubo físico: las consultas se traducen a SQL sobre un modelo en estrella | Escala mucho mejor | Más lento en consulta |
| **HOLAP** | Agregados en estructura multidimensional, detalle en la base relacional | Compromiso entre ambos | Complejidad de los dos mundos |

**Por qué la distinción se ha diluido.** Por dos motivos independientes:

- **Los motores columnares en memoria.** La razón de ser de MOLAP era evitar recorrer millones de filas en tiempo de consulta. Cuando el motor guarda los datos **por columnas**, comprimidos y en memoria, esa lectura deja de ser cara: un análisis que solo necesita dos columnas de una tabla de cincuenta no toca las otras cuarenta y ocho. Es la tecnología que hay debajo de Power BI, de Tableau y de los modelos tabulares de Analysis Services.
- **Los almacenes en la nube.** Escalan el cálculo bajo demanda, con lo que el enfoque relacional dejó de ser «el lento».

**Estado actual.** MOLAP sobrevive en instalaciones heredadas de SQL Server Analysis Services en modo multidimensional. Los desarrollos nuevos son, casi sin excepción, columnares en memoria o directamente sobre el almacén en la nube.

**Qué sí sobrevive intacto.** El **cubo como modelo lógico**: hechos, dimensiones, jerarquías y agregaciones. Y las operaciones OLAP —*drill-down*, *drill-up*, *slice*, *dice*, *pivot*—, que son literalmente lo que hace un usuario con el ratón sobre un cuadro de mando. Por eso en el curso se dan **dentro** del ejercicio de cuadro de mando y no como capítulo aparte.

### El «cubo» como producto frente al cubo como concepto

Durante años, «montar un cubo» significaba desplegar una pieza de software específica, procesarla por las noches y consultarla con un lenguaje propio (MDX). Hoy, en la mayoría de las herramientas, el cubo es simplemente **el modelo semántico**: la definición de qué son los hechos, cuáles son las dimensiones, qué jerarquías tienen y cómo se calcula cada medida. Se define una vez y lo usan todos los informes.

El temario habla de «creación de cubos multidimensionales» (punto 3.8). En el curso eso se traduce en **diseñar el modelo dimensional**: grano, tabla de hechos, dimensiones y jerarquías.

### El almacén de datos y el data lake

El temario, por su fecha, presenta el Datawarehouse como el único destino de los datos históricos. Conviene situar las dos arquitecturas:

```
BBDD  ──ETL─────────────────────►  DATA WAREHOUSE      El camino clásico, y el del temario
BBDD  ──►  DATA LAKE  ──ELT────►  DATA WAREHOUSE      El camino moderno
```

El **data warehouse** es de los años noventa; el **data lake**, de 2010 en adelante. Durante dos décadas los almacenes se alimentaron directamente de los sistemas operacionales mediante ETL, y muchísimas organizaciones siguen así hoy. El data lake **no es un paso obligatorio**: es una capa previa que aporta valor cuando hay mucho volumen, muchas fuentes o formatos heterogéneos.

⚠️ Y un matiz importante sobre qué guarda un data lake: no es solo el archivo de lo que ya no se usa. Normalmente guarda **copias** de los datos, incluidos los vivos, porque en BI se analizan las ventas de este mes, no solo las de hace tres años.

### ETL y ELT

**ETL** (*Extract, Transform, Load*): se extrae, se transforma en un servidor intermedio y se carga ya limpio.

**ELT** (*Extract, Load, Transform*): se extrae, se carga en bruto y la transformación se ejecuta **dentro** del propio almacén, aprovechando su capacidad de cálculo.

⚠️ El cambio de uno a otro modifica **dónde** se transforma, no **si** hace falta transformar. La idea de que con un data lake ya no hay que limpiar los datos es la causa directa de muchos proyectos que terminan en lo que el sector llama un *data swamp*: un depósito de ficheros que nadie sabe interpretar.

---

## 3. Tabla de equivalencias: ayer y hoy

| Lo que dice el temario | Lo que se usa hoy | Qué se ha dado en el curso |
|---|---|---|
| Servidores OLAP MOLAP | Motores columnares en memoria | Por qué el almacenamiento por columnas hace innecesario precalcular |
| Servidores OLAP ROLAP | El almacén en la nube consultado con SQL | El modelo en estrella como base de la consulta |
| HOLAP | Modelos híbridos: importación + consulta directa | Mención y contexto |
| «Crear un cubo» | Definir el modelo semántico | Grano, hechos, dimensiones, jerarquías |
| MDX | DAX, SQL, lenguajes de medida de cada herramienta | Concepto de medida calculada |
| Datawarehouse como único destino | Data lake + almacén, o almacén directo | Las dos arquitecturas y cuándo aplica cada una |
| ETL | ETL y ELT según dónde esté la capacidad de cálculo | Ambos, con sus criterios |
| Minería de datos | Aprendizaje automático, con las mismas técnicas | Categorías por problema de negocio |
| Reporting estático | Cuadros de mando con navegación y autoservicio gobernado | Ejercicio completo de dos sesiones |

---

## 4. Recorrido por los contenidos formativos

### Punto 1.1 — Introducción

**Impartido en la sesión 1.** El curso abre con el problema, no con la definición: alguien consigue las 5.000 nóminas de una empresa —medio metro de papel— y sigue sin entender cómo se paga allí. Podría leerlas durante cuatro días y, al terminar, recordaría anecdóticamente el salario más alto y el más bajo. Nada más.

De ahí sale todo lo demás: hace falta **resumir**, porque un cerebro humano no procesa 5.000 datos. Y resumir sin destruir lo esencial es exactamente lo que ofrece la estadística.

La definición de BI se construye al final de la sesión 1, cuando ya se ha visto para qué sirve: un conjunto de técnicas **estadísticas e informáticas** que convierten datos en bruto en información que mejora la toma de decisiones, para que esas decisiones sean **informadas y no subjetivas**.

### Punto 1.2 — La pirámide organizacional

**Sesión 5**, como primera pregunta del ejercicio de cuadro de mando: ¿para quién es este cuadro?

| Nivel | Pregunta típica | Granularidad | Frecuencia |
|---|---|---|---|
| Operativo | ¿Qué tengo pendiente hoy? | El registro individual | Continua |
| Táctico | ¿Cómo voy frente al objetivo? | Agregado por unidad y mes | Semanal |
| Estratégico | ¿Dónde invierto el año que viene? | Muy agregado, series largas | Trimestral |

La consecuencia práctica: **la misma cifra no sirve para los tres niveles**. El error más frecuente en cuadros de mando corporativos es diseñar uno solo y repartirlo hacia arriba y hacia abajo; termina siendo demasiado agregado para quien opera y demasiado detallado para quien dirige.

### Punto 1.3 — Herramientas de inteligencia de negocios

**Sesiones 1 y 2**, dentro del glosario del mundo del dato, y **sesiones 5 y 6** en la práctica.

Se trabajan por **familias** y no por marcas, porque las marcas de dentro de cinco años serán otras: integración, almacenamiento analítico, modelo semántico, visualización, analítica avanzada y gobierno.

Y el vocabulario que hay que manejar para entenderse en el sector:

```
Business Intelligence (BI)
Ciencia de datos (Data Science)     → Data Mining · Machine Learning · Deep Learning
Ingeniería de datos (Data Engineering) → Big Data
```

### Puntos 1.4, 1.5 y 1.6 — Fundamentos, características y ventajas del Datawarehouse

**Sesión 1** (introducción, dentro del glosario) y **sesión 2** (desarrollo).

Un data warehouse es «otro tipo de base de datos» que guarda datos históricos ya preparados (**cocinados**) para un uso concreto. Sus propiedades:

- **Integrado**: mismos códigos, mismas unidades, mismos nombres, vengan de donde vengan.
- **Temático**: organizado por áreas de negocio, no por la aplicación de origen.
- **No volátil**: los datos no se modifican ni se borran; un informe de hace dos años debe poder reproducirse hoy.
- **Histórico**: guarda cómo eran las cosas **cuando ocurrieron**.

**Ventajas:** una versión única de la verdad, rendimiento analítico sin tocar la operación, historia completa —incluso de fuentes que ya no existen— y reglas de negocio aplicadas una sola vez.

**Y su coste, que también se dice:** los datos no están al minuto, cualquier cambio en una fuente obliga a tocar la carga, y requiere mantenimiento continuo y un responsable.

### Punto 1.7 — Sistemas OLTP

**Sesión 1.** *On-Line Transaction Processing*: los sistemas que sostienen la operación diaria. Están optimizados para registrar **muchas operaciones pequeñas, muy deprisa y sin perder ninguna**: altas, modificaciones y bajas.

Y precisamente por eso **no favorecen la analítica**. Hacer consultas de análisis sobre ellos es lento e ineficiente, y puede llegar a dejar frito el servidor de producción.

Ejemplos trabajados según el sector: un banco (transacciones, cuentas, tarjetas, préstamos), una aseguradora (pólizas, expedientes de siniestros) o un comercio (clientes, productos, ventas, puntos de venta).

Hay un segundo motivo, menos evidente y más grave: **el sistema operacional guarda el estado actual, no el histórico**. Si un artículo cambia de familia, todas las ventas pasadas pasan a contabilizarse en la familia nueva, y el informe del año pasado deja de coincidir consigo mismo.

### Punto 1.8 — Implementación del Datawarehouse

**Sesiones 3 y 4.**

La sesión 2 aborda el soporte físico: tipos de datos informáticos, cuánto ocupa cada uno en bytes, y por qué elegir bien el formato de almacenamiento cambia radicalmente el rendimiento de un sistema analítico. Ahí entra la distinción **fila frente a columna**, que es la que explica por qué un almacén analítico consulta rápido y la que ha dejado obsoletos a los servidores MOLAP.

Las sesiones 3 y 4 abordan el modelo lógico: esquema en **estrella** y en **copo de nieve**.

### Punto 1.9 — Análisis OLAP: Drill Down, Drill Up

**Sesiones 5 y 6**, en vivo sobre el cuadro de mando.

| Operación | Qué hace |
|---|---|
| **Drill-down** | Baja un nivel en una jerarquía: de región a provincia, de provincia a tienda |
| **Drill-up** (*roll-up*) | Sube un nivel; agrega el detalle |
| **Slice** | Fija una dimensión y se queda con la rebanada: solo abril |
| **Dice** | Filtra por varias dimensiones a la vez |
| **Pivot** | Gira el cubo: intercambia filas y columnas |

El *drill-down* es **la operación diagnóstica por excelencia**: el total dice que la región ha caído, y al bajar un nivel resulta que no ha caído la región, ha caído una tienda. Por eso la navegación en profundidad no es una comodidad del cuadro de mando: es un requisito de fiabilidad.

### Punto 1.10 — Servidores OLAP y definiciones de Data Mining

La parte de **servidores OLAP** se trata en la sesión 2, ligada al almacenamiento columnar. Ver el [apartado 2](#2-conceptos-del-temario-que-hoy-hay-que-matizar).

La parte de **minería de datos** se trata en la sesión 7. La diferencia con todo lo anterior está en el punto de partida: en estadística descriptiva y en reporting, la pregunta la formula una persona y los datos responden; en minería de datos, es el algoritmo el que propone la pregunta, buscando estructura que nadie había pedido.

### Punto 1.11 — Categorías de Data Mining

**Sesión 7.** Cada técnica se presenta por el problema de negocio que resuelve, no por su formulación:

| Categoría | Qué hace | Pregunta de negocio |
|---|---|---|
| Clasificación | Asigna a una categoría conocida | ¿Este cliente se va a dar de baja? |
| Regresión | Estima un número | ¿Cuántas unidades venderemos? |
| Agrupamiento | Descubre grupos naturales | ¿Qué tipos de cliente existen realmente? |
| Reglas de asociación | Qué ocurre junto | ¿Qué se compra con qué? |
| Detección de anomalías | Identifica lo que se sale del patrón | ¿Hay operaciones raras? |
| Análisis de secuencias | Qué ocurre **después** de qué | ¿Qué compra tras la primera compra? |

Las tres primeras se dividen además en **supervisadas** (clasificación y regresión: hacen falta ejemplos ya etiquetados) y **no supervisadas** (agrupamiento: no hay respuesta conocida de antemano). El valor de las no supervisadas es que pueden **contradecir** la segmentación que la empresa creía tener.

### Punto 1.12 — Proceso de Minería de Datos

**Sesión 7.** Selección → preparación → transformación → modelado → evaluación → despliegue.

⚠️ Las tres primeras fases se llevan entre el 60 % y el 80 % del tiempo total. La de modelado, que es la única que la gente asocia con la minería de datos, suele ser la más corta.

### Punto 1.13 — Metodología

**Sesiones 7 y 8.** CRISP-DM, con sus seis fases cíclicas. Dos rasgos la hacen útil:

- **Empieza por el negocio, no por los datos.** La primera fase no consiste en mirar tablas, sino en averiguar qué decisión hay que mejorar y cómo se sabrá si el proyecto ha servido para algo.
- **Los bucles son la norma.** De modelado se vuelve a preparación constantemente, y de evaluación se vuelve al negocio cuando resulta que el problema estaba mal planteado.

También se trata el **sobreajuste** y por qué un modelo con un 99 % de acierto es casi siempre una mala noticia: o se ha aprendido los datos de memoria, o alguna variable de entrada contiene implícitamente la respuesta, o las clases están tan desequilibradas que predecir siempre lo mismo produce ese acierto.

### Puntos 1.14 y 3.3 — Reportes

**Sesiones 5 y 6.** Documentos estructurados, con periodicidad fija y destinatarios definidos. Siguen siendo la respuesta correcta cuando hay que dejar constancia, cuando el destinatario no va a entrar en ninguna herramienta o cuando el contenido es regulatorio. Se distingue entre informes **operativos** y **analíticos**.

### Punto 1.15 — Consultas

**Sesión 6.** Tres modelos, con sus consecuencias:

- **Ad hoc por el equipo de BI**: control total, cuello de botella garantizado.
- **Autoservicio libre**: rapidez y caos; cada usuario construye su versión de la verdad.
- **Autoservicio gobernado**: los usuarios construyen sus análisis, pero sobre un modelo semántico común con las métricas ya definidas. Es la opción recomendable.

### Punto 1.16 — Alertas

**Sesión 6.** Cuatro requisitos para que una alerta funcione: umbral **justificado** (un umbral estadístico envejece mucho mejor que un número fijo), destinatario con capacidad de actuar, **acción prevista** —una alerta sin acción asociada es ruido— y volumen controlado.

La *fatiga de alertas* es real: un sistema que avisa veinte veces al día deja de leerse en una semana, y entonces también se pierde la que importaba.

### Punto 1.17 — Análisis

**Sesiones 1 y 2.** Es el bloque más extenso del curso y el que sostiene todo lo demás.

**Sesión 1 · Análisis univariante.**

- **Tipos de datos desde el punto de vista estadístico**, que no es el informático: cualitativos (nominales y ordinales) y cuantitativos (de razón y de intervalo). La escalera `CUANTITATIVO → ORDINAL → NOMINAL`, y la regla de que siempre se puede bajar de escala y nunca subir.
- **Frecuencias**: absoluta y relativa, tabla de frecuencias, diagrama de barras y de sectores. Y cuándo la tabla de frecuencias **no** ayuda: con variables cuantitativas de muchos valores distintos hay que agrupar en intervalos, que es exactamente bajar la variable a una escala ordinal.
- **Tendencia central**: moda, mediana y media, con la escala de medida que admite cada una. El ejemplo de los dos pueblos —donde nueve vecinos con coche eléctrico y Manolo con su tractor dan una media de 104,5 g de CO que no describe a nadie— fija por qué la media puede mentir y cuándo hay que preferir la mediana.
- **Dispersión**: seis casas con la misma media y la misma mediana pero habitantes completamente distintos. De ahí, varianza, el problema de las unidades al cuadrado, y desviación típica. La interpretación se apoya en la **regla de Chebyshev**: multiplicando la desviación típica por la raíz de 2 y sumándola y restándola de la media se obtiene un intervalo que contiene **al menos al 50 %** de los individuos, sea cual sea la distribución.
- **Posición**: cuartiles, deciles y percentiles. El caso de los tiempos de conexión a una base de datos muestra por qué en muchos escenarios no interesan ni la media ni la mediana ni el máximo, sino el **P95 o el P99**: lo que hay que garantizar es que la inmensa mayoría trabaje bien.

> **Regla que no se negocia:** nunca se ofrece un indicador de tendencia central sin su indicador de dispersión asociado. Si se usa la media, la desviación típica. Si se usa la mediana, el rango intercuartílico.

**Sesión 2 · Análisis bivariante.** Relaciones entre dos variables, y el puente hacia la minería de datos: correlación, correlación frente a causalidad y variables de confusión.

### Punto 1.18 — Pronósticos

**Sesión 7.** Describir el pasado y anticipar el futuro son dos oficios distintos.

- Componentes de una serie temporal: **tendencia, estacionalidad, ciclo y ruido**.
- Por qué comparar con el mes anterior casi nunca informa en un negocio estacional, y qué comparaciones sí: interanual, acumulado y media móvil.
- Distinguir **señal de ruido**: antes de explicar por qué ha bajado un indicador, hay que demostrar que ha bajado.
- Modelos sencillos, del ingenuo al suavizado exponencial, con la regla de que **cualquier modelo que no supere al ingenuo no aporta nada**.
- Y la incertidumbre: un pronóstico sin margen de error no es un pronóstico, es una opinión con decimales.

### Puntos 2.1, 2.2 y 2.3 — Gestión de proyectos, planificación y riesgos

**Sesión 8**, junto con la calidad del dato, que es lo que alimenta el registro de riesgos.

Un proyecto de BI no es un proyecto de desarrollo clásico:

- **El alcance se descubre trabajando.** Nadie sabe qué contiene realmente una fuente hasta que la abre.
- **La mayor parte del esfuerzo está en los datos**, no en la herramienta. Construir el cuadro de mando es la parte rápida.
- **El valor no está en el entregable, está en el uso.** Un cuadro terminado que nadie abre es un proyecto fracasado, aunque cumpliera plazo y presupuesto.

El reparto realista del esfuerzo, que casi ninguna planificación inicial contempla:

```
Análisis, definición y acuerdo de KPIs    20 %
Perfilado y calidad de los datos          25 %
Desarrollo de los procesos de carga       30 %
Modelo y cuadros de mando                 15 %
Validación, formación y ajustes           10 %
```

Y los riesgos típicos, que en su mayoría salen del bloque de calidad del dato de esa misma sesión: mala calidad, definiciones divergentes del mismo indicador, fuentes incompatibles, datos incompletos, sesgos de recogida, falta de contexto, indicadores mal diseñados, confundir correlación con causalidad, automatizar una conclusión incorrecta y el clásico cuadro de mando que nadie usa.

### Puntos 3.1 y 3.7 — Procesos de Extracción, Transformación y Carga

**Sesión 1** (concepto, dentro del glosario) y **sesión 4** (desarrollo).

Una ETL es un programa que **extrae** datos de una fuente, los **transforma** y los **carga** en otro destino. Es lo que mueve la información entre las bases de datos operacionales, el data lake y el almacén.

La fase larga es la transformación: limpieza, normalización de formatos, conversión de unidades, deduplicación, conformado de códigos y aplicación de las reglas de negocio.

> Las reglas de negocio no las decide quien programa la carga. Las decide el negocio, y quedan documentadas. Es la única forma de que dos informes no den dos cifras distintas de la misma cosa.

### Punto 3.2 — El almacén de datos

**Sesiones 1 y 3.** La cadena completa del dato:

```
BBDD              DATA LAKE                  DATA WAREHOUSE
Datos vivos       Histórico en bruto         Histórico cocinado para un objetivo (BI)
OLTP              Cajón de sastre            OLAP
```

### Punto 3.4 — Dashboards

**Sesiones 5 y 6**, en formato de ejercicio completo: construir un cuadro de mando partiendo de datos ya cocinados.

Las preguntas que se responden **antes** de abrir la herramienta: quién lo va a mirar, con qué frecuencia, qué decisión permite tomar, qué hace esa persona si el indicador se pone en rojo y qué es lo primero que tiene que ver al abrirlo.

Y los criterios de construcción: resumen arriba y detalle abajo, estado codificado en la forma y no solo en el número, navegación en profundidad, y toda definición de indicador accesible desde el propio cuadro.

Se trabaja también la **visualización honesta**: elección del gráfico según la escala de medida, ejes truncados, dobles ejes, sectores con demasiadas categorías y exceso de color.

> Un cuadro de mando visualmente atractivo y conceptualmente falso es más peligroso que una hoja de cálculo fea: se cree más y se cuestiona menos.

### Punto 3.5 — Herramientas de visualización y consulta: OLAP

**Sesiones 5 y 6.** Ver el punto 1.9.

### Punto 3.6 — Herramientas de visualización y consulta: Data Mining

**Sesión 7.** Ver los puntos 1.10 a 1.13.

### Punto 3.8 — Creación de cubos multidimensionales

**Sesión 4** (diseño) y **sesión 6** (explotación).

El modelo dimensional organiza los datos en dos tipos de tabla: los **hechos**, que registran lo que ocurre y contienen las métricas, y las **dimensiones**, que aportan el contexto descriptivo.

Y aquí enlaza directamente con la estadística de la sesión 1:

> Las **dimensiones** son las variables medidas en escala **nominal y ordinal**. Los **hechos** son las variables medidas en escala **cuantitativa**.

Quien sepa determinar la escala de medida de cada columna tiene ya medio modelo diseñado. Un código postal es texto disfrazado de número: dimensión. Un importe tiene unidad de medida: hecho.

La decisión más importante del diseño es el **grano**: qué representa una fila de la tabla de hechos. Se elige siempre el más fino que sea viable, porque desde el detalle se puede agregar en cualquier momento y desde el agregado no se puede desagregar nunca.

---

## 5. Índice de sesiones

| # | Sesión | Contenido | Notas |
|---|---|---|---|
| **1** | Estadística univariante y glosario del dato | Tres partes del curso · tener datos ≠ entenderlos · tipos de datos estadísticos · frecuencias · tendencia central · dispersión · posición · BBDD, OLTP, data lake, DWH, OLAP, ETL | `dia1.md` |
| **2** | Vocabulario del dato y soporte informático | BI, minería, aprendizaje automático y Big Data · tipos de datos informáticos y tamaño en bytes · codificaciones y UTF-8 · fila frente a columna · normalización y desnormalización · calidad del dato y valores perdidos | `dia2.md` |
| **3** | Análisis bivariante y modelo dimensional | Correlación · correlación frente a causalidad · variables de confusión · hechos y dimensiones · esquema en estrella y en copo de nieve | `dia3.md` |
| **4** | Modelo dimensional y ETL | Granularidad · jerarquías · la dimensión Fecha · aditividad de las métricas · procesos ETL y ELT | `dia4.md` |
| **5** | Cuadro de mando (I) | Pirámide organizacional · de los datos cocinados al primer cuadro · KPIs y contexto · reportes | `dia5.md` |
| **6** | Cuadro de mando (II) | Navegación OLAP · consultas y autoservicio · alertas · visualización honesta | `dia6.md` |
| **7** | Analizar y predecir | Minería de datos: categorías, proceso y CRISP-DM · series temporales y pronósticos | `dia7.md` |
| **8** | Calidad y proyecto | Dimensiones de la calidad del dato · gestión de un proyecto de BI · planificación · riesgos · caso integrador | `dia8.md` |

---

## 6. Trazabilidad completa del temario oficial

Los 29 epígrafes del programa ADGG102PO y la sesión en la que se imparte cada uno.

### Bloque 1 · Inteligencia de negocios

| Epígrafe | Denominación oficial | Sesión |
|---|---|---|
| 1.1 | Introducción | 1 |
| 1.2 | La pirámide organizacional | 5 |
| 1.3 | Herramientas de inteligencia de negocios | 1 · 2 · 5 · 6 |
| 1.4 | Fundamentos del Datawarehouse | 1 · 2 |
| 1.5 | Características | 2 |
| 1.6 | Ventajas | 2 |
| 1.7 | Sistemas OLTP | 1 |
| 1.8 | Implementación del Datawarehouse | 2 · 3 · 4 |
| 1.9 | Análisis OLAP: Drill Down, Drill Up | 5 · 6 |
| 1.10 | Servidores OLAP: ROLAP, MOLAP, HOLAP · Minería de datos, definiciones | 2 · 7 |
| 1.11 | Categorías de Data Mining | 7 |
| 1.12 | Proceso de Minería de Datos | 7 |
| 1.13 | Metodología | 7 · 8 |
| 1.14 | Reportes | 5 · 6 |
| 1.15 | Consultas | 6 |
| 1.16 | Alertas | 6 |
| 1.17 | Análisis | 1 · 3 |
| 1.18 | Pronósticos | 7 |

### Bloque 2 · La gestión de proyectos de Business Intelligence

| Epígrafe | Denominación oficial | Sesión |
|---|---|---|
| 2.1 | Gestión de proyectos | 8 |
| 2.2 | Planificación del proyecto | 8 |
| 2.3 | Riesgos | 8 |

### Bloque 3 · Arquitectura de un proyecto de Business Intelligence

| Epígrafe | Denominación oficial | Sesión |
|---|---|---|
| 3.1 | Procesos de Extracción, Transformación y Carga | 1 · 2 · 4 |
| 3.2 | El almacén de datos | 1 · 2 |
| 3.3 | Herramientas de visualización y consulta: Reportes | 5 · 6 |
| 3.4 | Herramientas de visualización y consulta: Dashboards | 5 · 6 |
| 3.5 | Herramientas de visualización y consulta: OLAP | 5 · 6 |
| 3.6 | Herramientas de visualización y consulta: Data Mining | 7 |
| 3.7 | Procesos ETL | 4 |
| 3.8 | Creación de cubos multidimensionales | 4 · 6 |

### Contenidos impartidos que amplían el temario

Se incluyen porque, sin ellos, buena parte del temario oficial se aplica mal:

| Contenido | Sesión | Por qué se añade |
|---|---|---|
| Escalas de medida | 1 | Determinan qué es dimensión, qué es hecho y qué técnica se puede aplicar |
| Dispersión y medidas de posición | 1 | Sin ellas, cualquier indicador de tendencia central es engañoso |
| Análisis bivariante y causalidad | 3 | El puente entre «análisis» (1.17) y la minería de datos |
| Almacenamiento fila frente a columna | 2 | Es lo que explica ROLAP, MOLAP y HOLAP en términos actuales |
| Calidad del dato y valores perdidos | 2 · 8 | Es el 25 % del esfuerzo real de cualquier proyecto y el origen de la mayoría de los riesgos |

---
