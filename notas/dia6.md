
- Una vez entendidos los datos (su naturaleza, calidad)
- Una vez entendidos la información resultante (indicadores, gráficos, tablas) que voy a poder sacar y que va a interesar a según que tipo de usuario (operacional, táctico, estratégico).

- Pasamos a plantear el modelo de datos del datawarehouse -> ENTREGABLE
- Planteamos el origen de los datos y cómo debemos tratarlos para que lleguen al datawarehouse en el formato adecuado.
--> Especificación de las ETLs -> ENTREGABLE

- Yo mañana os daré un fichero EXCEL que tendrá exactamente la estructura del Datawarehouse.
- Cargado de datos limpios y transformados en el Datawarehouse.

Lo llevaremos a PowerBI de Microsoft y haremos varios informes de ejemplo con gráficos, tablas y KPIs.

Descargamos powerBI del store de Microsoft: PowerBI Desktop.
Jueves os pasaré un fichero similar para que trabajeís vosotras.

---

# EL CSV que tenemos no es el origen de los datos!

Cuando empiezo un proyecto habitualmente esto es de lo que voy a partir.
El equipo que quiere sus informes, tiene hojas excel o similares con datos más o menos agregados de alguna forma.
Que es lo que actualmente usan ellos para sus informes.

Esos archivos me ayudan a entender qué datos tienen, qué información quieren sacar y cómo la quieren ver.

Ahora bien, esos archivos no son el origen de los datos. Son un reflejo de los datos que ya han sido tratados y transformados de alguna forma.

La clave de un proyecto BI es definir unas buenas ETLs, es decir, un proceso (o varios) que sean capaces:
- De extraer datos de unos sistemas que se usen en la empresa (BBDD, ERPs, ficheros planos, etc)
- Extraigan esa información sin afectar a los sistemas de origen (sin bloquearlos, sin ralentizarlos, sin afectar a los usuarios)
- Eso además no se hará una única vez, sino que se hará de forma periódica (diaria, semanal, mensual, etc)
  Será necesario definir esos programas de forma que puedan hacer cargas INCREMENTALES. Es decir, que si el mes pasado ya cargué 100.000 registros, este mes solo cargue los 10.000 nuevos que se han generado.
- Generar sus propios identificadores de cada registro, para poder relacionarlos entre sí y con los datos de los sistemas originales.
  - Puede ser que parte de los datos lleguen de una BBDD y parte de los datos lleguen de otra.
  - Y puede ser que los datos que llegan de una tengan un identificador y los datos que llegan de otra tengan otro... incluso que se solapen. Que la encuesta 10 exista en ambas BBDD con el mismo identificador, pero que sean encuestas distintas. Por eso es importante generar un identificador propio para cada registro.

> Una ETL NO ES UN PROCESO DE LIMPIEZA/INTEGRACION DE DATOS QUE SE EJECUTA UNA VEZ.

Muchas veces, pensamos que es así! Y no lo es.
Yo parto de una situación HOY.. con unos datos. Pero el día de mañana habrá más datos... y quizás variaciones de ellos que yo no tengo contempladas.

Una ETL es un proceso INDUSTRIAL que debe producir el mismo resultado cada vez que se ejecute. Debe ser un proceso DETERMINISTA.

Además, las ETLS se definen contra ESPECIFICACIONES, NUNCA NUNCA defino una ETL en base a los datos que tengo ahora mismo.

> NO PROGRAMO CONTRA DATOS, PROGRAMO CONTRA ESPECIFICACIONES.

Esas especificaciones podrán cambiar con el tiempo.. y por ello iré teniendo distintas versiones de mis procesos ETL. Pero cada versión de la ETL debe cumplir con las especificaciones que se definieron para ella.

---

# Volvemos al fichero que teníamos:

id_encuesta
dni
fecha_llamada
hora_llamada
comunidad
edad
sexo
producto_contratado
importe_ultima_factura
satisfaccion_1_10
recomendaria
num_incidencias_previas
tipo_primera_incidencia
duracion_llamada_seg
operador

> Pensáis que algún sistema de producción de una empresa va a guardar un fichero con esta estructura? 
> Con esas 15 columnas? NO

Es más, posiblemente datos en varias BBDD diferentes.
Y ojo estamos hablando tanto a nivel de filas como a nivel de columnas!

