
# Ayer / RESUMEN

Estadística:
    - Graficas de barras y de sectores
    - Gráficos adicionales:
      - Histogramas
      - Diagramas de caja y bigotes (boxplots)
    - Estadística BIVARIABLE:
      - Nominal x Nominal           -> Tablas de contingencia -> Gráficas de barras apiladas/agrupadas
      - Nominal x Ordinal           -> Tablas de contingencia -> Gráficas de barras apiladas/agrupadas + Estudio de comparación de medianas
      - Nominal x Cuantitativa      -> Tablas de contingencia -> Gráficas de barras apiladas/agrupadas
                                                              -> Pirámides poblacionales
                                                              -> Heatmaps
                                                              -> Boxplot múltiples
                                                              -> Histogramas apilados 
                                    -> Estudio de comparación de medianas/medias
      - Ordinal x Ordinal           -> Como si fuera NOMINAL x ORDINAL
      - Ordinal x Cuantitativa      -> Como si fuera NOMINAL x CUANTITATIVA
      - Cuantitativa x Cuantitativa -> Gráfico de dispersión (Scatterplot)

Ejercicio práctico: Llamadas de un CallCenter sobre Encuestyas de satisfacción de clientes.
                    Analizar los datos:
                        - Tipos de columnas: Desde el punto de vista estadístico y desde el punto de vista informático.
                        - Calidad del dato:  Problemas detectados y propuesta de transformación (NORMALIZACIONES)
                            comunidad_autonoma
                            tipo_de_primera_incidencia  -> Nominal
                            nivel_de_satisfaccion       -> Ordinal
                            duracion_de_llamada_seg     -> Cuantitativa 

---

# HOY

Conceptos teóricos más clásicos del mundo BI:
- √ La pirámide organizativa de la información: Qué decide cada quién dentro de la organización y cómo se relaciona con los datos.
- √ Almacenes de datos (DATAWAREHOUSE): Qué le pedimos y que es un DATAMART
- √ Modelos de datos para DATAWAREHOUSE: Qué es un modelo en estrella y qué es un modelo en copo de nieve. Tablas de hechos y tablas de dimensiones. 
- √ CUBO OLAP... y las operaciones que hacemos habitualmente sobre él: Drill Down, Drill Up, Slice & Dice, Pivoting.
- √ Formas de almacenar datos: ROLAP vs HOLAP vs MOLAP. Ventajas e inconvenientes de cada uno.
- √ Data mining + Machine Learning
- Gestión de proyectos BI.

- EXAMEN!

---

# La pirámide organizativa (desde el punto de vista de BI)


Punto importante: Dentro de la empresa NO TODO EL MUNDO DECIDE LO MISMO. No van a usar para la toma de decisiones ni la misma información ni los mismos datos.

        ---------------------
          NIVEL ESTRATEGICO                 Necesita tomar decisiones con vista AÑOS!  
          Dirección
        ---------------------
          NIVEL TACTICO                     Necesita tomar decisiones con vista MESES!
          Mandos intermedios
          Menos gente
        ---------------------
          NIVEL OPERATIVO                   Necesita tomar decisiones sobre el HOY!
          Personal de a pie 
          BASE DE LA PIRAMIDE

El encargado de una tienda necesita saber:  Los pedidos que tiene que preparar HOY, para mañana tenerlos listos.
                                            Con qué gente cuénta hoy

El responsable de una comunidad autónoma:   Cómo van sus tiendas.
                                            Dónde mete promociones
                                            Dónde hace falta más personal
                                            Dónde sobra personal
                                            NECESITA EL DETALLE DE SU TIENDA

                                            A esta persona, los pedidos que hay hoy le dan igual
                                            A esta persona la gente que hay hoy trabajando en la tienda X le da igual
                                            NECESITA UN AGREGADO DE DATOS DE SUS TIENDA (Posiblemente agrega por tienda y por fecha)

El director general de la empresa:          Abrimos una tienda nueva en tal sitio?
                                            Dónde invierto 500.000€ en los próximos 6 meses/2 años.
                                            NECESITA DATOS MUY AGREGADOS y con una serie de tiempo muy larga (3/5 años de datos)


Cada uno de esos perfiles usa un tipo de sistema INFORMATICO diferente:

    TPS             Transaction Processing System (OLTP) Nivel OPERATIVO
                    El ordenador de la tienda. El gestor de expedientes, El ERP.
    MIS             Management Information System (OLAP) Nivel TACTICO
                    Informes periódicos para mandos intermedios. Tableros de control. Informes trimestrales / mensuales / semanales
    DSS/EIS         Decision Support System / Executive Information System (OLAP) Nivel ESTRATEGICO
                    El objetivo de estos sistemas es ayudar a decidir sobre problemas MAL ESTRUCTURADOS.
                    Problemas que no se pueden contestar con una FORMULA!
                    Abrimos en Bilbao o no?

    Cuanto más arriba, más agregados necesito los datos
    Cuanto más abajo, menos agregados necesito los datos

Cómo guardo entonces los datos en el DATAWAREHOUSE para su análisis? NO HAY RESPUESTA GENERICA

> EJEMPLO:
> Sease un conjunto de datos con ventas de videojuegos. En el excel tenemos estos datos:
1. Nombre
2. Plataforma
3. Año
4. Genero
5. Editorial
6. Ventas NA
7. Ventas EU
8. Ventas JP
9. Ventas Otros
10. Ventas Global

