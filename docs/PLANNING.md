# Plan de desarrollo — AccesoQR

Guía para construir el MVP **fase por fase**. Complementa a [ROADMAP.md](ROADMAP.md) (qué y cuándo) y [SPEC.md](SPEC.md) (qué debe hacer la app).

## Forma de trabajo

1. **Una fase por rama:** `git checkout -b fase-N-nombre`.
2. **Un issue por tarea** (usá los títulos de abajo) y un milestone por fase en GitHub.
3. **Commits chicos** con prefijo: `feat:`, `fix:`, `test:`, `docs:`, `chore:`.
4. **Pull request al cerrar la fase** con la lista de criterios de salida marcada.
5. **Primero la lógica pura y sus pruebas** (en `src/lib/`), después la pantalla que la usa.
6. **Probá en el teléfono** al terminar cada tarea de pantalla, no solo al final de la fase.

### Definición de «terminado» (vale para toda tarea y fase)

- [ ] `npm run lint`, `npx tsc --noEmit` y `npm test` sin errores.
- [ ] Funciona en modo avión.
- [ ] Estados vacíos, de carga y de error resueltos, con textos en español.
- [ ] Los datos persisten al reiniciar la app.
- [ ] La regla de negocio correspondiente tiene prueba o verificación manual anotada.
- [ ] CHANGELOG actualizado al cerrar la fase.

## Calendario sugerido

| Fase | Nombre | Días | Acumulado |
|---|---|---|---|
| 1 | Estructura base y navegación | 3 | 3 |
| 2 | Importación de asistentes | 4 | 7 |
| 3 | Escaneo y validación | 5 | 12 |
| 4 | Sincronización con SystemInscripciones | 5 | 17 |
| 5 | Exportación y reportes | 3 | 20 |

Total: **20 días** (~4 semanas). Si una fase se pasa del doble de lo estimado, recortá alcance dentro de la fase en lugar de saltar a la siguiente.

---

## Fase 1 — Estructura base y navegación

**Estimación:** 3 días.

**Objetivo:** crear el proyecto Expo con base de datos, navegación, tema y CI. Este repositorio hoy solo tiene documentación; la fase 1 agrega el código base.

**Tareas**

- [ ] **F1-T1** Crear el proyecto en una carpeta temporal con `npx create-expo-app@latest` (plantilla con TypeScript y Expo Router) y copiar su contenido a este repositorio sin pisar README, LICENSE ni los archivos de comunidad.
- [ ] **F1-T2** Alinear versiones y herramientas con los repositorios hermanos (por ejemplo PasanakuYa): Expo SDK 58, `expo-sqlite`, `jest-expo`, ESLint, TypeScript estricto, `.editorconfig` y `.prettierrc`.
- [ ] **F1-T3** Crear la estructura `app/`, `src/db/`, `src/components/`, `src/lib/` y `src/theme.ts`.
- [ ] **F1-T4** Implementar `src/db/index.ts` (apertura de la base) y `src/db/schema.ts` con la migración 1 tomada de la sección 6 del [SPEC](SPEC.md).
- [ ] **F1-T5** Definir la navegación principal en `app/_layout.tsx` con una ruta por sección.
- [ ] **F1-T6** Agregar el componente `Pantalla`, el tema y la pantalla de inicio con accesos a cada sección.
- [ ] **F1-T7** Configurar scripts `lint`, `test`, `build:apk` y el workflow de GitHub Actions (`.github/workflows/ci.yml`).
- [ ] **F1-T8** Escribir una primera prueba de `src/lib` para validar que Jest funciona.

**Criterio de salida:** la app abre, navega entre secciones vacías y no da errores; lint, tipos y tests en verde.

---

## Fase 2 — Importación de asistentes

**Estimación:** 4 días. **Rama sugerida:** `fase-2-importacion-de-asistentes`.

**Objetivo:** Importación de asistentes.

**Historias que se completan:** HU-01 (Importar la lista de asistentes).

**Tareas**