Si miramos los datos detenidamente, hay datos de muchas naturalezas distintas ahí dentro.
Y posiblemente esos datos se guardan en sistemas informáticos diferentes:

    - GESTION DE CLIENTES (CRM): CRM = Customer Relationship Management
        dni
        comunidad       *** Podría cambiar/caducar
                            Este dato, también es relativo a la fecha de llamada/encuesta. Una persona puede cambiar de comunidad autónoma.
        edad            *** Este dato es raro. CADUCA! Si pone que tiene 33 años.. Ese dato vale durante este año.
                            Fecha de nacimiento/Año de nacimiento es un dato que no caduca. Edad sí.
                            Yo puedo teniendo la fecha de nacimiento calcular:
                            - La edad de la persona en el momento de la llamada
                            - La edad de la persona actualmente
        sexo
    - PRODUCTOS CONTRATADOS: ERP = Enterprise Resource Planning
        dni 
        producto_contratado       *** Este es otro dato que caduca
                                      Es relativo a la fecha de llamada/encuesta
    - FACTURACION
        dni 
        importe_ultima_factura    *** Este dato caduca!
                                      Podría guardarlo, pero asociado a la fecha de la llamada.
    - GESTION DE INCIDENCIAS (CRM u otro sistema)
        dni 
        num_incidencias_previas   *** Este es otro dato que caduca
                                      Es relativo a la fecha de llamada/encuesta
        tipo_primera_incidencia
    - LLAMADAS DE UN CALL CENTER (CRM u otro sistema)
        id_encuesta
        dni
        fecha_llamada
        hora_llamada
        duracion_llamada_seg
        operador
        satisfaccion_1_10
        recomendaria

Una cosa son los datos con los que QUIEN SEA (un role de nuestra empresa) hace sus informes, y otra cosa es dónde estan los datos en la empresa y cómo se guardan.

Además de este supuesto que hacemos, vamos a hacer un segundo supuesto.
Los datos de las encuestas de satisfacción se han hecho por 2 vías diferentes:
- Por teléfono y los operadores han ido anotando las respuestas en un sistema informático.
- Hablando/entrevistas, donde los entrevistadores (operadores) han ido anotando las respuestas a mano (en un excel)

Si hay 50000 datos, 35.000 vienen de un sistema informático y 15.000 vienen de entrevistas a mano.
Seguramente los datos van a venir de forma distinta.

El hecho es que ahora mismo tengo 50.000 encuestas... pero el año que viene habrá más... el mes que viene habrá más. Esto lo tengo que tener en cuenta.

Desarrollar un programa (ETL) cuesta bastante dinero y tiempo. Y no se justifica su inversión si no es algo que vamos a reutilizar. Si solo quiero hacer una carga de datos a dia de hoy y nunca más, la hago a mano, tardo menos que en hacer un programa.
Pero esto no es lo que queremos en el mundo BI. Los datos cambian con el tiempo. Y lo que quiero es ir viendo esa evolución y tomando mejores decisiones dependiendo de cómo van los datos.

---

Lo primero es plantear COMO ME INTERESAN LOS DATOS EN EL DATAWAREHOUSE: MODELO QUE VOY A TENER EN EL DWH.
Hablábamos de modelos en estrella y modelos en copo de nieve. 

En este caso, nos interesará MUCHO un modelo en estrella, pero vamos a tratar de explicar porqué NO ME INTERESA UN MODELO EN COPO DE NIEVE.

Nosotros tendremos tablas de hechos y tablas de dimensiones.
En los modelos en copo de nieve, las tablas de dimensiones pueden estar agrupadas por más tablas de dimensiones. Es decir, una tabla de dimensión puede tener otra tabla de dimensión asociada a ella.
Además va a haber decisiones importantes.

Para nosotros, cuál va a ser la tabla de hechos?
Un hecho es algo que ocurre en un momento dado del tiempo. ESTO NOS DA UNA PISTA.
¿Qué hay en nuestro caso que ocurre en un momento dado del tiempo? La Llamada/Encuesta


