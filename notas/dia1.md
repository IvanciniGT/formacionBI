
# 3 partes

ANALISIS DE DATOS (Estadística)
INFORMATICO (Soporte de los datos + operaciones)

BI = Cuadros de mando, indicadores....

# Estadística

Ciencia que estudia CONJUNTOS DE DATOS. Nos ayuda a entender conjuntos de datos:

Tener los datos != Entender los datos.

Yo puedo tener los datos, todos los datos del mundo y no entender NADA.
Y cuántos más datos, menos entiendo.

Me hago de forma legítima con 5000 nóminas de todos los empelados de una empresa.
5000 nóminas = 5000 folios de papel.. Un tacode 500 hojas son 5~6 cms de alto... Tengo medio metro de papeles delante mia.
Eso son todos los datos que hay acerca de cuándo se paga en esa empresa. Tener los datos = Entender los datos? NO

Puedo leerme las 5000 nóminas. Le dedico 4 días. Cuando acabo... he entendido algo? Tampoco mucho.
Anécdoticamente puedo recordar el salario más alto... y el más bajo... pero poco más.

Necesito entender cómo se paga en la empresa a los distintos empleados. En base a qué criterios se paga más... Son muchas cosas que tengo que analizar.
Y básicamente se trata de RESUMIR la información. 5000 datos mi cerebro no es capaz de procesarlos.

Y ahí es donde entra la estadística. La estadística nos ayuda a resumir la información, con el objetivo de entender mejor los datos.

Qué técnicas nos ofrece la estadística para resumir la información? 

Antes de hablar de esas técnicas, debemos hablar de otro tema:

## Tipos de datos PERO OJO! Desde el punto de vista de la estadística, no desde el punto de vista informático.

Hay 2 grandes tipos de datos (o de formas de medir los datos) en estadística. Una de ellas se divide a la vez en 2 subtipos.

- CUALITATIVOS      No son cuantificables, no se pueden medir con una unidad de medida fija.
  - NOMINALES       Son simplemente NOMBRES, etiquetas, categorías.
                    Color de coche:                 Rojo, Azul, Verde, Amarillo...
                    Para qué sirven esas etiquetas? PARA CLASIFICAR A LOS SUJETOS DE ANALISIS
                    GENERAR GRUPOS
  - ORDINALES       Los medimos también con NOMBRES, pero hay una relación de INTENSIDAD entre ellos. Hay un orden, una jerarquía.
  
                    Por ejemplo: 
                        Nivel de estudios:          Primaria, Secundaria, Bachillerato, Universidad, Master, Doctorado...
                        Altura de una persona:      Baja, Media, Alta
                    Para qué sirven esas etiquetas? PARA CLASIFICAR A LOS SUJETOS DE ANALISIS
                                                    + ORDENAR A LOS SUJETOS
                                                        Poneros en fila por orden de altura . Los más altos primero, luego los de altura media y luego los de altura baja.
                                                        Poneros por orden de nivel de estudios. Los que tienen doctorado primero, luego los que tienen master, luego los que tienen universidad, luego los que tienen bachillerato, luego los que tienen secundaria y por último los que tienen primaria.
- CUANTITATIVOS     Que se puede cuantificar, medir... y para medir, qué hace falta? UNA UNIDAD DE MEDIDA
                    Los datos cuantitativos tienen unidad de medida
                    Por ejemplo:
                        - Salario de una persona:             CUANTITATIVO    Unidad de medida: Euros
                          1500€ , 5127€, 2000€, 3000€...
                          Pero... si os fijais... Esas cantidades no son sino ETIQUETAS:
                            1500€ es una etiqueta que significa que esa persona cobra 1500 euros. 
                            5127€ es una etiqueta que significa que esa persona cobra 5127 euros.
                    Para qué sirven esas etiquetas? PARA CLASIFICAR A LOS SUJETOS DE ANALISIS
                                                    + ORDENAR A LOS SUJETOS
                                                      Coloquense por orden de salario. Los que cobran más primero, luego los que cobran menos y por último los que cobran nada.
                                                    + Puedo hacer operaciones matemáticas.
                                                      Qué operaciones puedo hacer? DEPENDE.
                    Dentro de los datos CUANTITATIVOS hay 2 tipos (en general pasamos un poco de esto)
                        - DE RAZON          Cuando hay un CERO ABSOLUTO
                                            Número de hijos de una familia: CERO HIJOS... ES UN CERO ABSOLUTO? Hay valores posibles por debajo de ese número? NO
                                Puedo hacer SUMAS, RESTAS, MULTIPLICACIONES, DIVISIONES.
                        - DE INTERVALO      Cuando no hay un cero absoluto.
                                            Temperatura medida en Grados Centigrados: 20ºC 
                                                    ES UN CERO ABSOLUTO? NO.  Hay valores posibles por debajo de ese número? SI
                                Puedo hacer SUMAS, RESTAS.

                            Tengo una familia con 2 hijos y otra con 4.
                                Puedo decir que entre las 2 familias tienen 6 hijos? SI             SUMA
                                Puedo decir que una familia tiene 2 hijos menos que la otra? SI     RESTA
                                Puedo decir que una familia tiene el doble de hijos que la otra? SI     MULTIPLICACION
                                Puedo decir que una familia tiene la mitad de hijos que la otra? SI     DIVISION

                            En Madrid estamos a 15ºC.... Y en Moscú a -15ºC.
                                Puedo decir que en Madrid hace 15ºC más que en Moscú? SI     RESTA
                                Puedo decir que en Madrid hace el doble de temperatura que en Moscú? NO

                            Año de nacimiento de una persona 2000. Qué tipo de datos es? CUANTITATIVO O CUALITATIVO? Cuantitativo.
                                DE INTERVALO

                                Tengo otra persona que nació en el año 1000. 
                                    La primera persona nació 1000 años despues de la segunda? SI
                                    La primera persona nacio el DOBLE de años despues que la segunda? EIN???

                                Tengo otra persona que nació en el 1000 antes de Cristo. 
                                    La primera persona nació 3000 años despues de la segunda? SI
                                    La primera persona nacio el - triple de años despues que la segunda? EIN???


