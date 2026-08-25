
# Encuestas de satisfacción de clientes
## Columnas que tenemos en el conjunto de datos:

COLUMNA             | Tipo estadístico          | Tipo informático                 | Tratamiento
--------------------|---------------------------|----------------------------------|-----------------------
id                  | NOMINAL                   | ID                               |
dni                 | Cualitativa nominal       | ID (Texto)                       |
comunidad           | Cualitativa nominal       | Entero 1 byte (256 valores)      | Texto -> Codificación
edad                | Cuantitativa              | Entero 1 byte (256 valores)      | 
sexo                | Cualitativa nominal       | Boolean 1 bit                    | Texto -> Codificación
producto_contratado | Cualitativa nominal       | OPCION 1:
                                                    Entero 1 byte (256 valores)      | Texto -> Codificación
                                                  OPCION 2:
                                                    Boolean en columnas separadas por producto      
                                                    Para representarlo cómodamente puede interesar en este caso un PIVOTE!
                                                    Dado que hay pocos productos, podemos ponerlos en columnas:
                                                    Cliente  .... FIBRA, TELEFONO, TV
                                                    Menchu   .... TRUE   FALSE     TRUE
fecha_llamada       | Cuantitativa              | Fecha (Entero 4 bytes)           |
hora_llamada        | NOMINAL                   | Hora (Entero 4 bytes)            |
    Si la hora se junta con la fecha -> CUANTITATIVO


importe_ultima_factura      | Cuantitativa             | Decimal 4 bytes (hasta 7 dígitos significativos) |
satisfaccion_1_10           | Ordinal                  | Entero 1 byte (256 valores)      |
recomendaria                | Nominal                  | Boolean 1 bit                    | Texto -> Codificación
num_incidencias_previas     | Cuantitativa             | Entero 1 byte (256 valores)      |
tipo_primera_incidencia     | Nominal                  | Entero 1 byte (256 valores)      | Texto -> Codificación
duracion_llamada_seg        | Cuantitativa             | Entero 2 bytes (65536 valores)   |
operador                    | Nominal                  | Entero 1 byte (256 valores)      | Texto -> Codificación

    Si la hora la analizo por separado -> RARO! / ARTIFICIAL. NOMINAL! 
                                            En según qué caso, puedo tratarlo como ORDINAL

        Ejemplos y explicaciones adicionales:
            FECHAS:         CUANTITATIVOS. Se miden en DIAS!
                            Lo que pasa es que agrupamos esos días en meses y años, para entenderlos mejor.
                                1 año son 265(366) días!
                                1 mes son 28, 29, 30 o 31 días.
                                Y usamos como referencia el momento en el que nació JC: Año 0
            10-10-2025
                            Esta fecha: 
                                2025 * 365,25 = 739.631,25 dias
                                Mes 10: Enero 31 + Febrero 28 + Marzo 31 + Abril 30 + Mayo 31 + Junio 30 + Julio 31 + Agosto 31 + Septiembre 30 = 243 días
                                Día 10: 10 días
                            Total: 739.631,25 + 243 + 10 = 739.884 días desde el nacimiento de JC
                            Claro... llevar las fechas así me volvería loco.

            Como dato cuantitativo que es, medible en días
            PODEMOS SUMAR DIAS
            PODEMOS RESTAR DIAS
            NO TIENE SENTIDO LO DE MULTIPLICAR O DIVIDIR (ya que el cero no es absoluto, sino relativo al nacimiento de JC)

            De una fecha, puedo sacar:
                - DIA   -> NOMINAL (en algunos casos lo puedo considerar ORDINAL)
                - MES   -> NOMINAL (en algunos casos lo puedo considerar ORDINAL)
                - AÑO   -> El año sigue siendo un dato CUANTITATIVO (lo puedo seguir midiendo en días desde el nacimiento de JC o en AÑOS desde el nacimiento de JC)... simplemente tiene menos precisión que la fecha completa. Igual que si un número le quito decimales.

                Igual que con el día y mes, pasa con la hora, que a su vez es divisible (Agrupable) en HORAS, MINUTOS, SEGUNDOS...

                    - Imaginad que solo tengo MESES, O DIA DE LA SEMANA, O HORA
                     
                      - Encuesta 1              ENERO           SABADO      10:00
                      - Encuesta 2              MARZO           DOMINGO     14:00
                      - Encuesta 3              ENERO           SABADO      00:00
                      - ...
                      - Encuesta 100            SEPTIEMBRE      MARTES      13:00

                Miramos los valores de MES: ENERO, FEBRERO, MARZO, ABRIL, MAYO, JUNIO, JULIO, AGOSTO, SEPTIEMBRE, OCTUBRE, NOVIEMBRE, DICIEMBRE
                Miramos los valores de DIA DE LA SEMANA: LUNES, MARTES, MIÉRCOLES, JUEVES, VIERNES, SÁBADO, DOMINGO
                Miramos los valores de HORA: 00:00, 01:00, 02:00, 03:00, 04:00, 05:00, 06:00, 07:00, 08:00, 09:00, 10:00, 11:00, 12:00, 13:00, 14:00, 15:00, 16:00, 17:00, 18:00, 19:00, 20:00, 21:00, 22:00, 23:00

                Desde el punto de vista estadístico, que tipo de valores son?
                - Cuantitativos no son... y es que no son NI ORDINALES.
                - Qué va antes, ENERO o DICIEMBRE? NO LO SE. Depende del año. Analizando solo por mes (sin año) no puedo saberlo. NO HAY ORDEN. -> NOMINAL
                - Qué va antes, LUNES o DOMINGO? NO LO SE. Depende de la semana que hablemos. NO HAY ORDEN. -> NOMINAL
                - Qué va antes, 10:00 o 14:00? 
                    - Si solo tengo la hora, sin fecha, no puedo saberlo. NO HAY ORDEN. -> NOMINAL
                    No sé si las 00:00 va antes o después de las 23:00. Depende del día. NO HAY ORDEN. -> NOMINAL

                Podré hacer estudios del tipo...
                    - La gente se muestra más satisfecha en ENERO o DICIEMBRE (TABLA de frecuencias/Contingencia)
                    - Los LUNES la gente se muestra MAS INSATISFECHA que los DOMINGOS (TABLA de frecuencias/Contingencia)
                    - LOS FINES DE SEMANA la gente se muestra más satisfecha que entre semana (TABLA de frecuencias/Contingencia)
                Lo que no puedo decir es:
                    - Según pasan los meses, la gente tiende a estar más (o menos) satisfecha (esto sería usar la capacidad ORDINAL de la variable, si la tuviera, y no la tiene)
                    - Según pasa la semana, la gente tiende a estar más (o menos) satisfecha (esto sería usar la capacidad ORDINAL de la variable, si la tuviera, y no la tiene). Esto implicaría tener datos de la misma persona, medidos en diferentes momentos de tiempo, y no es el caso.







