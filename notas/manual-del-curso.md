# Business Intelligence: del dato a la decisión

**Manual del curso ADGG102PO · Business Intelligence**

---

## Sobre este manual

Este documento recoge, ordena y desarrolla el contenido de las ocho sesiones del
curso de Business Intelligence. No es una transcripción de las clases: es el
material de referencia que queda cuando el curso termina.

Todo lo que se explica aquí está construido sobre un mismo caso —una cadena de
tiendas de bicicletas llamada **Rueda Libre**— que a lo largo del curso pasa de
tener seis fuentes de datos incompatibles y un informe mensual en hoja de cálculo
a disponer de un modelo analítico, un cuadro de mando y un pronóstico de demanda.
Los problemas que aparecen en el caso no son inventados: son los que aparecen en
todos los proyectos de BI, siempre en el mismo orden.

> **La idea que sostiene todo el curso**
>
> Business Intelligence no consiste en hacer gráficos. Consiste en transformar
> datos en evidencia para tomar decisiones. Un gráfico bonito construido sobre un
> indicador mal definido no es inteligencia de negocio: es una forma más rápida
> de equivocarse.

Si al terminar de leer este manual sólo se retiene una idea, que sea ésta: **tener
datos no es lo mismo que entender los datos**. Todas las técnicas que se explican
en las páginas siguientes —las escalas de medida, el almacén, el modelo
dimensional, los indicadores, los pronósticos— existen para cerrar esa distancia.

---

## Cómo está organizado

El manual tiene dos partes y cinco apéndices.

**La Parte I explica los fundamentos**, organizados por materia. Cada capítulo se
puede leer por separado y sirve para consultar un concepto concreto meses después:
qué diferencia hay entre una escala ordinal y una cuantitativa, por qué un
porcentaje no se puede promediar, qué es exactamente el grano de una tabla de
hechos.

**La Parte II reconstruye el caso Rueda Libre paso a paso**, desde el primer
informe hasta el pronóstico. Aquí no se explican conceptos nuevos: se muestra cómo
se aplicaron, en qué orden, qué problema resolvía cada decisión y qué se rompió
por el camino. Es la parte que da sentido a la primera.

**Los apéndices** contienen el mapa del temario oficial, un glosario, una
referencia rápida de indicadores y su interpretación, las soluciones de los
ejercicios y una lista de errores estadísticos que circulan como verdades.

Cada capítulo termina con **ejercicios propuestos** sobre el propio caso. Las
soluciones comentadas están en el Apéndice D. Hacerlos es la diferencia entre
haber leído el manual y haber aprendido algo.

---

## Convenciones tipográficas

A lo largo del texto se usan tres marcas:

> Las citas enmarcadas recogen las ideas centrales del curso, tal y como se
> formularon en clase.

Los bloques monoespaciados muestran datos, fórmulas o resultados reales del caso.
Cuando se muestra una versión simplificada para explicar un concepto, se indica
expresamente.

⚠️ Los avisos señalan errores frecuentes, trampas conocidas y puntos donde la
teoría y la práctica no coinciden.

---

## Los dos personajes

Dos figuras recorren el manual de principio a fin. No son un adorno: cada una
representa un problema estadístico concreto que reaparece en cada capítulo.

**Manolo** lleva el taller de la tienda central de Rueda Libre. Repara bicicletas
de carretera, pero también acepta trabajos que nadie más acepta: restauraciones
completas que le llevan tres semanas. Manolo es el **valor atípico**. Infla la
media del tiempo de reparación, descuadra el margen del taller, rompe la carga
nocturna cuando teclea el modelo de bicicleta a mano y arruina el pronóstico de
demanda cada vez que le entra un encargo grande.

**Gertru** es la responsable de datos de la cadena. Es el contrapeso: el caso
extremo por el lado contrario, la que pregunta de dónde sale cada cifra y la que
recuerda —cada vez que alguien quiere borrar a Manolo del conjunto de datos— que
un valor atípico no es necesariamente un error.

> Todo el curso se puede resumir en una pregunta: ¿este dato es un Manolo, o es la
> realidad?

---

## El caso: Rueda Libre

**Rueda Libre** es una cadena de doce tiendas de bicicletas repartidas por España.
Vende bicicletas, componentes y accesorios, y cada tienda tiene un taller de
reparación. Desde hace tres años tiene además una tienda en línea.

Sus sistemas, tal y como estaban al empezar el curso:

```
TPV de tienda        Un terminal por tienda. Ventas, líneas de ticket, forma de pago.
Gestor de taller     Órdenes de reparación: entrada, salida, piezas, mano de obra.
Tienda en línea      Pedidos web, con su propio catálogo y sus propios códigos.
Fichero de clientes  Alta manual en cada tienda. Sin control de duplicados.
ERP de compras       Proveedores, stock, precio de coste.
Objetivos.xlsx       Objetivos mensuales por tienda. Lo mantiene una persona a mano.
```

Seis fuentes, cuatro definiciones distintas de «venta», tres catálogos de producto
que no casan y un cliente que puede estar dado de alta cinco veces. Es un punto de
partida absolutamente normal.

La pregunta que abre el proyecto la formula la dirección en la primera reunión:

> «¿Por qué la tienda de Valencia vende más que la de Zaragoza si son del mismo
> tamaño?»

Responderla bien ocupa las ocho sesiones del curso.

---

## Índice

### Parte I · Fundamentos

