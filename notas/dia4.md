
# Ayer / Resumen

Análisis BIVARIABLE. Las 6 combinaciones posibles:
    NOMINAL x NOMINAL       -> Tabla de contingencia, barras apiladas/agrupadas, heatmap
    NOMINAL x ORDINAL       -> Lo anterior + comparación de MEDIANAS
    NOMINAL x CUANTITATIVA  -> Lo anterior + comparación de MEDIAS + boxplot múltiple
    ORDINAL x ORDINAL       -> Véase NOMINAL x ORDINAL
    ORDINAL x CUANTITATIVA  -> Véase NOMINAL x CUANTITATIVA
    CUANT.  x CUANTITATIVA  -> SCATTERPLOT (nube de puntos)

Gráficos que nos faltaban del univariable: HISTOGRAMA y CAJA Y BIGOTES.

Y por la tarde, el ejercicio con los datos del call-center. Empezamos a meter mano
a la variable tipo_primera_incidencia y a duracion_llamada_seg.

# HOY

Hoy es un día gordo. Vamos a ver:

- La PIRAMIDE ORGANIZACIONAL. Quién decide qué en una empresa.
- El ALMACEN DE DATOS por dentro: qué le pedimos, y qué es un DATAMART.
- El MODELO DIMENSIONAL: HECHOS y DIMENSIONES. Esto es el corazón del día.
- El CUBO y las operaciones OLAP: drill down, drill up, slice, dice, pivot.
- ROLAP, MOLAP, HOLAP.
- Las CATEGORIAS de la minería de datos.
- CORRELACION Y CAUSALIDAD. Que se me quedó ayer en el tintero y es importante.
- Y GESTION DE PROYECTOS de BI.

Y hay 2 exámenes. Así que vamos al lío.

---

# La pirámide organizacional

En una empresa NO TODO EL MUNDO DECIDE LO MISMO. Ni con la misma información,
ni con los mismos plazos.

                        /\
                       /  \
                      /    \        ESTRATEGICO
                     / DSS  \       Dirección. Decisiones a AÑOS.
                    /--------\
                   /          \     TACTICO
                  /    MIS     \    Mandos intermedios. Decisiones a MESES.
                 /--------------\
                /                \  OPERATIVO
               /       TPS        \ Personal de a pie. Decisiones de HOY.
              /--------------------\

Vamos con un ejemplo. Una cadena de tiendas:

- El encargado de la tienda de Alcalá, a las 9 de la mañana:
  Cuántos pedidos tengo pendientes de servir HOY?
  Decisión: a quién mando primero. PLAZO: HORAS.
  -> Necesita el DETALLE. El pedido concreto. Uno a uno.

- El responsable de zona, el día 3 de cada mes:
  Cómo van mis 12 tiendas respecto al objetivo del trimestre?
  Decisión: dónde meto una promoción. PLAZO: MESES.
  -> Necesita AGREGADO por tienda y por mes. El pedido concreto no le sirve de nada.

- El director general, en el comité de diciembre:
  Abrimos tienda en Bilbao el año que viene?
  Decisión: dónde invierto 400.000 euros. PLAZO: AÑOS.
  -> Necesita MUY AGREGADO y con SERIE LARGA. 5 años de datos.

Y AQUI ESTA LA CLAVE:

    LA MISMA CIFRA NO SIRVE PARA LOS TRES NIVELES.

Si al director general le doy el listado de los 4.000 pedidos de ayer, NO PUEDE
DECIDIR NADA. Se ahoga.
Si al encargado de la tienda le doy el acumulado anual de la compañía, TAMPOCO
PUEDE DECIDIR NADA. No le dice qué hacer esta mañana.

El error más típico del mundo en cuadros de mando: hacer UNO SOLO y repartirlo
hacia arriba y hacia abajo. Resultado: demasiado agregado para el que opera y
demasiado detallado para el que dirige. NO LO USA NADIE.

## Los nombres de los sistemas

Cada nivel de la pirámide tiene su tipo de sistema, y esto tiene nombre y siglas
(que en los exámenes preguntan):

    TPS   Transaction Processing System        Nivel OPERATIVO
          Es el OLTP del que llevamos hablando desde el primer día.
          El TPV de la tienda, el gestor de expedientes, el ERP.

    MIS   Management Information System        Nivel TACTICO
          Informes periódicos para mandos intermedios.
          El cierre mensual, el informe de ventas por zona.

    DSS   Decision Support System              Nivel ESTRATEGICO
          Sistemas de Soporte a la Decisión.
          Ayudan a decidir sobre problemas MAL ESTRUCTURADOS:
          "abrimos en Bilbao?" no tiene una fórmula que lo conteste.
          Aquí es donde vive el Business Intelligence de dirección.

    (Y a veces veréis EIS, Executive Information System, que es un DSS
     para el comité de dirección, muy resumido y muy visual.)

OJO CON ESTO, que es de las pocas cosas del curso en las que los libros NO SE
PONEN DE ACUERDO:

    Unos manuales sitúan el DSS en el nivel ESTRATEGICO, arriba del todo.
    Otros lo sitúan en el nivel TACTICO / DE GESTION, en medio, y dejan
    arriba los EIS (Executive Information System).

    Las dos posturas se defienden con bibliografía en la mano. No es que
    una esté mal.

Entonces, cómo os lo aprendéis? PUES NO OS LO APRENDAIS POR EL DIBUJO.
Aprendéoslo por lo que HACE:

    Un DSS sirve para decidir sobre problemas MAL ESTRUCTURADOS.
    Esos que no tienen una fórmula que los conteste.
    "Abrimos en Bilbao?" no se resuelve con una cuenta.

    Y eso pasa CUANTO MAS SUBES en la pirámide. Por eso lo veréis dibujado
    unas veces en medio y otras arriba.

Si os cae en un test y tenéis que elegir entre "táctico" y "estratégico":
en materiales de BI como el nuestro suele dibujarse ARRIBA. Pero si os la dan
por mala, sabed que teníais argumentos.

---

# El almacén de datos, por dentro

Ya sabemos qué es: datos históricos COCINADOS para un objetivo concreto.
Ahora vamos a ponerle nombre a las 4 propiedades que le pedimos. Son de un señor
que se llama Bill Inmon, que es el padre de esto.

## 1. INTEGRADO