# Resumen de las variables:

COLUMNA                      | Tipo estadístico   
-----------------------------|--------------------
id                           | Cuantitativa       
dni                          | Cualitativa nominal
comunidad                    | Cualitativa nominal
edad                         | Cuantitativa       
sexo                         | Cualitativa nominal
producto_contratado          | Cualitativa nominal
fecha_llamada                | Cuantitativa       
hora_llamada                 | NOMINAL            
importe_ultima_factura       | Cuantitativa 
satisfaccion_1_10            | Ordinal      
recomendaria                 | Nominal      
num_incidencias_previas      | Cuantitativa 
tipo_primera_incidencia      | Nominal      
duracion_llamada_seg         | Cuantitativa 
operador                     | Nominal      

# Estudios:
    - Qué estudios podemos hacer con estas variables?
    - Si le pueden interesar a alguien?

    Cuando entienda qué información puedo sacar de estos datos, la agruparé por perfiles / nivel / roles: OPERACIONAL, TACTICO, ESTRATÉGICO.

    Y plantearé distintos cuadros de mando.

> duracion_llamada_seg de una encuesta de satisfacción.
    Media de las duraciones
        - Esto es algo que puedo calcular? Si por ser un dato CUANTITATIVO.
        - Le interesa este valor (MEDIA DE LAS DURACIONES) al directivo de la empresa? NADA
          Lo que le interesa es si la gente está satisfecha o no.
          Conocer acerca del grado de satisfacción de los clientes. 
        - Le puede interesar a otro perfil?
          - Operador? Posiblemente tampoco
          - Gerente de los operadores/callcenter SI LE INTERESA:
            - Conociendo ese dato, puede saber si:
              - Es capaz de atender un determinado número de llamadas o no dependiendo de la gente que tiene.
              - Si necesita más gente (o menos) para poder hacer una determinada campaña de encuestas en un periodo de tiempo.
          - De hecho, a nuestro GERENTE posiblemente el dato QUE NO LE INTERESA ES: si la gente está satisfecha o no

---

# Análisis UNIVARIABLE

id                           | NOMINAL              | 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
                RECUENTO / FRECUENCIA: 1987 <-    Número de encuestas realizadas

            id          frecuencias:
            1                   1
            2                   1               TABLA DE FECUENCIAS
            3                   1               En este caso es absurda. Todos tiene frecuencia 1
            4                   1               Pero si me da el número de valores distintos:
            ...                                 Cuántas filas hay. -> RECUENTO
            1897                1

                                                La estadística me dice también que esa tabla
                                                la puedo representar como gráfico de barras o sectores: 
                                                EN ESTE CASO ABSURDO!