Cuántas variables tengo? 10 variables? NO
    - Nombre es un IDENTIFICADOR
    - Plataforma? NOMINAL
    - Año? CUANTITATIVA
    - Genero? NOMINAL
    - Editorial? NOMINAL
    - Ventas NA? Esto no es una variable! Es una FRECUENCIA!
    - Ventas EU? Esto no es una variable! Es una FRECUENCIA!
    - Ventas JP? Esto no es una variable! Es una FRECUENCIA!
    - Ventas Otros? Esto no es una variable! Es una FRECUENCIA!
    - Ventas Global? Esto no es una variable! Es una FRECUENCIA! = Ventas NA + Ventas EU + Ventas JP + Ventas Otros
    - REGION: NA, EU, JP, Otros. Esto es una variable NOMINAL que no está en el excel pero que se puede crear a partir de las columnas de ventas.

Y resulta que tenemos menos variables de las que parecía y algunas mas que estaban ocultas.

Esa tabla es una tabla AGRUPADA de antemano.
La tabla original sería algo como:

    VENTAS DE VIDEOJUEGOS
    CLIENTE     FECHA       TIENDA  JUEGO       TIPO    Editorial   REGION      AÑO DEL JUEGO   PLATAFORMA
    Menchu      10-10-2015  A       Tetris      Puzzle  QuienSEA    NA          2013            PC
    Federico    11-10-2015  A       Tetris      Puzzle  QuienSEA    NA          2013            XBOX

Y esa tabla la han agrupado por juego, plataform y por región, y han sumado las ventas de cada juego para cada plataforma en cada región. (FRECUENCIA)

La tabla agrupada se podría haber representado de otra forma:

    VENTAS AGRUPADAS DE VIDEOJUEGOS POR REGION Y PLATAFORMA

    JUEGO   PLATAFORMA   AÑO     GENERO  EDITORIAL                          REGION     VENTAS 
    Alien   2600         1981    Action  20th Century Fox Video Games       NA           0,74
    Alien   2600         1981    Action  20th Century Fox Video Games       EU           0,04 
    Alien   2600         1981    Action  20th Century Fox Video Games       JP           0
    Alien   2600         1981    Action  20th Century Fox Video Games       Otros        0,01
    Tiene 64.000 filas (cada fila de la de abajo se ha convertido en 4 filas, una por cada región)

Pero tampoco me han dado esta tabla así.
Han convertido varias filas en columnas, mediante una operación llamada PIVOT. Y eso es lo que me han dado en el excel.

    Nombre                  Plataforma      Año         Genero      Editorial       Ventas NA   Ventas EU  Ventas JP    Ventas Otros   Ventas Global
    Boulder Dash: Rocks!    DS              2007        Puzzle      10TACLE Studios 0           0,03       0            0              0,03
    Tiene 16.000 filas

Por supuesto, si tengo los datos guardados con el mayor nivel de granularidad posible: PRIMERA TABLA!

    CLIENTE     FECHA       TIENDA  JUEGO       TIPO    Editorial   REGION      AÑO DEL JUEGO   PLATAFORMA
    Menchu      10-10-2015  A       Tetris      Puzzle  QuienSEA    NA          2013            PC
    Federico    11-10-2015  A       Tetris      Puzzle  QuienSEA    NA          2013            XBOX
    ....

podrá sin problemas calcular las otras 2 tablas.
Ahora bien.. podéis decirme el tamaño de esa tabla? Cuántas filas tiene? 8.800.000.000 datos: LOCURA!

Si tengo que generar desde esa tabla la segunda... se va a llevar un rato la computadora.
    8.800.000.000 datos -> 64.000 filas ES MUCHO TRABAJO DE COMPUTACION.. Puede tardar minutos/horas en calcularse.
    64.000 filas -> 16.000 filas ESTO YA NO ES TANTO TRABAJO... pero aún así, si se hace muchas veces cuidado! que suma

    Cómo me interesa guardar los datos? 
    Posiblemente de las 3 formas. Depende a quien se le den los datos.

    NIVEL OPERATIVO -> Necesita la tabla original, con el mayor nivel de granularidad posible.  BBDD PRODUCCION.    OLTP
    NIVEL TACTICO   -> Necesita la tabla intermedia, con un nivel de granularidad medio.        DATAWAREHOUSE.      OLAP
    NIVEL ESTRATEGICO-> Necesita la tabla final, con el menor nivel de granularidad posible.    DATAWAREHOUSE.      OLAP
                        Esta la guardo por separado... o no (se calcula bajo demanda si no tarda mucho: DECISION?! hay que tomarla)
    
---

# DATAWAREHOUSE

Hemos hablado que en el datawarehouse guardamos datos YA COCINADOS... Y ya vamos entendiendo un poco el concepto!
Cómo vamos preparando datos para los análisis que me interesan.
A los datawarehouse les pedimos una serie de cosas (4) . Eso lo definió un hombrecillo que estudió bastante este tema: Bill Inmon.

1. INTEGRADO
Los datos vienen de distintos sitios. Y cada sitio los escribe a su forma.
En el datawarehouse no puedo dejar los datos de esa forma (como estaban en origen).
Tengo que unificar TODO BAJO UN CRITERIO COMUN.
Mismos código, Mismas unidades, mismos nombres.
A este proceso le llamamos CONFORMACION DE DATOS.

2. TEMATIZADA (orientada a un tema)

Los datawarehouse los organizamos por áreas de negocio.
NUNCA POR LA APLICACION DE LA QUE VIENE EL DATO!