Los datos vienen de sitios distintos. Y cada sitio los escribe a su manera.
Lo vimos ayer con vuestro fichero: 24 formas distintas de escribir 5 comunidades.

    TPV dice          "BICICLETA"
    La web dice       "Bici"
    El ERP dice       "BIC"

En el almacén eso NO PUEDE ENTRAR ASI. Todo se unifica bajo un criterio único.
Mismos códigos, mismas unidades, mismos nombres.
A eso se le llama CONFORMAR.

## 2. TEMATICO (u orientado al asunto)

El almacén se organiza por AREAS DE NEGOCIO. No por la aplicación de la que viene
el dato.

    MAL                              BIEN
    Datos del TPV                    Ventas
    Datos de la web                  Clientes
    Datos del ERP                    Taller
                                     Stock

Porque al que analiza LE DA IGUAL de qué programa salió la venta. Él quiere ver
las ventas. Todas. Juntas.

## 3. NO VOLATIL

Y ésta es la que más choca. En el almacén:

    NO SE MODIFICA NADA.
    NO SE BORRA NADA.
    SOLO SE AÑADE.

Volátil quiere decir que cambia. Como la memoria RAM, que os conté el otro día:
apagas y se borra. El almacén es LO CONTRARIO. Lo que entra, se queda.

Y para qué quiero yo eso? Para una cosa muy concreta:

    Un informe que emití hace 2 años, tengo que poder volver a sacarlo HOY
    y que dé EXACTAMENTE EL MISMO RESULTADO.

Si el dato se puede modificar, eso no se puede garantizar. Y entonces mis informes
no valen nada, porque nadie se puede fiar de ellos.

En una BBDD de producción pasa justo lo contrario: el expediente 23uy-98/2026 se
actualiza 40 veces a lo largo de su vida, y solo queda la última versión.

## 4. HISTORICO (variante en el tiempo)

No solo guarda la historia. Guarda CÓMO ERAN LAS COSAS CUANDO OCURRIERON.

Imaginad: la tienda de Valencia estaba en la región "Levante". En julio la empresa
reorganiza y pasa a llamarse región "Este".

Pregunta: las ventas de Valencia de 2023, de qué región son?

    Si guardo solo el estado actual  -> aparecen como región "Este"
    Y entonces el informe de 2023 que emití en 2023 YA NO CUADRA con el que
    saco hoy. Los números han cambiado solos. DESASTRE.

El almacén tiene que poder decir: "en 2023 esa tienda era Levante".

---

# DATAMART

Un DATAMART es un TROZO del almacén, orientado a UN AREA CONCRETA.

    DATAWAREHOUSE (toda la empresa)
        |
        +--- DATAMART de VENTAS        <- lo usa comercial
        +--- DATAMART de RRHH          <- lo usa personal
        +--- DATAMART de FINANZAS      <- lo usa administración

Por qué se hace esto? Por 3 motivos:

1. RENDIMIENTO. Si comercial solo necesita ventas, no le hago recorrer todo.
2. SIMPLICIDAD. El de comercial abre su datamart y ve 8 tablas, no 200.
   Y esto es más importante de lo que parece: si el usuario no entiende lo que
   ve, NO LO USA. Y si no lo usa, el proyecto ha fracasado.
3. SEGURIDAD. El responsable de una tienda no tiene por qué ver los costes de
   proveedor ni las nóminas.

    ATENCION al matiz que os van a preguntar:

    DATA LAKE      -> Todo en bruto, SIN destino concreto. "Ya veremos".
    DATAWAREHOUSE  -> Cocinado, PARA UN OBJETIVO.
    DATAMART       -> Un trozo del warehouse, PARA UN DEPARTAMENTO.

---

# El modelo dimensional

Y ahora vamos a lo gordo del día.

Ayer, con el fichero del call-center, hicisteis esto:

    | CODIGO    |  ETIQUETA           |
    |-----------|---------------------|
    |        0  | No aplica           |
    |        1  | Atención al cliente |
    |        2  | Avería técnica      |
    |        3  | Facturación         |
    |        4  | Instalación         |
    |        5  | Portabilidad        |
    |       98  | No reconocido       |
    |       99  | No informado        |

Pues bien. ESO QUE HICISTEIS AYER TIENE NOMBRE.

    Eso es una TABLA DE DIMENSION.
    Y ese CODIGO es su CLAVE.

No lo sabíais, pero ya estabais modelando.

## Hechos y dimensiones

En un almacén de datos, las tablas son de 2 tipos. SOLO DE 2 TIPOS.

TABLAS DE HECHOS
    Guardan LO QUE PASA. Los eventos. Lo que se mide.
    Una venta. Una llamada. Una reparación. Una encuesta.
    Son las tablas GORDAS: millones y millones de filas.
    Dentro llevan: las CLAVES que apuntan a las dimensiones + las METRICAS.

TABLAS DE DIMENSION
    Guardan EL CONTEXTO. Quién, qué, dónde, cuándo, cómo.
    Son las tablas PEQUEÑAS y ANCHAS: pocas filas, muchas columnas.
    Dentro llevan: ATRIBUTOS, que casi siempre son textos.

Y AHORA VIENE LO BUENO. Cómo decido yo qué va dónde?

CUIDADO CON UN ATAJO QUE SE OYE MUCHO y que está mal dicho:
"las cualitativas son dimensiones y las cuantitativas son hechos".
Suena bien pero es falso por los dos lados. Vamos despacio.

## Primero: DONDE ESTA EL DATO y DONDE ESTA SU CATALOGO

Cuando ayer recodificasteis tipo_primera_incidencia, lo que pasa es esto:

    EN CADA FILA DEL HECHO va el CODIGO:        ... | 2 | ...
    EN LA DIMENSION va el CATALOGO:             2 = "Avería técnica"

    O sea: EL VALOR SIGUE ESTANDO EN EL HECHO. Lo que se va a la dimensión
    es la TRADUCCION. El diccionario que permite leer ese 2.

No es que las cualitativas "se vayan" de la tabla de hechos. Es que su
DESCRIPCION se guarda una sola vez en otro sitio, en vez de repetirla
un millón de veces.

## Segundo: hay 2 casos distintos, y no se comportan igual

