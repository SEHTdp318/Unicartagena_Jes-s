# Manejo de Archivos y Excepciones en Python

## 1. Trabajo con Archivos

### Apertura básica - open()
`python
# Modos principales
# r  = leer (default)
# w  = escribir (trunca/crea)
# a  = append (agrega al final)
# r+ = leer + escribir
# b  = binario (ej: rb, wb)
# t  = texto (default)

# Ejemplos
f = open("archivo.txt", "r")      # leer texto
f = open("datos.csv", "w")        # escribir (sobrescribe)
f = open("log.txt", "a")          # agregar al final
f = open("imagen.png", "rb")      # leer binario
`

### Context Manager (with) - **RECOMENDADO**
`python
# Cierre automático, incluso si hay excepción
with open("archivo.txt", "r", encoding="utf-8") as f:
    contenido = f.read()

# Múltiples archivos
with open("entrada.txt", "r") as fin, open("salida.txt", "w") as fout:
    for linea in fin:
        fout.write(linea.upper())
`

### Lectura
`python
with open("archivo.txt", "r", encoding="utf-8") as f:
    # Todo el contenido (string)
    texto = f.read()
    
    # Línea por línea (iterador perezoso)
    for linea in f:
        print(linea.strip())
    
    # Todas las líneas (lista)
    lineas = f.readlines()
    
    # Primera línea
    primera = f.readline()
`

### Escritura
`python
with open("salida.txt", "w", encoding="utf-8") as f:
    f.write("Línea 1\n")
    f.write("Línea 2\n")
    
    # writelines (no añade \n automático)
    lineas = ["a\n", "b\n", "c\n"]
    f.writelines(lineas)
    
    # print() redirigido
    print("Desde print", file=f)
`

### Modos binarios
`python
# Copiar archivo binario (imagen, PDF, etc.)
with open("origen.png", "rb") as origen, open("copia.png", "wb") as destino:
    destino.write(origen.read())

# O en chunks (para archivos grandes)
CHUNK = 8192
with open("grande.zip", "rb") as o, open("copia.zip", "wb") as d:
    while chunk := o.read(CHUNK):
        d.write(chunk)
`

---

## 2. Rutas de archivos - pathlib (Python 3.4+)

### Path - orientado a objetos
`python
from pathlib import Path

# Rutas
ruta = Path("carpeta") / "subcarpeta" / "archivo.txt"
ruta = Path.home() / "Documentos" / "archivo.txt"

# Propiedades
ruta.name           # "archivo.txt"
ruta.stem           # "archivo" (sin extensión)
ruta.suffix         # ".txt"
ruta.parent         # Path("carpeta/subcarpeta")
ruta.absolute()     # ruta absoluta

# Operaciones
ruta.exists()       # True/False
ruta.is_file()      # True/False
ruta.is_dir()       # True/False
ruta.stat()         # info (tamaño, fechas, etc.)
ruta.mkdir(parents=True, exist_ok=True)  # crear directorios
ruta.unlink()       # eliminar archivo
ruta.rmdir()        # eliminar directorio vacío

# Lectura/escritura directa
texto = ruta.read_text(encoding="utf-8")
ruta.write_text("Hola\n", encoding="utf-8")
bytes_data = ruta.read_bytes()
ruta.write_bytes(b"datos")

# Iterar directorio
for archivo in Path("carpeta").iterdir():
    print(archivo.name)

# Glob (patrones)
for py in Path(".").rglob("*.py"):
    print(py)
`

---

## 3. Formatos comunes

### CSV
`python
import csv

# Lectura
with open("datos.csv", "r", encoding="utf-8", newline="") as f:
    reader = csv.reader(f)
    for fila in reader:
        print(fila)  # lista de strings

# Con DictReader (acceso por nombre de columna)
with open("datos.csv", "r", encoding="utf-8", newline="") as f:
    reader = csv.DictReader(f)
    for fila in reader:
        print(fila["nombre"], fila["edad"])

# Escritura
with open("salida.csv", "w", encoding="utf-8", newline="") as f:
    writer = csv.writer(f)
    writer.writerow(["nombre", "edad"])
    writer.writerows([["Ana", 20], ["Juan", 22]])

# DictWriter
with open("salida.csv", "w", encoding="utf-8", newline="") as f:
    writer = csv.DictWriter(f, fieldnames=["nombre", "edad"])
    writer.writeheader()
    writer.writerows([{"nombre": "Ana", "edad": 20}])
`