Si yo soy de ventas, quiero ver datos de ventas.. con independencia de que el dato venga de la aplicación de ventas, de la aplicación de facturación, de la aplicación de almacén, etc.

    AREAS DE NEGOCIO
    Ventas
    Clientes
    Taller
    Stock
    ...
A cada uno de esos conjuntos, creados para un área de negocio, le llamamos DATA MART.
Un DATA MART Es un trozo del DATAWAREHOUSE, orientado a un área de negocio concreta.

> Por qué agupamos así los datos? Y hay 3 motivos clave:

- Rendimiento. Si una persona necesita acceso solamente a datos de ventas, no le doy acceso a todo el datawarehouse. Le doy acceso a su data mart. Y eso es mucho más rápido.
               Muchos menos datos = Mucho más rápido
- Simplicidad: Si yo necesito datos de ventas (que lo mismo son 8/10 tablas de datos) ... 
               No me saques en la pantalla 800 tablas de otros áreas de negocio que no me interesan. 
- Seguridad:   Si yo soy de ventas, no debo poder acceder a datos de clientes, proveedores.

3. NO VOLATIL

Dentro del datawarehouse:
    NO SE MODIFICA NADA!
    NO SE BORRA NADA!
    SOLO SE AÑADEN COSAS! <<< IMPORTANTISIMO
    
El datawarehouse NO ES UNA BBDD de producción, donde doy de alta pedidos, cancelo otros...
El datawarehouse es un almacén de datos, donde guardo datos históricos para poder analizarlos y tomar decisiones.

Si hace 2 años saqué un informe, debería poder volver a sacar hoy el mismo informe, con los mismos datos! 
- CONFIANZA
- AUDITORIA

4. HISTORICO
Pero esto no significa solo que guardemos todos los datos antiguos.
LO IMPORTANTE ES QUE GUARDAMOS LOS DATOS COMO ERAN CUANDO OCURRIERON.

> Ejemplo: 
Tengo una tienda en la región A.
Pero de repente nos reorganizamos... y esa tienda pasa a la REGION B.
A qué región imputo las ventas?
- Hasta la fecha de reorganización, las ventas de esa tienda van a la REGION A.
- A partir de la fecha de reorganización, las ventas de esa tienda van a la REGION B.

En una BBDD de producción, cambiaría la tienda de región. Y PUNTO PELOTA. Y guardo lo que está vigente!
En el datawarehouse, no. Guardamos los datos como eran cuando ocurrieron.
Y guardaré las ventas antiguas computadas a la REGION A y las ventas nuevas computadas a la REGION B.

Tengo que poder ver que antes esa tienda estaba en la REGION A y ahora está en la REGION B. Y eso es lo que guardo en el datawarehouse.

---

# MODELADO DE DATOS EN UN DATAWAREHOUSE

Esto es gordo e importante!

Ayer, cuando analizaqmos los campos del fichero de encuestas, salió este tema:

    CAMPO: tipo_de_primera_incidencia

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

Hicimos una tabla de normalización de los tipos de incidencia.
Esta tabla que hicimos ayer TIENE NOMBRE en un datawarehouse: TABLA DE DIMENSIONES.
Y el CODIGO QUE PUSIMOS ES SU CLAVE.

Ayer no lo sabíamos.. pero en realidad ya estábamos modelando EL DATAWAREHOUSE.

En un datawarehouse, tenemos 2 tipos de tablas:
- TABLAS DE HECHOS: Son las que contienen los datos que queremos analizar.

    GUARDAMOS LO QUE OCURRE. Los EVENTOS. Los HECHOS. Lo que queremos analizar.
        VAN ASOCIADOS A UNA FECHA.
    Una venta, una llamada, una reparación, una encuesta.
    O GRUPOS DE ELLAS.

    Son tablas grandes, con muchas filas y muchas columnas.

- TABLAS DE DIMENSIONES: Son las que contienen los datos que describen a los datos que queremos analizar.

    Guardan CONTEXTO: Quién, qué, dónde, cuándo, cómo, por qué, con qué, para qué.
    Son tablas pequeñas, con pocas filas y columnas.... pueden tener pocas o muchas columnas... pero filas POCAS!

Y cómo decido dónde va cada dato... Si un dato debe ir a una tabla de hechos o a una tabla de dimensiones?
- Toda variable cualitativa va a una tabla de dimensiones. REALMENTE LO QUE TENDRE ES SU NORMALIZACION / CODIFICACION
- Toda variable cuantitativa va a una tabla de hechos (casi siempre... gloriosas excepciones)

> Vamos a plantear el ejemplo de ayer: CALLCENTER

Tabla de hechos: Llamadas a cliente para encuestas de satisfacción.
    - Cada fila es una llamada a un cliente.
    - Cada columna es un dato de esa llamada.

Tablas de dimensiones:
    DIM_TIPO_DE_INCIDENCIA
    DIM_NIVEL_DE_SATISFACCION
    DIM_OPERADOR
    DIM_PRODUCTO_CONTRATADO


                                                  DIM_OPERADOR
                                                        ^
                DIM_PRODUCTO_CONTRATADO   <     HECHOS_ENCUESTAS  >  DIM_TIPO_DE_INCIDENCIA (esta es la tabla que creamos ayer)
                                                        v
                                            DIM_NIVEL_DE_SATISFACCION