Salario de una persona:             CUANTITATIVO    Unidad de medida: Euros
Número de hijos de una familia:     CUANTITATIVO    Unidad de medida: Hijos: 1 hijo, 2 hijos, 3 hijos...
COLOR DE UN COCHE:                  CUALITATIVO
Código postal:  28750               CUALITATIVO
Número de la calle en el que vives? CUALITATIVO

Hay datos, que parecen permitir hacer operaciones matemáticas... pero que realmente no lo permiten.. o al menos no sale nada con sentido:
    CP1: 28750
    CP2: 28751
    SUMA =====
         57501 ??? EIN??? Tiene sentido este número? NO


RESUMEN:

    CUALITATIVOS:
        NOMINALES:  Solo son etiquetas
                    Sirven para clasificar a los sujetos de análisis
        ORDINALES:  Son etiquetas con un orden entre ellas  
                    Sirven para clasificar a los sujetos de análisis
                      + Sirven para ordenar a los sujetos de análisis
    CUALITATIVOS:   Son eqtiquetas, con orden y unidad de medida 
                    Sirven para clasificar a los sujetos de análisis
                      + Sirven para ordenar a los sujetos de análisis
                      + Pueden permitir hacer operaciones matemáticas (depende del tipo de dato cuantitativo)

Y POR DEFINICION, TODO DATO CUALITATIVO ES ORDINAL
Y POR DEFINICION, TODO DATO ORDINAL ES NOMINAL
Y POR DEFINICION, TODO DATO CUANTITATIVO ES ORDINAL

Y según me interese además puedo cambiar de escala de medición HACIA ABAJO... NUNCA HACIA ARRIBA.

Hay datos que puedo medirlos de varias formas:

    Número de hijos: 
        - CUANTITATIVO : 1 hijo, 2 hijos, 3 hijos...
        - ORDINAL:       Ninguno, Poco, Algunos, Demasiados hijos
        - NOMINAL:       SI/NO

    Si los he medido de forma CUANTITATIVA, puedo convertir la medida a una escala ORDINAL? Sin problema.
        Los que tiene 0 hijos = Ninguno
        Los que tiene 1-2 hijos = Pocos
        Los que tiene 3-4 hijos = Algunos
        Los que tiene 5 o más hijos = Demasiados
    Si los he medido de forma ORDINAL, puedo pasar a una medición NOMINAL? Sin problema.
        Los que tienen Ninguno = NO
        Los que tienen Pocos, Algunos o Demasiados = SI

    Ahora... Si los he medido de forma NOMINAL, puedo pasar a una medición ORDINAL? NO

    Cuantitativo -> Ordinal -> Nominal
    Al revés NUNCA.

## Técnicas estadísticas para resumir la información

### Frecuencia

Cuántas veces se repite un valor en un conjunto de datos.

Tengo 1000 coches aparcados... en stock... Cada uno de su color...
Y quiero saber (entender) como son los coches que tengo... COMO COLECTIVO. No como caso concreto.
Tener los 1000 coches delante NO ME AYUDA a responder a esta pregunta. No me dan los ojos para mirar 1000 coches.
Voy a hacer una tabla de frecuencias. Es una forma de RESUMIR LOS DATOS:

                                                        FREC. RELATIVA
    COLOR           CUANTOS HAY = FRECUENCIA ABSOLUTA       %
    ------------------------------------------------------------
    Rojo                100                                10%
    Verde                50                                 5%
    Azul                200                                20%
    Morado                3                                 0.3%
    Negro               300                                30%
    Blanco              347                                34.7%
                -----------                             ---------
                1000 coches                             100%