### JSON
`python
import json

# Lectura
with open("config.json", "r", encoding="utf-8") as f:
    data = json.load(f)  # dict/list

# Escritura
with open("salida.json", "w", encoding="utf-8") as f:
    json.dump(data, f, ensure_ascii=False, indent=2)

# Serialización personalizada
class Punto:
    def __init__(self, x, y):
        self.x, self.y = x, y

def punto_a_dict(obj):
    if isinstance(obj, Punto):
        return {"__tipo__": "Punto", "x": obj.x, "y": obj.y}
    raise TypeError

json.dumps(Punto(1, 2), default=punto_a_dict, indent=2)
`

### Otros formatos
`python
# pickle (serialización Python nativa - NO seguro para datos no confiables)
import pickle
with open("datos.pkl", "wb") as f:
    pickle.dump(objeto, f)
with open("datos.pkl", "rb") as f:
    objeto = pickle.load(f)

# YAML (requiere pyyaml)
import yaml
with open("config.yaml") as f:
    data = yaml.safe_load(f)

# TOML (Python 3.11+ builtin: tomllib para leer, tomli_w para escribir)
import tomllib
with open("config.toml", "rb") as f:
    data = tomllib.load(f)
`

---

## 4. Excepciones

### Jerarquía básica
`
BaseException
 ├── KeyboardInterrupt
 ├── SystemExit
 └── Exception
      ├── ValueError
      ├── TypeError
      ├── IndexError
      ├── KeyError
      ├── FileNotFoundError
      ├── PermissionError
      ├── IOError (alias OSError en Py3)
      ├── ZeroDivisionError
      ├── ImportError
      └── ...
`

### Bloque 	ry / except / else / finally
`python
try:
    # código riesgoso
    resultado = 10 / 0
except ZeroDivisionError:
    print("No se puede dividir por cero")
except (ValueError, TypeError) as e:
    print(f"Error de valor/tipo: {e}")
except Exception as e:
    print(f"Otro error: {type(e).__name__}: {e}")
else:
    # se ejecuta SOLO si NO hubo excepción
    print("Operación exitosa")
finally:
    # SIEMPRE se ejecuta (limpieza)
    print("Fin del bloque")
`

### Capturar y relanzar
`python
try:
    risky_operation()
except ValueError as e:
    print(f"Error: {e}")
    raise  # relanza la MISMA excepción
    # raise TypeError("nuevo error") from e  # encadenar (Python 3+)
`

### aise - lanzar excepciones
`python
def dividir(a, b):
    if b == 0:
        raise ValueError("Divisor no puede ser cero")
    return a / b

# Excepciones personalizadas
class ErrorPropio(Exception):
    \"\"\"Excepción personalizada.\"\"\"
    def __init__(self, mensaje, codigo=0):
        super().__init__(mensaje)
        self.codigo = codigo

raise ErrorPropio("Algo falló", codigo=404)
`

---

## 4. Excepciones comunes y manejo

| Excepción | Causa típica | Manejo |
|---|---|---|
| FileNotFoundError | Archivo no existe | Verificar ruta, crear si necesario |
| PermissionError | Sin permisos | Ejecutar como admin / cambiar permisos |
| ValueError | Valor inválido (int("abc")) | Validar entrada antes de castear |
| TypeError | Tipo incorrecto ("a" + 1) | Verificar tipos antes de operar |
| IndexError | Índice fuera de rango | Verificar len() antes de acceder |
| KeyError | Clave no existe en dict | Usar .get() o in |
| ZeroDivisionError | División por cero | Verificar divisor != 0 |
| ImportError | Módulo no encontrado | Instalar paquete / verificar nombre |
| AttributeError | Atributo no existe | Verificar con hasattr() / getattr() |

---

## 5. Patrones de manejo robusto

