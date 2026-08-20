
# EXAMEN 1 · Inteligencia en los negocios — respuestas

## 1 · ¿Qué tipo de sistema de información se caracteriza por estar optimizado para el procesamiento de transacciones rápidas y el mantenimiento de la información operacional al día?

- a) OLAP (Online Analytical Processing)
- b) Data Warehouse (DW)
- **c) OLTP (Online Transaction Processing)** ✅
- d) Data Mart

**Por qué.** OLTP es el sistema del día a día: el que registra ventas, altas y
modificaciones. Está optimizado para muchas operaciones pequeñas y rápidas
—insertar, actualizar, borrar— y para tener el dato vivo al minuto. OLAP es justo
lo contrario: pocas consultas, pero enormes.


---

## 2 · ¿Cuál es la principal desventaja de usar el modelo relacional tradicional (OLTP) directamente para análisis complejos de Business Intelligence?

- a) El modelo relacional es demasiado simple
- **b) Las consultas analíticas largas degradan el rendimiento de las operaciones diarias** ✅
- c) No permite almacenar datos históricos
- d) Es incompatible con las herramientas de visualización de datos

**Por qué.** Una consulta analítica recorre millones de filas y bloquea recursos del
servidor de producción, con lo que se ralentiza la operación del negocio.

Cuidado con la (c): un OLTP **sí** puede guardar histórico. Lo que no hace es
conservar cómo eran las cosas cuando ocurrieron —guarda el estado actual—, que es
otro problema distinto y también real, pero no es «la principal desventaja» que
busca la pregunta.


---

## 3 · ¿Qué concepto describe un almacén de datos enfocado y segmentado para satisfacer las necesidades de una única área funcional o departamento?

- a) Data Lake
- **b) Data Mart** ✅
- c) Data Repository
- d) Cubo MOLAP

**Por qué.** Un **data mart** es un trozo del almacén orientado a un área concreta:
el de finanzas, el de marketing, el de ventas. Contiene sólo lo que ese
departamento necesita, lo que lo hace más rápido, más fácil de entender y más
sencillo de controlar en cuanto a permisos.

El **data lake** es lo contrario: guarda todo en bruto y sin destino concreto.


---

## 4 · La operación OLAP conocida como Drill Down se utiliza para:

- a) Agregar datos y subir de nivel jerárquico (ej. de Ciudad a País)
- b) Cambiar la orientación de las dimensiones en un cubo
- c) Filtrar los datos para mostrar solo una porción del cubo
- **d) Explorar el detalle de los datos y bajar de nivel jerárquico (ej. de Año a Mes)** ✅

**Por qué.** *Drill down* es bajar: de país a provincia, de año a mes. Es la
operación con la que se diagnostica — el total dice que la región ha caído, bajas
un nivel y resulta que ha caído una sola tienda.

Las otras tres opciones también son operaciones OLAP reales, pero son otras:
la (a) es *roll-up* o *drill-up*, la (b) es *pivot* y la (c) es *slice*.


---

## 5 · ¿Qué característica distingue a un Data Warehouse de una base de datos operacional?

- a) Está orientado al proceso
- b) Es volátil y se actualiza constantemente
- **c) Está orientado al asunto y es no volátil (histórico)** ✅
- d) Está diseñado para un máximo de 100 usuarios simultáneos

**Por qué.** Son dos de las cuatro propiedades clásicas de un almacén de datos:

- **Integrado** — unifica los datos de todas las fuentes bajo criterios comunes
- **Orientado al asunto** (temático) — se organiza por áreas de negocio, no por la
  aplicación de la que vienen los datos
- **No volátil** — los datos no se modifican ni se borran
- **Histórico** — guarda la evolución en el tiempo

La base de datos operacional es justo lo contrario: orientada al proceso y volátil.


---

## 6 · ¿Qué técnica de Minería de Datos se utiliza para predecir un valor continuo (ej. el precio de una acción o la temperatura)?