Importante NUNCA DEJAMOS UN HUECO apuntando a una tabla de dimensiones. Un dato vacío.
Los datos vacíos los codificamos con un valor que se registrará en la correspondiente tabla de dimensiones. 
Por ejemplo, en la tabla de tipos de incidencia, el código 99 es "No informado".
Y ese es el valor que pondremos en la tabla de hechos cuando no tengamos información sobre el tipo de incidencia.

Si nos fijamos en la pinta que tiene esas tablas al pintarlas gráficamente, parecen una estrella. Y a eso le llamamos MODELO EN ESTRELLA.

En los modelos en estrella, lo que tenemos es una gran tabla de hechos, rodeada de varias tablas de dimensiones.

Hay otro modelo que en realidad es una generalización del modelo en estrella, que es el MODELO EN COPOS DE NIEVE (snowflake).
En un modelo en copo de nieve las dimensiones pueden tener asociadas subdimensiones. Y esas subdimensiones pueden tener asociadas subsubdimensiones. Y así hasta donde queramos.

    EJEMPLO:

        Tabla ventas:   HECHOS_VENTAS
                            v
        Tabla tiendas:  DIM_TIENDAS
                            v
        Tabla Zonas:    DIM_ZONAS / DIM_REGIONES
                            v
        Tabla Países:   DIM_PAISES

Esto es un modelo en copo de nieve. Y es un modelo más complejo que el modelo en estrella.
Me permite hacer análisis más complejos, pero es más difícil de entender y de usar.

## Granularidad de los datos.

Ya hemos hablado antes un poco de este tema.

PERO ES POSIBLEMENTE LA DECISION MAS IMPORTANTE QUE TOMAMOS AL DISEÑAR UN DATAWAREHOUSE.
ES CRITICO!

Qué nivel de detalle (agrupamiento) le damos a los datos que guardamos en la tabla de hechos.
¿Qué representa cada fila de la tabla de hechos? 
- Cada fila representa una llamada a un cliente? (granularidad fina)                <----- MAXIMO DETALLE
- Cada fila representa las llamadas por un operador en un día? 
- Cada fila representa las llamadas hechas en un día? (granularidad media)
- Cada final representa las llamadas hechas al mes? (granularidad gruesa)           <----- MINIMO DETALLE

Como dije antes:
- A priori, cuanto más detalle, mejor.
- Pero, si no voy a necesitar ese detalle, y al final siempre acabo agrupando los datos, mejor guardarlos ya agrupados de antemano.
- Me ahorro espacio y tiempo de cálculo.

Importante también:
Cada tabla de hechos su propia granularidad. No tiene por qué ser la misma granularidad para todas las tablas de hechos.
Es más, puedo tener los mismos hechos con distintas granularidades en distintas tablas de hechos. Y eso es muy habitual.

    Tengo una tabla de hecho con las llamadas diarias
    Tengo una tabla de hecho con las llamadas mensuales

Cuando monte un cuadro de mando, algunas gráficas/tablas se alimentarán de la tabla de hechos diaria y otras de la tabla de hechos mensual.

---

# DIMENSION FECHA!

En cualquier datawarehouse vamos a encontrar una dimensión que se llama DIM_FECHA. Y es una dimensión muy especial.

Habitualmente tenemos una tabla llamada DIM_FECHA que contiene una fila por cada día del año, durante muchos años. 
Y esa tabla tiene muchas columnas que describen a cada día.

    DIM_FECHA
    FECHA       DIA     MES     AÑO     TRIMESTRE   SEMANA   DIA_DE_LA_SEMANA   ES_FESTIVO   ...
    01-01-2020  1       1       2020    1           1        Miercoles          SI
    02-01-2020  2       1       2020    1           2        Jueves             NO
    03-01-2020  3       1       2020    1           3        Viernes            NO
    ...



Por supuesto, dada una fecha, yo puedo calcular todas esas columnas. 
Pero es mucho más rápido tenerlas ya calculadas y guardadas en una tabla de dimensiones.
Además, esta tabla, por mucho que parezca, es una tabla muy pequeña en comparación con las tablas de hechos.
Aunque guarde 10 años de datos, tendrá 3.650 filas (10 años * 365 días). Y eso es muy poco comparado con las tablas de hechos, que pueden tener millones de filas.
Lo que guardamos son números, que ocupan muy poco tamaño.

Estamos haciendo BI, y todo lo que tiene que ver con el negocio se ve afectado por el tiempo. Y por eso es tan importante tener una dimensión de fecha.
Todo hecho va a tener asociada una fecha. Y esa fecha va a tener asociadas todas las columnas de la dimensión de fecha.

# Cuidado al agrupar datos.

En ocasiones hemos dicho que está perfecto.
No todos los datos son agrupables!
NO PUEDO SUMAR TODAS LAS METRICAS. EN ALGUNAS NO TIENE SENTIDO!

Hay métricas agrupables (ADITIVAS)
    Las puedo sumar por cualquier cosa. Importes, costes, tiempo de realización, número de llamadas, número de clientes, número de incidencias, número de ventas, etc.

    Si sumo las ventas de las 12 tiendas de una región, obtengo las ventas de la región. Perfecto.

Métricas semiaditivas
    Puedo sumarlas por algunas dimensiones pero por otras no.
    Ejemplo más típico: STOCK de un producto en una tienda.
    Puedo sumar el stock de un producto en las 12 tiendas de una región, y obtengo el stock de la región. Perfecto.
    Pero no puedo sumar el stock de un producto en una tienda a lo largo de los días del mes. No tiene sentido. 
    El stock de un producto en una tienda es el que hay hoy, no el que había ayer o el que habrá mañana. 
    No puedo sumar el stock de un producto en una tienda a lo largo de los días del mes. No tiene sentido.
    Esta contabilizado de antemano.

