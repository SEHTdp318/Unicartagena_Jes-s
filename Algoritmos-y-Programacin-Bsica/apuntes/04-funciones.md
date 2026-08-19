# Funciones en Python

## 1. Definición y llamada básica

`python
def saludar(nombre):
    """Saluda a una persona."""
    return f"¡Hola, {nombre}!"

mensaje = saludar("Jesús")
print(mensaje)  # ¡Hola, Jesús!
`

### Estructura
`python
def nombre_funcion(parametro1, parametro2, ...):
    \"\"\"Docstring: describe qué hace la función.\"\"\"
    # cuerpo de la función
    return valor_retorno  # opcional, por defecto None
`

---

## 2. Parámetros

### Posicionales
`python
def sumar(a, b):
    return a + b

sumar(3, 5)  # 8
`

### Por palabra clave (keyword arguments)
`python
def saludar(nombre, saludo="Hola"):
    return f"{saludo}, {nombre}!"

saludar("Jesús")           # Hola, Jesús!
saludar("Jesús", "Hola")   # Hola, Jesús!
saludar(saludo="Hi", nombre="Jesús")  # Hi, Jesús!
`

### Parámetros por defecto
`python
def crear_usuario(nombre, edad=18, activo=True):
    return {"nombre": nombre, "edad": edad, "activo": activo}

crear_usuario("Ana")                    # edad=18, activo=True
crear_usuario("Carlos", 25)             # edad=25, activo=True
crear_usuario("María", activo=False)    # edad=18, activo=False
`

> **Regla:** Parámetros con default **después** de los sin default.

### *args - argumentos posicionales variables
`python
def sumar_todos(*numeros):
    return sum(numeros)

sumar_todos(1, 2, 3)        # 6
sumar_todos(1, 2, 3, 4, 5)  # 15
sumar_todos()               # 0
`

### **kwargs - argumentos por palabra clave variables
`python
def crear_perfil(nombre, **datos):
    perfil = {"nombre": nombre}
    perfil.update(datos)
    return perfil

crear_perfil("Jesús", edad=20, curso="Ing. Software", ciudad="Cartagena")
# {'nombre': 'Jesús', 'edad': 20, 'curso': 'Ing. Software', 'ciudad': 'Cartagena'}
`

### Combinando todo
`python
def funcion_completa(obligatorio, por_defecto="valor", *args, **kwargs):
    print(f"Obligatorio: {obligatorio}")
    print(f"Por defecto: {por_defecto}")
    print(f"Args: {args}")
    print(f"Kwargs: {kwargs}")

funcion_completa("req", "def", 1, 2, 3, a=1, b=2)
`

### Parámetros solo por posición / solo por palabra clave (Python 3.8+)
`python
def funcion(pos_only, /, normal, *, kw_only):
    print(f"pos_only: {pos_only}, normal: {normal}, kw_only: {kw_only}")

funcion(1, 2, kw_only=3)      # OK
funcion(1, normal=2, kw_only=3)  # OK
# funcion(pos_only=1, normal=2, kw_only=3)  # Error: pos_only es positional-only
`

---

## 2. Retorno de valores

### Return simple
`python
def sumar(a, b):
    return a + b
`

### Múltiples valores (tupla implícita)
`python
def dividir(dividendo, divisor):
    cociente = dividendo // divisor
    resto = dividendo % divisor
    return cociente, resto

c, r = dividir(10, 3)  # c=3, r=1
`

### Return temprano (guard clauses)
`python
def dividir(a, b):
    if b == 0:
        return None, "Error: división por cero"
    return a / b, None

resultado, error = dividir(10, 0)
if error:
    print(error)
`

### Funciones que no retornan (procedimientos)
`python
def imprimir_lista(lista):
    for item in lista:
        print(item)
    # return implícito None
`

---

## 3. Scope (alcance de variables)

### LEGB Rule (orden de búsqueda)
`
Local → Enclosing → Global → Built-in
`

`python
x = "global"

def externa():
    x = "enclosing"
    
    def interna():
        x = "local"
        print(x)  # local
    
    interna()
    print(x)  # enclosing

externa()
print(x)  # global
`

### global y 
onlocal
`python
contador = 0

def incrementar():
    global contador
    contador += 1

def externa():
    x = 10
    def interna():
        nonlocal x
        x = 20
    interna()
    print(x)  # 20
`

---

## 3. Funciones como ciudadanos de primera clase

### Asignar a variable
`python
def sumar(a, b):
    return a + b

operacion = sumar
resultado = operacion(3, 5)  # 8
`