- [ ] **F2-T1** Definir el formato de importación (CSV con columnas nombre y codigo; JSON opcional) y documentarlo en el README.
- [ ] **F2-T2** Selector de archivo con expo-document-picker y lectura con expo-file-system.
- [ ] **F2-T3** Parser CSV robusto (comillas, separadores , y ;) con pruebas.
- [ ] **F2-T4** Inserción por lotes en transacción con resumen de errores y duplicados.
- [ ] **F2-T5** Pantalla Evento con datos básicos y contador.
- [ ] **F2-T6** Validaciones, estados vacíos y mensajes de error en español para todo lo nuevo.
- [ ] **F2-T7** Pruebas automáticas de la lógica de la fase y verificación manual de los criterios de abajo.
- [ ] **F2-T8** Probar en un teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

**Pruebas de aceptación manuales** (de [SPEC.md](SPEC.md)):

- [ ] HU-01 — un archivo con 500 asistentes (nombre y código) → lo importo → se crea el evento con 500 asistentes y un resumen de cuántos se importaron y cuántos tuvieron error.
- [ ] HU-01 — una lista con códigos repetidos → la importo → la app informa los duplicados y conserva uno por código.

**Criterio de salida:**

- [ ] Una lista de 2000 asistentes se importa en menos de 5 segundos.
- [ ] Definición de «terminado» cumplida.

---

## Fase 3 — Escaneo y validación

**Estimación:** 5 días. **Rama sugerida:** `fase-3-escaneo-y-validacion`.

**Objetivo:** Escaneo y validación offline.

**Historias que se completan:** HU-02 (Escanear y validar entradas), HU-03 (Ver el contador en vivo), HU-04 (Buscar y validar a mano).

**Tareas**

- [ ] **F3-T1** Escáner con expo-camera filtrando solo QR.
- [ ] **F3-T2** Función pura de validación (válido, duplicado, inválido) con pruebas.
- [ ] **F3-T3** Feedback inmediato: color, sonido y vibración (expo-haptics).
- [ ] **F3-T4** Registro del ingreso y alta en la cola de sincronización en una sola transacción.
- [ ] **F3-T5** Búsqueda y validación manual con marca «manual».
- [ ] **F3-T6** Contador en vivo y protección contra doble lectura del mismo QR en menos de 2 segundos.
- [ ] **F3-T7** Validaciones, estados vacíos y mensajes de error en español para todo lo nuevo.
- [ ] **F3-T8** Pruebas automáticas de la lógica de la fase y verificación manual de los criterios de abajo.
- [ ] **F3-T9** Probar en un teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

**Pruebas de aceptación manuales** (de [SPEC.md](SPEC.md)):

- [ ] HU-02 — un asistente que no ingresó → escaneo su QR → pantalla verde con su nombre y se registra el ingreso.
- [ ] HU-02 — un QR que ya ingresó → lo escaneo otra vez → pantalla roja con «Ya ingresó a las 19:42» y no se registra nada.
- [ ] HU-02 — un QR que no está en la lista → lo escaneo → pantalla roja «Entrada inválida».
- [ ] HU-03 — 100 ingresos de 400 → abro el evento → veo «100 / 400» y el porcentaje, actualizado en cada escaneo.
- [ ] HU-04 — un asistente sin QR legible → lo busco por nombre y confirmo → se registra el ingreso igual, marcado como manual.

**Criterio de salida:**

- [ ] Validación en menos de 2 segundos sin conexión.
- [ ] Un segundo escaneo del mismo código nunca crea otro ingreso.
- [ ] Definición de «terminado» cumplida.

---

## Fase 4 — Sincronización con SystemInscripciones

**Estimación:** 5 días. **Rama sugerida:** `fase-4-sincronizacion-con-systeminscripciones`.

**Objetivo:** Sincronización con SystemInscripciones.

**Historias que se completan:** HU-05 (Sincronizar con SystemInscripciones).

**Tareas**