Métrica no aditivas
    NO SE SUMAN POR NADA
    Por ejemplo: TODO LO QUE SEAN RATIOS / PORCENTAJES
    Margen%, ticket medio, tasa de conversión, satisfacción media.
    En general, estos datos preferimos calcularlos a partir de los datos que los generan, y no guardarlos en la tabla de hechos.

    Ejemplo:

        Tienda  Ventas  Margen      Margen%
        A       1000     500            50%
        B       2000    1000            50%
        C       3000    1000            33,33%
        D       4000    1500            37,5%
             --------   -----
               10000    4000

               No puedo hacer: 50%+50%+33,33%+37,5% = 170,83% / 4 = 42,7% de margen sobre ventas. Eso es incorrecto.
               La realidad sería: 4000 / 10000 = 40% de margen sobre ventas. Y eso es lo que tengo que calcular.

# El cubo de datos y las operaciones OLAP

Esto tiene que ver con la forma en que guardamos los datos en el datawarehouse y cómo los analizamos.

Habitualmente hablamos de un CUBO DE DATOS. Y es que los datos se pueden ver como un cubo de n dimensiones.
Ejemplo sencillo:

    VENTAS x REGION x TIPO DE PRODUCTO

                    PRODUCTO A          PRODUCTO B
        REGION A      1000                500  
        REGION B      2000                1000
        REGION C      3000                1500

    Ahí tengo 2 dimensiones (REGION y PRODUCTO) y una métrica (VENTAS). Esto NO ES UN CUBO DE DATOS.
    Un cubo tiene al menos 3 dimensiones. Y aquñi hay 2.
    La cosa es que en el mundo BI ya hemos dicho que todo lo que sea de negocio se ve afectado por el tiempo. Y por eso, la dimensión de fecha es tan importante.

    Lo normal sería tener esa tabla para el día 1.
    Y otra tabla como esa para el día 2.
    Y otra tabla como esa para el día 3.
    Y puedo pensar en esas tablas como fotos, que juntas hacen una pelicula.
    Si apilo todas esas tablas, obtengo un cubo de datos con 3 dimensiones (REGION, PRODUCTO y FECHA) y una métrica (VENTAS).
    Cada celda de ese cubo es el valor de ventas de un producto en una región en un día concreto.

    Esto pare representyarlo/explicarlo está bien... pero en realidad en el mundo BI, lo que trabajamos cson con subos n-dimensionales.
    
    Es un concepto más teórico.
    Lo que pasa es que entendiendo los datos como un cubo de datos, podemos hacer operaciones sobre él para analizar los datos de distintas formas.

# Las grandes 5 operaciones OLAP

Son operaciones en el análisis de los datos.

1. Drill Down: Ir a un nivel de detalle más fino. Ir a un nivel de granularidad más fino.
   Bajo de una dimensión a otra subdimensión. Por ejemplo, de año a trimestre, de trimestre a mes, de mes a día.
                                              Voy de región a tienda 
                                              BAJAR EN EL DETALLE DE LOS DATOS
2. Drill Up: Ir a un nivel de detalle más grueso. Ir a un nivel de granularidad más grueso.
   Subo de una subdimensión a otra dimensión. Por ejemplo, de día a mes, de mes a trimestre, de trimestre a año.
                                              Voy de tienda a región
                                              SUBIR EN EL DETALLE DE LOS DATOS
                                              AGRUPO DATOS/AGREGAR DATOS
3. Slice: Fijo el valor de una dimensión y me quedo con un subconjunto del cubo. (con una foto, con una tabla)
   Por ejemplo, me quedo con los datos de un año concreto, o de una región concreta, o de un producto concreto.
4. Dice (dado): filtro por varias dimensiones a la vez. Me quedo con un subconjunto del cubo. (pero con forma de dado, no de foto)
5. Pivot (girar): Datos que antes tenía en filas, los pongo en columnas. Datos que antes tenía en columnas, los pongo en filas.
   Cambio la perspectiva de los datos. Cambio la forma de ver los datos.
   (Es algo que por ejemplo en excel se hace con la tabla dinámica, girando los campos de filas a columnas y viceversa)

# ROLAP , MOLAP, HOLAP

Estas tres siglas tienen que ver con cómo un producto de BI almacena los datos.

MOLAP = Multidimensional OLAP
    Se guardan todas las agregaciones posibles de los datos ya calculadas de antemano.
    Permite generar informes muy rápido, pero ocupan mucho espacio en disco y tardan mucho en cargarse los datos al principio.

ROLAP = Relational OLAP
    Se guardan los datos en tablas relacionales. No se guardan las agregaciones de antemano.
    Permite generar informes más lentos, pero ocupan menos espacio en disco y tardan menos en cargarse los datos al principio.

HOLAP = Hybrid OLAP
    Se guardan los datos en tablas relacionales y algunas agregaciones de antemano.
    Permite generar informes más rápidos que ROLAP, pero ocupan menos espacio en disco que MOLAP y tardan menos en cargarse los datos al principio.

Esto tiene que ver con cómo guardan los sistemas los datos internamente... pero la realidad es que esto está ya OBSOLETO!