T_FACT_ENCUESTAS
        id_encuesta                 1,   327
        id_cliente                  Corresponde al número (id_cliente) de la tabla de dimensión T_DIM_CLIENTE
        comunidad_id                Corresponde al número (id_comunidad) de la tabla de dimensión T_DIM_COMUNIDAD                       
        id_genero                   Corresponde al número (id_genero) de la tabla de dimensión T_DIM_GENERO
        fecha_encuesta              FECHA: 2024-01-01, 2024-01-02, 2024-01-03, 2024-01-04, 2024-01-05, ...
                                    En esta tabla, varias encuestas pueden ocurrir en la misma fecha. Por eso no es un identificador único de la tabla de hechos.
        hora_encuesta               HORA: 00:00, 00:01, 00:02, 00:03, 00:04, 00:05, ...
        duracion_encuesta_seg       Número: 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, ...
        id_operador                 Corresponde al número (id_operador) de la tabla de dimensión T_DIM_OPERADOR
        satisfaccion_1_10           Número:   1, 2, 3, 4, 5, 6, 7, 8, 9, 10
        recomendaria                BOOLEANO: SI | NO
        num_incidencias_previas     Número: 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, ...
        id_tipo_primera_incidencia          Corresponde al número (id_tipo_incidencia) de la tabla de dimensión T_DIM_TIPO_INCIDENCIA
                A PRIORI NO DEBERIA DE IR AQUI... YA QUE TODAS LAS ENCUESTAS QUE HAGA AL MISMO CLIENTE , CON INDEPENDENCIA DE LA FECHA, VAN A TENER EL MISMO TIPO DE PRIMERA INCIDENCIA. PERO, POR SIMPLICIDAD, LO COLOCO AQUI.
        importe_ultima_factura      Número: 123.87€    89.76€ ...
        movil                       BOOLEANO: SI | NO
        fibra                       BOOLEANO: SI | NO
        tv                          BOOLEANO: SI | NO

        NO ->   id_producto_contratado      1
                SI LO PONGO AQUI SOLO PODRIA PONER 1 PRODUCTO ASOCIADO A LA ENCUESTA. 
                PERO UN CLIENTE PUEDE TENER VARIOS PRODUCTOS CONTRATADOS. NO VALE
                HAY 2 OPCIONES. Y VAMOS A IMPLEMENTAR LAS 2:
                - Tabla intermedia (Habitualmente las llamamos tablas PUENTE: BRIDGE) que asocie encuestas con productos.
                  En este caso tiene mucho sentido hacer esto.. ya que además, van a cambiar los productos con el tiempo? 
                  Y no me refiero a los que tiene un cliente contratado.. sino a los que ofrece la empresa? CON MUCHA FRECUENCIA!
                    MOVIL 10Gb
                    MOVIL 20Gb
                    MOVIL 50Gb
                    MOVIL ILLIMITADO
                    FIBRA 100Mb
                    FIBRA 300Mb
                    FIBRA 1Gb
                    TV BASICA
                    TV PREMIUM
                - OTRA OPCION. Aunque cambien los productos concretos que ofrece mi empresa, lo que posiblemente no cambie mucho son
                  los tipos de productos que ofrece mi empresa. Por ejemplo:
                    MOVIL
                    FIBRA
                    TV
                  Y me puede interesar MUCHO hacer análisis de satisfacción por tipo de producto. 
                        En este caso, podría crear una tabla de dimensión de tipos de productos y asociarla a la tabla de hechos T_FACT_ENCUESTAS. O incluso, mucho más interesante sería asociarla a la tabla de dimensión T_DIM_PRODUCTOS -> MODELO EN COPO DE NIEVE.
                        PERO... dado que hemos dicho que LOS TIPOS DE PRODUCTO QUE VOY A VENDER NO VAN A CAMBIAR SIGNIFICATIVAMENTE CON EL TIEMPO OPTEMOS POR OTRA SOLUCION MAS SIMPLE Y QUE OFRECERA UN MEJOR RENDIMIENTO AL HACER LAS CONSULTAS:
                            PIVOTE!
                            La operación PIVOTE de OLAP consiste en convertir un dato que está en FILAS a COLUMNAS.
                            Las filas que guardaría la tabla de T_BRIDGE_ENCUESTAS_TIPOS_PRODUCTOS las convierto en columnas.
                            3 columas nuevas en la tabla de hechos T_FACT_ENCUESTAS:
                                - MOVIL
                                - FIBRA
                                - TV
                            Este es el MISMO CASO que las VENTAS POR REGIONES de la tabla EXCEL de videojuegos que vimos días atrás.

T_BRIDGE_ENCUESTAS_PRODUCTOS
    id_encuesta                 Corresponde al número (id_encuesta) de la tabla de hechos T_FACT_ENCUESTAS
    id_producto                 Corresponde al número (id_producto) de la tabla de dimensión T_DIM_PRODUCTOS

T_DIM_PRODUCTOS
    id_producto                    NUMERO INTERNO a la bbdd: 1, 2, 3, 4 
    nombre_producto                TEXTO: "Producto A", "Producto B", "Producto C", "Producto D"

T_DIM_CLIENTE
    id_cliente                      NUMERO INTERNO a la bbdd: 1, 2, 3, 4 
    id_cliente_corporativo          TEXTO: DNI (lo ideal es que no sea el dni)
    dni                             TEXTO: 12345678A, 23456789B, 34567890C, 45678901D
    fecha_nacimiento_cliente        FECHA: 1980-01-01, 1981-01-01, 1982-01-01, 1983-01-01, 1984-01-01, ... 
    id_tipo_primera_incidencia          Corresponde al número (id_tipo_incidencia) de la tabla de dimensión T_DIM_TIPO_INCIDENCIA
        ESTE SERIA EL SITIO NATURAL, ya que el tipo de primera incidencia es un dato que no caduca/cambia. Se asocia al cliente... y permanece. 
        NO OBSTANTE, NO VOY A COLOCARLO AQUI!
        Y ESTO ES UNA DECISION PRACTICA. Si lo coloco aquí, paso a un modelo en copo de nieve.
            Entre otras coas, el clasificar a los clientes por el PRIMER TIPO DE INCIDENCIA que han tenido no es algo que nos vaya a interesar. A hacer eso, me podría ayudar el tener el campo aquí (Y EL MODELO EN COPO DE NIEVE).

T_DIM_TIPO_INCIDENCIA
    id_tipo_incidencia              NUMERO INTERNO a la bbdd: 1, 2, 3, 4 
    nombre_tipo_incidencia          TEXTO: "Incidencia de facturación", "Incidencia de producto", "Incidencia de servicio", "Incidencia de atención al cliente"