Esta es la forma más simple de resumir la información que nos ofrece la estadística.
Y estas tablas las puedo representar gráficamente. La estadística define algunas gráficas que nos ayudan a entender mejor los datos.

    Qué necesito para poder generar una tabla de frecuencias?
        Poder CLASIFICAR a los sujetos (en nuestro caso a los coches) por un dato.
        Los datos NOMINALES, los puedo clasificar en grupos? SI
        Y los ordinales... y los cuantitativos. TODOS SON CLASIFICABLES... AGRUPABLES.
        De todos puedo hacer una tabla de frecuencias.

        Y esa tabla la puedo representar mediante un:
        - DIAGRAMA DE BARRAS

        FRECUENCIA (Cantidad)
               ^
         350   |                                    XXX
         300   |                              XXX   XXX
         250   |                              XXX   XXX
         200   |        XXX                   XXX   XXX
         150   |        XXX                   XXX   XXX
         100   |  XXX   XXX                   XXX   XXX
         50    |  XXX   XXX    XXX    ___     XXX   XXX
               +--------------------------------------------------------- VARIABLE
                  ROJO  AZUL  VERDE  MORADO  NEGRO  BLANCO

También puedo representarla mediante un diagrama de SECTORES (TARTA, QUESITO)

    Pero ahí pondría los valores RELATIVOS... Los porcentajes.

El tipo de gráfica viene condicionado por el TIPO DE DATO que representa.

OJO. 
Puedo hacer la tabla de frecuencias de cualquier tipo de dato...
Pero eso no significa que deba hacerla... 
Hay casos en los que la tabla de frecuencias NO ME AYUDA A ENTENDER LOS DATOS! QUE ES EL OBJETIVO FINAL DE TODO ESTO.

Cuando tengo variables CUANTITATIVAS con muchos valores distintos NO COMPENSA.

Salario de las personas:
    Felipe:  1531.78
    Menchu:  1598.32
    Gertru:  2137.98
    ...
    5000 personas.
    
    Cuántos salarios se van a repetir? Pocos.
    Y al final, me quedaría una tabla de frecuencias que tendría casi las mismas filas que el total de datos que tengo... Y todas ellas con cantidad 1/2 
        NO HE RESUMIDO = RUINA!

En estos caso, qué hacemos?
Rebajo el nivel de medición de la variable: PASO DE CUANTITATIVA A ORDINAL

    Rango salarial          Cantidad
    --------------------------------
    1000-2000                200
    2000-3000                500
    3000-4000                150
    4000-5000                100
    Más de 5000               50
    ----------------------------
     TOTAL:                 1000

    Cuidado.. me da igual haber puesto 1000-2000 que POCO!

    Rango salarial          Cantidad
    --------------------------------
    Muy Poco                200
    Poco                    500
    Normnal                 150
    Bastante                100
    Mogollón                 50
    ----------------------------
     TOTAL:                 1000

Esta tabla SI RESUME... Y me ayuda a entender mejor la información.

Las tablas de frecuencias SON LA BASE DE LA ESTADISTICA.

Pero hay más cosas = LOS INDICADORES ESTADISTICOS. El objetivo es resumir la información a UN SOLO DATO

Y hay varios tipos de indicadores estadísticos, que los agrupamos en categorías:

### INDICADORES DE TENDENCIA CENTRAL.

Me informan de "POR DONDE VAN LOS TIROS". GROSO MODO cómo esta la "cosa" en mis datos:

- MODA          El valor que está de "moda"... el que más se repite.
                Busco en la tabla de frecuencias el valor que más se repite y ese es la MODA.
                Por ende, para calcularlo, solo necesito la tabla de frecuencias... 
                Por ende, a cualquier tipo de dato le puedo calcular la MODA.
- MEDIANA       El valor que divide en 2 grupos de igual tamaño (o lo más iguales de tamaño posible) a mis dato.
                Para calcularla, ORDENO todos los datos... Y tomo el del centro.

                Alturas de personas:
                    150cm
                    157cm
                    165cm - LA MEDIANA SERIA LA MEDIA ENTRE 165 y 175 = 170cm
                    175cm 
                    180cm
                    198cm
                Si en los salarios tengo una mediana de 1500 euros, eso significa que la mitad de los empleados cobran menos de 1500 euros y la otra mitad cobra más de 1500 euros.
                Es decir, que el 50% no llega a 1500 euros y el otro 50% pasa los 1500 euros.

                A qué tipo de datos puedo calcular una MEDIANA? A los datos ORDINALES y a los datos CUANTITATIVOS. A los NOMINALES NO.

