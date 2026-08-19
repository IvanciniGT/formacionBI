
# Ayer / Resumen

Temas estadísticos (UNIVARIABLE)
VOCABULARIO (DATAWAREHOUSE, DATALAKE, DATAMINING, MACHINE LEARNING, BIGDATA)
Temas informáticos a tener en cuenta en el mundo BI:
    - Tipos de datos informáticos (Números, Textos, Valores lógicos...)
    - Bytes (Y de cuántos bytes hacen falta para según qué tipo de dato)
    - Tipos de almacenamiento (Almacenamiento orientado a filas - BBDD, Almacenamiento orientado a columnas - Datawarehouse/Datalake)
Trasformaciones/Preprocesamiento de los datos:
- Calidad de dato (Datos que faltan, datos erroneos, datos duplicados, datos inconsistentes)
- Recodificación de datos (Agrupación de categorías, Agrupación de rangos, Asignación de Códigos, Conversión de tipos de datos)
- Desnormalización de datos (Desnormalización de tablas, Desnormalización de columnas), repitiendo información para mejorar la eficiencia de las consultas.
- PreCálculo de información de interés para los estudios que vamos a hacer:
  - Puedo tener una fecha de un evento, pero me puede interesar precalcular si esa fecha cae en fin de semana o no. O en qué mes/día de la semana.

# HOY

- Antes del descanso. Conceptos de estadística... principalmente de estadística BIVARIABLE
- Después del descanso. Ejercicio/Proyecto.
  Os voy a pasar una colección de datos... en bruto!
  El objetivo es estudiarlos y sacar conclusiones.
    - Tipos de datos que tenemos: Estadístico / Informático
    - Qué tipo de preguntas me interesa/puedo responder con esos datos.
      - Qué estadísticos vamos a calcular para cada campo, tablas, gráficas, filtros
    - Calidad de los datos y posibles pretratamientos que sean necesarios.

---
# Conceptos de estadística.

Desde el punto de vista estadística hablamos de datos:
- Cualitativos
  - Nominales
  - Ordinales
- Cuantitativos

Podemos analizar variable a variable de nuestro conjunto de datos. Eso lo hacemos siempre.
- Tablas de frecuencia (para variables cualitativas) -> Gráficos de barras, gráficos de sectores
- Indicadores de tendencia central:
                    Nominal     Ordinal     Cuantitativa 
    - Media                                     √
    - Mediana                      √            √        (hay que decidir aquí)
    - Moda            √            √            ~
- Indicadores de dispersión:
    - Desviación típica     <- Media
    - Rango intercuartílico <- Mediana
- Medidas de posición:
    - Cuartiles, Deciles, Percentiles

Nos falta 2 cositas con respecto al análisis univariable. Gráficos.