T_DIM_GENERO
    id_genero                       NUMERO INTERNO a la bbdd: 1, 2, 3, 4 
    nombre_genero                   TEXTO: "Hombre", "Mujer", "Otro"

T_DIM_COMUNIDAD
    id_comunidad                    NUMERO INTERNO a la bbdd: 1, 2, 3, 4 
    nombre_comunidad                TEXTO: "Andalucía", "Aragón", "Asturias", "Baleares", "Canarias", "Cantabria", "Castilla-La Mancha", "Castilla y León", "Cataluña", "Comunidad Valenciana", "Extremadura", "Galicia", "Madrid", "Murcia", "Navarra", "País Vasco", "La Rioja"

T_DIM_FECHA
    fecha                           FECHA: 2024-01-01, 2024-01-02, 2024-01-03, 2024-01-04, 2024-01-05, ...
                                    En esta tabla, una fecha se registra una única vez. No hay duplicados.
    dia_semana                      TEXTO: "Lunes", "Martes", "Miércoles", "Jueves", "Viernes", "Sábado", "Domingo"
    mes                             TEXTO: "Enero", "Febrero", "Marzo", "Abril", "Mayo", "Junio", "Julio", "Agosto", "Septiembre", "Octubre", "Noviembre", "Diciembre"
    anio                            NUMERO: 2024, 2025, 2026, ...
    bisiesto                        BOOLEANO: SI | NO
    fin_de_semana                   BOOLEANO: SI | NO
    festivo                         BOOLEANO: SI | NO

T_DIM_OPERADOR
    id_operador                     NUMERO: 1, 2, 3, 4
    nombre                          TEXTO: "Pepe", "Juan", "Maria"


                  T_DIM_TIPO_INCIDENCIA   T_DIM_COMUNIDAD 
                                ^          ^
       T_DIM_OPERADOR    <    T_FACT_ENCUESTAS   <  T_BRIDGE_ENCUESTAS_PRODUCTOS   >  T_DIM_PRODUCTOS
       T_DIM_FECHA       <       V         v
                        T_DIM_GENEROS   T_DIM_CLIENTE 

---

RESUMIENDO:

T_FACT_ENCUESTAS
        id_encuesta                 
        id_cliente                  
        comunidad_id                
        id_genero                   
        fecha_encuesta              
        hora_encuesta               
        duracion_encuesta_seg       
        id_operador                 
        satisfaccion_1_10           
        recomendaria                
        num_incidencias_previas     
        id_tipo_primera_incidencia  
        importe_ultima_factura      
        movil                       
        fibra                       
        tv                          

T_BRIDGE_ENCUESTAS_PRODUCTOS
    id_encuesta                 
    id_producto                 

T_DIM_PRODUCTOS
    id_producto                   
    nombre_producto               

T_DIM_CLIENTE
    id_cliente                    
    id_cliente_corporativo        
    dni                           
    fecha_nacimiento_cliente      
    id_tipo_primera_incidencia    

T_DIM_TIPO_INCIDENCIA
    id_tipo_incidencia              
    nombre_tipo_incidencia          

T_DIM_GENERO
    id_genero                       
    nombre_genero                   

T_DIM_COMUNIDAD
    id_comunidad                    
    nombre_comunidad                
    
T_DIM_FECHA
    fecha                           
    dia_semana                      
    mes                             
    anio                            
    bisiesto                        
    fin_de_semana                   
    festivo                         

T_DIM_OPERADOR
    id_operador                     
    nombre                          

7 tablas de dimensiones, 1 tabla de hechos y 1 tabla puente. 9 tablas en total.

PERO ESTO AUN NO ESTA ACABADO.
Faltan cosas... Cómo sé que faltan cosas... me estoy adelantando. LA EXPERIENCIA.

Pero no me voy a adelantar. 
Vamos a ver los procesos ETL que hemos de definir para que los datos lleguen a este modelo de datos del datawarehouse.
Y ahí es donde vamos a ver que faltan cosas.

---

# Diseño de las etls

Tenemos muchas tablas de dimensiones.
Y salen de BBDD diferentes de nuestros entornos de producción.
Vamos a ir analizando:

### Tabla de Productos: T_DIM_PRODUCTOS

    T_DIM_PRODUCTOS
        id_producto AUTOGENERADO
        nombre_producto

Esa tabla la cargaremos desde ERP de la empresa.

Problema: En el ERP, a día de hoy hay 6 productos:
    TABLA PRODUCTOS:
        ID       |      NOMBRE
        1        |      MOVIL 10Gb
        2        |      MOVIL 20Gb
        3        |      MOVIL ILLIMITADO
        4        |      FIBRA 100Mb
        5        |      FIBRA 300Mb
        6        |      TV