- MEDIA         Es muy compleja de interpretar y suele llevar a equivocaciones.
                Se calcula mediante una fórmula matemática: SUMO TODOS LOS VALORES Y DIVIDO ENTRE EL NUMERO DE VALORES
                Cómo se interpreta? Se interpreta como el valor que aporta cada sujeto al total SI TODOS APORTASEN LO MISMO.

                Alturas de personas:
                    150cm
                    160cm       MEDIA = 150 + 160 + 180 + 190 = 680 / 4 = 170cm
                    180cm
                    190cm
                
                A qué tipo de dato puedo calcularle la media? SOLO A LOS DATOS CUANTITATIVOS. A los NOMINALES y a los ORDINALES NO.
                Nominales y ordinales NO PERMITEN HACER OPERACIONES MATEMATICAS

                Tiene sentido hacer una media de Códigos postales? NO... Matematicamente puedo sumar los números (CP) y dividir ese resultado entre el número de códigos postales que he sumado... pero el resultado no tiene sentido. 
                No me aporta nada. No me ayuda a entender los datos.
                Y aquí hay gente que lo hace muy mal.

                    Nivel de dolor de un paciente del 0 al 10. Es cuantitativa? NO LO ES... NO TIENE UNIDAD DE MEDIDA.
                    No puedo hacer la media de los niveles de dolor de un paciente. No tiene sentido. 
                    De hecho lo que para mi es un 5 para otro puede ser un 7... porque no hay una unidad de medida que sirva de patrón.
                    Es una medida SUBJETIVA.
                    Y por qué lo preguntan entonces? Para qué les vale ese dato? PARA NADA!
                    Les vale cuando le preguntan de nuevo a las 2 horas después de haberle puesto un tratamiento...
                    Ver si mejora o empeora o está igual.

                Se usa mucho... pero es MUY MENTIROSA!

### Ejemplo

#### El pueblo de villa arriba de arriba

Viven 10 personas... con sus coches.
Cada coche expulsa de CO a la atmósfera :
    
    5gCO/día
    5gCO/día
    5gCO/día

    10gCO/día               Media:      10gCO/día
    10gCO/día               Mediana:    10gCO/día
    10gCO/día
    10gCO/día
    
    15gCO/día
    15gCO/día
    15gCO/día

    A la media le gusta ser igual a la mediana... es una envidiosa!
    Pero no siempre ocurre.

        Tabla de frecuencias:
        
        5gCO/día      3
        10gCO/día     4
        15gCO/día     3

        Gráfica de barras:

        |         X  
        |   X     X     X                   Esta gráfica es simética?
        |   X     X     X                   Puedo trazar una linea vertical que divida la gráfica en 2 partes simétricas? SI
        |   X     X     X                       TOTALMENTE SIMETRICA.. como las manos.
        +------------------------
            5    10    15
                 
        Cuando los datos al representarlos gráficamente tienden a ser simétricos, la media y la mediana tienden a ser iguales.


#### El pueblo de villa arriba de abajo

Viven 10 personas... con sus coches.
Pero están muy mentalizados con el medio ambiente. 
Se han puesto de acuerdo para ir a comprar el mismo coche (y que les hagan un barato en la tienda).
Eléctricos... que contaminan poco.
Cada coche expulsa de CO a la atmósfera :
    
    5gCO/día
    5gCO/día
    5gCO/día
    5gCO/día                Media: 5x9 + 1000 = 1045 / 10 = 104.5gCO/día
    5gCO/día                Mediana: 5gCO/día
    5gCO/día
    5gCO/día
    5gCO/día
    5gCO/día
    Manolo.... 1000gCO/día

La media en villa arriba de arriba es de    10gCO/día
La media en villa arriba de abajo es de    104.5gCO/día

    Tabla de frecuencias:
        
        5gCO/día      9
        1000gCO/día   1

        Gráfica de barras:

        ^
        | X
        | X
        | X
        | X
        | X
        | X
        | X
        | X
        | X                                                                                                         X
        +--------------------------------------------------------------------------------------------------------------
          5                       104                                                                              1000
          ^                       ^
          MEDIANA                 ^
          ---------------------–> MEDIA MANOLO TIRA DE LA MEDIA HACIA EL!

        Esta gráfica es totalmente ASIMETRICA. No puedo trazar una linea vertical que divida la gráfica en 2 partes simétricas.
        En este caso, media y mediana no son iguales. 

        Por cierto... Los salarios en una empresa... pensáis que tienen una distribución simétrica o asimétrica? ASIMETRICA.
        La mayor parte de la gente tiene salarios BAJOS...
        Y los 4 jerifantes son los que se reparten el turrón... Los salarios altos (SON MANOLOS!!!!) que suben la media de salarios... y que noss hacen pensar que la gente gana más de lo que gana... al ver el salario medio.
        Nunca me fiaría de una media salarial.
        Me interesa mirar la MEDIANA salarial.