- [ ] **F4-T1** Acordar y documentar el contrato de API con SystemInscripciones (por ejemplo POST con lista de { codigo, fecha_hora } autenticada con token); este punto es una dependencia externa y debe confirmarse antes de programar.
- [ ] **F4-T2** Cliente HTTP con reintentos y backoff.
- [ ] **F4-T3** Envío por lotes de los pendientes y marcado como enviados solo tras confirmación.
- [ ] **F4-T4** Detección de conexión con expo-network y sincronización manual o automática.
- [ ] **F4-T5** Pantalla de sincronización con errores legibles.
- [ ] **F4-T6** Pruebas del cliente con un servidor falso.
- [ ] **F4-T7** Validaciones, estados vacíos y mensajes de error en español para todo lo nuevo.
- [ ] **F4-T8** Pruebas automáticas de la lógica de la fase y verificación manual de los criterios de abajo.
- [ ] **F4-T9** Probar en un teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

**Pruebas de aceptación manuales** (de [SPEC.md](SPEC.md)):

- [ ] HU-05 — 20 ingresos pendientes y conexión disponible → toco «Sincronizar» → se envían y quedan marcados como enviados.
- [ ] HU-05 — falla la conexión a mitad de envío → reintento → solo se reenvían los pendientes, sin duplicar.

**Criterio de salida:**

- [ ] Los ingresos se sincronizan sin pérdidas ni duplicados tras cortes de conexión simulados.
- [ ] Definición de «terminado» cumplida.

---

## Fase 5 — Exportación y reportes

**Estimación:** 3 días. **Rama sugerida:** `fase-5-exportacion-y-reportes`.

**Objetivo:** Exportación y reportes.

**Historias que se completan:** HU-06 (Exportar asistencia).

**Tareas**

- [ ] **F5-T1** Reporte del evento: esperados, ingresaron, faltan, manuales, duplicados bloqueados.
- [ ] **F5-T2** Exportación CSV con expo-file-system y expo-sharing.
- [ ] **F5-T3** Pruebas del generador CSV.
- [ ] **F5-T4** Pantalla de reporte.
- [ ] **F5-T5** Validaciones, estados vacíos y mensajes de error en español para todo lo nuevo.
- [ ] **F5-T6** Pruebas automáticas de la lógica de la fase y verificación manual de los criterios de abajo.
- [ ] **F5-T7** Probar en un teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

**Pruebas de aceptación manuales** (de [SPEC.md](SPEC.md)):

- [ ] HU-06 — un evento terminado → toco «Exportar CSV» → el archivo incluye nombre, código, ingresó (sí/no), hora y origen (QR o manual).

**Criterio de salida:**

- [ ] El CSV coincide con los contadores de la app.
- [ ] Definición de «terminado» cumplida.

---

## Lanzamiento del MVP (v1.0.0)

- [ ] Revisar todas las historias de [SPEC.md](SPEC.md) y marcar sus criterios.
- [ ] Capturas de pantalla para el README.
- [ ] Revisar textos, íconos y nombre de la app en `app.json`.
- [ ] `npm run build:apk` y probar el APK en un teléfono limpio, sin Expo Go y en modo avión.
- [ ] Publicar el release v1.0.0 con el APK adjunto.
- [ ] Pasar las ideas pendientes a issues para la versión 1.1.

## Plantilla de issue

Copiá esto al crear cada issue:

```
Título: [F2-T1] <descripción de la tarea>
Fase: N — <nombre>
Historia: HU-XX
Criterios:
- [ ] ...
Notas técnicas: ...
```

---

## Cronograma día por día

Guía para no perder el hilo: cada día tiene tareas concretas. Si un día se atrasa, mové la tarea al siguiente, no la saltees.

### Fase 1 — Estructura base y navegación

- **Día 1** (día 1 de la fase): `F1-T1` Crear el proyecto Expo con TypeScript y Expo Router; `F1-T2` Alinear versiones y herramientas con los repos hermanos; `F1-T3` Crear la estructura de carpetas.
- **Día 2** (día 2 de la fase): `F1-T4` Implementar apertura de base y migración 1; `F1-T5` Definir la navegación principal; `F1-T6` Agregar componente Pantalla, tema e inicio.
- **Día 3** (día 3 de la fase): `F1-T7` Configurar scripts y CI; `F1-T8` Primera prueba de src/lib.

### Fase 2 — Importación de asistentes

