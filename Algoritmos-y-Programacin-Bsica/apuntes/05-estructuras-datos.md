# Estructuras de Datos en Python

## 1. Listas (list)

### Características
- **Mutables**: se modifican tras creación
- **Ordenadas**: mantienen orden de inserción
- **Indexadas**: acceso por índice (0-based)
- **Heterogéneas**: aceptan cualquier tipo mezclado

### Operaciones básicas
`python
lista = [1, 2, 3]

# Acceso
lista[0]        # 1
lista[-1]       # 3 (último)
lista[1:3]      # [2, 3] (slice: inicio:fin:paso)

# Modificación
lista.append(4)         # [1,2,3,4]
lista.insert(0, 0)      # [0,1,2,3,4]
lista.extend([5, 6])    # [0,1,2,3,4,5,6]
lista.remove(2)         # elimina primera ocurrencia de 2
pop = lista.pop()       # elimina y devuelve último
pop = lista.pop(0)      # elimina y devuelve índice 0
del lista[0]            # elimina por índice
lista.clear()           # vacía la lista

# Búsqueda
3 in lista              # True/False
lista.index(3)          # índice de primera ocurrencia
lista.count(3)          # cuántas veces aparece

# Ordenamiento
lista.sort()            # in-place, ascendente
lista.sort(reverse=True) # descendente
lista.sort(key=len)     # por longitud (para strings)
sorted(lista)           # nueva lista ordenada (no modifica original)
reversed(lista)         # iterador inverso
`

### Métodos útiles
`python
lista = [3, 1, 4, 1, 5]
len(lista)      # 5
max(lista)      # 5
min(lista)      # 1
sum(lista)      # 14
lista.copy()    # copia superficial
lista * 3       # repite: [1,2,3,1,2,3,1,2,3]
lista + [7,8]   # concatenación: nueva lista
`

### List comprehensions (revisión)
`python
[x**2 for x in range(10)]              # [0,1,4,9,16,25,36,49,64,81]
[x for x in range(20) if x % 2 == 0]   # pares
[x**2 for x in range(10) if x % 2 == 0] # cuadrados de pares
`

### Copias: superficial vs profunda
`python
import copy

original = [[1, 2], [3, 4]]
superficial = original.copy()      # o original[:], list(original)
profunda = copy.deepcopy(original)

original[0][0] = 99
superficial[0][0]  # 99 (comparten sublista interna!)
profunda[0][0]     # 1 (independiente)
`

---

## 2. Tuplas (	uple)

### Características
- **Inmutables**: no se modifican tras creación
- **Ordenadas**, indexadas, heterogéneas
- Más ligeras y rápidas que listas
- Usadas como claves de dict (si elementos inmutables)

`python
tupla = (1, 2, 3)
tupla[0]        # 1
tupla[1:3]      # (2, 3)
len(tupla)      # 3
3 in tupla      # True
tupla.index(2)  # 1
tupla.count(1)  # 1

# Desempaquetado
a, b, c = tupla
a, *resto = (1, 2, 3, 4)  # a=1, resto=[2,3,4]

# Tupla de un elemento (coma obligatoria)
uno = (1,)      # tupla
no_tupla = (1)  # int

# Conversión
list((1,2,3))   # [1,2,3]
tuple([1,2,3])  # (1,2,3)
`

### Named tuples (Python 3.6+)
`python
from collections import namedtuple
from typing import NamedTuple

# Forma clásica
Punto = namedtuple("Punto", ["x", "y"])
p = Punto(3, 4)
p.x, p.y  # 3, 4

# Forma moderna (type hints)
class Punto(NamedTuple):
    x: float
    y: float

p = Punto(3.0, 4.0)
p.x  # 3.0
`

---

## 3. Diccionarios (dict)

### Características
- Pares **clave: valor**
- **Mutables**, **ordenados** (Python 3.7+ mantiene orden de inserción)
- Claves: **inmutables** (str, int, tuple, frozenset...)
- Valores: **cualquier tipo**

### Operaciones básicas
`python
d = {"nombre": "Jesús", "edad": 20}

# Acceso
d["nombre"]           # "Jesús"
d.get("edad")         # 20
d.get("curso", "N/A") # "N/A" (default si no existe)

# Modificación
d["edad"] = 21
d["curso"] = "Ing. Software"
d.update({"ciudad": "Cartagena", "semestre": 3})

# Eliminación
del d["edad"]
d.pop("curso")        # devuelve valor y elimina
d.pop("no_existe", None)  # None si no existe (evita KeyError)
d.clear()

# Iteración
for clave in d:           # claves
    print(clave)

for valor in d.values():  # valores
    print(valor)

for k, v in d.items():    # pares
    print(k, v)
`

### Métodos útiles
`python
d = {"a": 1, "b": 2}
d.keys()      # dict_keys(['a', 'b'])
d.values()    # dict_values([1, 2])
d.items()     # dict_items([('a', 1), ('b', 2)])

# Dict comprehensions
{x: x**2 for x in range(5)}     # {0:0, 1:1, 2:4, 3:9, 4:16}
{k: v for k, v in d.items() if v > 1}  # filtrado

# Merge (Python 3.9+)
d1 = {"a": 1}
d2 = {"b": 2}
d1 | d2          # {"a": 1, "b": 2}
d1 |= d2         # in-place merge
`

### dict.fromkeys()
`python
dict.fromkeys(["a", "b", "c"], 0)  # {"a": 0, "b": 0, "c": 0}
`

### Defaultdict y Counter
`python
from collections import defaultdict, Counter

# defaultdict - valor por defecto automático
dd = defaultdict(list)
dd["frutas"].append("manzana")  # no KeyError

# Counter - conteo automático
contador = Counter("abracadabra")  # {'a': 5, 'b': 2, 'r': 2, 'c': 1, 'd': 1}
contador.most_common(2)  # [('a', 5), ('b', 2)]
`