VALOR CATEGORICO
    Un código y su etiqueta. Y se acabó. No tiene nada más detrás.
        tipo de incidencia, forma de pago, canal, recomendaría sí/no
    -> El código va en el hecho. La dimensión tiene 2 columnas y 8 filas.

ENTIDAD
    Algo que tiene MUCHOS atributos propios.
        cliente, producto, tienda, empleado
    -> En el hecho va SOLO LA CLAVE (id_cliente).
       El nombre, el DNI, el sexo, la comunidad... viven en DIM_CLIENTE.
       El nombre del cliente NO aparece en la tabla de hechos.

    Por eso DIM_INCIDENCIA tiene 2 columnas y DIM_CLIENTE tiene 8.

## Tercero: CUANTITATIVA NO ES LO MISMO QUE METRICA

Y aquí es donde el atajo falla del todo. Mirad:

    Edad del cliente          cuantitativa, en años   -> NO es una métrica
    Metros cuadrados tienda   cuantitativa, en m²     -> NO es una métrica
    Año de fabricación        cuantitativa            -> NO es una métrica

Las tres tienen unidad de medida. Las tres son cuantitativas de libro.
Y ninguna es un hecho. Por qué? Porque NO DESCRIBEN EL EVENTO: describen a
alguien que participa en el evento.

    LA PREGUNTA QUE LO DECIDE DE VERDAD:

        Esto, ¿SE MIDE EN EL MOMENTO EN QUE PASA EL EVENTO?
        Y si lo SUMO a lo largo de muchas filas, ¿el resultado significa algo?

        SI a las dos  -> METRICA. Se queda en el hecho.
        NO            -> ATRIBUTO. Vive en su dimensión.

    Columna                    ¿Se mide en el evento?   Dónde vive
    -------------------------------------------------------------------
    Importe de la venta        SI, en esa venta         MÉTRICA
    Unidades vendidas          SI                       MÉTRICA
    Duración de la llamada     SI, en esa llamada       MÉTRICA
    Edad del cliente           NO, es del cliente       Atributo DIM_CLIENTE
    m² de la tienda            NO, es de la tienda      Atributo DIM_TIENDA

    LA PRUEBA RAPIDA:
        Sumo la columna en 1.000 filas. El resultado significa algo?
        Sumar importes      -> da la facturación.     SI significa algo.
        Sumar edades        -> no da NADA.            No es métrica.

## Entonces, para qué sirve el tipo estadístico?

Sirve, y mucho. Pero como PRIMER FILTRO, no como respuesta final:

    NOMINAL u ORDINAL  ->  SEGURO que es contexto. Va a tener su catálogo.
    CUANTITATIVA       ->  DEPENDE. Hay que preguntarse lo de arriba.

El que tiene claro el tipo estadístico de cada columna lleva medio camino.
Pero le queda la otra mitad.

## El modelo del call-center

Vuestro fichero de ayer, montado como modelo:

                            DIM_FECHA
                                |
        DIM_CLIENTE ----- HECHO_ENCUESTAS ----- DIM_PRODUCTO
                                |
              DIM_OPERADOR -----+----- DIM_SATISFACCION
                                |
                    DIM_INCIDENCIA -+- DIM_RECOMENDACION

    HECHO_ENCUESTAS
        id_encuesta          <- se queda aquí. Es la clave del evento.
        id_fecha             -> DIM_FECHA
        id_cliente           -> DIM_CLIENTE
        id_producto          -> DIM_PRODUCTO
        id_operador          -> DIM_OPERADOR
        id_satisfaccion      -> DIM_SATISFACCION
        id_incidencia        -> DIM_INCIDENCIA
        ------------------ METRICAS ------------------
        duracion_llamada_seg
        num_incidencias_previas
        encuestas = 1        <- el contador. Ya veréis para qué.

Y fijaos dónde ha acabado la tabla que hicisteis ayer: es DIM_INCIDENCIA.
Con sus 8 filas. Incluidas las de "No aplica" y "No informado".

    Y ESTO ES IMPORTANTISIMO:

    En un almacén NUNCA se deja un hueco apuntando a una dimensión.
    Si lo dejas, al cruzar las tablas ESE REGISTRO DESAPARECE del informe.
    Y los totales dejan de cuadrar SIN QUE NADIE SEPA POR QUE.

    Por eso se crean miembros explícitos: el 0 y el 99 que pusisteis vosotros.

## La trampa de la satisfacción

Mirad DIM_SATISFACCION. Es una DIMENSION.

Pero satisfaccion_1_10 es un NUMERO! Del 1 al 10! No debería ser un hecho?

    PUES NO. Y aquí es donde se cae todo el mundo.

Es un número, sí. Pero es una variable ORDINAL: no tiene unidad de medida.
Lo que para un cliente es un 7, para otro es un 5.

Y las variables ordinales NO SE SUMAN NI SE PROMEDIAN.

Entonces qué hago con ella? CONTAR. Cuántas encuestas tengo con un 8? Y con un 3?
Por eso va como DIMENSION, y la métrica que se cuenta es "encuestas = 1".

    Si la meto como HECHO, qué va a pasar?
    Que el primero que llegue va a poner PROMEDIO(satisfaccion) en el cuadro de
    mando. Y ya hemos mentido.

    EL TIPO ESTADISTICO DECIDE EL MODELO. No es teoría. Es esto.

## LA GRANULARIDAD (el grano)

Esta es LA DECISION MAS IMPORTANTE de todo el proyecto. Y hay que tomarla LA
PRIMERA, antes que ninguna otra.

El grano es: QUE REPRESENTA UNA FILA de la tabla de hechos.

Y la prueba es completar esta frase sin dudar:

    "Una fila de HECHO_ENCUESTAS es ................"

Opciones que teníamos:

    ... una encuesta respondida                        <- MAXIMO DETALLE
    ... las encuestas de un producto y un día
    ... las encuestas de un día
    ... las encuestas de un mes                        <- MINIMO DETALLE

Ahora bien, CUIDADO: no penséis que un almacén tiene UN grano y ya está.
Eso es otro atajo que se cuenta mucho y que es falso. Un almacén de verdad
tiene VARIAS tablas de hechos y VARIOS granos. Por dos motivos distintos:

## 1. Cada proceso de negocio tiene SU grano

Y esto no es una excepción rara. Es lo normal:

    HECHO_VENTAS        línea de ticket             el máximo detalle
    HECHO_OBJETIVOS     tienda y mes                nadie pone objetivos por línea
    HECHO_STOCK         producto, almacén y día     es una foto diaria