- De momento no tenemos NINGUN GRAFICO PARA VARIABLES CUANTITATIVAS
  Y de hecho hay 2:


    DATOS de una variable que hemos medido: SALARIO €
    1000        1400        1600
    1000        1400        1700
    1100        1400        2000
    1200        1400        3000
    1200        1500
    1300        1500

    Tabla de frecuencias:

    Salario   | Frecuencia
    1000      | 2
    1100      | 1
    1200      | 2
    1300      | 1
    1400      | 4
    1500      | 2
    1600      | 1
    1700      | 1
    2000      | 1
    3000      | 1

    Este es el problema de hacer una tabla de frecuencias para datos cuantitativos... Al final me salen muchas filas... con frecuencias muy bajas. Y en general la tabla será muy grande! Habremos resumido poco! No compensan.

    Ya hablamos que en este escenario lo que hacemos son trabajar con intervalos -> Convertir la variable en ORDINAL.

        Intervalos de Salario   | Frecuencia | Etiquetas
        1000 - 1250             | 5          | Muy poco       
        1251 - 1500             | 7          | Algo
        1501 - 1750             | 2          | Más
        1751 - 2000             | 1          | Bastante
        2001 - 2250             | 0          | Mucho
        2251 - 2500             | 0          | Muchísimo
        2501 - 2750             | 0          | Muchísimo más
        2751 - 3000             | 1          | Mogollón

    Esta tabla si empieza a generar un buen resumen.
    Esa tabla la podemos pintar gráficamente... De hecho podríamos usar un gráfico de barras... si trato la variable como ORDINAL.

    Frecuencia

        ^
      7  |            X
      6  |            X
      5  |   X        X
      4  |   X        X
      3  |   X        X
      2  |   X        X      X
      1  |   X        X      X       X                                          X 
         +------------------------------------------------------------------------------         Variable: Rango salarial
            Muy poco  Algo  Más  Bastante  Mucho  Muchísimo  Muchísimo más  Mogollón

    Si la variable la trato como ORDINAL, las filas (intervalos) de la tabla de frecuencias no los escribiría, ni los dibujaría.

                     | Frecuencia | Tipos de mascota
                     | 5          | Perros
                     | 7          | Gatos
                     | 2          | Loros
                     | 1          | Chimpancés
                     | 1          | Cocodrilos
                     | 0          | Iguanas        Esto no tiene sentido. Si hay categorias sin datos, no se añaden.
                     | 0          | Tortugas       Añado hasta el infinito.
                     | 0          | Peces
                     | 0          | Agüilas

         ^
      7  |            X
      6  |            X
      5  |   X        X
      4  |   X        X
      3  |   X        X
      2  |   X        X        X
      1  |   X        X        X        X          X 
         +-----------------------------------------------         Variable: Rango salarial
            Perros   Gatos  Loros  Chimpancés  Cocodrilos



## Histograma

    La variable, al ser cuantitativa me permite graduar el eje de abscisas (X) y dibujar un histograma.


         ^
      7  |            X
      6  |            X
      5  |   X        X
      4  |   X        X
      3  |   X        X
      2  |   X        X      X
      1  |   X        X      X       X                               X 
         +------------------------------------------------------------------------------>         Variable: Salarios
            1000    1250   1500   1750   2000   2250   2500   2750   3000

                                        ^^^^^^^^^^^^^^^^^^^^^^^^^^
                                        A pesar de no haber datos, el tramo se respeta. 
                                        El histograma es un gráfico continuo, no valores discretos. 
                                        El eje horizontal está graduado
                                        Y por eso se dibuja el tramo aunque no haya datos.

    Se parecen mucho a la vista los gráficos de barras y los hisogramas.
    De hecho un histograma a priori parece un gráfico de barras con las barras muy gordas y pegadas unas a otras.
    Pero la diferencia es más sutil... y tiene que ver con esa GRADUACION DEL EJE!

    RESUMEN:
        Si tengo una variable CUANTITATIVA con bastante riqueza de valores (muchos valores distintos):
        - Creo una tabla de frecuencias con intervalos (ORDINAL)
        - La represento con un histograma (GRADUACION DEL EJE)

## Gráfico de CAJA Y BIGOTES (Boxplot)

Son extraordinarios. De los más potentes que tenemos a nuestra disposición.
Solo hay uno mejor: Gráfico de violín (Violinplot).


    Salario   | Frecuencia | Frecuencia relativa |Frecuencia aumulada relativa
    1000      | 2          | 2/16 = 12.5%     | 12.5%
    1100      | 1          | 1/16 = 6.25%     | 18.75%
    1200      | 2          | 2/16 = 12.5%     | 31.25%
    1300      | 1          | 1/16 = 6.25%     | 37.5%
    1400      | 4          | 4/16 = 25%       | 62.5%
    1500      | 2          | 2/16 = 12.5%     | 75%
    1600      | 1          | 1/16 = 6.25%     | 81.25%
    1700      | 1          | 1/16 = 6.25%     | 87.5%
    2000      | 1.         | 1/16 = 6.25%     | 93.75%
    3000      | 1          | 1/16 = 6.25%     | 100%
        -----------------
        Total | 16

    Q1 = P25        -> 1200
    Q2 = Mediana    -> 1400
    Q3 = P75        -> 1500 
    MEDIA = 1000x2 + 1100x1 + 1200x2 + 1300x1 + 1400x4 + 1500x2 + 1600x1 + 1700x1 + 2000x1 + 3000x1 / 16 = 1475

    HISTOGRAMA
                         X
                         X
      X        X         X    X
      X   X    X    X    X    X    X    X             X                                                  X 

    CAJA Y BIGOTES                      
                                    1.5 x RIC           3 x RIC
                              _______________________ _______________________

     |         +---------+----+                     |
     +---------+         |   #+---------------------+ O                                                  X
     |         +---------+----+                     |



  -----------------------------------------------------------------------------------------------------------------> Salarios €
   1000 1100 1200 1300 1400 1500 1600 1700 1800 1900 2000 2100 2200 2300 2400 2500 2600 2700 2800 2900 3000 3100
    ^                                               ^
   MINIMO     ^          ^    ^                   MAXIMO
   RAZONABLE  Q1        Q2    Q3                 RAZONABLE
                     Mediana

                                            O = Valor atípico (outlier)
                                            X = Valor extremo (extreme value)
                                            # = Media