Puedo facilmente exportar esos datos a un fichero plano y cargarlos en la tabla de dimensión T_DIM_PRODUCTOS.
Funcionaría? SI... HOY.... y mañana? 
RARO SERIA que los productos cambien de ID... eso no va a pasar.
    MOVIL 10GB -> 1.     Tiene el número 1... y seguirá teniendo el número 1. No va a cambiar. A no ser que cambié el ERP y me cambien los ID de los productos. Pero eso es raro. Pero no me cuesta mucho tenerlo en cuenta.
    En esta tabla voy a tener 20 productos... Otra cosa es que tenga 10k productos... La cosa se complica


    T_DIM_PRODUCTOS
        id_producto
        id_bbdd_origen  (Esto lo asigna el programa ETL que carga los datos desde el ERP)
        id_producto_origen (SERA UN SECUENCIAL QUE ME ASIGNE EL ERP A CADA PRODUCTO)
        nombre_producto

    ERP SAP:
        1        MOVIL 10Gb
        2        MOVIL 20Gb
        3        MOVIL ILLIMITADO
        4        FIBRA 100Mb
        5        FIBRA 300Mb
        6        TV

    T_DIM_PRODUCTOS
        id_producto  |  id_bbdd_origen  |  id_producto_origen  |  nombre_producto
        1            |       1          |          1           |   MOVIL 10Gb
        2            |       1          |          2           |   MOVIL 20Gb
        3            |       1          |          3           |   MOVIL ILLIMITADO
        4            |       1          |          4           |   FIBRA 100Mb
        5            |       1          |          5           |   FIBRA 300Mb
        6            |       1          |          6           |   TV

                            1 -> SAP

    Si el día de mañana se añade un producto nuevo en SAP
        7        |      MOVIL 50Gb
    Lo llevo a la tabla de dimensión T_DIM_PRODUCTOS
        7            |       1          |          7           |   MOVIL 50Gb
    Si el día de mañana se elimina un producto en SAP, lo que hago es nada... paso de eso.... en el DWH seguirá el dato histórico.

        LA ETL Serían 2 queries a BBDD:
            EXTRACT: Dame todos los IDs y nombres de productos de SAP desde el ID que corresponda al MAYOR ID que tengo en 
                     la tabla de dimensión T_DIM_PRODUCTOS para el id_bbdd_origen = 1 (SAP)
            LOAD(CARGA):   Inserta en la tabla de dimensión T_DIM_PRODUCTOS los datos:
                        id_bbdd_origen = 1
                        id_producto_origen = ID que me da SAP
                        nombre_producto = Nombre que me da SAP
                        id_producto = AUTOINCREMENTAL (lo asigna la bbdd del DWH)
    Si el día de mañana cambio de ERP o instalan una versión nueva, que cambié las cosas dramáticamente:
        Nuevo ERP:
            1        |      MOVIL 20Gb
            2        |      MOVIL 50Gb
            3        |      MOVIL ILLIMITADO
            4        |      FIBRA 300Mb
            5        |      FIBRA 1Gb
            6        |      TV BASICA
            7        |      TV PREMIUM
        
        Modificaríamos la ETL para que ahora en lugar de sacar los datos de SAP, los saque del nuevo ERP. Y el id_bbdd_origen = 2 (Nuevo ERP)
    T_DIM_PRODUCTOS
        id_producto  |  id_bbdd_origen  |  id_producto_origen  |  nombre_producto
        1            |       1          |          1           |   MOVIL 10Gb
        2            |       1          |          2           |   MOVIL 20Gb
        3            |       1          |          3           |   MOVIL ILLIMITADO
        4            |       1          |          4           |   FIBRA 100Mb
        5            |       1          |          5           |   FIBRA 300Mb
        6            |       1          |          6           |   TV
        7            |       2          |          1           |   MOVIL 20Gb
        8            |       2          |          2           |   MOVIL 50Gb
        9            |       2          |          3           |   MOVIL ILLIMITADO
        10           |       2          |          4           |   FIBRA 300Mb
        11           |       2          |          5           |   FIBRA 1Gb
        12           |       2          |          6           |   TV BASICA
        13           |       2          |          7           |   TV PREMIUM

        EN UN CASO TAN AGRESIVO COMO ESTE ES UNA MIGRACION DE DATOS EN EL DWH.
        Buscar en la tabla de dimensión T_DIM_PRODUCTOS los productos que ya existían en el ERP antiguo (nombre) y que también existen en el nuevo ERP. Borrar las entradas del ERP antiguo y dejar las del nuevo ERP. Y reasignar los ids de la tabla de hechos T_FACT_ENCUESTAS y de la tabla puente T_BRIDGE_ENCUESTAS_PRODUCTOS a los nuevos ids de la tabla de dimensión T_DIM_PRODUCTOS.


        id_producto  |  id_bbdd_origen  |  id_producto_origen  |  nombre_producto
        1            |       1          |          1           |   MOVIL 10Gb
        4            |       1          |          4           |   FIBRA 100Mb
        6            |       1          |          6           |   TV
        7            |       2          |          1           |   MOVIL 20Gb
        8            |       2          |          2           |   MOVIL 50Gb
        9            |       2          |          3           |   MOVIL ILLIMITADO
        10           |       2          |          4           |   FIBRA 300Mb
        11           |       2          |          5           |   FIBRA 1Gb
        12           |       2          |          6           |   TV BASICA
        13           |       2          |          7           |   TV PREMIUM

        Reasigno: 2->7
                  3->9
                  5->10  
        Borro 2, 3 y 5

        Pero incluso con algo tan agresivo como eso (UN CAMBIO EN LOS IDs) para nosotros el impacto es MINIMO!
            Reasignar 4 números y borrar otros tantos.
            Que se puede hacer con una QUERY SIMPLE.

            Y en el programa ETL solo cambiar el id_bbdd_origen = 1 (SAP) por id_bbdd_origen = 2 (Nuevo ERP) y el resto de la ETL sigue igual.

            CAMBIO SIMPLE
            Y lo importante es que el MODELO NO CAMBIA. Por ende los cuadros de mando que genero POWERBI no hay que tocarlos.