Los tres conviven en el mismo modelo, y comparten dimensiones (la de fecha,
la de tienda, la de producto). Cada uno con el grano que le corresponde.

## 2. Y ENCIMA, agregados precalculados

Si resulta que el 90% de las consultas de dirección van siempre por tienda y
por mes... POR QUE VOY A RECORRER 40 MILLONES DE LINEAS DE TICKET CADA VEZ?

    HECHO_VENTAS         línea de ticket             la tabla base
    HECHO_VENTAS_DIA     tienda, producto y día      agregado precalculado
    HECHO_VENTAS_MES     tienda, familia y mes       agregado precalculado

Esto NO ES HACER TRAMPA. Es exactamente lo mismo que os conté con el día de la
semana: precalcular lo que se va a consultar mucho. Solo que ahí precalculaba
una COLUMNA y aquí precalculo FILAS ENTERAS.

    Y TIENE SU COSTE, que hay que decirlo:
    Cada agregado es UNA TABLA MAS que mantener, UN PROCESO DE CARGA MAS,
    y UN SITIO MAS donde las cifras se pueden desviar. Si mañana alguien
    cambia una regla de negocio y se olvida de actualizar un agregado,
    tienes dos números distintos para la misma pregunta.

## Y entonces qué regla queda?

Solo una, y es sobre LA TABLA BASE:

    LA TABLA BASE DE CADA PROCESO, AL DETALLE QUE PUEDAS PERMITIRTE.

Por qué? Pues por lo mismo que os dije el primer día con las escalas de medida:

    Del DETALLE puedo AGREGAR siempre que quiera.
    Del AGREGADO no puedo BAJAR NUNCA.

    Cuantitativo -> Ordinal -> Nominal      SI
    Al revés                                NUNCA

Es exactamente el mismo principio.

Los agregados los puedo reconstruir cuando quiera a partir del detalle.
El detalle no lo reconstruyo a partir de nada.

    OJO CON ESTO, que es de las cosas que MAS DUELE en un proyecto:

    Si cargo el almacén al grano de "tienda y mes", el día que alguien pregunte
    "qué productos se venden juntos?" o "a qué hora vendemos más?"...
    NO PUEDO CONTESTAR. NUNCA. El dato ya no está.

    Y arreglarlo significa recargar TODO EL HISTORICO... si es que las fuentes
    todavía lo conservan. Que muchas veces no.

Y en vuestro fichero hay una cosita que afecta al grano, y la encontrasteis ayer:
LOS 14 DNIs REPETIDOS.

    Son 2 encuestas distintas del mismo cliente?  -> el grano es LA ENCUESTA
    O es un error y está la misma metida 2 veces? -> hay que deduplicar antes

    ESA DECISION LA TIENE QUE TOMAR EL NEGOCIO. No vosotros. Y define el modelo.

## Estrella y copo de nieve

Hay 2 formas de dibujar esto.

ESQUEMA EN ESTRELLA (star schema)
Cada dimensión, UNA SOLA TABLA. Aunque se repita información.

        DIM_FECHA      DIM_PRODUCTO
              \           /
               \         /
        DIM_CLIENTE - HECHO - DIM_TIENDA
               /         \
              /           \
        DIM_OPERADOR   DIM_INCIDENCIA

    DIM_TIENDA
    | id | tienda   | ciudad   | provincia | comunidad | region |
    |  1 | Alcalá   | Alcalá   | Madrid    | Madrid    | Centro |
    |  2 | Getafe   | Getafe   | Madrid    | Madrid    | Centro |
    |  3 | Sabadell | Sabadell | Barcelona | Cataluña  | Este   |

    Fijaos que "Madrid" y "Centro" se repiten. Y NO PASA NADA.

ESQUEMA EN COPO DE NIEVE (snowflake)
Las dimensiones se NORMALIZAN. Se parten en varias tablas encadenadas.

        DIM_TIENDA -> DIM_CIUDAD -> DIM_PROVINCIA -> DIM_COMUNIDAD -> DIM_REGION

    Ahorro espacio, porque "Madrid" lo escribo una vez.
    Pero para saber la región de una venta tengo que dar 5 saltos.

Y CUAL USO?

    ESTRELLA. Casi siempre. Y por 2 motivos:

    1. El espacio es barato y los saltos son caros.
    2. Y sobre todo: EL USUARIO LO ENTIENDE. Abre la herramienta, ve 6 tablas
       y sabe qué es cada una. Con un copo de nieve ve 25 y se pierde.

    Acordaos: si el usuario no lo entiende, no lo usa. Y si no lo usa,
    el proyecto ha fracasado por muy bonito que sea el modelo.

    El copo de nieve compensa cuando la dimensión es ENORME (millones de filas)
    o cuando un nivel lo comparten varios hechos distintos.
    En vuestro fichero, con 5 comunidades y 5 productos? NI DE BROMA.

## Y hoy se aplana todavía más

Y aquí hay algo importante, que además enlaza con lo que os conté el otro día
sobre el almacenamiento ORIENTADO A COLUMNAS.

Los libros de modelo dimensional se escribieron en los noventa. Y entonces
el almacenamiento era POR FILAS y el espacio en disco era carísimo. Repetir
"Menchu Rodríguez" un millón de veces era impensable.

HOY LA CUENTA ES OTRA. Y mucha gente, en lugar de sacar los datos a una
dimensión, los DEJA APLANADOS en la propia tabla de hechos. Nombre incluido.

    Por qué? PARA NO TENER QUE HACER EL JOIN.
    Cada cruce entre tablas cuesta. Si me lo puedo ahorrar, me lo ahorro.

Y no sale caro en espacio, y ahora viene lo bonito:

    LOS MOTORES COLUMNARES HACEN LA NORMALIZACION ELLOS SOLOS, POR DENTRO.

    Se llama DICTIONARY ENCODING. El motor ve que esa columna tiene 28.000
    valores distintos repartidos en 40 millones de filas, y automáticamente
    se monta su propio diccionario y guarda códigos.

    O sea: por dentro acaba guardando EXACTAMENTE la estructura de código +
    catálogo que hemos estado dibujando... pero SIN QUE TU TENGAS QUE HACER
    EL JOIN al consultar.

    Te sale gratis por los dos lados.

