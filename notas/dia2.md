3(4) partes en esta formación
- Repaso de conceptos estadísticos
 - Análisis de una variable de mi conjunto de datos
 - Análisis de dos variables de mi conjunto de datos
 - Análisis de tres variables de mi conjunto de datos (POCO HABITUAL. Nuestro cerebro humano NO DA)
   - Una cosa es que use 25 variables de mi conjunto de datos para hacer FILTROS! y reducir la base de análisis.
 - El mundo del análisis a partir de 3 variables es responsabilidad del DATA MINING
- Tratamiento informatizado de datos (almacenamiento, organización, transporte, etc.)
- Análisis BI (FECHA)
- Gestión de proyectos de BI

# Tipos de datos desde el punto de vista estadístico

- Datos cualitativos  (No cantidades con unidad medida)
  - Nominales           NOMBRES         
  - Ordinales           NOMBRES CON RELACION DE ORDEN ENTRE SI
- Datos cuantitativos (Cantidades -> Unidad de medida)

                        Clasificar   Ordenar    Operar
    Nominales               √   
    Ordinales               √           √
    Cuantitativos           √           √         √ 

    Cuantitativo > Ordinal > Nominal

# Análisis de una variable de mi conjunto de datos

## Tablas de frecuencia

Frecuencia absoluta: Número de veces que se repite un valor en el conjunto de datos -> Gráficos de barras
Frecuencia relativa: Frecuencia absoluta / Número total de datos (Porcentaje)       -> Gráficos de sectores

A pesar de que los datos cuantitativos son CLASIFICABLES, acabo con tantos grupos diferentes que en muchos casos hacer una tabla de frecuencia de datos cuantitativos no tiene sentido/No aporta valor. NO RESUMO.
En estos casos, transformo la variable Cuantitativa en una variable Cualitativa (ORDINAL) y hago la tabla de frecuencia de la variable cualitativa resultante. Genero intervalos de valores y agrupo los datos en esos intervalos.

## Indicadores estadísticos

### Medidas de tendencia central (Por dónde están las cosas en general con mi variable (groso modo) )

                Nominal      Ordinal     Cuantitativo
- Moda            √             √             ~
- Mediana         x             √             √
- Media           x             x             √

Ojo a la media y los datos cuantitativos. Si los datos son muy asiméticos (Muchos manolos) la media se perturba y no es representativa. En estos casos, la mediana es más representativa. La media nos despista... no nos ayuda a entender los datos... a pesar de ser un valor real.

### Medidas de posición.

No siempre me interesa saber por dónde están las cosas en general con mi variable, sino que me interesa saber por dónde están las cosas en un determinado porcentaje de los datos. Para ello, utilizo medidas de posición.

Percentiles, Cuartiles, Deciles. 
Me permiten saber por dónde están las cosas para la mayoría de los casos de estudio, o para la inmensa mayoría, o para la minoría, o para la inmensa minoría.

Tiempo de resolución de incidencias en una empresa.
    El valor medio puede interesar... para estimaciones... pero no tanto.
        Si quiero saber cuántas incidencias saca una persona al día de trabajo.. me puede servir la media.
    El mínimo y máximo son anécdotas... pasamos de ellos.
    Me suele interesar algo como el P95: Quiero saber el tiempo máximo que tardo NORMALMENTE (95% de los casos) en resolver una incidencia. El 5% restante son casos excepcionales que habrá que tratar de forma independiente. El P95 me da un límite de tiempo.

Mediana = D5 = Q2 = P50

### Medidas de dispersión

Nunca puedo dar una medida de tendencia central sin su medida de dispersión asociada... Es engañoso. Puedo malinformar en lugar de informar.

Media -> Desviación típica (σ)
Mediana -> Rango intercuartílico (IQR)

...



---

VOCABULARIO

    BBDD                    Están en entornos de producción.   
                            Contienen datos vivos
      v
     ETLs 
      v
    DataLake                Contienen datos históricos en bruto 
                            (en ocasiones también pueden tener datos que están vivos... pero no se gestionan aquí)
                            El concepto de DataLake es relativamente nuevo.
                            Antiguamente lo normal es que los datos de las BBDD de producción se movían directamente a un DataWarehouse, pero ahora se hace un paso intermedio que es el DataLake.
                            Hoy en día hemos aprendido que hay muchos potenciales análisis que podemos hacer sobre los datos...
                            Y cada uno requiere de un tipo de preparación diferente.
                            Es preferible dejarlos en bruto (como están en origen en un DATALAKE) y cuando ya tengo un objetivo concreto, los transformo y los cargo en un almacenamiento específico creado para ese objetivo concreto (DataWarehouse).
                            El mismo dato puede acabar con distintas transformaciones aplicadas en varios datawarehouses diferentes, según los objetivos de análisis que tenga cada uno de ellos.
      v
     ETLs
      v
    DataWarehouse           Contienen datos históricos preparados.


# ETLs. 

Son programas que permiten mover datos de unos entornos a otros.

El nombre significa Extract, Transform and Load => Extraer, transformar y cargar.

Pero realmente hay muchas variantes:
EL -> Extract and Load (sin transformación)
TEL -> Transformo los datos en origen, los saco de allí y los cargo en destino.
ETL -> Extraigo los datos, los transformo y los cargo en destino.
ELT -> Extraigo los datos, los cargo EN BRUTO en destino y allí los transformo.
       OJO A ESTA. Es la única variante que tiene nombre propio en la industria,
       y es la que domina hoy en los entornos cloud: el almacén tiene más
       capacidad de cálculo que cualquier servidor intermedio, así que compensa
       cargar primero y cocinar después, dentro del propio almacén.
TELT -> Transformo los datos en origen, los saco de allí y los cargo en destino. Una vez cargados, los transformo en destino.

Estos conceptos: BBDD/DataLake/DataWarehouse/ETL son conceptos relativos a la forma de almacenar los datos... y de moverlos entre esos tipos de almacenamiento. 

No son conceptos relativos al análisis de datos en sí mismo. Son conceptos relativos a la gestión de los datos.