LAS ETLS DEL RESTO DE TABLAS DE DIMENSIONES SERIAN IGUAL A ESTA:
    T_DIM_TIPO_INCIDENCIA
    T_DIM_GENERO
    T_DIM_COMUNIDAD
    T_DIM_OPERADOR
    En todas estas, añado: id_bbdd_origen y id_origen.

T_DIM_FECHA NO SE CARGA DINAMICAMENTE... se popula inicalmente con datos de 10 años vista... Y el mismo programa lo ejecuto dentro de 10 años.


Sería parecida pero tiene un par de cositas:
- Añadimos id_bbdd_origen y id_origen (~id_cliente_corporativo). Con este último entro en otras bbdd para traer información adicional:
- id_tipo_primera_incidencia. Este dato. sale de otra BBDD. Y la ETL debe preguntar a esa BBDD por el id_tipo_primera_incidencia de ese cliente (id_cliente_corporativo -podría ser el dni u otro-). Y si no lo encuentra, lo deja en blanco.

### Tabla de Hechos:


T_FACT_ENCUESTAS
        id_encuesta                 
        id_origen_encuesta             (Esto lo asigna el programa ETL que carga los datos desde el sistema de encuestas)
        id_encuesta_origen          
        id_cliente                  
        comunidad_id                
        id_genero                   
        fecha_encuesta              
        hora_encuesta               
        duracion_encuesta_seg       
        id_operador                 
        satisfaccion_1_10           
        recomendaria                
        num_incidencias_previas     
        id_tipo_primera_incidencia  
        importe_ultima_factura      
        movil                       
        fibra                       
        tv             

        NO VOY A TENER SOLO 1 ETL... VOY A TENER 2 ETLS:
            ETL que extrae datos del sistema informático de encuestas vía llamada
                id_origen_encuesta = 1
            ETL que extrae datos de las encuestas realizadas a mano (excel)
                id_origen_encuesta = 2
        
        Ambas ETLS necesitan extraer datos de otras BBDD para poder completar los datos de la tabla de hechos T_FACT_ENCUESTAS:
        Eso lo harán a través del id_cliente_corporativo (DNI) que es el que me permite relacionar los datos de las distintas BBDD.

        Aquí hay 2 grupos:    
            Datos que se insertan tal cual:
            - comunidad_id
            - id_genero
            
            - id_tipo_primera_incidencia
             
            - importe_ultima_factura
            Datos que pivotamos:
            Pregunto a la BBDD de productos contratados, los productos que tiene ese cliente:

                Necesito una tabla de AGRUPACION DE PRODUCTOS (Esta tabla irá cambiando con el tiempo)
                    MOVIL 10Gb          -> MOVIL
                    MOVIL 20Gb          -> MOVIL
                    MOVIL 50Gb          -> MOVIL
                    MOVIL ILLIMITADO    -> MOVIL
                    FIBRA 100Mb         -> FIBRA
                    FIBRA 300Mb         -> FIBRA
                    FIBRA 1Gb           -> FIBRA
                    TV BASICA           -> TV
                    TV PREMIUM          -> TV   
                    ????                -> OTRO

            - movil
            - fibra
            - tv
            - otro <<< ESTE SERIA MUY IMPORTANTE
            - errores
              Adicionalmente, sería conveniente, tener un campo con potenciales errores encontrados. Llamemos al campo errores
              Imaginad que hemos dado de alta el producto MOVIL 100G en el ERP, pero no lo hemos añadido a la tabla de agrupación de productos. 
              - Rellenaria OTRO = true
              - En error: PRODUCTO_NO_ENCONTRADO: MOVIL 100G

                Qué me aporta o permite este trabajo:
                    1. Que el programa no fallé si hay datos que no están contemplados en la tabla de agrupación de productos.
                    2. Que el programa no se coma ese ERROR sin dejar CONSTANCIA! 
                    3. Gracias a ese Mensaje de error, fácilmente podríamos SUBSANAR LAS CONSECUENCIAS DEL FALLO:
                       Podría hacer una query simple a la BBDD, una vez añadida la regla de agrupación para el nuevo producto:
                            MOVIL 100G          -> MOVIL
                       Que hiciera lo siguiente:
                        - OTRO -> false
                        - errores = ""
                        - movil = true
               El problema de fondo es que las ETLs NO CARGAN NI ACTUALIZAN DATOS QUE YA HAN SIDO CARGADOS. Solo cargan datos nuevos. 
               Por eso es importante que el programa deje constancia de los errores encontrados en un dato antiguo, para al menos tener una forma de identificar esos datos y poder subsanarlos.

               Esto serán cosas muy puntuales que pueden ocurrir.. Pero necesito tener un mecanismo definido de antemano para poder subsanarlas.

                En el mundo de la informática hablamos de PROCEDIMIENTOS DE RECUPERACION ANTE DESASTRES.

        ESTO ES MUY IMPORTANTE CUANDO DEFINO UN PROCESO ETL.
        No solo ponerme en los casos FELICES (HAPPY PATH) donde todo va bien y los datos son correctos. Sino también en los casos donde los datos no son correctos o no están contemplados en las reglas de negocio que he definido.

