# Python3
Мои заметки по питону

```python3
{True: 'да', 1: 'нет', 1.0: 'возможно'}
#Получиться: {True: 'возможно'}
```
Форматирование строк
```python3
world = "world"
log_message = 'Hello {world} !'.format(world=world)
# Лучше
world = "world"
log_message = f'Hello {world} !'
#Еще есть на подобии Си
' %s! %s!' %("Hello", world)
#Еще
'%(name)s %(world)s!' % { "name": "Hello", "world": "world" }
# И еще
name = "ты"
from string import Template 
t = Template('Эй $name!')
t.substitute(name=name) # 'Эй ты!'
```

Dict
```python3
a = {"hi" : 70}
b = a.get("Hey", 100) # b = 100
```

Spread 
```python3
a, *b, c = range(5) # a = 0, b = [1, 2, 3], c = 4
```

Мемоизация с помощью lru_cache
```python3
from functools import lru_cache
@lru_cache(maxsize=512)
def fib_memoization(number: int) -> int:
    if number == 0: return 0
    if number == 1: return 1
    
    return fib_memoization(number-1) + fib_memoization(number-2)
start = time.time()
fib_memoization(40)
print(f'Duration: {time.time() - start}s') # # Duration: 6.866455078125e-05s
```
Значение "__var" в классе
```python3
class ManglingTest:
     def __init__(self):
         self.__mangled = 'Привет'
        
     def get_mangled(self):
         return self.__mangled
ManglingTest().get_mangled()
ManglingTest().__mangled # AttributeError: "'ManglingTest' object has no attribute '__mangled'"
# Python3 автоматически не дает доступа к методам, переменным и т.д. начинающимся с __
```
Декоратор
```python3
def bread(func):
    def wrapper():
        print("1      ^")
        func()
        print("5 </_______\>")
    return wrapper
 
def ingredients(func):
    def wrapper():
        print("2    </_\>")
        func()
        print("4  </_____\>")
    return wrapper
 
def sandwich(food="3   </___\>"):
    print(food)
    
@bread
@ingredients
def sandwich(ent="3   </___\>"): # or sandwich = bread(ingredients(sandwich))
    print(ent)
sandwich() 
#1      ^
#2    </_\>
#3   </___\>
#4  </_____\>
#5 </_______\>
# Трэйсер декораторов
import functools
     @functools.wraps(func)
```

Странная работа с методом класса, "__" - дандер
```python3
_Example__bar = "привет из глобалки"
class Example:
     def test(self):
         return __bar
Example().test()
```
Класс с абстрактным методом
```python3
class Base(metaclass=ABCMeta):
    @abstractmethod
    def foo(self):
        pass
```
Дата класс
```python3
from dataclasses import dataclass
@dataclass
class Armor:
    armor: float
```
 __slots__ для оптимизации класса
```python3
>>> import attr
>>> @attr.s(slots=True)
... class Coordinates(object):
...     x = attr.ib()
...     y = attr.ib()
...
>>> c = Coordinates(x=1, y=2)
>>> c.z = 3
Traceback (most recent call last):
    ...
AttributeError: 'Coordinates' object has no attribute 'z'
```
различия __str__ и __repr__
```python3
class Example:
     def __init__(self, color):
         self.color = color
     def __repr__(self):
         return '__repr__ для объекта Example'
     def __str__(self):
         return '__str__ для объекта Example'
         
         
>>> my_ex = Example('красный')
>>> print(my_ex)
__str__ для объекта Example
>>> '{}'.format(my_ex)
'__str__ для объекта Example'
>>> my_ex
__repr__ для объекта Example

# Флаг !r для ототбражения __repr__ вместо __str__
def __repr__(self):
     return f'Example({self.color!r})'
```
deepcopy точная копия без связи, клонирован рекурсивно
```python3
import copy
xs = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
zs = copy.deepcopy(xs)
```

namedtuple - кортеж с доступом по имени 
```python3
from collections import namedtuple
Car = namedtuple('Авто', [
     'цвет',
     'пробег',
 ])
 
 >>> my_car = Car('красный', 3812.4) >>> my_car.цвет
'красный'
>>> my_car.пробег
3812.4
```