### Validación de entrada
`python
def leer_entero_positivo(mensaje: str) -> int:
    while True:
        try:
            valor = int(input(mensaje))
            if valor <= 0:
                print("Debe ser positivo")
                continue
            return valor
        except ValueError:
            print("Entrada inválida. Ingresa un número entero.")

edad = leer_entero_positivo("Edad: ")
`

### Reintentos con backoff
`python
import time
from functools import wraps

def reintentar(max_intentos=3, espera=1, factor=2):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            intentos = 0
            while intentos < max_intentos:
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    intentos += 1
                    if intentos == max_intentos:
                        raise
                    print(f"Intento {intentos} falló: {e}. Reintentando en {espera}s...")
                    time.sleep(espera)
                    espera *= factor
        return wrapper
    return decorator

@reintentar(max_intentos=3, espera=1)
def conexion_inestable():
    # código que puede fallar temporalmente
    pass
`

### Context manager personalizado
`python
from contextlib import contextmanager

@contextmanager
def temporizador(nombre):
    import time
    inicio = time.time()
    try:
        yield
    finally:
        print(f"{nombre} tardó {time.time() - inicio:.3f}s")

with temporizador("procesamiento"):
    # código a medir
    pass
`

---

## 5. Logging (reemplaza print para debugging/producción)

`python
import logging

# Configuración básica
logging.basicConfig(
    level=logging.DEBUG,
    format="%(asctime)s | %(levelname)-8s | %(name)s | %(message)s",
    datefmt="%H:%M:%S"
)

logger = logging.getLogger(__name__)

logger.debug("Detalle de depuración")
logger.info("Información general")
logger.warning("Advertencia")
logger.error("Error recuperable")
logger.critical("Error crítico")

# En except
try:
    1 / 0
except ZeroDivisionError:
    logger.exception("División por cero")  # incluye traceback
`

### Configuración avanzada (archivo + consola)
`python
import logging.config

LOGGING_CONFIG = {
    "version": 1,
    "formatters": {
        "detallado": {
            "format": "%(asctime)s | %(levelname)-8s | %(name)s | %(message)s"
        }
    },
    "handlers": {
        "console": {
            "class": "logging.StreamHandler",
            "formatter": "detallado",
            "level": "INFO"
        },
        "archivo": {
            "class": "logging.FileHandler",
            "filename": "app.log",
            "formatter": "detallado",
            "level": "DEBUG"
        }
    },
    "root": {
        "handlers": ["console", "archivo"],
        "level": "DEBUG"
    }
}

logging.config.dictConfig(LOGGING_CONFIG)
`

---

## 6. Buenas prácticas

| Práctica | Descripción |
|---|---|
| **Usa with** | Cierre automático de recursos |
| **Especifica encoding** | encoding="utf-8" siempre en texto |
| **Captura excepciones específicas** | No except: vacío |
| **No silencies excepciones** | Al menos loggea |
| **Usa else en try** | Código que solo corre si éxito |
| **Usa inally** | Limpieza garantizada |
| **Excepciones personalizadas** | Para errores de dominio |
| **Logging > print** | En producción |
| **Valida temprano** | Fail fast |
| **No uses excepciones para control de flujo normal** | Solo para errores |

---

## 📌 Resumen del tema

- [ ] open() con with y encoding="utf-8"
- [ ] Lectura: ead(), eadline(), eadlines(), iteración
- [ ] Escritura: write(), writelines(), print(file=f)
- [ ] pathlib.Path para rutas modernas
- [ ] CSV, JSON, pickle, YAML, TOML
- [ ] 	ry / except / else / finally
- [ ] Capturar excepciones específicas
- [ ] aise y excepciones personalizadas
- [ ] Logging en lugar de print
- [ ] Patrones robustos: validación, reintentos, context managers

---

## 🔗 Recursos
- [Python Docs - File I/O](https://docs.python.org/3/tutorial/inputoutput.html)
- [Python Docs - Exceptions](https://docs.python.org/3/tutorial/errors.html)
- [Python Docs - pathlib](https://docs.python.org/3/library/pathlib.html)
- [Real Python - File I/O](https://realpython.com/read-write-files-python/)
- [Real Python - Exceptions](https://realpython.com/python-exceptions/)
- [Real Python - Logging](https://realpython.com/python-logging/)
