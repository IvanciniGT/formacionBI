# Práctica · Preparación de los datos de un call-center

**Sesión 3 · segundo bloque · trabajo individual**

---

## El encargo

Una empresa de telecomunicaciones ha hecho encuestas telefónicas de satisfacción a
sus clientes. El fichero `encuestas-callcenter.csv` contiene **1.214 respuestas**,
anotadas a mano por los operadores del call-center a lo largo de dos años.

Dirección quiere un cuadro de mando con la satisfacción de los clientes, para
decidir dónde invertir en atención al cliente el año que viene.

Tu trabajo **no es** hacer el cuadro de mando. Es lo de antes, que es lo que de
verdad cuesta: decidir cómo hay que dejar estos datos.

> Estos mismos datos son los que modelaremos mañana y los que alimentarán el cuadro
> de mando de la semana que viene. Lo que decidas hoy lo vas a arrastrar hasta el
> final del curso.

---

## LA REGLA

**Hoy no se toca ni una sola celda del fichero.**

Hoy se diagnostica, no se opera. Puedes filtrar, ordenar, hacer tablas dinámicas y
contar todo lo que quieras. Lo que no puedes es corregir un dato.

El entregable es un **documento de decisiones**, no un fichero limpio.

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

## En qué orden trabajar

Hazlas en **este** orden, no en el del fichero. Las cinco primeras son las que
enseñan; el resto es refuerzo.

```
 1  satisfaccion_1_10
 2  num_incidencias_previas   ┐ van juntas
 3  tipo_primera_incidencia   ┘
 4  comunidad
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

## Cómo perfilar cada columna

Una **tabla dinámica** con la columna en filas y en valores te da, en menos de un
minuto, todos los valores distintos con su recuento. Es la herramienta correcta y la
vas a usar el resto de tu vida profesional. Lo veremos en pantalla con `comunidad`
antes de empezar.

Para lo demás: `CONTAR.BLANCO` para los huecos, `CONTAR.SI` o un filtro para los
valores fuera de rango, y **Formato condicional › Duplicados** para los repetidos.

⚠️ **Dos excepciones.** En `fecha_llamada` y en `importe_ultima_factura` no
intentes contar con fórmulas: recorre la columna de arriba abajo con la vista y
anota lo que veas raro. Ahí lo que importa no es cuántos hay, es **haberlos visto**.

---

## Lo que hay que entregar

### 1 · La ficha de preparación

Una fila por columna, con estas cinco decisiones:

| Columna | Tipo estadístico | Tipo informático y bytes | Problemas detectados | Transformación propuesta |
|---|---|---|---|---|
| | Nominal / Ordinal / Cuantitativo | El más pequeño que sirva | Con cuántas filas afecta | Y si se recodifica o no |

Para el **tipo estadístico**, la pregunta que decide es siempre la misma: ¿tiene
unidad de medida? Cuidado, que hay al menos una columna que parece cuantitativa y
no lo es.

Para el **tipo informático**, elige el tipo más pequeño que sirva, y justifica el
tamaño: cuántos valores distintos necesitas poder representar.

### 2 · Los valores perdidos

Hay huecos en el fichero. **No todos significan lo mismo.** Sepáralos en:

- **No aplica** — la pregunta no tenía sentido para ese cliente. No genera incertidumbre.
- **No informado** — el dato existe pero no lo tenemos. Sí genera incertidumbre.

Y con eso, responde a esta pregunta **con un intervalo**, no con un número:

```
¿Qué porcentaje de clientes había tenido alguna incidencia previa?
```

### 3 · El cálculo del ahorro

Estima cuánto ocupa una fila **tal y como viene el fichero**, y cuánto ocuparía
**después de tus transformaciones**. Expresa el ahorro en porcentaje.

| Tipo | Tamaño |
|---|---|
| Booleano | 1 byte |
| Entero corto (hasta 255) | 1 byte |
| Entero mediano (hasta 65.535) | 2 bytes |
| Entero largo | 4 bytes |
| Decimal corto | 4 bytes |
| Fecha | 4 bytes |
| Fecha y hora | 8 bytes |
| Texto | 1 byte por carácter simple, 2 si lleva tilde o ñ |

---

## Si acabas antes

Con tu ficha delante, contesta a esto:

> Si tuvieras que llevar esta tabla a un almacén de datos para analizar la
> satisfacción, ¿qué columnas serían **dimensiones** y cuáles **hechos**?
> ¿Y qué harías con `satisfaccion_1_10`?

Es literalmente por donde empezamos mañana.

---

## Las preguntas que te tienes que hacer

1. ¿Qué mide realmente esta columna?
2. ¿Tiene unidad de medida?
3. ¿Cuántos valores distintos puede tomar?
4. ¿Este hueco es «no aplica» o «no informado»?
5. ¿Este valor es raro o es imposible?
6. ¿Puedo deducir esta columna a partir de otra?
7. ¿Qué decisión de negocio se va a tomar con este dato?