Con respecto al análisis de datos hay varios tipos de análisis diferentes que podemos hacer:
- Business Intelligence (BI) -> Análisis de datos históricos para entender el pasado y el presente y poder tomar decisiones de negocio (a futuro). Realmente es la forma más simple de análisis de datos. Estamos aplicando técnicas de estadística descriptiva más o menos sencillas a los datos.
    Mirar lo que acabamos de decir!
    - Me baso en datos del pasado para entender el presente/futuro y poder tomar decisiones de negocio a futuro.
      Estoy jugando a ser ADIVINO!
      Es totalmente IMPOSIBLE predecir el futuro con certeza. Hay muchas técnicas desarrolladas desde hace siglos para ello:
      - Lectura de posos del café
      - Bolita de cristal
      - Lectura de cartas del tarot
      - Lectura de la mano
      - Técnicas Estadísticas.. Y parecen que estas suenan guay y tienen mucha credibilidad.. porque se hacen cuentas matemáticas.
       PERO NO NOS ENGAÑEMOS.
       Por más cuentas que yo haga, estamos haciendo una suposición de partida ENORME!
       Y es que el futuro va a seguir comportándose como el pasado. Y eso es una suposición que no tiene por qué cumplirse. 

    Ejemplo:
        En el año 2020 hemos vendido 200k zapatos
        En el año 2021 hemos vendido 300k zapatos
        En el año 2022 hemos vendido 450k zapatos
        En el año 2023 hemos vendido 600k zapatos
        En el año 2024 hemos vendido 800k zapatos
        En el año 2025 hemos vendido 1000k zapatos
        Espero, pienso, debería si todo va bien este año 2026 vender 1200k zapatos.

        OJO A LA LINEA QUE ACABAMOS DE CRUZAR:
        Contar lo que vendí de 2020 a 2025 es DESCRIPTIVO. Es BI en sentido estricto.
        Decir que en 2026 venderé 1200k YA NO LO ES: eso es ANALITICA PREDICTIVA.
        Y ahí es donde empieza el juego de los adivinos.

        La escalera completa, que es el índice del resto del curso:
            ¿Qué ha pasado?       DESCRIPTIVA    informes, cuadros de mando
            ¿Por qué ha pasado?   DIAGNOSTICA    drill-down, segmentación
            ¿Qué puede pasar?     PREDICTIVA     <- aquí empieza la adivinación
            ¿Qué debería hacer?   PRESCRIPTIVA
        Como no tengo almacenes suficientes para tantos zapatos... Hay que alquilar almacenes. DECISION DE NEGOCIO BASADA EN DATOS!

- Data Mining (Minería de datos)
    Esto es más avanzado. Los humanos entendemos bien hasta 2/3 variables y sus relaciones.
    Más no. 
    Cuando tengo más variables que quiero analizar, necesito técnicas más avanzadas que me permitan entender las relaciones entre esas variables. Y eso es lo que hace el Data Mining.
    En realidad lo que hago con data mining es pedirle (suplicarle) a una computadora que estudie ella (ya que mi cerebro no da) las relaciones entre esas variables para ver si encuentra algún patrón raro... que no encaje... que no sea esperable... que no sea lo que yo esperaba... y que me pueda dar una pista de que hay algo interesante en mis datos que yo no había visto.
    NO SE QUE BUSCO. Solo pido a la computadora QUE BUSQUE! A VER SI SALE ALGO...
    Igual que cuando hago MINERIA. Cojo pico y pala y me pongo a escarbar... a ver si encuentro algo interesante. No sé qué busco... pero busco algo que me pueda interesar.

- Machine Learning (Aprendizaje automático)
    Esto es todavía más avanzado.
    Pedimos a la computadora (nosotros humanos no somos capaces de hacerlo) que genere un programa (MODELO) que sea capaz de:
    PREDECIR/CLASIFICAR/GENERAR datos a partir de datos históricos.
    AQUI JUGAMOS AUN MAS a ADIVINAR!

    Hay muchas técnicas que usamos en el mundo del MACHINE LEARNING.. para crear esos programas.
    La que más éxito ha dado son las REDES NEURONALES.

    Y hay una disciplina entera dentro del mundo del MACHINE LEARNING para la generación de redes neuronales: DEEP LEARNING (Aprendizaje profundo). Que es la que más éxito ha dado en los últimos años.

Ejemplo.
Sea una compañía de Alarmas!
Esa empresa instala alarmas en casas/negocios.
Esas alarmas tienen un panel de control... Antes iba con claves (teclado) hoy en día con un llaverito.
Pones llaverito y activa/desactiva alarma

Lo que muchas veces no sabemos es que en la sede de la empresa de las alarmas hay unas BBDD que registran cada operación que se hace.
Quién (cada persona de la familia tiene sus códigos/llaveritos...), Cuándo (fecha y hora), Dónde (código de la alarma), Qué operación (activar/desactivar alarma, abrir puerta, etc.) y muchas otras cosas más.

Hay una motivación REAL para guardar esos datos.
Ahora bien... de repente la empresa de las alarmas se da cuenta que tiene MOGOLLON DE DATOS de muchos años guardados.
Y a alguien se le enciende la bombilla. PUEDO HACER ALGO MAS CON ESOS DATOS?... ya que los tengo.

Y se lanza un proyecto de Data Mining. NO SE SABE QUE SE BUSCA. Solo se busca! A ver si sale algo.
Y SALIÓ!
A la postre lo ves claro... A priori no se te habría ocurrido.
Resulta que los humanos generamos patrones de uso de las alarmas. Las ponemos a la misma hora dependiendo del dia de la semana/mes.
Y se puede hacer un seguimiento de esos patrones.
Igual que puedo hacer un seguimiento de cuándo se ROMPEN ESOS PATRONES!
Y de repente se cruza esa BBDD con otra BBDD de bajas de clientes.
Y sorpresa. Se identifica que tras ciertas rupturas de patrón hay clientes que se dan de baja pasado un tiempo.
Por qué ? Ni idea. El data mining no lo cuenta. SOLO IDENTIFICA ESE HALLAZGO, ESE PATRON.

Esa información se pasa al dpto de marketing y se ponen a investigar... Llamadas de teléfono, emails.
Sorpresa: RESULTA QUE HAY GENTE QUE SE QUEDA EN PARO! Y cuando se quedan en paro, el patrón de uso de la alrma se vuelve loco.
- Se quedan un dia hasta las 5am haciendo un maraton de netflix
- Otro dia a las 8 arriba que hay una entrevista
- Otro día a las 10 es el perrito el que le despierta que tiene que mear!
- DESCONTROL!

Con esa información de vuelta SE DECIDE LANZAR UN PROYECTO DE MACHINE LEARNING.

El objetivo, generar un programa que vaya monitoreando los patrones de uso de las alarmas y que sea capaz de predecir cuándo un cliente se va a dar de baja próximamente. Y ese programa, sobre los datos histórico alcanza una tasa de éxito del 83% de aciertos. Y eso es un éxito! Porque el departamento de marketing no tenía ni idea de cómo predecirlo. Ahora sí.

Ese programa se pone en manos del dpto de marketing... y cuando se enciende la luz: LLAMADA DE TELEFONO:
OIGA ! COMO CLIENTE PREMIUM QUE ES USTED, le vamos a bajar la cuota los próximos 4 meses al 25%. Es una oferta especial para usted que es muy buen cliente!
A mi que me he quedado en paro me viene que ni pintado. La alarma es de los primeros gastos que quizás quitaría.
La empresa no gana dinero en esos meses... pero no pierde un cliente que le costaría un huevo recuperar!

ES LEGAL QUE LA EMPRESA HAGA USO DE LOS DATOS PARA ESTO? ALEGALIDAD (HAY UN VACIO donde las empresas se mueven: MEJORAR LA CALIDAD DEL SERVICIO Y OFERTAS PERSONALIZADAS)
ES MORAL? ESTO ES OTRO TEMA... que no vamos a discutir en este curso.

---

Otro ejemplo de análisis de datos complejo.