dni                          | Cualitativa nominal  | 12345678Z
                RECUENTO / FRECUENCIA: 1987 <-    Número de clientes distintos que han respondido a la encuesta
                                                    de los que han informado de su DNI. INCERTIBUMBRE?

            dni         frecuencias:
            12345678Z           1
            23456789A           1               TABLA DE FECUENCIAS
            34567890B           2               En este caso es absurda. Todos tiene frecuencia 1 ( o 2)
            45678901C           1               
            ""                187
                                                La estadística me dice también que esa tabla
                                                la puedo representar como gráfico de barras o sectores: 
                                                EN ESTE CASO ABSURDO!

                En este caso, el recuento me interesa, pero también me interesa el recuento distintivo:

                Cuántas encuestas tengo con DNI informado       
                Cuántas encuentas tengo con DNI NO informado
                A cuántos clientes distintos he hecho encuesta

comunidad                    | Cualitativa nominal  | Madrid, Cataluña, Andalucía,..

        Puedo hacer de nuevo la tabla de frecuencias y el gráfico de barras o sectores.

            comunidad       frecuencias:
            Madrid              500
            Cataluña            400         Cómo la represento gráficamente? Con un gráfico de barras o sectores.
            Andalucía           300         En un caso como este (con 17+2 comunidades autónomas) posiblemente 
                                            me interese más un gráfico de barras que de sectores.
            ...                                 Si tengo más de 10 valores distintos NO PLANTEO UN SECTORES
                                                (a no ser que agrupe: OTROS) Esto de agrupar tiene sentido si OTROS < 5%

            Le interesa a alguien?
                DIRECCION? No mucho
                OPERACIONES? SI... saber cómo vamos . Posiblemente comparado con un valor objetivo
                             O al menos saber que estoy trabajando en todos los frentes
                Esta encuesta la habrá planteado MARKETING, y le puede interesar también para VALIDAR/VERIFICAR si 
                la muestra es representativa de la población de clientes que tenemos.


    LUEGO en un rato juntaremos variables 2 a 2:
        COMUNIDAD X NIVEL DE SATISFACCION
        NOMINAL   X ORDINAL

            Tabla de contingencia

                            SATISFACCIONES
            COMUNIDADES             1   2   3   4   5   6   7   8   9   10
                Madrid             47  37  57  89  ...
                Cataluña           12  34  56  78  ... 
                Andalucía          23  45  67  89  ...

                Esta tabla sería la que haríamos si trato las 2 variables como NOMINALES.
                Y me puede servir para identificar casillas ANOMALAS!
                Si en una COMUNIDAD hay mucha más o menos gente satisfecha de lo esperado.
                Es raro.. En Madrid no hay gente que haya dado el valor 4.
            
            En general en este caso, este estrudio me vale para poco.
        
            Pero hay yun segundo estudio que podemos hacer aquí:
            Comparación de MEDIANAS, haciendo uso de la capacidad ORDINAL de la variable NIVEL DE SATISFACCION.

                                
            COMUNIDADES         SATISFACCION MEDIANA
            Madrid              7
            Cataluña            6
            Andalucía           8

            La gente de Andalucía tiende a estar más satisfecha que la de Cataluña, y que la de Madrid.
            La gente de Madrid tiende a estar más satisfecha que la de Cataluña, y menos que la de Andalucía.
            Puedo ordenar por nivel de satisfacción mediano, y ver en qué comunidades tengo más asatisfechos a mis clientes.

                Andalucia:  8
                Madrid:     7       Lo puedo representar como un gráfico de BARRAS / BOXPLOT MULTIPLE
                Cataluña:   6

            Y esta INFORMACION le puede interesar mucho a DIRECCIÓN!
            A operaciones de nuevo, esta INFORMACION no le aporta nada. Lo que le interesa a operaciones es saber si estamos cumpliendo con el objetivo de encuestas.

edad                         | Cuantitativa

    Tabla de frecuencias? Si acaso trabajando en INTERVALOS:

        Rango de edades: 18-25, 26-35, 36-45, 46-55, 56-65, 66-75, 76-85, 86-95, 96-105 

        Rango                FRECUENCIA    FRECUENCIA RELATIVA
        18-25                   100             5%
        26-35                   200             10%
        36-45                   300             15%
        46-55                   400             20%
        56-65                   300             15%
        66-75                   200             10%
        > 75                    300             15%

        Representación GRAFICA: HISTOGRAMA!

        Podríamos calcular adicionalmente: 
            - MEDIA     DESV. TIPICA
            - MEDIANA   RIC
            - Medidas de posición: 1er cuartil, 3er cuartil, percentiles 10, 90, etc
        Estamos hablando de encuestas de satisfacción a clientes.
        Posiblemente la edad media/mediana, sus percentiles... NO LE INTERESAN A NADIE!  
        Otra cosa es que fueran los datos de mis clientes... y quiero perfilar a mis clientes

        En cambio, la tabla de arriba si puede interesar... para asegurar que estoy tratando con una variedad de tipos de clientes.
        Sería al contrarío. YA TENGO POR OTRA VIA PERFILADOS A MIS CLIENTES. LO QUE QUIERO ES ASEGURARME DE QUE ESTOY HACIENDO LA ENCUESTA A UNA MUESTRA REPRESENTATIVA DE MIS CLIENTES.    
            PARA ESA COMPARACION, ME INTERESA: FRECUENCIA RELATIVA, no la absoluta.
                Quiero asegurarme que tengo en la muestra la misma proporción de clientes de cada rango de edad que en la población de clientes que tengo.


