# Roadmap de AccesoQR

> Que en la puerta del evento se valide cada entrada en un segundo, aunque no haya internet, y que después todo se sincronice solo.

Ruta de desarrollo del MVP, fase por fase. Cada fase termina con algo que se puede **usar y probar en un teléfono**. Trabajá una fase a la vez y no pases a la siguiente hasta cumplir su criterio de salida.

## Resumen

- **Objetivo del MVP:** Check-in de eventos con QR sin conexión: valida entradas en la puerta y sincroniza después con SystemInscripciones.
- **Usuario:** Organizador de eventos o personal de puerta (1 a 3 personas) que controla desde 50 hasta 2000 asistentes con celulares.
- **Duración estimada:** ~20 días de trabajo efectivo (~4 semanas a ritmo tranquilo).
- **Costo:** $0 (sin servidores, sin suscripciones, todo offline en SQLite).

## Línea de tiempo

| Fase | Nombre | Objetivo | Días | Semana acumulada |
|---|---|---|---|---|
| 1 | Estructura base y navegación | Crear el proyecto Expo con base de datos, navegación, tema y CI. | 3 | 1 |
| 2 | Importación de asistentes | Importación de asistentes. | 4 | 2 |
| 3 | Escaneo y validación | Escaneo y validación offline. | 5 | 3 |
| 4 | Sincronización con SystemInscripciones | Sincronización con SystemInscripciones. | 5 | 4 |
| 5 | Exportación y reportes | Exportación y reportes. | 3 | 4 |

## Hitos

### Hito 1 — Estructura base y navegación

**Demo:** La app abre en Expo Go, navega entre secciones vacías y el CI pasa en verde.

### Hito 2 — Importación de asistentes

**Demo:** Una lista de 2000 asistentes se importa en menos de 5 segundos.

### Hito 3 — Escaneo y validación

**Demo:** Validación en menos de 2 segundos sin conexión. Un segundo escaneo del mismo código nunca crea otro ingreso.

### Hito 4 — Sincronización con SystemInscripciones

**Demo:** Los ingresos se sincronizan sin pérdidas ni duplicados tras cortes de conexión simulados.

### Hito 5 — Exportación y reportes

**Demo:** El CSV coincide con los contadores de la app.

## Detalle por fase

### Fase 1 — Estructura base y navegación

- **Objetivo:** dejar el proyecto creado y funcionando con la base de datos y la navegación.
- **Historias cubiertas:** ninguna (infraestructura).
- **Depende de:** nada.
- **Criterio de salida:** la app abre, navega y el CI está en verde.

### Fase 2 — Importación de asistentes

- **Objetivo:** Importación de asistentes.
- **Historias cubiertas:** HU-01 Importar la lista de asistentes
- **Depende de:** fase 1.
- **Duración:** 4 días.
- **Criterio de salida:**
  - Una lista de 2000 asistentes se importa en menos de 5 segundos.

### Fase 3 — Escaneo y validación

- **Objetivo:** Escaneo y validación offline.
- **Historias cubiertas:** HU-02 Escanear y validar entradas; HU-03 Ver el contador en vivo; HU-04 Buscar y validar a mano
- **Depende de:** fase 2.
- **Duración:** 5 días.
- **Criterio de salida:**
  - Validación en menos de 2 segundos sin conexión.
  - Un segundo escaneo del mismo código nunca crea otro ingreso.

### Fase 4 — Sincronización con SystemInscripciones

- **Objetivo:** Sincronización con SystemInscripciones.
- **Historias cubiertas:** HU-05 Sincronizar con SystemInscripciones
- **Depende de:** fase 3.
- **Duración:** 5 días.
- **Criterio de salida:**
  - Los ingresos se sincronizan sin pérdidas ni duplicados tras cortes de conexión simulados.

### Fase 5 — Exportación y reportes

- **Objetivo:** Exportación y reportes.
- **Historias cubiertas:** HU-06 Exportar asistencia
- **Depende de:** fase 4.
- **Duración:** 3 días.
- **Criterio de salida:**
  - El CSV coincide con los contadores de la app.

## Estado

- [ ] **Fase 1:** Estructura base y navegación
- [ ] **Fase 2:** Importación de asistentes
- [ ] **Fase 3:** Escaneo y validación
- [ ] **Fase 4:** Sincronización con SystemInscripciones
- [ ] **Fase 5:** Exportación y reportes

## Dependencias y supuestos

- **API de SystemInscripciones:** el contrato de sincronización debe acordarse antes de la fase 4 (ver [SPEC.md](SPEC.md)). Mientras tanto se desarrolla contra un servidor falso.
- Un teléfono Android real para probar (Expo Go) y, al final, para instalar el APK.
- Node.js 20+ y la app Expo Go.
- Todo lo demás (cuentas, nube, pagos) queda fuera del MVP.

## Después del MVP (v1.1 en adelante)

- [ ] Modo oscuro.
- [ ] Búsqueda y filtros avanzados en las listas.
- [ ] Estadísticas y gráficos simples en la pantalla de inicio.
- [ ] Publicación en Google Play (cuenta de desarrollador, pago único de USD 25).
- [ ] Respaldo y restauración de la base de datos.
- [ ] Sincronización opcional entre dispositivos (solo si hay demanda real).

## Cómo usar este documento

1. [SPEC.md](SPEC.md) define **qué** debe hacer la app (historias, reglas, pantallas, datos).
2. [PLANNING.md](PLANNING.md) define **cómo** construirla fase por fase (tareas, pruebas, issues).
3. Al cerrar una fase: marcá el estado acá, actualizá el [CHANGELOG](../CHANGELOG.md) y fusioná la rama a `main`.

## Trazabilidad: historias por fase

| Historia | Fase | Entrega |
|---|---|---|
| HU-01 Importar la lista de asistentes | 2 | Importación de asistentes |
| HU-02 Escanear y validar entradas | 3 | Escaneo y validación |
| HU-03 Ver el contador en vivo | 3 | Escaneo y validación |
| HU-04 Buscar y validar a mano | 3 | Escaneo y validación |
| HU-05 Sincronizar con SystemInscripciones | 4 | Sincronización con SystemInscripciones |
| HU-06 Exportar asistencia | 5 | Exportación y reportes |

## Indicadores de avance

| Indicador | Cómo se mide | Meta |
|---|---|---|
| Avance de tareas | Issues cerrados / 41 | 100 % al lanzar |
| Historias aceptadas | Casos CP- marcados ✔ / 10 | 100 % |
| Reglas cubiertas | RN con prueba / 7 | 100 % |
| Ritmo | Días reales vs estimados por fase | Desvío menor al 50 % |
| Calidad | Errores abiertos al cierre de fase | 0 críticos |

## Revisión de fin de fase

Antes de pasar a la siguiente fase respondé por escrito (5 minutos):

1. ¿Se cumplieron todos los criterios de salida?
2. ¿Cuántos días tomó realmente frente a lo estimado?
3. ¿Qué se recortó o se dejó para después?
4. ¿Hay algo que cambie el SPEC? Actualizalo antes de seguir.

## Documentos de apoyo

- [DATOS.md](DATOS.md): campos, validaciones, consultas y migraciones.
- [PANTALLAS.md](PANTALLAS.md): ruta, estados y bosquejo de cada pantalla.
- [PRUEBAS.md](PRUEBAS.md): casos de prueba por regla, historia y borde.
- [BACKLOG.md](BACKLOG.md): issues listos para GitHub con milestones y etiquetas.