Hace justicia el dato? Ayuda ese datoa entender la realidad de los pueblos?
Si solo miro la media, qué pueblo pienso que están más concienciado/preocupado por el medio ambiente? villa arriba de arriba
Y nada más lejos de la realidad. En Villa arriba de abajo la gente está mucho más concienciada que en villa arriba de arriba.
Si tuviera que montar un negocio ecológico lo montaría posiblemente en villa arriba de abajo, que la gente está por la labor (en el rollito, en el mood!)

La mediana en villa arriba de arriba es de 10gCO/día
La mediana en villa arriba de abajo es de 5gCO/día

La mediana es mucho más representativa en este caso de la situación. Nos ayuda mejor a entender los datos, que es el objetivo.
OJO, los 2 datos son reales... y válidos. PERO LA MEDIA me despista! NO ME AYUDA A ENTENDER LOS DATOS, que era el objetivo!

Los 104.5 es lo que contaminaría cada coche si TODOS CONTAMINASEN LO MISMO... Pero eso no es lo que ocurre en villa arriba de abajo. La realidad es que 9 coches contaminan 5gCO/día y uno contamina 1000gCO/día. La media me da un dato que no me ayuda a entender la realidad de los datos. La mediana sí.

La media es GUAY! NOS ENCANTA.. pero solo podemos usarla a veces.
Otras veces la media me despista.. y entonces preferimos usar la mediana.

Cuándo ? Dependiendo de la SIMETRIA de los datos.

En las práctica lo solemos hace más fácil.
Calculo las 2, media y mediana.
Si son más o menos parecidas, me quedo con la media.
Si son muy diferentes, en el informe, gráfica, cuadro de mando, lo que sea, pongo la mediana!

RESUMIENDO:

    Medidas de tendencia central:

         MODA           Nominal    (Ordinal   Cuantitativo)
         MEDIANA                    Ordinal   Cuantitativo
         MEDIA                                Cuantitativo

    La moda solo si el dato es nominal.
    Si el dato es ordinal: MEDIANA
    Si el dato es cuantitativo: MEDIANA o MEDIA despendiendo de si se parecen o no.
                                Si se parecen: MEDIA
                                Si no se parecen MEDIANA (o incluso ambas)

### Medidas de dispersión

No sirve (NO HACER NUNCA!!!!) poner una medida de tendencia central sin su medida de dispersión asociada.
ESTO ES IMPORTANTISIMO!

Hemos dicho que el objetivo de la estadística es darnos técnicas, indicadores, gráficas que ayuden a resumir los datos para ENTENDERLOS MEJOR!
Una media, o una mediana son un resumen MUY AGRESIVO de los datos.

Estamos pasando de 5000 nóminas a 1 número!
Eso es resumir mucho. Y en ese resumen perdemos información.
Y a veces perdemos más de la cuenta!

Cuando vemos un dato de tendencia central, nuestro limitado cerebro humano tiende a pensar que todos los sujetos tienen valores cercanos a ese dato. YA nos ha pasado con Manolo y villa arriba de abajo:

Media: 104,5 ... y tendemos a pensar que todos andan por ahí.. y no era verdad.

Mirad este ejemplo:

    Viviendas con gente dentro...   que tienen una determinada altura.

    CASA 1:
        175cm   175cm   175cm   175cm
          0       0       0       0   Diferencias a la media al cuadrado
                                      Media de esas diferencias: 0 cm²
                                      Raiz de la media de esas diferencias: 0 cm
                                            Media:      175cm *
                                            Mediana:    175cm
                                      Multiplico la desviación típica por 1.4 = 0
                                      Calculo el intervalo:
                                        Mínimo: 175 - 0 = 175
                                        Máximo: 175 + 0 = 175
                                        Entre 175 y 175 hay al menos el 50% de los habitantes de la casa. Osea, todos los habitantes de la casa.


    CASA 2:
        165cm   165cm   185cm   185cm
          10cm   10cm     10cm     10cm  Diferencias a la media
         100cm²  100cm²   100cm²   100cm²  Diferencias a la media al cuadrado
                                         Media de esas diferencias: 100 cm²
                                         Raiz de la media de esas diferencias: 10 cm
                                            Media:      175cm
                                            Mediana:    175cm
                                        Multiplico la desviación típica por 1.4 = 14
                                        Calculo el intervalo:
                                        Mínimo: 175 - 14 = 161
                                        Máximo: 175 + 14 = 189
                                        Entre 161 y 189 hay al menos el 50% de los habitantes de la casa. Osea, todos los habitantes de la casa.

    CASA 3:
        165cm   175cm   175cm   185cm
         100cm²  0cm²   0cm²    100cm²   Diferencias a la media al cuadrado
                                         Media de esas diferencias: 50 cm²
                                            Raiz de la media de esas diferencias: 7.07 cm
                                            Media:      175cm
                                            Mediana:    175cm

    CASA 3.5:
        170cm   170cm   180cm   180cm
         25cm²  25cm²   25cm²   25cm²   Diferencias a la media al cuadrado
                                           Media de esas diferencias: 25 cm²
                                           Raiz de la media de esas diferencias: 5 cm
                                            Media:      175cm
                                            Mediana:    175cm
                                        Multiplico la desviación típica por 1.4 = 7
                                        Calculo el intervalo:
                                        Mínimo: 175 - 7 = 168
                                        Máximo: 175 + 7 = 182
                                        Entre 168 y 182 hay al menos el 50% de los habitantes de la casa. Osea, todos los habitantes de la casa.

    CASA 4:
        155cm   165cm   185cm   195cm
         400cm²  100cm²   100cm²   400cm²   Diferencias a la media al cuadrado
                                        Media de esas diferencias: 250 cm
                                        Raiz de la media de esas diferencias: 15.81 cm
                                            Media:      175cm
                                            Mediana:    175cm
    CASA 5:
        150cm   150cm   200cm   200cm
        625cm²  625cm²  625cm²  625cm²   Diferencias a la media al cuadrado
                                        Media de esas diferencias: 625 cm
                                        Raiz de la media de esas diferencias: 25 cm
                                            Media:      175cm
                                            Mediana:    175cm