- **Día 4** (día 1 de la fase): `F2-T1` Definir el formato de importación (CSV con columnas nombre y codigo; JSON opcional) y documentarlo en el README.; `F2-T2` Selector de archivo con expo-document-picker y lectura con expo-file-system..
- **Día 5** (día 2 de la fase): `F2-T3` Parser CSV robusto (comillas, separadores , y ;) con pruebas.; `F2-T4` Inserción por lotes en transacción con resumen de errores y duplicados..
- **Día 6** (día 3 de la fase): `F2-T5` Pantalla Evento con datos básicos y contador.; `F2-T6` Validaciones, estados vacíos y mensajes en español.
- **Día 7** (día 4 de la fase): `F2-T7` Pruebas automáticas y verificación manual de criterios; `F2-T8` Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

### Fase 3 — Escaneo y validación

- **Día 8** (día 1 de la fase): `F3-T1` Escáner con expo-camera filtrando solo QR.; `F3-T2` Función pura de validación (válido, duplicado, inválido) con pruebas..
- **Día 9** (día 2 de la fase): `F3-T3` Feedback inmediato: color, sonido y vibración (expo-haptics).; `F3-T4` Registro del ingreso y alta en la cola de sincronización en una sola transacción..
- **Día 10** (día 3 de la fase): `F3-T5` Búsqueda y validación manual con marca «manual».; `F3-T6` Contador en vivo y protección contra doble lectura del mismo QR en menos de 2 segundos..
- **Día 11** (día 4 de la fase): `F3-T7` Validaciones, estados vacíos y mensajes en español; `F3-T8` Pruebas automáticas y verificación manual de criterios.
- **Día 12** (día 5 de la fase): `F3-T9` Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

### Fase 4 — Sincronización con SystemInscripciones

- **Día 13** (día 1 de la fase): `F4-T1` Acordar y documentar el contrato de API con SystemInscripciones (por ejemplo POST con lista de { codigo, fecha_hora } autenticada con token); este punto es una dependencia externa y debe confirmarse antes de programar.; `F4-T2` Cliente HTTP con reintentos y backoff..
- **Día 14** (día 2 de la fase): `F4-T3` Envío por lotes de los pendientes y marcado como enviados solo tras confirmación.; `F4-T4` Detección de conexión con expo-network y sincronización manual o automática..
- **Día 15** (día 3 de la fase): `F4-T5` Pantalla de sincronización con errores legibles.; `F4-T6` Pruebas del cliente con un servidor falso..
- **Día 16** (día 4 de la fase): `F4-T7` Validaciones, estados vacíos y mensajes en español; `F4-T8` Pruebas automáticas y verificación manual de criterios.
- **Día 17** (día 5 de la fase): `F4-T9` Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

### Fase 5 — Exportación y reportes

- **Día 18** (día 1 de la fase): `F5-T1` Reporte del evento: esperados, ingresaron, faltan, manuales, duplicados bloqueados.; `F5-T2` Exportación CSV con expo-file-system y expo-sharing.; `F5-T3` Pruebas del generador CSV..
- **Día 19** (día 2 de la fase): `F5-T4` Pantalla de reporte.; `F5-T5` Validaciones, estados vacíos y mensajes en español; `F5-T6` Pruebas automáticas y verificación manual de criterios.
- **Día 20** (día 3 de la fase): `F5-T7` Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

## Rutina diaria

1. Empezá releyendo la tarea del día en [BACKLOG.md](BACKLOG.md) y su regla o historia en [SPEC.md](SPEC.md).
2. Escribí primero la prueba de la lógica, después el código.
3. Probá en el teléfono antes de hacer commit.
4. Cerrá el día con un commit, el issue marcado y una línea en el CHANGELOG si corresponde.

## Qué hacer si te trabás

- Más de 2 horas sin avanzar: simplificá la tarea al caso mínimo y anotá lo que quedó fuera como issue `mejora`.
- Duda de alcance: la respuesta está en la sección 2 del [SPEC](SPEC.md); si no está, no entra al MVP.
- Duda de datos o validaciones: [DATOS.md](DATOS.md).
- Duda de pantallas: [PANTALLAS.md](PANTALLAS.md).
- Duda de cómo probar: [PRUEBAS.md](PRUEBAS.md).