Hoy en día los derroteros han ido por otro lado.
Los sistemas modernos lo que usan son almacenamientos columnar, que permiten hacer agregaciones de datos muy rápido, sin necesidad de guardarlas de antemano.
Y los datos ocupan poco espacio en disco, porque se comprimen muy bien.

En ocasiones lo que hacemos es generar ficheros de datos columnares por fechas... Y tenemos de alguna forma, ese mismo concepto del CUBO MOLAP.
Hay formatos como las TABLAS DELTA, que permiten almacenar datos columnares y e ir haciendo diseccionado por fechas.

Ayer hablaba del formato parquet, que es el rey hoy en día para el almacenamiento columnar de datos. 
Y una generalización del formato parquet son lo que llamamos las TABLAS DELTA, que permiten almacenar datos columnares y e ir haciendo diseccionado por fechas.

Por aquí es por donde van las cosas. Lo de MOLAP, ROLAP y HOLAP es un concepto antiguo que ya no se usa. Hoy en día los sistemas modernos de BI usan almacenamiento columnar y formatos como parquet o delta.

O me voy a un sistema BI de hace 10 años que no ha tocado nadie, o eso de MOLAP no lo voy a ver.

Lo que sí sigue vigente es el concepto de CUBO DE DATOS y las operaciones OLAP que se hacen sobre él.
Pero más como marco teórico que como algo que se implemente en la práctica.


---

# Minería de datos (datamining) y Machine Learning

Antiguamente, a todo le llamábamos minería de datos. Hoy en día, parte de lo que antiguamente se llamaba minería de datos, lo llamamos Machine Learning. 
Básicamente son técnicas para analizar datos y sacar conclusiones de ellos.

Hay muchas.. ahora hablaremos de algunas.
El hecho es que antiguamente todas se catalogaban como minería de datos, y hoy en día parte de ellas se llaman Machine Learning.
Digo esto, porque el temario está viejuno aquí también.
Y hay muchas cosas que vienen bajo el epígrafe de minería de datos, y no se habla nada de Machine Learning.
Pero el nombre Machine Learning es más moderno y más cool, y es lo que se usa hoy en día.

Vamos a divir las técnicas en 2 grupos:

            Minería de datos (datamining)               Machine Learning
        DESCUBRIR COSAS QUE DESCONOZCO              PREDECIR COSAS EN BASE A DATOS PASADOS
        -------------------------------             ---------------------------------------
        Reglas de asociación                        Predicción
        Técnicas de agrupamiento (clustering)       Clasificación
        Análisis de secuencias 
        (incluidas las series temporales)        
        Detección de anomalías

        Aquí no se qué busco                       Aquí sé lo que busco
        NO SUPERVISADO                                      SUPERVISADO

    La idea es: primero descubro cosas que no sabía que existían, 
                y luego, en base a lo que he descubierto, hago predicciones de cosas.

    Predicciones: Quiero estimar un valor numérico cuantitativo
        - Regresión lineal              Predicción de ventas
        - Regresión logística           Predicción de si me interesa o no un cliente
        - Árboles de decisión          
        - Redes neuronales
    Clasificación: Quiero estimar un valor cualitativo (categoría, grupo, clase)
        - KNN (K-Nearest Neighbors)
        - SVM (Support Vector Machines)
        - Árboles de decisión           Predecir a qué grupo pertenece un cliente (VIP, normal, moroso, etc.)
        - Redes neuronales 
    Reglas de asociación:
        - Análisis factorial
    Agrupamiento (clustering):
        - K-means
        - DBSCAN
        - Clústering Jerárquico 
Todo esto son técnicas mucho más avanzadas. Habitualmente estas técnicas son desarrolladas por expertos en el área (CIENTIFICOS DE DATOS).
y no por los usuarios de negocio. 
CIENTIFICOS DE DATOS = Licenciados en matemáticas, estadística.
Hay que tener bastantes conocimientos de estadística y matemáticas para poder desarrollar estas técnicas.
Los usuarios de negocio normalmente usan herramientas que se han creado por científicos de datos,
y que les ofrecen valores que ellos usan.

> Ejemplo que no es BI, pero si data mining y e ilustra el concepto.
> Tragsa

Querían definir un INDICADOR: NIVEL DE EMPORAMIENTO DE UNA MUJER.
No es solo que no haya un aparato de medida que me diga el nivel de empoderamiento de una mujer. 
Es que ni siquiera soy capaz de definir ese concepto.
Lo que si se sabe es que ese nivel de empoderamiento tiene que ver con algunos factores:
- Autoestima
- Capacidad de gestión su tiempo
- Nivel de ingresos / Capacidad para generar ingresos
- ...

Y lo que se trata es de medir esos factores.
Para ello se hizo un test. 50 preguntas.
Ese test lo desarrolla profesionales del área de integración social.
Pero esas personas no son expertas en conceptos de psicología, ni mucho menos de estadística.

Hacen un test pensando en preguntas que tienen que ver con esos factores.
Definen 5 factores.. y plantean 10 preguntas para cada factor.

Hacen el test y me llaman a mi!
Para qué, para validar el test!
Yo no tengo NPI de integración social, ni de psicología, pero sí de estadística y de psicometría.
Y lo que hice fue aplicar técnicas de minería de datos al test: Análisis factorial y test Cronbach.
Realmente cuando aplicamos esas técnicas NO TENEMOS NI IDEA DE LO QUE VA A SALIR. No se qué busco.. solo busco.
Pero salieron cosas.
Resulta que esos análisis identificaron no 5 sino 6 factores.
Y algunas preguntas de las que ellas habían planteado para un factor, en realidad estaban midiendo otro factor.
La estadística NO DABA NOMBRES A LOS FACTORES... pero si decía:
Ojo estas 7 preguntas miden lo mismo, pero miden algo distinto a estas otras 5 preguntas.