Osea, tengo 4 casas que dan misma media y misma mediana.. y se parecen en algo los habitantes de esas 4 casas? NO. Son muy diferentes.

Si solo tuviera el dato de media y mediana:

                    Media     Mediana
        Casa 1      175cm       175cm
        Casa 2      175cm       175cm
        Casa 3      175cm       175cm
        Casa 4      175cm       175cm
        Casa 5      175cm       175cm

Si solo veo esa tabla, y no tengo el detalle de todos los datos como arriba, tendería yo a pensar que los habitantes de esas 4 casas son muy parecidos... y no es así.

Este es el problema de dar solamente la media o la mediana. Que varios grupos diferentes pueden tener la misma media y mediana, pero ser muy diferentes entre sí.

Tengo que comprar 2 bancos para sentarse.. Los hay de tres tallas. Grandes, medianos, y pequeños.
Para la casa 1: 2 medianos
Para la casa 2: 1 pequeño y 1 grande
Para la casa 3: 2 medianos
Para la casa 4: 1 pequeño y 1 grande

Aquí entran las medias de dispersión. 
Las de tendencia central me informan de "por donde van los tiros" de mis datos. Por dónde está la cosa con mi variable

Las medidas de dispersión me informan de "Cuánto cambian los sujetos con respecto a la medida de tendencia central".

Antiguamente se usaba la Desviación MEDIA. Que es la media del valor absoluto de las diferencias entre cada dato y la media de todos los datos.
Lo que hemos calculado se llama VARIANZA!
Es la media de las diferencias de los datos con respecto a la media, pero elevadas al cuadrado.
Tampoco nos gusta mucho... Es muy compleja de interpretar.
La complejidad viene porque elevamos la unidad de medida al cuadrado...
    cms -> cm²
    euros -> euros²
    hijos -> hijos²
Y eso no tiene sentido. No me ayuda a entender los datos.

Cómo lo resolvemos.. pues tirando por la calle de en medio... le hago la raiz cuadrada a esa varianza... y lo que me queda se llama desviación TIPICA.
Al hacer la raiz cuadrada, recupero la unidad de medida original y eso me ayuda a entender mejor los datos.

Eso es la desviación típica. 
Es un dato que para casas (colectivos) con misma media me da números diferentes.
Me ayuda a saber que 2 colectivos son distintos entre si a pesar de tener la misma media y mediana.

Ahora bien... COMO SE INTERPRETA ESE DATO?
La interpretación la da la Regla de Chebyshev. 

    Salario medio de 1500€ con una desviación típica de 200€.
    Cómo se interpreta eso?
    La forma de interpretarlo es la siguiente:
    - Tomo la desviación típica y la multiplico por RAIZ DE 2 = 1.414 (redondeo a 1.4)
    - 200 x 1.4 = 280
    - Lo sumo y resto de la media:
      - 1500 - 280 = 1220
      - 1500 + 280 = 1780
    - Al menos el 50% de los empleados cobran entre 1220€ y 1780€.
    - Es decir, si tengo al menos el 50% de la gente, eso significa la MAYOR PARTE DE LA GENTE.
    - Y si son la mayor parte alrededor de la media, eso significa la mayor parte MAS REPRESENTATIVA de la gente.

