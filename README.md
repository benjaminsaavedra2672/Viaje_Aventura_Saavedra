# Viajes Aventura

Proyecto de la asignatura **TI3021 – Programación Orientada a Objeto Seguro**, Unidad 4 (ES4).

## Contexto

La agencia Viajes Aventura necesita gestionar destinos, paquetes turísticos, usuarios y reservas, evitando procesos manuales y protegiendo los datos de sus clientes.

## Estado actual: base del Sprint 1

**Implementado:** entrada `main.py`, conexión SQLite, creación automática de las siete tablas, integridad referencial y prueba repetible de persistencia. El menú inicial solo permite comprobar la conexión y la estructura de la BD.

**Pendiente:** CRUD de destinos/paquetes, registro e inicio de sesión, autorización por roles, reservas, consulta de avances, modelos UML/BPMN y pruebas funcionales. No se debe considerar este proyecto como sistema final.

## Requisitos

- Python 3.10 o superior.
- SQLite (incluido en Python mediante `sqlite3`).
- No requiere instalar paquetes de terceros para esta base inicial.

## Estructura

```text
Viaje_Aventura_Saavedra/
├── README.md
├── .gitignore
├── .env.example
├── docs/
├── src/
│   ├── main.py
│   └── persistence/
│       ├── connection.py
```

## Ejecución

Desde la carpeta raíz del proyecto:

```bash
python src/main.py
```

En Windows también puedes usar `py src/main.py`. Al arrancar se crea automáticamente `data/viajes_aventura.db` si no existe. Elige **1** para verificar la conexión y ver las siete tablas; **2** para mostrar la ruta de la base de datos y **0** para salir.

### Prueba repetible

```bash
python -m unittest discover -s tests -v
```

La prueba crea una base SQLite temporal, inicializa dos veces el esquema para comprobar que no falla y verifica que una reserva con claves foráneas inexistentes sea rechazada. No modifica la base de datos principal.

## Configuración

La ruta de la base puede personalizarse con la variable de entorno `DATABASE_PATH` (ejemplo en `.env.example`). El programa lee variables del entorno del sistema; **no carga automáticamente** el archivo `.env`. El archivo `.env` real y la base `.db` deben mantenerse fuera de Git.

## Modelo inicial de datos

- `users`: clientes y administradores, con `role` (`admin` o `client`) y `password_hash`.
- `destinations`: destinos y costos.
- `packages`: paquetes, fechas, costo y capacidad.
- `package_destinations`: relación N:M entre paquetes y destinos.
- `reservations`: reservas de cada usuario para un paquete.
- `trip_stages`: etapas planificadas de cada paquete.
- `reservation_stages`: estado y fechas reales de cada etapa para una reserva.

Relaciones: `users` 1:N `reservations`; `packages` 1:N `reservations`; `packages` 1:N `trip_stages`; `packages` 1:N `package_destinations`; `destinations` 1:N `package_destinations`; `reservations` 1:N `reservation_stages`; `trip_stages` 1:N `reservation_stages`.

**Decisiones pendientes de validar:** el modelo supone una persona por reserva; para varios pasajeros por reserva se requiere otra entidad. La aplicación futura debe validar que cada etapa de `reservation_stages` pertenezca al paquete de su reserva. La capacidad, el cálculo de costos y el cambio de estados requieren reglas de negocio acordadas. El campo `role` no constituye por sí solo una autenticación implementada.

## Planificación

**Sprint 1 / Hito 1:** requerimientos verificables, boceto de BD, revisión de diseño, repositorio y estructura por capas, conexión SQLite y evidencia en Jira. El Hito 1 se cierra al final de la Clase 2. Los diagramas UML y BPMN, el sistema funcional y la documentación final se completan en siguientes hitos.

**Git:** trabajar en ramas, abrir pull request hacia `main`, solicitar revisión y enlazar el PR y la prueba desde Jira; no hacer push directo a `main`.


