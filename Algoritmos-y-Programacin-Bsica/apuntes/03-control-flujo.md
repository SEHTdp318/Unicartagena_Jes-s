# Estructuras de Control de Flujo en Python

## 1. Condicionales

### if simple
`python
edad = int(input("Edad: "))

if edad >= 18:
    print("Mayor de edad")
`

### if - else
`python
nota = float(input("Nota: "))

if nota >= 3.0:
    print("Aprobado")
else:
    print("Reprobado")
`

### if - elif - else
`python
nota = float(input("Nota (0-5): "))

if nota >= 4.5:
    print("Excelente")
elif nota >= 3.5:
    print("Bueno")
elif nota >= 3.0:
    print("Aprobado")
else:
    print("Reprobado")
`

> **Importante:** Python usa **indentación (4 espacios)** para definir bloques. No usa llaves {}.

### Expresión condicional (ternario)
`python
# valor_si_verdadero if condicion else valor_si_falso
estado = "Aprobado" if nota >= 3.0 else "Reprobado"
mensaje = "Par" if numero % 2 == 0 else "Impar"
`

### match - case (Python 3.10+)
`python
comando = input("Comando: ").strip().lower()

match comando:
    case "iniciar":
        print("Iniciando...")
    case "pausar":
        print("Pausado")
    case "detener":
        print("Detenido")
    case "ayuda" | "help":
        print("Comandos: iniciar, pausar, detener, ayuda")
    case _:
        print("Comando no reconocido")
`

---

## 2. Bucles

### while - mientras condición sea verdadera
`python
# Contador
contador = 0
while contador < 5:
    print(f"Contador: {contador}")
    contador += 1  # incremento

# Con break / continue
while True:
    entrada = input("Escribe 'salir' para terminar: ")
    if entrada == "salir":
        break  # sale del bucle
    if entrada == "":
        continue  # salta a la siguiente iteración
    print(f"Escribiste: {entrada}")
`

### or - iterar sobre secuencias
`python
# Iterar lista
frutas = ["manzana", "banana", "cereza"]
for fruta in frutas:
    print(fruta)

# Con range()
for i in range(5):        # 0,1,2,3,4
    print(i)

for i in range(2, 10, 2): # 2,4,6,8 (inicio, fin, paso)
    print(i)

# Con enumerate() - índice + valor
for i, fruta in enumerate(frutas):
    print(f"{i}: {fruta}")

# Iterar diccionario
estudiante = {"nombre": "Jesús", "edad": 20, "curso": "Ing. Software"}
for clave, valor in estudiante.items():
    print(f"{clave}: {valor}")

# Con zip() - iterar múltiples secuencias
nombres = ["Ana", "Juan", "María"]
edades = [20, 22, 19]
for nombre, edad in zip(nombres, edades):
    print(f"{nombre} tiene {edad} años")
`

### else en bucles (se ejecuta si NO hubo reak)
`python
for n in range(2, 10):
    for x in range(2, n):
        if n % x == 0:
            break
    else:
        print(f"{n} es primo")
`

### reak y continue
| Palabra | Efecto |
|---|---|
| reak | Sale **inmediatamente** del bucle más interno |
| continue | Salta a la **siguiente iteración** del bucle |

---

## 3. Excepciones básicas en control de flujo

`python
try:
    edad = int(input("Edad: "))
except ValueError:
    print("Debes ingresar un número entero")
else:
    print(f"Edad registrada: {edad}")
finally:
    print("Fin del bloque")
`

---

## 4. Expresiones generadoras (comprehensions)

### List comprehensions
`python
# [expresion for item in iterable if condicion]
cuadrados = [x**2 for x in range(10)]           # [0,1,4,9,16,25,36,49,64,81]
pares = [x for x in range(20) if x % 2 == 0]    # [0,2,4,6,8,10,12,14,16,18]
pares_cuadrados = [x**2 for x in range(10) if x % 2 == 0]  # [0,4,16,36,64]
`

### Dict comprehensions
`python
cuadrados_dict = {x: x**2 for x in range(5)}    # {0:0, 1:1, 2:4, 3:9, 4:16}
`

### Set comprehensions
`python
pares_set = {x for x in range(10) if x % 2 == 0}  # {0, 2, 4, 6, 8}
`

### Generator expressions (perezosos)
`python
gen = (x**2 for x in range(10))  # generator object
next(gen)  # 0
next(gen)  # 1
list(gen)  # [4, 9, 16, 25, 36, 49, 64, 81]
`

---

## 5. Funciones útiles para control de flujo

| Función | Descripción | Ejemplo |
|---|---|---|
| ange(inicio, fin, paso) | Genera secuencia de enteros | ange(5) → 0,1,2,3,4 |
| enumerate(iterable, inicio=0) | Índice + valor | enumerate(["a","b"]) → (0,"a"), (1,"b") |
| zip(*iterables) | Combina iterables | zip([1,2], ["a","b"]) → (1,"a"), (2,"b") |
| enumerate(zip(...)) | Índice + múltiples valores | |
| eversed(seq) | Iterador inverso | eversed([1,2,3]) → 3,2,1 |
| sorted(iterable, key=, reverse=) | Lista ordenada nueva | sorted([3,1,2]) → [1,2,3] |
| ny(iterable) | True si algún elemento es truthy | ny([0, "", 5]) → True |
| ll(iterable) | True si todos son truthy | ll([1, 2, 3]) → True |

---

## 6. Patrones comunes

### Menú interactivo
`python
def menu():
    opciones = {
        "1": ("Saludar", lambda: print("¡Hola!")),
        "2": ("Despedir", lambda: print("¡Adiós!")),
        "3": ("Salir", None)
    }
    
    while True:
        print("\n--- MENÚ ---")
        for k, (desc, _) in opciones.items():
            print(f"{k}. {desc}")
        
        opcion = input("Elige: ")
        if opcion == "3":
            break
        if opcion in opciones:
            accion = opciones[opcion][1]
            if accion:
                accion()
        else:
            print("Opción inválida")
`

### Validación de entrada robusta
`python
def leer_entero(mensaje, minimo=None, maximo=None):
    while True:
        try:
            valor = int(input(mensaje))
            if minimo is not None and valor < minimo:
                print(f"Debe ser >= {minimo}")
                continue
            if maximo is not None and valor > maximo:
                print(f"Debe ser <= {maximo}")
                continue
            return valor
        except ValueError:
            print("Entrada inválida. Intenta de nuevo.")

edad = leer_entero("Edad (18-100): ", 18, 100)
`

---

## 📌 Resumen del tema

- [ ] if / elif / else con indentación correcta
- [ ] match - case (Python 3.10+)
- [ ] while con reak / continue
- [ ] or sobre listas, ange(), enumerate(), zip(), dict.items()
- [ ] else en bucles (ejecuta si no hubo reak)
- [ ] 	ry / except / else / finally
- [ ] List/Dict/Set comprehensions
- [ ] Patrones: menú, validación de entrada

---

## 🔗 Recursos
- [Python Docs - Control Flow](https://docs.python.org/3/tutorial/controlflow.html)
- [Python Docs - Compound Statements](https://docs.python.org/3/reference/compound_stmts.html)
- [Real Python - Conditional Logic](https://realpython.com/python-conditional-statements/)
- [Real Python - Loops](https://realpython.com/python-for-loop/)
