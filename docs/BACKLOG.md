# Backlog de issues — AccesoQR

> Lista lista para pasar a GitHub Issues, con milestones, etiquetas, estimaciones y trazabilidad a las historias. Se puede crear todo de una vez con [crear-issues.sh](crear-issues.sh).

## 1. Milestones

| Milestone | Fase | Días | Tareas | Horas est. |
|---|---|---|---|---|
| Fase 1 — Estructura base y navegación | 1 | 3 | 8 | 20 |
| Fase 2 — Importación de asistentes | 2 | 4 | 8 | 24 |
| Fase 3 — Escaneo y validación | 3 | 5 | 9 | 31.5 |
| Fase 4 — Sincronización con SystemInscripciones | 4 | 5 | 9 | 31.5 |
| Fase 5 — Exportación y reportes | 5 | 3 | 7 | 17.5 |

Total: **41 tareas**, ~125 horas (20 días a 6 horas efectivas).

## 2. Etiquetas

| Etiqueta | Uso |
|---|---|
| ui | Pantallas y componentes |
| datos | Tablas, consultas y migraciones |
| lógica | Reglas de negocio y cálculos |
| pruebas | Pruebas automáticas y manuales |
| integración | Cámara, notificaciones, PDF, compartir |
| tarea | Otras tareas |
| bug | Errores |
| mejora | Ideas para v1.1+ |

## 3. Issues del MVP

### Fase 1 — Estructura base y navegación

| ID | Título | Historias | Etiquetas | Prioridad | Horas |
|---|---|---|---|---|---|
| `F1-T1` | Crear el proyecto Expo con TypeScript y Expo Router | — | tarea | Alta | 2.5 |
| `F1-T2` | Alinear versiones y herramientas con los repos hermanos | — | tarea | Alta | 2.5 |
| `F1-T3` | Crear la estructura de carpetas | — | tarea | Alta | 2.5 |
| `F1-T4` | Implementar apertura de base y migración 1 | — | datos | Alta | 2.5 |
| `F1-T5` | Definir la navegación principal | — | ui | Alta | 2.5 |
| `F1-T6` | Agregar componente Pantalla, tema e inicio | — | ui | Alta | 2.5 |
| `F1-T7` | Configurar scripts y CI | — | tarea | Alta | 2.5 |
| `F1-T8` | Primera prueba de src/lib | — | pruebas | Alta | 2.5 |

### Fase 2 — Importación de asistentes

| ID | Título | Historias | Etiquetas | Prioridad | Horas |
|---|---|---|---|---|---|
| `F2-T1` | Definir el formato de importación (CSV con columnas nombre y codigo; JSON opcional) y documentarlo en el README. | HU-01 | integración | Alta | 3 |
| `F2-T2` | Selector de archivo con expo-document-picker y lectura con expo-file-system. | HU-01 | tarea | Alta | 3 |
| `F2-T3` | Parser CSV robusto (comillas, separadores , y ;) con pruebas. | HU-01 | pruebas, integración | Alta | 3 |
| `F2-T4` | Inserción por lotes en transacción con resumen de errores y duplicados. | HU-01 | tarea | Alta | 3 |
| `F2-T5` | Pantalla Evento con datos básicos y contador. | HU-01 | ui | Alta | 3 |
| `F2-T6` | Validaciones, estados vacíos y mensajes en español | HU-01 | lógica | Alta | 3 |
| `F2-T7` | Pruebas automáticas y verificación manual de criterios | HU-01 | pruebas | Alta | 3 |
| `F2-T8` | Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG | HU-01 | pruebas | Alta | 3 |

### Fase 3 — Escaneo y validación