La caja nos informa de dónde se concentra la mayor parte de la población/ de los datos más centrados -> más repreentativos
Si contiene el Q3-Q1 es que contiene al menos el 50% de los datos.
En este gráfico veo GENIAL si los datos son SIMETRICOS O NO.
Y esto era muy importante... Si eran simétricos, podría usar la media.
Si no eran simétricos, la media no me representaba bien y tenía que usar la mediana.

El ancho de la caja nos da información de la dispersión de los datos. Cuanto más ancha, más dispersos están los datos.
De hecho el ancho de la caja es Q3-Q1 = Rango intercuartílico. Y el rango intercuartílico es una medida de dispersión.

RIC = Q3-Q1 = 1500-1200 = 300 euros
Si multiplico ese RIC x 1.5, obtengo el rango de valores razonables. Los valores que estén fuera de ese rango son valores atípicos (outliers).
En nuestro caso: 300 x 1.5 = 450

Q1 - 450 = 1200 - 450 = 750 <- Mínimo razonable
Q3 + 450 = 1500 + 450 = 1950 <- Máximo razonable
Q3 + 3xRIC = 1500 + 3x300 = 2400 <- Máximo extremo

Si la caja está centrada en el carril (bigotes) y la mediana divide la caja a la mitad, los datos son simétricos.
Si la caja está desplazada hacia un lado o si la mediana no divide la caja a la mitad, los datos son asimétricos.

GRAFICO DE VIOLIN (Violinplot)

Este es guay:
Es un grafico de caja y bigote rodeado por el histograma


---

Todo esto ha sido lo relativo a el análisis de una variable (univariable). 
Pero donde las cosas se ponen interesantes es cuando empezamos a juntar variables.

Las variables las empezamos juntando 2 a 2.

Y dado que tenemos variables NOMINALES, ORDINALES y CUANTITATIVAS, tenemos 6 combinaciones posibles:
El tipo de gráfico y el tipo de análisis que podemos hacer SOLO depende del tipo de variable que tengamos.
No soy creativo. No miro a ver cuál queda mejor de los que da el EXCEL. QUE NO!
Depende el tipo de dato desde el punto de vista estadístico, HAY UN GRÁFICO U OTRO!
Esto es SOTA, CABALLO Y REY!

En general al juntar 2 variables lo que hacemos son estudios de CORRELACION.
Es decir, lo que buscamos es ver si hay relación entre las variables. 

## NOMINAL x NOMINAL

Sexo (hombre|mujer)
Lugar| Unicación

¿Hay una relación entre el SEXO y el estar en un sitio concreto? O no hay relación!

                  50%            50% <- Dato que conozco de antemano
                Hombre          Mujer          TOTAL
    Autobus       25/23          25/27         50 personas
    Tren         100/105         100/95        200 personas
    Mercado       40/43          40/37         80 personas
    Médico        25/1           25/49         50 personas

Esto es lo que espero a PRIORI!
Pero ahora entro y cuento la realidad.

Al comparar los datos esperados con los datos reales... AQUELLO ME LLAMA LA ATENCION O NO?

