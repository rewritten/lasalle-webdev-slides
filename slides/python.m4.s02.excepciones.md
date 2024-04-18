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

## Módulo 4 :: Temas adicionales de programación estructurada :: Excepciones

Profesor: Saverio Trioni

> Convocatoria de 2023 de los Programas de formación profesional para el empleo, de
> especialidades de la oferta de formación no formal, para personas trabajadoras ocupadas,
> que promueve el Consorcio para la Formación Continua de Cataluña (ref. BDNS 709943)

---

<!--
_class: chapter-front
_paginate: false
header: Control de excepciones
-->

# Control de excepciones

---

## Introducción a las excepciones en Python

Las excepciones en Python son eventos que ocurren durante la ejecución de un programa cuando se encuentra un error. Cuando Python se encuentra con una situación que no puede manejar, genera una excepción.

Por ejemplo, dividir un número por cero es un error en Python y genera una excepción llamada `ZeroDivisionError`.

```python
# Esto generará una excepción ZeroDivisionError
resultado = 10 / 0
```

El código en este ejemplo imprime `ZeroDivisionError: division by zero`.

---

## Manejo de excepciones con try y except

Python proporciona las declaraciones `try` y `except` para manejar las excepciones. El código que puede generar una excepción se coloca
dentro del bloque `try` y el código que maneja la excepción se coloca dentro del bloque `except`.

```python
try:
    # Código que puede generar una excepción
    resultado = 10 / 0
except ZeroDivisionError:
    # Código que maneja la excepción
    print("No se puede dividir por cero!")
```

---

## Uso de else en el manejo de excepciones

El bloque `else` en el manejo de excepciones en Python se ejecuta cuando no se produce ninguna excepción en el bloque `try`. Es útil cuando queremos ejecutar algún código sólo si no se produjo ninguna excepción.

```python
try:
    # Código que puede generar una excepción
    resultado = 10 / 2
except ZeroDivisionError:
    # Código que maneja la excepción
    print("No se puede dividir por cero!")
else:
    # Código que se ejecuta si no se produjo ninguna excepción
    print("La división fue exitosa!")
```

---

## Uso de finally en el manejo de excepciones

El bloque `finally` en el manejo de excepciones en Python se ejecuta siempre, independientemente de si se produjo una excepción o no. Es útil para limpiar recursos o ejecutar código que debe ejecutarse sin importar qué.

```python
try:
    # Código que puede generar una excepción
    resultado = 10 / 0
except ZeroDivisionError:
    # Código que maneja la excepción
    print("No se puede dividir por cero!")
finally:
    # Código que se ejecuta siempre
    print("Fin del manejo de excepciones.")
```

---

<!--
_class: chapter-front
_paginate: false
header: Lanzamiento de excepciones
-->

# Lanzamiento de excepciones

---

## Lanzamiento de excepciones con raise

Python permite lanzar excepciones explícitamente utilizando la declaración `raise`. Esto es útil cuando queremos indicar que ha ocurrido un error en nuestro programa.

```python
# Lanzar una excepción
raise ValueError("Un valor no válido!")
```

---

## Creación de excepciones personalizadas

Python permite crear excepciones personalizadas definiendo nuevas clases de excepciones. Esto es útil cuando queremos definir nuestros propios tipos de errores.

```python
# Definir una nueva clase de excepción
class MiExcepcion(Exception):
    pass

# Lanzar la excepción personalizada
raise MiExcepcion("Esto es una excepción personalizada!")
```

---

## Encadenamiento de excepciones

Python permite encadenar excepciones, lo que significa que una excepción puede causar otra excepción. Esto es útil cuando una excepción ocurre como resultado directo de otra excepción.

```python
try:
    # Código que puede generar una excepción
    resultado = 10 / 0
except ZeroDivisionError as e:
    # Lanzar una nueva excepción como resultado de la excepción anterior
    raise ValueError("Un valor no válido!") from e
```

Podemos recuperar la excepción original en el atributo `__cause__` de la excepción lanzada.

---

## Relanzado de excepciones

Python permite relanzar excepciones, lo que significa que una excepción capturada puede ser lanzada nuevamente. Esto es útil cuando queremos capturar una excepción, hacer algo y luego lanzar la excepción nuevamente.

```python
try:
    # Código que puede generar una excepción
    resultado = 10 / 0
except ZeroDivisionError:
    # Hacer algo
    print("Ocurrió un error!")
    # Relanzar la excepción
    raise
```

---

## Supresión de excepciones

Python permite suprimir excepciones utilizando la declaración pass en un bloque except. Esto es útil cuando queremos ignorar ciertos tipos de excepciones.

```python
try:
    # Código que puede generar una excepción
    resultado = 10 / 0
except ZeroDivisionError:
    # Ignorar la excepción
    pass
```

---

<!--
_class: chapter-front
_paginate: false
header: Observabilidad de excepciones
-->

# Observabilidad

---

En cualquier sistema mínimamente complejo, es importante tener un buen manejo de las excepciones. Esto implica no sólo capturar y manejar las excepciones, sino también registrarlas y monitorizarlas.

Hay servicios online y herramientas que se pueden instalar en tu propio servidor que permiten monitorizar las excepciones de tu aplicación y recibir alertas en caso de que ocurran errores.

---

La herramienta fundamental para la observabilidad de excepciones en Python es el módulo `logging`. Este módulo proporciona una forma de registrar mensajes de error y advertencia en un archivo o en la consola.

```python
import logging

logging.debug("Este es un mensaje de depuración.")
logging.info("Este es un mensaje de información.")
logging.warning("Este es un mensaje de advertencia.")
logging.error("Este es un mensaje de error.")
```

Los mensajes que hemos pasado a funciones del módulo `logging` se acaban distribuyendo a las destinaciones que hayamos configurado. Por ejemplo, pueden ser escritos en el output del proceso, en un fichero o en un servicio de monitorización.

---

Cuando queremos informar de una excepción, podemos usar la función `exception` del módulo `logging`. Esta función registra un mensaje de error junto con la traza de la excepción.

```python
try:
    resultado = 10 / 0
except ZeroDivisionError:
    logging.exception("Ocurrió un error al dividir por cero.")
    float("nan")
```

En este caso la traza entera de la ejecución se envía, para poder identificar el origen del error.

---

En un fichero de log o en el output del proceso, el mensaje de error se podría ver así:

```plaintext
ERROR:root:Ocurrió un error al dividir por cero.
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
ZeroDivisionError: division by zero
```

Sin embargo en un servicio online de monitorización podremos ver más claramente los distintos ficheros y líneas de código que han causado el error, y asociar otros errores similares.

---

<!--
_class: chapter-front
_paginate: false
header: Q&A
-->

![bg left](../img/questions.jpeg)

# Q&A