sexo                         | Cualitativa nominal

    Pasa exactamente igual que con las anteriores... Especialmente es igual que la de comunidad.

                        Frecuencias
        Hombre              50%
        Mujer               45%
        No informado        3% <--- INCERTIDUMBRE \
        Inválido            2% <--- INCERTIDUMBRE / Le sirve a quién? OPERACIONES / TACTICO
                                                    A dirección lo que le tengo que contar es SI HAY CONFIANZA SUFICIENTE O NO EN LOS RESULTADOS.
                                                    Si tengo DEMASIADA INCERTIDUMBRE O SUFICIENTE COMO PARA PODER TRAGARME LOS RESULTADOS (EXTRAPOLARLOS A LA POBLACION DE CLIENTES) O NO.
        LA diferencia en este caso es el gráfico: 
        Optaríamos claramente por un gráfico de SECTORES Y NO DE BARRAS.


producto_contratado          | Cualitativa nominal
*fecha_llamada               | Cuantitativa       
hora_llamada                 | NOMINAL            
importe_ultima_factura       | Cuantitativa 
*satisfaccion_1_10           | Ordinal      

    Puedo calcular la tabla de frecuencias y representarla gráficamente -> Barras

        Satisfacción        Frecuencia
        ------------------  ----------
        1                   10
        2                   20              -> BARRAS
        3                   30
        4                   40
        5                   30
        6                   47
        7                   22
        8                   30
        9                   15
        10                  10    

    Estadísticos:
        - Media     NO PUEDO CONCEPTUALMENTE    No es cuantitativa... por lo tanto 
                                                sus valores no so sumables entre si. 
                    SI MATEMATICAMENTE          5.46

                    Mi nivel de satisfacción 4 no es igual a tu nivel de satisfacción 4.
                    Quizás mi nivel de satisfacción 4 es más parecido a tu nivel de satisfacción 6.
                    Como no hay unidad de medida, conceptualmente es un error sumar o restar valores ordinales.

                    En muchos casos se hace... Especialmente en este tipo de escalas (0-10) BUENO.
                    Lo que puedo calcular es la MEDIANA!
        - Mediana   Por ser una variable ORDINAL. <- ME dice GRANDES RASGOS, COMO DE SATISFECHOS ESTAN LA MAYOR PARTE DE LOS CLIENTES. GROSO MODO
        - Cuartiles -> RIC      \
        - Percentiles           / ME DICE GROSO MODO, si los cliente stienen a desviarse mucho de esa tendencia general que he identificado: MEDIANA.

                Mediana 7
                P90: 8
                P10:6

                Joder! que guay!
                El 80% de mis clientes están entre 6 y 8 de satisfacción.
                Y la mayor parte de ellos en torno a 7.

                La mediana (la mayor parte de mis cliente) está en torno a 7
                Pero el P10 es 1 y el P90 es 10. Hay clientes que están muy insatisfechos y otros que están muy satisfechos. -> TENGO DE TODO... La tendencia central es 7, pero hay de todo!
                HAY GENTE MUY INSATISFECHA Y GENTE MUY SATISFECHA. LA MEDIANA NO ME DICE NADA DE ESO. SI QUIERO SABER ESO, TENGO QUE MIRAR LOS PERCENTILES.


      Satisfacción MEDIANA! <- ESTO LE INTERESA A DIRECCION! (y a operaciones no le interesa nada) 

    Esta misma pregunta la podrían haber hecho: de 0 a 5
    INDIQUE SU GRADO DE SATISFACCION DE 1 a 5... donde  1 es MUY INSATISFECHO y 5 es MUY SATISFECHO
    De hecho, podrían haber escrito directamente:
    RODEE CON LAPICERITO SU GRADO DE SATISFECCION:
        MUY INSATISFECHO    INSATISFECHO    NEUTRAL    SATISFECHO    MUY SATISFECHO
    
    Qué media hago ahi? Cómo sumo INSATISFECHO + NEUTRAL + SATISFECHO?
    Qué más da, que hayan escrito MUY INSATISFECHO que 1
    O que hayan escrito INSATISFECHO que 2
    Son simples etiquetas, para un "NIVEL" .
    NIVEL/RANGO/GRADO no es CUANTITATIVO. Y por ende no puedo calcular la media. Puedo calcular la mediana, y los cuartiles, percentiles, etc.