- a) Clasificación
- b) Agrupamiento (Clustering)
- **c) Regresión** ✅
- d) Reglas de Asociación

**Por qué.** La palabra clave es **continuo**, es decir, un número:

| Técnica | Qué devuelve | Ejemplo |
|---|---|---|
| **Regresión** | Un número | ¿Cuánto venderemos el mes que viene? |
| Clasificación | Una categoría conocida | ¿Este cliente se dará de baja: sí o no? |
| Agrupamiento | Grupos que no conocías | ¿Qué tipos de cliente hay? |
| Reglas de asociación | Qué ocurre junto | ¿Qué se compra con qué? |


---

## 7 · ¿Cuál es la principal ventaja de utilizar un servidor MOLAP (Multidimensional OLAP)?

- a) Su escalabilidad ilimitada
- **b) La rapidez de consulta, ya que los datos están precalculados y almacenados en un formato de array propietario** ✅
- c) Su dependencia total del almacén de datos relacional
- d) La sencillez en la implementación del proceso ETL

**Por qué.** Hay tres formas clásicas de montar un cubo:

| | Cómo guarda | Ventaja | Inconveniente |
|---|---|---|---|
| **MOLAP** | Estructura multidimensional propia, con las agregaciones ya calculadas | Consultas rapidísimas | Proceso largo y límite de volumen |
| **ROLAP** | No hay cubo: traduce a SQL sobre tablas | Escala mucho mejor | Más lento al consultar |
| **HOLAP** | Mezcla de los dos | Compromiso | Complejidad de ambos |

La (a) es falsa porque la escalabilidad es precisamente el punto débil de MOLAP —y
la ventaja de ROLAP—.


---

## 8 · En la pirámide organizacional de la Inteligencia de Negocios, ¿dónde se sitúan los Sistemas de Soporte a la Decisión (DSS)?

- a) Nivel operativo (transaccional)
- **b) Nivel directivo superior (estratégico)** ⬅ probablemente la esperada
- **c) Nivel táctico y medio** ⬅ defendible con bibliografía
- d) Nivel de proveedores

⚠️ **Esta pregunta no tiene una respuesta única, y no dispongo de la plantilla de
corrección.** Lo que sigue es el razonamiento, no una certeza.

Las opciones (a) y (d) se descartan sin discusión. La pelea real está entre (b) y
(c), y ahí los manuales no coinciden:

| Fuente | Dónde sitúa el DSS |
|---|---|
| Laudon y Laudon, el manual de referencia de sistemas de información | Nivel de **gestión (táctico)**, por atender decisiones semiestructuradas. Lo estratégico se lo llevan los EIS |
| La mayoría de materiales de BI en español, incluidos los de este tipo de acción formativa | Nivel **estratégico**, arriba del todo |

Por el estilo de redacción del examen, **lo más probable es que espere la (b)**.
Pero si a alguien le puntúan mal la (c), tiene argumentos para reclamar.

**Lo que sí es seguro** es el criterio de fondo, y es lo que conviene que aprendan:
un DSS sirve para decidir sobre problemas **mal estructurados**, los que no tienen
una fórmula que los resuelva. Y eso ocurre cada vez más según se sube en la
pirámide. De ahí que unos lo dibujen en medio y otros arriba.


---

## 9 · ¿Cuál de las siguientes es una función de los Reportes y Consultas en la etapa de análisis de BI?

- a) Crear el Data Warehouse
- b) Extraer datos de los sistemas OLTP
- **c) Presentar información relevante y agregada para la toma de decisiones** ✅
- d) Estandarizar formatos de fecha

**Por qué.** Los reportes y consultas están al **final** de la cadena: son la capa
con la que el usuario consume la información ya preparada.

Las otras tres son fases anteriores: la (b) y la (d) son parte del proceso ETL, y
la (a) es la construcción del almacén.


---

## 10 · ¿Qué se entiende por «Datos No Volátiles» en el contexto de un Data Warehouse?