> SOY UNA EMPRESA DE SEGUROS DE AUTOMOVILES.

Voy a dar tasaciones a los clientes (cuanto les voy a cobrar por su seguro). O incluso a decidir si aseguro a un cliente o no!
Antiguamente esta decisión la hacía un humano... con mucha experiencia y conocimiento del sector.

Pero hoy en día, con la cantidad de datos que hay, es imposible que un humano pueda analizar todos esos datos y tomar decisiones correctas.

Imaginad los siguientes datos:

- Años de carnet de conducir
- Edad del conductor
- Kilometros que recorre al año

De alguna forma, la empresa lo que quiere es determinar la probabilidad de que un cliente vaya a dar un parte el próximo año.
Si la probabilidad es muy alta le cobro caro o no le aseguro.

Esos datos que he puesto arriba guardan relación con esa probabilidad?
El tema es muy complejo. Y DE HECHO la realidad es que esos datos NO GUARDAN RELACION CON ESA PROBABILIDAD.
Lo que hay es FACTORES INTERMEDIOS que son los que realmente guardan relación con esa probabilidad. Y esos factores intermedios son MUCHOS y muy complejos de analizar. Por ejmplo.

                                                    EXPERIENCIA AL VOLANTE      + -> -      PROBABILIDAD DE ACCIDENTE
                                                    EXPOSICION AL PELIGRO       + -> +  
                                                    CAPACIDAD DEL CONCUCTOR
                                                    AGRESIVIDAD AL CONDUCIR
                                                Estos factores son independientes entre si

Años de carnet de conducir. Más años de carnet significa que la persona tiene más experiencia al volante (a priori)... si hace pocos kilometros al año.. entonces no.
Edad del conductor. 
    Más edad implica más experiencia al volante (a priori)... si muchos años que tiene carnet y hace bastantes kms al año si no no.
    Ahora.. más años implica menores REFLEJOS.
Sexo, Edad -> AGRESIVIDAD AL CONDUCIR.

    OJO CON EL SEXO: en la Unión Europea NO SE PUEDE USAR para tarificar seguros.
    Lo prohibió el Tribunal de Justicia de la UE (sentencia Test-Achats, 2011,
    en vigor desde diciembre de 2012).
    Y es un caso precioso: la variable ES estadísticamente predictiva y aun así
    está PROHIBIDO usarla. Que un dato prediga bien no significa que se pueda
    usar. Ahí tenemos el debate del sesgo y la ética en los modelos.

Kilometros al año:
    - Más kilometros = Más experiencia           -> A priori - probabilidad de accidente
    - Más kilometros = Más exposición al peligro -> A priori + probabilidad de accidente
  
Pero el tema es más complejo.

e importa a la empresa acertar con esa persona? NADA
Igual que a un banco no le importa NADA si una persona ha estimado que le va a pagar la hipoteca y luego resulta que no se la paga.

LO UNICO QUE LE IMPORTA A LA EMPRESA ES SER CAPAZ DE ANTEMANO DE SABEREN CUANTOS SE VA A EQUIVOCAR Y CUANTOS VA A ACERTAR.
La empresa sabe que esto es jugar a los adivinos... y que se va a equivocar. Lo que quiere saber es EN CUANTOS.
Si sé que me quivoco en el 20%. El importe de ese 20% lo subo a los otros 80%, le aplico mi 10% y sigo ganando pasta!


# BigData

Es cuando tengo:
- Un volumen de datos 
- Produzco datos a tal velocidad
- Que tienen una ventana de vida útil tan corta
Que las ténicas que hemos utilizado tradicionalmente no valen ya.

En estos casos, necesito ir a técnicas BIGDATA.

BIGDATA no tiene que ver con análisis de datos.

Puedo aplicar técnicas de bigdata para lo que sea que necesite hacer con mis datos:
- Quizás solo almacenarlos 
- Quizás solo transportarlos
- Quizás analizarlos

Para cualquiera de esas cosas puede ser que necesite aplicar técnicas BIGDATA.

> Lista de la compra

 200 items (producto y cantidad) -> Teléfono en una app de notas
 2000 items (producto y cantidad) -> EXCEL
 200.000 items (producto y cantidad) -> EXCEL SE HACE KKITA! BBDD (MySQL, POSTGRES)
 200.000.000 items (producto y cantidad) -> MAL... empiezan a ir lentitas. SQLSERVER
 20.000.000.000 items (producto y cantidad) -> ORACLE
 ----
 20.000.000.000.000 Y AHORA QUE? QUE ME QUEDA?


MySQL es una BBDD relacional
SQLServer (Microsoft) es una BBDD relacional
PostgreSQL es una BBDD relacional

SQL es un lenguaje para consultar BBDD relaciones

> Tengo un USB que acabo de sacar de la caja. De 16GBs.

Quiero guardar en él un archivo de 4 Gbs.. puedo?
DEPENDE... Depende del formato del disco.
    FAT16
    FAT32
    NTFS
    EXT4
    ZFS
Una cosa es la capacidad total del disco... y otra lo más grande que puedo guardar.
Imaginad una habitacion Grande, pero la lleno de estanterías con huecos muy pequeños.
En total quizás tengo 20m3 de espacio de almacenaje, pero por como estan montadas las estanterias no entran cajas más grandes que una caja de zapatos.

Un formato como FAT32 solo admite ficheros de no más de 4Gbs, aunque el disco tenga
libres 400Gbs de espacio. Por eso el archivo de 4Gbs del USB de 16Gbs NO ENTRA si el
USB viene formateado en FAT32, que es como vienen casi todos de fábrica.
(FAT16, el anterior, era aún peor: 2Gbs por fichero.)

Bueno... hay formatos que soportan archivos más grandes... pero todos tienen límite.
Y el primer límite me lo dan los discos físicos. 
Puedo comprar discos de 2Tbs, 10 Tbs
Pero.. y si tengo un archivos de 20eB = 20.000.000 Tbs (1 exabyte = 1 millón de Tbs) Qué disco compro? NO EXISTE. Qué formato le pongo? NO EXISTE
Y no hablemos de si tengo un archivo de 20ZB = 20.000.000.000 Tbs (1 zettabyte = 1000 exabytes) Qué disco compro? NO EXISTE. Qué formato le pongo? NO EXISTE

PROBLEMON!AHORA QUE? QUE ME QUEDA?

> Juego de teléfonos

Clash Royale 2v2 (3 minutos)

4 jugadores. En un segundo podía hacer fácil 2 movimientos -> Es necesario mandar 2x3 mensajes a los teléfonos de mis contrincantes/compañero.
Mis 2 movimientos -> 6 mensajes/segundo
Somos 4 jugando -> 24 mensajes por segundo

Pero en sus mejores tiempos podía haber 50k guerras en marcha a la vez. 50k guerras x 24 mensajes/segundo = 1.200.000 mensajes/segundo
Que deben llegar a destino en menos de 500ms para que el juego tenga sentido, sea interactivo y divertido.

