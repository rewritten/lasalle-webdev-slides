---
footer: |
    ![h:50px Consorci](../themes/consorci-logo.png)
    ![h:50px SEPE](../themes/sepe-logo.jpeg)
    Elementos de programación estructurada
    ![h:50px La Salle URL](../themes/lasalle-logo.png)
paginate: true
marp: true
---

<!-- 
_class: front
_paginate: false
_footer: |
    ![h:100px Consorci](../themes/consorci-logo.png)
    ![h:100px SEPE](../themes/sepe-logo.jpeg)
    ![h:100px La Salle URL](../themes/lasalle-logo.png)
-->

# Fundamentos de programación en Python

## Módulo 3 :: Temáticas intermedias en Python :: Procesamiento de datos

Profesor: Saverio Trioni

> Convocatoria de 2023 de los Programas de formación profesional para el empleo, de
> especialidades de la oferta de formación no formal, para personas trabajadoras ocupadas,
> que promueve el Consorcio para la Formación Continua de Cataluña (ref. BDNS 709943)

---

<!--
_class: chapter-front
_paginate: false
header: Funciones sobre iterables
-->

# Funciones estándar sobre iterables

---

## Funciones de construcción de iterables

Los tipos iterables en ptyhon tienen funciones de construcción que aceptan otros iterables como argumento.

```python
iterable = [1, 2, 3]

# Crear una lista a partir de un iterable
list(iterable) # [1, 2, 3]

# Crear un conjunto a partir de un iterable
set(iterable) # {1, 2, 3}

# Crear una tupla a partir de un iterable
tuple(iterable) # (1, 2, 3)
```

---

## Transformación de iterables

Python provee una función `map` que aplica una función a cada elemento de un iterable.

```python
def cuadrado(x):
    return x ** 2

iterable = [1, 2, 3]

# Aplicar la función `cuadrado` a cada elemento de `iterable`

cuadrados = map(cuadrado, iterable)
next(cuadrados) # 1
next(cuadrados) # 4
next(cuadrados) # 9
next(cuadrados) # StopIteration!!
```

---

## Filtrado de iterables

Python provee una función `filter` que aplica una función de filtro a cada elemento de un iterable.

```python
def es_impar(x):
    return x % 2 == 1

iterable = iter([1, 2, 3, 4, 5])

# Conservamos los elementos impares de `iterable`
impares = filter(es_impar, iterable)
next(impares) # 1
next(impares) # 3
next(impares) # 5
next(impares) # StopIteration!!
```

La función `filter` **mantiene** los valores que cumplen la condición.

---

## Reordenación

Hay dos funciones que retornan un iterable en un orden concreto:

```python
iterable = [3, 1, 2, 4, 5]
```

La función `sorted` retorna una **lista** ordenada de menor a mayor.

```python
sorted(iterable) # [1, 2, 3, 4, 5]
```

La función `reversed` acepta solamente ciertos tipos de iterables (que se puedan recorrer en orden inverso) y retorna una lista.

```python
reversed(iterable) # [5, 4, 2, 1, 3]
```

Por ejemplo, no se puede invertir un iterador infinito:

```python
from itertools import count

reversed(count()) # TypeError: 'count' object is not reversible
```

---

## Indexado de iterables

Dado un enumerable, podemos obtener tuplas con el índice del orden:

```python
iterable = iter([10, 9, 8, 7])

# Obtener tuplas con el índice y el valor
indexed = enumerate(iterable)
next(indexed) # (0, 10)
next(indexed) # (1, 9)
next(indexed) # (2, 8)
next(indexed) # (3, 7)
```

---

## Información sobre iterables

De un iterable podemos calcular la longitud, el máximo y el mínimo.

```python
iterable = iter([1, 2, 3, 4, 5])
```

```python
len(iterable) # 5
```

```python
max(iterable) # 5
```

```python
min(iterable) # 1
```

Estas funciones también consumen el iterable (si es un objeto `iterator` como en el ejemplo).

---

## Encontrar elementos en un iterable

De un iterable, podemos verificar si un elemento está presente.

```python
iterable = iter([1, 2, 3, 4, 5])
```

```python
3 in iterable # True
```

```python
8 in iterable # False
```

---

## Comprobar condiciones sobre iterables

De un iterable podemos comprobar si alguno o todos los elementos cumplen una condición.

```python
iterable = iter([1, 2, 3, 4, 5])
```

```python
any(x > 4 for x in iterable) # True
```

```python
all(x > 4 for x in iterable) # False
```

