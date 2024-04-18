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

## Módulo 3 :: Temáticas intermedias en Python :: Identificadores y su alcance

Profesor: Saverio Trioni

> Convocatoria de 2023 de los Programas de formación profesional para el empleo, de
> especialidades de la oferta de formación no formal, para personas trabajadoras ocupadas,
> que promueve el Consorcio para la Formación Continua de Cataluña (ref. BDNS 709943)

---

<!--
_class: chapter-front
_paginate: false
header: Las "variables"
-->

# Las "variables": los nombres y su resolución

---

## Ambigüedad del término "variable"

Históricamente, se usa el término "variable" para referirse a algo que puede, justamente, variar. En matemáticas, una variable es un símbolo que representa un valor desconocido, y que puede tomar cualquier valor dentro de un conjunto. Es decir, es un símbolo que no tiene un valor definido previamente.

En computación, hay dos maneras en que algo puede variar:

- **Mutación**: un valor puede cambiar a lo largo del tiempo. Por ejemplo, una lista puede cambiar su contenido, un diccionario puede ganar o perder claves, el estado interno de un objeto puede cambiar.
- **(re)Asignación**: un nombre puede referirse a diferentes valores según el momento en que se le consulta.

En el primer caso hablaríamos de *valores mutables*, en el segundo caso de *identificadores*.

---

## Identificadores

En Python podemos obtener identificadores de diferentes maneras:

- Importando algo desde otro módulo.

```python
from math import pi
```

- Definición directa de un identificador, asignando un valor a un nombre.

```python
x = 42
```

- Descomponiendo una secuencia (lista o tupla) en sus partes.

```python
a, b = [1, 2] # El número de elementos debe coincidir
c, d, *resto = (3, 4, 5, 6) # Los elementos extra se capturan en una lista
```

- definiendo una función.

```python
# `mi_funcion` es un identificador
def mi_funcion(x):
    return x + 1
```

---

- Como argumento de una función.

```python
def f(arg):
    # `arg` es un identificador
    return arg + 1
```

- Como elemento de iteración:

```python
for item in range(10):
    # `item` es un identificador
    print(item)
```

- Como elemento transitorio de una comprensión:

```python
# `x` es un identificador _en el contexto de la comprensión_
cuadrados = [x ** 2 for x in range(10)]
```

---

## Reasignación

Todos los identificadores se pueden reasignar, mientras estemos en cierto sentido "cerca de la definición".

```python
a = 1
a = 2
a = "hola"
```

En casos donde la sintaxis no lo permite (lambdas y comprensiones), obviamente no se puede reasignar identificadores.

---

<!--
_class: chapter-front
_paginate: false
header: Alcance
-->

# Alcance

---

En los ejemplos anteriores, los identificadores están disponibles en un contexto determinado. A esto se le llama *alcance*.

El alcance (o *scope*) de un identificador es el contexto en el que es accesible. En Python, el alcance de un identificador se define por la estructura del código

---

## Alcance de argumentos de una función

En el caso de los argumentos de una función, el alcance es el cuerpo de la función. Los argumentos son accesibles en el cuerpo de la función, pero no fuera de ella.

```python
def f(x):
    return x + 1

f(3) # 4

print(x) # NameError: name 'x' is not defined
```

---

Una función sin embargo puede **leer** identificadores cuyo alcanze es superior, como vimos en

```python
suma = lambda x: lambda y: x + y
```

La función *interna* (`lambda y: x + y`) puede leer el valor de `x` que se le pasó a la función *externa* (`lambda x: ...`).

Lo mismo ocurre con las funciones definidas con `def`.

```python
def suma(x):
    def interna(y):
        return x + y
    return interna
```

---

Los argumentos de una función se pueden reasignar, sin afectar al valor original:

```python
def suma_plus_1_plus_1(x, y):
    x = x + 1
    y += 1
    return x + y

a = 1
b = 2
print(suma_plus_1_plus_1(a, b)) # 5
print(a) # 1
```

---

## Argumentos mutables

Si un argumento es mutable, se puede modificar su contenido. Esto afecta al objeto original.