Genero datos a cascoporro. Y tienen una vida útil muy baja (500ms). Si no llegan ya no son útiles.
NO QUIERO ANALIZAR LOS DATOS. NO QUIERO ALMACENAR LOS DATOS. SOLO NECESITO TRANSMITIR LOS DATOS.
No hay computadora en el mundo capaz de soportar 1.2 millones de mensajes/segundo. 

PROBLEMON!AHORA QUE? QUE ME QUEDA?

A estos problemas da solución el BIG DATA.
Básicamente se trata de formar una GRANJA DE COMPUTADORAS (de mierda) - COMMODITY HARDWARE pero poniéndolos todos a trabajar como si fueran 1 solo.

BIGDATA es cuando creo esa granja e instalo programas y creo programas especiales para sacar provecho a esa GRANJA de computadoras.
Tiene que ver con INFRAESTRUCTURA + SOFTWARE

Quien arranca con esto es GOOGLE. 

Para escrapear la web y atender a las queries de los usuarios va montando tres piezas,
y EN ESTE ORDEN:
   2003  GFS (GOOGLE FILE SYSTEM)  su propio sistema de archivos distribuido
   2004  MAPREDUCE                 su forma de repartir el cálculo por la granja
   2006  BIGTABLE                  su almacén de datos sobre todo lo anterior

Y publicaron unos papers de cómo lo habían hecho.
Posteriormente Doug Cutting (con Mike Cafarella) creó una copia opensource de esos
programas: HADOOP.
ESE ES EL ORIGEN DEL BIGDATA.

Hoy en día, en muchas empresas, principalmente por el mal uso del término por parte de muchos periodistas y comentaristas, se usa el término bigdata para referirse a técnicas de análisis de datos. Pero no tiene por qué ser así.

---

# Tratamiento informático de datos.

Ayer hablamos de los tipos de datos desde el punto de vista estadístico (Nominales, Ordinales, Cuantitativos). 
Hoy vamos a hablar de los tipos de datos desde el punto de vista informático. Eso tiene su impacto. Importante.

Los ordenadores por dentro hablan en ceros y unos (binario). En realidad eso es mentira.
Los ordenadores entienden de estados duales:
- Entra corriente por una patilla del microprocesador o no entra corriente en un momento dado?
- Entra corriente por un cable de red o no entra corriente por un cable de red en un momento dado?
- Hay un trozo del HDD magnetizado o no?

Los humanos solemos asignar a esos estados duales un 0 o un 1.
Si hay un trozo del disco magnetizado -> 1
Si no hay un trozo del disco magnetizado -> 0
Si entra corriente por un cable de red -> 1
Si no entra corriente por un cable de red -> 0

Lo representamos así para entendernos. Y es una buena aproximación.

Los datos los guardamos en 2 sitios dentro de un ordenador:
- RAM
- Disco duro

Lo normal es que los datos estén guardados de forma persistente en el disco duro y que cuando los necesitamos, los cargamos en la RAM para poder trabajar con ellos.

El disco duro es como un cuaderno de cuadrícula enorme. Cada cuadricula la puedo rellenar de lapicero o dejarla en blanco.
Los humanos, lo solemos representar con un 1 o un 0.
Si hemos rellenado la cuadricula (magnetizado ese trozo de HDD) -> 1
Si no hemos rellenado la cuadricula (no magnetizado ese trozo de HDD) -> 0

Igual pasa con la Memoria RAM. EXACTAMENTE IGUAL. La diferencia es que:
- LA RAM funciona más rápido que el disco duro
- LA RAM es volátil. Cuando apago el ordenador, la RAM se borra en automático. 

Ahora bien...si en el HDD solo puedo marcar casillas o no marcarlas (0/1), como hago para guardar textos, números, fechas, imágenes, vídeos, etc.?
Inventamos sistemas de codificación. Sistemas que nos permiten representar esos datos complejos en binario (ceros y unos).

Hay datos muy simples: BOOLEANOS (LOGICOS)
- Solo pueden tomar dos valores: VERDADERO (1) o FALSO (0)
  Llueve? SI -> Marco casilla (magnetizo) -> 1
  Llueve? NO -> No marco casilla (no magnetizo) -> 0
  Estos los usamos mucho. Este tipo de datos. 
  Puedo representarlos en una cuadrícula. 
  Esa cuadricula del HDD que relleno o no, es lo que llamamos un BIT. Un bit es la unidad mínima de información que puede almacenar un ordenador. Un bit puede tomar dos valores: 0 o 1.

  Qué significa que una casilla la rellene? ESO LO DECIDO YO.
  Puede ser que si esté rellena es que ha llovido, que me han comprado un producto, que he aprobado un examen, que he hecho una llamada de teléfono, etc.
  O puedo decidir que si no está rellena es que ha llovido, que me han comprado un producto, que he aprobado un examen, que he hecho una llamada de teléfono, etc.

Normalmente, salvo estos datos simples, no me vale con una casilla. Necesito más de una casilla para guardar un dato.
Hay datos que toman más de 2 valores: 

EDAD DE UNA PERSONA 0-120 -> 121 valores diferentes.
Eso no entra en una casilla.
En una casilla puedo marcar o no. 2 valores potenciales... pero quiero poder guardar 121. Necesito más casillas.
Esas casillas en el mundo de la informática las agrupamos de 8 en 8. 8 casillas es lo que llamamos un byte.

    Esas casillas pueden tomar muchas combinaciones

    1 casilla    0      2 valores potenciales
                 1

    2 casillas   00                                     rubio
                 01     4 valores potenciales           morena
                 10                                     castaña
                 11                                     otros (pelirrojo, blanco canoso, etc.)
    
    3 casillas   000
                 001
                 010
                 011    8 valores potenciales
                 100
                 101
                 110
                 111

  8 casillas : 2^8 = 256 valores potenciales
  Si quiero guardar las edades de una persona, puedo hacerlo en 8 casillas. Podría guardar hasta 255 años. Perfecto. Me sobra espacio.

    00000000 -> 0 años
    00000001 -> 1 año
    00000010 -> 2 años
    00000011 -> 3 años
    00000100 -> 4 años
    ...
    11111111 -> 255 años

Claro.. el ordenador solo guarda casillas marcadas o no.
Lo que significan lo digo yo.
Pueden ser números, letras, colores...
Incluso, siendo números, puede haber variaciones:

                    EDAD (años)    TEMPERATURA (ºC)
    00000000 ->     0                   -128
    00000001 ->     1                   -127
    00000010 ->     2                   -126
    00000011 ->     3                   -125
    00000100 ->     4                   -124
    10000000 ->     128                  0
    ...
    11111111 ->     255                  127

    Y con 8 casillas puedo guardar 256 valores diferentes... que pueden ser:
    Edades entre 0-255 años
    Temperaturas entre -128ºC y 127ºC

Esto son las CODIFICACIONES. Sistemas que nos permiten representar datos complejos en binario (ceros y unos).

Qué pasa con los textos?
Vamos a representar letra a letra: "FEDERICO": F, E, D, E, R, I, C, O