- a) Que los datos son muy importantes
- **b) Que una vez cargados, los datos no se modifican ni se eliminan, sino que se conservan históricamente** ✅
- c) Que los datos pueden consultarse sin conexión a internet
- d) Que los datos se borran al final del día

**Por qué.** En un almacén de datos sólo se añade; no se corrige ni se borra. Y ésa
es la razón de que un informe emitido hace dos años pueda reproducirse hoy con el
mismo resultado.

Es lo contrario de una base de datos operacional, donde el mismo registro se
actualiza continuamente y sólo queda la última versión.


































===============================================================================

# EXAMEN 2 · Gestión de proyectos de BI — respuestas

## 1 · ¿Qué rol o figura es esencial en un proyecto BI para garantizar que el proyecto mantenga su dirección estratégica y provea los recursos necesarios?

- **a) El Sponsor o Patrocinador del proyecto** ✅
- b) El Tester o Evaluador de calidad
- c) El Programador de ETL
- d) El Usuario final operativo

**Por qué.** El **patrocinador** es alguien de dirección que hace dos cosas que
nadie más puede hacer: consigue el presupuesto y las personas, y mantiene la
prioridad del proyecto cuando aparece cualquier urgencia operativa.

Los otros tres roles son imprescindibles, pero ejecutan; no deciden ni financian.
Sin patrocinador, un proyecto de BI se queda sin recursos en cuanto surge algo más
urgente.


---

## 2 · La planificación de un proyecto BI debe comenzar por:

- a) La selección de la herramienta ETL
- b) La carga de datos inicial
- **c) La definición del alcance, los objetivos y la justificación de la necesidad de negocio** ✅
- d) La capacitación de los usuarios finales

**Por qué.** Primero el **para qué**: qué decisión hay que mejorar y por qué merece
la pena gastarse el dinero. Todo lo demás —herramienta, modelo, cargas— son
consecuencias de esa respuesta.

Empezar por la herramienta es el error clásico: se acaba adaptando lo que se quiere
preguntar a lo que la herramienta sabe responder.


---

## 3 · ¿Cuál de los siguientes es un riesgo habitual y específico de los proyectos de Business Intelligence?

- a) Fallo en el sistema de aire acondicionado
- **b) Alto nivel de ambigüedad en los requisitos y expectativas del usuario** ✅
- c) La imposibilidad de acceder a internet
- d) Un cambio en la normativa de seguridad laboral

**Por qué.** Fíjate en la palabra **específico**. Las otras tres le pueden pasar a
cualquier proyecto de cualquier sector. La ambigüedad en los requisitos es propia
del BI porque el usuario no sabe qué quiere ver hasta que ve algo: pide «un cuadro
de mando de ventas», se lo enseñas, y entonces empieza a saber qué necesitaba de
verdad.


---

## 4 · ¿Por qué se considera que los proyectos BI son inherentemente más difíciles de gestionar que los proyectos transaccionales tradicionales?

- a) Porque solo usan software de código abierto
- b) Porque requieren menos personal de IT
- c) Porque nunca superan la fase de definición de requisitos
- **d) Porque el éxito del proyecto está directamente ligado a la satisfacción de la necesidad analítica y no a una función operativa clara** ✅

**Por qué.** En un proyecto transaccional el criterio de éxito es objetivo: la
factura se emite o no se emite. En BI el criterio es que alguien **entienda mejor su
negocio y decida distinto**, que es mucho más difícil de especificar de antemano y
de comprobar al final.

De ahí se deriva todo lo demás: el alcance se descubre trabajando, y un cuadro de
mando terminado que nadie abre es un fracaso aunque cumpliera plazo y presupuesto.


---

## 5 · En la fase de Desarrollo o Construcción de un proyecto BI, ¿cuál es la tarea principal que ocupa la mayor parte del tiempo del equipo técnico?

- a) Redacción de informes de riesgos
- **b) Implementación y prueba de los procesos ETL** ✅
- c) Elaboración de la justificación económica del proyecto
- d) Diseño de la estrategia de comunicación