Esos datos se pasan a una psicóloga!
- Por las mañanas, lo primero que hago es planificar mi día. -> Factor 1: Gestión del tiempo
  PERO NO ENCAJABA con el resto de preguntas que habían planteado para ese factor. 
Cuando lo cogío la psicóloga... a la vista del resto de preguntas con las que esa se alineaba (eso salío del estudio)
lo vió claro. ESA PREGUNTA NO MIDE GESTIÓN DEL TIEMPO, SINO INDEFENSION APRENDIDA... otro factor.

Elefantito atado a la cuerda.
Las mujeres no planificaban su día por falta de capacidad de gestión de su tiempo, 
sino por que habían fracaso tanto en su vida cuando lo había hecho en el pasado, que ya no lo intentaban.

Una vez que había una forma definida de medir el nivel de empoderamiento de una mujer, se podía usar para otra cosa:
- Vamos a calcular el incremento del nivel de empoderamiento de una mujer después de su participación en un programa de integración social.
- Vamos a ordenar a las mujeres por ese incremento. A ver a cuáles les ha funcionado mejor.
Y montamos un proyecto de MACHINE LEARNING.
El objetivo: Buscar un programa (árboles de decisión - random forest) que me permita predecir
a qué mujeres (por sus atributos/características (VARIABLES)) les va a funcionar mejor el programa de integración social
basado en los datos de las mujeres que ya han participado en el programa y su incremento de nivel de empoderamiento.
---

NOTA:

El temario del curso.
El temario del curso lo propone FUNDAE. Es un temario OFICIAL.
Y tiene unos que otros años!
Y está un poco viejete!
Y yo os estoy contando lo del temario .. pero más actualizado y más moderno.

En el temario no se menciona el concepto de DATALAKE, que yo ya lo he contado varias veces.
Pero es que antiguamente:
    BBDD -> DATAWAREHOUSE
Eso a día de hoy ya no se hace!
    BBDD -> DATALAKE -> DATAWAREHOUSE
            ^^^^^^^^
            Es un concepto más moderno: Ante no lo usábamos:
            - El almacenamiento era aún más caro
            - No había las mismas tecnologías que hoy en día
            - ...
---

# Gestión de proyectos BI

Un proyecto de BI tiene sus peculiaridades. No es lo mismo que otro tipo de proyectos.

En otros proyectos, el criterio de éxito es CLARO.
Estoy haciendo una aplicación -> CRITERIO DE EXITO = La aplicación funciona y hace lo que tiene que hacer.
Estoy haciendo un edificio    -> CRITERIO DE EXITO = El edificio está construido y es habitable.

En un proyecto de BI, el criterio de éxito es DIFUSO.
Básicamente que alguien entienda mejor su negocio y pueda tomar mejores decisiones. Pero eso es muy subjetivo.
 - Esto es complejo de especificar antes de comenzar
 - Es muy complejo de comprobar al final del proyecto si se ha conseguido o no.

Esto implica:
1. EL ALCANCE MUCHAS VECES SE DESCUBRE MIENTRAS SE HACE EL PROYECTO. 
- En muchos casos, nadie sabe que hay en una fuente de datos hasta que se empieza a analizar.
- No se sabe la calidad real de los datos hasta que se empieza a analizar.

2. EL ESFUERZO ESTA EN LOS DATOS , NO EN LA ENTREGA
- HAcer un cuadro de mando (que es lo primero que me viene a la cabeza cuando pienso en BI) es relativamente fácil y rápido, y bastante barato.
- Preparar los datos, consolidarlos, normalizarlos, mejorarlos... 
  Eso es lo que lleva mucho tiempo y esfuerzo. Y es lo que cuesta dinero.

3. EL VALOR REAL ESTA EN EL USO DEL SISTEMA.
- Un cuadro de mando que nadie usa, no tiene valor. ES UN FRACASO DE PROYECTO, aunque haya cumplido plazos y costes/presupuesto.

Un riesgo grande es que los requisitos en este tipo de proyectos son AMBIGUOS:
- El usuario no sabe lo que quiere hasta que lo ve.
  "quiero un cuadro de mando que me diga cómo va mi negocio"
  Y yo digo.. pues vale... 
  Y le monto algo.. 3 semanas.. se lo enseño.
  "Ah.. pues esto no me sirve
  Esta gráfica si me gusta... pero esta otra no me aporta nada...."

Y no es que se acaprichoso... es que hasta que no lo ve, no sabe si le vale/aporta o no.
Quien lo pide no es experto en analítica de datos, ni en BI, ni en estadística, ni en informática.
Es un usuario de negocio que quiere tomar mejores decisiones.

Mi trabajo es ayudarle a definir un buen cuadro de mando/reportes que le ayude a tomar mejores decisiones.
Pero él / ella no sabe qué debe/puede incluir en ese cuadro de mando/reportes hasta que no lo ve.
Ni siquiera conoce todos los tipos de gráficos, datos, indicadores que le puedo suministrar.