Por eso en el mundo del data lake, con ficheros PARQUET, se aplana muchísimo
más de lo que dicen los libros clásicos.

    RESUMEN DE ESTO:
    - El modelo en estrella es un modelo LOGICO. Dice qué describe y qué se mide.
    - Que físicamente lo guardes en tablas separadas o aplastado en una sola
      tabla ancha es UNA DECISION DE MOTOR, no de modelo.
    - Y la distinción entre lo que describe y lo que se mide SOBREVIVE IGUAL
      en la tabla aplastada. Lo que cambia es dónde está guardado, no qué es.
      La edad del cliente sigue sin poderse sumar, esté donde esté.

## Jerarquías

Dentro de una dimensión, los atributos se organizan en NIVELES. De más general
a más concreto. Eso es una JERARQUIA.

    GEOGRAFIA   Región  ->  Provincia  ->  Ciudad  ->  Tienda
    PRODUCTO    Sección ->  Familia    ->  Marca   ->  Artículo
    TIEMPO      Año     ->  Trimestre  ->  Mes     ->  Día
    EMPRESA     Dirección -> Zona      ->  Tienda  ->  Empleado

Para qué sirven? PARA NAVEGAR. Ahora lo vemos con el cubo.

Una dimensión puede tener VARIAS jerarquías a la vez. El tiempo, por ejemplo:

    Año -> Trimestre -> Mes -> Día        (la natural)
    Año -> Semana -> Día                  (la semanal)

Y no encajan una en otra: una semana puede caer en 2 meses distintos.

## La dimensión FECHA

Esta merece capítulo aparte, porque está en TODOS los modelos del mundo y
SIEMPRE SE CONSTRUYE A MANO.

Se genera una tabla con UNA FILA POR DIA, cubriendo todo el histórico:

    fecha       año  trim mes  nombre_mes  semana  dia_sem   laborable  festivo
    2026-08-20  2026   3    8  Agosto        34    Jueves      Si         No
    2026-08-21  2026   3    8  Agosto        34    Viernes     Si         No
    2026-08-22  2026   3    8  Agosto        34    Sábado      No         No

Y AQUI ES DONDE ENCAJA LO QUE PEDISTEIS VOSOTROS AYER.

Cuando os pregunté qué columnas nuevas habría que calcular, salió:
"el día de la semana", "la franja horaria", "el mes".

PUES ESO ES ESTO. Lo habíais inventado sin saberlo.

Y por qué a mano y no calculándolo al vuelo? Porque:
- Lo calculo UNA VEZ al cargar, y no en cada una de las 50.000 consultas.
- Y porque hay cosas que NO SE PUEDEN CALCULAR de la fecha: si es festivo en
  Guadalajara, si es campaña de rebajas, si es puente. Eso hay que METERLO.

Un ejemplo de por qué importa: la Semana Santa. Unos años cae en marzo y otros en
abril. Si comparas "abril contra abril" sin tenerlo en cuenta, un año te sale un
crecimiento del 30% que no es real: es que este año la Semana Santa cayó en abril.

## Aditividad: la trampa de los ratios

Ya casi acabamos con el modelo. Pero queda una cosa, y es de las que más daño
hacen en cuadros de mando de verdad.

No todas las métricas se pueden sumar igual.

METRICAS ADITIVAS
    Se suman POR TODO. Importe, unidades, coste, segundos.
    Sumo las ventas de las 12 tiendas -> tengo la venta de la cadena. CORRECTO.

METRICAS SEMIADITIVAS
    Se suman por unas dimensiones y por otras NO.
    El caso típico: EL STOCK.
    Sumo el stock de las 12 tiendas de hoy -> stock total de hoy. CORRECTO.
    Sumo el stock de los 30 días del mes  -> UN NUMERO SIN NINGUN SENTIDO.
    Por la dimensión TIEMPO el stock no se suma: se coge el último, o la media.
    Igual pasa con: saldo de una cuenta, número de empleados, temperatura.

METRICAS NO ADITIVAS
    No se suman por NADA.
    Y aquí está el grupo peligroso: TODOS LOS RATIOS Y PORCENTAJES.
    Margen %, ticket medio, tasa de conversión, satisfacción media.

    UN PORCENTAJE NO SE SUMA. Vale, eso lo ve todo el mundo.
    PERO ES QUE UN PORCENTAJE TAMPOCO SE PROMEDIA. Y eso no lo ve casi nadie.

Vamos a verlo, que si no no se cree:

    Tienda      Ventas    Margen €   Margen %
    Alcalá     400.000     60.000     15,0 %
    Getafe     100.000     18.000     18,0 %
    Sabadell    50.000     10.000     20,0 %
    Vigo        30.000      6.600     22,0 %
    Lugo        20.000      5.000     25,0 %

    Cuánto es el margen de la CADENA?

    MAL:  (15 + 18 + 20 + 22 + 25) / 5 = 20,0 %
    BIEN: 99.600 / 600.000            = 16,6 %

    CASI 4 PUNTOS DE DIFERENCIA.

    Y no en un ejercicio de clase: en el indicador con el que la dirección
    decide la política de precios de todo el año.

Por qué falla? Porque la media simple trata IGUAL a Alcalá, que hace el 67% de la
facturación, que a Lugo, que hace el 3%. Y resulta que las tiendas pequeñas son
las que más margen porcentual tienen, y tiran del número hacia arriba.

    LA REGLA, y esto lo escribís en la pared:

    LOS RATIOS NO SE AGREGAN. SE RECALCULAN.

    En la tabla de hechos guardo el NUMERADOR y el DENOMINADOR por separado.
    Y el ratio lo calculo EN EL MOMENTO DE LA CONSULTA, al nivel que sea.

    Es decir: guardo importe y margen. NUNCA guardo margen_%.

El mismo error con otras caras (todas son la misma):
- Promediar el ticket medio de 12 meses para sacar el del año.
- Promediar la conversión de campañas de tamaños distintos.
- Promediar la satisfacción media de tiendas con 500 y con 12 encuestas.

---

# El CUBO y las operaciones OLAP

Cuando todo esto que hemos montado se prepara para consultarlo rápido, a la
estructura resultante la llamamos CUBO MULTIDIMENSIONAL.