### Pasar como argumento (higher-order functions)
`python
def aplicar_operacion(a, b, operacion):
    return operacion(a, b)

def sumar(a, b): return a + b
def restar(a, b): return a - b

aplicar_operacion(10, 5, sumar)   # 15
aplicar_operacion(10, 5, restar)  # 5
`

### Funciones anónimas - lambda
`python
# lambda parametros: expresion
cuadrado = lambda x: x ** 2
sumar = lambda a, b: a + b

cuadrado(5)    # 25
sumar(3, 4)    # 7

# Útil con higher-order functions
numeros = [1, 2, 3, 4, 5]
cuadrados = list(map(lambda x: x**2, numeros))  # [1, 4, 9, 16, 25]
pares = list(filter(lambda x: x % 2 == 0, range(10)))  # [0, 2, 4, 6, 8]
`

### Closures (clausuras)
`python
def crear_contador():
    cuenta = 0
    def contador():
        nonlocal cuenta
        cuenta += 1
        return cuenta
    return contador

c1 = crear_contador()
c2 = crear_contador()

c1()  # 1
c1()  # 2
c2()  # 1 (independiente)
`

### Decoradores (Python 3.9+)
`python
def temporizador(func):
    import time
    def wrapper(*args, **kwargs):
        inicio = time.time()
        resultado = func(*args, **kwargs)
        fin = time.time()
        print(f"{func.__name__} tardó {fin - inicio:.4f}s")
        return resultado
    return wrapper

@temporizador
def tarea_lenta():
    import time
    time.sleep(1)
    return "listo"

tarea_lenta()  # tarea_lenta tardó 1.0001s
`

---

## 4. Documentación y type hints (Python 3.5+)

### Docstrings (Google/NumPy style)
`python
def calcular_promedio(notas: list[float]) -> float:
    \"\"\"
    Calcula el promedio de una lista de notas.
    
    Args:
        notas: Lista de notas (float)
        
    Returns:
        float: Promedio calculado
        
    Raises:
        ValueError: Si la lista está vacía
    \"\"\"
    if not notas:
        raise ValueError("La lista no puede estar vacía")
    return sum(notas) / len(notas)
`

### Type hints (Python 3.9+)
`python
from typing import List, Dict, Optional, Union

def procesar_estudiantes(
    nombres: List[str],
    edades: Dict[str, int],
    umbral: Optional[int] = None
) -> List[str]:
    \"\"\"Filtra estudiantes mayores a umbral.\"\"\"
    if umbral is None:
        umbral = 18
    return [n for n, e in edades.items() if e >= umbral]
`

---

## 4. Recursividad

### Ejemplo: Factorial
`python
def factorial(n: int) -> int:
    \"\"\"Calcula n! recursivamente.\"\"\"
    if n <= 1:
        return 1  # caso base
    return n * factorial(n - 1)  # paso recursivo

factorial(5)  # 120
`

### Fibonacci (con memoization - Python 3.9+)
`python
from functools import lru_cache

@lru_cache(maxsize=None)
def fibonacci(n: int) -> int:
    if n <= 1:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

fibonacci(10)  # 55
`

> **Nota:** Python tiene límite de recursión (~1000). Para profundidad mayor, usar iterativo o sys.setrecursionlimit().

---

## 5. Buenas prácticas

| Práctica | Descripción |
|---|---|
| **Una responsabilidad** | Una función = una tarea |
| **Nombres descriptivos** | calcular_promedio() no calc() |
| **Docstrings** | Siempre en funciones públicas |
| **Type hints** | En funciones públicas |
| **Evitar side effects** | Funciones puras cuando sea posible |
| **DRY** | No repetir código → extraer a función |
| **Pequeñas** | < 50 líneas idealmente |
| **Testeable** | Fácil de probar con unit tests |

---

## 📌 Resumen del tema

- [ ] Definir funciones con def, parámetros, retorno
- [ ] Usar *args, **kwargs, parámetros por defecto
- [ ] Entender scope: LEGB, global, 
onlocal
- [ ] Funciones como first-class: lambda, higher-order, closures
- [ ] Decoradores básicos
- [ ] Type hints y docstrings
- [ ] Recursividad y memoization

---

## 🔗 Recursos
- [Python Docs - Functions](https://docs.python.org/3/tutorial/controlflow.html#defining-functions)
- [Python Docs - Lambda](https://docs.python.org/3/tutorial/controlflow.html#lambda-expressions)
- [Real Python - Functions](https://realpython.com/defining-your-own-python-function/)
- [Real Python - Decorators](https://realpython.com/primer-on-python-decorators/)
- [PEP 484 - Type Hints](https://peps.python.org/pep-0484/)