Igual que las demás funciones, estas consumen el iterable.

---

## Suma de elementos de un iterable

Si los elementos de un iterable son compatibles entre si (por ejemplo, son todos números, o todos listas), podemos sumarlos.

```python
sum([1, 2, 3, 4, 5]) # 15
sum([1.5, 2.5, 3.5]) # 7.5
sum([[1, 2], [3, 4], [5, 6]], []) # [1, 2, 3, 4, 5, 6]
```

---

## Combinación de iterables

Podemos combinar dos o más iterables en una lista de tuplas.

```python
combi = zip(iterable1, iterable2)
next(combi) # (1, 10)
```

Esta construcción nos permite incluso separar un mismo iterable en pares:

```python
iterable = iter(range(20))
de_tres_en_tres = zip(iterable, iterable, iterable)
next(de_tres_en_tres) # (0, 1, 2)
next(de_tres_en_tres) # (3, 4, 5)
```

---

## Final boss: `reduce`

La función `reduce` es el comodín de las funciones de iterables. Prácticamente puede hacer cualquier cosa.

Importamos la función de la librería `functools`, y replicamos algunos ejemplos anteriores.

```python
from functools import reduce

# any
reduce(lambda acc, item: acc or item, iterable, False)

# map
reduce(lambda acc, item: acc + [item ** 2], iterable, [])

# filter
reduce(lambda acc, item: acc + [item] if item % 2 == 0 else acc, iterable, [])
```

---

<!--
_class: chapter-front
_paginate: false
header: Procesamiento de datos
-->

# Procesamiento de datos

---

## El módulo `csv`

Python provee un módulo para leer y escribir archivos CSV (*comma-separated values*, valores separados por comas).

El formato CSV es muy común y permite transpoortar y conumicar datos de una forma sencilla y libre de ataduras a formatos propietarios. Sin embargo aún existen programas que no soportan correctamente el formato CSV. Un programa de hojas de cálculo bastante conocido, por ejemplo, separa los campos por punto y coma en lugar de por comas.

```python
import csv

with open("datos.csv") as archivo:
    datos = csv.reader(archivo)
    for fila in datos:
        print(fila)
```

---

Cada línea del fichero se produce como lista de strings, respetando las comillas:

```csv
1,25,"Hola, mundo"
```

se convierte en

```python
[["1", "25", "Hola, mundo"]]
```

---

## Encabezados en archivos CSV

El ejemplo del slide anterior no distingue entre encabezados y datos. Cada fila del fichero se produce como simple lista de strings.

Si el ficher tiene una primera fila con los nombres de las columnas, podemos usar la función `DictReader` para producir diccionarios.

```python
with open("datos.csv") as archivo:
    datos = csv.DictReader(archivo)
    for fila in datos:
        print(fila)
```

---

En este caso, cada fila se produce como diccionario, con los nombres de las columnas como claves.

```csv
nombre,edad
Juan,25
```

se convierte en

```python
[{"nombre": "Juan", "edad": "25"}]
```

---

Los lectores de CSV son muy eficientes, ya que se basan en iteración y lectura de ficheros, y no cargan todo el contenido en memoria.

El tamaño de los ficheros CSV puede ser muy grande, y no siempre es posible cargarlos en memoria. Por ello debemos tener cuidado con convertir el contenido en una lista de listas o de diccionarios.

---

## Pandas

La librería `pandas` es una de las más populares para el procesamiento de datos en Python. Provee estructuras de datos y herramientas para trabajar con ellas.

```python
import pandas as pd

datos = pd.read_csv("datos.csv")
```

Entre las funcionalidades avanzadas de lectura de `pandas`, se incluyen:

- Leer ficheros en formato Excel
- Leer directamente de ficheros comprimidos (con `gzip` o `bz2`)
- Seleccionar previamente las columnas a leer
- Cargar las filas en bloques

---

## Funciones de `pandas`

Una vez cargados datos en un `DataFrame` (la estructura de tabla de `pandas`), podemos realizar operaciones de filtrado, transformación y agregación.

Suponiendo que los datos sea un CSV con columnas `nombre`, `edad` y `ciudad`, podemos hacer:

```python
# Filtrar los datos de personas de Barcelona
datos[datos.ciudad == "Barcelona"]

# Calcular la media de edad
datos.edad.mean()

# Agrupar por nombre
datos.groupby("nombre").count()
```

---

<!--
_class: chapter-front
_paginate: false
header: Q&A
-->

![bg left](../img/questions.jpeg)

# Q&A