Se llama cubo porque es fácil imaginarlo con 3 dimensiones:

                    PRODUCTO
                   /
                  /
                 +--------+--------+
                /        /        /|
               +--------+--------+ |
              /        /        /| +
             +--------+--------+ |/|
             |        |        | + |
    TIEMPO   |        |        |/| +
             +--------+--------+ |/
             |        |        | +
             |        |        |/
             +--------+--------+
                    TIENDA

Cada celdita contiene el valor de las métricas para una combinación:
"lo vendido de bicicletas, en Alcalá, en marzo".

Pero OJO: lo de 3 dimensiones es solo para poder dibujarlo. Un cubo real tiene
las que hagan falta: 8, 12, 20 dimensiones. Se sigue llamando cubo igual.

## Las 5 operaciones

Y esto es lo que hace un usuario con el ratón delante de un cuadro de mando.
No es teoría: es literalmente lo que va a hacer.

DRILL DOWN (bajar)
    Bajo un nivel en una jerarquía. De Región a Provincia. De Año a Mes.
    ES LA OPERACION ESTRELLA. Es con la que se DIAGNOSTICA.

        "Las ventas de la región Este han caído un 12%"
                -> drill down ->
        "No ha caído la región. Ha caído UNA tienda."

    Sin drill down, el cuadro de mando te dice QUE pasa pero no DONDE.
    Y entonces no sirve para decidir.

DRILL UP o ROLL UP (subir)
    Lo contrario. Subo un nivel y agrego. De Tienda a Región.

SLICE (rebanada)
    Fijo el valor de UNA dimensión y me quedo con esa "loncha".
    "Solo el mes de abril".

DICE (dado)
    Filtro por VARIAS dimensiones a la vez.
    "Bicicletas, en la región Este, en el segundo trimestre".

PIVOT (girar)
    Giro el cubo. Lo que tenía en filas lo pongo en columnas.
    "Ahora quiero los meses en filas y las familias en columnas".
    Los que hacéis tablas dinámicas en Excel lleváis años haciendo pivot.

---

# ROLAP, MOLAP y HOLAP

Estas 3 siglas os las van a preguntar. Son 3 formas de MONTAR un cubo por dentro.

MOLAP (Multidimensional OLAP)
    El cubo se guarda en una estructura multidimensional PROPIA del fabricante.
    Con TODAS las agregaciones YA CALCULADAS de antemano.
        VENTAJA:      Las consultas van como un tiro. Está todo hecho.
        INCONVENIENTE: El proceso de construirlo tarda horas. Y hay un límite
                       de volumen que no puedes pasar. Poco flexible.

ROLAP (Relational OLAP)
    NO hay cubo físico. Las consultas se traducen a SQL contra las tablas
    (el modelo en estrella que acabamos de ver).
        VENTAJA:       Escala muchísimo mejor. No hay límite práctico.
        INCONVENIENTE: Más lento al consultar, porque calcula al vuelo.

HOLAP (Hybrid OLAP)
    Mezcla. Los agregados en estructura multidimensional (rápido) y el detalle
    en las tablas relacionales (escalable).

## Y qué queda de todo esto hoy?

Pues os voy a ser sincero: CASI NADA. Y os explico por qué, porque el porqué sí
es importante.

    MOLAP existía para no tener que recorrer millones de filas al consultar.
    Era la única forma de que fuera rápido.

    Pero eso lo resolvimos ayer POR OTRO CAMINO. Os acordáis?

    EL ALMACENAMIENTO ORIENTADO A COLUMNAS.

Si los datos están guardados por columnas, comprimidos y en memoria, una consulta
que solo necesita 2 columnas de una tabla de 50 NO TOCA LAS OTRAS 48. Y va
igual de rápido que un MOLAP, pero SIN tener que precalcular nada y SIN límite
de volumen.

Eso es lo que hay por debajo de Power BI, de Tableau y de los modelos tabulares.

    MOLAP hoy solo lo vais a ver en instalaciones antiguas que nadie ha
    tocado en 10 años.

Lo que SI sobrevive intacto, y por eso lo hemos dado, es:
- El CUBO como MODELO LOGICO: hechos, dimensiones, jerarquías.
- Y las 5 OPERACIONES: drill down, drill up, slice, dice, pivot.

Eso lo vais a usar toda la vida.

---

# Las categorías de la minería de datos

El otro día os conté qué era la minería: pedirle a la máquina que busque patrones
que yo no sé ni que existen. Coger el pico y la pala.

Pero hay distintos TIPOS de búsqueda, según qué quiero que me devuelva.

CLASIFICACION
    Le pido que meta cada individuo en una CATEGORIA QUE YO YA CONOZCO.
        "Este cliente, se va a dar de baja: SI o NO?"
        "Este correo, es spam o no?"
        "Esta reparación, se va a retrasar?"
    Necesito ejemplos ya etiquetados para que aprenda. (SUPERVISADO)

REGRESION
    Igual que la clasificación, pero lo que devuelve es UN NUMERO. Un valor
    CONTINUO.
        "Cuántas unidades venderemos el mes que viene?"
        "Cuánto va a costar reparar esto?"
        "Cuál va a ser la temperatura mañana?"
    También necesita ejemplos. (SUPERVISADO)

    OJO A LA DIFERENCIA, que es la que preguntan:
        CLASIFICACION -> devuelve una ETIQUETA (sí/no, alto/medio/bajo)
        REGRESION     -> devuelve un NUMERO

AGRUPAMIENTO (clustering)
    Aquí NO le digo lo que busco. Le digo: "mira estos clientes y dime si se
    parecen entre sí de alguna forma".
        "Qué tipos de cliente tengo REALMENTE?"
    No hay respuesta conocida de antemano. (NO SUPERVISADO)

    Y esto es lo bonito: MUCHAS VECES TE CONTRADICE.
    Tú tienes a los clientes segmentados en oro, plata y bronce por lo que
    gastan. Y el algoritmo te dice que los grupos de verdad son otros:
    el que compra por urgencia, el que compra por rutina, el que compra por
    regalo. Y esos SI se comportan distinto.

REGLAS DE ASOCIACION
    Qué cosas OCURREN JUNTAS. Es el análisis de la cesta de la compra.
        "El que compra pañales compra cerveza"     (el ejemplo mítico)
        "El que compra bici de montaña compra casco el 62% de las veces,
         pero guantes solo el 11%"
    De ahí salen decisiones concretas: qué pongo al lado de qué en el lineal,
    qué recomiendo en la web, qué meto en un pack.

