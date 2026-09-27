# Guía de estudio - API de Matrícula

## Estado de la entrega

- Repositorio: https://github.com/genesismachado-png/Matricula-api-valeria
- UML: `uml.png`
- API modular: `app/main.py`, `app/database.py`, `app/schemas.py`, `app/routers/` y `app/services/`
- Pruebas verificadas: `12 passed`
- No subir: `venv`, `.venv`, `__pycache__`, `.pytest_cache` ni `sistema_academico.db`

## Cómo ejecutar

```powershell
cd "C:\Users\valer\Downloads\API_Sistema_Academico_con_Login_Dashboard\API_Sistema_Academico_FastAPI_SQLite"
python -m uvicorn main:app --reload
```

Abrir:

- http://127.0.0.1:8000
- http://127.0.0.1:8000/docs
- http://127.0.0.1:8000/salud

## Cómo explicar la arquitectura

1. `main.py` importa la aplicación desde `app.main`.
2. `app/main.py` crea FastAPI, inicializa la base de datos y registra los routers.
3. `app/database.py` abre conexiones SQLite, crea tablas y controla transacciones.
4. `app/schemas.py` valida entradas y respuestas.
5. `app/routers/` recibe las peticiones HTTP y construye respuestas.
6. `app/services/matricula_service.py` aplica las reglas de negocio.
7. `frontend/` contiene el login y el dashboard que la API sirve en `/`.

## Ejemplo: crear una matrícula

La petición llega a `app/routers/matriculas.py`, valida el esquema, llama al servicio de matrícula, consulta SQLite y devuelve JSON. El servicio comprueba estudiante activo, período activo, sección abierta, carrera, requisitos, cupo, unidades valorativas y choques de horario.

## Reglas que debes poder explicar

- No se repiten cuenta, correo ni códigos únicos.
- Un estudiante inactivo no puede matricularse.
- Solo se matricula en un período activo y sección abierta.
- No se repite una matrícula.
- No se supera el cupo ni 20 unidades valorativas.
- No hay choques de horario.
- Docente y aula no se asignan a dos secciones simultáneas.
- Cancelar cambia el estado y libera el cupo sin borrar el historial.
- Las consultas usan `?` para parametrizar valores y evitar inyección SQL.

## Preguntas probables

### ¿Qué hace `APIRouter`?
Agrupa endpoints relacionados para registrarlos después en la aplicación principal con `include_router`.

### ¿Por qué usar esquemas?
Para validar tipos, identificadores y datos recibidos antes de ejecutar SQL.

### ¿Por qué usar transacciones?
Para confirmar todos los cambios juntos o revertirlos si ocurre un error.

### ¿Qué diferencia hay entre router y service?
El router atiende HTTP; el service concentra reglas de negocio reutilizables.

### ¿Cómo se calcula una nota?
Se guardan tres parciales, se calcula la nota final y se cambia el estado a `APROBADA` si alcanza 65; de lo contrario, `REPROBADA`.

### ¿Cómo se protege una contraseña?
Se guarda un hash SHA-256 en `clave_hash`; nunca se compara ni se guarda la contraseña directamente.

## Diferencia con la guía del ingeniero

La guía menciona una API inicial monolítica y nombres como `seguridad.py`, `esquemas.py`, `cursos.py`, `dashboard.py` y `paginas.py`. Este repositorio contiene una versión más completa y ya modularizada, con `auth.py`, `schemas.py`, `asignaturas.py`, `reportes.py` y frontend estático. No conviene renombrar módulos sin tener el `main.py` original del ingeniero porque podría romper rutas y pruebas.

## Antes de entregar

- Confirmar que el enlace abre en GitHub.
- Mostrar `uml.png`.
- Ejecutar la API y abrir `/docs`.
- Ejecutar `python -m pytest -q`.
- Poder explicar una matrícula de principio a fin.
