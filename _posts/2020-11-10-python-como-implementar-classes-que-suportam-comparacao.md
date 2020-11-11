---
layout: post
title:  "Como implementar classes que suportam comparação em Python [PT]"
date:   2020-11-10 22:00:00 -0300
categories: [python]
---

#### Problema:
Como implementar classes cujas instâncias suportam comparação por meio dos operadores ```>, >=, <, <=, ==, !=```

----------------

#### Solução:
Para não ter que implementar todos os métodos especiais (```__eq__(), __lt__(), __le__(), __gt__(), __ge__(), __ne__()```) vamos importar de ```functools``` o decorador ```total_ordering```. 
Desta forma, só teremos que implementar o método ```__eq__()``` e mais um dos outros métodos, como por exemplo ```__lt__()```.

----------------

#### Implementação:
```python
from functools import total_ordering

@total_ordering
class Square:
    def __init__(self, side):
        self.side = side

    @property
    def area(self):
        return self.side ** 2

    def __eq__(self, other):
       return self.area == other.area

    def __lt__(self, other):
        return self.area < other.area
```
#### Sessão de console:

```python
In [1]: square_2 = Square(2)

In [2]: square_5 = Square(5)

In [3]: square_2.area
Out[3]: 4

In [4]: square_5.area
Out[4]: 25

In [5]: square_2 == square_5
Out[5]: False

In [6]: square_2 != square_5
Out[6]: True

In [7]: square_2 > square_5
Out[7]: False

In [8]: square_2 >= square_5
Out[8]: False

In [9]: square_2 < square_5
Out[9]: True

In [10]: square_2 <= square_5
Out[10]: True
```

Os seguintes métodos foram criados pelo decorador ```@total_ordering```:
```python
__le__ = lambda self, other: self < other or self == other
__gt__ = lambda self, other: not (self < other or self == other)
__ge__ = lambda self, other: not (self < other)
__ne__ = lambda self, other: not self == other
```