Esto que hemos hecho aquí, realmente nos va a pasar en casi todas las columnas... o en muchas de ellas.H
Hasta ahora solo habíamos definido las E y las L : EXTRACT y LOAD. Pero no habíamos definido la T: TRANSFORM.

Hay datos que me pueden llegar CODIFICADOS DE FORMA DIFERENTE... y necesito transformarlos y puede haber problemas.

Por ejemplo: encuestas escritas a mano:
    - Comunidad
    - Sexo
    Esos 2 datos necesito codificarlos:
    La etl de carga de encuestas escritas a mano tendrá tablas de codificación para esas columnas:
            H -> 1 (T_DIM_GENERO 1=Hombre)
            M -> 2 (T_DIM_GENERO 2=Mujer)
            varón -> 1 (T_DIM_GENERO 1=Hombre)
            mujer -> 2 (T_DIM_GENERO 2=Mujer)
            Castilla la Mancha -> 7 (T_DIM_COMUNIDAD 7=Castilla la Mancha)
            Castilla Mancha -> 7 (T_DIM_COMUNIDAD 7=Castilla la Mancha)
            Castilla-Mancha -> 7 (T_DIM_COMUNIDAD 7=Castilla la Mancha)
            C.Mancha -> 7 (T_DIM_COMUNIDAD 7=Castilla la Mancha)
            ???? -> DESCONOCIDO (T_DIM_COMUNIDAD 0=DESCONOCIDO)
            Y en el campo error, añado: "COMUNIDAD_NO_ENCONTRADA: C.Mancha"

    Una encuesta puede tener errores en varias columnas. Por eso el campo error de la tabla de hechos T_FACT_ENCUESTAS puede tener varios errores separados por comas:
    error = "COMUNIDAD_NO_ENCONTRADA: C.Mancha, SEXO_NO_ENCONTRADO: varón, PRODUCTO_NO_ENCONTRADO: MOVIL 100G"

    Y voy dejando constancia y posibilidad de subsanar los errores que puedan surgir en el futuro.

Antes dije que NO PROGRAMO CONTRA DATOS, PROGRAMO CONTRA ESPECIFICACIONES. Y eso es lo que estamos haciendo aquí. 
NO MIRO LOS DATOS QUE TENGO: "H,M, h, m, varón, mujer, Varon, Mujer, Varon, Mujer, HOMBRE, MUJER, HOMBRE, MUJER"
DEFINO UNA ESPECIFICACION: 
- Sease una tabla de codificación para el campo sexo, donde se definan unos valores, los que sean... 
- a día de hoy la popularé con: "H,M, h, m, varón, mujer, Varon, Mujer, Varon, Mujer, HOMBRE, MUJER, HOMBRE, MUJER"
- Pero si mañana me llega un valor distinto, el programa no fallará, sino que lo marcará como error en el campo correspondiente.
- Y lo asigno como DESCONOCIDO (T_DIM_GENERO 0=DESCONOCIDO)


---

Para un sistema como este:

- Una tabla de hechos:      T_FACT_ENCUESTAS
- Una tabla puente:         T_BRIDGE_ENCUESTAS_PRODUCTOS
- 7 tablas de dimensiones:  T_DIM_PRODUCTOS
                            T_DIM_CLIENTE
                            T_DIM_TIPO_INCIDENCIA
                            T_DIM_GENERO
                            T_DIM_COMUNIDAD
                            T_DIM_FECHA
                            T_DIM_OPERADOR
