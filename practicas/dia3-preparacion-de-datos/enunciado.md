# Práctica: preparar los datos de un call-center

**Preparación de los datos para su análisis**

---

## El encargo

Una empresa de telecomunicaciones ha hecho encuestas telefónicas de satisfacción a
sus clientes. El fichero `encuestas-callcenter.csv` contiene **1.214 respuestas**,
anotadas a mano por los operadores del call-center a lo largo de dos años.

Dirección quiere un cuadro de mando con la satisfacción de los clientes, para
decidir dónde invertir en atención al cliente el año que viene.

Tu trabajo **no es** hacer el cuadro de mando. Es lo de antes, que es lo que de
verdad cuesta: decidir cómo hay que dejar estos datos para que ese cuadro de mando
se pueda construir y no mienta.

> Estos mismos datos son los que modelaremos en la próxima sesión y los que
> alimentarán el cuadro de mando de la semana que viene. Lo que decidas hoy lo vas a
> arrastrar hasta el final del curso.

---

## La regla

**Hoy no se toca ni una sola celda del fichero.**

Hoy se diagnostica, no se opera. Puedes filtrar, ordenar, hacer tablas dinámicas y
contar todo lo que quieras. Lo que no puedes es corregir un dato.

Lo que entregas es un **documento de decisiones**, no un fichero limpio.

---

## El fichero

Separador `;`, codificación UTF-8, quince columnas:

```
id_encuesta                 Identificador de la encuesta
dni                         DNI del cliente
fecha_llamada               Fecha en que se hizo la encuesta
hora_llamada                Hora de la llamada
comunidad                   Comunidad autónoma del cliente
edad                        Edad del cliente
sexo                        Sexo del cliente
producto_contratado         Producto que tiene contratado
importe_ultima_factura      Importe de su última factura
satisfaccion_1_10           Valoración del servicio, de 1 a 10
recomendaria                Si recomendaría la compañía
num_incidencias_previas     Incidencias que había tenido antes de la llamada
tipo_primera_incidencia     Tipo de la primera incidencia que tuvo
duracion_llamada_seg        Duración de la llamada, en segundos
operador                    Operador que hizo la llamada
```

---

## Qué hay que entregar

### 1 · La ficha de preparación

Una fila por cada columna del fichero, con estas cinco decisiones:

| Columna | Tipo estadístico | Tipo informático y bytes | Problemas detectados | Transformación propuesta |
|---|---|---|---|---|
| | Nominal · Ordinal · Cuantitativo | El más pequeño que sirva | Cuál, y a cuántas filas afecta | Qué harías, y si se recodifica |

**Ejemplo resuelto.** Así es como tiene que quedar cada fila:

```
Columna              comunidad

Tipo estadístico     NOMINAL
                     Es un nombre. Sirve para clasificar clientes, pero no hay
                     ningún orden entre las comunidades ni unidad de medida.

Tipo informático     Ahora: texto, hasta 21 bytes por fila (unos 15 de media).
                     Propuesto: entero corto, 1 byte.
                     Sólo hay 5 comunidades reales y en 1 byte caben 255.

Problemas            24 grafías distintas para 5 comunidades.
                     Varía mayúsculas, acentos, guiones, abreviaturas y hay
                     alguna con un espacio al final.

Transformación       Conformar a un catálogo único de 5 valores y recodificar
                     a número. Ahorro: de ~15 bytes a 1 por fila.
```

Para el **tipo estadístico**, la pregunta que decide es siempre la misma: ¿tiene
unidad de medida? Cuidado, que hay al menos una columna que parece cuantitativa y
no lo es.

Para el **tipo informático**, elige el tipo más pequeño que sirva, y justifica el
tamaño diciendo cuántos valores distintos necesitas poder representar.

### 2 · Los valores perdidos

Hay huecos en el fichero. **No todos significan lo mismo.** Sepáralos en dos grupos:

- **No aplica** — la pregunta no tenía sentido para ese cliente. No genera incertidumbre.
- **No informado** — el dato existe pero no lo tenemos. Sí genera incertidumbre.

Y con eso hecho, responde a esta pregunta **con un intervalo**, no con un número:

```
¿Qué porcentaje de clientes había tenido alguna incidencia previa?
```

### 3 · El cálculo del ahorro