Por esto, en estros proyectos se hace crítico:
1. ENTREGA ITERATIVA.
   No puedo plantearme pasarme u año de proyecto y que el cliente vea el cuadro de mando al final.
   ESO ES RUINA SEGURO!

   Le voy dando de poco a poco. Que tengo un gráfico nuevo.. Se lo pongo ya... que lo vea.. que me de feedback.
   Podré usar ese feedback para tomar decisiones YO sobre lo próximo que voy a montar.
2. CONTROL DE VERSIONES DEL PROYECTO
    Cada cambio, cada entrega debe convertirse en un acto formal.
    - Cada petición se valora, se escribe, se presupuesta, se aprueba, se implementa, se entrega y se valida.

# Los roles. Quién hace falta.

El otro día os hable de perfiles técnicos que nos hacen falta:
- Analista de datos
- Programador ETL
- Administrador de BBDD

Pero hay otros .. y muy importantes (y no son técnicos):
1. PATROCINADOR (SPONSOR)
   Es alguien de dirección... con billetes o capacidad de mover billetes. 
   Hace cosas que nadie más puede hacer:
   - PRESUPUESTO Y EQUIPO
   - MANTIENE EL FOCO DEL PROYECTO (según pasa el tiempo, otras cosas van surgiendo y el proyecto se puede retrasar)
   Sin patrocinador convencido, el proyecto se muere. Y no hay nada que hacer. 

2. EXPERTO DE NEGOCIO (Subject Matter Expert)
   Es alguien que conoce el negocio y los datos. 
   - Conoce los datos que hay en las fuentes de datos
   - Conoce la calidad de esos datos
   - Conoce cómo se calculan los indicadores de negocio
   - Conoce cómo se toman decisiones en el negocio
   - Conoce qué decisiones se pueden tomar con los datos que hay, y cuáles no.
   Sin un experto de negocio, el proyecto se muere. Y no hay nada que hacer.

# Las fases del proyecto

1. VIABILIDAD DEL PROYECTO
   - Qué datos hay y dónde están
   - Qué quiero conseguir
   - Empiezo a definir un poco los cuadros de mando

2. ANALISIS
  - Inventario de datos
  - Calidad real de los datos
  - Defino indicadores de negocio

3. DISEÑO
   Diseño del datawarehouse y el modelado de los datos 
   Restricciones de seguridad y acceso a los datos

4. IMPLEMENTACION
   - Desarrollo de los procesos ETL
   - Desarrollo de los cuadros de mando y reportes 

5. PRUEBAS Y VALIDACION
   - Es enseñar los datos/cuadros a los usuarios de negocio y que los validen / entiendan
   - Que reconozcan los datos/información que sale del sistema

6. CIERRE
  - Formación a usuarios

ESFUERZO:

    Análisis:                          20%
    Perfilado y calidad de datos:      25%
    Desarrollo de ETLs:                30%
    Modelo y cuadros de mando:         15%
    Validación, formación ajustes:     10%


    AQUI HAY ALGO INTERESANTE:
        55% Perfilado, calidad y ETLS.
        Solo un 15% es el desarrollo de los cuadros de mando. <- QUE ES LO QUE LA GENTE PIENSA QUE TENEMOS QUE HACER EN EL PROYECTO.

    Un plan/presupuesto de proyecto de BI que contemple un 50% del tiempo en cuadros de MANDO = RUINA!
    Está mal hecho ese plan de negocio.

# Problemas REALES a los que nos enfrentamos en estos proyectos:

- Que me den permiso para acceder a los datos de una BBDD: 3 semanas
- Que el que conoce el sistema ese antiguo está de vacaciones: 2 semanas
- Que hay una decisión de lo que se conidera un "CLIENTE ACTIVO" / "PROYECTO EXITOSO" / "CLIENTE SATISFECHO" y que nadie toma!

Estas nos pasan MUCHISIMO EN ESTOS PROYECTOS y son las que retrasan todo un huevo.
Hay que cazarlas el día 1 y ponerlas sobre la mesa.


---

# Información vs datos

Los datos no son los mismo que la información.

Un dato es un hecho aislado, una cifra, un valor.

Los datos transportan información... y a veces (normalmente) mucha más de la que parece a priori.

- Peso de una persona: 140kgs
  Eso es un dato o información? AMBOS

    Es un dato, pero ese dato me informa de muchas cosas:
    - Peso de la persona
    - Talla de ropa de la persona? Esa persona tiene una talla S? NO 
                                                               M? NO
                                                               L? NO
                                                               XL? NO
    - Me da información acerca de su estado de salud
    - Incluso posiblemente de su presión arterial
    - De sus hábititos alimenticios
    - De sus hábitos de vida

    No me da información totalmente precisa.
    Pero si juntase ese dato con más datos, podría llegar a informaciones más precisas.
 
    Incertidumbre:
        Esa persona podría tener cierto sobrepeso.
        O, si es una persona de 220cm, quizas no tenga sobrepeso.
        O podría ser culturista.
    Si se que la persona delgada no está! Que no usa ciertas tallas de ropa! ...!

    Si completara ese dato con otros: altura, contorno de cintura, edad... podría afinar mucho la información que me da ese dato.
    Cuantos más datos junte, mejor calidad tendrá la información que soy capaz de extraer de ellos.
    Pero eso no quita el hecho de que cada dato per se me está aportando mucha información.

Partimos de datos.. montones de datos.
Y lo que queremos es procesar y analizar esos datos para sacarles el jugo: OBTENER/EXTRAER INFORMACIÓN de ellos.
Y usar luego esa información para tomar decisiones mejores.