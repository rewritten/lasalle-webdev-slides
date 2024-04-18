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

## Módulo 3 :: Temáticas intermedias en Python :: Funciones

Profesor: Saverio Trioni

> Convocatoria de 2023 de los Programas de formación profesional para el empleo, de
> especialidades de la oferta de formación no formal, para personas trabajadoras ocupadas,
> que promueve el Consorcio para la Formación Continua de Cataluña (ref. BDNS 709943)

---

<!--
_class: chapter-front
_paginate: false
header: Funciones
-->

# Funciones

---

## Invocación de funciones

Una función es un bloque de código que se ejecuta cuando se llama a la función.

La sintaxis para llamar a una función es la siguiente:

```python
nombre_de_la_funcion(...argumentos...)
```

Donde `nombre_de_la_funcion` es el nombre de la función y `argumentos` son los valores que se pasan a la función.

Si la función se encuentra en algún contenedor (módulo - los veremos en la próxima unidad - o objeto - los veremos al final del curso), se debe anteponer el nombre del contenedor seguido de un punto.

```python
contenedor.nombre_de_la_funcion(...argumentos...)
```

---

## Definición de funciones

La forma más sencilla de definir funciones es con la palabra clave `lambda`.

```python
mi_funcion = lambda x: x + 2
```

A partir de que se define, la función se puede invocar usando su nombre.

```python
mi_funcion(3) # 5
```

---

## Peculiardidades de las funciones `lambda`

Las funciones lambda son una especie de literal (como los números o las cadenas de texto). Existen independientemente de haberle asignado un nombre, y se pueden usar directamente:

```python
(lambda x: x + 2)(3) # 5
```

O como argumentos de otras funciones:

```python
map(lambda x: x + 2, [1, 2, 3]) # iterador que produce 3, 4, 5
```

El resultado de invocar la función `lambda` es simplemente el valor de la expresión a la derecha de los dos puntos.

---

Las funciones `lambda` son necesariamente sencillas, ya que solo pueden contener una expresión. No pueden contener ninguna construcción de control de flujo, como `if`, `for`, `while`, ni utilizar nombres temporales para sus cálculos.

Las lambdas son por tanto útiles para definir funciones sencillas que se van a usar una sola vez, o como argumentos de otras funciones.

La conexión con el lenguaje matemático del lambda-cálculo se puede ver en la forma en que las funciones lambda pueden retornar otras funciones lambda. Por ejemplo:

```python
suma = lambda x: lambda y: x + y
suma(3)(4) # 7
```

es equivalente a la expresión `λx.(λy.(x + y))`; al aplicar `3` a la función, esa equivale a `λy.(3 + y)`, que al aplicar `4` da `3 + 4`.

---

## Definición de funciones con `def`

La forma más común de definir funciones es con la palabra clave `def`.

```python
def mi_funcion(x):
    return x + 2
```

En el bloque de código que sigue a la definición de la función, se pueden usar todas las construcciones de control de flujo que se quieran.

El valor retornado por la función (es decir el valor de la expresión `mi_funcion(3)`) es el valor de la expresión que sigue a la palabra clave `return`.

Una función puede tener múltiples `return`, pero solo uno se ejecutará.

---

## Los argumentos

Las funciones pueden recibir argumentos. El nombre asignado al valor de un argumento es un asunto propio de la función, y no tiene por qué coincidir con el nombre de la variable que se pasa como argumento.

```python
def suma(a, b):
    return a + b

def tambien_suma(x, y):
    return suma(y, x)

tambien_suma(3, 4) # 7
```

---

## Valores por defecto de los argumentos

Las funciones se pueden definir con argumentos opcionales, que toman un valor predefinido si no se les pasa un valor.

```python
def suma(a, b=0):
    return a + b

suma(3) # 3
suma(3, 4) # 7
```

Es **extremadamente importante** que los valores predefinidos de los argumentos sean términos simples y sobretodo **inmutables**. Si se usan términos mutables, se pueden producir efectos secundarios no deseados.

---

## Argumentos con nombre

Cuando se llama a una función, se pueden pasar valores a los argumentos. Esto rompe la independencia de la definición de la función respecto a su invocación, ya que el código que invoca la función debe conocer el nombre de los argumentos.

```python
def suma(a, b):
    return a + b

suma(b=3, a=4) # 7
```

Al invocar una función, podemos pasar algunos valores por orden y otros por nombre.

```python
def suma3(a, b, c):
    return a + b + c

suma(3, c=4, b=5) # 12
```

Los valores pasados por orden "ocupan" los argumentos corrrespondientes.

---

## Argumentos variables

Las funciones pueden aceptar una cantidad variable de argumentos. Para ello, se usan los marcadores `*` y `**` en la definición de la función.

```python
def suma(a, b, *args):
    # args es una tupla con todos los valores pasados por orden, a partir del tercero.
    return a + b + len(args)

suma(3, 4, 5, 6, 7) # 10
```

```python
def suma(a, b, **kwargs):
    # kwargs es un diccionario con todos los valores pasados por nombre, a partir del tercero.
    return a + b + sum(kwargs.values())

suma(3, 4, some=5, thing=6, more=7) # 25
```

---

## Forzar el paso de argumentos por nombre

Si se quiere forzar que los argumentos se pasen por nombre, se puede usar un argumento con un solo asterisco `*`.

```python
def suma(a, b, *, c):
    return a + b + c

suma(3, 4, c=5) # 12
```

El asterisco `*` es el mismo marcador de argumentos variables pero, al no estar asociado a un nombre, fuerza a que no haya ningún argumento extra. Si queremos aceptar (e ignorar) argumentos extra, podemos usar un asterisco `*` con un nombre que no vayamos a usar:

```python
def suma(a, b, *_, c):
    return a + b + c

suma(3, 4, 5, c=6) # 13
```

---

## Forzar el paso de argumentos por orden

Si se quiere forzar que los argumentos se pasen por orden, se puede usar un argumento especial `/`. Este argumento simplemente fuerza a que los argumentos que le preceden se pasen por orden.

```python
def suma(a, b, /, c):
    return a + b + c

suma(3, 4, 5) # 12
suma(3, 4, c=5) # 12
suma(3, b=4, c=5) # Error
```

Se pueden combinar los dos marcadores para forzar que los argumentos se pasen por orden y por nombre.

```python
def suma(a, /, b, *, c):
    return a + b + c

suma(3, 4, 5) # Error
suma(3, 4, c=5) # 12
suma(3, b=4, c=5) # 12
suma(a=3, b=4, c=5) # Error
```

---

<!--
_class: chapter-front
_paginate: false
header: Q&A
-->

![bg left](../img/questions.jpeg)

# Q&A