---

## 4. Conjuntos (set y rozenset)

### Características
- **Únicos** (no duplicados)
- **No ordenados**
- **Mutables** (set) / **Inmutables** (rozenset)
- Operaciones matemáticas de conjuntos

`python
a = {1, 2, 3, 3, 2}  # {1, 2, 3}
b = {3, 4, 5}

# Operaciones
a | b   # Unión: {1, 2, 3, 4, 5}
a & b   # Intersección: {3}
a - b   # Diferencia: {1, 2}
a ^ b   # Diferencia simétrica: {1, 2, 4, 5}

# Mutables
s = {1, 2}
s.add(3)        # {1, 2, 3}
s.update([4,5]) # {1, 2, 3, 4, 5}
s.discard(2)    # {1, 3, 4, 5} (no error si no existe)
s.remove(3)     # {1, 4, 5} (KeyError si no existe)
s.pop()         # elimina y devuelve elemento arbitrario
s.clear()

# frozenset (inmutable, usable como clave de dict)
fs = frozenset([1, 2, 3])
d = {fs: "valor"}  # válido como clave
`

---

## 5. Diccionarios avanzados

### ChainMap (combinar diccionarios)
`python
from collections import ChainMap

defaults = {"color": "azul", "tamaño": "M"}
usuario = {"color": "rojo"}
config = ChainMap(usuario, defaults)

config["color"]    # "rojo" (usuario tiene prioridad)
config["tamaño"]   # "M" (defaults)
`

### OrderedDict (Python 3.7+ dict ya mantiene orden, pero OrderedDict tiene métodos extra)
`python
from collections import OrderedDict

od = OrderedDict()
od["a"] = 1
od["b"] = 2
od.move_to_end("a")  # mueve al final
od.popitem(last=False)  # elimina primero (FIFO)
`

---

## 6. Colas y pilas (stacks/queues)

### Stack (LIFO) - usar lista
`python
pila = []
pila.append(1)   # push
pila.append(2)
pila.pop()       # 2 (pop)
pila[-1]         # top (peek)
`

### Queue (FIFO) - collections.deque
`python
from collections import deque

cola = deque([1, 2, 3])
cola.append(4)      # enqueue
cola.popleft()      # 1 (dequeue - O(1))
cola.appendleft(0)  # push izquierda
cola.pop()          # pop derecha
cola.rotate(1)      # rota derecha
cola.rotate(-1)     # rota izquierda
`

### Priority Queue - heapq
`python
import heapq

cola = []
heapq.heappush(cola, (3, "tarea C"))
heapq.heappush(cola, (1, "tarea A"))
heapq.heappush(cola, (2, "tarea B"))

heapq.heappop(cola)  # (1, "tarea A") - menor prioridad primero
`

---

## 7. Arrays numéricos - rray y 
umpy

### rray (built-in, compacto)
`python
from array import array

enteros = array("i", [1, 2, 3, 4])  # "i" = signed int
flotantes = array("d", [1.0, 2.0])  # "d" = double
enteros.append(5)
`

### NumPy (recomendado para computación numérica)
`python
import numpy as np

arr = np.array([1, 2, 3, 4])
matriz = np.array([[1, 2], [3, 4]])

arr * 2           # [2, 4, 6, 8] (vectorizado)
matriz.T          # transpuesta
matriz @ matriz   # producto matriz
np.mean(arr)      # 2.5
np.std(arr)       # desviación estándar
`

---

## 8. Resumen comparativo

| Estructura | Mutable | Ordenado | Índice | Clave dict | Uso principal |
|---|---|---|---|---|---|
| **list** | ✅ | ✅ | ✅ | ❌ | Secuencia mutable general |
| **tuple** | ❌ | ✅ | ✅ | ✅* | Datos inmutables, claves dict |
| **dict** | ✅ | ✅** | ❌ | N/A | Mapeo clave-valor |
| **set** | ✅ | ❌ | ❌ | ❌ | Unicidad, operaciones de conjuntos |
| **frozenset** | ❌ | ❌ | ❌ | ✅ | Conjunto inmutable, clave dict |
| **deque** | ✅ | ✅ | ✅ | ❌ | Cola/cola doble eficiente |
| **heapq** | ✅ | Parcial | ❌ | ❌ | Cola de prioridad |
| **array** | ✅ | ✅ | ✅ | ❌ | Números compactos |
| **np.array** | ✅ | ✅ | ✅ | ❌ | Computación numérica |

*Si todos sus elementos son inmutables
**Python 3.7+ mantiene orden de inserción

---

## 📌 Resumen del tema

- [ ] Listas: CRUD, slicing, sorting, comprehensions, copias
- [ ] Tuplas: inmutables, desempaquetado, named tuples
- [ ] Diccionarios: CRUD, get(), comprehensions, merge, defaultdict/Counter
- [ ] Sets: unicidad, operaciones matemáticas, frozenset
- [ ] Estructuras avanzadas: defaultdict, Counter, ChainMap, deque, heapq
- [ ] Arrays numéricos: rray, 
umpy
- [ ] Elegir estructura adecuada según caso de uso

---

## 🔗 Recursos
- [Python Docs - Data Structures](https://docs.python.org/3/tutorial/datastructures.html)
- [Python Docs - collections](https://docs.python.org/3/library/collections.html)
- [Real Python - Lists and Tuples](https://realpython.com/python-lists-tuples/)
- [Real Python - Dictionaries](https://realpython.com/python-dicts/)
- [Real Python - Sets](https://realpython.com/python-sets/)
- [NumPy Tutorial](https://numpy.org/devdocs/user/quickstart.html)
