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

## Módulo 2 :: Elementos de programación estructurada :: Generadores

Profesor: Saverio Trioni

> Convocatoria de 2023 de los Programas de formación profesional para el empleo, de
> especialidades de la oferta de formación no formal, para personas trabajadoras ocupadas,
> que promueve el Consorcio para la Formación Continua de Cataluña (ref. BDNS 709943)

---

<!--
_class: chapter-front
_paginate: false
header: Generadores
-->

# Los generadores

---

##  Generadores en general

Un generador es un objeto especial, que produce valores bajo demanda.

Es parecido a los iteradores, pero se puede definir de una forma muy sencilla, a partir de un iterable preexistente:

```python
generador = (x for x in range(10))
```

Los paréntesis son necesarios en la definición.

---

##  Generadores en general (cont.)

Este tipo de construcciones se llaman también "comprensiones" (en inglés, *comprehensions*).

Se pueden usar en cualquier lugar donde se espera un iterable, como en un bucle `for`:

```python
for x in generador:
    print(x)
```

Igual que un iterable en general, se consumen solamente una vez.

---

## Transformación de los elementos

Podemos aplicar una transformación a los elementos del generador, usando una expresión:

```python
generador = (x + 1 for x in range(9999999999999999999999999999))
```

Esta expresión se evalúa para cada elemento del iterable original, solamente caundo se pide un nuevo valor. En el ejemplo, la expresión no efectúa miles de millones de sumas de antemano.

---

## Filtrado de los elementos

Podemos filtrar los elementos del generador, usando una expresión condicional:

```python
generador = (x for x in range(9999999999999999999999999999) if x % 2 == 0)
```

En este caso, el generador produce solamente los números pares del rango del 0 al 9999999999999999999999999999.

Igual que antes, la expresión condicional se evalúa solamente cuando se pide un nuevo valor, y solamente hasta que se encuentra un valor que cumple la condición.

---

## Anidamiento de generadores

Una comprensión puede iterar en varios niveles. Suponiendo que `data` es una lista de listas, podemos hacer una comprensión anidada:

```python
generador = (item for sublist in data for item in sublist)
#                 ^^^^^^^^^^^^^^^^^^^                     iteración externa
#                                     ^^^^^^^^^^^^^^^^^^^ iteración interna
```

La comprensión en el ejemplo es parecida a aplicar la función

```python
from itertools import chain

chain.from_iterable(data)
```

pero con una comprensión podemos también filtrar y transformar los elementos.

---

<!--
_class: chapter-front
_paginate: false
header: Generadores como iteradores
-->

# Generadores como iteradores

---

## Iteración manual

Como hemos visto antes, un generador se puede usar en una estructure de bucle `for`.

En general, qué es lo que hace un bucle `for` cuando se encuentra un generador?

Vamos a recorrer **manualmente** un generador, para entender mejor cómo funciona.

---

## Iteración manual (cont.)

Vamos a definir un generador muy simple:

```python
generador = (x for x in range(10))
```

Para recorrerlo manualmente, usamos la función `next()`:

```python
print(next(generador))
# 0
print(next(generador))
# 1
print(next(generador))
# 2
```

---

## Iteración manual (cont. - 2)

También podemos usar un *dunder* (método especial) para recorrer un generador:

```python
print(generador.__next__())
# 3
print(generador.__next__())
# 4
print(generador.__next__())
# 5
```

---

## Iteradores y generadores al descubierto

Este es el primer ejemplo de *dunder* que tiene un impacto directo en el comportamiento de un objeto.

- Un objeto que tiene un método `__next__()` es iterable.
- Python usa la función `next()` para recorrer un objeto iterable, elemento tras elemento.
- Cuando tal función no encuentra más elementos, lanza una excepción `StopIteration`.

Así que para hacer que un objeto sea iterable, basta con definir un método `__next__()`.

---

## Iteradores y generadores al descubierto (cont.)

En realidad hay otra forma de definir generadores, que es más explícita:

```python
def generador_de_1_2_3():
    yield 1
    yield 2
    yield 3
```

Este generador produce los números 1, 2 y 3:

```python
g = generador_de_1_2_3()
print(next(g))
# 1
# etc
```

---

<!--
_class: chapter-front
_paginate: false
header: Comprensiones hacia estructuras
-->

# Comprensiones hacia estructuras

---

## Funciones de creación de estructuras

Python provee una serie de funciones que permiten generar estructuras de datos a partir de iterables.

Estas funciones son:

- `list()` - crea una lista
- `tuple()` - crea una tupla
- `set()` - crea un conjunto
- `dict()` - crea un diccionario

En este último caso, el iterable debe producir pares clave-valor (pueden ser listas o tuplas).

---

## Ejemplos de creación de estructuras

Vamos a generar una lista, una tupla y un conjunto a partir de un range:

```python
lista = list(range(10))
tupla = tuple(range(10))
conjunto = set(range(10))
```

Vamos a generar un diccionario a partir de una lista de tuplas y de una tupla de listas:

```python
diccionario1 = dict([("uno", 1), ("dos", 2), ("tres", 3)])
diccionario2 = dict(([1, "uno"], [2, "dos"], [3, "tres"]))
```

---

Las funciones constructoras aceptan un iterable, así que podemos usar también generadores:

```python
lista = list(x + 4 for x in range(10) if x % 2 == 0)
# [4, 6, 8, 10, 12]
```

En este caso **no necesitamos paréntesis** alrededor de la comprensión, porque la función `list()` ya asegura que no haya ambigüedad.

---

## Comprensiones en forma breve

Python acepta una forma más breve, que es lo que se llama más comúnmente "comprensión".

Por ejemplo, para crear una lista de los cuadrados de los números del 0 al 9:

```python
cuadrados = [x ** 2 for x in range(10)]
```

Para crear un conjunto o una tupla de los cuadrados de los números del 0 al 9:

```python
cuadrados = {x ** 2 for x in range(10)}
```

---

No se puede generar una tupla con una comprensión, ya que los paréntesis *no son lo que define las tuplas*.
Para crear una tupla, se puede usar solamente la función `tuple()`:

```python
generador = (x ** 2 for x in range(10))
tambien_generador ((x ** 2 for x in range(10)))
tupla = tuple(x ** 2 for x in range(10))
```

---

## Comprensión de diccionarios

Para crear un diccionario con una comprensión, se usa la misma sintaxis que para los conjuntos, pero con pares clave-valor:

```python
diccionario = {str(x): x ** 2 for x in range(10) if x % 2 == 0}
#              ^^^^^^^^^^^^^ clave: valor
#                            ^^^^^^^^^^^^^^^^^^ iteración
#                                               ^^^^^^^^^^^^^ condición
```

El resultado del ejemplo es el siguiente diccionario:

```python
{
    '0': 0,
    '2': 4,
    '4': 16,
    '6': 36,
    '8': 64
}
```

---

<!--
_class: chapter-front
_paginate: false
header: Q&A
-->

![bg left](../img/questions.jpeg)

# Q&A