recomendaria                 | Nominal      
num_incidencias_previas      | Cuantitativa 
tipo_primera_incidencia      | Nominal      
*duracion_llamada_seg        | Cuantitativa 

    MEDIA           -> Me ayuda a planificarme (cuántas llamadas puedo realizar. Cuánta gente necesito.)
    MEDIANA         -> Me da una información más realista de la probabilidad que dure una llamada.
    PERCENTILES     -> ME ayudan a informar a un cliente del tiempo máximo (o mínimo) previsto NORMALMENTE (90% de las llamadas duran menos de 15 minutos, 10% duran más de 15 minutos, etc)

operador                     | Nominal      


---
RESULTADOS DEL UNIVARIABLE:
Número de encuestas realizadas:             Dirección (CLARO QUE SI)
                                                        Otra cosa es cómo se lo presente
                                                        Pero ese dato guarda relación con 1 cosa IMPORTANTISIMA:
                                                        Qué confianza tengo en los resultados de la encuesta?

                                                Los clientes están encantados: LA MEDIANA DE SATISFACCION ES 9 sobre 10. GUAU!!!!! VAMOS GENIAL!!! SOMOS LA MEJOR EMPRESA
                                                A cuántos hemos preguntado...? A 10 del 1.000.000 de clientes que tenemos.  MUY POCA CONFIANZA EN LA ENCUESTA
                                                A cuántos hemos preguntado...? Al 20% de los cliente            
                                                          MUCHISIMA CONFIANZA EN LA ENCUESTA

                                                Este dato le va a interesar A TODO EL MUNDO

                                            Operaciones (CLARO QUE LE INTERESA)
                                                - No por la confianza en el resultado de la encuesta
                                                - Para saber si hemos acabado, si hemos llegado a objetivo
                                                - Llevamos 500 de 2700 planificadas.. y hemos consumido el 50% del tiempo.
                                                - ESTAMOS JODIDOS!
                                                - Aquí posiblemente un indicador gráfico de tipo gauge me interese!

Cuántas encuestas tengo con DNI informado   ~ Número de encuestas realizadas    
Cuántas encuentas tengo con DNI NO informado    --> CALIDAD DE DATOS BAJA Operaciones | Táctico

A cuántos clientes distintos he hecho encuesta  --> Dirección (CONFIANZA EN LA ENCUESTA ~ Número de encuestas realizadas)
    Lo ideal es que estos 2 datos COINCIDAN!

---

# BIVARIABLE


COLUMNA                      | Tipo estadístico   
-----------------------------|--------------------
id                           | Cuantitativa       
dni                          | Cualitativa nominal

comunidad                    | Cualitativa nominal
sexo                         | Cualitativa nominal
producto_contratado          | Cualitativa nominal
hora_llamada                 | Cualitativa nominal
recomendaria                 | Cualitativa nominal
tipo_primera_incidencia      | Cualitativa nominal
operador                     | Cualitativa nominal

satisfaccion_1_10            | Ordinal      

fecha_llamada                | Cuantitativa       
edad                         | Cuantitativa       
importe_ultima_factura       | Cuantitativa 
num_incidencias_previas      | Cuantitativa 
duracion_llamada_seg         | Cuantitativa 

Tenemos 13 variables reales de estudio:
7 Nominales, 1 Ordinal y 5 Cuantitativas

Si quiero combinar todas ellas 2 a 2, tengo 13*12/2 = 78 combinaciones posibles.

Que debería analizar 1 a 1.
Esto es una locura. Si hago eso, me paso semanas analizando datos, y no me da tiempo a sacar conclusiones ni a plantear cuadros de mando.

Ahí lo ideal es aplicar 2 criterios de selección:
- Contar con un experto de negocio. Es quiñen me puede ayudar a decidir cuales de esas combinaciones son más relevantes para el negocio. Y por tanto, cuales son las que me interesa analizar primero.
- Aplicar ciertos criterios estadísticos para descartar combinaciones que no aporten información relevante.
  Nosotros querremos medir ciertas cosas o tomar decisiones sobre ciertas cosas...
  Esas variables SON LAS QUE QUIERO CRUZAR! 
- Hay datos que yo no controlo. VARIABLES INDEPENDIENTES. Por ejemplo, la edad de mis clientes, su comunidad autónoma, su sexo, el producto que han contratado, el importe de su última factura, el número de incidencias previas que han tenido.
 En general, en primera instancia, esas variables NO LAS CRUZO ENTRE SI.
 Tomaré 1 variabel que no controlo, y una variable que me interese para la toma de alguna decisión de negocio. 


QUE CONTROLO: Sobre las que puedo influir/TOMAR DECISIONES

fecha_llamada                | Cuantitativa       
hora_llamada                 | Cualitativa nominal

recomendaria                 | Cualitativa nominal
operador                     | Cualitativa nominal
satisfaccion_1_10            | Ordinal      
duracion_llamada_seg         | Cuantitativa 


QUE NO CONTROLO: Sobre las que no puedo influir/TOMAR DECISIONES

comunidad                    | Cualitativa nominal
sexo                         | Cualitativa nominal
producto_contratado          | Cualitativa nominal
tipo_primera_incidencia      | Cualitativa nominal