DETECCION DE ANOMALIAS
    Buscar lo que se sale del patrón. Es lo de los valores atípicos, pero
    hecho por una máquina y en continuo.
        "Esta tarjeta se está usando de forma rara" -> fraude
        "Esta máquina vibra distinto" -> se va a averiar
    Aquí el atípico NO es un estorbo: ES EL OBJETIVO.

ANALISIS DE SECUENCIAS
    Como las reglas de asociación, pero con el TIEMPO metido dentro.
    No qué se compra junto, sino QUE SE COMPRA DESPUES DE QUE.
        "El que compra una casa, a los 3 meses compra electrodomésticos"

---

# Correlación y causalidad

Esto se me quedó ayer y NO PUEDE QUEDARSE SIN DAR. Es de lo más importante del
curso, y es de lo que más se incumple ahí fuera.

Ayer vimos que al cruzar 2 variables cuantitativas hacemos un scatterplot, y que
si los puntos forman un patrón hay relación. A eso lo medimos con un número que
se llama COEFICIENTE DE CORRELACION, y va de -1 a +1:

    +1     relación perfecta: cuando sube una, sube la otra
    +0,7   relación fuerte hacia arriba
     0     no hay relación (lineal)
    -0,7   relación fuerte hacia abajo: sube una, baja la otra
    -1     relación inversa perfecta

3 avisos, y hay que darlos SIEMPRE LOS TRES:

1. MIDE RELACION LINEAL. Solo lineal.
   Una correlación de 0 no significa "no hay relación". Significa "no hay
   relación EN LINEA RECTA". Puede haber una relación en curva perfecta y
   darte 0.
   Ejemplo: la venta de helados y la temperatura. Sube y sube... hasta que a
   42 grados no sale nadie a la calle y se desploma. Eso no es una recta.

   POR ESO SE MIRA SIEMPRE EL SCATTERPLOT ANTES DE CREERSE EL NUMERO.

2. LE AFECTAN MUCHISIMO LOS ATIPICOS.
   Un solo Manolo puede crearte una correlación que no existe, o cargarse una
   que sí existía.

3. Y LA GORDA: CORRELACION NO ES CAUSALIDAD.

## Las 4 explicaciones

Cuando 2 cosas se mueven juntas, hay CUATRO explicaciones posibles. Y la gente
solo se plantea la primera:

    1. A CAUSA B
       La temperatura hace que se venda más helado. Vale.

    2. B CAUSA A
       Es al revés de lo que yo pensaba.

    3. UNA TERCERA VARIABLE CAUSA LAS DOS      <- LA VARIABLE DE CONFUSION
       Y ésta es la que hace daño de verdad.

    4. CASUALIDAD
       Si cruzo 400 variables entre sí, POR PURA ESTADISTICA me van a salir
       correlaciones altísimas que no significan absolutamente nada.

## El caso 3, que es el asesino

Ejemplo real de manual:

    OBSERVACION:  Las tiendas con más empleados venden más.
                  Correlación +0,8. Buenísima.

    CONCLUSION:   Contratemos más gente en todas las tiendas!

    REALIDAD:     Las tiendas que están en ciudades grandes tienen más
                  empleados Y venden más.
                  LA POBLACION DE LA CIUDAD causa las dos cosas.

    Y si contrato 2 personas más en Lugo, NO VOY A VENDER MAS.
    Porque no era la plantilla lo que causaba las ventas.

Otro clásico, para que se os quede:

    Correlación altísima entre el consumo de helados y los ahogamientos.
    Prohibimos los helados?
    La variable de confusión es EL CALOR. Con calor se come más helado Y se
    baña más gente.

## Y entonces, cómo demuestro una causa?

Hacen falta 3 cosas, y la correlación sola no da ninguna de las 3:

    1. PRECEDENCIA: la causa tiene que ocurrir ANTES que el efecto.
    2. ASOCIACION: cuando cambia la causa, cambia el efecto.
    3. QUE NO HAYA OTRA EXPLICACION: hay que descartar las variables de
       confusión.

Y la única forma limpia de conseguir las 3 es EL EXPERIMENTO:
Elijo AL AZAR quién recibe el tratamiento y quién no, y comparo.

En el mundo de la empresa eso se llama PRUEBA A/B, y se hace constantemente:
    Lanzo la promoción en 6 tiendas elegidas al azar y en las otras 6 no.
    Y a los 2 meses comparo.

    SI UNA DECISION IMPORTANTE DEPENDE DE UNA RELACION CAUSAL,
    HAY QUE HACER EL EXPERIMENTO.

    Y si no se puede hacer, hay que decir CLARAMENTE en el informe que eso
    es una HIPOTESIS y no un hecho. Que no se nos olvide.

---

# La gestión de un proyecto de BI

Y acabamos con esto, que también entra.

## Por qué un proyecto de BI no es como los demás

Un proyecto informático normal (una aplicación de facturación, por ejemplo)
tiene un criterio de éxito OBJETIVO: la factura sale o no sale.

En BI NO. En BI el criterio de éxito es que alguien ENTIENDA MEJOR SU NEGOCIO
Y DECIDA DISTINTO. Y eso:
- Es dificilísimo de especificar antes de empezar.
- Y es dificilísimo de comprobar al terminar.

De ahí salen los 3 rasgos que hacen estos proyectos distintos:

1. EL ALCANCE SE DESCUBRE TRABAJANDO.
   Nadie sabe qué hay dentro de una fuente de datos hasta que la abre.
   Ayer lo vivisteis: hasta que no perfilasteis, no sabíais lo que había.

2. EL ESFUERZO ESTA EN LOS DATOS, NO EN LA HERRAMIENTA.
   Hacer el cuadro de mando es la parte rápida y barata.
   Preparar los datos es lo caro. Y esto os lo llevo diciendo 3 días.

3. EL VALOR ESTA EN EL USO, NO EN LA ENTREGA.
   Un cuadro de mando terminado que nadie abre ES UN PROYECTO FRACASADO,
   aunque haya cumplido plazo y presupuesto.

## El riesgo número uno: LA AMBIGÜEDAD DE LOS REQUISITOS