**Por qué.** Es lo que venimos repitiendo desde el primer día: **la preparación de
los datos se lleva el grueso del esfuerzo**. Extraer, limpiar, conformar,
recodificar, cargar y comprobar que la carga es correcta.

Construir los cuadros de mando, definir las gráficas y los indicadores es la parte
rápida y barata.


---

## 6 · El concepto de «Entrega Iterativa» (o enfoques ágiles como Scrum) es muy común en BI porque permite:

- a) Finalizar todo el proyecto de una sola vez
- b) Usar siempre el mismo software sin importar las necesidades
- c) Eliminar completamente la necesidad de documentación
- **d) Entregar valor al negocio en ciclos cortos y recibir retroalimentación temprana sobre los resultados analíticos** ✅

**Por qué.** Enlaza directamente con la pregunta 3: si el usuario no sabe qué
quiere hasta que lo ve, la única forma sensata de trabajar es enseñarle algo
pequeño y pronto, y corregir con lo que diga.

Es preferible un cuadro de mando de un área funcionando en seis semanas que un
almacén corporativo completo en año y medio.


---

## 7 · ¿Qué se gestiona formalmente mediante un proceso de Control de Cambios en la planificación de un proyecto BI?

- **a) La modificación de los requisitos una vez que el alcance ha sido acordado y bloqueado** ✅
- b) El salario del gestor del proyecto
- c) La elección de los usuarios finales
- d) La revisión de los informes de incidentes menores

**Por qué.** El control de cambios existe para que las peticiones nuevas no entren
por la puerta de atrás. Cada cambio sobre el alcance ya acordado se valora —cuánto
cuesta, a qué afecta, quién lo aprueba— antes de aceptarlo.

Sin él aparece lo que se conoce como *scope creep*: el alcance crece poco a poco sin
que nadie ajuste plazo ni presupuesto.


---

## 8 · ¿Cuál de estas tareas pertenece a la fase de Cierre y Despliegue del proyecto BI?

- a) Definición del modelo dimensional
- **b) El traspaso del sistema a operación (Go-Live) y la formación final de los usuarios** ✅
- c) La adquisición del hardware del servidor
- d) El análisis de la viabilidad económica inicial

**Por qué.** Basta con ordenar las cuatro opciones en el tiempo:

```
Viabilidad económica  →  inicio
Hardware              →  preparación
Modelo dimensional    →  diseño
Go-Live y formación   →  cierre        ← ésta
```


---

## 9 · ¿Qué riesgo específico puede surgir si el equipo de BI no tiene acceso a expertos del negocio durante la fase de análisis?

- a) El proyecto tardará menos
- **b) El modelo de datos será irrelevante o incorrecto para las necesidades reales del negocio** ✅
- c) Se contratará menos personal
- d) Se elegirá una base de datos más barata

**Por qué.** Sin alguien del negocio, las decisiones las acaba tomando por defecto
quien programa la carga, que es justamente quien menos contexto tiene.

Y son decisiones de negocio disfrazadas de técnicas: si una devolución resta del mes
en que se produce o del mes de la venta, qué se considera un cliente activo, si dos
encuestas del mismo DNI son una o dos.


---

## 10 · ¿Cuál es el producto principal que define la «visión» del BI y que debe ser el foco de la planificación inicial?

- a) El listado de todas las tablas fuente
- **b) La especificación detallada de los Dashboards y KPIs que responderán a las preguntas clave del negocio** ✅
- c) El manual de instalación del sistema operativo
- d) El contrato con el proveedor de internet

**Por qué.** La visión del proyecto no es la lista de lo que hay, es **lo que se
quiere poder responder**. De ahí sale todo lo demás hacia atrás: qué indicadores
hacen falta, qué modelo los sostiene, qué datos hay que cargar y cuáles no.

Fíjate en que la (a) es justo el error inverso: empezar por el inventario de tablas
en vez de por las preguntas.


---