- Carga de datos en las tablas:
    T_DIM_FECHA:            Tabla con datos pregenerados de 10 años vista. No se carga dinámicamente.
    T_DIM_PRODUCTOS:        ETL que extrae datos del ERP de la empresa.
    T_DIM_CLIENTE:          ETL que extrae datos del CRM de la empresa.
    T_DIM_TIPO_INCIDENCIA:  ETL que extrae datos del CRM de la empresa.
    T_DIM_GENERO:           ETL que extrae datos del CRM de la empresa.
    T_DIM_COMUNIDAD:        ETL que extrae datos del CRM de la empresa.
    T_DIM_OPERADOR:         ETL que extrae datos del sistema de encuestas
    T_FACT_ENCUESTAS:       
                            ETL1 que extrae datos del sistema de encuestas informatizado
                            ETL2 que extrae datos de las encuestas realizadas a mano (excel)
                            Ambas 2 ETLs hacen queries adicionales a otras BBDD para completar los datos de la tabla de hechos T_FACT_ENCUESTAS.
                            Y Además hacen pivote de los productos contratados por el cliente para rellenar los campos movil, fibra, tv y otro de la tabla de hechos T_FACT_ENCUESTAS.
                            Y Normalizan campos de texto escritos a mano (sexo, comunidad) para rellenar los campos id_genero y comunidad_id de la tabla de hechos T_FACT_ENCUESTAS.
                            Voy plasmando los casos potenciales de error y cómo trato esos errores para que el programa no falle y deje constancia de los errores encontrados.

Otras cosas que hacemos cuando especificamos ETLs a tener en cuenta:
- Cada cuánto tiempo deben ejecutarse. Cada noche, Cada semana, Cada mes.
- Cantidad de datos que se espera:
    - en la carga inicial
    - en las cargas incrementales.
      Aquí hay una cosa importante a tener en cuenta o a especificar: Si las cargas incrementales tienen un volumen constante o no. 
      Esto es casi más importante que la cantidad de datos que se espera en las cargas incrementales.
      No es tanto si se cargan 1000 o 1000000 sino si habrá variabilidad IMPORTANTE en la cantidad de datos que se cargan.
- Notificaciones asociadas a las cargas:
  - Si se quieren notificaciones al iniciar! (que solemos querer!)
  - Si se quieren notificaciones al terminar! (que solemos querer!)
  - Si se quieren notificaciones si hay errores! (que solemos querer!)
  Estas notificaciones vienen con cantidades resumen.
    La ETL de carga de encuestas informatizadas ha finalizado su ejecución.
    Se han cargado 35.000 encuestas.
    Se han encontrado 2 encuestas con errores:
        - 1 encuesta con error en el campo genero="varón" (no encontrado en la tabla de codificación)
        - 871 encuestas con error en el campo producto_contratado="MOVIL 100G" (no encontrado en la tabla de codificación)
- Tiempo máximo de ejecución de la ETL
  - Los datos deben procesarse en menos de 2 horas, por ejemplo
  - Los datos deben procesarse en menos de 30 minutos, por ejemplo
  
  Por qué esto es importante:
    - Muchos de estos programas operan sobre las BBDD de producción... y pueden afectar gravemente a su rendimiento.
      Una query de estas que saca potencialmente millones de datos, puede tener un gran impacto en los usuarios "operaciones" que estén usando el sistema de producción. Habitualmente estos programas se ejecutan en ventanas de tiempo donde el sistema de producción tiene menos carga de trabajo. Por ejemplo, por la noche. Pero tengo solo una ventana de tiempo... que consensuó con la gente del sistema de producción.
      En esas ventanas de tiempo es necesario hacer muchos trabajos.,NO SOLO DE BI vive la empresa:
      - Backups de la BBDD de producción
      - Operaciones de mantenimiento de la BBDD de producción: REGENERAR INDICES, REORGANIZAR TABLAS, ...
      - Actualizaciones de software de la BBDD de producción

  Si manejo muchos datos y tengo una ventana muy pequeña... QUIZÁS NECESITE IR A TÉCNICAS BIGDATA PARA LAS ETLs.
    Usar herramientas de software como Apache Spark.
  Si no son tantos datos o hay una ventana más grande:
    Usar herramientas de software como Pentaho, Java Spring-batch

Hay veces, que la información es tan critica (NIVEL: OPERACIONES) nque necesitamos ir cargando datos en el DWH en tiempo real. 
Hay herramientas especializadas en el desarrollo de ETLs en tiempo real. Y en la generación de cuadros de mando en tiempo real.
Por ejemplo, El stack ELK: Elasticsearch, Logstash y Kibana.
ESTO ES MAS COMPLEJO... Y realmente, aunque las formas de trabajo son similares, NO ENTRA DENTRO DE LO QUE LLAMAMOS BI. 
Es un mundo aparte y necesitamos tener otro tipo de factores en consideración.
    Entra dentro de lo que llamamos "Monitorización y Observabilidad"


Hablamos el otro día de la pirámide organizativa de la empresa y de los niveles de información que se manejan en cada nivel.
- Nivel: OPERACIONES            ESTOS TOMAN DECISIONES PARA AHORA! PARA YA!
                                A veces pueden esperar 3 horas.
                                Otras veces ni 10 segundos! 
- Nivel: TACTICO                ESTOS NO TOMAN DECISIONES CON TIEMPOS APRETADOS... Las decisiones que toman son para los próximos días/semanas/meses
- Nivel: ESTRATEGICO            ESTOS NO TOMAN DECISIONES CON TIEMPOS APRETADOS... Las decisiones que toman son para los próximos años