Siempre, SIEMPRE que ponga una media en un informe he de acompañar esa media de su desviación típica. SIEMPRE. SIEMPRE. SIEMPRE. Así evitaré malinterpretaciones y que la gente piense que todos los sujetos están en la media, o que grupos con misma media son iguales entre si, cuando no es así.
Claro... eso en el caso de que elija la media como medida de tendencia central. 
Si elijo la mediana como medida de tendencia central, NO TIENE SENTIDO CALCULAR LA DESVIACION TIPICA.
Si elijo la mediana, es porque la MEDIA NO SEA REPRESENTATIVA DEL COLECTIVO (como con Manolo y villa arriba de abajo). Y si la media no es representativa, la desviación típica (que se calcula usando la media) tampoco lo va a ser. No tiene sentido calcularla.
Si uso la mediana lo que entrego es otro estadístico: Rango Intercuartílico.
    Mediana = Q2
    RANGO INTERCUARTILICO = Q3 - Q1

# Medidas de POSICION

Esto lo usamos mucho en estadística y en análisis de datos.
Imaginad el siguiente escenario:
- Tengo una aplicación informática. 
  Esa aplicación se conecta a un servidor de base de datos.
  Y esa conexión tarda tiempo en establecerse.
  Mido los tiempos que tarda en establecerse esa conexión a lo largo de un mes.... para cientos de empleados que usan la aplciación.
  Acabo con miles de datos... Medidas del tiempo que tarda la conexión en establecerse. 
  Necesito entender aquello... Los miles de datos sueltos NO LOS ENTIENDO...
  Necesito resumir.
  Qué me interesa dar aquí?
  - Media      No me interesa. No me interesa por donde van los tiros.
  - Mediana    No me interesa. No me interesa por donde van los tiros.
    Ambos 2 me informa de MAS O MENOS cuanto tarda la conexión en establecerse. 
  - Máximo     Este interesa? NO. Es una ANECDOTA. Aquí no analizamos ANECDOTAS.
  - Mínimo     Este interesa? NO. Es una ANECDOTA. Aquí no analizamos ANECDOTAS. 
  - ???

    Me interesa otra cosa.
    Me interesa GARANTARIZAR que la inmensa mayoría de las conexiones se establecen en menos de un tiempo determinado que considero aceptable!
    ESO ME LO OFRECEN LAS MEDIDAS DE POSICION, por ejemplo los PERCENTILES!

    Aquí me interesaría un P95 o un P99
    Me interesa saber el tiempo máximo que tarda en establecerse la conexión para el 95% o el 99% de los usuarios.
    Oye que algunos les tarda más... Cosas raras pasan.. que voy a hacer. No me voy a cortar las venas. Tengo que asegurar que LA INMENSA MAYORIA de la gente pueda trabajar bien! Que tengo a 2 jodidos... mala suerte... Intentaré que no... pero mala suerte. TE HA TOCADO. 
    Mi trabajo no es para ti... ES PARA EL COLECTIVO! Mi prioridad es que la inmensa mayoría trabaje bien.
    Que tengo 5 personas con problemas... ANECDOTAS... Que si quiero analizaré COMO CASOS SUELTOS... no como colectivo. 5 datos / usuarios: NO SON UN COPLECTIVO.
    Quizás menchu tiene el windows desactualizado y por eso está jodida.
    Quizás Gertru tiene el antivirus a tope... y por eso le va lento.
    Quizás manolo esta conectado por telefono... y por eso le va lento.

Usamos mucho los percentiles en análisis de datos.
Un percentil X es el valor de la variable que deja por debajo de él al X% de los datos.
El percentil 47 es el valor de la variable que deja por debajo de él al 47% de los datos.
El percetil 50 es el valor de la variable que deja por debajo de él al 50% de los datos = MEDIANA

La mediana, además de una medida de tendencia central es una medida de posición. Es el percentil 50.

A veces en lugar de percentiles trabajamos con los CUARTILES:
Q1 = P25
Q2 = P50 = MEDIANA
Q3 = P75

A veces también se usan los deciles:
D3 = P30
D7 = P70

Usamos mucho las medidas de posición: Especialmente P95 y P99.

Al final, el objetivo de BI es aportar datos que mejoren la toma de decisiones en la empresa.
Business Intelligence es un conjunto de técnicas y herramientas que nos ayudan a tener datos que ayuden a la toma de decisiones en la empresa.
Pero no los datos en bruto. En una empresa hay millones de datos en bruto... que no entendemos
Necesitamos aplicar técnicas de estadística para resumir esos datos y entender mejor lo que está pasando en nuestro negocio, para así poder tomar decisiones INFORMADAS = NO SUBJETIVAS.

El objetivo final es tener CUADROS DE MANDO, INFORMES, GRAFICAS, INDICADORES (generados aplicando técnicas estadísticas) que nos ayuden a entender mejor los datos y así poder tomar decisiones INFORMADAS = NO SUBJETIVAS.