Estima cuánto ocupa una fila **tal y como viene el fichero**, y cuánto ocuparía
**después de tus transformaciones**. Expresa el ahorro en porcentaje.

| Tipo | Tamaño |
|---|---|
| Booleano | 1 byte |
| Entero corto, hasta 255 | 1 byte |
| Entero mediano, hasta 65.535 | 2 bytes |
| Entero largo | 4 bytes |
| Decimal corto | 4 bytes |
| Fecha | 4 bytes |
| Fecha y hora | 8 bytes |
| Texto | 1 byte por carácter; 2 si lleva tilde o ñ |

### 4 · El cuadro de mando

Esto se hace **al final**, con la ficha ya rellena. Ahora que sabes lo que hay
dentro del fichero, y no antes:

**a) Escribe cinco preguntas** que dirección debería poder responder con el cuadro
de mando. Del estilo de *«¿la satisfacción es distinta según…?»* o *«¿ha mejorado…
a lo largo del tiempo?»*.

**b) Señala las columnas que no sirven para ninguna de tus cinco preguntas.**
Prepararlas costaría lo mismo que las demás. ¿Merece la pena cargarlas?

**c) ¿Qué columnas nuevas habría que calcular?** No están en el fichero, pero salen
de lo que ya hay. Piensa en qué necesitarías para poder responder a tus preguntas.

**d) ¿Y qué te falta que no está y no se puede calcular?** Aquí la respuesta
incómoda es válida: puede que alguna de tus cinco preguntas **no se pueda contestar
con estos datos**, y eso también es un resultado del análisis.

**e)** Por último, mirando tu ficha: ¿qué columnas serían **dimensiones** —las que
describen y sirven para filtrar y agrupar— y cuáles **hechos** —las que se miden—?
¿Y qué harías con `satisfaccion_1_10`?

> El apartado (e) es por donde empezamos la próxima sesión.

---

## En qué orden trabajar

Hazlas en **este** orden, no en el del fichero. Las cinco primeras son las que
enseñan; el resto es refuerzo.

```
 1  satisfaccion_1_10
 2  num_incidencias_previas   ┐ van juntas
 3  tipo_primera_incidencia   ┘
 4  comunidad                   ← la del ejemplo resuelto
 5  importe_ultima_factura
 6  edad
 7  dni
 8  fecha_llamada
 9  sexo
10  recomendaria
11  producto_contratado
12  operador
13  duracion_llamada_seg
14  hora_llamada
15  id_encuesta
```

Si no llegas a las quince, no pasa nada. Si no llegas a las cinco primeras, sí.

---

## Cómo perfilar una columna

**La tabla dinámica es la herramienta.** Te da, en menos de un minuto, todos los
valores distintos de una columna con su recuento:

```
1 · Selecciona los datos y ve a Insertar › Tabla dinámica
2 · Arrastra la columna que quieras al área de FILAS
3 · Arrastra esa MISMA columna al área de VALORES
4 · Te sale la lista de valores distintos y cuántas veces aparece cada uno
```

Es la técnica que vas a usar el resto de tu vida profesional para lo mismo:
saber qué hay realmente dentro de una columna antes de fiarte de ella.

Para lo demás:

| Qué quieres saber | Cómo |
|---|---|
| Cuántos huecos hay | `CONTAR.BLANCO` |
| Cuántos valores fuera de rango | `CONTAR.SI` con criterio, o un filtro |
| Si hay valores repetidos | Formato condicional › Reglas › Duplicados |

⚠️ **Dos excepciones.** En `fecha_llamada` y en `importe_ultima_factura` no
intentes contar con fórmulas: recorre la columna de arriba abajo con la vista y
anota lo que veas raro. Ahí lo que importa no es cuántos hay, sino **haberlos
visto**.

---

## Las preguntas que te tienes que hacer

Ante cada columna, siempre las mismas siete:

1. ¿Qué mide realmente esta columna?
2. ¿Tiene unidad de medida?
3. ¿Cuántos valores distintos puede tomar?
4. ¿Este hueco es «no aplica» o «no informado»?
5. ¿Este valor es raro o es imposible?
6. ¿Puedo deducir esta columna a partir de otra?
7. ¿Qué decisión de negocio se va a tomar con este dato?