Cuántos caracteres tenemos potenciales para guardar?
28-29 letras... En informática: A a á à ä â Son diferentes grafías... Son diferentes caracteres.

Los americanos se apañan con 256 caracteres para la mayor parte de las cosas:
A-Z, a-z, 0-9, signos de puntuación, símbolos matemáticos, etc.
()+-[]{}_/€$

Los españoles necesitamos más caracteres. Ñ, á, é, í, ó, ú, ü, ¡, ¿, etc.
Los chinos? FLIPAS... El alfabeto chino son más de 8000 caracteres diferentes. Y eso es solo el chino simplificado. El chino tradicional son más de 50.000 caracteres diferentes.
Japonés? FLIPAS... El alfabeto japonés son más de 2000 caracteres diferentes. Y eso es solo el japonés simplificado. El japonés tradicional son más de 50.000 caracteres diferentes.

Hay un estandar que recoge TODOS LOS CARACTERES QUE USA LA HUMANIDAD: UNICODE. Va por unos 150.000 caracteres diferentes. Y sigue creciendo.
A ver.. yo soy español no uso chino... no necesito tanta variedad.
Y los americanos menos aún.

Hay distintos JUEGOS DE CARACTERES dependiendo los caracteres que quiera representar.

    El más simple ASCII: 128 caracteres diferentes (usa solo 7 de las 8 casillas).
    Suficiente para los americanos.
    Al aprovechar la octava casilla llegamos a 256 y aparecen los juegos "extendidos",
    como ISO-8859-1 (latin-1), que es el que mete la ñ y las vocales acentuadas.

                            ESPAÑA
                ASCII       ISO-8859-1     UTF-8
    00000000 -> 
    00000001 -> 
    00000010 -> 
    00000011 -> A
    00000100 -> B
    00010101 -> Z
    00010110 -> a
    00010111 -> b
    00011000 -> c
    00011001 -> d
    00111010 -> 1               á
    00111011 -> 2               é
    01111100 -> #               1
    ....
                256 caracteres diferentes

