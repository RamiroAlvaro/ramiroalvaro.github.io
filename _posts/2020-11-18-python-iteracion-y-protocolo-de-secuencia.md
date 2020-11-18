---
layout: post
title:  "Iteración y protocolo de secuencia [ES]"
date:   2020-11-18 11:00:00 -0300
categories: [python]
---

Veremos como nuestra clase puede soportar iteración, implementando el protocolo de secuencia de Python. Para ello nuestra clase debe contener los métodos especiales ```__len__()``` y ```__getitem__()```.
A modo de ejemplo vamos a crear la clase ```SequenceOfPositiveIntegers```, la que recibirá como parámetro un número entero positivo, el cual será el último número de nuestra secuencia.

----------------

#### Implementación:
```python
class SequenceOfPositiveIntegers:
    def __init__(self, number):
        self.number = number

    def __len__(self):
        return self.number

    def __getitem__(self, position):
        index = position if position >= 0 else self.number + position
        if 0 <= index < self.number:
            return f'Number: {index + 1}'
        raise IndexError('Index out of range')
```

----------------

#### Sesión de consola:

```python
In [1]: sequence = SequenceOfPositiveIntegers(5)

In [2]: sequence[0]
Out[2]: 'Number: 1'

In [3]: sequence[1]
Out[3]: 'Number: 2'

In [4]: sequence[-1]
Out[4]: 'Number: 5'

In [5]: len(sequence)
Out[5]: 5

In [6]: for n in sequence:
    ...:     print(n)
    ...:
Number: 1
Number: 2
Number: 3
Number: 4
Number: 5
```