edad                         | Cuantitativa       
importe_ultima_factura       | Cuantitativa 
num_incidencias_previas      | Cuantitativa 

Aquí hay 7 variables que no controlo: 7x6 = 42 combinaciones posibles que ya no voy a hacer de las 78 posibles. Me quedan 36 combinaciones posibles.

Con una primera pasada, hemos quitado más de la mitad de las combinaciones posibles.
Por otro lado, muchas de las varables de estudio, de las que controlo o sobre las que quiero influir, tampoco tiene sentido cruzarlas... No dependen unas de otras. Pero muchas otras combinaciones SI QUE LAS HARE.



recomendaria x satisfaccion_1_10
    Si estas variables están muy relacionadas, puede ser que no me interese obtener ambas.
        Recomiendan los que tienen satisfacción > 7
        Para qué voy a preguntar si recomiendan o no, si ya sé que los que tienen satisfacción > 7 recomiendan y los que tienen satisfacción < 7 no recomiendan.
    Si veo que no están relacionados, seguiré preguntando ambos.

recomendaria x operador
operador x satisfaccion_1_10
    Quiero ver si Distintos operadores tienen distintos niveles de satisfacción de sus clientes o si todos consiguen que el cliente recomiende por igual.
    Hay gente que está haciendo un gran trabajo.

recomendaria x duracion_llamada_seg
    Si la duración (tiempo que invierto en la persona) tiene que ver con que el cliente recomiende o no.
    NO DEBERIA! Pero deberlo en consideración a la hora de tomar conclusiones. 

    A DIRECCION LE DA IGUAL
    LE INTERESAN A UN ANALISTA DE DATOS, PARA PODER RECALIBRAR LA MEDICION DE RECOMENDARIA QUE HAGO, TENIENDO EN CUENTA EL TIEMPO QUE HE INVERTIDO EN CADA LLAMADA.

recomendaria x fecha_llamada

    Las fechas son especiales en BI... estas siempre!
    DIRECCION: Si cambia la sensación de recomendarnos o no a lo largo del tiempo.


recomendaria x hora_llamada
    Si me interesa hacer las llamadas en un determinado momento del día o de la semana, para que el cliente esté más receptivo.
    O lo contrario, identificar si a ciertas horas, el cliente está menos receptivo y los datos que obtengo de esas llamadas no son representativos.

    A DIRECCION LE DA IGUAL    
    LE INTERESAN A UN ANALISTA DE DATOS, PARA PODER RECALIBRAR LA MEDICION DE SATISFACCION QUE HAGO, TENIENDO EN CUENTA LA HORA EN LA QUE HE HECHO CADA LLAMADA.


operador x duracion_llamada_seg

satisfaccion_1_10 x duracion_llamada_seg
    A DIRECCION LE DA IGUAL
    LE INTERESAN A UN ANALISTA DE DATOS, PARA PODER RECALIBRAR LA MEDICION DE SATISFACCION QUE HAGO, TENIENDO EN CUENTA EL TIEMPO QUE HE INVERTIDO EN CADA LLAMADA.
satisfaccion_1_10 x fecha_llamada
satisfaccion_1_10 x hora_llamada

satisfaccion_1_10 x fecha_llamada

     DIRECCION: Si cambia el nivel de satisfacción de los clientes a lo largo del tiempo, me interesa saberlo.



satisfaccion_1_10 x hora_llamada

---

A nivel de BI, los cruces que si quiero hacer siempre son los de VARIABLES QUE CONTROLO x VARIABLES QUE NO CONTROLO.



fecha_llamada                | Cuantitativa             \
hora_llamada                 | Cualitativa nominal      / Podrían actuar como filtro.. O en un análisis de series temporales... Para ver si las tendencias de lo que sea que me interese cambian a lo largo del tiempo.


recomendaria                 | Cualitativa nominal
    comunidad                    | Cualitativa nominal
    sexo                         | Cualitativa nominal
    producto_contratado          | Cualitativa nominal
    tipo_primera_incidencia      | Cualitativa nominal
    edad                         | Cuantitativa       
    importe_ultima_factura       | Cuantitativa 
    num_incidencias_previas      | Cuantitativa 

    DIRECCION: Me interesan TODOS

satisfaccion_1_10            | Ordinal      
    comunidad                    | Cualitativa nominal
    sexo                         | Cualitativa nominal
    producto_contratado          | Cualitativa nominal
    tipo_primera_incidencia      | Cualitativa nominal
    edad                         | Cuantitativa       
    importe_ultima_factura       | Cuantitativa 
    num_incidencias_previas      | Cuantitativa 

duracion_llamada_seg         | Cuantitativa 
    comunidad                    | Cualitativa nominal
    sexo                         | Cualitativa nominal
    producto_contratado          | Cualitativa nominal
    tipo_primera_incidencia      | Cualitativa nominal
    edad                         | Cuantitativa       
    importe_ultima_factura       | Cuantitativa 
    num_incidencias_previas      | Cuantitativa 


