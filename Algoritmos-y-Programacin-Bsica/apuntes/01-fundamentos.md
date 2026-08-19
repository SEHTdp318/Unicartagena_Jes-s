# Fundamentos de Programación y Algoritmos

## 1. ¿Qué es un algoritmo?

**Definición:** Secuencia finita, ordenada y no ambigua de instrucciones para resolver un problema.

### Características esenciales
| Característica | Descripción |
|---|---|
| **Finitud** | Termina en pasos finitos |
| **Definición precisa** | Sin ambigüedad |
| **Entrada** | 0+ datos de entrada |
| **Salida** | 1+ resultados |
| **Efectividad** | Ejecutable en tiempo finito |

### Ejemplo: Algoritmo para hacer café
`
INICIO
  agua ← hervir_agua()
  café ← moler_granos()
  verter(agua, café)
  esperar(3 min)
  colar()
  servir()
FIN
`

---

## 2. Representación de algoritmos

| Método | Uso | Ejemplo |
|---|---|---|
| **Pseudocódigo** | Diseño independiente de lenguaje | SI edad >= 18 ENTONCES ... |
| **Diagrama de flujo** | Visualización (ISO 5807) | Óvalos, rombos, rectángulos |
| **Código real** | Implementación final | Python, C++, Java |

### Pseudocódigo - Convenciones
`
VARIABLES
  entero edad
  real precio
  cadena nombre

INICIO
  LEER edad
  SI edad >= 18 ENTONCES
    ESCRIBIR "Mayor de edad"
  SINO
    ESCRIBIR "Menor de edad"
  FIN SI
FIN
`

---

## 3. Pensamiento computacional

| Pilar | Descripción |
|---|---|
| **Descomposición** | Dividir problema en subproblemas menores |
| **Reconocimiento de patrones** | Identificar similitudes |
| **Abstracción** | Ignorar detalles irrelevantes |
| **Diseño de algoritmos** | Crear solución paso a paso |

---

## 4. Complejidad básica (intro)

| Notación | Significado | Ejemplo |
|---|---|---|
| **O(1)** | Constante | Acceso a array[i] |
| **O(log n)** | Logarítmica | Búsqueda binaria |
| **O(n)** | Lineal | Búsqueda lineal |
| **O(n²)** | Cuadrática | Bucles anidados |

> **Nota:** En este curso nos enfocamos en corrección y legibilidad primero; optimización en cursos avanzados.

---

## 5. Herramientas del curso

| Herramienta | Propósito | Instalación |
|---|---|---|
| **Python 3.11+** | Intérprete | python.org / winget install Python.Python.3.11 |
| **VS Code** | Editor | code.visualstudio.com |
| **Extensiones VS Code** | Python, Pylance, Python Debugger, GitLens | Extensions panel |
| **Git** | Control de versiones | git-scm.com / winget install Git.Git |
| **GitHub** | Repositorio remoto | github.com |

---

## 5. Primer programa en Python

`python
# hola_mundo.py
print("¡Hola, Algoritmos y Programación!")

# Variables
nombre = "Jesús"
edad = 20
es_estudiante = True

print(f"{nombre} tiene {edad} años. ¿Es estudiante? {es_estudiante}")
`

### Ejecutar
`ash
python hola_mundo.py
`

---

## 📌 Resumen del tema

- [ ] Definir algoritmo y sus características
- [ ] Representar algoritmos en pseudocódigo y diagrama de flujo
- [ ] Aplicar pensamiento computacional
- [ ] Instalar y configurar entorno Python + VS Code
- [ ] Ejecutar primer script en Python

---

## 🔗 Recursos
- [Guía oficial UNR - Introducción](https://mpru.github.io/introprog/introducci%C3%B3n-a-la-programaci%C3%B3n.html)
- [Python Tutorial Oficial](https://docs.python.org/3/tutorial/)
- [Diagramas de flujo - ISO 5807](https://es.wikipedia.org/wiki/Diagrama_de_flujo)