```python
def suma(lista_de_numeros):
    lista_de_numeros.append(42)
    return sum(lista_de_numeros)

l = [1, 2, 3]
print(suma(l)) # 48
print(l) # [1, 2, 3, 42]
```

Es muy importante no mutar argumentos (a menos que sea intencionado) para evitar efectos secundarios.

---

## Alcance de los identificadores en iteraciones

En una comprensión, los identificadores solamente son accesibles en el cuerpo de la comprensión.

```python
cuadrados = [x ** 2 for x in range(10)]
coordenadas = [(x, y) for x in range(10) for y in range(10) if x != y]
```

En una iteración, los identificadores "traspasan" al contexto donde se incluye el bucle:

```python
for x in range(10):
    print(x)

print(x) # 9
```

---

# Alcance global

En un fichero de Python, todos los identificadores definidos "a la izquierda" (sin estar en un bloque) se consideran globales - en el sentido de que son accesibles desde cualquier parte del fichero.

```python
import math
a = 4
# `math` y `a` son global
```

Además, hay unos identificadores que siempre son disponibles, que se conocen como "builtins". Hemos usado muchos de ellos:

```python
print("Hola")
map(lambda x: x + 1, [1, 2, 3])
list(range(10))
```

---

## Alcance de lectura vs alcance de reasignación

Creamos una función que hace algo sencillo, a la vez que mantiene un contador de las veces que se ha llamado:

```python
def incremento_contando_llamadas(delta):
    contador = 0
    def interna(x):
        contador += 1
        return (x + delta, contador)

incrementa3 = incremento_contando_llamadas(3)
```

Si intentamos llamar a `incrementa3(5)`, obtendremos un error:

```python
incrementa3(5)
# UnboundLocalError: local variable 'contador' referenced before assignment
```

Esto es porque la línea `contador += 1` fuerza `contador` a ser una variable local, pero se intenta leer antes de asignarla.

---

## Salto de alcance

Si la sintaxis nos permite asignaciones (es decir no estamos en una lambda o comprensión), podemos permitir la asignación a un identificador que está en un alcance superior.

```python
def incremento_contando_llamadas(delta):
    contador = 0
    def interna(x):
        nonlocal contador
        contador += 1
        return (x + delta, contador)

incrementa3 = incremento_contando_llamadas(3)
incrementa3(5) # (8, 1)
incrementa3(25) # (28, 2)
```

---

También podemos reasignar un identificador global desde una función, si usamos la palabra clave `global`.

```python
a = 1

def incrementa_a():
    global a
    a += 1

incrementa_a()
incrementa_a()
incrementa_a()
print(a) # 4
```

Sin embargo es imposible afectar a otros módulos, es decir que si importamos un valor

```python
from math import pi

pi = 3.25
```

no existe manera de cambiar el valor de `pi` en el módulo `math` para que en otro momento que
se importe `pi` se obtenga el valor `3.25`.

---

<!--
_class: chapter-front
_paginate: false
header: with
-->

# `with`, el gestor de contexto

---

Existe una última forma de obtener referencias a objetos, que es mediante el uso de un *gestor de contexto*.

Un gestor de contexto (*context manager*) es un objeto que provee tres responsabilidades:

- **Entrada al contexto**: se ejecuta al principio del bloque.
- **Objeto de contexto**: se obtiene una referencia al objeto que se va a usar.
- **Salida del contexto**: se ejecuta al final del bloque.

---

Un ejemplo común es el uso de ficheros, pero es interesante ver que nos lleva a otra fuente de identificadores:

```python
with open("fichero.txt") as my_file:
    # `my_file` es un identificador
    contenido = my_file.read()
```

Las reglas de alcance de `my_file` son las mismas que los identificadores en un `for`, es decir, el alcance es la función o módulo que contiene la expresión `with`.

Sin embargo, una vez fuera del bloque `with`, la función de salida del contexto se ha ejecutado, así que es posible que no podamos hacer nada interesante con el objeto referenciado.

---

<!--
_class: chapter-front
_paginate: false
header: Q&A
-->

![bg left](../img/questions.jpeg)

# Q&A