Al final, me quedaré posiblemente con 20-25 combinaciones para estudio.
Solo estamos haciendo un cribado inicial, para descartar combinaciones que no aporten información relevante.
Ahora nos pondremos a hacer el análisis.

Y estamos juntando variables 2 a 2... Imaginad que junto variables 4 a 4: 13x12x11x10 = 17160 combinaciones posibles. Esto es una locura. Yo como ser humano no puedo enfrentarme a eso. Esto es terreno del data mining.
---

recomendaria  x comunidad               <- Cualitativa nominal x nominal

    Puedo solamente hacer TABLA DE CONTINGENCIA y su potencial representación sería: Barras agrupadas o apiladas.

                              recomendaria  
    comunidades                si  / no
            Madrid              100   200
            Cataluña            300   400
            Andalucía           500   600
            ...

    Qué puedo sacar de aqui: Si en ciertas comunidades la gente recomienda más que en otras. 
    Solo eso. No sé por qué? La estadística solo me ayuda a poner ahí el foco.
    Esto es lo mismo que cuando hicimos el ejemplo de sexo x lugar(bus, tren, medico)
        Hay un 80% de la gente sé que me recomienda... espero ese % en todas las comunidades. Si no es así, me interesa saberlo, puede ser que en Madrid suba al 100% y en Cataluña baje al 50%.
        Entonces, sabiéndo eso : Miro a ver qué se está haciendo en Madrid bien... para implantarlo en Cataluña.

    No hay otra cosa que pueda hacer. Una tabla si acaso comparando los valores esperados con los valores observados. 


nivel de satisfacción x comunidad       <- Ordinal x nominal

    En este caso, puedo:
    - Usar la capacidad NOMINAL de la variable ORDINAL
      Igual al de arriba: Misma tabla de contingencia y mismos gráficos. Y misma interpretación.

    - Usar la capacidad ORDINAL de la variable ORDINAL
        Cuando tengo una variable al menos ORDINAL (Es decir también aplica a las cuantitativas), puedo comenzar a hablar de TENDENCIAS. 
        En este caso, puedo hacer un estudio de comparación de MEDIANAS.
        Al ser la variable nivel de satisfacción ORDINAL, puedo calcularle su mediana y compararla entre comunidades.
        Y puedo llegar a conclusiones del tipo:
            - La gente de Andalucía tiende a estar más satisfecha que la de Cataluña, y que la de Madrid.

                                MEDIANA del nivel de satisfacción
            Andalucia           8
            Madrid              7
            Cataluña            6

        No significa que toda la gente de Andalucía esté más satisfecha que la de Cataluña en 2 puntos.
        Sino que la mediana de satisfacción de los clientes de Andalucía es 2 puntos superior a la mediana de satisfacción de los clientes de Cataluña.
        Es decir, groso modo, los clientes de Andalucía tienden a estar 2 puntos más satisfechos que los de Cataluña, y 1 más que los de Madrid.

        Esta información que extraigo es más rica!

duracion_llamada_seg x comunidad        <- Cuantitativa x nominal

    Dado que tengo una variable CUANTITATIVA, y dijimos que toda variable CUANTITATIVA es por definición ORDINAL, y que toda ORDINAL es NOMINAL, puedo:
    - Tratar las 2 como nominales:
       Agrupo las duraciones de las llamadas en intervalos, y hago una tabla de contingencia. Y puedo representarla gráficamente con un gráfico de barras apiladas o agrupadas.

       Las duraciones en valor absoluto en realidad no me aportan mucho.
       Me da igual que una llamada haya durado 327 segundos y otra 350.
       Quiero saber en que ORDEN DE MAGNITUD están las duraciones de las llamadas. Y para eso, agrupo en intervalos.

                                            Comunidades
        ETIQUETAS       INTERVALO           Madrid  Cataluña   Andalucía
        MUY CORTA       50-120               327       133       10    
        CORTA           120-240              415       250      100    
        MEDIA           240-300              224       380      150    
        LARGA           300-400               70       350      450
        MUY LARGA       400-600               20       100      375
        
        Puedo representarla con barras apiladas o agrupadas.
        Identifico casillas discretas anómalas.
        No hay todavía estudio de tendencias: MAS o MENOS largas.

    - Trato la variable al menos como ORDINAL, y hago un estudio de comparación de MEDIANAS.
        Puedo calcular la mediana de duración de las llamadas en cada comunidad, y compararlas entre si.
        Y puedo llegar a conclusiones del tipo:
            - La mediana de duración de las llamadas en Andalucía tiende a ser superior a la de Cataluña, y que la de Madrid.
                            Mediana de duración de las llamadas (Intervalos)
            Andalucia           Largas
            Cataluña            Medias
            Madrid              Cortas

                Las llamadas en Andalucía tienden a ser más largas que las de Cataluña, y que las de Madrid.
                HABLO DE TENDENCIAS.. aunque no puedo cuantificar la tendencia (NO ESTOY HACIENDO USO DE LA CAPACIDAD CUANTITATIVA DE LA VARIABLE DURACION_LLAMADA_SEG)
             
    - Al ser la variable duracion_llamada_seg CUANTITATIVA, puedo  hacer el mismo estudio pero con la media:

                            Mediana de duración de las llamadas   Media de duración de las llamadas
            Andalucía           500s                                    520s
            Cataluña            440s                                    450s
            Madrid              400s                                    430s

                No solo digo, las llamadas en Andalucía tienden a ser más largas que las de Cataluña, y que las de Madrid, sino que además cuantifico esa tendencia: 70 segundos más largas que las de Cataluña, y 90 segundos más largas que las de Madrid.

                Puedo generar un boxplot múltiple o un violin plot múltiple.
                Si no lo permie la herramienta que use, puedo crear un histograma apilado... pero si hay muchas comunidades va a ser complejo de interpretar.