Claro.. pero eso es la ESTADISTICA... porque llamarlo entonces BI. Porque para poder hacer esos análisis/aplicar esas técnicas, los datos habrá que procesarlos y guardarlos de forma OPTIMIZADA en soportes INFORMATICOS de forma que esas gráficas, cuadros de mando, informes, indicadores se puedan generar de forma rápida y eficiente.

La estadística me dice como generar / resumir los datos para entenderlos mejor.
Pero eso lo podría hacer en papel y con lapicero y calculadora... y tardar meses en ir calculando toda esa informaicón.
Lo que se trata es de poder hacer esos cálculos de forma rápida y eficiente, para poder tomar decisiones rápidas en la empresa, sobre millones de datos. Y entonces NO ES SOLO APLICAR TECNICAS ESTADISTICAS.
TAMBIEN ES APLICAR TECNICAS INFORMATICAS. Y eso es BI. La estadística es una parte de BI, pero no toda la parte de BI.

---

# GLOSARIO / VOCABULARIO que se usa en el mundo de los de datos.

BBDD (bases de datos)
    Es donde las empresas guardan la información con la que están trabajando activamente.
    Están en los entornos de producción de la empresa.
    Depende la empresa, el tipo de dato que voy a guardar.
    - Banco: Transacciones de clientes, cuentas, tarjetas, préstamos, etc...
    - Compañía de seguros: Contratos, Expedientes de siniestros, clientes, pólizas, etc...
    - Retail: Clientes, productos, ventas, puntos de venta, etc...

    Esas bases de datos están optimizadas para ejecutar sobre ellas procesos OLTP (OnLine Transaction Processing) = Procesamiento de Transacciones en Línea.
    
    Es decir, favorecen la rápida actualización de los datos: Nuevos datos (insert), modificación de datos (update), borrado de datos (delete). PERO NO FAVORECEN LA ANALITICA DE DATOS.
    En las BBDD hacer consultas para ANALITICA DE DATOS es muy lento e ineficiente.
    De hecho, puede incluso dejar FRITO el servidor tratar de hacer una de estas consultas de analitica de datos.

Para evitar este problema, muchas (todas) las empresa, van sacando datos de sus BBDD de producción cuando ya no los usan activamente.
    Si soy una compañía de seguros, el parte de siniestro que ocurrío a Menchu hace 3 años.. y que se cerró hace 2, NO ME INTERESA TENERLO OCUPANDO ESPACIO en la BBDD de producción. Ese datos esta MUERTO Y ENTERRAO!
    Pero cuidado... una cosa es que no me valga para la OPERACION DIARIA, y otra cosa es que:
    - No tenga OBLIGACION LEGAL de mantener el dato X años.
    - Que pueda sacarle partido haciendo ANALISIS DE DATOS.
      Soy un banco y doy préstamos. Hay prestamos ya cerrados. vencidos.. que se pagaron. O NO SE PAGARON y quedaron como deuda. Me puede interesar analizar esos datos con perspectiva para saber que tipo de cliente SI ME CUMPLE CON LOS PAGOS y cuales no... para decidir a futuro a quien le voy a dar un prestamo.

Y esos datos los guardan en un DATA LAKES

Un data lake es otro tipo "de base de datos". Pero que guarda información en bruto histórica.
Guarda datos, según salen de las BBDD de producción de las empresas.... a la espera de que esos datos puedan usarse en un futuro para algo... Para qué? NPI

Es un cajón desastre donde voy echando TODO EL HISTORIAL DE DATOS DE LA EMPRESA.

En el momento que decido que ciertos datos son interesantes para una finalidad (ANALISIS) los preparo (COCINO) para que pueda hacer esa finalidad de forma eficiente. Y ya cocinados los dejo en un DATA WARE HOUSE

Un DATAWARE HOUSE es "otro tipo de BBDD" que tiene datos históricospreparados para un uso concreto: Por ejemplo, para hacer Business Intelligence.

    En estas "bases de datos" que llamamos DATA WARE HOUSE, los datos se guardan de forma que se optimicen la ejecución de procesos OLAP: Procesos de Análisis de datos.

    BBDD         -> DATALAKES            -> DATAWAREHOUSE
    Datos vivos     Historico en bruto      Histórico cocinado/preparado para un objetivo (BI)

Creamos programas que mueven los datos entre las BBDD y los datalakes y los datawarehouse. Esos programas reciben el nombre de ETLs.
Una ETL (Extract, Transform, Load) es un programa que extrae datos de una fuente de dato, los transforma y los carga en otro tipo de BBDD.







Business Intelligence (BI)

Ciencia de datos (Data Science)
    DataMining
    Machine Learning
    Deep Learning

Ingeniería de datos (Data Engineering)
    BigData