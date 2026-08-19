# Solución comentada de la práctica

**Preparación de los datos de un call-center**

---

## Qué se buscaba

La práctica no iba de limpiar datos. Iba de **decidir** qué habría que hacer con
ellos, que es lo que de verdad consume el tiempo de un proyecto de Business
Intelligence.

Y de comprobar una cosa concreta: que cada columna tiene **dos naturalezas a la
vez**, la estadística y la informática, y que hay que llevarlas por separado.
Confundirlas es el origen de la mayoría de los cuadros de mando que mienten.

En las páginas siguientes está la ficha completa de las quince columnas, el
tratamiento de los valores perdidos y el cálculo del ahorro. Si tu respuesta no
coincide con alguna, lo importante no es el resultado: es si el razonamiento se
sostiene.

---

## Las cinco columnas que decidían la práctica

### `satisfaccion_1_10` · la que parecía cuantitativa y no lo era

Es la columna clave, y por eso iba la primera.

Son números del 1 al 10. Informáticamente, un entero corto de 1 byte. Pero
**estadísticamente es ORDINAL**, porque no hay unidad de medida: lo que para un
cliente es un 7 para otro es un 5, y la distancia entre un 3 y un 4 no tiene por
qué ser la misma que entre un 8 y un 9.

La consecuencia es la que importa:

> Excel te deja calcular su media. Y esa media no significa nada.

Lo correcto es la **mediana**, acompañada de la distribución completa. Una media de
5,5 puede venir de que todo el mundo puntúe 5 y 6, o de que la mitad puntúe 1 y la
otra mitad 10. Son dos negocios opuestos y la media no los distingue.

Tiene **200 huecos**, un 16 %. Son «no informado».

### `num_incidencias_previas` y `tipo_primera_incidencia` · los huecos que no significan lo mismo

Iban juntas porque solo se entienden juntas. Si miras los huecos de
`tipo_primera_incidencia` sin mirar la columna de al lado, los cuentas todos como
carencia. Y no lo son:

| Situación | Filas | Qué significa el hueco |
|---|---|---|
| `num_incidencias_previas` = 0, tipo vacío | **388** | **No aplica.** Ese cliente no tuvo incidencias, así que no hay primera incidencia que informar. El hueco es información, no ausencia. |
| `num_incidencias_previas` > 0, tipo vacío | **134** | **No informado.** Tuvo incidencias y no sabemos de qué tipo. |
| `num_incidencias_previas` vacío | **109** | **No informado** por partida doble: ni sabemos cuántas tuvo ni de qué tipo. |

Sólo 243 de los 631 huecos generan incertidumbre. Los otros 388 son un dato
perfectamente conocido que resulta que se escribe dejando la celda en blanco.

### `comunidad` · las 24 grafías

Cinco comunidades reales escritas de veinticuatro formas: mayúsculas, minúsculas,
acentos, guiones, abreviaturas y alguna con un espacio al final.

Ocupa de media **11,7 bytes por fila** como texto. Recodificada a un entero corto,
**1 byte**. Un 91 % menos, y de paso deja de ser imposible agrupar por comunidad.

### `importe_ultima_factura` · cuatro formatos y una trampa

| Formato | Filas |
|---|---|
| Coma decimal: `154,60` | 640 |
| Punto decimal: `154.60` | 279 |
| Con símbolo: `154,60 €` | 164 |
| Vacío | 90 |
| **Sin separador: `15460`** | **41** |

Los cuatro primeros dan guerra pero se ven venir. **El quinto es el peligroso**: son
importes guardados en céntimos. No dan error, se convierten a número
perfectamente, y multiplican la facturación por cien.

Se detectan porque no encajan con el negocio: una factura de móvil de 2.437 € entre
otras de 19 a 190 € no es un cliente premium, es un fallo de unidad.

### `dni` · el identificador con dos sorpresas

No es un dato, es un identificador. Nominal, y no se opera con él.

**Sorpresa 1:** hay **14 DNIs repetidos**. El mismo cliente encuestado dos veces. Si
no se detectan, todos los porcentajes por cliente salen mal.

**Sorpresa 2:** la letra se calcula a partir del número, así que guardarla es
guardar información derivada. Guardando sólo el número: de 9 bytes a 4.

---

## La ficha completa

| Columna | Estadístico | Informático propuesto | Problemas | Transformación |
|---|---|---|---|---|
| `id_encuesta` | Nominal | Entero largo · 4 B | Ninguno | Ninguna. Es la clave |
| `dni` | Nominal | Entero largo · 4 B | 14 repetidos; letra derivable | Deduplicar; guardar sólo el número |
| `fecha_llamada` | Cuantitativa de intervalo | Fecha · 4 B | 3 formatos, más algunas en formato americano | Normalizar a fecha única |
| `hora_llamada` | Cuantitativa de intervalo | Entero corto · 1 B | Ninguno | Recodificar a franja horaria |
| `comunidad` | Nominal | Entero corto · 1 B | 24 grafías para 5 valores | Conformar y recodificar |
| `edad` | Cuantitativa de razón | Entero corto · 1 B | 163 imposibles, 43 vacías | Imposibles a «no informado» |
| `sexo` | Nominal | Entero corto · 1 B | 8 grafías para 2 valores | Conformar y recodificar |
| `producto_contratado` | Nominal | Entero corto · 1 B | 10 grafías para 5 productos | Conformar y recodificar |
| `importe_ultima_factura` | Cuantitativa de razón | Decimal corto · 4 B | 4 formatos, 41 en céntimos, 90 vacíos | Normalizar unidad y separador |
| `satisfaccion_1_10` | **Ordinal** | Entero corto · 1 B | 200 vacíos | Mantener escala. **No promediar** |
| `recomendaria` | Nominal | Entero corto · 1 B | 9 grafías para 3 estados | Conformar y recodificar |
| `num_incidencias_previas` | Cuantitativa de razón | Entero corto · 1 B | 109 vacíos | Marcar «no informado» |
| `tipo_primera_incidencia` | Nominal | Entero corto · 1 B | 522 vacíos de dos clases distintas | Separar «no aplica» de «no informado» |
| `duracion_llamada_seg` | Cuantitativa de razón | Entero mediano · 2 B | 140 valores extremos | Conservar y analizar aparte |
| `operador` | Nominal | Entero corto · 1 B | 7 grafías para 6 personas | Conformar y recodificar |