duracion_llamada_seg x edad             <- Cuantitativa x cuantitativa

    Aquí el scatterplot me puede ayudar a identificar tendencias.
    También llamado diagrama de dispersión., o nube de puntos.

    Puedo buscar formas en las que los PUNTOS se agrupan/evolucionan en el gráfico.
    Puedo ver si a más edad tiende a haber más duración de las llamadas, o menos.
    O patrones más raros.
    En el intervalo 20-35 años, la duración de las llamadas aumenta conforme a la edad, pero a partir de los 35 años, la duración de las llamadas disminuye conforme aumenta la edad.

    Y ESTO ES LO QUE HAY!

    Identifico que información puedo sacar. 
    Miro a ver a quién le puede interesar.
    Planteo cómo puedo representarla.

Juntando la información que le interesa a cada persona, planteo los cuadros de mando que voy a generar. 
El cuadro de mando es el objetivo final de un proceso de Business Intelligence.
Lo que pasa es que hay mucho trabajo previo.
La mayor parte del tiempo es análisis previo y preparación de los datos.

HAY MAS VARIABLES EN ESTE CONJUNTO DE DATOS. LAS MIRAIS VOSOTROS!
MAÑANA... Nos plantearemos COMO VAMOS A GUARDAR LOS DATOS y QUE ETLs NECESITAMOS CREAR PARA QUE LOS DATOS ESTEN DISPONIBLES PARA SU ANÁLISIS y poder montar los cuadros de mando.
---

Nivel de satisfacción
- Cómo mido esto? No hay un aparato que mida el nivel de satisfacción de una persona.
  Es un ejemplo similar al que os puse el otro día cuando hablamos del nivel de empoderamiento de una mujer.
- Pero es más complejo de hecho... NI SIQUEIRA SE DEFINIR qué significa que un cliente esté más o menos satisfecho con mi empresa.

Es algo que no puedo ni definir ni medir directamente.
Pero si se que hay factores que guardan relación con esa idea de "satisfacción" que tengo en la cabeza. Y esos factores son los que puedo/quiero medir.
- Que un cliente por teléfono me diga que su "nivel de satisfacción conmigo" es de 7... es un dato.
  Y ese dato, transporta información... de muchos tipos:
  - Transporta información acerca de la satisfacción del cliente con mi empresa.
  - Transporta información de si mi cliente ha discutido con su pareja o sus hijos ese día.
  - Transporta información de si en ese momento está en una actividad gratificante o estresante.
 Esta variable TIENE MUCHO RUIDO!
 Eso no significa que descarte la variable. Significa que tengo que tener en cuenta ese ruido a la hora de tomar decisiones.
 Tengo que saber como INTERPRETAR ESE DATO. 

 Quizás averigüe que las llamadas por la tarde tienen en general 2 puntos de satisfacción menos que las llamadas por la mañana. Y eso no significa que mis clientes estén más insatisfechos por la tarde, sino que a esas horas están más estresados y no me dan una información representativa de su nivel de satisfacción real.

 Quizás averigüe que los clientes a los que llama tal operador, tienen en general 2 puntos de satisfacción más que los clientes a los que llama otro operador. Y eso no significa que a uno le hayan tocado los clientes enfadados y al otro los clientes contentos, sino que uno es más empático y sabe tratar mejor a los clientes y el otro no. Y eso me da información de cómo puedo mejorar la satisfacción de mis clientes.

 Y posiblemente me interese RECALIBRAR la medición de satisfacción que hago, teniendo en cuenta estos factores.

Aquí ENTRARIAMOS EN EL TERRENO DEL MACHINE LEARNING.
Podría plantear una REGRESION, o UNA RED NEURONAL, o un ARBOL DE DECISIONES, que genere una PREDICCION DE LA SATISFACCION DE UN CLIENTE, teniendo en cuenta todos los factores que he medido y que guardan relación con la satisfacción de un cliente.