1. **[Qué es Business Intelligence](#1-qué-es-business-intelligence)**
   Dato frente a información · La pirámide organizacional · Los cuatro niveles de
   analítica · Las familias de herramientas · El recorrido completo del dato a la
   decisión

2. **[Escalas de medida: cómo está medido el dato](#2-escalas-de-medida-cómo-está-medido-el-dato)**
   Cualitativo y cuantitativo · Escala nominal · Escala ordinal · Escala
   cuantitativa · Discreta y continua · De razón y de intervalo · La escalera de
   medida · Por qué esto decide todo lo demás

3. **[La calidad del dato](#3-la-calidad-del-dato)**
   Las seis dimensiones de la calidad · Valores nulos · Duplicados ·
   Codificaciones inconsistentes · Unidades y divisas · Fechas · Granularidades
   distintas · El coste de no limpiar

4. **[Resumir: frecuencias y tendencia central](#4-resumir-frecuencias-y-tendencia-central)**
   Frecuencia absoluta, relativa y acumulada · Tablas de frecuencias · Agrupación
   en intervalos · Moda · Mediana · Media · Cuándo la media engaña · Robusto y
   suficiente

5. **[Dispersión, posición y forma](#5-dispersión-posición-y-forma)**
   Por qué una media sola no dice nada · Rango · Varianza · Desviación típica ·
   Cómo se interpreta · Cuartiles, deciles y percentiles · Rango intercuartílico ·
   Simetría y asimetría · Valores atípicos · El diagrama de caja y bigotes

6. **[Sistemas operacionales y sistemas analíticos](#6-sistemas-operacionales-y-sistemas-analíticos)**
   Sistemas OLTP · Normalización · OLTP frente a OLAP · El almacén de datos ·
   Características y ventajas · Datamarts · Enfoques descendente y ascendente ·
   Capas e historificación

7. **[Procesos ETL](#7-procesos-etl)**
   Extracción · Transformación · Carga · Carga completa e incremental · Captura de
   cambios · Conformado de dimensiones · ETL frente a ELT · Orquestación · Gobierno
   del dato

8. **[El modelo dimensional](#8-el-modelo-dimensional)**
   Hechos y dimensiones · La granularidad · Esquema en estrella y en copo de nieve ·
   Jerarquías · La dimensión Fecha · Métricas aditivas, semiaditivas y no aditivas ·
   La media ponderada · Cubos · Drill-down y drill-up · ROLAP, MOLAP y HOLAP

9. **[Analizar: relaciones y segmentación](#9-analizar-relaciones-y-segmentación)**
   Análisis exploratorio · Segmentación · Comparar poblaciones equivalentes · La
   paradoja de Simpson · Correlación · Correlación y causalidad · Variables de
   confusión

10. **[Minería de datos](#10-minería-de-datos)**
    Qué es y qué no es · Clasificación · Regresión · Agrupamiento · Reglas de
    asociación · Detección de anomalías · El proceso de minería · CRISP-DM ·
    Sobreajuste y validación

11. **[Indicadores, informes y cuadros de mando](#11-indicadores-informes-y-cuadros-de-mando)**
    Métrica y KPI · La definición operativa · Absolutos y relativos · Ratios y
    tasas · La media de medias · El contexto · Informes · Consultas · Alertas ·
    Diseño de un cuadro de mando · Visualización honesta

12. **[Series temporales y pronósticos](#12-series-temporales-y-pronósticos)**
    Descripción frente a predicción · Tendencia, estacionalidad, ciclo y ruido ·
    Comparaciones entre períodos · Medias móviles · Modelos sencillos de pronóstico ·
    Incertidumbre · Tamaño de muestra · Riesgos del pronóstico automatizado

13. **[La gestión de un proyecto de BI](#13-la-gestión-de-un-proyecto-de-bi)**
    Por qué no es un proyecto de desarrollo · Las fases · Roles · Planificación ·
    Los diez riesgos típicos · Validación y evolución

### Parte II · El caso Rueda Libre, paso a paso

14. **[Versión 1 — El informe mensual en hoja de cálculo](#14-versión-1--el-informe-mensual-en-hoja-de-cálculo)**
15. **[Versión 2 — Unificar las fuentes](#15-versión-2--unificar-las-fuentes)**
16. **[Versión 3 — El modelo dimensional](#16-versión-3--el-modelo-dimensional)**
17. **[Versión 4 — El cuadro de mando](#17-versión-4--el-cuadro-de-mando)**
18. **[Versión 5 — Del descriptivo al predictivo](#18-versión-5--del-descriptivo-al-predictivo)**
19. **[Balance de la evolución](#19-balance-de-la-evolución)**

### Apéndices

- **A.** [El temario oficial, punto por punto](#apéndice-a--el-temario-oficial-punto-por-punto)
- **B.** [Glosario de términos](#apéndice-b--glosario-de-términos)
- **C.** [Referencia rápida de indicadores](#apéndice-c--referencia-rápida-de-indicadores)
- **D.** [Soluciones de los ejercicios](#apéndice-d--soluciones-de-los-ejercicios)
- **E.** [Errores que circulan como verdades](#apéndice-e--errores-que-circulan-como-verdades)

---
# Parte I · Fundamentos

---

## 1. Qué es Business Intelligence

Antes de montar nada conviene entender para qué sirve lo que se va a montar. La
mayor parte de los proyectos de BI que fracasan no fracasan por la tecnología:
fracasan porque nadie se detuvo a preguntar qué decisión iba a mejorar el
resultado.

### 1.1. Tener datos no es entenderlos

Imagínese que alguien quiere saber cómo se mueven los salarios en una empresa de
cinco mil empleados y le entregan las cinco mil nóminas. Un paquete de quinientas
hojas ocupa unos cinco centímetros: son cincuenta centímetros de papel. Medio
metro de datos.

Ahí está toda la verdad. No hay más información disponible sobre la cuestión: esa
es la fuente. Y sin embargo, alguien podría pasarse una semana leyendo esas cinco
mil nóminas y, al terminar, seguiría sin saber cómo se mueven los salarios en esa
empresa. Recordaría anecdóticamente el más alto y el más bajo. Nada más.

> Tener datos y entender los datos son dos cosas distintas. Toda la estadística
> descriptiva —y por extensión todo el Business Intelligence— existe para cubrir
> la distancia entre una y otra.

El problema no es de acceso al dato. Es de capacidad de proceso: un cerebro humano
no puede sostener cinco mil valores a la vez. Lo que hace falta es un mecanismo
para **resumir** que no destruya lo esencial por el camino. Y ahí está la
dificultad: todo resumen pierde información, y la habilidad consiste en perder la
que no importa.

### 1.2. Dato e información no son lo mismo

Un dato es un valor registrado. La información es lo que ese valor permite
concluir, y casi siempre es mucho más de lo que el dato dice literalmente.

Supóngase que se conoce el peso de una persona de altura promedio: 140 kg. El dato
son los 140 kg. La información que transporta es otra cosa:

- Con altísima probabilidad, la persona tiene más de veinte años.
- Es probable que tenga problemas de salud asociados al sobrepeso.
- Es probable que su alimentación no sea equilibrada.
- Es probable que no haga ejercicio con regularidad.
- Su talla de ropa es 3XL o superior.

Ninguna de esas conclusiones está escrita en el dato. Todas se derivan de él. Y
todas pueden ser falsas: la persona podría ser un culturista con una masa muscular
enorme. Con más datos —porcentaje de grasa corporal, nivel de actividad física— las
estimaciones mejorarían.

Esa es exactamente la mecánica de un proyecto de BI: se recogen datos concretos
sobre un fenómeno para poder afirmar cosas que los datos no dicen literalmente.

⚠️ El riesgo también es el mismo: cuanto más lejos se llega en la inferencia, más
fácil es equivocarse. Un cuadro de mando que muestra datos es inofensivo. Uno que
sugiere conclusiones sin decir con qué confianza, no.

### 1.3. Qué es y qué no es Business Intelligence

Business Intelligence es el conjunto de procesos, arquitecturas y técnicas que
convierten datos en bruto en información útil para la toma de decisiones de
negocio.

Conviene delimitarlo por contraste:

| No es | Es |
|---|---|
| Hacer gráficos bonitos | Definir qué se mide y por qué |
| Sacar un listado de un sistema | Integrar varias fuentes en una versión única |
| Una herramienta concreta | Una capa entre los sistemas operativos y las personas |
| Un proyecto que termina | Un servicio que se mantiene y evoluciona |
| Analítica avanzada | La base sobre la que la analítica avanzada se sostiene |

La confusión más frecuente es la primera. Una organización puede tener veinte
cuadros de mando y ninguna inteligencia de negocio, si esos cuadros no responden a
preguntas que alguien se esté haciendo de verdad.

### 1.4. La pirámide organizacional

Las decisiones no se toman igual en todos los niveles de una organización, y por
tanto no necesitan la misma información.

```
                    ┌─────────────┐
                    │ ESTRATÉGICO │   Dirección
                    └─────────────┘   Horizonte: años
                  ┌─────────────────┐
                  │     TÁCTICO     │   Mandos intermedios
                  └─────────────────┘   Horizonte: meses
              ┌─────────────────────────┐
              │       OPERATIVO         │   Personal de tienda y taller
              └─────────────────────────┘   Horizonte: hoy
```

| Nivel | Pregunta típica | Granularidad | Frecuencia | Formato |
|---|---|---|---|---|
| Operativo | ¿Qué reparaciones tengo hoy sin cerrar? | Máximo detalle: el registro individual | Continua | Listado, alerta |
| Táctico | ¿Cómo va mi tienda respecto al objetivo del trimestre? | Agregado por tienda y mes | Semanal o mensual | Cuadro de mando |
| Estratégico | ¿Debemos abrir tienda en Bilbao? | Muy agregado, con series largas | Trimestral o anual | Informe con escenarios |

Este cuadro tiene una consecuencia práctica inmediata: **la misma cifra no sirve
para los tres niveles**. Un director general al que se le entrega el detalle de
cada ticket no puede decidir nada, y un encargado de tienda al que se le entrega
únicamente el acumulado anual tampoco.

⚠️ El error más frecuente en cuadros de mando corporativos es diseñar uno solo y
repartirlo hacia arriba y hacia abajo. Termina siendo demasiado agregado para
quien opera y demasiado detallado para quien dirige.

### 1.5. Los cuatro niveles de analítica

Es el mapa que ordena todo el curso, y también la escalera por la que se sube.

```
DESCRIPTIVA      ¿Qué ha pasado?           Informes, cuadros de mando, KPIs
      ↓
DIAGNÓSTICA      ¿Por qué ha pasado?       Segmentación, correlación, drill-down
      ↓
PREDICTIVA       ¿Qué puede pasar?         Series temporales, modelos, pronósticos
      ↓
PRESCRIPTIVA     ¿Qué debería hacer?       Optimización, simulación, reglas
```

Tres observaciones que ahorran disgustos:

1. **No se puede saltar escalones.** Una organización que no sabe cuánto vendió el
   mes pasado —porque cada sistema da una cifra distinta— no está en condiciones de
   predecir cuánto venderá el mes que viene.
2. **El valor sube y la fiabilidad baja.** Cuanto más arriba, más útil es la
   respuesta y más incierta.
3. **La mayor parte del retorno está en los dos primeros escalones.** Buena parte
   de los proyectos que se venden como predictivos habrían dado más valor
   arreglando el descriptivo.

### 1.6. Las familias de herramientas

El mercado de BI cambia cada pocos años; las categorías no. Conviene conocer las
familias y no las marcas, porque las marcas de dentro de cinco años serán otras.

| Familia | Qué hace | Ejemplos actuales |
|---|---|---|
| Integración | Extraer, transformar y cargar datos | Talend, Pentaho, Azure Data Factory, dbt |
| Almacenamiento analítico | Guardar el histórico integrado | Snowflake, BigQuery, Redshift, SQL Server |
| Modelado semántico | Definir métricas y jerarquías una sola vez | Analysis Services, modelos tabulares, dbt metrics |
| Visualización | Informes y cuadros de mando | Power BI, Tableau, Looker, Qlik |
| Analítica avanzada | Modelos estadísticos y de aprendizaje | Python, R, servicios gestionados |
| Gobierno | Catálogo, linaje, calidad, permisos | Purview, Collibra, Amundsen |

> La herramienta es la última decisión de un proyecto de BI, no la primera. Cuando
> una organización empieza eligiendo herramienta, casi siempre acaba adaptando sus
> preguntas a lo que la herramienta sabe responder.

### 1.7. El recorrido completo

Este esquema aparece en todas las sesiones del curso. Cada capítulo del manual
desarrolla uno o dos de sus eslabones.

```
Pregunta de negocio        ¿Qué decisión hay que tomar?          cap. 1, 13
        ↓
Datos                      ¿Qué fuentes existen?                 cap. 6, 7
        ↓
Calidad del dato           ¿Se puede confiar en ellas?           cap. 3
        ↓
Análisis estadístico       ¿Qué dicen realmente?                 cap. 4, 5, 9
        ↓
Modelo / indicador         ¿Cómo se resume sin mentir?           cap. 8, 11
        ↓
Visualización              ¿Cómo se muestra?                     cap. 11
        ↓
Interpretación             ¿Qué se puede concluir?               cap. 5, 9, 12
        ↓
Decisión                   ¿Qué se hace distinto mañana?         cap. 13
```

La versión reducida que circula en muchas organizaciones —*datos → ETL → almacén →
cuadro de mando*— se salta tres eslabones: la pregunta, la calidad y la
interpretación. Son precisamente los tres que determinan si el cuadro de mando
dice la verdad.

### 1.8. Las nueve preguntas

Se presentan aquí y se aplican en todos los ejercicios del manual. Son la
herramienta de trabajo más útil del curso, y no requieren ninguna tecnología.

1. ¿Qué estoy midiendo exactamente?
2. ¿De dónde viene el dato?
3. ¿Es comparable con aquello con lo que lo estoy comparando?
4. ¿Qué distribución tiene?
5. ¿Es representativa la media?
6. ¿Hay valores extremos y qué significan?
7. ¿Estoy comparando poblaciones equivalentes?
8. ¿Existe realmente una tendencia?
9. ¿Qué decisión tomaría con este dato?

### Ejercicios del capítulo 1

**1.1.** La dirección de Rueda Libre pregunta: «¿Por qué la tienda de Valencia
vende más que la de Zaragoza si son del mismo tamaño?» Reformula la pregunta en
términos que se puedan medir. ¿Cuántas preguntas distintas hay realmente escondidas
dentro?

**1.2.** Clasifica cada una de estas peticiones según el nivel de la pirámide al
que pertenece, y di qué granularidad de dato necesita cada una:
(a) el listado de reparaciones pendientes de recoger;
(b) el margen por familia de producto y trimestre;
(c) la evolución de la cuota de mercado en los últimos cinco años.

**1.3.** Una consultora propone a Rueda Libre un proyecto de predicción de demanda
con aprendizaje automático. Al preguntar, se descubre que el TPV y la tienda en
línea dan cifras de venta distintas para el mismo día y nadie sabe cuál es la
buena. ¿Qué recomendarías y por qué? Sitúa la respuesta en la escalera de los
cuatro niveles de analítica.

**1.4.** Toma un indicador que veas en tu trabajo. Aplícale las nueve preguntas.
¿Cuántas puedes responder sin consultar a nadie?

---

## 2. Escalas de medida: cómo está medido el dato

Este capítulo parece elemental y no lo es. Todas las técnicas estadísticas, todos
los gráficos y buena parte de las decisiones de modelado dependen de la escala en
la que está medido el dato. Saltárselo es la causa número uno de cuadros de mando
conceptualmente falsos.

### 2.1. La distinción fundamental

La estadística distingue entre datos **cualitativos** y **cuantitativos**. Más
exactamente: entre datos *medidos de forma* cualitativa y *medidos de forma*
cuantitativa, porque lo que se clasifica no es el dato sino la manera de
representarlo.

La diferencia está en una sola cosa: **si el dato representa una cantidad o no**.

Y aquí llega el punto que casi nadie tiene claro. *Cuantitativo* viene de
*cantidad*, y **un número no es una cantidad**. Un número de teléfono está
compuesto de cifras y no es una cantidad. Un código postal tampoco. Un número de
cliente tampoco. Un código de artículo tampoco.

> Para que algo sea una cantidad tiene que tener una **unidad de medida** asociada,
> que diga qué se está contando.

140 kg es una cantidad: la unidad es el kilogramo. El código postal 28850 no lo es:
no hay unidad. Y esto no es una sutileza académica; tiene una consecuencia
inmediata en cualquier herramienta de BI, que verá una columna de números y ofrecerá
sumarla.

⚠️ Casi todas las herramientas de BI clasifican los datos por su **naturaleza
técnica** —texto, número, fecha— y no por su escala de medida. Es responsabilidad
de quien modela decidir cuáles de esos números son cantidades. Una herramienta que
suma códigos postales no está fallando: está haciendo lo que se le ha pedido.

Las unidades no tienen que ser del Sistema Internacional. Son perfectamente válidas:

```
Número de hijos          unidad: hijos
Número de visitas        unidad: visitas
Número de reparaciones   unidad: reparaciones
Importe de venta         unidad: euros
Tiempo de reparación     unidad: horas
```

### 2.2. La escala nominal

*Nominal* viene de *nombre*. Medir en escala nominal es asignar un nombre —una
palabra o una frase corta— a cada una de las formas que puede adoptar el dato.

Ejemplos en Rueda Libre:

```
Tipo de producto     bicicleta · componente · accesorio · ropa
Forma de pago        efectivo · tarjeta · financiación · transferencia
Tienda               Valencia · Zaragoza · Bilbao · …
Canal                tienda física · web
```

Lo que una escala nominal permite es **clasificar**: agrupar los individuos según
el valor del dato. «Las ventas de bicicleta a la izquierda, las de componente en
el centro, las de accesorio a la derecha.» Nada más. No hay orden: no tiene sentido
decir que *efectivo* va antes que *tarjeta*.

Con una variable nominal se puede calcular la frecuencia de cada categoría y la
moda. No se puede calcular ni la mediana ni la media.

### 2.3. La escala ordinal

Considérese ahora el nivel de satisfacción de un cliente medido como *bajo*,
*medio*, *alto*. Son tres palabras, tres categorías: sirven para clasificar, luego
es una escala nominal.

Pero además esas palabras tienen un **orden implícito**. Alguien con satisfacción
alta está más satisfecho que alguien con satisfacción media, y éste más que alguien
con satisfacción baja. Eso permite algo que la escala nominal no permitía:
**ordenar** a los individuos.

> Por definición, toda escala ordinal es también una escala nominal. Lo que añade
> es la posibilidad de ordenar.

Ejemplos en Rueda Libre:

```
Prioridad de la reparación    baja · normal · urgente
Segmento de cliente           ocasional · habitual · premium
Valoración del servicio       1 a 5 estrellas
Nivel de stock                agotado · bajo · normal · exceso
```

Con una variable ordinal se pueden calcular frecuencias, moda, **mediana**,
cuartiles y percentiles, y tienen sentido las frecuencias acumuladas. Sigue sin
poderse calcular la media.

⚠️ Ésta es la trampa más común de todo el capítulo. Una valoración de 1 a 5
estrellas está compuesta de números, pero es ordinal: no hay unidad de medida. La
distancia entre 1 y 2 estrellas no es necesariamente la misma que entre 4 y 5, y
lo que para un cliente es un 3 para otro es un 4. Promediar valoraciones es una
operación matemáticamente posible y conceptualmente discutible.

En la práctica todas las empresas del mundo promedian sus valoraciones. Lo que
puede hacerse, sin declararle la guerra a la organización, es acompañar siempre esa
media de la **distribución** —cuántos 1, cuántos 2, cuántos 5— o convertirla en un
indicador construido a propósito, como el porcentaje de detractores y promotores.
Una media de 3,5 puede venir de todo el mundo puntuando 3 y 4, o de la mitad
puntuando 1 y la otra mitad 5. Son dos negocios completamente distintos.

### 2.4. La escala cuantitativa

Los datos cuantitativos son los que representan una cantidad y tienen unidad de
medida. Esa unidad es lo que abre la puerta a las **operaciones matemáticas con
sentido real**.

La distinción importa. Matemáticamente se pueden sumar dos códigos postales:

```
19200 + 28850 = 48050
```

El resultado es correcto y no significa nada. Lo mismo con los números de portal:
vivir en el 17 y en el 25 no permite decir que entre los dos suman 42.

En cambio:

```
2 hijos + 3 hijos = 5 hijos          Tiene sentido
80 kg − 70 kg = 10 kg de diferencia  Tiene sentido
```

> La potencia de los datos cuantitativos no está en que sean números. Está en que
> las operaciones que se hagan con ellos siguen significando algo en el mundo real.

Los datos cuantitativos se clasifican de dos maneras distintas y complementarias.

**Según la cantidad de valores posibles:**

- **Discretos**: sólo admiten valores enteros. Número de reparaciones, unidades
  vendidas, número de visitas. Suelen tener un número reducido de valores
  distintos.
- **Continuos**: admiten cualquier valor dentro de un rango, decimales incluidos.
  Importe de venta, tiempo de reparación, peso de una bicicleta. Pueden tener
  infinitos valores.

Esta diferencia no es teórica: determina si se puede hacer una tabla de frecuencias
directamente o hay que agrupar en intervalos, y si el gráfico adecuado es un
diagrama de barras o un histograma.

**Según dónde está el cero:**

- **De razón**: el cero es absoluto y significa ausencia de la propiedad. No
  existen valores negativos. Importe de venta, unidades vendidas, tiempo de
  reparación, peso.
- **De intervalo**: el cero es arbitrario, fijado por convenio. Temperatura en
  grados Celsius, año de nacimiento, altitud sobre el nivel del mar.

La consecuencia es concreta. Con datos de razón se puede multiplicar y dividir; con
datos de intervalo, no:

```
Familia A: 4 hijos · Familia B: 2 hijos
→ «A tiene el doble de hijos que B»            Correcto

Madrid: 30 °C · Oslo: 15 °C
→ «En Madrid hace el doble de calor que en Oslo»   Incorrecto
```

Se ve mejor llevándolo al extremo. Si en Oslo hiciera −15 °C, la razón sería
30 / −15 = −2. ¿Menos el doble de calor? Matemáticamente hay un resultado; en la
práctica no significa nada. El cero de la escala Celsius no es la ausencia de
temperatura, es el punto de congelación del agua.

⚠️ En un cuadro de mando esto aparece disfrazado. «El margen ha crecido un 200 %»
es correcto si el margen es de razón y positivo. Si el margen del año anterior fue
negativo, el porcentaje de variación deja de tener interpretación. Lo mismo con
cualquier indicador que pueda cruzar el cero.

### 2.5. La escalera de medida

Se ha visto que toda escala ordinal es también nominal. Ahora conviene fijarse en
algo más: **todo dato cuantitativo es también ordinal**.

Las etiquetas «0 reparaciones», «1 reparación», «2 reparaciones» son categorías con
las que se puede clasificar. Y además tienen orden. Por tanto:

```
CUANTITATIVA  ⊃  ORDINAL  ⊃  NOMINAL
```

De lo que se derivan las dos reglas más útiles de todo el capítulo.

**Primera: siempre se puede bajar de escala.** Un dato cuantitativo puede tratarse
como ordinal, y uno ordinal como nominal. La misma variable, medida de tres
maneras:

```
Cuantitativa   Número de reparaciones al mes: 0, 1, 2, 3, 7, 12…
Ordinal        Volumen de taller: ninguno · bajo · medio · alto
Nominal        ¿Tiene actividad de taller?: sí / no
```

Agrupar una variable cuantitativa en intervalos no es más que eso: bajarla a una
escala ordinal.

```
0 reparaciones      → ninguno
1–2 reparaciones    → bajo
3–5 reparaciones    → medio
6 o más             → alto
```

**Segunda: nunca se puede subir.** Sabiendo únicamente que una tienda «tiene
actividad de taller» es imposible deducir si es baja o alta, y mucho menos el
número exacto de reparaciones.

> Esta asimetría es la razón de una regla práctica de diseño de almacenes: se
> guarda siempre el dato al máximo nivel de detalle disponible. Bajar de escala se
> puede hacer en cualquier momento; subir requiere volver a la fuente, si es que
> todavía existe.

### 2.6. Por qué esto decide todo lo demás

El resumen de las consecuencias, que se irán encontrando en los capítulos
siguientes:

| | Nominal | Ordinal | Cuantitativa |
|---|---|---|---|
| Frecuencias | Sí | Sí | Sí |
| Frecuencia acumulada | No tiene sentido | Sí | Sí |
| Moda | Sí | Sí | Sí |
| Mediana | No | Sí | Sí |
| Media | No | No | Sí |
| Cuartiles y percentiles | No | Sí | Sí |
| Desviación típica | No | No | Sí |
| Diagrama de barras / sectores | Sí | Sí | Sí (si discreta) |
| Histograma | No | No | Sí (si continua) |
| Caja y bigotes | No | Con cautela | Sí |
| En el modelo dimensional | Dimensión | Dimensión con orden | Hecho (métrica) |

La última fila es la que conecta este capítulo con el 8, y merece leerse dos veces:
**las dimensiones de un modelo analítico son variables nominales y ordinales; los
hechos son variables cuantitativas**. Quien tenga clara la escala de medida de cada
columna tiene ya medio modelo diseñado.

### Ejercicios del capítulo 2

**2.1.** Clasifica la escala de medida de cada campo del TPV de Rueda Libre e
indica si en el modelo analítico sería dimensión o hecho:
`id_ticket`, `fecha`, `id_tienda`, `código_postal_cliente`, `forma_de_pago`,
`unidades`, `importe`, `descuento_%`, `valoración_1_a_5`.

**2.2.** El director comercial pide «la media del código postal de los clientes
para saber dónde abrir la próxima tienda». Explica por qué la petición no tiene
sentido y propón dos formas correctas de responder a la necesidad que hay detrás.

**2.3.** La variable «tiempo de reparación» está en horas. Propón una agrupación en
intervalos que resulte útil para el jefe de taller y otra distinta que resulte útil
para la dirección. Justifica por qué no son la misma.

**2.4.** Rueda Libre mide la satisfacción con una escala de 1 a 10. La media es 7,2.
¿Qué información adicional pedirías antes de incluir ese 7,2 en el cuadro de mando
de dirección? Escribe la petición en una frase.

**2.5.** Una tienda registra la temperatura media diaria para estudiar su efecto en
las ventas. En enero fue de 5 °C y en julio de 25 °C. Un informe afirma: «en julio
hizo cinco veces más calor que en enero». Corrige la afirmación y explica cómo
habría que expresarla.

---
## 3. La calidad del dato

Los datos son como los ingredientes de una cocina. Si no se limpian, si no se
retira lo que no sirve, si no se cortan como toca, el resultado no será bueno por
mucho que la técnica de cocinado sea impecable.

El error más común de quien empieza en análisis de datos es lanzarse a aplicar
técnicas sobre un conjunto que nadie ha revisado. Lo que se obtiene entonces no
refleja la realidad: refleja los defectos del conjunto de datos.

> En un proyecto de BI la preparación de los datos ocupa más tiempo que el
> análisis. Habitualmente entre el 60 % y el 80 % del esfuerzo total. Cualquier
> planificación que no lo contemple está mal hecha desde el primer día.

### 3.1. Las seis dimensiones de la calidad

| Dimensión | Pregunta | Ejemplo de fallo en Rueda Libre |
|---|---|---|
| Exactitud | ¿El valor es correcto? | El TPV de Bilbao lleva dos meses con la fecha del sistema mal |
| Completitud | ¿Falta información? | El 18 % de los tickets no tiene cliente asociado |
| Consistencia | ¿Las fuentes coinciden? | La web y el TPV dan cifras distintas de la misma venta |
| Puntualidad | ¿Llega a tiempo? | El ERP vuelca el coste con un mes de retraso |
| Unicidad | ¿Hay duplicados? | Un mismo cliente dado de alta cinco veces |
| Validez | ¿Cumple las reglas? | Reparaciones con fecha de salida anterior a la de entrada |

Ninguna de las seis se arregla en la capa de visualización. Todas se arreglan
antes, en el proceso de carga, o no se arreglan.

### 3.2. Valores nulos

Un valor ausente no es un cero. Confundirlos es una de las formas más rápidas de
falsear un indicador.

```
Tienda      Ventas    Objetivo
Valencia    142.000    130.000
Zaragoza     98.000    110.000
Bilbao        NULL     120.000     ← ¿No vendió nada, o no llegó el dato?
```

Si el nulo se trata como cero, la cadena aparece con un 30 % de desviación sobre
objetivo que no es real. Si se excluye la fila, el total baja pero el porcentaje de
cumplimiento sube. Las dos opciones son defendibles; lo que no es defendible es no
saber cuál se ha aplicado.

Estrategias habituales, de menos a más intervención:

1. **Dejarlo como nulo y hacerlo visible.** Casi siempre la mejor opción en BI: el
   cuadro de mando muestra «sin dato» y quien lo lee sabe a qué atenerse.
2. **Excluir el registro.** Válido si los ausentes son pocos y no siguen un patrón.
3. **Imputar un valor.** Sustituirlo por la media, la mediana o el valor anterior.
   Útil en modelado, peligroso en reporting: se está inventando un dato.
4. **Excluir la variable entera.** Si falta el 70 % de los valores, esa columna no
   sirve para nada.

⚠️ Antes de decidir, hay que preguntarse **por qué** falta. Si los tickets sin
cliente son precisamente los de venta rápida en efectivo, la ausencia no es
aleatoria: contiene información. Excluirlos sesgaría el análisis de tipos de venta.

### 3.3. Duplicados

Los duplicados exactos son fáciles: se detectan y se eliminan. Los peligrosos son
los otros.

```
id   nombre                 email                  teléfono
1    Manuel Rodríguez       manolo@correo.es       666112233
2    Manolo Rodriguez       manolo@correo.es       666 11 22 33
3    M. Rodríguez Pérez     manolo@correo.es       +34666112233
```

Tres registros, una persona. El impacto no es cosmético: el número de clientes está
inflado, la venta media por cliente está deflactada y cualquier segmentación por
frecuencia de compra es falsa, porque las compras de Manolo están repartidas entre
tres identidades.

La solución técnica se llama **deduplicación** y consiste en definir una regla de
identidad —qué campos determinan que dos registros son la misma entidad— y
normalizar antes de comparar. La solución organizativa se llama gobierno del dato,
y consiste en que alguien sea responsable del maestro de clientes.

### 3.4. Codificaciones inconsistentes

El mismo concepto escrito de formas distintas en fuentes distintas.

```
TPV               Tienda en línea      ERP
"BICICLETA"       "Bici"               "BIC"
"COMPONENTE"      "Componentes"        "CMP"
"ACCESORIO"       "Accesorios"         "ACC"
"ROPA"            "Textil"             "TEX"
```

Si no se unifica, un cuadro de mando por familia de producto mostrará ocho
categorías donde hay cuatro, y ninguna cifra cuadrará con ninguna otra. El proceso
que arregla esto se llama **conformado** y se ve en el capítulo 7.

### 3.5. Unidades, divisas y escalas

Un número sin unidad no es un dato: es un número.

```
peso_bicicleta:  9.4     ¿kilogramos o libras?
importe:         1250    ¿euros o céntimos?
tiempo:          180     ¿minutos u horas?
descuento:       0.15    ¿15 % o 0,15 %?
```

El caso de los céntimos es especialmente frecuente porque muchos sistemas
transaccionales guardan importes como enteros para evitar errores de redondeo. Si
el proceso de carga no lo convierte, el cuadro de mando muestra ventas cien veces
mayores de lo real y nadie se da cuenta hasta que un director pregunta por qué la
cadena factura más que su sector entero.

⚠️ El descuento como `0.15` frente a `15` es la trampa silenciosa: ambos valores
son plausibles, ninguno da error, y el resultado se desvía un factor cien. La única
defensa es una regla de validación en la carga que compruebe rangos esperados.

### 3.6. Fechas

Las fechas concentran una cantidad desproporcionada de errores.

- **Formato.** `03/04/2026` es 3 de abril en España y 4 de marzo en Estados Unidos.
- **Zona horaria.** Una venta en línea a las 00:30 puede caer en un día u otro
  según cómo se almacene. En un cierre mensual esto mueve cifras.
- **Fecha de qué.** Fecha del pedido, del cobro, de la entrega y de la
  contabilización son cuatro fechas distintas. Un informe de ventas por mes da
  resultados distintos según cuál se use, y las cuatro son «la fecha de la venta»
  para alguien de la organización.
- **Fechas imposibles.** `31/02`, fechas futuras, `01/01/1900` como marca de nulo.

> Cuando dos informes de ventas no cuadran, en la mitad de los casos el problema no
> está en el importe: está en qué fecha se ha usado para asignar la venta a un mes.

### 3.7. Granularidades distintas

Es el problema de integración más difícil, porque no da ningún error: simplemente
produce resultados equivocados.

```
TPV               una fila por LÍNEA de ticket
Tienda en línea   una fila por PEDIDO
ERP               una fila por ALBARÁN
Objetivos.xlsx    una fila por TIENDA y MES
```

Cruzar directamente el TPV con los objetivos multiplica las filas: cada línea de
ticket se emparejaría con el objetivo mensual completo, y el objetivo aparecería
sumado tantas veces como líneas haya. El resultado es un cumplimiento del 3 % que
provoca una reunión de urgencia innecesaria.

La regla es: **antes de cruzar dos conjuntos de datos, hay que saber decir en voz
alta qué es una fila en cada uno**. Si no coinciden, hay que agregar uno de los dos
hasta el grano común, y hacerlo explícitamente.

### 3.8. El coste de no limpiar

Se resume en una frase que conviene repetir en la reunión de arranque de cualquier
proyecto:

> Un dato malo en un sistema operativo afecta a una transacción. El mismo dato malo
> en un almacén analítico afecta a todas las decisiones que se tomen con él.

Y en una consecuencia práctica: el sitio donde se arregla la calidad del dato es la
**fuente**, no el proceso de carga. Un proceso de carga que corrige errores
sistemáticos está ocultando un problema que seguirá creciendo. Lo correcto es
corregir en origen y usar la carga como red de seguridad y como detector.

### Ejercicios del capítulo 3

**3.1.** En el conjunto de reparaciones de Rueda Libre hay 40 registros con fecha
de salida anterior a la de entrada. ¿A qué dimensión de la calidad afecta? Propón
tres causas posibles y qué harías con esos registros en cada caso.

**3.2.** El 18 % de los tickets no tiene cliente asociado. La dirección quiere el
indicador «venta media por cliente». Explica qué tres valores distintos podría
tener ese indicador según cómo se traten los nulos, y cuál publicarías.

**3.3.** Se detecta que el TPV guarda los importes en céntimos y la tienda en línea
en euros. Nadie se dio cuenta durante dos meses. ¿Qué indicadores del cuadro de
mando estuvieron mal y cuáles no? Justifica por qué algunos se salvaron.

**3.4.** Diseña tres reglas de validación automática que hubieran detectado, en la
carga, los errores de los ejercicios anteriores.

**3.5.** El fichero `Objetivos.xlsx` lo mantiene una persona a mano y a veces lo
actualiza después del cierre. ¿A qué dimensión de la calidad afecta? Propón una
solución que no dependa de la buena voluntad de esa persona.

---

## 4. Resumir: frecuencias y tendencia central

Vuelta al problema de las cinco mil nóminas. Hace falta un mecanismo para reducir
el volumen de información hasta hacerlo manejable. La estadística ofrece dos
familias: las tablas de frecuencias y los indicadores.

### 4.1. El concepto de frecuencia

*Frecuencia* es una palabra de uso cotidiano: cuántas veces ocurre algo. En
estadística significa exactamente eso, aplicado a los datos: **cuántos individuos
comparten un mismo valor**.

Para poder calcular una frecuencia hace falta poder clasificar. Y clasificar es
justo lo que permite una escala nominal. Como toda escala ordinal y toda escala
cuantitativa son también nominales, **a cualquier variable se le pueden calcular
frecuencias**. Es la única técnica del manual de la que se puede decir eso.

### 4.2. La tabla de frecuencias

Es la representación básica. Ventas de Rueda Libre por familia de producto en un
mes:

| Familia | Frecuencia absoluta |
|---|---:|
| Bicicleta | 310 |
| Componente | 1.240 |
| Accesorio | 890 |
| Ropa | 260 |
| **Total** | **2.700** |

La **frecuencia absoluta** es el número de veces que aparece cada categoría.

La **frecuencia relativa** es la proporción sobre el total, normalmente expresada
en porcentaje: frecuencia absoluta dividida entre el total, por cien.

| Familia | Absoluta | Relativa |
|---|---:|---:|
| Bicicleta | 310 | 310/2.700 × 100 = 11,5 % |
| Componente | 1.240 | 45,9 % |
| Accesorio | 890 | 33,0 % |
| Ropa | 260 | 9,6 % |
| **Total** | **2.700** | **100 %** |

La relativa es más útil cuando lo que interesa es el peso de cada categoría y no la
cifra bruta, y es imprescindible para comparar dos períodos o dos tiendas de tamaño
distinto.

### 4.3. La frecuencia acumulada

Cuando la variable está medida **al menos en escala ordinal**, se puede calcular la
frecuencia acumulada: la suma de las frecuencias de todas las categorías anteriores
a una dada, más la propia.

Tiempo de reparación en el taller, agrupado en intervalos:

| Tiempo (horas) | Absoluta | Relativa | Acum. absoluta | Acum. relativa |
|---|---:|---:|---:|---:|
| 0 – 4 | 420 | 42 % | 420 | 42 % |
| 5 – 8 | 310 | 31 % | 730 | 73 % |
| 9 – 24 | 160 | 16 % | 890 | 89 % |
| 25 – 72 | 80 | 8 % | 970 | 97 % |
| más de 72 | 30 | 3 % | 1.000 | 100 % |

La acumulada permite afirmaciones que ninguna otra columna permite:

- El 73 % de las reparaciones se resuelven en 8 horas o menos.
- 270 reparaciones tardaron más de 8 horas.
- El 3 % tardó más de tres días. Ésas son las de Manolo.

⚠️ **La frecuencia acumulada no tiene sentido con variables nominales.** Se puede
calcular —matemáticamente nada lo impide— pero no significa nada. Sobre la tabla de
familias de producto, la acumulada del 45,9 % + 33,0 % no permite decir que «el
78,9 % de las ventas son de familia inferior a accesorio». No hay orden.

Hay una salvedad importante: aunque la variable no tenga orden propio, se le puede
imponer uno según **otro criterio**, típicamente la propia frecuencia. Ordenando
descendentemente:

| Familia | Absoluta | Relativa | Acum. relativa |
|---|---:|---:|---:|
| Componente | 1.240 | 45,9 % | 45,9 % |
| Accesorio | 890 | 33,0 % | 78,9 % |
| Bicicleta | 310 | 11,5 % | 90,4 % |
| Ropa | 260 | 9,6 % | 100 % |

Ahora sí tiene sentido: «dos familias concentran el 79 % de las operaciones». Es el
fundamento del análisis de Pareto y de cualquier cálculo de cuota de mercado.

### 4.4. Agrupar en intervalos

Con una variable continua como el importe de venta, contar cuántas veces aparece
cada valor exacto produce una tabla con miles de filas y frecuencia 1 en casi
todas. No se ha resumido nada.

```
Importe    Frecuencia
34,90            1
35,00            2
35,15            1
35,20            1
…                …
```

La solución es agrupar en intervalos, que —como se vio en el capítulo 2— no es más
que bajar la variable a una escala ordinal:

| Importe (€) | Frecuencia | Relativa |
|---|---:|---:|
| 0 – 25 | 620 | 23 % |
| 26 – 75 | 980 | 36 % |
| 76 – 200 | 640 | 24 % |
| 201 – 800 | 340 | 13 % |
| más de 800 | 120 | 4 % |
| **Total** | **2.700** | **100 %** |

La elección de los intervalos no es neutra: **cambiar los cortes cambia la
historia** que cuenta el gráfico. Es una decisión de análisis, no un detalle
técnico, y conviene documentarla.

### 4.5. Los gráficos asociados

- **Diagrama de barras.** Una barra por categoría, la altura es la frecuencia. El
  eje X no está graduado: las barras pueden ir en cualquier orden. Sirve para
  nominales, ordinales y cuantitativas discretas con pocos valores.
- **Diagrama de sectores.** Cada sector es una categoría y su tamaño es proporcional
  a la frecuencia relativa. El círculo entero es el 100 %. Sirve para mostrar
  **composición**, y sólo si hay pocas categorías.
- **Histograma.** Similar al de barras, pero el eje X está **graduado** porque la
  variable es continua. Cada barra ocupa su posición real y su anchura es la del
  intervalo. El área total es proporcional al número de individuos.
- **Polígono de frecuencias.** Une los puntos medios de las barras del histograma
  con una línea. Es una versión simplificada, útil para ver la forma de la
  distribución y para superponer dos grupos.

⚠️ Barras y sectores no son intercambiables. La barra sirve para **comparar**
magnitudes; el sector, para mostrar **partes de un todo**. El ojo humano compara
longitudes con precisión y ángulos con muy poca, así que ante la duda, barras. Un
diagrama de sectores con más de cinco categorías es prácticamente ilegible.

### 4.6. La moda

Es el indicador de tendencia central más sencillo: **el valor que más se repite**,
el de mayor frecuencia.

En la tabla de familias, la moda es *componente*. En la de tiempos de reparación,
el intervalo *0 – 4 horas*. En un diagrama de barras es la barra más alta; en uno
de sectores, el sector mayor; en un histograma, el pico.

Se puede calcular para **cualquier escala de medida**, y es el único indicador de
tendencia central disponible para variables nominales.

Precisiones:

- Puede no haber moda, si todos los valores aparecen una sola vez.
- Puede haber varias modas, si varios valores empatan.
- Aunque no haya empate exacto, si el histograma presenta varios picos se habla de
  distribución **multimodal**. Y eso es una señal importante: casi siempre indica
  que dentro del conjunto hay **dos poblaciones distintas mezcladas**. En Rueda
  Libre, el histograma de importe de venta tiene dos picos porque mezcla la venta
  de accesorios con la venta de bicicletas completas. La reacción correcta no es
  calcular indicadores sobre el conjunto: es separarlo.

### 4.7. La mediana

Es el valor que ocupa el **centro** del conjunto una vez ordenado de menor a mayor.
Divide a los individuos en dos mitades del mismo tamaño.

> Es la altura a la que se pondría el listón del limbo para que pasara justo la
> mitad de la gente.

Cálculo: se ordenan los datos. Si el número de valores es impar, la mediana es el
central. Si es par, la media de los dos centrales.

```
5  7  8  11  14  19  40         →  mediana = 11
5  7  8  11  14  19  40  55     →  mediana = (11+14)/2 = 12,5
```

Sobre una tabla de frecuencias se localiza buscando el primer valor o intervalo
cuya frecuencia acumulada relativa **alcanza o supera el 50 %**. En la tabla de
tiempos de reparación, el primer intervalo que llega al 50 % es *5 – 8 horas*: ahí
está la mediana.

En un histograma, la mediana es el valor que divide el **área** en dos partes
iguales.

Requiere poder ordenar, así que se puede calcular para variables ordinales y
cuantitativas, pero **no para nominales**.

### 4.8. La media

Es el indicador más usado y el peor entendido. Se calcula sumando todos los valores
y dividiendo entre el número de individuos.

Como requiere **sumar**, sólo se puede calcular sobre variables cuantitativas: las
únicas que tiene sentido sumar. No se puede calcular la media de una variable
cualitativa de ningún tipo, ordinal incluida.

La interpretación correcta, que casi nunca se enseña:

> La media representa lo que aportaría cada individuo al total si todos aportaran
> exactamente lo mismo.

No dice que los individuos estén en torno a ese valor. Eso es lo que el cerebro
humano asume automáticamente, y es la fuente de casi todos los errores de
interpretación.

### 4.9. Cuándo la media engaña

El ejemplo canónico del curso. Dos pueblos de diez habitantes, midiendo los gramos
de CO₂ que emiten al día sus vehículos.

**Villa Arriba:**

```
5  5  5  10  10  10  10  15  15  15
Media = 10        Mediana = 10
```

**Villa Abajo.** Nueve vecinos concienciados con un híbrido que emite 5. Y Manolo,
con un tractor de mil caballos que emite 1.000:

```
5  5  5  5  5  5  5  5  5  1000
Media = 104,5     Mediana = 5
```

Mirando sólo la media, Villa Abajo parece diez veces más contaminante que Villa
Arriba. Y es exactamente al revés: nueve de sus diez vecinos emiten la mitad que el
vecino menos contaminante de Villa Arriba.

La media no miente —es cierto que si repartiéramos las emisiones a partes iguales
tocarían a 104,5 por cabeza— pero **no resume bien al colectivo**, que era el
objetivo. La mediana, 5, describe mucho mejor a esa población.

La versión de negocio del mismo fenómeno, que aparecerá en el cuadro de mando de
Rueda Libre:

```
Tiempo de resolución de reparaciones

Media       3,2 días
Mediana     1,1 días
Percentil 90  8,7 días
```

Publicar sólo la media da una imagen falsa por arriba: la mayoría de los clientes
recoge su bicicleta al día siguiente. Publicar sólo la mediana da una imagen falsa
por abajo: uno de cada diez clientes espera más de ocho días, y ésos son los que
escriben reseñas. **Los tres números juntos describen el servicio; ninguno lo hace
por separado.**

### 4.10. Robusto y suficiente

La estadística tiene dos palabras para nombrar la diferencia:

- La **mediana** es un indicador **robusto**: no le afectan los valores extremos.
  Pero no es **suficiente**: para calcularla sólo se mira el centro, y se ignora el
  resto de la información.
- La **media** es **suficiente**: usa todos los valores del conjunto. Pero no es
  **robusta**: precisamente por usarlos todos, un solo valor extremo la desplaza.

> La media aprovecha toda la información disponible, y por eso es sensible a los
> extremos. La mediana ignora casi toda la información, y por eso está protegida
> frente a ellos.

### 4.11. Cómo elegir

El criterio **no** es «si hay valores atípicos». El criterio es la **simetría de la
distribución**, que se mira en el histograma o en el polígono de frecuencias.

- Si se puede trazar una línea vertical que deje dos mitades aproximadamente
  especulares, la distribución es **simétrica**: media y mediana serán parecidas y
  se puede usar la media.
- Si no, es **asimétrica**: media y mediana se separarán, y la mediana representará
  mejor al colectivo.

En la práctica se calculan las dos y se comparan. Si difieren mucho, hay asimetría.
¿Cuánto es «mucho»? Es un criterio subjetivo y depende del contexto: una diferencia
de dos grados es irrelevante para decidir si se coge abrigo y enorme para estudiar
el deshielo polar. Como orientación inicial, muchos analistas consideran que si la
diferencia no supera el 10–15 % del valor de la mediana, las dos son suficientemente
cercanas. No es una regla, es un punto de partida.

Y por encima de cualquier regla: **mirar el histograma primero**. La visualización
resuelve en dos segundos lo que la comparación numérica sugiere en cinco minutos.

### Ejercicios del capítulo 4

**4.1.** Con la tabla de tiempos de reparación del apartado 4.3, responde: ¿qué
porcentaje de reparaciones tarda más de 24 horas? ¿Cuántas reparaciones tardan
entre 5 y 24 horas? ¿En qué intervalo está la mediana?

**4.2.** El responsable de la tienda en línea afirma: «el 90,4 % de nuestras ventas
son de familia igual o inferior a bicicleta». Explica qué está mal en la afirmación
y qué habría que decir en su lugar.

**4.3.** El histograma del importe de venta de Rueda Libre tiene dos picos claros:
uno hacia 40 € y otro hacia 900 €. ¿Qué está pasando? ¿Qué harías antes de calcular
ningún indicador sobre esa variable?

**4.4.** Estos son los importes de las diez últimas ventas de una tienda:
`38, 42, 45, 51, 55, 60, 68, 74, 90, 2.400`.
Calcula media y mediana. ¿Cuál publicarías en el cuadro de mando y por qué? ¿Qué
harías con el valor de 2.400?

**4.5.** Rueda Libre presenta a la prensa el dato «ticket medio: 128 €». La mediana
es 54 €. Redacta en dos frases cómo lo comunicarías tú para que sea igual de cierto
y menos engañoso.

**4.6.** ¿Por qué no se puede calcular la media de la variable «prioridad de la
reparación» (baja, normal, urgente) aunque se codifique como 1, 2 y 3? ¿Qué
indicador de tendencia central usarías?

---

## 5. Dispersión, posición y forma

Este capítulo contiene la regla más importante del manual, y conviene enunciarla
antes de explicar nada:

> **Nunca se ofrece un indicador de tendencia central sin su indicador de
> dispersión asociado.** Nunca. Ni en un informe, ni en un cuadro de mando, ni en
> una reunión.

### 5.1. Por qué una media sola no dice nada

Cuatro casas, cuatro habitantes cada una, midiendo su altura en centímetros:

```
Casa 1    160  165  175  180      Media 170    Mediana 170
Casa 2    160  160  180  180      Media 170    Mediana 170
Casa 3    170  170  170  170      Media 170    Mediana 170
Casa 4    155  170  170  185      Media 170    Mediana 170
```

Las cuatro casas son completamente distintas y los indicadores de tendencia central
son idénticos. Con esos dos números no hay forma de distinguirlas.

La versión de negocio:

```
Empresa A     98  100  101   99  102      Media 100
Empresa B     20   50  100  150  180      Media 100
```

Misma media, realidad empresarial opuesta. La A es predecible y se puede
planificar; la B no. Cualquier decisión —dimensionar un almacén, fijar un objetivo,
contratar personal— sería distinta en cada caso.

Hace falta un segundo número que mida **cuánto se alejan los datos de la tendencia
central**.

### 5.2. El camino hasta la desviación típica

Merece la pena recorrerlo, porque explica por qué la fórmula es como es.

**Primer intento: la diferencia respecto a la media.** Se llama *puntuación
diferencial*.

```
Casa 1    −10   −5    5   10
Casa 2    −10  −10   10   10
Casa 3      0    0    0    0
Casa 4    −15    0    0   15
```

**Segundo intento: promediar esas diferencias.** Y aquí llega el chasco: da cero en
las cuatro casas. Los valores por encima de la media anulan exactamente a los que
están por debajo. Siempre. Por construcción.

Lo que interesa no es si se desvían por arriba o por abajo, sino **cuánto** se
desvían. Hay que eliminar el signo, y en matemáticas hay dos operaciones habituales
para ello.

**Tercer intento: el valor absoluto.** Promediando los valores absolutos de las
diferencias se obtiene la **desviación media**:

```
Casa 1    (10+5+5+10)/4  = 7,5
Casa 2    (10+10+10+10)/4 = 10
Casa 3    0
Casa 4    (15+0+0+15)/4  = 7,5
```

Mejor, pero insuficiente: las casas 1 y 4 obtienen el mismo valor teniendo
distribuciones claramente distintas. Por eso hoy apenas se usa.

**Cuarto intento: el cuadrado.** Promediando los cuadrados de las diferencias se
obtiene la **varianza**:

```
Casa 1    (100+25+25+100)/4   = 62,5
Casa 2    (100+100+100+100)/4 = 100
Casa 3    0
Casa 4    (225+0+0+225)/4     = 112,5
```

Ahora sí se distinguen las cuatro. Elevar al cuadrado penaliza más las desviaciones
grandes que las pequeñas, que es justo lo que se quería.

El problema de la varianza es su interpretación: al elevar al cuadrado, **las
unidades también se elevan al cuadrado**. La varianza de la casa 1 es 62,5 cm². Una
superficie, midiendo alturas. Y con importes es peor: ¿alguien ha visto alguna vez
un euro cuadrado?

**Quinto y definitivo: la raíz cuadrada de la varianza**, que devuelve el resultado
a las unidades originales. Es la **desviación típica** o **desviación estándar**:

```
Casa 1    √62,5  = 7,9 cm
Casa 2    √100   = 10 cm
Casa 3    √0     = 0 cm
Casa 4    √112,5 = 10,6 cm
```

Ahora el número está en centímetros y se puede interpretar.

### 5.3. Cómo se interpreta la desviación típica

La desviación típica indica cuánto se alejan los datos de la media en términos
generales. Cuanto mayor, más dispersos; cuanto menor, más concentrados.

Para convertir eso en una afirmación concreta hay dos herramientas, y conviene no
confundirlas porque la confusión está muy extendida.

**La desigualdad de Chebyshev** dice que, *sea cual sea la distribución*, la
proporción de individuos que caen a menos de *k* desviaciones típicas de la media
es al menos 1 − 1/k²:

| k | Intervalo | Contiene al menos |
|---|---|---|
| 1 | media ± 1σ | — (la desigualdad no dice nada) |
| 1,41 | media ± 1,41σ | 50 % |
| **2** | **media ± 2σ** | **75 %** |
| 3 | media ± 3σ | 89 % |

**La regla empírica** dice que si la distribución es **aproximadamente normal** —una
campana simétrica— entonces:

| Intervalo | Contiene aproximadamente |
|---|---|
| media ± 1σ | 68 % |
| media ± 2σ | 95 % |
| media ± 3σ | 99,7 % |

⚠️ Es muy frecuente oír que «media ± 1 desviación típica contiene al menos el 50 %
de los casos, sea cual sea la distribución». **Es falso.** Chebyshev para k = 1 no
garantiza nada, y el 68 % de la regla empírica exige normalidad. La casa 2 del
ejemplo (160, 160, 180, 180) tiene media 170 y σ = 10: el intervalo [160, 180]
contiene el 100 % de los individuos, no el 50 %. La afirmación segura, la que
siempre se cumple, es la de **±2σ y el 75 %**.

Aplicado a Rueda Libre: si el ticket medio de una tienda es de 128 € con una
desviación típica de 45 €, se puede afirmar sin conocer la distribución que **al
menos tres de cada cuatro tickets están entre 38 € y 218 €**. Es una frase que un
comité de dirección entiende, y es cierta.

### 5.4. El rango

El indicador de dispersión más simple: máximo menos mínimo.

```
Casa 1: 180 − 160 = 20 cm
Casa 4: 185 − 155 = 30 cm
```

Es muy fácil de calcular y muy poco fiable, porque depende exclusivamente de los
dos valores más extremos: exactamente los más propensos a ser errores. Sirve como
comprobación rápida de plausibilidad —si el rango de un importe de venta va de
−300 € a 2.000.000 €, hay un problema de calidad— y poco más.

### 5.5. Las medidas de posición

La mediana, vista en el capítulo anterior, es en realidad una medida de posición:
el valor que deja por debajo al 50 %. Generalizando la idea:

- **Cuartiles.** Dividen el conjunto en cuatro partes iguales.
  - **Q1**: deja por debajo al 25 %.
  - **Q2**: deja por debajo al 50 %. **Es la mediana.**
  - **Q3**: deja por debajo al 75 %.
- **Deciles.** Diez partes. D1 deja por debajo al 10 %, D9 al 90 %.
- **Percentiles.** Cien partes. P25 = Q1, P50 = mediana, P90, P95, P99.

Todos coinciden en los puntos comunes: D5 = P50 = Q2 = mediana.

Se localizan en una tabla de frecuencias mirando la columna de frecuencia acumulada
relativa: el cuartil Q1 es el primer intervalo que alcanza el 25 %, Q3 el primero
que alcanza el 75 %.

**Por qué le importan al negocio.** Los percentiles altos son el lenguaje natural
de los compromisos de servicio y del dimensionamiento:

```
P90 del tiempo de reparación = 8,7 días
   → «9 de cada 10 clientes recogen su bicicleta antes de 9 días»
   → Es el número que se puede comprometer en el acuerdo de servicio

P95 de la concurrencia en la web = 340 usuarios simultáneos
   → Es el número con el que se dimensiona el servidor, no la media
```

> La media sirve para entender el conjunto. Los percentiles sirven para
> comprometerse con alguien. Ningún cliente vive en la media.

### 5.6. El rango intercuartílico

Cuando la distribución es asimétrica y se usa la mediana como tendencia central, la
desviación típica —que se calcula a partir de la media— deja de ser el compañero
adecuado. El indicador de dispersión asociado a la mediana es el **rango
intercuartílico** (IQR):

```
IQR = Q3 − Q1
```

Como Q1 deja por debajo al 25 % y Q3 al 75 %, el IQR mide la amplitud del intervalo
que contiene al **50 % central** de los individuos: los más representativos del
colectivo, los que están alrededor de la mediana.

Ese 50 % es el mismo 50 % del que hablaba Chebyshev con ±1,41σ, y por eso ambos
indicadores tienen una interpretación equivalente:

| Tendencia central | Dispersión | Interpretación |
|---|---|---|
| Media | Desviación típica (σ) | media ± 2σ contiene al menos el 75 % |
| Mediana | Rango intercuartílico | Q1 a Q3 contiene el 50 % central |

En Rueda Libre, con reparaciones de mediana 1,1 días, Q1 = 0,6 y Q3 = 3,4:

```
IQR = 3,4 − 0,6 = 2,8 días
→ «La mitad de las reparaciones se resuelven entre medio día y tres días y medio»
```

### 5.7. Simetría y asimetría

La **forma** de la distribución es lo que determina qué indicadores usar. Se mira
en el histograma o en el polígono de frecuencias.

**Simétrica.** Se puede trazar un eje vertical que deje dos mitades especulares.
Media, mediana y moda coinciden aproximadamente. Se puede usar la media.

**Asimétrica positiva** (cola hacia la derecha). La mayoría de los valores están en
la parte baja y unos pocos muy altos estiran la cola. Es la forma más frecuente en
negocio: importes de venta, salarios, tiempos de espera, valor de cliente.

```
media  >  mediana  >  moda
```

En Rueda Libre, el importe de venta: muchísimas ventas de 30 €, unas pocas
bicicletas de 3.000 €. La media está inflada por esas pocas; la mediana describe
mejor a la clientela habitual.

**Asimétrica negativa** (cola hacia la izquierda). La mayoría de los valores están
en la parte alta y unos pocos muy bajos estiran la cola. Menos frecuente. El
ejemplo clásico es la edad de jubilación: casi todo el mundo se jubila alrededor de
los 65 y unos pocos mucho antes.

```
media  <  mediana  <  moda
```

> Regla práctica: si la media es claramente mayor que la mediana, hay asimetría
> positiva y unos pocos valores altos están tirando de ella. Si es claramente
> menor, asimetría negativa. Si son parecidas, la distribución es aproximadamente
> simétrica.

Y una imagen que ayuda a recordarlo: la media siempre quiere estar junto a la
mediana, pero los valores extremos tiran de ella. Manolo, con su tractor, arrastra
la media hacia arriba. Gertru, que consume CO₂ en lugar de producirlo, tira hacia
abajo y la devuelve a su sitio.

### 5.8. Valores atípicos

Un **valor atípico** es una observación que se aleja mucho del resto. El criterio
más extendido para detectarlos usa el rango intercuartílico:

```
Límite inferior razonable  =  Q1 − 1,5 × IQR
Límite superior razonable  =  Q3 + 1,5 × IQR

Fuera de esos límites          → valor atípico
Más allá de Q1/Q3 ± 3 × IQR    → valor atípico extremo
```

El factor 1,5 es una convención, no una ley: en algunos contextos se usa 2 o más,
según la naturaleza de los datos.

Y ahora la pregunta que de verdad importa, y que no responde ninguna fórmula:

> ¿Este valor atípico es un **error de dato** o es la **realidad**?

Son dos situaciones opuestas y la reacción es opuesta:

| Es un error | Es real |
|---|---|
| Importe de 1.250.000 € por céntimos mal convertidos | Una venta corporativa de 40 bicicletas |
| Reparación de −3 días por fechas invertidas | La restauración completa de Manolo, 21 días |
| Cliente con 400 años por fecha `01/01/1900` | El cliente que compra doce veces al año |
| **Se corrige o se elimina** | **Se conserva, y a menudo se estudia aparte** |

⚠️ Eliminar valores atípicos porque «ensucian el gráfico» es una de las peores
prácticas del análisis de datos. Con frecuencia el atípico es lo más interesante
del conjunto: el fraude, el cliente más rentable, el fallo de proceso, la
oportunidad de negocio. En detección de fraude y en mantenimiento predictivo, el
atípico **es** el objeto de estudio.

Manolo, con sus restauraciones de tres semanas, es un valor atípico en el tiempo de
reparación. No es un error: es una línea de negocio distinta, con márgenes distintos
y clientes distintos. La decisión correcta no es borrarlo del conjunto, es
**segmentar**: analizar por separado la reparación estándar y la restauración.

### 5.9. El diagrama de caja y bigotes

Con la mediana, los cuartiles, el rango intercuartílico y los atípicos ya se puede
leer uno de los gráficos más compactos de la estadística.

```
        mín. razonable    Q1   mediana   Q3       máx. razonable
              ├────────────┏━━━━━━━┳━━━━━━━┓────────────┤        ○        ·
                          ┗━━━━━━━┻━━━━━━━┛                   atípico   extremo
              │◄──────────►│◄─────────────►│◄──────────►│
               hasta 1,5×IQR      IQR       hasta 1,5×IQR
```

Cómo se lee, en este orden:

1. **La caja** va de Q1 a Q3: contiene el 50 % central de los datos. Su anchura es
   el IQR, es decir, la dispersión.
2. **La línea interior** es la mediana. Si no está centrada en la caja, hay
   asimetría.
3. **Los bigotes** llegan hasta el último dato que cae dentro de 1,5 × IQR. Son el
   mínimo y el máximo *razonables*, no el mínimo y el máximo reales.
4. **Los puntos sueltos** son los valores atípicos, cada uno individualmente.
5. Si la caja entera está desplazada a un lado, la distribución es asimétrica.
6. A veces se marca también la media con una ✕. Si la ✕ y la mediana están
   separadas, hay asimetría.

Es la navaja suiza del análisis de datos cuantitativos: en un dibujo minimalista
muestra dónde está el centro, cuánta dispersión hay, si la distribución es simétrica
y si hay Manolos sueltos.

Su mayor virtud aparece al **comparar grupos**: doce cajas, una por tienda, en el
mismo eje, permiten ver en un segundo qué tienda tiene el ticket más alto, cuál el
más irregular y cuál acumula operaciones anómalas.

### 5.10. El gráfico de violín

Es un diagrama de caja al que se le añade, a ambos lados, la forma de la
distribución —un histograma suavizado—, lo que le da su silueta característica.

Aporta lo único que el boxplot no puede mostrar: **si la distribución es
multimodal**. Dos distribuciones con idénticos cuartiles pueden tener una sola
concentración central o dos grupos separados, y el boxplot las dibuja igual. El
violín las distingue de un vistazo.

### Ejercicios del capítulo 5

**5.1.** La tienda de Valencia tiene un ticket medio de 128 € con desviación típica
de 45 €. La de Zaragoza, 128 € con desviación típica de 12 €. Describe en dos
frases en qué se diferencian los dos negocios y qué decisión distinta tomarías en
cada uno.

**5.2.** Usando Chebyshev, escribe la afirmación más fuerte que puedas hacer sobre
el ticket de Valencia sin conocer su distribución. Después escribe la que podrías
hacer si supieras que es aproximadamente normal. ¿Cuál publicarías?

**5.3.** Reparaciones de una tienda, en días:
`0,5 · 0,5 · 1 · 1 · 1 · 1,5 · 2 · 2 · 3 · 4 · 5 · 21`
Calcula mediana, Q1, Q3 e IQR. Aplica el criterio de 1,5 × IQR: ¿cuáles son
atípicos? ¿Qué harías con ellos?

**5.4.** Un analista propone eliminar del conjunto todas las reparaciones de más de
10 días «porque distorsionan el indicador». Argumenta a favor y en contra, y decide.

**5.5.** Dibuja aproximadamente el histograma del importe de venta de Rueda Libre
sabiendo que media = 128 €, mediana = 54 € y moda = 35 €. ¿Cómo se llama esa forma?
¿Qué indicador de tendencia central publicarías?

**5.6.** Explica por qué dos distribuciones pueden tener exactamente el mismo
diagrama de caja y bigotes y ser muy distintas. ¿Qué gráfico usarías para
distinguirlas?

**5.7.** El comité de dirección pide «un solo número» para el tiempo de reparación.
Explica por qué te niegas y qué propones en su lugar, en tres cifras como máximo.

---
## 6. Sistemas operacionales y sistemas analíticos

Hasta aquí se ha hablado de qué hacer con los datos. Este capítulo trata de dónde
salen y por qué hace falta moverlos de sitio antes de analizarlos.

### 6.1. Sistemas OLTP

**OLTP** son las siglas de *On-Line Transaction Processing*: procesamiento de
transacciones en línea. Son los sistemas que sostienen la operación diaria: el TPV
de la tienda, el gestor del taller, la tienda en línea, el ERP.

Están diseñados con un objetivo muy concreto: **registrar muchas operaciones
pequeñas, muy deprisa y sin perder ninguna**. Todo en ellos está optimizado para
eso.

La consecuencia de diseño más visible es la **normalización**: los datos se
reparten en muchas tablas pequeñas sin repetir información. El nombre de un cliente
está en un sitio y sólo en uno; las líneas de ticket guardan un identificador que
apunta a él.

```
CLIENTES        (id, nombre, email, teléfono, id_población)
POBLACIONES     (id, nombre, cp, id_provincia)
PROVINCIAS      (id, nombre, id_comunidad)
TICKETS         (id, fecha, id_cliente, id_tienda, id_forma_pago)
LINEAS_TICKET   (id, id_ticket, id_articulo, unidades, precio, descuento)
ARTICULOS       (id, descripción, id_familia, id_marca, id_proveedor)
FAMILIAS        (id, nombre, id_seccion)
…
```

La normalización es excelente para operar: si un cliente cambia de teléfono se
modifica un único registro, y no hay forma de que queden dos versiones distintas.
Es pésima para analizar: responder «ventas por familia de producto y provincia en
el último trimestre» exige cruzar seis o siete tablas y recorrer millones de
líneas.

⚠️ Lanzar consultas analíticas contra la base de datos de producción es una mala
idea por dos motivos independientes. El primero es de rendimiento: una consulta que
recorre tres años de ventas bloquea recursos y ralentiza el cobro en caja. El
segundo es más sutil: el sistema operativo **guarda el estado actual, no el
histórico**. Si un artículo cambia de familia, todas las ventas pasadas empiezan a
contabilizarse en la familia nueva, y el informe del año pasado deja de coincidir
con el que se emitió el año pasado.

### 6.2. OLTP frente a OLAP

**OLAP** es *On-Line Analytical Processing*. Es la contrapartida: sistemas diseñados
para responder pocas preguntas muy grandes.

| | OLTP | OLAP |
|---|---|---|
| Propósito | Operar | Analizar y decidir |
| Operación típica | Insertar y actualizar registros | Leer y agregar millones de filas |
| Diseño | Normalizado, muchas tablas | Desnormalizado, modelo dimensional |
| Granularidad | El registro individual | Agregados y detalle histórico |
| Horizonte temporal | El estado actual | Años de historia |
| Volumen por consulta | Unas pocas filas | Millones de filas |
| Frecuencia de escritura | Continua | Por lotes, típicamente nocturna |
| Concurrencia | Cientos o miles de usuarios | Decenas de analistas |
| Criterio de éxito | Milisegundos por transacción | Segundos por consulta compleja |
| Si se cae | Se para el negocio | No se puede decidir, pero se opera |

> Son dos oficios distintos que exigen dos diseños distintos. Intentar que un solo
> sistema haga los dos bien es la razón por la que muchas organizaciones tienen una
> base de datos lenta para operar e insuficiente para analizar.

### 6.3. El almacén de datos

Un **Data Warehouse** o almacén de datos es un repositorio único donde se integra
la información procedente de todas las fuentes, preparada para el análisis.

La definición clásica de Bill Inmon lo caracteriza con cuatro propiedades:

**Integrado.** Los datos de todas las fuentes se unifican bajo criterios comunes:
mismos códigos, mismas unidades, mismos nombres. Es lo que resuelve el problema de
las cuatro definiciones de «venta» de Rueda Libre.

**Temático** (orientado a materias). Se organiza por áreas de negocio —ventas,
clientes, taller, stock— y no por la aplicación de la que procede el dato. A quien
analiza le da igual si la venta vino del TPV o de la web.

**No volátil.** Los datos no se modifican ni se borran. Se cargan y se conservan.
Un informe emitido hace dos años debe poder reproducirse hoy con el mismo
resultado.

**Histórico** (variante en el tiempo). Guarda la evolución, no sólo el estado
actual. Y guarda cómo eran las cosas *cuando ocurrieron*: si una tienda cambió de
región comercial en 2024, las ventas de 2023 siguen perteneciendo a la región
antigua.

**Ventajas concretas:**

- Una versión única de la verdad. Todos los informes parten del mismo dato.
- Rendimiento analítico, sin afectar a los sistemas de operación.
- Historia completa, incluso de fuentes que ya no existen.
- Reglas de negocio aplicadas una sola vez y en un solo sitio.
- Trazabilidad: se puede saber de dónde salió cada cifra.

**Y su coste, que también conviene decir:**

- Los datos no están al minuto: se cargan por lotes.
- Cualquier cambio en una fuente obliga a tocar el proceso de carga.
- Requiere mantenimiento continuo y alguien responsable.
- El esfuerzo inicial es alto y el retorno tarda meses en verse.

### 6.4. Datamarts

Un **datamart** es un subconjunto del almacén orientado a un área concreta:
ventas, taller, compras. Contiene sólo lo que ese departamento necesita, con el
nivel de detalle que ese departamento usa.

Se justifica por tres razones: rendimiento, simplicidad para el usuario final, y
control de acceso. Un responsable de tienda no necesita —ni debe— ver los costes
de proveedor.

Hay dos formas de llegar a ellos, que históricamente dieron lugar a un debate
considerable:

**Enfoque descendente** (Inmon). Primero se construye el almacén corporativo
completo y normalizado, y de él se derivan los datamarts. Más coherente y más
lento de rentabilizar.

**Enfoque ascendente** (Kimball). Se construyen primero los datamarts de las áreas
más urgentes, garantizando que comparten **dimensiones conformadas** —el mismo
cliente, el mismo producto, el mismo calendario—, y el almacén emerge de su unión.
Da resultados antes y exige más disciplina.

> En la práctica casi todos los proyectos actuales son ascendentes por necesidad:
> nadie financia dos años de construcción sin ver un cuadro de mando. Lo que no es
> negociable, sea cual sea el enfoque, es que las dimensiones estén conformadas. Un
> datamart de ventas y otro de taller que definan «cliente» de forma distinta son
> dos silos, no un almacén.

### 6.5. Las capas del almacén

La arquitectura moderna organiza el almacén en capas sucesivas de refinado. Los
nombres varían; la idea es siempre la misma:

```
FUENTES          TPV · web · taller · ERP · clientes · objetivos.xlsx
   ↓
STAGING          Copia en bruto, tal cual llegó. Sin transformar.
   ↓             Sirve para reprocesar sin volver a molestar a la fuente.
INTEGRACIÓN      Datos limpios, deduplicados, conformados, historificados.
   ↓             Aquí está la versión única de la verdad.
PRESENTACIÓN     Modelo dimensional: hechos y dimensiones, listo para consultar.
   ↓
CONSUMO          Cuadros de mando, informes, análisis, modelos
```

La capa de *staging* parece redundante y no lo es: permite reprocesar toda la
historia si se descubre un error en las reglas de transformación, sin depender de
que los sistemas de origen conserven el dato.

### 6.6. Historificación y dimensiones lentamente cambiantes

Un atributo descriptivo puede cambiar con el tiempo. Una tienda cambia de
responsable, un cliente cambia de segmento, un artículo cambia de familia. ¿Qué
debe pasar con la historia?

Las tres estrategias clásicas, conocidas como *Slowly Changing Dimensions*:

**Tipo 1 · Sobrescribir.** Se actualiza el valor y se pierde el anterior. Toda la
historia pasa a verse con el valor nuevo. Correcto para corregir errores —una
provincia mal escrita— e incorrecto para cambios reales.

**Tipo 2 · Versionar.** Se crea un registro nuevo con fechas de validez y se cierra
el anterior. Cada venta queda asociada a la versión que estaba vigente cuando
ocurrió.

```
id_tienda  nombre     región      desde        hasta        vigente
  17       Valencia   Levante     2019-01-01   2024-06-30     No
  17       Valencia   Este        2024-07-01   9999-12-31     Sí
```

Las ventas de 2023 siguen perteneciendo a Levante. Es lo correcto para la mayoría
de los cambios de negocio, y lo que hace que un informe antiguo se pueda
reproducir.

**Tipo 3 · Conservar el valor anterior en otra columna.** Permite comparar el
antes y el después, pero sólo guarda un cambio. De uso poco frecuente.

⚠️ La decisión sobre qué atributos se historifican y cuáles no es una decisión de
**negocio**, no técnica. Debe tomarla quien va a leer los informes, porque es quien
sabe si «las ventas de la región Este» deben incluir o no las que Valencia hizo
cuando pertenecía a Levante. Cambiarla más adelante obliga a reconstruir el
histórico.

### Ejercicios del capítulo 6

**6.1.** Rueda Libre reorganiza sus regiones comerciales en julio. El director
comercial quiere comparar la venta de la región Este de este año con la del año
pasado. ¿Qué tipo de dimensión lentamente cambiante necesita? ¿Y si lo que quisiera
es analizar la evolución con la estructura actual?

**6.2.** Un consultor propone conectar el cuadro de mando directamente al TPV «para
tener el dato en tiempo real y ahorrarse el almacén». Enumera cuatro problemas
concretos que aparecerían, al menos uno que no sea de rendimiento.

**6.3.** Explica con el ejemplo de Rueda Libre qué significa cada una de las cuatro
propiedades de Inmon. Usa un caso distinto para cada una.

**6.4.** El departamento de taller quiere su propio datamart y propone definir
«cliente» como quien trae la bicicleta, mientras que ventas lo define como quien
paga. ¿Qué problema causará y cómo lo resolverías?

**6.5.** ¿Para qué sirve la capa de *staging* si los datos luego se transforman de
todas formas? Da un caso concreto en el que salve un proyecto.

---

## 7. Procesos ETL

**ETL** son las tres operaciones que llevan el dato desde los sistemas de origen
hasta el almacén: **Extracción**, **Transformación** y **Carga**. Es la parte menos
vistosa de un proyecto de BI y la que consume la mayor parte del presupuesto.

### 7.1. Extracción

Consiste en obtener los datos de cada fuente. Las decisiones importantes:

**Qué se extrae cada vez.**

- *Carga completa*: se trae todo el conjunto en cada ejecución. Simple y robusto,
  inviable con volúmenes grandes.
- *Carga incremental*: sólo lo que ha cambiado desde la última ejecución. Eficiente
  y frágil: exige un criterio fiable de «lo que ha cambiado», normalmente una marca
  de tiempo de modificación. Si la fuente no la mantiene bien, se pierden registros
  silenciosamente.
- *Captura de cambios* (CDC): se leen los cambios directamente del registro de
  transacciones de la base de datos de origen. Es lo más preciso y lo más
  intrusivo.

**Cuándo se extrae.** La ventana de extracción suele ser nocturna, para no competir
con la operación. Eso fija la latencia del almacén: si la carga es a las tres de la
mañana, el cuadro de mando muestra los datos de ayer. Es una decisión de negocio
disfrazada de decisión técnica, y hay que hacerla explícita.

**Cómo se trata el fallo.** Toda extracción falla alguna vez: la fuente no responde,
el fichero no está, el formato cambió. El proceso debe poder repetirse sin duplicar
datos —lo que se llama ser *idempotente*— y debe avisar.

⚠️ El fallo más caro no es el que rompe el proceso: es el que lo deja terminar con
datos incompletos. Un cuadro de mando que muestra media jornada de ventas sin
advertirlo provoca decisiones erróneas con total confianza. Toda carga debe validar
volúmenes esperados y marcar el resultado como completo o parcial.

### 7.2. Transformación

Es la fase larga. Agrupa todo lo que hay que hacerle al dato para que sea
analizable. En el caso de Rueda Libre:

**Limpieza.** Corregir o marcar lo que está mal: fechas imposibles, importes
negativos donde no puede haberlos, campos obligatorios vacíos.

**Normalización de formatos.** Todo a un mismo criterio: fechas en formato único,
textos sin espacios sobrantes, mayúsculas y minúsculas homogéneas, teléfonos sin
separadores.

**Conversión de unidades.** Céntimos a euros, minutos a horas, divisas a euros con
el cambio de la fecha de la operación —no con el de hoy.

**Deduplicación.** Aplicar la regla de identidad y unificar los tres Manolos en uno,
conservando la trazabilidad de qué registros originales se fusionaron.

**Conformado de dimensiones.** Traducir los códigos de cada fuente a un catálogo
común:

```
TPV "BICICLETA"   ┐
Web "Bici"        ├─→  familia = BIC · «Bicicletas»
ERP "BIC"         ┘
```

Este paso es el que hace que el almacén sea un almacén y no un montón de tablas
juntas.

**Aplicación de reglas de negocio.** Aquí se decide, de una vez y para toda la
organización, qué significa cada concepto:

```
¿Una venta es el pedido o el cobro?
¿Las devoluciones restan de la venta del mes original o del mes de la devolución?
¿El descuento va antes o después de impuestos?
¿Un traspaso entre tiendas es una venta?
```

> Estas decisiones no las toma quien programa el proceso de carga. Las toma el
> negocio, y quedan documentadas. Es la única forma de que dos informes no den dos
> cifras distintas de la misma cosa.

**Cálculo de derivados.** Márgenes, duraciones, edades, indicadores intermedios. Se
calculan una vez en la carga en lugar de en cada consulta.

**Enriquecimiento.** Añadir información externa: población del municipio, datos
meteorológicos por fecha y tienda —muy relevante en un negocio de bicicletas—,
calendario de festivos.

### 7.3. Carga

Escribir el resultado en el almacén. Los puntos a decidir:

- **Orden.** Primero las dimensiones, después los hechos. Un hecho que apunta a una
  dimensión inexistente queda huérfano.
- **Gestión de la historia.** Aplicar la estrategia de dimensión lentamente
  cambiante que corresponda a cada atributo.
- **Huérfanos.** ¿Qué hacer con una venta cuyo artículo no está en el catálogo? La
  práctica habitual es asignarla a un miembro «desconocido» de la dimensión, nunca
  descartarla: si se descarta, los totales dejan de cuadrar y nadie sabe por qué.
- **Transaccionalidad.** O se carga todo el lote o no se carga nada. Un almacén con
  media carga aplicada es peor que uno con los datos de ayer.

### 7.4. ETL frente a ELT

El orden clásico es extraer, transformar y luego cargar: la transformación ocurre
en un servidor intermedio antes de escribir en el almacén.

En los últimos años se ha impuesto el orden **ELT**: extraer, cargar en bruto y
transformar **dentro** del propio almacén.

El motivo es económico y técnico: los almacenes actuales —columnares, distribuidos,
en la nube— tienen mucha más capacidad de cálculo que cualquier servidor intermedio,
y se paga por uso. Cargar primero y transformar después permite además reprocesar
toda la historia sin volver a las fuentes.

| | ETL | ELT |
|---|---|---|
| Dónde se transforma | Servidor intermedio | Dentro del almacén |
| Qué se carga | Sólo el dato ya limpio | Todo, también el bruto |
| Reprocesar | Hay que volver a la fuente | Se rehace en el almacén |
| Coste de almacenamiento | Menor | Mayor |
| Encaja con | Volúmenes moderados, infraestructura propia | Nube, grandes volúmenes |

⚠️ El cambio de ETL a ELT es un cambio de **dónde** se ejecuta la transformación, no
de **si** hace falta transformar. La idea de que con un *data lake* ya no hace falta
limpiar los datos es la causa directa de muchos proyectos que terminan en lo que el
sector llama, sin cariño, un *data swamp*: un depósito de ficheros que nadie sabe
interpretar.

### 7.5. Orquestación

Los procesos de carga tienen dependencias: el hecho de ventas no puede cargarse
antes que la dimensión de producto. Un orquestador define ese grafo, ejecuta cada
paso cuando le toca, reintenta lo que falla y avisa.

Lo que hay que dejar resuelto antes de la primera ejecución en producción:

- Qué pasa si una fuente no responde: ¿se para todo o se carga el resto?
- Cuánto tiempo puede tardar como máximo antes de considerarse un fallo.
- A quién se avisa, por qué canal y con qué información.
- Cómo se relanza una carga concreta sin rehacerlo todo.
- Cómo sabe quien lee el cuadro de mando que el dato de hoy es incompleto.

### 7.6. Gobierno del dato

El proceso técnico no basta. Hace falta una estructura de responsabilidad:

**Propietario del dato.** Una persona concreta, del negocio, responsable de cada
conjunto: quién decide qué es un cliente activo, quién autoriza cambios en el
catálogo de familias. Sin propietario, las decisiones las acaba tomando por defecto
quien programa la carga, que es quien menos contexto tiene.

**Diccionario de datos.** Para cada campo: qué significa, de dónde sale, cómo se
calcula, con qué frecuencia se actualiza, quién responde de él.

**Linaje.** La trazabilidad de cada cifra hasta su origen. Cuando alguien pregunta
«¿de dónde sale este 142.000?», debe haber respuesta.

**Definición única de KPI.** El catálogo de indicadores, con su fórmula exacta y su
propietario. Es lo que evita que comercial y finanzas lleven dos cifras de venta a
la misma reunión.

> Si en una reunión dos departamentos discuten sobre qué cifra es la buena, el
> problema no es de datos: es de gobierno. Y no lo arregla ninguna herramienta.

### Ejercicios del capítulo 7

**7.1.** El TPV de Bilbao estuvo tres días sin conexión y volcó sus ventas de golpe
el cuarto. ¿Qué habría pasado con una carga incremental basada en fecha de
modificación? ¿Y con una carga completa? ¿Qué control lo habría detectado?

**7.2.** Escribe la regla de negocio para las devoluciones en Rueda Libre. Considera
las dos opciones posibles, di cuál eliges, y explica qué informe cambia según la
elección.

**7.3.** Una venta llega con un código de artículo que no existe en el catálogo.
Enumera tres formas de tratarlo en la carga y las consecuencias de cada una en el
cuadro de mando.

**7.4.** Diseña el orden de carga de las tablas de Rueda Libre —cliente, tienda,
producto, fecha, ventas, reparaciones— y justifica las dependencias.

**7.5.** La dirección quiere el cuadro de mando «en tiempo real». La carga es
nocturna. Enumera tres preguntas que le harías antes de aceptar o rechazar el
requisito.

---

## 8. El modelo dimensional

El modelo es la decisión más duradera de un proyecto de BI. Determina qué preguntas
se van a poder hacer y, sobre todo, cuáles no.

### 8.1. Hechos y dimensiones

El modelo dimensional organiza los datos en dos tipos de tabla.

**Las tablas de hechos** guardan lo que ocurre: eventos medibles. Contienen
**métricas** —valores cuantitativos— y las claves que apuntan a las dimensiones.
Son las tablas grandes: millones de filas.

**Las tablas de dimensión** guardan el contexto: quién, qué, dónde, cuándo, cómo.
Contienen **atributos** descriptivos, casi siempre textos. Son tablas pequeñas y
anchas.

Y aquí está la conexión con el capítulo 2, que conviene subrayar:

> Las dimensiones son las variables medidas en escala **nominal y ordinal**. Los
> hechos son las variables medidas en escala **cuantitativa**.

Quien sepa determinar la escala de medida de cada columna tiene el modelo medio
diseñado. Un código postal es texto disfrazado de número: dimensión. Un importe
tiene unidad: hecho.

En Rueda Libre:

```
HECHO_VENTAS                      DIMENSIONES
  id_fecha        →  DIM_FECHA      fecha, mes, trimestre, año, festivo, temporada
  id_tienda       →  DIM_TIENDA     nombre, ciudad, provincia, región, m², apertura
  id_producto     →  DIM_PRODUCTO   descripción, familia, marca, gama
  id_cliente      →  DIM_CLIENTE    segmento, antigüedad, canal de captación
  id_forma_pago   →  DIM_PAGO       tipo, plazo
  ─────────────────────────────
  unidades                     ← métricas
  importe_bruto
  descuento
  importe_neto
  coste
  margen
```

### 8.2. La granularidad

Es **la decisión más importante del proyecto**, y hay que tomarla antes que
ninguna otra. La granularidad, o grano, es lo que representa una fila de la tabla
de hechos.

La prueba consiste en completar esta frase sin ambigüedad:

```
«Una fila de HECHO_VENTAS es …»
```

Opciones para Rueda Libre, de más fino a más grueso:

```
… una línea de ticket                    ← máximo detalle
… un ticket completo
… las ventas de un producto en una tienda y un día
… las ventas de una tienda y un día
… las ventas de una tienda y un mes      ← mínimo detalle
```

La regla es clara y se deduce directamente de la escalera de medida del capítulo 2:
**se elige siempre el grano más fino que sea viable**. Desde el detalle se puede
agregar en cualquier momento; desde el agregado no se puede desagregar nunca.

⚠️ Un almacén cargado al grano de «tienda y mes» no podrá responder jamás a «¿qué
productos se venden juntos?» ni a «¿a qué hora vendemos más?». No es un problema
que se arregle con una consulta: el dato ya no está. Y volver atrás significa
recargar toda la historia, si las fuentes todavía la conservan.

También hay que decidir el grano de las reparaciones, que es distinto:

```
«Una fila de HECHO_REPARACIONES es una orden de reparación cerrada»
```

Y ahí ya aparece una decisión de negocio: ¿y las que siguen abiertas? Si sólo se
cargan las cerradas, el tiempo medio de reparación estará sistemáticamente
subestimado, porque las que más tardan son precisamente las que aún no han cerrado.
Es un sesgo real, tiene nombre —sesgo de supervivencia— y se resuelve cargando
también las abiertas con su estado.

### 8.3. Estrella y copo de nieve

**Esquema en estrella.** Una tabla de hechos en el centro y las dimensiones
alrededor, cada una en una sola tabla, desnormalizada: la dimensión de tienda
contiene ciudad, provincia y región en columnas, aunque eso repita «Valencia» miles
de veces.

```
        DIM_FECHA     DIM_TIENDA
              \          /
               \        /
    DIM_CLIENTE ─ HECHO ─ DIM_PRODUCTO
               /        \
              /          \
        DIM_PAGO      DIM_EMPLEADO
```

**Esquema en copo de nieve.** Las dimensiones se normalizan en varias tablas
enlazadas: tienda → ciudad → provincia → región.

La estrella ahorra cruces y es mucho más comprensible para quien construye
informes. El copo ahorra espacio y evita algunas inconsistencias. En la práctica
**se prefiere la estrella** casi siempre: el espacio es barato y la comprensión del
modelo por parte del usuario final es lo que determina si el almacén se usa o se
abandona.

### 8.4. Jerarquías

Dentro de una dimensión, los atributos se organizan en niveles de detalle. Son las
jerarquías, y son las que hacen posible la navegación del análisis:

```
Geografía   Región → Provincia → Ciudad → Tienda
Producto    Sección → Familia → Marca → Artículo
Tiempo      Año → Trimestre → Mes → Semana → Día
Empresa     Dirección → Región comercial → Tienda → Empleado
```

Una dimensión puede tener varias jerarquías alternativas. El tiempo, por ejemplo,
tiene la natural (año–trimestre–mes–día) y la semanal (año–semana–día), que no
encajan una en otra: una semana puede pertenecer a dos meses.

### 8.5. La dimensión Fecha

Merece apartado propio porque está en todos los modelos y siempre se construye a
mano. Nunca se deriva de la fecha de la transacción sobre la marcha: se genera una
tabla con una fila por día, cubriendo todo el horizonte del almacén.

```
fecha       año  trim  mes  nombre_mes  semana  día_sem  laborable  festivo  temporada
2026-04-03  2026   2     4   Abril         14   Viernes     Sí        No     Alta
```

Lo que aporta es la posibilidad de responder preguntas que la fecha por sí sola no
permite: comparar el mismo día de la semana entre meses, excluir festivos, medir el
efecto de la Semana Santa —que cambia de mes cada año y descuadra cualquier
comparación mensual—, o segmentar por temporada, que en un negocio de bicicletas
explica más varianza que ninguna otra variable.

### 8.6. Aditividad de las métricas

Aquí está el contenido de este capítulo que evita más errores reales en cuadros de
mando de producción.

No todas las métricas se comportan igual al agregarlas.

**Aditivas.** Se pueden sumar por todas las dimensiones. Importe, unidades, coste,
margen en valor absoluto. Sumar las ventas de todas las tiendas de un mes da la
venta del mes, y eso es correcto.

**Semiaditivas.** Se pueden sumar por unas dimensiones y por otras no. El caso
típico es el **stock**: sumar el stock de todas las tiendas de un día da el stock
total de ese día, que es correcto. Sumar el stock de todos los días del mes da un
número sin ningún significado. Por la dimensión tiempo, el stock no se suma: se
promedia, o se toma el valor final del período.

**No aditivas.** No se pueden sumar por ninguna dimensión. Y aquí está el gran
grupo peligroso: **todos los ratios, porcentajes y medias**.

```
Margen %      Ticket medio      Tasa de conversión      Satisfacción media
```

> Un porcentaje no se suma. Y —esto es lo que casi nadie tiene presente— **un
> porcentaje tampoco se promedia**.

### 8.7. El error de la media de medias

Es el error número uno en cuadros de mando reales. Cinco tiendas de Rueda Libre:

```
Tienda      Ventas    Margen €   Margen %
Valencia   400.000     60.000      15,0 %
Zaragoza   100.000     18.000      18,0 %
Bilbao      50.000     10.000      20,0 %
Sevilla     30.000      6.600      22,0 %
Vigo        20.000      5.000      25,0 %
```

Si el cuadro de mando calcula el margen de la cadena promediando la columna de
porcentajes:

```
(15 + 18 + 20 + 22 + 25) / 5 = 20,0 %
```

Y el margen real de la cadena es:

```
99.600 / 600.000 = 16,6 %
```

**Casi cuatro puntos de diferencia.** Y no en un ejercicio de clase: en el
indicador que la dirección usa para decidir la política de precios de todo el año.

La causa es que el promedio simple trata igual a Valencia, que hace el 67 % de la
facturación, y a Vigo, que hace el 3 %. Las tiendas pequeñas, que casualmente son
las de mayor margen porcentual, arrastran el indicador hacia arriba.

**La solución correcta** es una de estas dos, que son equivalentes:

*Recalcular el ratio desde los totales* —siempre la mejor opción:

```
Margen % de la cadena  =  Σ margen €  /  Σ ventas €  =  16,6 %
```

*O usar una media ponderada*, cuando sólo se dispone de los porcentajes y los pesos:

```
Σ (margen%ᵢ × ventasᵢ) / Σ ventasᵢ
= (15×400.000 + 18×100.000 + 20×50.000 + 22×30.000 + 25×20.000) / 600.000
= 16,6 %
```

⚠️ La regla práctica, que conviene escribir en la pared del equipo de BI:

> **Los ratios no se agregan: se recalculan.** Se guardan en la tabla de hechos el
> numerador y el denominador por separado, y el ratio se calcula en el momento de
> la consulta, al nivel de agregación que se esté mostrando.

Es decir: en `HECHO_VENTAS` se guardan `importe` y `margen`, nunca `margen_%`. El
porcentaje se define como una medida calculada. Así, cualquiera que sea el filtro o
el nivel de detalle, el número sale bien.

El mismo error, con otras caras:

- Promediar el ticket medio de doce meses para obtener el ticket medio del año.
- Promediar tasas de conversión de campañas de tamaños muy distintos.
- Promediar la satisfacción media de tiendas con volúmenes de encuestas distintos.
- Promediar porcentajes de cumplimiento de objetivo de tiendas desiguales.

### 8.8. Cubos y operaciones OLAP

Un **cubo multidimensional** es la estructura que materializa el modelo dimensional
para consulta rápida: los hechos organizados por las dimensiones, con las
agregaciones precalculadas en cada nivel de cada jerarquía.

La metáfora del cubo viene de tres dimensiones —producto, tiempo y tienda— pero un
cubo real tiene tantas como haga falta. Cada celda contiene el valor de las métricas
para una combinación concreta.

Las operaciones que se hacen sobre un cubo, que son literalmente las que hace
cualquier usuario en un cuadro de mando:

**Drill-down.** Bajar un nivel en una jerarquía. De región a provincia, de provincia
a ciudad, de ciudad a tienda. Es la operación diagnóstica por excelencia: «las
ventas de la región Este han caído» → *drill-down* → «no ha caído la región: ha
caído una tienda».

**Drill-up** o *roll-up*. Subir un nivel. Agrega el detalle.

**Slice.** Fijar el valor de una dimensión y quedarse con la «rebanada»: sólo el
mes de abril.

**Dice.** Filtrar por varias dimensiones a la vez: bicicletas, en la región Este,
en el segundo trimestre.

**Pivot.** Girar el cubo: intercambiar qué dimensión va en filas y cuál en columnas.

> Estas cinco operaciones son todo lo que hace un usuario de negocio delante de un
> cuadro de mando. Que las pueda hacer con fluidez depende por completo de que las
> jerarquías estén bien definidas en el modelo.

### 8.9. ROLAP, MOLAP y HOLAP

Son tres formas históricas de implementar un cubo. Conviene conocerlas porque
aparecen en el temario y en la documentación de sistemas heredados, y porque
explican por qué las herramientas actuales son como son.

**MOLAP** (*Multidimensional OLAP*). El cubo se almacena en una estructura
multidimensional propia, con todas las agregaciones precalculadas. Consultas muy
rápidas, procesamiento largo, poca flexibilidad y límite de volumen.

**ROLAP** (*Relational OLAP*). No hay cubo físico: las consultas se traducen a SQL
contra el modelo en estrella de una base de datos relacional. Escala mucho mejor y
es más lento en consulta.

**HOLAP** (*Hybrid OLAP*). Combina ambos: los agregados en estructura
multidimensional y el detalle en la base relacional.

**Qué queda de todo esto hoy.** La distinción ha perdido casi toda su relevancia
práctica, por dos motivos:

1. Los **motores columnares en memoria** —la tecnología que hay debajo de Power BI,
   Tableau o Analysis Services tabular— comprimen y consultan tan rápido que la
   necesidad de precalcular agregaciones ha desaparecido en la mayoría de los casos.
2. Los **almacenes en la nube** escalan el cálculo bajo demanda, de forma que el
   enfoque relacional dejó de ser el lento.

> Es un buen ejemplo de cómo envejece la tecnología en BI: los tres acrónimos
> siguen en todos los temarios y la decisión que representaban ya casi no se toma.
> El concepto que sí sobrevive intacto es el de cubo como **modelo lógico**:
> hechos, dimensiones, jerarquías y agregaciones.

### Ejercicios del capítulo 8

**8.1.** Define el grano de `HECHO_REPARACIONES` para Rueda Libre. Justifícalo y di
qué pregunta dejaría de poder responderse si se eligiera un grano más grueso.

**8.2.** Clasifica cada métrica como aditiva, semiaditiva o no aditiva:
unidades vendidas · importe neto · stock a fin de día · margen % · ticket medio ·
número de clientes distintos · tiempo medio de reparación · coste de las piezas.
Presta especial atención al número de clientes distintos: ¿por qué es un caso
particular?

**8.3.** Con los datos del apartado 8.7, calcula el margen porcentual de la cadena
por los dos métodos y explica en una frase, apta para un comité de dirección, por
qué el 20 % es falso.

**8.4.** Diseña la dimensión `DIM_PRODUCTO` de Rueda Libre con al menos dos
jerarquías distintas. ¿Qué pregunta de negocio responde cada una?

**8.5.** El responsable de compras quiere analizar el stock por mes. Explica qué
pasa si la herramienta suma el stock diario y propón la agregación correcta.

**8.6.** La región comercial de una tienda cambia a mitad de año. Explica qué
mostraría el informe anual de ventas por región con una dimensión de tipo 1 y qué
mostraría con una de tipo 2. ¿Cuál es «la correcta»?

---
## 9. Analizar: relaciones y segmentación

Hasta aquí se ha trabajado con una variable cada vez. Eso es el análisis
univariante, y es imprescindible para entender la estructura de los datos y
detectar anomalías. Pero la información que cambia decisiones aparece cuando se
estudian las **relaciones entre variables**.

### 9.1. El análisis exploratorio

Antes de cualquier modelo, cualquier indicador y cualquier cuadro de mando, se mira
el dato. Es un paso obligatorio, no opcional, y consiste en algo tan poco
sofisticado como esto:

1. ¿Cuántas filas hay? ¿Cuántas se esperaban?
2. Para cada variable: escala de medida, valores distintos, nulos, mínimo y máximo.
3. Para cada variable cuantitativa: media, mediana, cuartiles, histograma.
4. Para cada variable cualitativa: tabla de frecuencias.
5. ¿Qué valores son imposibles? ¿Qué valores son sospechosos?
6. ¿La distribución tiene la forma que cabría esperar del negocio?

> El análisis exploratorio no busca respuestas: busca sorpresas. La mitad de los
> problemas de calidad de un proyecto de BI se detectan en este paso, y detectarlos
> aquí cuesta cien veces menos que detectarlos en el comité de dirección.

### 9.2. Segmentación

Segmentar es dividir el conjunto en grupos con comportamiento homogéneo y
analizarlos por separado. Es la técnica diagnóstica más rentable que existe, y casi
siempre es la respuesta correcta cuando un indicador global no cuadra.

En Rueda Libre, el ticket medio de la cadena es 128 €. Segmentando:

```
Segmento                    Tickets    Ticket medio
Accesorios y ropa            1.980         41 €
Componentes y taller           410        112 €
Bicicleta completa             310      1.020 €
```

El 128 € de la cadena no describe a **ningún** cliente real. Es el resultado
aritmético de mezclar tres negocios distintos. La distribución multimodal del
capítulo 4 era la señal de alarma; la segmentación es la respuesta.

Criterios de segmentación habituales: por producto, por canal, por geografía, por
tipo de cliente, por temporada, por tamaño de operación.

### 9.3. Comparar poblaciones equivalentes

La pregunta que abría el caso —«¿por qué Valencia vende más que Zaragoza si son del
mismo tamaño?»— contiene una trampa. «Del mismo tamaño» se refería a los metros
cuadrados de local. Pero:

```
                        Valencia    Zaragoza
Superficie                 220 m²      215 m²
Población del área      800.000     650.000
Años abierta                  12           3
Renta media del barrio     Alta       Media
Aparcamiento                  Sí          No
Carril bici en la calle       Sí          No
```

Las dos tiendas no son comparables en nada excepto en superficie. Cualquier
conclusión sobre la gestión de sus responsables extraída de la diferencia de ventas
es injusta y probablemente falsa.

> Antes de comparar dos cifras hay que preguntarse qué hace falta que sea igual
> para que la comparación signifique algo. Casi nunca se cumple del todo.

La solución práctica es **normalizar**: en lugar de comparar ventas absolutas,
comparar ventas por metro cuadrado, por habitante del área de influencia, o por
empleado. Cada normalización responde a una pregunta distinta y ninguna es
neutra.

### 9.4. La paradoja de Simpson

Es el caso extremo de lo anterior, y merece conocerse porque produce conclusiones
exactamente contrarias a la realidad.

Rueda Libre lanza una campaña de descuento en dos tiendas. Tasa de conversión de
visita a venta:

```
                    Valencia            Zaragoza            TOTAL
Con campaña      70/100 = 70 %       18/300 =  6 %      88/400 = 22 %
Sin campaña      27/300 =  9 %       80/100 = 80 %     107/400 = 27 %
```

Mirando el total, la campaña **empeora** la conversión: 22 % frente a 27 %. Mirando
cada tienda por separado, la campaña **mejora** la conversión en las dos: 70 % frente
a 9 % en Valencia, y 6 % frente a 80 %… no, ahí también empeora.

Rehagamos el ejemplo con números limpios, que es como se ve mejor:

```
                    Valencia            Zaragoza            TOTAL
Con campaña      63/70  = 90 %       10/50  = 20 %      73/120 = 61 %
Sin campaña      27/30  = 90 %       36/180 = 20 %      63/210 = 30 %
```

En cada tienda la conversión es **idéntica** con y sin campaña. Pero en el total, la
campaña parece duplicar la conversión. La causa es que la campaña se concentró en
Valencia, que convierte mucho mejor por razones que nada tienen que ver con la
campaña.

> La agregación puede invertir o inventar una relación. Cuando un total dice una
> cosa y los segmentos dicen otra, hay una variable oculta que está desequilibrando
> los grupos.

⚠️ Ésta es la razón por la que un cuadro de mando que sólo muestra totales es
peligroso: no permite detectar el problema. La capacidad de hacer *drill-down* no es
una comodidad, es un requisito de fiabilidad.

### 9.5. Correlación

La correlación mide en qué grado dos variables cuantitativas varían conjuntamente.
Se expresa con un coeficiente entre −1 y +1:

```
 +1    relación lineal positiva perfecta
 +0,7  relación positiva fuerte
 +0,3  relación positiva débil
  0    sin relación lineal
 −0,7  relación negativa fuerte
 −1    relación lineal negativa perfecta
```

En Rueda Libre, la correlación entre la temperatura media diaria y las ventas de
bicicleta es de +0,68: cuando sube la temperatura, sube la venta.

Tres advertencias que hay que dar siempre a la vez:

**Mide relación lineal.** Una correlación de 0 no significa que no haya relación:
significa que no hay relación *lineal*. La venta de bicicletas frente a la
temperatura no es lineal a partir de cierto punto: a 42 °C nadie sale a pedalear. Un
diagrama de dispersión enseña en un segundo lo que el coeficiente esconde.

**Es sensible a los atípicos.** Un solo Manolo puede crear o destruir una
correlación. Siempre se mira el diagrama de dispersión antes de creerse el número.

**Correlación no es causalidad.** Es la advertencia más repetida del análisis de
datos y la más ignorada en la práctica.

### 9.6. Correlación y causalidad

Que dos variables se muevan juntas admite cuatro explicaciones distintas, y sólo una
es la que la gente asume:

1. **A causa B.** La temperatura hace que se venda más bicicleta.
2. **B causa A.** La relación es la inversa de la que se suponía.
3. **Una tercera variable causa las dos.** Es la *variable de confusión*.
4. **Casualidad.** Con suficientes variables, siempre aparecen correlaciones altas
   sin ningún significado.

El caso 3 es el que más daño hace en la práctica. En Rueda Libre:

```
Observación:  las tiendas con más empleados venden más.
Conclusión:   contratemos más gente en todas las tiendas.
Realidad:     las tiendas con más población en su área tienen más empleados
              Y más ventas. La población es la variable de confusión.
```

Contratar dos empleados más en Vigo no aumentará sus ventas, porque no era la
plantilla lo que las causaba.

Para afirmar causalidad hacen falta tres condiciones, y ninguna la da la
correlación por sí sola:

- **Precedencia temporal**: la causa ocurre antes que el efecto.
- **Asociación**: cuando varía la causa, varía el efecto.
- **Ausencia de explicación alternativa**: se han descartado las variables de
  confusión.

La única forma limpia de establecer causalidad es el **experimento controlado**:
asignar al azar quién recibe el tratamiento y quién no. En negocio se llama prueba
A/B, y es perfectamente factible para una cadena de tiendas: aplicar la campaña en
seis tiendas elegidas al azar y no en las otras seis.

> Si una decisión importante depende de una relación causal, hay que hacer el
> experimento. Si no se puede hacer, hay que decir explícitamente que la relación
> es una hipótesis y no un hecho.

### Ejercicios del capítulo 9

**9.1.** El cuadro de mando muestra que el ticket medio de la cadena ha caído un
8 %. Diseña la secuencia de segmentaciones que harías, en orden, para averiguar qué
ha pasado. ¿Por qué en ese orden?

**9.2.** Se observa que las reparaciones que entran los lunes tardan más en
resolverse. Propón tres explicaciones distintas —una de cada tipo de los cuatro del
apartado 9.6— y di qué dato pedirías para distinguir entre ellas.

**9.3.** Un análisis encuentra correlación de +0,82 entre las ventas de ropa técnica
y las de cascos. ¿Se puede concluir que promocionar cascos aumentará la venta de
ropa? ¿Qué prueba diseñarías para averiguarlo?

**9.4.** Construye un ejemplo numérico propio de paradoja de Simpson con los datos
de Rueda Libre, distinto del del manual.

**9.5.** «Las tiendas que abren los domingos venden un 22 % más.» Enumera cuatro
razones por las que esa cifra podría no justificar abrir todas las tiendas los
domingos.

---

## 10. Minería de datos

### 10.1. Qué es y qué no es

La **minería de datos** es el conjunto de técnicas que buscan patrones en grandes
volúmenes de datos de forma automática o semiautomática.

La diferencia con todo lo anterior está en el punto de partida. En estadística
descriptiva y en reporting, la pregunta la formula una persona y los datos
responden. En minería de datos, es el algoritmo el que propone la pregunta: busca
estructura que nadie había pedido.

Lo que no es:

- No es magia: sobre datos malos produce patrones malos, con más convicción.
- No sustituye al conocimiento del negocio: un patrón sin interpretación es ruido.
- No es lo mismo que aprendizaje automático, aunque comparte casi todas las
  técnicas. La minería de datos es la disciplina orientada a **descubrir**
  conocimiento; el aprendizaje automático, la familia de algoritmos que se usa para
  ello y también para **predecir**.

> La minería de datos encuentra patrones. Decidir cuáles de esos patrones son
> conocimiento y cuáles casualidad sigue siendo trabajo de una persona que entienda
> el negocio.

### 10.2. Las categorías

Cada técnica resuelve un tipo de problema distinto. Lo importante en este curso no
es saber implementarlas, sino saber **cuál pedir** ante un problema de negocio.

**Clasificación.** Asignar cada individuo a una categoría conocida de antemano. Es
aprendizaje *supervisado*: se necesitan ejemplos ya etiquetados.

```
Rueda Libre:  ¿Este cliente va a darse de baja en los próximos tres meses?
              ¿Esta reparación va a superar el plazo comprometido?
              ¿Este pedido web es fraudulento?
```

**Regresión.** Igual que la clasificación, pero la salida es un número en lugar de
una categoría.

```
Rueda Libre:  ¿Cuántas unidades de este modelo venderemos el mes que viene?
              ¿Cuánto tardará esta reparación?
              ¿Cuál es el valor esperado de este cliente a tres años?
```

**Agrupamiento** (*clustering*). Descubrir grupos naturales sin saber de antemano
cuáles son ni cuántos. Es aprendizaje *no supervisado*.

```
Rueda Libre:  ¿Qué tipos de cliente existen realmente en nuestra base?
```

Es especialmente valioso porque suele contradecir la segmentación que la empresa
creía tener. Rueda Libre segmentaba por importe de compra; el agrupamiento reveló
que los grupos con comportamiento homogéneo eran otros: el ciclista de competición,
el cliente urbano de trayecto diario, el comprador de regalo y el aficionado de fin
de semana. Cuatro grupos con necesidades, márgenes y estacionalidades distintas.

**Reglas de asociación.** Encontrar qué cosas ocurren juntas. Es el clásico análisis
de la cesta de la compra.

```
Rueda Libre:  «Quien compra una bicicleta de montaña compra casco en el 62 % de
               los casos, pero guantes sólo en el 11 %.»
```

De ahí salen decisiones concretas de colocación en tienda, de recomendación en la
web y de paquetes de venta.

**Detección de anomalías.** Identificar lo que se sale del patrón. Es el capítulo 5
convertido en algoritmo y aplicado en continuo.

```
Rueda Libre:  Un TPV que registra devoluciones fuera de lo habitual.
              Una carga nocturna con un volumen anómalo.
              Un sensor de la web con un pico de tráfico no humano.
```

**Análisis de secuencias.** Como las reglas de asociación, pero incorporando el
orden temporal: qué compra un cliente *después* de qué.

### 10.3. El proceso de minería de datos

Las fases, en orden:

1. **Selección.** Decidir qué datos entran en el análisis y cuáles no.
2. **Preparación.** Limpiar, tratar nulos y atípicos, unificar formatos.
3. **Transformación.** Derivar variables nuevas, normalizar escalas, codificar
   variables cualitativas, reducir dimensiones.
4. **Modelado.** Aplicar el algoritmo y ajustar sus parámetros.
5. **Evaluación.** Medir si el modelo funciona sobre datos que no ha visto, e
   interpretar si el patrón tiene sentido de negocio.
6. **Despliegue.** Poner el resultado en producción, donde alguien lo use.

⚠️ Las fases 1 a 3 se llevan entre el 60 % y el 80 % del tiempo total. La fase 4,
la única que la gente asocia con la minería de datos, suele ser la más corta.

### 10.4. CRISP-DM

Es la metodología estándar del sector. Añade a lo anterior dos cosas
imprescindibles: empieza por el negocio y es **cíclica**.

```
        ┌──────────────────────┐
        │ 1 Comprensión del    │
        │   NEGOCIO            │◄──────────┐
        └──────────┬───────────┘           │
                   ↓                       │
        ┌──────────────────────┐           │
        │ 2 Comprensión de     │           │
        │   los DATOS          │           │
        └──────────┬───────────┘           │
                   ↓                       │
        ┌──────────────────────┐           │
        │ 3 PREPARACIÓN        │◄────┐     │
        └──────────┬───────────┘     │     │
                   ↓                 │     │
        ┌──────────────────────┐     │     │
        │ 4 MODELADO           │─────┘     │
        └──────────┬───────────┘           │
                   ↓                       │
        ┌──────────────────────┐           │
        │ 5 EVALUACIÓN         │───────────┘
        └──────────┬───────────┘
                   ↓
        ┌──────────────────────┐
        │ 6 DESPLIEGUE         │
        └──────────────────────┘
```

Dos rasgos que la hacen útil:

- **Empieza por el negocio, no por los datos.** La primera fase no consiste en
  mirar tablas: consiste en averiguar qué decisión hay que mejorar y cómo se sabrá
  si el proyecto ha servido para algo.
- **Los bucles son la norma.** De modelado se vuelve a preparación constantemente,
  y de evaluación se vuelve al negocio cuando resulta que el problema estaba mal
  planteado. Un proyecto de minería que avanza en línea recta es un proyecto que no
  está aprendiendo nada.

### 10.5. Sobreajuste y validación

Un modelo puede aprenderse los datos de memoria en lugar de aprender el patrón. Eso
es el **sobreajuste**: funciona perfectamente sobre los datos con los que se
construyó e inútilmente sobre datos nuevos.

La defensa mínima es separar los datos:

```
ENTRENAMIENTO  (70 %)   Se construye el modelo
VALIDACIÓN     (15 %)   Se ajustan los parámetros
PRUEBA         (15 %)   Se mide el resultado final. Se usa UNA vez.
```

Y en series temporales, la separación **no puede ser aleatoria**: hay que entrenar
con el pasado y validar con el futuro. Entrenar un modelo de demanda con datos de
diciembre para predecir noviembre es hacer trampa, y da resultados excelentes que no
sobreviven a la producción.

> Un modelo con un 99 % de acierto es casi siempre una mala noticia: o hay
> sobreajuste, o se ha colado en las variables de entrada alguna que contiene
> implícitamente la respuesta.

El segundo caso tiene nombre —*fuga de información*— y es un error frecuente.
Predecir si una reparación se va a retrasar usando como variable la fecha de salida
real da un acierto perfecto y no sirve para nada: en el momento en que hay que
predecir, esa fecha todavía no existe.

⚠️ El acierto global tampoco basta cuando las clases están desequilibradas. Si sólo
el 2 % de los clientes se da de baja, un modelo que prediga «nadie se va a dar de
baja» acierta el 98 % de las veces y es completamente inútil. Hay que mirar cuántas
bajas reales detecta y cuántas falsas alarmas genera.

### Ejercicios del capítulo 10

**10.1.** Para cada necesidad de Rueda Libre, indica qué categoría de minería de
datos corresponde:
(a) saber qué clientes están a punto de dejar de comprar;
(b) descubrir qué tipos de cliente hay realmente;
(c) saber qué productos conviene colocar juntos;
(d) estimar la venta de la próxima temporada;
(e) detectar un TPV que está registrando operaciones raras.

**10.2.** Un modelo predice con un 96 % de acierto qué reparaciones se retrasarán.
Al revisar las variables se descubre que una de ellas es «número de veces que se ha
llamado al cliente para avisar del retraso». ¿Qué está pasando? ¿Cómo se llama?

**10.3.** El agrupamiento de clientes de Rueda Libre devuelve cuatro grupos.
¿Cómo decidirías si esos grupos son útiles o son un artefacto del algoritmo? Da dos
criterios: uno estadístico y uno de negocio.

**10.4.** Explica por qué en un modelo de predicción de demanda no se puede repartir
la muestra al azar entre entrenamiento y prueba. Propón la alternativa correcta.

**10.5.** La dirección quiere empezar un proyecto de minería de datos. Aplicando
CRISP-DM, escribe las tres preguntas que harías en la fase 1 antes de mirar un solo
dato.

---

## 11. Indicadores, informes y cuadros de mando

Todo lo anterior sirve para llegar hasta aquí: el momento en que el análisis se
convierte en algo que alguien mira y con lo que alguien decide.

### 11.1. Métrica y KPI

No son sinónimos, aunque se usen como tales.

Una **métrica** es cualquier cosa que se puede medir. Un **KPI** (*Key Performance
Indicator*) es una métrica que mide el avance hacia un objetivo y que **cambia una
decisión**.

La prueba es sencilla y despiadada:

> Si el indicador sube o baja, ¿alguien hace algo distinto? Si la respuesta es no,
> no es un KPI: es decoración.

En un cuadro de mando típico, entre el 50 % y el 70 % de lo que hay no pasa esta
prueba. «Número de visitas a la web» es una métrica; «tasa de conversión de visita a
venta» puede ser un KPI, si alguien tiene capacidad de actuar sobre ella.

**Indicadores accionables frente a métricas decorativas.** Un indicador accionable
cumple cuatro condiciones: alguien es responsable de él, alguien puede influir en
él, hay un objetivo o una referencia, y existe una acción prevista si se desvía.

### 11.2. La definición operativa

Un KPI no está definido hasta que están escritas estas siete cosas:

```
Nombre           Ticket medio
Pregunta         ¿Cuánto gasta un cliente por operación?
Fórmula          Importe neto total / número de tickets
Fuente           HECHO_VENTAS, capa de presentación
Grano            Un ticket. Excluye traspasos entre tiendas.
Filtros          Excluye devoluciones y ventas a empleados
Período          Mes natural, por fecha de cobro
Responsable      Dirección comercial
```

Sin esto, el mismo nombre significa cosas distintas en departamentos distintos. En
Rueda Libre, «ventas» significaba cuatro cosas:

```
Comercial    Importe con IVA, en el momento del pedido
Finanzas     Importe sin IVA, en el momento del cobro
Tienda       Todo lo que pasa por caja, traspasos incluidos
Web          Pedidos confirmados, incluidos los que luego se cancelan
```

Las cuatro son razonables. Ninguna es la buena. Lo que no es aceptable es que
convivan sin que nadie lo sepa: dos personas llegan a la misma reunión con dos
cifras y la discusión pasa de qué hacer a quién tiene razón.

> La tarea más rentable de un proyecto de BI no es técnica: es conseguir que la
> organización acuerde una definición por indicador y la escriba.

### 11.3. Absolutos, relativos, ratios y tasas

Un **indicador absoluto** cuenta magnitud: 142.000 € de venta. Un **indicador
relativo** la pone en contexto: 108 % del objetivo, +12 % interanual, 645 € por m².

Se necesitan los dos. El absoluto dice el tamaño; el relativo, si es bueno.

**Los ratios y las tasas son donde se miente**, casi siempre en el denominador.

```
Tasa de conversión = ventas / visitas
```

Parece incontrovertible. Pero: ¿visitas o visitantes únicos? ¿Se cuentan los robots?
¿Y quien entra a consultar el horario? ¿Y quien compra por teléfono después de
visitar la web, en qué canal cuenta?

Cambiando la definición del denominador, la misma tasa pasa del 2 % al 7 % sin que
haya cambiado nada en el negocio. Por eso la definición operativa incluye la fuente
y los filtros: no es burocracia.

⚠️ Dos avisos concretos sobre porcentajes:

**Base pequeña.** «La tienda de Vigo ha mejorado su conversión un 200 %» suena
espectacular hasta que se ve que pasó de 1 venta a 3. Un porcentaje sin su valor
absoluto al lado es una cifra incompleta.

**Puntos porcentuales frente a porcentaje.** Pasar del 15 % al 18 % de margen es una
subida de **3 puntos porcentuales**, o del **20 %** en términos relativos. Las dos
frases son correctas y describen lo mismo; usarlas indistintamente confunde a todo
el mundo, y elegir la más favorable es una forma habitual de exagerar un resultado.

### 11.4. La media de medias, otra vez

El capítulo 8 explicó por qué los ratios no se agregan. Aquí es donde el error
aparece: en el cuadro de mando.

Los síntomas de que está ocurriendo:

- El total de la fila «Cadena» no coincide con lo que sale al calcularlo a mano.
- El indicador cambia al aplicar un filtro que no debería afectarle.
- El valor de la cadena está fuera del rango de los valores de las tiendas —
  imposible en una media ponderada, habitual en una media simple mal hecha.

La solución, ya vista, es guardar numerador y denominador y definir el ratio como
medida calculada. Es una decisión de modelado que se toma en el capítulo 8 y cuyo
efecto se ve aquí.

Otros casos del mismo error, todos frecuentes:

```
Ticket medio del año     ≠  media de los doce ticket medio mensuales
Conversión de la campaña ≠  media de las conversiones por tienda
Satisfacción global      ≠  media de las satisfacciones por tienda
Cumplimiento de la cadena ≠ media de los cumplimientos por tienda
```

### 11.5. El contexto

Un número solo no significa nada. «142.000 €» no es bueno ni malo.

Un indicador necesita al menos una referencia, y preferiblemente varias:

- **Objetivo.** ¿Cuánto se esperaba?
- **Período anterior.** ¿Cómo va respecto al mes pasado?
- **Mismo período del año anterior.** Imprescindible en negocios estacionales, y en
  bicicletas lo es de forma extrema: comparar julio con junio no dice nada.
- **Referencia externa.** ¿Cómo va el sector?
- **Distribución.** ¿Cómo se reparte entre tiendas, productos o clientes?

Y la regla que enlaza con todo el bloque estadístico:

> Ningún indicador de tendencia central se publica sin su dispersión. En un cuadro
> de mando, eso significa que junto a la media va la mediana, un percentil alto, o
> el rango entre el mejor y el peor.

En Rueda Libre, la tarjeta del tiempo de reparación quedó así:

```
┌──────────────────────────────────────────┐
│  TIEMPO DE REPARACIÓN            ▲ 0,3 d │
│                                          │
│  Mediana        1,1 días                 │
│  P90            8,7 días    obj. < 7     │
│  Media          3,2 días                 │
│                                          │
│  ▁▃▇█▆▃▂▁▁ ·      ·        ·             │
│  distribución · 3 % supera 3 días        │
└──────────────────────────────────────────┘
```

Tres cifras y una distribución, en el espacio que ocupaba un solo número. Y ahora la
tarjeta permite decidir: el problema no es el tiempo típico, es la cola.

### 11.6. Informes, consultas y alertas

Además del cuadro de mando, un sistema de BI entrega información de otras tres
formas, y cada una tiene su momento.

**Informes.** Documentos estructurados, con periodicidad fija y destinatarios
definidos. Siguen siendo la respuesta correcta cuando hay que dejar constancia,
cuando el destinatario no va a entrar en ninguna herramienta, o cuando el contenido
es regulatorio. Se distingue entre informes *operativos* —el listado de
reparaciones pendientes— e informes *analíticos* —el cierre mensual comentado.

**Consultas.** El acceso directo a los datos para responder preguntas no previstas.
Tres modelos:

- *Ad hoc por el equipo de BI*: control total, cuello de botella garantizado.
- *Autoservicio libre*: rapidez y caos; cada usuario construye su versión de la
  verdad.
- *Autoservicio gobernado*: los usuarios construyen sus análisis, pero sobre un
  modelo semántico común con las métricas ya definidas. Es la opción recomendable, y
  la que hace que la definición operativa del apartado 11.2 sea rentable.

**Alertas.** Avisos automáticos cuando un indicador cruza un umbral. Requisitos para
que funcionen:

- Umbral justificado, no elegido a ojo. Un umbral estadístico —más de 2σ sobre la
  media histórica— envejece mucho mejor que un número fijo.
- Destinatario con capacidad de actuar.
- **Acción prevista.** Una alerta sin acción asociada es ruido.
- Volumen controlado. La *fatiga de alertas* es real: un sistema que avisa veinte
  veces al día deja de leerse en una semana, y entonces también se pierde la que
  importaba.

### 11.7. Diseñar un cuadro de mando

Un cuadro de mando no es una colección de gráficos. Las preguntas a responder
**antes** de abrir la herramienta:

1. ¿Quién lo va a mirar? Un cuadro sirve a una audiencia, no a tres.
2. ¿Con qué frecuencia?
3. ¿Qué decisión permite tomar?
4. ¿Qué hace esa persona si el indicador se pone en rojo?
5. ¿Qué es lo primero que tiene que ver al abrirlo?

De la audiencia sale el nivel, volviendo a la pirámide del capítulo 1:

| | Estratégico | Táctico | Operativo |
|---|---|---|---|
| Audiencia | Dirección | Responsable de tienda | Encargado de turno |
| Horizonte | Años y trimestres | Meses y semanas | Hoy |
| Indicadores | 5 a 8 | 10 a 15 | Los que caben en una pantalla |
| Detalle | Muy agregado | Agregado con drill-down | Registro individual |
| Actualización | Mensual | Diaria | Continua |

**Principios de composición:**

- **Resumen arriba, detalle abajo.** Lo primero que se ve responde a «¿va bien o
  mal?». El detalle está para quien pregunta por qué.
- **Máximo tres niveles de lectura**: estado, evolución, desglose.
- **Codificar el estado en la forma, no sólo en el número.** Un color, un símbolo, una
  banda. Debe leerse sin leer.
- **La navegación en profundidad no es un extra.** Es lo que evita la paradoja de
  Simpson.
- **Todo indicador lleva su definición accesible.** Un icono de información con la
  fórmula, la fuente y el período.

### 11.8. Visualización honesta

**Elegir el gráfico según la escala de medida**, que es el cierre del capítulo 2:

| Qué se quiere mostrar | Gráfico |
|---|---|
| Comparar categorías | Barras |
| Composición de un todo | Barras apiladas al 100 % · sectores si son pocas |
| Evolución en el tiempo | Líneas |
| Distribución de una variable continua | Histograma · caja y bigotes |
| Relación entre dos cuantitativas | Dispersión |
| Comparar distribuciones entre grupos | Cajas múltiples · violines |
| Un valor contra su objetivo | Barra con marcador de objetivo |

**Las trampas más habituales**, que conviene saber detectar tanto en los cuadros
propios como en los ajenos:

*Eje truncado.* Empezar el eje vertical en 95 en lugar de en 0 convierte una
variación del 2 % en un precipicio. En gráficos de barras el eje debe empezar en
cero, porque la longitud de la barra codifica la magnitud. En gráficos de líneas,
donde lo que importa es la variación, truncar es aceptable si se indica.

*Doble eje.* Dos series con escalas distintas en el mismo gráfico: moviendo las
escalas se puede hacer que parezcan correlacionadas o independientes a voluntad.

*Sectores con demasiadas categorías.* Ocho porciones son ilegibles. Y el efecto 3D
distorsiona las áreas: la porción de delante siempre parece mayor.

*Escala temporal irregular.* Puntos no equiespaciados en el eje X que sugieren
tendencias inexistentes.

*Exceso de color.* Si todo está coloreado, el color deja de significar nada. El
color se reserva para codificar información: estado, categoría, atención.

*Agregación que oculta.* Un total que esconde una caída fuerte en un segmento. Es la
razón del drill-down.

> Un cuadro de mando visualmente atractivo y conceptualmente falso es más peligroso
> que una hoja de cálculo fea: se cree más y se cuestiona menos.

### Ejercicios del capítulo 11

**11.1.** Escribe la definición operativa completa —las siete líneas— del indicador
«tasa de devolución» de Rueda Libre. Toma las decisiones que hagan falta y déjalas
explícitas.

**11.2.** El cuadro de mando muestra «Margen de la cadena: 20 %». Las tiendas van del
15 % al 25 %. Explica cómo detectarías si está mal calculado, sin acceso a los datos
de detalle.

**11.3.** Diseña la tarjeta del indicador «ticket medio» siguiendo el modelo del
apartado 11.5. ¿Qué tres cifras pondrías y por qué?

**11.4.** De estos seis indicadores, señala cuáles son KPI y cuáles decoración,
justificando con la prueba del apartado 11.1: número de visitas a la web · tasa de
conversión · seguidores en redes · margen por familia · número de referencias en
catálogo · porcentaje de reparaciones fuera de plazo.

**11.5.** Un gráfico de barras muestra las ventas mensuales con el eje vertical
empezando en 130.000. La variación real entre el mejor y el peor mes es del 6 %.
Describe qué impresión da el gráfico y cómo lo corregirías.

**11.6.** Diseña una alerta útil para el jefe de taller: qué mide, con qué umbral,
a quién avisa y qué acción dispara. Después explica por qué el umbral que has
elegido es mejor que un número fijo.

---

## 12. Series temporales y pronósticos

### 12.1. Describir no es predecir

Son dos oficios distintos.

| | Descripción | Predicción |
|---|---|---|
| Pregunta | ¿Qué ha pasado? | ¿Qué va a pasar? |
| Datos | Los observados | Los observados, para inferir los futuros |
| Validación | Que el cálculo sea correcto | Que acierte sobre datos no vistos |
| Resultado | Un hecho | Una estimación con incertidumbre |
| Si se equivoca | Estaba mal el dato | Puede estar todo bien y aun así fallar |

> Un pronóstico sin margen de error no es un pronóstico: es una opinión con
> decimales.

### 12.2. Los componentes de una serie temporal

Una serie temporal —una variable medida a lo largo del tiempo— se descompone
conceptualmente en cuatro partes:

**Tendencia.** El movimiento de fondo a largo plazo. Rueda Libre crece un 4 % anual.

**Estacionalidad.** El patrón que se repite con período fijo y conocido. En
bicicletas es brutal: abril a septiembre concentra el 68 % de la venta anual. Hay
estacionalidad a varias escalas: anual, mensual (los días de cobro), semanal (sábado
es el mejor día) y hasta diaria.

**Ciclo.** Oscilaciones de período largo e irregular, típicamente ligadas a la
economía. Se distingue de la estacionalidad en que su duración no es fija.

**Ruido.** Lo que queda. Variación aleatoria sin patrón.

```
Serie observada  =  Tendencia + Estacionalidad + Ciclo + Ruido
```

La utilidad práctica de la descomposición es inmediata: permite responder a «¿hemos
crecido de verdad, o es que era verano?».

### 12.3. Comparar períodos

Es el punto donde más se equivocan los cuadros de mando reales.

```
«Las ventas han caído un 31 % respecto al mes pasado»
```

Si el mes pasado era agosto y éste es septiembre, en una cadena de bicicletas esa
frase no aporta ninguna información: la caída es estacional y ocurre todos los años.

Las formas correctas de comparar, cada una con su uso:

**Interanual** (mismo mes del año anterior). Elimina la estacionalidad de golpe. Es
la comparación por defecto en negocios estacionales.

```
Septiembre 2026 frente a septiembre 2025:  +6 %   ← esto sí informa
```

⚠️ Con un aviso: si el año anterior fue anómalo, la comparación hereda la anomalía.
Comparar contra un mes en que la tienda estuvo cerrada por obras produce
crecimientos espectaculares y falsos.

**Acumulado del año.** Suaviza el ruido mensual y mide el avance hacia el objetivo
anual.

**Media móvil.** El promedio de los últimos *n* períodos. Con n = 12 en datos
mensuales elimina la estacionalidad y deja ver la tendencia limpia. Es la
herramienta más simple y más útil de todo el capítulo.

**Frente a objetivo.** La comparación que más le importa a quien tiene que rendir
cuentas.

### 12.4. Señal y ruido

El error operativo más caro no es de cálculo: es reaccionar a cada punto del
gráfico.

Una serie con ruido sube y baja continuamente sin que nada haya cambiado. Si cada
subida provoca una felicitación y cada bajada una reunión de urgencia, la
organización se agota persiguiendo variaciones aleatorias.

La forma de distinguir:

- Mirar la **variabilidad histórica**. Si la serie oscila habitualmente ±8 % y este
  mes ha bajado un 5 %, no ha pasado nada.
- Exigir **persistencia**. Un punto no es una tendencia. Tres períodos consecutivos
  en la misma dirección empiezan a serlo.
- Usar **límites de control**: media histórica ± 2σ. Dentro de esa banda, es ruido;
  fuera, merece investigación. Es el mismo criterio del capítulo 5, aplicado al
  tiempo.

> Antes de explicar por qué ha bajado un indicador, hay que demostrar que ha bajado.

### 12.5. Modelos sencillos de pronóstico

En orden creciente de sofisticación, y todos comprensibles sin matemáticas:

**Ingenuo.** El próximo período será igual que el último. Parece una broma y es la
referencia obligatoria: cualquier modelo que no supere al ingenuo no aporta nada.

**Ingenuo estacional.** El próximo julio será como el julio pasado. En negocios
estacionales es sorprendentemente difícil de batir.

**Media móvil.** El próximo período será el promedio de los últimos *n*. Suaviza el
ruido y va con retraso ante los cambios reales.

**Suavizado exponencial.** Como la media móvil, pero dando más peso a lo reciente.
Sus variantes incorporan tendencia y estacionalidad; es lo que usa la mayoría de los
sistemas de previsión de demanda.

**Regresión sobre la tendencia.** Ajustar una recta —o una curva— a la serie y
prolongarla.

⚠️ Sobre la regresión, dos avisos. Primero: **extrapolar fuera del rango observado
es una apuesta**, no un cálculo; una recta ajustada sobre tres años de crecimiento
predice crecimiento indefinido, y ningún negocio crece indefinidamente. Segundo:
ajustar bien el pasado no garantiza nada sobre el futuro. Un polinomio de grado alto
puede pasar exactamente por todos los puntos históricos y dar predicciones absurdas
al período siguiente.

### 12.6. La incertidumbre

Un pronóstico se comunica siempre con tres elementos:

```
Valor esperado    Ventas de julio: 148.000 €
Intervalo         Entre 131.000 y 165.000 € (80 % de confianza)
Supuestos         Sin apertura de tiendas nuevas · Sin cambios de precio
                  · Meteorología dentro de lo normal
```

El intervalo no es una debilidad del modelo: es información. Un rango estrecho
permite comprometer stock; uno ancho obliga a preparar dos escenarios.

Y hay una regla que conviene decir siempre: **la incertidumbre crece con el
horizonte**. Un pronóstico a un mes puede ser útil; a tres años, con la misma
metodología, es un ejercicio de ficción. Cuando alguien pida una previsión a cinco
años, lo honesto es entregar escenarios —optimista, central, pesimista— y no una
cifra.

### 12.7. Tamaño de muestra y representatividad

Un porcentaje calculado sobre pocos casos no es un porcentaje: es una anécdota.

```
La nueva página de producto convierte al 100 %.   ← sobre 3 visitas
La tienda de Vigo tiene 5 estrellas de media.     ← sobre 2 reseñas
El nuevo modelo se devuelve el 50 % de las veces. ← se han vendido 2
```

Tres reglas prácticas para un cuadro de mando:

1. Mostrar siempre el tamaño de la muestra junto al porcentaje.
2. Definir un mínimo por debajo del cual el indicador no se publica —se muestra
   «datos insuficientes».
3. Desconfiar de los extremos. En cualquier ranking, los primeros y los últimos
   puestos suelen estar ocupados por los grupos más pequeños, simplemente porque
   varían más.

### 12.8. Riesgos del pronóstico automatizado

**Degradación del modelo.** Un modelo entrenado con datos de hace dos años deja de
ser válido cuando el negocio cambia. Hay que medir su acierto en continuo y
reentrenarlo.

**Cambios de régimen.** Una pandemia, un cambio de precios, la apertura de un
competidor. Ningún modelo entrenado con el pasado los anticipa, y todos siguen
prediciendo con total confianza después de que ocurran.

**Realimentación.** Si el pronóstico determina el pedido de stock, y el stock
determina la venta, el modelo acaba prediciendo su propia decisión y no la demanda.
Es un bucle difícil de detectar: los indicadores de acierto salen excelentes.

**Automatizar una conclusión incorrecta.** Es el riesgo mayor y conecta con todo el
manual. Un error de definición de indicador ejecutado una vez es un error. El mismo
error dentro de un proceso automático se ejecuta cada noche, alimenta decisiones
cada día, y nadie lo revisa porque ya funcionaba.

> La automatización no mejora la calidad de un análisis. Multiplica lo que haya:
> si el razonamiento es bueno, lo escala; si es malo, lo industrializa.

### Ejercicios del capítulo 12

**12.1.** Las ventas de Rueda Libre en octubre caen un 28 % respecto a septiembre.
Enumera las tres comprobaciones que harías, en orden, antes de escribir una sola
línea de explicación.

**12.2.** Una misma serie se presenta de cuatro maneras: mensual, interanual,
acumulada del año y con media móvil de 12 meses. Explica qué pregunta responde cada
una y cuál usarías ante el comité de dirección.

**12.3.** Diseña el pronóstico de venta de cascos para el próximo trimestre usando
el método ingenuo estacional. ¿Qué datos necesitas? ¿Contra qué compararías el
resultado de un modelo más sofisticado?

**12.4.** Redacta el pronóstico de ventas de julio para la dirección, con los tres
elementos del apartado 12.6. Inventa las cifras pero sé coherente.

**12.5.** El cuadro de mando muestra un ranking de tiendas por satisfacción media.
Los tres primeros puestos son las tres tiendas más pequeñas. ¿Es casualidad? ¿Cómo
lo comprobarías? ¿Cómo cambiarías el cuadro de mando?

**12.6.** Un modelo de previsión de demanda lleva ocho meses funcionando y su
acierto ha bajado del 88 % al 61 %. Enumera cuatro causas posibles y cómo
distinguirlas.

---

## 13. La gestión de un proyecto de BI

### 13.1. Por qué no es un proyecto de desarrollo

Un proyecto de BI tiene tres rasgos que lo hacen distinto y que explican por qué las
planificaciones clásicas fallan sistemáticamente:

**El alcance se descubre trabajando.** Nadie sabe qué contiene realmente una fuente
de datos hasta que la abre. Un proyecto de BI que promete alcance cerrado antes de
haber perfilado los datos está prometiendo algo que no puede saber.

**La mayor parte del esfuerzo está en los datos, no en la herramienta.** Construir
el cuadro de mando es la parte rápida. Conseguir que las cifras sean correctas es la
parte larga.

**El valor no está en el entregable, está en el uso.** Un cuadro de mando terminado
que nadie abre es un proyecto fracasado, aunque cumpliera plazo y presupuesto.

> Los proyectos de BI no fracasan por tecnología. Fracasan por preguntas mal
> planteadas y por definiciones que nadie acordó.

### 13.2. Las fases

```
 1  Pregunta de negocio          ¿Qué decisión hay que mejorar?
 2  Audiencia e interlocutores   ¿Quién decide y quién sabe?
 3  Inventario de fuentes        ¿Qué hay y dónde está?
 4  Perfilado y calidad          ¿Se puede confiar en ello?
 5  Propietarios del dato        ¿Quién responde de cada cosa?
 6  Definición de KPIs           ¿Qué significa exactamente cada indicador?
 7  Diseño del modelo            Grano, hechos, dimensiones, jerarquías
 8  Construcción                 ETL, almacén, modelo semántico
 9  Validación con usuarios      ¿Coincide con lo que ellos saben?
10  Puesta en producción         Orquestación, permisos, formación
11  Evolución                    Medir el uso y responder a lo que surja
```

Dos observaciones sobre el orden:

- **Las fases 1 a 6 no son preliminares: son el proyecto.** Si están bien hechas, la
  8 es mecánica. Si están mal hechas, la 8 no las arregla.
- **La 9 no es una formalidad.** La validación consiste en enseñarle al responsable
  de tienda su propia cifra y ver si la reconoce. Si no la reconoce, hay un problema
  —de dato, de definición o de expectativa— y hay que resolverlo antes de publicar.

### 13.3. Los roles

| Rol | Quién es | De qué responde |
|---|---|---|
| Patrocinador | Dirección | Que el proyecto exista y tenga prioridad |
| Propietario del dato | Negocio | Qué significa cada dato y quién accede |
| Usuario clave | Negocio | Validar que las cifras son correctas |
| Analista de BI | Técnico | Modelo, indicadores, cuadros de mando |
| Ingeniero de datos | Técnico | Extracción, transformación, carga |
| Responsable de sistemas | TI | Acceso a las fuentes, infraestructura |

Qué pasa cuando falta cada uno:

- Sin patrocinador, el proyecto pierde prioridad ante cualquier urgencia operativa.
- Sin propietario del dato, las decisiones de negocio las toma quien programa.
- Sin usuario clave, se construye algo correcto que nadie reconoce como suyo.
- Sin acceso a las fuentes, todo se retrasa y nadie sabe a quién reclamar.

### 13.4. Planificación

**La estimación realista.** El reparto habitual del esfuerzo:

```
Análisis, definición y acuerdo de KPIs    20 %
Perfilado y calidad de los datos          25 %
Desarrollo de los procesos de carga       30 %
Modelo y cuadros de mando                 15 %
Validación, formación y ajustes           10 %
```

Un plan que dedique la mitad del tiempo a construir cuadros de mando está mal hecho.

**Las dependencias críticas casi nunca son técnicas.** Son de acceso: permisos para
leer una base de datos, disponibilidad de la persona que conoce el sistema antiguo,
decisión pendiente sobre qué significa «cliente activo». Conviene identificarlas al
principio y hacerlas visibles, porque son las que paran el proyecto.

**El criterio de terminado.** Debe estar escrito antes de empezar. No es «el cuadro
de mando está publicado», sino algo como: *el responsable de cada tienda consulta su
cuadro al menos una vez por semana y las cifras coinciden con su cierre de caja*.

**Iterar.** La primera entrega debe ser pequeña, completa de punta a punta y útil:
una sola área, tres o cuatro indicadores, una fuente. Es preferible un cuadro de
mando de ventas funcionando en seis semanas que un almacén corporativo completo en
dieciocho meses.

### 13.5. Los diez riesgos típicos

| Riesgo | Cómo se manifiesta | Mitigación |
|---|---|---|
| Mala calidad del dato | Las cifras no cuadran con lo que sabe el negocio | Perfilar antes de planificar; validación en la carga |
| Definiciones divergentes de un KPI | Dos áreas llegan con dos cifras | Catálogo de indicadores con propietario |
| Fuentes incompatibles | Los cruces multiplican filas o pierden registros | Conformar dimensiones; declarar el grano |
| Datos incompletos | Períodos o entidades sin dato | Hacer visible la ausencia, nunca imputar en silencio |
| Sesgos de recogida | Sólo hay datos de una parte de la realidad | Documentar qué no se está midiendo |
| Falta de contexto | Cifras sin objetivo ni comparación | Ningún indicador sin referencia |
| Indicadores mal diseñados | Ratios agregados incorrectamente | Guardar numerador y denominador |
| Correlación tomada por causalidad | Se decide sobre una relación no verificada | Exigir experimento o declarar hipótesis |
| Automatizar una conclusión errónea | El error se repite cada noche | Revisión periódica de definiciones |
| Cuadro de mando que nadie usa | Uso nulo tras el entusiasmo inicial | Medir el uso; validar con usuarios reales |

El último riesgo merece un comentario: **hay que medir el uso del cuadro de mando**.
Es un dato que casi nadie recoge y que dice más sobre el éxito del proyecto que
cualquier otro. Si el uso cae, hay que averiguar por qué antes de construir nada
nuevo.

### 13.6. Evolución

Un sistema de BI no se entrega: se mantiene. Lo que hay que prever desde el
principio:

- Qué pasa cuando una fuente cambia de estructura.
- Quién autoriza un indicador nuevo y con qué criterio.
- Cada cuánto se revisan las definiciones existentes.
- Cómo se retiran los indicadores que ya nadie usa.
- Cómo se gestiona el crecimiento del volumen.

> Un catálogo de indicadores que sólo crece termina siendo inservible. Retirar
> indicadores es tan importante como añadirlos.

### Ejercicios del capítulo 13

**13.1.** Escribe la pregunta de negocio inicial del proyecto de Rueda Libre en un
formato que permita saber si el proyecto ha tenido éxito. Debe contener una
decisión, un responsable y una forma de medirlo.

**13.2.** Elabora el inventario de fuentes de Rueda Libre indicando, para cada una:
propietario probable, grano, riesgo de calidad principal y dependencia crítica.

**13.3.** Un proyecto planifica ocho semanas: una de análisis, una de datos, cinco
de cuadros de mando y una de validación. Critica el reparto y propón otro.

**13.4.** Define el criterio de «terminado» para la primera entrega del proyecto de
Rueda Libre. Debe ser verificable por alguien que no sea del equipo técnico.

**13.5.** Elige tres de los diez riesgos de la tabla y diseña, para cada uno, una
comprobación automática que lo detecte antes de que llegue al comité de dirección.

---
# Parte II · El caso Rueda Libre, paso a paso

Esta parte no explica conceptos nuevos. Reconstruye el proyecto en el orden en que
ocurrió: qué se hizo, qué problema resolvía, qué se rompió y qué costó. Es la parte
que da sentido a la primera.

---

## 14. Versión 1 — El informe mensual en hoja de cálculo

### 14.1. El punto de partida

Antes del proyecto, Rueda Libre tenía un informe. Lo hacía Gertru el primer lunes de
cada mes y le llevaba dos días completos:

1. Exportar las ventas del TPV de cada una de las doce tiendas, una a una.
2. Exportar los pedidos de la tienda en línea.
3. Pegarlo todo en una hoja de cálculo.
4. Buscar y corregir a mano las familias de producto que no coincidían.
5. Cruzar con `Objetivos.xlsx` mediante búsquedas verticales.
6. Generar seis gráficos.
7. Enviarlo por correo a dirección y a los doce responsables de tienda.

### 14.2. Qué funcionaba

Conviene reconocerlo antes de criticarlo, porque explica por qué había durado tres
años: el informe **respondía a una pregunta real** y Gertru conocía el negocio. Los
ajustes manuales que hacía —descartar los traspasos entre tiendas, corregir las
familias— eran conocimiento de negocio aplicado correctamente.

### 14.3. Qué no funcionaba

**Dos días al mes de una persona.** Veinticuatro días al año dedicados a copiar y
pegar.

**No era reproducible.** El informe de marzo no se podía volver a generar en junio,
porque el TPV sobrescribe los datos antiguos al cerrar el ejercicio.

**El conocimiento estaba en una persona.** Las reglas —qué se descarta, cómo se
homogeneizan las familias— no estaban escritas en ninguna parte. Estaban en la
cabeza de Gertru.

**No permitía preguntar.** El informe respondía siempre a las mismas seis preguntas.
Cualquier pregunta nueva costaba otro día de trabajo.

**Las cifras no cuadraban con las de finanzas.** Nadie sabía por qué. Se descubrió
en la versión 2: Gertru usaba la fecha del ticket y finanzas la fecha del cobro.

### 14.4. El diagnóstico

Aplicando el capítulo 13: el problema no era la herramienta. Una hoja de cálculo es
perfectamente capaz de generar ese informe. El problema era que **la lógica de
negocio estaba en un proceso manual y no documentado**, y que **no había historia**.

> La primera decisión del proyecto no fue comprar nada. Fue escribir, con Gertru,
> las quince reglas que ella aplicaba a mano.

Ese documento de quince reglas se convirtió en la especificación del proceso de
carga, y es el entregable más valioso de toda la versión 1.

---

## 15. Versión 2 — Unificar las fuentes

### 15.1. El encargo

Que las cifras cuadren. Que haya una sola definición de venta. Que el informe se
genere solo.

### 15.2. El inventario de fuentes

Primera tarea, y la que reveló el verdadero alcance:

| Fuente | Grano | Historia | Riesgo principal |
|---|---|---|---|
| TPV (×12) | Línea de ticket | 18 meses | Importes en céntimos; se purga al cerrar el año |
| Tienda en línea | Pedido | Completa | Catálogo propio; estados de pedido |
| Gestor de taller | Orden de reparación | Completa | Fechas nulas en órdenes abiertas |
| Fichero de clientes | Cliente | Completa | Duplicados masivos |
| ERP de compras | Albarán | 5 años | Vuelca el coste con un mes de retraso |
| Objetivos.xlsx | Tienda y mes | 3 años | Manual; se modifica después del cierre |

Dos hallazgos que cambiaron la planificación:

**El TPV purga los datos al cerrar el ejercicio.** Sólo había 18 meses de historia
disponible. La primera acción urgente del proyecto fue empezar a copiar el TPV a la
capa de *staging* **antes** de tener nada más construido, para no perder más
historia mientras se diseñaba el resto.

**El grano del ERP no coincidía con el del TPV.** El coste llegaba por albarán de
proveedor, no por línea de venta. Calcular el margen por ticket exigía una regla de
imputación de coste que nadie había definido nunca.

### 15.3. Las cuatro definiciones de venta

El problema que impedía que las cifras cuadraran. Se convocó una reunión con
comercial, finanzas, tiendas y web, y se llegó a un acuerdo:

```
VENTA NETA  (indicador oficial de la compañía)

  Importe sin IVA
  En la fecha del COBRO
  Excluye traspasos entre tiendas
  Excluye ventas a empleados
  Las devoluciones restan en el mes de la devolución
```

Se conservaron además dos indicadores secundarios —*venta bruta* y *pedidos
confirmados*— con nombres distintos, para que comercial y web pudieran seguir
usando su cifra sin llamarla «ventas».

> Ese acuerdo tardó tres semanas en cerrarse y fue la parte más difícil del
> proyecto entero. No tuvo ningún componente técnico.

### 15.4. La deduplicación de clientes

El fichero tenía 41.200 registros. Aplicando una regla de identidad basada en correo
electrónico normalizado y teléfono normalizado, quedaron 28.700 clientes reales.

El efecto sobre los indicadores fue considerable:

```
                         Antes      Después
Número de clientes      41.200      28.700
Venta media por cliente   84 €       121 €
Clientes recurrentes       12 %        31 %
```

El negocio no había cambiado. Lo único que había cambiado era que ahora se estaba
midiendo bien. El porcentaje de clientes recurrentes, que la dirección consideraba
un problema estratégico, resultó ser casi el triple de lo que se creía.

⚠️ Este es el mejor ejemplo del capítulo 3 en todo el caso: **un problema de calidad
del dato se había convertido en un diagnóstico erróneo de negocio**, y se habían
tomado decisiones comerciales sobre él durante dos años.

### 15.5. Lo que se construyó

```
FUENTES  →  STAGING  →  INTEGRACIÓN  →  Informe automatizado
```

Sin modelo dimensional todavía. Una capa de integración con las reglas aplicadas y
el mismo informe de siempre, generado automáticamente cada noche.

### 15.6. El balance de la V2

**Se ganó:** cifras que cuadran; dos días al mes liberados; reglas documentadas;
historia que empieza a acumularse; el hallazgo de los clientes recurrentes.

**Se pagó:** nueve semanas de trabajo, seis de ellas en calidad de datos y
definiciones. Cero en herramienta de visualización.

**Quedó mal:** el informe seguía respondiendo a seis preguntas fijas. Nadie podía
preguntar nada nuevo. Y el margen seguía sin poder calcularse por ticket.

---

## 16. Versión 3 — El modelo dimensional

### 16.1. Por qué cambiar

La dirección hizo una pregunta que la V2 no podía responder: «¿qué familias de
producto tiran del margen en las tiendas grandes y cuáles en las pequeñas?».

No era una limitación de la herramienta. Era que los datos estaban organizados como
un informe y no como un modelo.

### 16.2. La decisión del grano

Se dedicó una sesión entera a completar la frase, y hubo discusión:

```
«Una fila de HECHO_VENTAS es una LÍNEA de ticket.»
```

La alternativa propuesta —una fila por ticket— habría reducido el volumen a la
quinta parte y era tentadora. Se descartó porque impedía para siempre el análisis
por producto, que era exactamente la pregunta que había originado la versión.

Para el taller:

```
«Una fila de HECHO_REPARACIONES es una orden de reparación,
 esté cerrada o abierta, con su estado.»
```

La decisión de incluir las abiertas fue de Gertru, y evitó el sesgo de
supervivencia: si sólo se cargaban las cerradas, el tiempo medio de reparación
excluía sistemáticamente a las que más tardaban, que son las que siguen abiertas.
El indicador habría estado permanentemente subestimado.

### 16.3. El modelo

```
                    DIM_FECHA
                        │
     DIM_CLIENTE ── HECHO_VENTAS ── DIM_PRODUCTO
                        │
                    DIM_TIENDA ──────┐
                        │            │
                  HECHO_REPARACIONES │
                        │            │
                   DIM_EMPLEADO      │
                                DIM_FORMA_PAGO
```

Dimensiones conformadas: `DIM_FECHA`, `DIM_TIENDA`, `DIM_CLIENTE` y `DIM_PRODUCTO`
son las mismas para los dos hechos. Eso es lo que permite preguntar «¿los clientes
que usan el taller compran más?», que cruza los dos mundos.

`DIM_TIENDA` se implementó como dimensión de **tipo 2**, previendo la
reorganización de regiones comerciales que efectivamente ocurrió seis meses después.

`DIM_FECHA` se generó a mano, con festivos por provincia y una columna `temporada`
—alta de abril a septiembre, baja el resto— que resultó ser la variable más
explicativa de todo el modelo.

### 16.4. La decisión sobre los ratios

Se estableció una regla de modelado que no se ha roto desde entonces:

> En las tablas de hechos sólo se guardan **numeradores y denominadores**. Ningún
> porcentaje, ningún promedio, ningún ratio se almacena.

```
HECHO_VENTAS guarda:      importe_neto · coste · unidades · num_tickets
NO guarda:                margen_% · ticket_medio
```

Margen porcentual y ticket medio se definieron como medidas calculadas en el modelo
semántico. La consecuencia práctica es que salen bien a cualquier nivel de
agregación y con cualquier filtro, automáticamente.

Fue esta decisión la que destapó el error del margen: el informe de la V2 daba un
margen de cadena del 20 % promediando los porcentajes de las doce tiendas. El
margen real era del 16,6 %.

⚠️ Casi cuatro puntos de error en el indicador con el que se había fijado la
política de precios del año anterior.

### 16.5. Lo que se rompió

La carga de reparaciones falló durante dos semanas sin que nadie se diera cuenta.
Causa: Manolo teclea a mano el modelo de bicicleta cuando no lo encuentra en el
desplegable, y una de sus entradas contenía un carácter que rompía el proceso de
conformado. El proceso terminaba sin error y sin cargar la mitad de las órdenes.

De ahí salió la regla de validación de volumen: toda carga compara el número de
registros con la media de las últimas cuatro ejecuciones y marca el resultado como
incompleto si se desvía más de 2σ. Es el capítulo 5 aplicado a la operación.

---

## 17. Versión 4 — El cuadro de mando

### 17.1. Tres cuadros, no uno

La primera propuesta fue un único cuadro de mando para todos. Se descartó aplicando
la pirámide del capítulo 1, y se construyeron tres:

**Operativo · encargado de turno.** Reparaciones pendientes, las que superan el
plazo comprometido, stock por debajo del mínimo. Actualización continua, detalle al
registro individual.

**Táctico · responsable de tienda.** Venta frente a objetivo, ticket medio con su
distribución, margen por familia, tiempo de reparación con mediana y P90,
comparación interanual. Actualización diaria, con navegación en profundidad hasta el
ticket.

**Estratégico · dirección.** Seis indicadores: venta neta, margen, clientes
recurrentes, ticket medio, satisfacción y tiempo de reparación. Cada uno con
objetivo, interanual y distribución entre tiendas. Actualización mensual.

### 17.2. La tarjeta que cambió la conversación

El indicador de tiempo de reparación se publicaba como un solo número: 3,2 días de
media. La dirección llevaba un año presionando al taller sin resultado.

Al publicarlo con dispersión:

```
Mediana        1,1 días
P90            8,7 días      objetivo: < 7
Media          3,2 días
3 % de las reparaciones supera los 3 días
```

Quedó claro de inmediato que **no había un problema de tiempo típico**: nueve de
cada diez clientes recogían su bicicleta al día siguiente. Había un problema de
cola, concentrado en el 3 % de las órdenes: las restauraciones completas de Manolo.

Y ese 3 % no era un fallo de proceso: era una línea de negocio distinta, con un
margen tres veces superior. La decisión que se tomó no fue apretar al taller, fue
**separar el flujo**: dos circuitos, dos compromisos de plazo, dos indicadores.

> Un año de presión sobre el taller se había basado en confundir la media con el
> colectivo. La solución no requirió ninguna tecnología: requirió publicar tres
> cifras en lugar de una.

### 17.3. La auditoría del cuadro de mando

Antes de publicar se pasó el cuadro por una revisión sistemática. Lo que se
encontró:

| Problema encontrado | Corrección |
|---|---|
| Margen de cadena como media de porcentajes | Recalcular desde totales |
| Gráfico de ventas con eje empezando en 130.000 | Eje desde cero |
| «Satisfacción media 4,3» sin distribución | Añadir % de valoraciones 1 y 2 |
| Ranking de tiendas por conversión sin volumen | Añadir tamaño de muestra; ocultar bajo 30 casos |
| Comparación con el mes anterior en un negocio estacional | Sustituir por interanual |
| Sectores con nueve familias de producto | Barras horizontales ordenadas |
| Diez indicadores sin objetivo | Definir objetivo o retirar |

Siete correcciones. Ninguna era un fallo técnico: todas eran errores conceptuales
sobre un cuadro que funcionaba perfectamente.

---

## 18. Versión 5 — Del descriptivo al predictivo

### 18.1. Por qué ahora

Sólo cuando el descriptivo era fiable —dos años de historia limpia, definiciones
acordadas, modelo estable— tuvo sentido subir el siguiente escalón de la escalera
del capítulo 1.

### 18.2. La descomposición de la serie

Antes de predecir nada, se descompuso la serie de ventas:

```
Tendencia         +4,1 % anual
Estacionalidad    Abril–septiembre concentra el 68 % de la venta
                  Sábado es el mejor día (2,3× la media semanal)
Ciclo             No identificable con dos años de historia
Ruido             ±7 % mensual
```

El último dato fue el más útil de los cuatro y no requirió ningún modelo: **la serie
oscila habitualmente un ±7 %**. Eso significa que las reuniones de urgencia por
caídas del 5 % llevaban dos años convocándose por ruido.

### 18.3. El pronóstico

Se construyeron tres modelos y se compararon todos contra el ingenuo estacional:

| Modelo | Error medio |
|---|---|
| Ingenuo (mes anterior) | 31 % |
| Ingenuo estacional (mismo mes año anterior) | 9,4 % |
| Media móvil de 12 meses | 14 % |
| Suavizado exponencial con estacionalidad | 6,8 % |

El suavizado exponencial ganó, pero por poco margen sobre el ingenuo estacional. La
conclusión que se llevó a dirección fue honesta: *el modelo sofisticado aporta 2,6
puntos de precisión sobre mirar el mismo mes del año pasado*. Con esa información,
la dirección decidió adoptarlo igualmente, pero sabiendo lo que compraba.

La presentación del pronóstico se hizo siempre con los tres elementos:

```
Julio:  148.000 €   ·   intervalo 131.000 – 165.000 (80 %)
Supuestos: sin aperturas · sin cambios de precio · meteorología normal
```

### 18.4. La segmentación de clientes

Se aplicó agrupamiento sobre la base de clientes. La empresa segmentaba por importe
de compra —bronce, plata, oro—. El algoritmo encontró cuatro grupos distintos:

```
Ciclista de competición   Alto gasto, alta frecuencia, alto uso de taller, estacional
Urbano de trayecto        Gasto medio, muy recurrente, poco estacional
Comprador de regalo       Compra única, alto importe, diciembre y junio
Aficionado de fin de semana  Gasto medio-bajo, estacional extremo
```

El grupo urbano de trayecto era el más rentable a tres años y el que la
segmentación anterior no distinguía en absoluto: quedaba repartido entre bronce y
plata. Sobre ese hallazgo se construyó la campaña del año siguiente.

### 18.5. El límite

Se planteó automatizar el pedido de stock a partir del pronóstico. Se descartó, por
el riesgo de realimentación del capítulo 12: si el pronóstico determina el pedido y
el pedido determina la venta —porque no se puede vender lo que no está en la
tienda—, el modelo acabaría prediciendo su propia decisión.

La solución fue mantener el pronóstico como **propuesta** revisada por el
responsable de compras, y medir aparte la demanda perdida por rotura de stock.

---

## 19. Balance de la evolución

### 19.1. Lo que costó cada salto

| Versión | Duración | Esfuerzo principal |
|---|---|---|
| V2 · Unificar fuentes | 9 semanas | 6 en calidad de datos y definiciones |
| V3 · Modelo dimensional | 6 semanas | 2 sólo en decidir el grano |
| V4 · Cuadros de mando | 4 semanas | 1 en la auditoría previa a publicar |
| V5 · Predictivo | 5 semanas | 3 en validar que el descriptivo aguantaba |

Veinticuatro semanas en total. **De ellas, catorce se dedicaron a datos y
definiciones, y cuatro a construir cuadros de mando.** Es el reparto normal, y es el
que casi ninguna planificación inicial contempla.

### 19.2. Lo que se reutilizó

Cada versión se apoyó en la anterior sin rehacerla:

- Las quince reglas escritas con Gertru en la V1 siguen siendo la especificación del
  proceso de carga.
- La capa de *staging* de la V2 permitió reconstruir todo el histórico cuando en la
  V3 cambió la regla de imputación de coste. Sin ella habrían sido dos años perdidos.
- Las dimensiones conformadas de la V3 permitieron añadir el hecho de reparaciones
  sin tocar nada de ventas.
- El modelo semántico con ratios calculados hizo que los tres cuadros de la V4
  dieran las mismas cifras sin ningún esfuerzo adicional.

### 19.3. Lo que no está bien

Un balance honesto incluye lo que quedó pendiente:

**El coste sigue imputándose por una regla aproximada.** El ERP no da coste por
línea de venta y la regla de reparto por albarán es una estimación. El margen por
producto individual es orientativo, y así está marcado en el cuadro de mando.

**Objetivos.xlsx sigue siendo un fichero manual.** Se añadió una validación que
detecta modificaciones posteriores al cierre, pero la fuente sigue dependiendo de
una persona.

**No hay medición de la demanda perdida.** No se sabe cuántas ventas se pierden por
rotura de stock, lo que sesga a la baja todos los análisis de demanda.

**El uso del cuadro de mando operativo es bajo.** Cuatro de las doce tiendas apenas
lo abren. Está pendiente averiguar si es un problema de diseño, de formación o de que
resuelve algo que no era un problema.

### 19.4. Las cinco lecciones

1. **El proyecto no empezó cuando se compró la herramienta.** Empezó cuando se
   escribieron las reglas que Gertru aplicaba a mano.

2. **El hallazgo más valioso fue de calidad de datos, no de análisis.** Los clientes
   recurrentes eran el triple de lo que se creía porque el fichero tenía duplicados.
   Ninguna técnica avanzada habría encontrado eso.

3. **El error más caro fue conceptual, no técnico.** Promediar porcentajes de margen
   desvió la política de precios de un año entero, sobre un sistema que funcionaba
   perfectamente.

4. **Publicar la dispersión cambió una decisión de negocio.** Un año de presión sobre
   el taller se resolvió sustituyendo un número por tres.

5. **Lo más difícil no tuvo componente técnico.** Fueron las tres semanas de reuniones
   hasta acordar qué significa «venta».

> Business Intelligence no consiste en hacer gráficos. Consiste en transformar datos
> en evidencia para tomar decisiones. Todo lo demás es infraestructura.

### Ejercicios de la Parte II

**II.1.** La V2 descubrió que había 28.700 clientes reales y no 41.200. Enumera tres
indicadores del cuadro de mando anterior que estaban mal y explica en qué dirección
se equivocaban.

**II.2.** Se decidió cargar las reparaciones abiertas además de las cerradas.
Explica con un ejemplo numérico cómo habría quedado sesgado el tiempo medio si sólo
se hubieran cargado las cerradas.

**II.3.** El suavizado exponencial mejora el error del 9,4 % al 6,8 % frente al
ingenuo estacional. Escribe el argumento que llevarías a dirección a favor de
adoptarlo, y el que llevarías en contra. ¿Cuál defiendes?

**II.4.** Diseña la validación automática que habría detectado el fallo de carga de
reparaciones provocado por la entrada manual de Manolo.

**II.5.** Cuatro de las doce tiendas no usan el cuadro de mando operativo. Diseña el
plan para averiguar por qué, con al menos tres fuentes de información distintas.

**II.6.** Toma la lección 3 —el error del margen promediado— y escribe el correo de
dos párrafos que enviarías a dirección explicando qué pasó, sin culpar a nadie y sin
minimizar el impacto.

---
# Apéndices

---

## Apéndice A · El temario oficial, punto por punto

Este apéndice relaciona cada punto del programa formativo oficial **ADGG102PO ·
Business Intelligence** con el capítulo de este manual donde se estudia y con el
punto del caso Rueda Libre que lo ilustra. Sirve para dos cosas: comprobar la
cobertura del temario y localizar rápidamente el ejemplo de cualquier concepto.

### Bloque 1 · Inteligencia de negocios

| Punto del temario | Capítulo | Dónde se ve en el caso |
|---|---|---|
| **1.1** · Introducción | 1.1 – 1.3 | Las cinco mil nóminas · El informe manual de la V1 |
| **1.2** · La pirámide organizacional | 1.4 | Los tres cuadros de mando de la V4 (17.1) |
| **1.3** · Herramientas de inteligencia de negocios | 1.6, 11.6 | Familias de herramientas; decisión deliberada de no empezar por ahí |
| **1.4** · Fundamentos del Datawarehouse | 6.3 | La capa de integración de la V2 |
| **1.5** · Características | 6.3 | Las cuatro propiedades de Inmon aplicadas a Rueda Libre |
| **1.6** · Ventajas | 6.3 | Cifras que cuadran; historia que el TPV purgaba |
| **1.7** · Sistemas OLTP | 6.1, 6.2 | TPV, gestor de taller, ERP y sus límites |
| **1.8** · Implementación del Datawarehouse | 6.5, 6.6, 8 | Capas, *staging* e historificación de `DIM_TIENDA` |
| **1.9** · Análisis OLAP: Drill Down, Drill Up | 8.8 | Navegación región → tienda en el cuadro táctico |
| **1.10** · Servidores OLAP: ROLAP, MOLAP, HOLAP | 8.9 | Tratamiento histórico y qué sobrevive hoy |
| **1.10** · Minería de datos, definiciones de Data Mining | 10.1 | Qué es y qué no es |
| **1.11** · Categorías de Data Mining | 10.2 | Las seis categorías con su caso de negocio |
| **1.12** · Proceso de Minería de Datos | 10.3 | Las seis fases |
| **1.13** · Metodología | 10.4, 13.2 | CRISP-DM y las once fases del proyecto |
| **1.14** · Reportes | 11.6 | El informe mensual de la V1 y su automatización |
| **1.15** · Consultas | 11.6 | Autoservicio gobernado sobre el modelo semántico |
| **1.16** · Alertas | 11.6 | Umbral estadístico de la validación de carga (16.5) |
| **1.17** · Análisis | 5, 9 | Análisis exploratorio, segmentación y correlación |
| **1.18** · Pronósticos | 12 | Los cuatro modelos comparados de la V5 (18.3) |

### Bloque 2 · La gestión de proyectos de Business Intelligence

| Punto del temario | Capítulo | Dónde se ve en el caso |
|---|---|---|
| **2.1** · Gestión de proyectos | 13.1 – 13.3 | Por qué no es un proyecto de desarrollo |
| **2.2** · Planificación del proyecto | 13.4 | El reparto real de las 24 semanas (19.1) |
| **2.3** · Riesgos | 13.5 | Los diez riesgos · El error del margen (16.4) |

### Bloque 3 · Arquitectura de un proyecto de Business Intelligence

| Punto del temario | Capítulo | Dónde se ve en el caso |
|---|---|---|
| **3.1** · Procesos de Extracción, Transformación y Carga | 7.1 – 7.3 | Las quince reglas de Gertru convertidas en proceso |
| **3.2** · El almacén de datos | 6.3 – 6.5 | Las capas de la V2 |
| **3.3** · Visualización y consulta: Reportes | 11.6 | Informes operativos y analíticos |
| **3.4** · Visualización y consulta: Dashboards | 11.7, 11.8 | Los tres cuadros y su auditoría (17.3) |
| **3.5** · Visualización y consulta: OLAP | 8.8 | Las cinco operaciones sobre el modelo |
| **3.6** · Visualización y consulta: Data Mining | 10 | La segmentación de clientes de la V5 (18.4) |
| **3.7** · Procesos ETL | 7.4, 7.5 | ETL frente a ELT; orquestación y fallos |
| **3.8** · Creación de cubos multidimensionales | 8.1 – 8.8 | El modelo dimensional de la V3 |

### Contenidos añadidos que no están en el temario oficial

Se incluyen porque, sin ellos, buena parte del temario oficial se aplica mal:

| Contenido | Capítulo | Por qué se añade |
|---|---|---|
| Escalas de medida | 2 | Determina qué es dimensión, qué es hecho y qué técnica se puede aplicar |
| Calidad del dato | 3 | Es el 25 % del esfuerzo real de cualquier proyecto |
| Dispersión y posición | 5 | Sin ella, cualquier KPI de tendencia central es engañoso |
| Aditividad y media ponderada | 8.6 – 8.7 | Es el error más frecuente en cuadros de mando reales |
| Correlación y causalidad | 9.5 – 9.6 | El puente entre «análisis» y «decisión» del punto 1.17 |
| Definición operativa de KPI | 11.2 | Lo que hace que los puntos 1.14 a 1.16 sean fiables |

---

## Apéndice B · Glosario de términos

**Aditiva (métrica).** Métrica que puede sumarse a lo largo de todas las
dimensiones. El importe de venta lo es; el stock y los porcentajes, no.

**Agrupamiento** (*clustering*). Técnica de minería de datos que descubre grupos
naturales en un conjunto sin conocerlos de antemano. Aprendizaje no supervisado.

**Almacén de datos** (*Data Warehouse*). Repositorio integrado, temático, no volátil
e histórico donde se consolidan los datos de todas las fuentes para su análisis.

**Asimetría.** Falta de simetría de una distribución. Positiva si la cola se
extiende hacia la derecha (media > mediana), negativa en el caso contrario.

**Atípico** (*outlier*). Observación que se aleja marcadamente del resto. Criterio
habitual: fuera de Q1 − 1,5·IQR o Q3 + 1,5·IQR.

**Captura de cambios** (*CDC*). Técnica de extracción que lee las modificaciones
directamente del registro de transacciones del sistema de origen.

**Chebyshev (desigualdad de).** Garantiza que, sea cual sea la distribución, al
menos 1 − 1/k² de los individuos están a menos de k desviaciones típicas de la
media. Para k = 2, al menos el 75 %. Para k = 1 no garantiza nada.

**Conformado (de dimensiones).** Proceso de unificar los códigos y descripciones de
las distintas fuentes en un catálogo común, de modo que la misma entidad se
identifique igual en todo el almacén.

**Correlación.** Medida del grado en que dos variables cuantitativas varían
conjuntamente, entre −1 y +1. No implica causalidad.

**CRISP-DM.** Metodología estándar de proyectos de minería de datos, en seis fases
cíclicas que empiezan por la comprensión del negocio.

**Cuartil.** Cada uno de los tres valores que dividen un conjunto ordenado en cuatro
partes iguales. Q2 es la mediana.

**Cubo multidimensional.** Estructura que organiza los hechos según las dimensiones,
con las agregaciones precalculadas por nivel de jerarquía.

**Datamart.** Subconjunto del almacén orientado a un área concreta de negocio.

**Desviación típica** (σ). Raíz cuadrada de la varianza. Mide la dispersión en las
mismas unidades que la variable.

**Dimensión.** Tabla que aporta el contexto descriptivo de los hechos: quién, qué,
dónde, cuándo. Sus atributos son variables nominales u ordinales.

**Dimensión lentamente cambiante** (*SCD*). Estrategia para tratar los cambios en
los atributos de una dimensión. Tipo 1 sobrescribe, tipo 2 versiona con fechas de
validez, tipo 3 conserva el valor anterior en otra columna.

**Drill-down / drill-up.** Operaciones de navegación que bajan o suben un nivel en
una jerarquía.

**ELT.** Variante del ETL en la que se carga el dato en bruto y la transformación se
ejecuta dentro del propio almacén.

**ETL.** Extracción, transformación y carga. El conjunto de procesos que lleva el
dato desde las fuentes hasta el almacén.

**Frecuencia absoluta.** Número de individuos que comparten un mismo valor.

**Frecuencia acumulada.** Suma de las frecuencias hasta un valor dado. Sólo tiene
sentido en escalas ordinales o superiores.

**Frecuencia relativa.** Proporción de individuos con un valor dado sobre el total.

**Fuga de información** (*data leakage*). Error por el que una variable de entrada
de un modelo contiene, directa o indirectamente, la respuesta que se quiere predecir.

**Granularidad** (grano). Lo que representa una fila de una tabla de hechos. La
decisión más importante de un modelo dimensional.

**Hecho.** Tabla que registra los eventos medibles del negocio. Contiene métricas
cuantitativas y claves hacia las dimensiones.

**Histograma.** Gráfico de la distribución de una variable continua, con el eje
horizontal graduado y el área proporcional a la frecuencia.

**HOLAP.** Implementación híbrida de OLAP: agregados en estructura
multidimensional y detalle en base relacional.

**Interanual.** Comparación con el mismo período del año anterior. Elimina el efecto
de la estacionalidad.

**IQR** (rango intercuartílico). Q3 − Q1. Amplitud del intervalo que contiene al
50 % central de los individuos. Es la medida de dispersión asociada a la mediana.

**KPI.** Indicador que mide el avance hacia un objetivo y cuyo valor cambia una
decisión. Una métrica que no cambia ninguna decisión no es un KPI.

**Media.** Suma de todos los valores dividida entre el número de individuos. Sólo
tiene sentido en escalas cuantitativas. Indicador suficiente pero no robusto.

**Media ponderada.** Media en la que cada valor se pondera por su peso relativo. Es
la forma correcta de agregar ratios cuando no se dispone de los totales.

**Mediana.** Valor que ocupa el centro del conjunto ordenado. Deja el 50 % por
debajo. Indicador robusto pero no suficiente.

**Moda.** Valor más frecuente. Único indicador de tendencia central disponible para
variables nominales.

**MOLAP.** Implementación de OLAP sobre una estructura multidimensional propia con
las agregaciones precalculadas.

**Multimodal.** Distribución con varios picos. Suele indicar que hay dos o más
poblaciones distintas mezcladas.

**Nominal (escala).** Escala de medida que sólo permite clasificar. No hay orden.

**No aditiva (métrica).** Métrica que no puede sumarse por ninguna dimensión. Todos
los ratios, porcentajes y promedios lo son.

**OLAP.** *On-Line Analytical Processing*. Sistemas diseñados para consultas
analíticas sobre grandes volúmenes.

**OLTP.** *On-Line Transaction Processing*. Sistemas diseñados para registrar
transacciones operativas.

**Ordinal (escala).** Escala de medida que permite clasificar y ordenar, pero sin
unidad de medida. Admite mediana y percentiles; no admite media.

**Paradoja de Simpson.** Fenómeno por el que una relación observada en los datos
agregados se invierte o desaparece al analizar los subgrupos.

**Percentil.** Valor que deja por debajo un porcentaje dado de los individuos. P90
deja por debajo al 90 %.

**Puntuación diferencial.** Diferencia entre el valor de un individuo y la media del
conjunto. Su promedio es siempre cero.

**Ratio.** Cociente entre dos magnitudes. No se agrega: se recalcula desde los
totales.

**Razón (escala de).** Escala cuantitativa con cero absoluto que indica ausencia de
la propiedad. Admite multiplicación y división.

**Regla empírica.** Si la distribución es aproximadamente normal, el 68 % de los
casos está a ±1σ de la media, el 95 % a ±2σ y el 99,7 % a ±3σ.

**Robusto (indicador).** Indicador insensible a los valores extremos. La mediana lo
es; la media no.

**ROLAP.** Implementación de OLAP que traduce las consultas a SQL sobre un modelo
relacional en estrella.

**Semiaditiva (métrica).** Métrica que puede sumarse por unas dimensiones y no por
otras. El stock se suma entre tiendas y no entre días.

**Sesgo de supervivencia.** Error de análisis por trabajar sólo con los casos que
han llegado al final, ignorando los que no. Cargar sólo las reparaciones cerradas lo
produce.

**Sobreajuste** (*overfitting*). Situación en que un modelo aprende de memoria los
datos de entrenamiento y falla sobre datos nuevos.

**Staging.** Capa donde se deposita el dato en bruto tal como llega, antes de
transformarlo. Permite reprocesar sin volver a la fuente.

**Suficiente (indicador).** Indicador que utiliza toda la información del conjunto
para su cálculo. La media lo es; la mediana no.

**Varianza.** Media de los cuadrados de las desviaciones respecto a la media. Sus
unidades son las de la variable al cuadrado, lo que la hace difícil de interpretar.

---

## Apéndice C · Referencia rápida de indicadores

### C.1. Qué se puede calcular con cada escala

| Indicador | Nominal | Ordinal | Cuantitativa |
|---|:---:|:---:|:---:|
| Frecuencias absoluta y relativa | ✔ | ✔ | ✔ |
| Frecuencia acumulada | ✘ | ✔ | ✔ |
| Moda | ✔ | ✔ | ✔ |
| Mediana | ✘ | ✔ | ✔ |
| Cuartiles y percentiles | ✘ | ✔ | ✔ |
| Rango intercuartílico | ✘ | ✔ | ✔ |
| Media | ✘ | ✘ | ✔ |
| Varianza y desviación típica | ✘ | ✘ | ✔ |
| Correlación | ✘ | Con reservas | ✔ |

### C.2. Cómo se interpreta cada indicador

| Indicador | Qué dice | Cómo se enuncia |
|---|---|---|
| Moda | El valor más frecuente | «Lo más habitual es…» |
| Mediana | El centro del colectivo | «La mitad está por debajo de…» |
| Media | Lo que tocaría a cada uno si todos aportaran igual | «Si repartiéramos por igual…» |
| Desviación típica | Dispersión en torno a la media | «Al menos el 75 % está entre media ± 2σ» |
| IQR | Amplitud del 50 % central | «La mitad central está entre Q1 y Q3» |
| P90 | El valor que sólo supera el 10 % | «9 de cada 10 están por debajo de…» |
| Correlación | Grado de variación conjunta | «Cuando sube A, tiende a subir B» |

### C.3. Qué indicador usar según la forma

| Forma de la distribución | Tendencia central | Dispersión |
|---|---|---|
| Simétrica | Media | Desviación típica |
| Asimétrica | Mediana | Rango intercuartílico |
| Multimodal | **Ninguno**: segmentar primero | — |
| Con atípicos que son reales | Mediana, y analizar los atípicos aparte | IQR + recuento de atípicos |
| Con atípicos que son errores | Corregir el dato y volver a empezar | — |

### C.4. Reglas de interpretación de la dispersión

```
SIEMPRE CIERTO (Chebyshev)
  media ± 2σ  contiene al menos el 75 % de los individuos
  media ± 3σ  contiene al menos el 89 %
  media ± 1σ  no garantiza nada

SÓLO SI LA DISTRIBUCIÓN ES APROXIMADAMENTE NORMAL (regla empírica)
  media ± 1σ  contiene ≈ 68 %
  media ± 2σ  contiene ≈ 95 %
  media ± 3σ  contiene ≈ 99,7 %

ATÍPICOS
  fuera de  Q1 − 1,5·IQR  o  Q3 + 1,5·IQR     → atípico
  fuera de  Q1 − 3·IQR    o  Q3 + 3·IQR       → atípico extremo
```

### C.5. Agregación de métricas

| Tipo | Se suma por | Ejemplo | Cómo se agrega |
|---|---|---|---|
| Aditiva | Todas las dimensiones | Importe, unidades, coste | Suma |
| Semiaditiva | Todas menos el tiempo | Stock, saldo, plantilla | Suma entre entidades; último valor o media en el tiempo |
| No aditiva | Ninguna | Margen %, ticket medio, conversión | **Recalcular desde los totales** |

```
NUNCA          media de los porcentajes de cada tienda
SIEMPRE        Σ numerador / Σ denominador
ALTERNATIVA    Σ (valorᵢ × pesoᵢ) / Σ pesoᵢ      (media ponderada)
```

### C.6. Elección de gráfico

| Objetivo | Gráfico | Evitar |
|---|---|---|
| Comparar categorías | Barras | Sectores con más de 5 |
| Composición | Barras apiladas al 100 % | Sectores en 3D |
| Evolución temporal | Líneas | Barras si hay muchos puntos |
| Distribución | Histograma, caja y bigotes | Sólo la media |
| Comparar distribuciones | Cajas múltiples, violines | Medias en una tabla |
| Relación entre variables | Dispersión | Sólo el coeficiente |
| Valor contra objetivo | Barra con marcador | Semáforo sin la cifra |

### C.7. Comparaciones temporales

| Comparación | Qué responde | Cuándo usarla |
|---|---|---|
| Frente al período anterior | Movimiento inmediato | Series sin estacionalidad |
| Interanual | Crecimiento real | Siempre en negocios estacionales |
| Acumulado del año | Avance hacia el objetivo anual | Seguimiento de objetivos |
| Media móvil de 12 | Tendencia limpia | Detectar cambios de fondo |
| Frente a objetivo | Cumplimiento | Rendición de cuentas |

### C.8. Lista de comprobación antes de publicar un indicador

```
☐ ¿Está escrita su definición operativa completa?
☐ ¿Tiene un propietario?
☐ ¿Qué decisión cambia según su valor?
☐ Si es un ratio, ¿se recalcula desde los totales?
☐ ¿Lleva su indicador de dispersión?
☐ ¿Lleva contexto: objetivo, interanual o distribución?
☐ ¿Se muestra el tamaño de la muestra?
☐ ¿El eje del gráfico empieza donde debe?
☐ ¿Se puede hacer drill-down para ver los segmentos?
☐ ¿Alguien del negocio ha validado que reconoce la cifra?
```

---

## Apéndice D · Soluciones de los ejercicios

### Capítulo 1 · Qué es Business Intelligence

**1.1.** Hay al menos cinco preguntas distintas: ¿vende más en importe o en
unidades? ¿En qué familias? ¿Es una diferencia de número de tickets o de ticket
medio? ¿Es estable en el tiempo o de un período concreto? ¿Y «del mismo tamaño»
significa superficie, plantilla o mercado potencial? Sin desdoblarla, cualquier
respuesta será parcial. Se retoma en 9.3.

**1.2.** (a) Operativo, grano de orden individual, actualización continua.
(b) Táctico, grano de familia y trimestre. (c) Estratégico, muy agregado, con serie
larga. Cada uno necesita un almacén con detalle suficiente: por eso el grano se
elige al máximo detalle, no al nivel del informe.

**1.3.** No se puede subir al escalón predictivo sin tener resuelto el descriptivo.
La recomendación es dedicar el presupuesto a unificar la definición de venta, y sólo
después plantear el modelo. Un modelo entrenado sobre dos cifras contradictorias
producirá predicciones precisas de un número que no significa nada.

**1.4.** Respuesta abierta. El resultado habitual es que se responden dos o tres de
las nueve, y las que más cuesta responder son la 2 (de dónde viene) y la 5 (si la
media es representativa).

### Capítulo 2 · Escalas de medida

**2.1.** `id_ticket` nominal (identificador, dimensión degenerada); `fecha`
cuantitativa de intervalo, dimensión; `id_tienda` nominal, dimensión;
`código_postal_cliente` nominal aunque sea numérico, dimensión; `forma_de_pago`
nominal, dimensión; `unidades` cuantitativa discreta de razón, hecho; `importe`
cuantitativa continua de razón, hecho; `descuento_%` es un ratio: se guarda el
importe de descuento como hecho y el porcentaje se calcula; `valoración_1_a_5`
ordinal, dimensión, y no se promedia.

**2.2.** El código postal no es una cantidad: no tiene unidad de medida y su media
no señala ningún lugar. La necesidad real es saber dónde se concentran los clientes.
Dos respuestas correctas: la moda —el código postal más frecuente— o un mapa con la
frecuencia por área, que además muestra la dispersión.

**2.3.** Para el jefe de taller, intervalos finos en la parte baja (0–2 h, 2–4 h,
4–8 h, 8–24 h, más de 24 h): sus decisiones son de asignación diaria. Para
dirección, intervalos ligados al compromiso de servicio (dentro de plazo, hasta 2
días de retraso, más de 2 días). No son la misma porque responden a decisiones
distintas: la agrupación es una decisión de análisis.

**2.4.** «¿Cuál es la distribución completa de las valoraciones y cuántas respuestas
hay?» Un 7,2 puede venir de todo el mundo puntuando 7 o de la mitad puntuando 4 y la
otra mitad 10. Se publicaría junto a la mediana y al porcentaje de valoraciones de 5
o menos.

**2.5.** La temperatura en grados Celsius es una escala de intervalo: su cero es
arbitrario. La afirmación correcta es «en julio la temperatura media fue 20 grados
superior a la de enero». Para hablar de proporciones habría que usar una escala de
razón como el kelvin, donde la diferencia sería del 7 %.

### Capítulo 3 · La calidad del dato

**3.1.** Afecta a la **validez**. Causas posibles: fechas invertidas al teclear;
órdenes reabiertas conservando la fecha de salida antigua; zona horaria distinta en
entrada y salida. Con la primera se corrige intercambiando; con la segunda hay que
definir qué es «la salida»; con la tercera se normaliza en la carga. En ningún caso
se descartan sin registrar cuántos eran.

**3.2.** Tres valores posibles: (a) excluyendo los tickets sin cliente, sube porque
se divide entre menos; (b) tratando los nulos como un cliente ficticio único, baja
mucho; (c) tratando cada ticket sin cliente como un cliente distinto, baja también.
Se publicaría (a), indicando expresamente que se calcula sobre el 82 % de los
tickets identificados.

**3.3.** Estuvieron mal todos los indicadores absolutos que mezclaban ambos canales
—venta total, ticket medio global, margen— y todos los ratios cuyo numerador o
denominador cruzaba canales. Se salvaron los indicadores calculados **dentro** de un
solo canal, porque el factor cien afectaba a numerador y denominador por igual, y
los de unidades, que no llevan importe.

**3.4.** (a) Rango: ningún importe unitario fuera de [0, 20.000] €. (b) Coherencia:
fecha de salida ≥ fecha de entrada. (c) Volumen: número de registros dentro de
media ± 2σ de las últimas ejecuciones. Una cuarta útil: total por canal comparado
con el del día anterior, para detectar cambios de unidad.

**3.5.** Afecta a **puntualidad** y a **no volatilidad**. Solución: congelar el
fichero en la carga —guardar una copia sellada con fecha en *staging*— y comparar en
cada ejecución con la versión anterior, avisando de las modificaciones retroactivas.

### Capítulo 4 · Frecuencias y tendencia central

**4.1.** Más de 24 horas: 8 % + 3 % = 11 %. Entre 5 y 24 horas: 310 + 160 = 470. La
mediana está en el intervalo 5–8 horas, que es el primero cuya acumulada (73 %)
alcanza el 50 %.

**4.2.** «Familia igual o inferior a bicicleta» no significa nada: la familia de
producto es nominal y no tiene orden. El 90,4 % de la tabla ordenada por frecuencia
sólo permite decir que «tres familias concentran el 90 % de las operaciones».

**4.3.** La distribución es bimodal: hay dos poblaciones mezcladas, la venta de
accesorios y la de bicicletas completas. Antes de calcular ningún indicador global
hay que segmentar y analizarlas por separado, porque ninguna media describirá a
ninguna de las dos.

**4.4.** Media = 292,3; mediana = 57,5. Se publica la mediana: la media está
determinada por un solo valor. El 2.400 no se elimina; se comprueba si es real —una
venta de bicicleta completa— y en ese caso se analiza en el segmento que le
corresponde.

**4.5.** «El gasto habitual de nuestros clientes está en torno a 54 €. La media, de
128 €, refleja el peso de las ventas de bicicleta completa, que son pocas pero de
importe muy alto.» Dos frases, las dos ciertas, ninguna engañosa.

**4.6.** Porque codificar como 1, 2 y 3 no crea una unidad de medida: sigue sin
haber garantía de que la distancia entre «baja» y «normal» sea la misma que entre
«normal» y «urgente». Se usa la mediana, y se publica la distribución de frecuencias.

### Capítulo 5 · Dispersión, posición y forma

**5.1.** Valencia tiene un negocio heterogéneo —mezcla ventas pequeñas y grandes— y
Zaragoza uno muy homogéneo. En Valencia tiene sentido segmentar y trabajar los
tickets altos; en Zaragoza, buscar crecer el ticket típico. El mismo objetivo
comercial produciría acciones distintas.

**5.2.** Con Chebyshev: «al menos el 75 % de los tickets está entre 38 € y 218 €».
Con normalidad: «aproximadamente el 68 % está entre 83 € y 173 €». Se publicaría la
primera, porque no exige suponer nada sobre la distribución y en un ticket de venta
—claramente asimétrico— la suposición de normalidad no se sostiene.

**5.3.** Ordenados, n = 12. Mediana = (1,5 + 2)/2 = 1,75. Q1 = 1, Q3 = 3,5,
IQR = 2,5. Límite superior = 3,5 + 1,5 × 2,5 = 7,25. El valor 21 es atípico; supera
también 3,5 + 3 × 2,5 = 11, luego es atípico extremo. No se elimina: es una
restauración de Manolo, un caso real de otra línea de negocio.

**5.4.** A favor: el indicador global sería más estable y representaría mejor a la
reparación estándar. En contra: se estaría ocultando el 3 % de casos que genera la
mayor parte de las quejas y el mayor margen. Decisión correcta: no eliminar,
**segmentar** en dos flujos con dos compromisos de plazo distintos.

**5.5.** Un histograma con un pico pronunciado hacia 35 €, descendiendo deprisa, y
una cola larga hacia la derecha. Es asimetría positiva. Se publica la mediana como
tendencia central, acompañada del P90 para no perder la cola.

**5.6.** Porque el diagrama de caja sólo usa cinco números: dos distribuciones con
los mismos cuartiles pueden tener una sola concentración central o dos grupos
separados. Se distinguen con un histograma o con un gráfico de violín.

**5.7.** Porque un solo número no puede describir a la vez el caso típico y la cola,
y las decisiones sobre el taller dependen de las dos cosas. Se propone: mediana
(1,1 d), P90 (8,7 d) y porcentaje que supera el compromiso de plazo.

### Capítulo 6 · Sistemas operacionales y analíticos

**6.1.** Necesita una dimensión de **tipo 2**, para que las ventas de cada año
queden asociadas a la región vigente en su momento. Si lo que quiere es analizar la
evolución con la estructura actual, necesita además el atributo de región actual en
la misma dimensión: son dos análisis distintos y ambos legítimos.

**6.2.** (1) Rendimiento: las consultas analíticas bloquean el cobro en caja.
(2) No hay historia: el TPV purga al cerrar el ejercicio. (3) No hay integración:
faltan la web, el taller y el ERP. (4) No hay historificación: los cambios de
catálogo reescriben el pasado y los informes antiguos dejan de ser reproducibles.

**6.3.** Integrado: unificar las cuatro definiciones de venta. Temático: organizar
por ventas, taller y clientes, no por TPV, web y ERP. No volátil: que el informe de
marzo se pueda regenerar en junio. Histórico: que las ventas de 2023 sigan
perteneciendo a la región Levante.

**6.4.** Los dos datamarts darían cifras distintas de «número de clientes» y ninguna
sería comparable. Se resuelve conformando la dimensión cliente: una única
`DIM_CLIENTE` con un atributo que distinga el rol, de modo que cada área filtre lo
que necesite sobre el mismo maestro.

**6.5.** Porque permite reprocesar todo el histórico si cambia una regla de
transformación. En el caso Rueda Libre salvó el proyecto cuando en la V3 cambió la
regla de imputación de coste: sin *staging* habrían sido dos años de historia
perdidos.

### Capítulo 7 · Procesos ETL

**7.1.** Con carga incremental por fecha de modificación, si el TPV sella los
registros con la fecha original, las ventas de los tres días no se habrían cargado
nunca. Con carga completa se habrían recuperado solas. Lo detecta un control de
volumen por tienda y día comparado con su media histórica.

**7.2.** Opción A: la devolución resta del mes de la venta original. Opción B: resta
del mes de la devolución. Con A, las cifras publicadas de meses cerrados cambian
retroactivamente; con B, no. Se elige B por no volatilidad, y se publica aparte un
indicador de tasa de devolución por mes de venta.

**7.3.** (1) Descartar la línea: los totales dejan de cuadrar y nadie sabe por qué.
(2) Asignarla a un miembro «desconocido» de la dimensión: los totales cuadran y el
problema es visible. (3) Crear el artículo al vuelo con los datos disponibles:
cuadra, pero contamina el catálogo. Se recomienda (2).

**7.4.** Primero las dimensiones sin dependencias: `DIM_FECHA`, `DIM_TIENDA`,
`DIM_PRODUCTO`, `DIM_CLIENTE`, `DIM_EMPLEADO`. Después los hechos: `HECHO_VENTAS` y
`HECHO_REPARACIONES`. Un hecho cargado antes que su dimensión genera huérfanos.

**7.5.** (1) ¿Qué decisión concreta se va a tomar con un dato de hace cinco minutos
que no se pueda tomar con el de anoche? (2) ¿Está la fuente preparada para
extracciones continuas sin afectar a la operación? (3) ¿Quién asume el coste de
infraestructura y de mantenimiento que multiplica? Casi siempre, la respuesta a la
primera hace innecesarias las otras dos.

### Capítulo 8 · El modelo dimensional

**8.1.** Una orden de reparación, abierta o cerrada, con su estado. Con un grano más
grueso —por tienda y mes— se perdería la posibilidad de calcular percentiles del
tiempo de reparación, que es exactamente el análisis que resolvió el problema del
taller en la V4.

**8.2.** Aditivas: unidades, importe neto, coste de las piezas. Semiaditiva: stock a
fin de día. No aditivas: margen %, ticket medio, tiempo medio de reparación. El
número de clientes distintos es un caso particular: es no aditivo porque un mismo
cliente puede comprar en dos tiendas y no debe contarse dos veces; hay que
recalcularlo a cada nivel de agregación.

**8.3.** Media simple de porcentajes: 20,0 %. Recalculado: 99.600 / 600.000 =
16,6 %. Para un comité: «el 20 % trata igual a Valencia, que hace dos tercios de la
facturación, que a Vigo, que hace el 3 %. Las tiendas pequeñas tienen más margen
porcentual y arrastran la media hacia arriba».

**8.4.** Jerarquía comercial: Sección → Familia → Subfamilia → Artículo, que responde
a «¿qué categorías tiran del margen?». Jerarquía de proveedor: Proveedor → Marca →
Artículo, que responde a «¿qué proveedores nos aportan más margen y cuál es nuestra
dependencia de cada uno?».

**8.5.** Sumar el stock de los treinta días da un número sin significado. La
agregación correcta por la dimensión tiempo es el último valor del período —el stock
a fin de mes— o la media diaria, según se quiera medir la posición final o la
inmovilización media.

**8.6.** Con tipo 1, todas las ventas del año aparecen en la región nueva, incluidas
las anteriores al cambio. Con tipo 2, las anteriores quedan en la región antigua. La
«correcta» depende de la pregunta: para evaluar el rendimiento histórico de la
región antigua, tipo 2; para planificar con la estructura actual, hace falta además
el atributo de región vigente. Lo habitual es implementar ambos.

### Capítulo 9 · Analizar

**9.1.** Primero por familia de producto, porque el ticket medio es muy sensible al
mix y es la causa más frecuente. Después por tienda, para descartar que sea un
efecto local. Después por canal, porque la web tiene un ticket estructuralmente
distinto. Y por último por segmento de cliente. El orden va de la causa más probable
a la menos.

**9.2.** (a) A causa B: la acumulación del fin de semana satura el taller el lunes.
(b) B causa A: se reserva el lunes para los trabajos que se sabe que son largos.
(c) Confusión: los lunes entran más restauraciones porque el cliente las trae tras
usar la bici el fin de semana, y son largas por su naturaleza. Para distinguir, hace
falta el tipo de trabajo y la carga del taller por día.

**9.3.** No. Ambas pueden estar causadas por el mismo factor: la compra de una
bicicleta nueva, que arrastra ropa y casco a la vez. La prueba sería un experimento:
promocionar cascos en seis tiendas elegidas al azar y no en las otras seis, y
comparar la venta de ropa entre ambos grupos.

**9.4.** Respuesta abierta. La construcción funciona repartiendo desigualmente los
casos entre dos grupos con tasas base muy distintas.

**9.5.** (1) Las tiendas que abren en domingo pueden ser las de zonas comerciales,
con más tráfico de por sí. (2) Puede haber desplazamiento de venta desde el sábado,
no venta adicional. (3) El coste laboral del domingo puede consumir el margen extra.
(4) La comparación no controla el tamaño ni la antigüedad de las tiendas. Es un caso
de poblaciones no equivalentes.

### Capítulo 10 · Minería de datos

**10.1.** (a) Clasificación. (b) Agrupamiento. (c) Reglas de asociación.
(d) Regresión. (e) Detección de anomalías.

**10.2.** Fuga de información: la variable «número de llamadas de aviso de retraso»
sólo existe **después** de que el retraso se haya producido. En el momento en que
hay que predecir vale siempre cero. El modelo no predice nada; describe el pasado.

**10.3.** Criterio estadístico: que los grupos sean compactos y estén separados, y
que la solución sea estable al repetir el algoritmo con otra muestra o semilla.
Criterio de negocio: que cada grupo se pueda describir en una frase y que sugiera
una acción comercial distinta. Si dos grupos merecen la misma acción, sobran grupos.

**10.4.** Porque el reparto aleatorio permitiría entrenar con datos posteriores a
los de prueba, lo que en producción es imposible. La alternativa es la partición
temporal: entrenar con los primeros períodos y validar con los últimos, avanzando la
ventana.

**10.5.** (1) ¿Qué decisión concreta va a mejorar este proyecto y quién la toma?
(2) ¿Cómo sabremos dentro de seis meses si ha servido para algo? (3) ¿Qué se hace
hoy para tomar esa decisión y qué tiene de insuficiente?

### Capítulo 11 · Indicadores y cuadros de mando

**11.1.** Ejemplo: Nombre — tasa de devolución. Pregunta — ¿qué proporción de lo
vendido se devuelve? Fórmula — importe devuelto / importe vendido. Fuente —
`HECHO_VENTAS`, capa de presentación. Grano — línea de ticket. Filtros — excluye
traspasos y ventas a empleados. Período — la devolución se imputa al mes de la venta
original, para que el indicador sea comparable. Responsable — dirección comercial.

**11.2.** Dos señales bastan sin ver el detalle: que el valor de la cadena esté
fuera del rango de valores de las tiendas —imposible en una agregación correcta— o
que el indicador cambie al aplicar un filtro que no debería afectarle. Con tiendas
entre el 15 % y el 25 %, un 20 % es posible, pero se puede comprobar recalculando
desde los totales de margen y venta.

**11.3.** Mediana del ticket, porque describe al cliente típico; P90, porque
identifica la venta grande; y número de tickets, porque sin volumen el ticket medio
no se puede interpretar. La media se muestra en segundo plano.

**11.4.** KPI: tasa de conversión, margen por familia, porcentaje de reparaciones
fuera de plazo —los tres tienen responsable, objetivo y acción asociada. Decoración:
número de visitas a la web (sin conversión no cambia ninguna decisión), seguidores
en redes, número de referencias en catálogo.

**11.5.** Da la impresión de una variación enorme entre meses cuando es del 6 %. En
un gráfico de barras el eje debe empezar en cero, porque la longitud de la barra
codifica la magnitud. Si lo que interesa es la variación, se cambia a un gráfico de
líneas indicando el rango.

**11.6.** Ejemplo: mide el número de órdenes que superan el plazo comprometido.
Umbral: más de 2σ sobre la media de las últimas ocho semanas. Avisa al jefe de
taller. Acción: reasignar carga o comunicar el retraso al cliente. Es mejor que un
número fijo porque se adapta a la estacionalidad: en julio la carga normal es el
triple que en enero, y un umbral fijo dispararía todos los días de verano.

### Capítulo 12 · Series temporales y pronósticos

**12.1.** (1) Comprobar la comparación interanual: en un negocio estacional, la
caída de octubre respecto a septiembre es normal. (2) Comprobar si la variación está
dentro del ruido histórico (±7 %); una caída del 28 % lo supera, luego es real.
(3) Comprobar que la carga de datos está completa antes de buscar explicaciones de
negocio.

**12.2.** Mensual: movimiento inmediato, inútil en negocio estacional. Interanual:
crecimiento real. Acumulado: avance hacia el objetivo anual. Media móvil: tendencia
de fondo. Ante el comité se llevan interanual y acumulado; la media móvil como apoyo
si se está discutiendo un cambio de tendencia.

**12.3.** Se necesitan las ventas de cascos del mismo trimestre del año anterior y
una comprobación de que ese trimestre no fue anómalo. El resultado se usa como
referencia: cualquier modelo más sofisticado debe batir su error, y si no lo bate,
no se adopta.

**12.4.** Respuesta abierta. Debe contener el valor esperado, un intervalo con su
nivel de confianza y los supuestos explícitos.

**12.5.** No es casualidad: los grupos pequeños tienen más varianza y por eso ocupan
los extremos de cualquier ranking. Se comprueba viendo si las tres últimas
posiciones también son tiendas pequeñas. Se corrige mostrando el tamaño de la
muestra, ocultando los grupos por debajo de un mínimo, o representando el intervalo
de cada valor en lugar del punto.

**12.6.** (1) Cambio de régimen: algo cambió en el negocio. (2) Degradación normal:
el modelo necesita reentrenarse. (3) Problema de calidad en los datos de entrada.
(4) Realimentación: el modelo está prediciendo su propia decisión. Se distinguen
mirando si la caída fue brusca o gradual, si coincide con un cambio conocido, y si
los datos de entrada mantienen su distribución histórica.

### Capítulo 13 · Gestión del proyecto

**13.1.** Ejemplo: «Que el comité de dirección pueda decidir, cada trimestre y con
datos que las doce tiendas reconozcan como suyos, dónde invertir en superficie y
plantilla. Responsable: dirección general. Se medirá por el uso semanal del cuadro
de mando y por que la cifra de venta coincida con el cierre contable.»

**13.2.** Respuesta abierta; debe coincidir en lo esencial con la tabla del apartado
15.2. Las dependencias críticas son el acceso a los doce TPV y la disponibilidad de
la persona que mantiene `Objetivos.xlsx`.

**13.3.** El reparto está invertido: dedica el 62 % a cuadros de mando y el 25 % a
análisis y datos. Propuesta: dos semanas de definición y acuerdo de KPIs, dos de
perfilado y calidad, dos y media de procesos de carga, una de cuadros de mando y
media de validación. Y admitir que ocho semanas para seis fuentes es optimista.

**13.4.** Ejemplo verificable: «los doce responsables de tienda han validado que la
cifra de venta neta de su tienda coincide con su cierre de caja del último mes, y
ocho de los doce han abierto el cuadro al menos una vez en la última semana».

**13.5.** Respuesta abierta. Ejemplos: para la calidad, control de volumen y rango
en la carga; para las definiciones divergentes, prueba automática que compare la
venta del cuadro con el dato contable; para el cuadro que nadie usa, registro de
accesos con alerta si una tienda no entra en dos semanas.

### Parte II · El caso

**II.1.** El número de clientes estaba inflado en un 44 %; la venta media por
cliente estaba subestimada en un 31 %; el porcentaje de clientes recurrentes estaba
subestimado a menos de la mitad. Los tres errores empujaban en la misma dirección:
hacían parecer que la empresa captaba mucho y fidelizaba poco.

**II.2.** Supóngase que hay 100 reparaciones cerradas con media de 2 días y 10
abiertas que llevan ya 15 días. Con sólo las cerradas, el tiempo medio publicado es
de 2 días. Incorporando las abiertas con su duración hasta hoy, la media sube a 3,2.
El sesgo es sistemático y siempre a la baja, porque las que más tardan son las que
todavía no han cerrado.

**II.3.** A favor: 2,6 puntos de error sobre una previsión de 148.000 € son unos
3.800 € de desviación menos, y el modelo se automatiza. En contra: añade una
dependencia técnica y un mantenimiento, y el ingenuo estacional lo entiende
cualquiera. Defendible en ambos sentidos; lo importante es que la dirección decida
sabiendo cuánto compra.

**II.4.** Comparar el número de órdenes cargadas con la media de las últimas cuatro
ejecuciones y marcar la carga como incompleta si se desvía más de 2σ. Añadir una
regla de validación de caracteres en los campos de texto libre y un informe de
registros rechazados que no se pueda quedar vacío en silencio.

**II.5.** (1) Registro de accesos: cuándo entran, a qué páginas, cuánto tiempo.
(2) Entrevistas con los cuatro responsables, sin la presencia de dirección.
(3) Observación directa de cómo resuelven hoy las decisiones que el cuadro debía
soportar. La hipótesis más probable es que el cuadro operativo resuelve algo que no
era un problema para ellos.

**II.6.** Respuesta abierta. Debe contener: qué se calculaba mal y por qué (promedio
de porcentajes en lugar de recálculo desde totales), la magnitud (3,4 puntos), qué
decisiones se apoyaron en él, qué se ha corregido y qué control impide que vuelva a
pasar. Sin atribuir culpa individual: es un error de método, y el método era el
mismo en toda la organización.

---

## Apéndice E · Errores que circulan como verdades

Recopilación de afirmaciones que se oyen con frecuencia en entornos de BI, y que son
falsas o incompletas.

### E.1. «Media ± 1 desviación típica contiene al menos el 50 % de los casos»

**Falso.** La desigualdad de Chebyshev garantiza al menos 1 − 1/k², que para k = 1
da cero: no garantiza nada. El 68 % que mucha gente recuerda pertenece a la regla
empírica y **exige que la distribución sea aproximadamente normal**.

La afirmación segura, válida sea cual sea la distribución, es: **media ± 2σ contiene
al menos el 75 %**. Y es más útil, porque conecta directamente con la detección de
atípicos.

### E.2. «Los valores atípicos se eliminan para que no distorsionen»

**Incompleto y peligroso.** Sólo se elimina un atípico si se ha comprobado que es un
error de dato. Si es real, se conserva y con frecuencia se estudia aparte: en
detección de fraude, mantenimiento predictivo y análisis de rentabilidad, el atípico
es precisamente el objeto de estudio.

### E.3. «La media es el indicador más representativo»

**Falso en distribuciones asimétricas**, que son la mayoría en negocio: importes,
salarios, tiempos de espera y valor de cliente son casi siempre asimétricos
positivos. En esos casos la mediana representa mejor al colectivo, y ninguno de los
dos basta por sí solo.

### E.4. «Se puede calcular la media de una valoración de 1 a 10»

**Discutible.** Es una escala ordinal sin unidad de medida: no hay garantía de que la
distancia entre 3 y 4 sea la misma que entre 8 y 9, ni de que el 7 de una persona
equivalga al de otra. Se hace en todas partes; lo que no se puede omitir es publicar
la distribución al lado.

### E.5. «El margen de la cadena es la media de los márgenes de las tiendas»

**Falso.** Los ratios no se promedian: se recalculan desde los totales, o se ponderan
por el volumen. En el ejemplo del capítulo 8, la diferencia es de 3,4 puntos
porcentuales sobre el indicador que fija la política de precios.

### E.6. «Si dos variables están correlacionadas, una causa la otra»

**Falso.** Hay cuatro explicaciones posibles y sólo una es la causalidad directa. La
más frecuente en negocio es la variable de confusión. Para afirmar causalidad hace
falta precedencia temporal, asociación y descarte de explicaciones alternativas;
idealmente, un experimento controlado.

### E.7. «Con un data lake ya no hace falta transformar los datos»

**Falso.** El cambio de ETL a ELT modifica **dónde** se transforma, no **si** hay que
transformar. Un depósito de ficheros sin limpiar, conformar ni documentar no es un
data lake: es un almacén de datos que nadie sabe interpretar.

### E.8. «Un modelo con 99 % de acierto es un buen modelo»

**Casi siempre falso.** O hay sobreajuste, o hay fuga de información, o las clases
están tan desequilibradas que predecir siempre lo mismo produce ese acierto. Con un
2 % de bajas, un modelo que diga «nadie se da de baja» acierta el 98 % y es inútil.

### E.9. «El dato en tiempo real siempre es mejor»

**Falso en la mayoría de los casos.** La latencia debe fijarse por la frecuencia de
la decisión, no por la capacidad técnica. Si la decisión se toma en el comité
mensual, un dato al minuto no aporta nada y multiplica el coste y la fragilidad. El
tiempo real se justifica en decisiones operativas: stock, fraude, incidencias.

### E.10. «Las devoluciones se restan del mes en que ocurren»

**No es un error, es una decisión** —pero hay que tomarla explícitamente y por
escrito. Cada opción tiene consecuencias distintas sobre la comparabilidad y sobre
la volatilidad de los meses ya cerrados. Lo inaceptable es que dos informes usen
criterios distintos sin que nadie lo sepa.

### E.11. «Un porcentaje de variación siempre es interpretable»

**Falso cuando la base puede ser cero o negativa.** Si el margen del año anterior fue
negativo, el porcentaje de variación no tiene interpretación. Y con bases pequeñas,
pasar de 1 a 3 ventas es un +200 % que no significa nada sin el valor absoluto al
lado.

### E.12. «Si el cuadro de mando está publicado, el proyecto ha terminado»

**Falso.** El valor de un proyecto de BI está en el uso, no en el entregable. Un
cuadro que nadie abre es un proyecto fracasado aunque cumpliera plazo y presupuesto.
El uso se mide, y es el indicador de éxito más fiable que existe.

---