UTF-8 es el estandar a dia de hoy para el almacenamiento de textos.
Soporta todos los caracteres de UNICODE.
Un caracter ocupa:
- Si es un caracter simplón: A-Za-z0-9., etc. -> 1 byte (8 casillas)
- Si es un poco más complejo: á, é, í, ó, ú, ü, ñ, à, etc. -> 2 bytes (16 casillas)
- Si es de otro alfabeto: árabe, hebreo, griego, cirílico -> 2 bytes (16 casillas)
- Si el caracter es mu raro (pa' nosotros: chino, japonés, coreano) -> 3 bytes (24 casillas)
- Y los rarísimos, como los emojis -> 4 bytes (32 casillas)
- Es un sistema de codificación variable. Un caracter puede ocupar 1, 2, 3 o 4 bytes dependiendo de su complejidad.
- UTF empieza como ASCII y va añadiendo caracteres según la necesidad. Por eso es un sistema de codificación variable.

En 1 byte puedo guardar: 2^8 = 256 valores diferentes... Si son números positivos: 0-255
En 2 bytes puedo guardar: 2^16 = 65.536 valores diferentes... Si son números positivos: 0-65.535
En 4 bytes puedo guardar: 2^32 = 4.294.967.296 valores diferentes... Si son números positivos: 0-4.294.967.295

> Por qué motivo todo esto importa?

Imaginad que quiero guardar en una BBDD un DNI (españa). Cómo lo guardo?  12345678A
1-8 dígitos + LETRA
- Puedo guardarlo como texto... caracter a caracter. 
  En este caso cuánto ocuparía? 9 bytes (9 caracteres x 1 byte cada caracter)
- Puedo guardar el número por un lado y la letra por otro.
   El número ocuparía 4 bytes (32 casillas) y la letra 1 byte (8 casillas). Total 5 bytes.
   REDUCCION DE ESPACIO EN DISCO DEL 44% (9 bytes -> 5 bytes) CASI LA MITAD!
- Puedo ser más agresivo. La letra del DNI se calcula en automático desde el número. No necesito guardarla.
  Si solo guardo el número, ocuparía 4 bytes (32 casillas). REDUCCION DE ESPACIO EN DISCO DEL 55% (9 bytes -> 4 bytes) MÁS DE LA MITAD!


Dependiendo de cómo elija guardar los datos, puedo ahorrar mucho espacio en disco.
Pero es solo el principio.
Si los datos ocupan la mitad, tardaré la mitad de tiempo en leerlos y escribirlos. 
Tardaré la mitad de tiempo en transferirlos a la memoria RAM para trabajar con ellos.
Tardaré la mitad de tiempo en transferirlos a otra computadora por la red.
Tardaré la mitad de tiempo en procesarlos en la CPU.

El elegir una buena forma de guardar los datos es CLAVE/CRITICO en una empresa.
Normalmente las empresas tienen un departamento encargado de definir las políticas de almacenamiento de datos. Cómo se guardan, cómo se leen, cómo se transportan, etc.

Cuando diseño un proceso/proyecto BI (y esto implica crear un DATAWAREHOUSE) es clave definir cómo voy a guardar los datos. Cómo voy a leerlos, cómo voy a transportarlos, etc. Quiero que no me salga CARO y quiero que las operaciones (análisis de datos) sean rápidas. Y eso depende de cómo guarde los datos.

Una cosa es qué análisis voy a hacer de los datos.
Y otra cosa es cómo voy a guardar los datos para que ese análisis sea rápido y barato.
RAPIDO / BARATO son 2 restricciones / variables con las que tengo que jugar a la hora de diseñar un proyecto BI. Tengo que minimizar el coste de almacenamiento y maximizar la velocidad de análisis.

Desde el punto de vista informático hablamos de:
- Datos booleanos: conceptualmente 1 BIT (2 valores), aunque en la práctica casi
  todos los sistemas reservan 1 byte entero para guardarlo
- Datos numéricos:
  - Enteros:
      - Cortos: 1 byte (permiten guardar hasta 256 valores diferentes)
      - Medianos: 2 bytes (permiten guardar hasta 65.536 valores diferentes)
      - Largos: 4 bytes (permiten guardar hasta 4.294.967.296 valores diferentes)
      - Muy largos: 8 bytes (permiten guardar hasta 18.446.744.073.709.551.616 valores diferentes)
    - Decimales:
      - Cortos: 4 bytes (unas 7 cifras SIGNIFICATIVAS, no decimales)
      - Largos: 8 bytes (unas 15 cifras significativas)
      - Muy largos: 16 bytes (unas 34 cifras significativas)
- Fechas
  - Fecha:        4 bytes
  - Fecha y hora: 8 bytes 
- Textos (UTF-8)... los contamos por caracter:
  - Caracter simplón: 1 byte
  - Caracter un poco más complejo (á, ñ) u otro alfabeto: 2 bytes
  - Caracter muy raro (chino, japonés, coreano): 3 bytes
  - Emojis y similares: 4 bytes

Estos son los principales tipos de datos. 
Hay más que soportan distintos sistemas:
- Geoposicionamiento: (latitud, longitud) 16 bytes
- IP: 4 bytes (IPv4) o 16 bytes (IPv6)
- Image: Depende del formato de la imagen. JPEG, PNG, BMP, GIF, etc.
- Video: Depende del formato del video. MP4, AVI, MKV, MOV, etc.
- Audio: Depende del formato del audio. MP3, WAV, FLAC, AAC, etc.

En muchos casos, convertimos datos que para los humanos son textos a números. Simplemente para:
- Ahorrar espacio en disco
- Ahorrar tiempo de lectura/escritura/red
- Ahorrar tiempo de procesamiento en la CPU

Se hace mucho.
Es parte de esa T(Transform) de los procesos ETL. Transformar los datos para que ocupen menos espacio y se procesen más rápido.
Este trabajo lo solemos llamar RECODIFICACION DE DATOS.
Por ejemplo: 
    CAMPO PAIS:
        "España"
        "Francia"
        "Estados Unidos de América"
    Como tenga que guardar esto como caracteres... necesito la hueva
        España -> 7 bytes
        Francia -> 7 bytes
        Estados Unidos de América -> 26 bytes (la é ocupa 2)
    Ahora si los codifico como números:
        España -> 1
        Francia -> 2
        Estados Unidos de América -> 3
        ...
        En total 255 paises.

        En 1 bytes me entran 255 paises. Perfecto.
        Aquí no he ahorrado un 45 o un 55%.
        Mirándolo por registro: "Estados Unidos de América" pasa de 26 bytes a 1 byte.
        Un 96% de ahorro. Y los tres juntos, de 40 bytes a 3.

Este trabajo le hacemos muchísimo en transformación de datos. 
A nosotros, desde el punto de vista informático nos interesaría que todos los datos los pudieramos representar como números.
No siempre puedo... Por ejemplo, NOMBRE Y APELLIDOS, EMAIL... Dirección... Pues no queda otra... COMO TEXTO!

Las variables NOMINALES y las VARIABLES ORDINALES son las que más nos interesan recodificar a números.
Las cuantitativas ya son números... y no hay que recodificarlas.

Y por eso es tan importante que nos olvidemos de que los datos CUANTITATIVOS son números, ya que los datos NOMINALES y ORDINALES también los vamos representar como números.
Y evidentemente, aunque estén representados como números, no podemos hacer operaciones matemáticas con ellos. No les voy a calcular una media ni cosas así. Aunque sean números.

Y es importante llevar por separado y con claridad: EL TIPO DE DATO ESTADISTICO y EL TIPO DE DATO INFORMATICO.
    - TIPO DE DATO ESTADISTICO: Nominal, Ordinal, Cuantitativo -> Tiene que ver con la naturaleza del dato y con el análisis estadístico que puedo hacer con él.
    - TIPO DE DATO INFORMATICO: Texto, Número, Fecha, Booleano, Geoposicionamiento, IP, Imagen, Video, Audio -> Tiene que ver con cómo se almacenan y procesan los datos en un sistema informático.

Y necesito estudiar ambas cosas cuando me enfrento a un proyecto de BI.

Aquí hay un problema que luego nos encontramos al ponernos a trabajar. CASI TODAS LAS HERRAMIENTAS DE BI hablan de tipos de datos informáticos. Y no hablan de tipos de datos estadísticos.
Hay salvedades: SPSS permite definir el tipo de dato estadístico de cada variable.
POWERBI (Microsoft) permite definir el tipo de dato informático de cada variable, pero no permite definir el tipo de dato estadístico.
Eso lo tengo que llevar yo por mi lado.
Las BBDD solo hablan de tipos de datos informáticos. Y no hablan de tipos de datos estadísticos.

---

# Hay otro tema importantísimo acerca de cómo guardar los datos.

Las BBDD están optimizadas para actualizaciones DISCRETAS de datos. (Que yo pueda actualizar un dato concreto de una tabla concreta de la BBDD).
Esto es lo que ayer llamamos procesos OLTP (OnLine Transaction Processing).

Los datawarehouses deben estár optimizados para consultas masivas de datos. (Que yo pueda consultar muchos datos de muchas tablas del datawarehouse). Esto es lo que ayer llamamos procesos OLAP (OnLine Analytical Processing).

En el entorno de producción quiero poder poner el expediente 23uy-98/2026 en estado-> FINALIZADO
Esto es una transacción que afecta a un solo expediente. Es una actualización discreta de un dato concreto. OLTP

En un datawarehouse quiero poder consultar el estado de todos los expedientes del año 2026, para montar una gráfica.
Esto necesita datos de muchos expedientes. Es una consulta masiva de datos. OLAP

Esto no se ve solo afectado por el tipo de dato informático. Eso hay que tenerlo en cuenta.. pero hay más.

Hay 2 grandes formas de guardar información de una tabla en un sistema informático:
- Orientada a filas      EXCEL, BBDD relacionales
- Orientada a columnas   DATAWAREHOUSE

> Qué significa esto?

Imaginad que tengo los datos de 4 usuarios de un sistema informático. 
Cada usuario tiene: Id, Nombre, Apellidos, Edad y Email

    ID | Nombre | Apellidos | Edad | Email
    1  | Juan   | Pérez     | 30   | juan.perez@email.com
    2  | María  | García    | 25   | maria.garcia@suempresa.es
    3  | Pedro  | López     | 40   | pedro.lopez@tuempresa.es
    4  | Ana    | Martínez  | 35   | ana.martinez@ella.com

    ESTO DE ARRIBA ES UN FORMATO ORIENTADO A FILAS. Cada fila es un usuario. Cada columna es un atributo del usuario.

    Pero esos datos los puedo guardar en un formato orientado a columnas:

    ID:       1,     2,     3,     4
    Nombre:   Juan,  María, Pedro, Ana
    Apellidos: Pérez, García, López, Martínez
    Edad: 30, 25, 40, 35
    Email: juan.perez@email.com, maria.garcia@suempresa.es, pedro.lopez@tuempresa.es, ana.martinez@ella.com

    ESTO SERIA UN FORMATO ORIENTADO A COLUMNAS.

    En un formato orientado a filas, los datos de un registro se guardan juntos. 
    En un formato orientado a columnas, cada campo se guarda junto con los datos de ese campo de todos los registros.

    Si quiero modificar la edad de Pedro, en un formato orientado a filas, voy a la fila de Pedro y modifico el campo Edad. FACIL!
    Si quiero hacer análitica de datos, querré la edad de todos mis usuarios. Voy a la fila que contiene el campo Edad y leo todos los datos de ese campo. FACIL!
    Si intento hacer esto último en un formato orientado a filas, pongo la aguja del HDD a saltar más que una pulga: Voy a la fila de Juan, leo el campo Edad, voy a la fila de María, leo el campo Edad, voy a la fila de Pedro, leo el campo Edad, voy a la fila de Ana, leo el campo Edad. SALTO DE FILA EN FILA. LENTO!

    El objetivo del sistema de almacenamiento que use marcará la forma de guardar los datos.
    Datawarehouse: OLAP -> Orientado a columnas
    BBDD:          OLTP -> Orientado a filas

    Y esto es muy importante.
    
    En los datalakes, usamos también almacenamientos orientados a columnas. 
    Ahora bien, en los datawarehouse solemos usar aplicaciones especiales (tipo BBDD) para guardar los datos.
    En los datalakes solemos usar sistemas de ficheros. Hay un formato que es el rey: PARQUET.
    PARQUET es un formato orientado a columnas. Y es el que más se usa en el mundo del bigdata para almacenar datos en datalakes.

    Parte del trabajo de la T (transformación) de los procesos ETL es transformar los datos de un formato orientado a filas a un formato orientado a columnas.

Como vemos con estos ejemplos, el tema informático va a tener una gran relevancia en el diseño de un proyecto BI.

Necesitamos ir haciendo por separado el análisis estadístico de los datos y el análisis informático de los datos (qué tipos de datos usaré y qué tipo de formato usaré para el almacenamiento de los datos).

---

# Cómo se guardan los datos en las BBDD y en los datawarehouses. En cuanto a su NORMALIZACION.

    En las BBDD de producción los datos tienden a estar normalizados.
    Qué significa esto?
    Imaginad que tenemos datos de empleados y empresas. Cada empleado trabaja en una empresa. Cada empresa tiene un nombre, una dirección, un CIF, etc. Y cada empleado tiene un nombre, un apellido, un DNI, una fecha de nacimiento, etc.


    Tabla:
        Empleados:
        ID | Nombre | Apellidos | DNI       | FechaNacimiento | EmpresaID
        1  | Juan   | Pérez     | 12345678A | 01/01/1980      | 1
        2  | María  | García    | 87654321B | 02/02/1990      | 2
        3  | Pedro  | López     | 11223344C | 03/03/1985      | 1
        4  | Ana    | Martínez  | 55667788D | 04/04/1995      | 3
    
        Empresas:
        ID | NombreEmpresa | Dirección           | CIF
        1  | Empresa A     | Calle Falsa 123     | A123456
        2  | Empresa B     | Avenida Siempreviva | B654321
        3  | Empresa C     | Plaza Mayor 1       | C987654

    Los datos tendemos a NO REPETIRLOS. Así si un dato hay que modificarlo solo lo modifico en un sitio.
    Decimos que hay una relación entre las tablas. En este caso una relación de 1 a N (una empresa tiene muchos empleados, un empleado trabaja en una empresa).

        EMPRESA     <    EMPLEADOS
        1 empresa        N empleados

    Esta forma está optimizada para procesos OLTP (OnLine Transaction Processing). Para actualizar datos concretos de la BBDD. Para procesos de producción.

    En un Datawarehouse, los datos por contra tienden a estar desnormalizados.
    Qué significa esto?
    Imaginad que tenemos los mismos datos de empleados y empresas.
    Ahora, en lugar de tener 2 tablas, tenemos una sola tabla que contiene todos los datos de empleados y empresas.

    Tabla:
        EmpleadosYEmpresas:
        ID | Nombre | Apellidos | DNI       | FechaNacimiento | EmpresaID | NombreEmpresa | Dirección           | CIF
        1  | Juan   | Pérez     | 12345678A | 01/01/1980      | 1         | Empresa A     | Calle Falsa 123     | A123456
        2  | María  | García    | 87654321B | 02/02/1990      | 2         | Empresa B     | Avenida Siempreviva | B654321
        3  | Pedro  | López     | 11223344C | 03/03/1985      | 1         | Empresa A     | Calle Falsa 123     | A123456
        4  | Ana    | Martínez  | 55667788D | 04/04/1995      | 3         | Empresa C     | Plaza Mayor 1       | C987654

    En esta tabla, aparecen datos repetidos. El nombre de las empresas, su cif, su dirección, etc. aparecen repetidos. Esto es lo que llamamos desnormalización.

    No me da problemas de cara a las actualizaciones. YA QUE EN DATAWAREHOUSE NO HAY ACTUALIZACIONES. SOLO LECTURAS. No es una BBDD de producción. Es un almacén de datos para análisis de datos.

    Pero... es muy eficiente para procesos OLAP (OnLine Analytical Processing). Para consultar muchos datos de muchas tablas del datawarehouse. Si quiero consultar todos los empleados de la empresa cuyo cif es A123456, solo tengo que ir a la tabla de empleados y empresas y filtrar por el cif. 
    Si lo tuviera normalizado, como en una BBDD de producción:
    - primero tengo que ir a la tabla de empresas a buscar el identificador de la empresa cuyo cif es A123456
    - luego tengo que ir a la tabla de empleados a buscar todos los empleados cuyo identificador de empresa es el que he encontrado en la tabla de empresas.
    Esto es mucho más lento que ir a una sola tabla y filtrar por el cif.

    El desnormalizar los datos es una técnica que se usa mucho en los datawarehouses.
    NOTA: En el ejemplo he escrito la tabla del datawarehouse como si estuviera guardada en un formato orientado a filas. En realidad, en un datawarehouse, los datos se guardan en un formato orientado a columnas. Pero para entender el concepto de normalización/desnormalización no es necesario entrar en ese detalle.

    Además, solemos aplicar otras técnicas en los datawarehouses... como por ejemplo el precalcular ciertos datos.

    Por ejemplo:
    Tenemos unos datos guardados de ventas: 
    
        ID| Fecha       | Producto   | Cantidad | Precio | ClienteID
        1 | 01/01/2023  | Producto A | 10       | 5.00   | 1
        2 | 02/01/2023  | Producto B | 5        | 10.00  | 2
        3 | 03/01/2023  | Producto A | 20       | 5.00   | 1
        4 | 04/01/2023  | Producto C | 15       | 20.00  | 3

        Imaginad que un interés especial que tengo es analizar en base al día de la semana. Quiero saber cuántas ventas he tenido en lunes, martes, miércoles, jueves, viernes, sábado y domingo.
        Para ello, en lugar de calcularlo cada vez que hago la consulta, puedo precalcularlo y guardarlo en la tabla de ventas en el datawarehouse:

        ID| Fecha       | Producto   | Cantidad | Precio | ClienteID | DiaSemana | Mes
        1 | 01/01/2023  | Producto A | 10       | 5.00   | 1         | Domingo   | Enero
        2 | 02/01/2023  | Producto B | 5        | 10.00  | 2         | Lunes     | Enero   
        3 | 03/01/2023  | Producto A | 20       | 5.00   | 1         | Martes    | Enero
        4 | 04/01/2023  | Producto C | 15       | 20.00  | 3         | Miércoles | Enero

    Todo esto forma parte de la T (transformación) de los procesos ETL. Transformar los datos para que ocupen menos espacio y se procesen más rápido.

Cuando ayer hablaba de COCINAR los datos, me refería a todo esto. 
A transformar los datos para que ocupen menos espacio y se procesen más rápido para los tipos concretos de análisis que quiero hacer.
Y empieza a coger sentido el concepto de DATAWAREHOUSE.
Decía que es un almacén de datos para un uso concreto... para un tipo de análisis concreto.
Si en mi análisis voy a buscar mucho o a agregar mucho por el día de la semana, me interesa precalcularlo y guardarlo en la tabla de ventas. Si no me interesa, no lo hago.

Y quizás para otro tipo de análisis no me interesa. Y no lo tengo calculado.

No penseís que por tener un ordenador/computadora las cosas van a ir rápido. No. 
Vamos a manejar GRANDES VOLUMENES DE DATOS.
Hay que hacer un trabajo de optimización de los datos para que el análisis sea rápido y barato.

Dentro de un proyecto de BI, la parte de preparación de los datos es CRITICA... y es la que más tiempo y dinero se lleva.
El hacer cuadros de mando, definir gráficas, generar indicadores... Eso es barato y rápido.
Lo que es caro y lo que se consume el tiempo es la preparación de los datos. La parte de ETL.

> Y aquí en paralelo con todo esto, nos sale otro concepto: CALIDAD DE LOS DATOS.

Muchos proyectos fracasan por la mala calidad de los datos.
Parte del trabajo de preparación de los datos es mejorar la calidad de los datos: limpiar los datos, depurarlos, corregirlos, etc.

En el año 2001 me toco un proyecto con un gran call-center.
Habían hecho encuestas por teléfono a un huevo de clientes.
Tenían 2.000.000 encuestas. Y las habían guardado en una BBDD.
Pero los datos los habían anotado los operadores a mano.
"Castilla la Mancha" aparecía de 52 formas diferentes escrito!
"castilla la mancha"
"Castilla-La Mancha"
"Castilla La Mancha"
"C.Mancha"
...
Y así 52.

Evidentemente con esto NO HACEMOS NADA! Esos datos es imprescindible limpiarlos y depurarlos. Y eso es parte de la preparación de los datos.
Eso de nuevo entra en la T (transformación) de los procesos ETL. Transformar los datos para que ocupen menos espacio y se procesen más rápido.

Siempre voy a tener que dedicar tiempo a asegurar la calidad de los datos. 
Como no lo haga bien, el proyecto de BI va a fracasar. Y eso es un hecho.

Aquí el tema puede ser más complejo:

> Estadística : VALORES PERDIDOS

    Tabla PERSONAS y MASCOTAS

        Nombre Persona | Número de mascotas que tiene  | Tipo de la primera mascota
        Juan           | 2                             | Perro
        María          | 0                             | 
        Menchu         | 1                             | 
        Federico       | 3                             | Perro
        Luciano        |                               |

En esa tabla cuantos dato desconozco? 3 datos desconocidos.
María -> Tipo de la primera mascota -> No tiene mascotas. No es un dato desconocido. Es un dato conocido: No tiene mascotas.
Los otros que si desconozco son:
- Menchu -> Tipo de la primera mascota -> No sé qué tipo de mascota tiene. 
- Luciano -> Número de mascotas que tiene -> No sé cuántas mascotas tiene.
Y esos datos ME GENERAN INCERTIDUMBRE en los análisis. 
El dato que "falta" de María NO genera incertidumbre.

Muchas veces, como parte del proceso de preparación de los datos, separo los datos que faltan (vacíos) en:
- No informado
- No aplica

        Nombre Persona | Número de mascotas que tiene  | Tipo de la primera mascota
        Juan           | 2                             | Perro
        María          | 0                             | No aplica
        Menchu         | 1                             | No informado
        Federico       | 3                             | Perro
        Luciano        | No informado                  | No informado

        Cuántas familias tienen mascotas? Entre el 60% y el 80% de las familias tienen mascotas. INCERTIDUMBRE
        Cuántas familias tienen perro?    Entre el 40% y el 80% de las familias tienen perro.    INCERTIDUMBRE
        Cuántas de las familias con mascota tienen perro? Entre el 50% y el 100% de las familias con mascota tienen perro. INCERTIDUMBRE

El estudio de valores perdidos es imprescindible hacerlo en la fase de preparación de los datos.

Todos estos estudios y preparaciones de los datos, suelen requerir mucha intervención humana. Y eso es caro. Y eso es lo que hace que los proyectos de BI sean caros y lentos. Depende de la calidad de origen de los datos el proyecto puede ser más sencillo o extraordinariamente complejo. Y eso es algo que no podemos controlar. Nos lo encontramos y tenemos que adaptarnos a ello.

ES ALGO QUE TENGO QUE ANALIZAR A PRIORI, antes de comprometerme y de dar fechas y presupuesto.


---



# El espacio en disco es caro o barato?

Los HDD son de las cosas más caras que hay en un entorno empresarial.
Y muchas veces, si no estamos metidos en este mundo es algo que no entendemos.

Para casa, puedo ir al Mediamarkt (no soy tonto!) y comprar un disco de 2Tbs por 100€.
Pues no es tanto. Ahñi entran la hueva de fotos y peliculas.

En la empresa esta cuenta no sale. Por muchos motivos. 
1. En la empresa con 2Tbs ni empiezo... La empresa genera muchos datos... Pero olvidando esto....
2. Ese disco que he comprado en casa es de baja calidad. 
   Esta construido con materiales que se degradan más rápido con cada lectura/escritura.
   Claro que en casa escribo la foto 1 vez... y la leo a los 6 meses (2 veces al año) Casi ni lo uso.
   En la empresa necesito un HDD que se degrade más despacio... y eso implica componentes químicos de más calidad y más caros. x2 del precio
   En realidad esa cifra puede subir en un disco bueno hasta un x10. 
3. En la empresa, en el entorno de producción cada dato se guarda al menos en 3HDD distintos.. por si se estropea uno, que no pierda acceso al dato. Eso implica x3 del precio. -> ALTA DISPONIBILIDAD (si un disco se rompe, sigo con acceso al dato)
4. Y luego están las copias de seguridad... que no es lo mismo -> RECUPERACION DE DESASTRES 
  Llega un degenerado y borra sin querer (por descuido) una tabla de una BBDD... ese borrado SE REPLICA a los 3 HDD de producción. Y si no tengo copias de seguridad, he perdido el dato para siempre.
  Y hago backups diarios de mis datos. Y los guardo al menos por duplicado... y de un dato tengo copias de al menos 2 semanas atrás.

Poniendo los factores uno a uno, y siendo generosos:

    100€    precio doméstico de 2Tbs
    x 10    disco de calidad empresarial, pensado para escritura continua
    x 3     tres copias en producción            -> ALTA DISPONIBILIDAD
    x 2     backups duplicados                   -> RECUPERACION DE DESASTRES
    x 5     cabina, controladoras, red, licencias, mantenimiento, sala, energía
    -------
    30.000€ por 2Tbs de almacenamiento en la empresa

(La cifra es orientativa y del lado alto: sirve para el orden de magnitud, que es
lo que importa aquí. Lo relevante es que NO son 100€.)

En casa 2Tbs me cuestan 100€... 
En la empresa 2Tbs me cuestan 30.000€.

Es una locura!