El usuario NO SABE LO QUE QUIERE hasta que ve algo.

    "Quiero un cuadro de mando de ventas."
    Vale. Se lo hago. 3 semanas.
    Se lo enseño.
    "Ah, no, pues yo esto lo quería por comercial, no por tienda. Y con el
     año pasado al lado. Y sin las devoluciones."

Y NO ES QUE SEA UN CAPRICHOSO. Es que hasta que no lo ha visto, no ha podido
saber qué necesitaba. Es NORMAL. Y hay que contar con ello.

    Este es EL RIESGO ESPECIFICO de los proyectos de BI.
    Que se caiga el aire acondicionado le puede pasar a cualquier proyecto.
    Esto solo nos pasa a nosotros.

## Y de ahí salen 2 cosas: la ENTREGA ITERATIVA y el CONTROL DE CAMBIOS

ENTREGA ITERATIVA (los enfoques ágiles, Scrum y compañía)
    Si el usuario no sabe qué quiere hasta que lo ve, la única forma sensata
    de trabajar es: ENSEÑARLE ALGO PEQUEÑO Y PRONTO.

    Ciclos cortos, entregas parciales, y corregir con lo que diga.

    Mejor un cuadro de mando de UN AREA funcionando en 6 semanas,
    que el almacén corporativo completo en año y medio.
    (Que además, cuando lo entregues, ya no será lo que querían.)

CONTROL DE CAMBIOS
    Ahora bien. Si todo se puede cambiar siempre, el proyecto no acaba nunca.
    Y ahí está el equilibrio.

    El control de cambios es un procedimiento FORMAL: una vez que el alcance
    está acordado y cerrado, toda petición nueva:
        - se escribe
        - se valora (cuánto cuesta, a qué afecta, qué retrasa)
        - y alguien la aprueba o la rechaza

    Sin eso pasa lo que se llama SCOPE CREEP: el alcance va creciendo poco a
    poco, "es solo una columnita más", y nadie ajusta ni el plazo ni el
    presupuesto. Y el proyecto muere de sobrepeso.

## Los roles. Quién hace falta

El otro día os dije los perfiles técnicos: analistas de negocio, analistas de
datos, programadores de ETL, administradores de BBDD.

Pero falta el más importante de todos, y no es técnico:

    EL PATROCINADOR (SPONSOR)

    Es alguien de DIRECCION. Y hace 2 cosas que no puede hacer nadie más:
        1. Consigue el PRESUPUESTO y las PERSONAS.
        2. Mantiene la PRIORIDAD del proyecto cuando aparece cualquier
           urgencia operativa. Y aparecen todas las semanas.

    Sin patrocinador, un proyecto de BI se queda sin gente en cuanto se cae un
    servidor de producción. Porque lo urgente siempre le gana a lo importante.

Y hay otro que también es clave:

    EL EXPERTO DE NEGOCIO (Subject Matter Expert, SME)

    Es el que sabe qué significan los datos de verdad.

    Si el equipo no tiene acceso a él durante el análisis, qué pasa?
    Pues que las decisiones LAS ACABA TOMANDO EL QUE PROGRAMA LA CARGA.
    Que es JUSTO EL QUE MENOS CONTEXTO TIENE.

    Y son decisiones de negocio disfrazadas de técnicas:
        Una devolución resta del mes de la venta o del mes de la devolución?
        Un traspaso entre tiendas es una venta?
        Las 2 encuestas del mismo DNI son una o dos?

    Ayer os las encontrasteis vosotros. Y ninguna se puede decidir mirando
    el fichero.

    RESULTADO SI NO HAY EXPERTO: el modelo sale IRRELEVANTE O INCORRECTO
    para lo que el negocio necesita de verdad.

## Las fases

    1. VIABILIDAD / VISION
       Qué decisión queremos mejorar? Merece la pena el dinero?
       Y aquí está el producto clave de la planificación inicial:
       LA ESPECIFICACION DE LOS CUADROS DE MANDO Y LOS KPIs.
       Es decir: QUE PREGUNTAS QUEREMOS PODER RESPONDER.

       OJO: no es "la lista de tablas que tengo". Eso es empezar por el final.
       Es "lo que quiero poder contestar". De ahí sale todo hacia atrás.

    2. ANALISIS
       Inventario de fuentes, perfilado, calidad, definición de indicadores.
       (Esto es lo que hicisteis ayer.)

    3. DISEÑO
       El modelo dimensional. Grano, hechos, dimensiones, jerarquías.
       (Esto es lo de hoy.)

    4. CONSTRUCCION / DESARROLLO
       Aquí es donde el equipo técnico se pasa la mayor parte del tiempo.
       Y en qué? EN IMPLEMENTAR Y PROBAR LAS ETLs.
       No en hacer gráficas. En las ETLs.

    5. PRUEBAS Y VALIDACION
       Y validar no es "que no dé error". Validar es enseñarle al responsable
       de la tienda SU cifra y ver si la reconoce.
       Si no la reconoce, hay un problema. Y hay que resolverlo ANTES de
       publicar.

    6. CIERRE Y DESPLIEGUE
       El GO-LIVE: el traspaso del sistema a operación.
       Y la FORMACION FINAL de los usuarios.
       Que sin formación, por muy bueno que sea, no lo usa nadie.
       Y ya sabéis: si no lo usan, ha fracasado.

## Y una cosa más: el REPARTO REAL del esfuerzo

Para que no os pillen desprevenidos cuando os toque planificar uno:

    Análisis, definición y acuerdo de indicadores     20 %
    Perfilado y calidad de los datos                  25 %
    Desarrollo de los procesos de carga (ETL)         30 %
    Modelo y cuadros de mando                         15 %
    Validación, formación y ajustes                   10 %

    Fijaos: 55% en datos. 15% en lo que la gente cree que es el proyecto.

    Un plan que dedique la mitad del tiempo a hacer cuadros de mando
    ESTA MAL HECHO. Y va a llegar tarde.

Y las dependencias que de verdad paran un proyecto de BI casi nunca son técnicas.
Son:
    - El permiso para leer una base de datos, que tarda 3 semanas.
    - La persona que conoce el sistema antiguo, que está de vacaciones.
    - La decisión pendiente sobre qué es un "cliente activo", que nadie toma.

    Esas hay que identificarlas EL PRIMER DIA y ponerlas encima de la mesa.
