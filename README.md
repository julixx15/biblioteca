# 📚 Sistema de Biblioteca — Pruebas Top Down con Stubs

Este proyecto implementa un sistema de biblioteca en Python como ejercicio práctico de **pruebas Top Down**, utilizando **stubs** para simular módulos que aún no han sido desarrollados (como autenticación y base de datos). Las pruebas están escritas con `pytest`.

---

## 🎯 Objetivo del Proyecto

- Aplicar la técnica de **pruebas Top Down** en un sistema realista.
- Simular módulos incompletos con **stubs** para permitir la verificación temprana del flujo de control del sistema principal.
- Validar funcionalidades críticas como autorización de usuarios, disponibilidad de libros y registro de préstamos, incluso sin tener módulos reales implementados.

---

## 🛠️ Tecnologías y Herramientas

- **Python 3.8+**
- **pytest** (para ejecutar las pruebas unitarias)
- **VS Code** u otro editor de texto
- Terminal o consola para ejecutar comandos

---

## 🧩 Estructura del Proyecto

proyecto_biblioteca/
├── biblioteca_sistema.py # Módulo principal del sistema
├── test_top_down.py # Pruebas unitarias que usan stubs
├── README.md # Documentación completa del proyecto
└── stubs/
├── init.py # Inicializa el paquete de stubs
├── database_stub.py # Stub para simular base de datos
└── auth_stub.py # Stub para simular autenticación

python
Copiar código

---

## 📄 Descripción de los Componentes

### ✅ `biblioteca_sistema.py`

Contiene la clase `BibliotecaSistema`, que incluye la lógica principal para prestar libros. El sistema realiza tres pasos:

1. **Verificar autorización del usuario**
2. **Verificar disponibilidad del libro**
3. **Registrar el préstamo**

La clase recibe los módulos `db` y `auth` por **inyección de dependencias**, permitiendo reemplazarlos por stubs o versiones reales.

```python
class BibliotecaSistema:
    def __init__(self, db, auth):
        self.db = db
        self.auth = auth

    def prestar_libro(self, usuario_id, libro_id):
        if not self.auth.verificar_usuario(usuario_id):
            return "Usuario no autorizado"
        if not self.db.libro_disponible(libro_id):
            return "Libro no disponible"
        self.db.registrar_prestamo(usuario_id, libro_id)
        return "Préstamo exitoso"
✅ stubs/database_stub.py
Simula el módulo de base de datos. Contiene dos métodos:

libro_disponible(libro_id): Devuelve True si el ID del libro es par (simula disponibilidad).

registrar_prestamo(usuario_id, libro_id): Simula el registro exitoso del préstamo e imprime un mensaje de consola.

python
Copiar código
class DatabaseStub:
    def libro_disponible(self, libro_id):
        return libro_id % 2 == 0

    def registrar_prestamo(self, usuario_id, libro_id):
        print(f"[STUB] Préstamo: User={usuario_id}, Book={libro_id}")
        return True
✅ stubs/auth_stub.py
Simula el sistema de autenticación. Contiene un único método:

verificar_usuario(usuario_id): Devuelve True si el ID es mayor que 0 (simula usuario autorizado).

python
Copiar código
class AuthStub:
    def verificar_usuario(self, usuario_id):
        return usuario_id > 0
🧪 Pruebas Unitarias — test_top_down.py
Contiene pruebas del sistema utilizando los stubs creados. Permite probar el flujo principal sin necesidad de módulos reales.

Pruebas Implementadas:
🔹 test_prestamo_exitoso()
Usuario autorizado: usuario_id = 1

Libro disponible: libro_id = 2 (par)

Resultado esperado: "Préstamo exitoso"

🔹 test_usuario_no_autorizado()
Usuario no autorizado: usuario_id = 0

Resultado esperado: "Usuario no autorizado"

python
Copiar código
import pytest
from biblioteca_sistema import BibliotecaSistema
from stubs.database_stub import DatabaseStub
from stubs.auth_stub import AuthStub

def test_prestamo_exitoso():
    db_stub = DatabaseStub()
    auth_stub = AuthStub()
    sistema = BibliotecaSistema(db_stub, auth_stub)
    resultado = sistema.prestar_libro(usuario_id=1, libro_id=2)
    assert resultado == "Préstamo exitoso"

def test_usuario_no_autorizado():
    db_stub = DatabaseStub()
    auth_stub = AuthStub()
    sistema = BibliotecaSistema(db_stub, auth_stub)
    resultado = sistema.prestar_libro(usuario_id=0, libro_id=2)
    assert resultado == "Usuario no autorizado"
🧰 Instalación y Configuración
1. Clonar o descargar el repositorio
bash
Copiar código
git clone <url-del-repositorio>
cd proyecto_biblioteca
O simplemente crea los archivos y carpetas manualmente como se indica en la estructura.

2. Instalar pytest
Si no tienes pytest, instálalo con:

bash
Copiar código
python -m pip install pytest
En Mac/Linux o si usas Python 3 específicamente:

bash
Copiar código
python3 -m pip install pytest
3. Ejecutar las pruebas
Asegúrate de estar dentro del directorio del proyecto (proyecto_biblioteca) y ejecuta:

bash
Copiar código
python -m pytest test_top_down.py -v --tb=short
✅ Resultado Esperado:
bash
Copiar código
================= test session starts =================
test_top_down.py::test_prestamo_exitoso PASSED [50%]
test_top_down.py::test_usuario_no_autorizado PASSED [50%]
================= 2 passed in 0.03s ===================
📌 Ventajas del Enfoque Top Down
Permite probar la lógica del sistema principal incluso antes de tener todos los módulos implementados.

Facilita el desarrollo por etapas.

Aísla errores más rápidamente, ya que los stubs tienen comportamiento controlado.

Fomenta el diseño modular gracias a la inyección de dependencias.

📦 Siguientes pasos recomendados
🔄 Reemplazar los stubs por implementaciones reales:

Autenticación basada en base de datos

Registro de préstamos persistente

➕ Agregar más pruebas:

Préstamo con libro no disponible

Usuario autorizado pero libro no existente

🧪 Probar casos borde y errores inesperados

👨‍🏫 Créditos
Este proyecto es parte de una práctica educativa sobre técnicas de pruebas de software utilizando Python. Está diseñado para enseñar el enfoque Top Down Testing aplicando buenas prácticas de modularización y pruebas unitarias.

⚖️ Licencia
Este proyecto puede ser usado, modificado y distribuido libremente con fines educativos.