Sobre `duracion_llamada_seg` conviene detenerse: hay llamadas de 4 segundos y de más
de dos horas. **No son errores de tecleo, son casos reales**: una llamada que se
cortó y otra en la que nadie colgó. No se borran. Se separan y se estudian aparte,
porque probablemente dicen algo sobre el proceso.

---

## El intervalo de los valores perdidos

La pregunta era qué porcentaje de clientes había tenido alguna incidencia previa.
Y había que responder con un intervalo:

```
Con incidencias, seguro   717 clientes
Sin incidencias, seguro   388 clientes
No informado              109 clientes
                         ----
                         1.214

Mínimo:  717 / 1.214 = 59,1 %      si ninguno de los 109 tuvo incidencias
Máximo:  826 / 1.214 = 68,0 %      si todos las tuvieron

    Entre el 59 % y el 68 % de los clientes había tenido alguna incidencia.
```

Nueve puntos de horquilla. Y eso es lo honesto: cualquier cifra exacta que se dé
aquí está inventando el dato de 109 personas.

Fíjate en que los 388 «no aplica» **no abren el intervalo**. Están dentro del dato
conocido. Por eso había que separarlos: si se tratan como huecos, la horquilla se va
del 59 % al 90 % y el indicador deja de servir para nada.

---

## El cálculo del ahorro

| | Por fila |
|---|---|
| Tal y como viene el fichero, todo texto | ~130 bytes |
| Después de las transformaciones | ~30 bytes |
| **Ahorro** | **~77 %** |

Sobre los 2.000.000 de encuestas reales de la empresa: de unos 260 MB a unos 60 MB.

Y aquí viene lo que de verdad había que ver: **200 MB no son nada**. Con la cuenta
del coste de almacenamiento empresarial, unos 3.000 €. Está bien, pero no cambia
ningún proyecto.

Entonces, ¿para qué tanto trabajo?

Porque el espacio es la menor de las ventajas. Si los datos ocupan una cuarta parte:

- Se leen del disco en una cuarta parte de tiempo.
- Se cargan en memoria en una cuarta parte de tiempo.
- Viajan por la red en una cuarta parte de tiempo.
- Y se procesan en la CPU mucho más rápido, porque comparar dos enteros de un byte
  no se parece en nada a comparar dos cadenas de texto de veinte caracteres.

Cambia la pregunta: en vez de dos millones de encuestas, imagina dos mil millones de
registros de tráfico de red que hay que recorrer enteros cada noche. Ahí esos 100
bytes por fila son la diferencia entre un proceso de veinte minutos y uno de tres
horas. Y esa diferencia sí decide si un proyecto es viable.

---

## Las cinco ideas que quedan

**1 · Cada columna tiene dos naturalezas.** La estadística —nominal, ordinal,
cuantitativa— y la informática —texto, entero, fecha—. Van por separado y hay que
decidir las dos. Casi todas las herramientas del mercado sólo te preguntan por la
segunda; la primera la tienes que llevar tú.

**2 · Un hueco no siempre es una carencia.** Distinguir «no aplica» de «no
informado» es lo que separa un indicador honesto de uno inventado. Y cuando hay
incertidumbre, la respuesta correcta es un intervalo.

**3 · Un valor raro y un valor imposible no se tratan igual.** Una edad de 88 años
es rara y probablemente cierta. Una de −3 es imposible. Una llamada de dos horas es
rara, real e interesante.

**4 · Ninguna de estas decisiones era técnica.** ¿El importe en céntimos o en euros?
¿Las dos encuestas del mismo DNI son una o dos? ¿La edad imposible se descarta o se
marca? Todas necesitan que alguien de negocio responda. Ésa es la razón de que la
preparación de los datos sea lenta, y de que no se pueda encargar sin más al
departamento de informática.

**5 · Esto es donde se va el proyecto.** Quince columnas y mil filas han ocupado una
sesión entera. En un proyecto real hay cientos de columnas y millones de filas, y
esta fase se lleva en torno a la cuarta parte del esfuerzo total. El cuadro de mando
que se ve al final es, con diferencia, la parte barata.

---

## Y lo que viene ahora

Con la ficha hecha, la siguiente pregunta es cómo llevar esta tabla a un almacén de
datos. Y ahí las quince columnas se reparten en dos grupos: las que describen —que
serán **dimensiones**— y las que se miden —que serán **hechos**.

Con una sorpresa que ya puedes anticipar: `satisfaccion_1_10` es un número y va a
acabar siendo una **dimensión**, precisamente porque es ordinal y no se puede
promediar.
