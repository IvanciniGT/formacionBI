# Práctica · Preparación de los datos de un call-center

**Sesión 3 · segundo bloque · 1 h 45 min**

---

## El encargo

Una empresa de telecomunicaciones ha hecho encuestas telefónicas de satisfacción a
sus clientes. El fichero `encuestas-callcenter.csv` contiene **1.214 respuestas**,
anotadas a mano por los operadores del call-center a lo largo de dos años.

Dirección quiere un cuadro de mando con la satisfacción de los clientes, para
decidir dónde invertir en atención al cliente el año que viene.

Vuestro trabajo **no es** hacer el cuadro de mando. Es lo de antes, que es lo que
de verdad cuesta: **dejar los datos en condiciones**.

> Estos mismos datos son los que modelaréis mañana y los que alimentarán el cuadro
> de mando de la semana que viene. Lo que hagáis hoy lo vais a arrastrar hasta el
> final del curso.

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

## Lo que hay que entregar

### 1 · La ficha de preparación

Una fila por columna del fichero, con estas seis decisiones:

| Columna | Tipo estadístico | Tipo informático y bytes | Problemas detectados | Transformación propuesta | ¿Se recodifica? |
|---|---|---|---|---|---|
| | Nominal / Ordinal / Cuantitativo | Entero corto, texto, fecha… | | | Sí / No |

Para el **tipo estadístico**, acordaos de la pregunta que decide: ¿tiene unidad de
medida? Y cuidado, que hay al menos una columna que parece cuantitativa y no lo es.

Para el **tipo informático**, elegid el tipo más pequeño que sirva. Justificad el
tamaño: cuántos valores distintos necesitáis representar.

### 2 · El inventario de problemas de calidad

Para cada problema que encontréis, anotad: **qué columna afecta, cuántas filas,
y qué proponéis hacer**. No vale «limpiar»: hay que decir cómo.

### 3 · El tratamiento de los valores perdidos

Hay huecos en el fichero. **No todos significan lo mismo.** Separadlos en:

- **No aplica** — la pregunta no tenía sentido para ese cliente. No genera incertidumbre.
- **No informado** — el dato existe pero no lo tenemos. Sí genera incertidumbre.

Y después responded a estas tres preguntas **con un intervalo**, no con un número:

```
¿Qué porcentaje de clientes había tenido alguna incidencia previa?
¿Qué porcentaje de clientes recomendaría la compañía?
¿Cuál es la satisfacción media de los clientes que han tenido incidencias?
```

### 4 · El cálculo del ahorro

Estimad cuánto ocupa una fila **tal y como viene el fichero**, y cuánto ocuparía
**después de vuestras transformaciones**. Expresad el ahorro en porcentaje.

Después multiplicadlo por los 2.000.000 de encuestas reales que tiene la empresa, y
traducidlo a euros con la cuenta del coste de almacenamiento empresarial que vimos:
**30.000 € por cada 2 TB**.

---

## Cómo trabajamos

```
15'   Presentación del caso y primer vistazo al fichero
45'   Trabajo en grupos: fichas 1, 2 y 3
30'   Puesta en común, grupo por grupo
15'   El cálculo del ahorro y cierre
```

---

## Las preguntas que os tenéis que hacer

1. ¿Qué mide realmente esta columna?
2. ¿Tiene unidad de medida?
3. ¿Cuántos valores distintos puede tomar?
4. ¿Este hueco es «no aplica» o «no informado»?
5. ¿Este valor es raro o es imposible?
6. ¿Puedo deducir esta columna a partir de otra?
7. ¿Qué decisión de negocio se va a tomar con este dato?