| ID | Título | Historias | Etiquetas | Prioridad | Horas |
|---|---|---|---|---|---|
| `F3-T1` | Escáner con expo-camera filtrando solo QR. | HU-02, HU-03, HU-04 | tarea | Alta | 3.5 |
| `F3-T2` | Función pura de validación (válido, duplicado, inválido) con pruebas. | HU-02, HU-03, HU-04 | lógica, pruebas | Alta | 3.5 |
| `F3-T3` | Feedback inmediato: color, sonido y vibración (expo-haptics). | HU-02, HU-03, HU-04 | tarea | Alta | 3.5 |
| `F3-T4` | Registro del ingreso y alta en la cola de sincronización en una sola transacción. | HU-02, HU-03, HU-04 | tarea | Alta | 3.5 |
| `F3-T5` | Búsqueda y validación manual con marca «manual». | HU-02, HU-03, HU-04 | lógica | Alta | 3.5 |
| `F3-T6` | Contador en vivo y protección contra doble lectura del mismo QR en menos de 2 segundos. | HU-02, HU-03, HU-04 | tarea | Alta | 3.5 |
| `F3-T7` | Validaciones, estados vacíos y mensajes en español | HU-02, HU-03, HU-04 | lógica | Alta | 3.5 |
| `F3-T8` | Pruebas automáticas y verificación manual de criterios | HU-02, HU-03, HU-04 | pruebas | Alta | 3.5 |
| `F3-T9` | Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG | HU-02, HU-03, HU-04 | pruebas | Alta | 3.5 |

### Fase 4 — Sincronización con SystemInscripciones

| ID | Título | Historias | Etiquetas | Prioridad | Horas |
|---|---|---|---|---|---|
| `F4-T1` | Acordar y documentar el contrato de API con SystemInscripciones (por ejemplo POST con lista de { codigo, fecha_hora } autenticada con token); este punto es una dependencia externa y debe confirmarse antes de programar. | HU-05 | ui | Media | 3.5 |
| `F4-T2` | Cliente HTTP con reintentos y backoff. | HU-05 | tarea | Media | 3.5 |
| `F4-T3` | Envío por lotes de los pendientes y marcado como enviados solo tras confirmación. | HU-05 | tarea | Media | 3.5 |
| `F4-T4` | Detección de conexión con expo-network y sincronización manual o automática. | HU-05 | tarea | Media | 3.5 |
| `F4-T5` | Pantalla de sincronización con errores legibles. | HU-05 | ui | Media | 3.5 |
| `F4-T6` | Pruebas del cliente con un servidor falso. | HU-05 | pruebas | Media | 3.5 |
| `F4-T7` | Validaciones, estados vacíos y mensajes en español | HU-05 | lógica | Media | 3.5 |
| `F4-T8` | Pruebas automáticas y verificación manual de criterios | HU-05 | pruebas | Media | 3.5 |
| `F4-T9` | Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG | HU-05 | pruebas | Media | 3.5 |

### Fase 5 — Exportación y reportes

| ID | Título | Historias | Etiquetas | Prioridad | Horas |
|---|---|---|---|---|---|
| `F5-T1` | Reporte del evento: esperados, ingresaron, faltan, manuales, duplicados bloqueados. | HU-06 | tarea | Media | 2.5 |
| `F5-T2` | Exportación CSV con expo-file-system y expo-sharing. | HU-06 | integración | Media | 2.5 |
| `F5-T3` | Pruebas del generador CSV. | HU-06 | pruebas, integración | Media | 2.5 |
| `F5-T4` | Pantalla de reporte. | HU-06 | ui | Media | 2.5 |
| `F5-T5` | Validaciones, estados vacíos y mensajes en español | HU-06 | lógica | Media | 2.5 |
| `F5-T6` | Pruebas automáticas y verificación manual de criterios | HU-06 | pruebas | Media | 2.5 |
| `F5-T7` | Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG | HU-06 | pruebas | Media | 2.5 |

## 4. Orden recomendado de ataque

1. Terminá cada fase completa antes de abrir la siguiente; dentro de la fase, de arriba hacia abajo.
2. Las tareas `lógica` y `datos` van antes que las `ui` que las usan.
3. Cada issue se cierra con un commit o PR que lo mencione (`Cierra #N`).
4. Nada de abrir issues de v1.1 hasta tener el APK de la v1.0.0.

## 5. Ideas para después del MVP

| Idea | Esfuerzo | Valor |
|---|---|---|
| Modo oscuro | Bajo | Medio |
| Búsqueda y filtros avanzados | Medio | Alto |
| Estadísticas y gráficos simples en inicio | Medio | Medio |
| Respaldo y restauración de la base | Medio | Alto |
| Publicación en Google Play | Bajo | Alto |
| Sincronización opcional entre dispositivos | Alto | Depende de demanda |