La estadística ME AYUDA A PONER EL FOCO EN LO QUE ME LLAMA LA ATENCION, que no encaja!
No da una explicación. Para eso tengo que analizar desde otros puntos de vista.
A lo que llego a una conclusión: En GINECOLOGIA hay más mujeres que hombres.
A lo que llego a una concusión: En GINECOLOGIA hay más mujeres que hombres.
En el autobus no se encuentra relación entre el sexo y el lugar donde se encuentra. En el tren tampoco. En el mercado tampoco. Pero en el médico (ginecólogo) si hay relación.

Lo que hemos montado es lo que en estadística se llama una tabla de contingencia. Es como una tabla de frecuencias, pero con 2 variables. Y en este caso, las 2 variables son NOMINALES.
Esta tabla se puede representar gráficamente:
- Barras apiladas (Stacked bar chart)
- Barras agrupadas (Grouped bar chart)
En realidad hay otra que adoramos (son de esas nuevas que solo algunos programas hoy en día generan: HEATMAP)
Es la misma tabla, pero con las casillas pintadas de colores, en base a la cantidad de datos de esa casilla.

## NOMINAL x ORDINAL

Si recordaís, dijimos que por definición TODA VARIABLE ORDINAL es también NOMINAL. 
- Siempre puedo hacer el mismo estudio en este caso que si tuviera 2 variables NOMINALES:
  - Generar la tabla de contingencia
  - Generar el gráfico de barras apiladas o agrupadas | Heatmap

Hay una cosa más que podemos hacer en este caso.
Porque a la variable ORDINAL le puedo calcular la mediana.
Y lo que puedo es lo que se llama un ESTUDIO DE COMPARACION DE MEDIANAS.
Calculo la MEDIANA de la variable ORDINAL para cada categoría de la variable NOMINAL.
Ejemplo:

    2 variables:
        País
        Nivel de estudios

                        Primaria        Secundaria        Bachillerato    Grado        Master    Doctorado
        España           1000             2000              4000          3000           500      200   
        China            50000            300000            500000        4000000        50000    20000
        Suecia           400              300               200           100            50       10
        ....

Podría calcular la mediana de cada PAIS
            Mediana
    España. Bachillerato
    China.  Grado
    Suecia. Secundaria

    En general, a la vista de estos datos, parece que los chinos tienen un nivel de estudios más alto que los españoles y los suecos.
    Si todas las categorias tienen la misma mediana entonces concluyo que los grupos son más o menos iguales.. y que por ende no hay relación entre las variables.
    Si las medianas son diferentes, entonces concluyo que los grupos son diferentes.. y que por ende hay relación entre las variables.

## NOMINAL x CUANTITATIVA

Como por definición toda variable CUANTITATIVA es también ORDINAL, puedo hacer el mismo estudio que si tuviera una variable NOMINAL y una variable ORDINAL.
Es decir:
    - Generar la tabla de contingencia
    - Generar el gráfico de barras apiladas o agrupadas | Heatmap
    - Hacer un estudio de comparación de medianas
    - Hacer un estudio de comparación de medias
  
Adicionalmente hay algunos gráficos que puedo usar:
- Boxplot múltiple
- Histograma apilado (stacked histogram)
- Pirámide de población (Population pyramid) SOLO SI LA VARIABLE NOMINAL TIENE SOLO 2 CATEGORIAS (Hombre|Mujer)
  2 histogramas que se juntan por la base 

## ORDINAL x ORDINAL

    Véase ORDINAL X NOMINAL

## ORDINAL x CUANTITATIVA

    Véase NOMINAL X CUANTITATIVA

## CUANTITATIVA x CUANTITATIVA

    Aquí hay un gráfico muy especial y muy diferente a todos los demás. Es el SCATTERPLOT (Gráfico de dispersión | Nube de puntos)
    En ese gráfico NO SE REPRESENTAN FRECUENCIAS. Representa a TODOS los individuos de la población.
    Cada sujeto/caso/dato es un punto en el gráfico. Y la posición de ese punto depende de los valores de las 2 variables.


    Variable1
        ^
        |
        |  X
        |
        |               X
        |       X
        |
        +------------------------------------> Variable 2

    Si los puntos se distribuyen de forma aleatoria, no hay relación entre las variables.
    Si los puntos forman algún patrón (linea recta, curva, etc) entonces hay relación entre las variables.

