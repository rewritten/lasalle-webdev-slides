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

## Módulo 4 :: Temas adicionales de programación estructurada :: Módulos y paquetes

Profesor: Saverio Trioni

> Convocatoria de 2023 de los Programas de formación profesional para el empleo, de
> especialidades de la oferta de formación no formal, para personas trabajadoras ocupadas,
> que promueve el Consorcio para la Formación Continua de Cataluña (ref. BDNS 709943)

---

<!--
_class: chapter-front
_paginate: false
header: Modulos
-->

# Organización de código en módulos

---

Un módulo en Python es simplemente un archivo que contiene definiciones y declaraciones de funciones, clases y variables. Los módulos son una forma de organizar el código de manera que sea más fácil de entender y de usar.

Por ejemplo, podríamos tener un módulo llamado `matematicas.py` que contiene varias funciones relacionadas con operaciones matemáticas.

```python
# matematicas.py
def suma(a, b):
    return a + b

def resta(a, b):
    return a - b
```

---

Para utilizar un módulo en Python, debemos importarlo utilizando la palabra clave import. Una vez que un módulo ha sido importado, podemos acceder a sus funciones, clases y variables utilizando la notación de punto.

Por ejemplo, para utilizar las funciones del módulo `matematicas.py`, lo importaríamos y luego llamaríamos a sus funciones de la siguiente manera:

```python
import matematicas

resultado_suma = matematicas.suma(5, 3)
resultado_resta = matematicas.resta(10, 2)
```

---

También es posible importar funciones específicas de un módulo en lugar de importar todo el módulo. Para hacer esto, podemos utilizar la palabra clave from seguida del nombre del módulo y la palabra clave import seguida de los nombres de las funciones que queremos importar.

Por ejemplo, para importar solo la función `suma` del módulo `matematicas.py`, lo haríamos de la siguiente manera:

```python
from matematicas import suma

resultado_suma = suma(5, 3)
```

---

La estructuración del código en módulos permite separar las diferentes partes de un programa, lo que facilita su mantenimiento y comprensión. Cada módulo debe tener una responsabilidad única y todas sus funciones y clases deben estar relacionadas con esa responsabilidad.

Por ejemplo, podríamos tener un módulo para manejar las operaciones de la base de datos y otro módulo para las estructura de datos, cada uno con sus propias funciones y clases.

```python
from db import insert # también habrá funciones `update`, `delete`, `select`, ...
from data_structures import User # también habrá `Product`, `Order`, ...

def insert_user(data):
    user = User(data)
    insert(user)
```

---

Cuando se importa un módulo por primera vez, el código en el módulo se ejecuta. Podemos aprovechar esto para realizar tareas de inicialización, como la configuración de variables globales o la carga de datos.

```python
# db.py

connection = None

def connect():
    global connection
    connection = create_connection()

def insert(data):
    ...

def read(criteria):
    ...

connect()
print('Connected to database...')
```

Este módulo `db.py` se encarga de la conexión a la base de datos y se conecta automáticamente cuando se importa por primera vez.

---

Para usar ese módulo, simplemente lo importamos y llamamos a sus funciones.

```python
# users.py

import db

def insert_user(data):
    db.insert(data)
```

```python
# products.py

import db

def read_products(**criteria):
    db.read(model='products', **criteria)
```

---

Cuando un fichero se ejecuta directamente (usando `python fichero.py`), la variable `__name__` se establece a `'__main__'`. Podemos aprovechar esto para ejecutar código de inicialización solo cuando el módulo se ejecuta directamente y no cuando se importa.

```python
# matematicas.py

def suma(a, b):
    ...

if __name__ == '__main__':
    # No se ejecuta al importar, ya que __name__ != '__main__'
    print("Testing...")
    assert suma(2, 2) == 4
    print("All tests passed!")
```

Un caso de uso muy frecuente para esto es escribir simples pruebas unitarias para el módulo.

---

Todos los identificadores pueden en teoria ser importados, no existe el concepto de "privado" en Python. Cualquier variable, clase o función que esté definida en la primera collumna de un módulo se puede importar.

Usamos la convención de que los identificadores que empiezan por `_` son "privados" y no deberían ser usados fuera del módulo. Pero esto es solo una convención, no una regla.

```python
# matematicas.py
_some_private_variable = 42

def _some_private_function():
    return _some_private_variable
```

```python
# main.py

from matematicas import _some_private_variable, _some_private_function
```

---

Si usamos `from some_module import *`, todos los identificadores se importarán, sobreescribiendo cualquier cosa se haya definido previamente con el mismo nombre en el módulo que importa, o se haya previamente importado. Sin embargo un módulo puede definir su propio significado de `*`, mediante la variable `__all__`.

```python
# matematicas.py
__all__ = ['suma']
```

De esta forma, solo se importará la función `suma` cuando se importe el módulo `matematicas`.

```python
from matematicas import *

suma(2, 2) # OK
resta(2, 2) # NameError: name 'resta' is not defined
```

---

Cualquier identificador importado es a su vez algo que se puede importar otra vez:

```python
# suma.py

def suma(a, b):
    return a + b
```

```python
# matematicas.py
from suma import suma
```

```python
# main.py
from matematicas import suma
```

---

<!--
_class: chapter-front
_paginate: false
header: Paquetes
-->

# Organización de código en paquetes

---

Un paquete en Python es una carpeta que contiene uno o más módulos. Los paquetes son una forma de organizar el código de manera que sea más fácil de entender y de usar, especialmente cuando se tiene un gran número de módulos relacionados.

Por ejemplo, podríamos tener un paquete llamado `matematicas` que contiene los módulos `suma.py` y `resta.py`, cada uno con funciones relacionadas con operaciones matemáticas.

```plaintext
matematicas/
    suma.py
    resta.py
```

Para que Python reconozca una carpeta como un paquete, debe contener un archivo especial llamado `__init__.py`. Este archivo puede estar vacío.

```plaintext
matematicas/
    __init__.py
    suma.py
    resta.py
```

---

En el ejemplo anterior, suponiendo que los módulos `suma.py` y `resta.py` contienen las funciones `suma` y `resta`, respectivamente, podríamos importarlas de la siguiente manera:

```python
from matematicas.suma import suma
from matematicas.resta import resta

resultado_suma = suma(5, 3)
```

---

En alternativa podemos importar un módulo completo:

```python
from matematicas import suma

resultado_suma = suma.suma(5, 3)
```

o simplemente importar el paquete:

```python
import matematicas

resultado_suma = matematicas.suma.suma(5, 3)
```

---

Dentro de un paquete se pueden importar módulos anidados, con una notación específica:

```python
# matematicas/operaciones.py

from .suma import suma
from .resta import resta
```

```python
from matematicas.operaciones import suma, resta
```

Los paquetes se consideran separados entre si, pero dentro de un mismo paquete no hay restricciones para importar módulos a cualquier nivel de profundidad:

```python
from ....some.other.module import something
```

---

El archivo `__init__.py` es un fichero de python que sirve dos propósitos:

- Indica a Python que la carpeta es un paquete.
- Contiene el código del **módulo** correspondiente al paquete.

En el ejemplo anterior, el contenido del fichero `matematicas/__init__.py` podría ser simplemente:

```python
from .suma import suma
from .resta import resta
```

De esta forma, al importar el paquete `matematicas`, se importarán automáticamente las funciones `suma` y `resta`.

```python
from matematicas import suma
```

---

<!--
_class: chapter-front
_paginate: false
header: Q&A
-->

![bg left](../img/questions.jpeg)

# Q&A