NO HAY OTRO GRAFICO CON 2 VARIABLES CUANTITATIVAS QUE NO SEA EL SCATTERPLOT.

---

En cualquier CUADRO DE MANDO lo que meteremos serán ESTAS COSAS!

Estadísticos:
    Mínimo, Máximo
    Mediana, Media / Dispersión(desviación típica, Rango intercuartílico)
    Percentiles, Cuartiles, Deciles
    Recuentos (COUNT) / TOTALES (SUM)
Gráficos:
    1 variable: Barras, Sectores, Histograma, Boxplot
    2 variables: Barras apiladas, Barras agrupadas, Heatmap, Boxplot múltiple, Scatterplot, Histograma apilado, Pirámide de población
Tablas de frecuencia
Tablas de contingencia

Y SE ACABÓ. No hay muchas más cosas que metyer en cuadros de mando.

Hay cositas especiales de las que ya os hablaré:
- Diagramas de Sankey
- Nubes de palabras
- KPIs (Indicadores) = ESTADISTICO + MEDIDA OBJETIVO
- Mapas sobre los que se pueden pintar datos (Mapas de calor, Mapas de coropletas, Mapas de burbujas, Mapas de puntos)

Cuando planteemos esos cuadros de mando, habrá varias preguntas que deberemos hacernos:
- Para quién es el cuadro de mando?

  - Operaciones   ->  Quiero tomar deciones sobre el AHORA                      Ven con un plazo de horas/días
  - Estrategia    ->  Quiero tomar decisiones sobre el FUTURO                   Ver con un plazo de meses
  - Dirección     ->  Quiero tomar decisiones sobre el FUTURO de la empresa     Ver con un plazo de años

Lo siguente es ir estableciendo una JERARQUIA entre los componentes que voy a meter en el cuadro de mando.
Empiezo por un análisis muy general de los datos y voy bajando de nivel hasta llegar a un análisis muy concreto de los datos.
Aquí hablamos de técnicas de drill down y drill up. Y de técnicas de filtrado de datos.

Por ahora NO OS DARÉ MAS LA CHAPA CON COSAS DE ESTADISTICA!
Ejercicio para poner todo esto en práctica.

En un proyecto, lo primero es:
- Entender los datos que tengo
- Estudiar su calidad -> Tratamiento que requieren  <-----------------------------------+
- Ver cómo los guardo                                                                   |
                                                                                        |
- Pensar en lo que quiero producir: A qué preguntas quiero responder con esos datos? ---+

- Automatizar en la medida de lo posible ese tratamiento de los datos: ETLs
  Vamos a estar teniendo datos actualizados de continuo. No podemos hacer tratamiento a MANO de los datos. Tenemos que automatizarlo.

---

# Automatizar

Crear una máquina (o cambiar el comportamiento de una mediante PROGRAMAS) que haga el trabajo que antes hacía un humano con sus manos.

Puedo automatizar el lavado de la ropa (LAVADORA)... lavadora a la que incluso le puedo cambiar el comportamiento con PROGRAMAS ( PROGRAMAS DE LAVADO: frio, prendas delicadas, algodón 90º-60º)

En nuestro caso, no vamos a inventar MAQUINAS... la máquina ya la tenemos = COMPUTADORA
Lo que haremos son PROGRAMAS, escritos con LENGUAJES DE PROGRAMACION o Con ayuda de OTROS PROGRAMAS .

La creación de ETLs es trabajo de programadores.

Dentro de un proyecto de BI, necesitamos un gran repertorio de profesionales con diferentes perfiles:
- Analistas de negocio (Business Analysts)
- Analistas de datos (Data Analysts)
- Programadores de ETLs (ETL Developers)
- Administradores de bases de datos (Database Administrators)

Yo no puedo hacerlo todo... a no ser que sea algo muy simple... y con poca trasformación de los datos.