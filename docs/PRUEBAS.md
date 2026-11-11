# Plan de pruebas — AccesoQR

> Qué probar, cuándo y cómo. Cada historia y cada regla de negocio tiene casos concretos. Complementa a [SPEC.md](SPEC.md).

## 1. Estrategia

| Nivel | Herramienta | Qué cubre | Cuándo |
|---|---|---|---|
| Unitarias | jest-expo (`npm test`) | Lógica pura de `src/lib/` (cálculos, validaciones, formatos) | En cada commit (CI) |
| Manuales por historia | Teléfono con Expo Go | Criterios de aceptación de SPEC (casos CP-) | Al cerrar cada tarea de pantalla |
| Regresión por fase | Teléfono real | Lo ya terminado sigue andando | Al cerrar cada fase |
| Aceptación final | APK en teléfono limpio, modo avión | Criterios de lanzamiento | Antes de v1.0.0 |

Meta de cobertura: **100 % de las reglas de negocio** con prueba automática o caso manual, y **100 % de los criterios de aceptación** ejecutados al menos una vez en un teléfono real.

## 2. Pruebas unitarias por regla de negocio

Crear un archivo `src/lib/<tema>.test.ts` por tema. Una prueba por regla como mínimo; agregar los bordes de la sección 4.

| ID | Regla | Tipo de prueba | Casos mínimos |
|---|---|---|---|
| RN-01 | Cada código QR es único por asistente dentro de un evento. | Unitaria o manual | Caso válido, caso inválido, límite |
| RN-02 | Un asistente solo puede ingresar una vez; el segundo escaneo se muestra como duplicado con la hora del primer ingreso. | Unitaria o manual | Caso válido, caso inválido, límite |
| RN-03 | Un código que no está en la lista del evento es inválido y no registra nada. | Unitaria o manual | Caso válido, caso inválido, límite |
| RN-04 | Cada ingreso válido se guarda con fecha y hora y se agrega a la cola de sincronización. | Unitaria (fechas) | Hoy, mañana, fin de mes, año bisiesto |
| RN-05 | La sincronización es idempotente: reenviar un ingreso ya enviado no lo duplica en el servidor. | Unitaria o manual | Caso válido, caso inválido, límite |
| RN-06 | Los ingresos nunca se borran por una falla de sincronización; se reintentan. | Unitaria o manual | Caso válido, caso inválido, límite |
| RN-07 | La lista de asistentes se importa antes del evento y se puede refrescar sin perder los ingresos ya registrados. | Unitaria o manual | Caso válido, caso inválido, límite |

## 3. Casos manuales por historia

Formato: ejecutar en el teléfono, marcar ✔ o ✘ y anotar la fecha. Si falla, abrir un issue con el ID del caso.

### HU-01 — Importar la lista de asistentes (fase 2)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-01-1 | un archivo con 500 asistentes (nombre y código) | lo importo | se crea el evento con 500 asistentes y un resumen de cuántos se importaron y cuántos tuvieron error | ☐ |
| CP-01-2 | una lista con códigos repetidos | la importo | la app informa los duplicados y conserva uno por código | ☐ |

Casos transversales: **CP-01-T1** modo avión · **CP-01-T2** cerrar y reabrir la app (persistencia) · **CP-01-T3** texto del sistema en tamaño grande.

### HU-02 — Escanear y validar entradas (fase 3)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-02-1 | un asistente que no ingresó | escaneo su QR | pantalla verde con su nombre y se registra el ingreso | ☐ |
| CP-02-2 | un QR que ya ingresó | lo escaneo otra vez | pantalla roja con «Ya ingresó a las 19:42» y no se registra nada | ☐ |
| CP-02-3 | un QR que no está en la lista | lo escaneo | pantalla roja «Entrada inválida» | ☐ |

Casos transversales: **CP-02-T1** modo avión · **CP-02-T2** cerrar y reabrir la app (persistencia) · **CP-02-T3** texto del sistema en tamaño grande.

### HU-03 — Ver el contador en vivo (fase 3)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-03-1 | 100 ingresos de 400 | abro el evento | veo «100 / 400» y el porcentaje, actualizado en cada escaneo | ☐ |

Casos transversales: **CP-03-T1** modo avión · **CP-03-T2** cerrar y reabrir la app (persistencia) · **CP-03-T3** texto del sistema en tamaño grande.

### HU-04 — Buscar y validar a mano (fase 3)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-04-1 | un asistente sin QR legible | lo busco por nombre y confirmo | se registra el ingreso igual, marcado como manual | ☐ |

Casos transversales: **CP-04-T1** modo avión · **CP-04-T2** cerrar y reabrir la app (persistencia) · **CP-04-T3** texto del sistema en tamaño grande.

### HU-05 — Sincronizar con SystemInscripciones (fase 4)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-05-1 | 20 ingresos pendientes y conexión disponible | toco «Sincronizar» | se envían y quedan marcados como enviados | ☐ |
| CP-05-2 | falla la conexión a mitad de envío | reintento | solo se reenvían los pendientes, sin duplicar | ☐ |

Casos transversales: **CP-05-T1** modo avión · **CP-05-T2** cerrar y reabrir la app (persistencia) · **CP-05-T3** texto del sistema en tamaño grande.

### HU-06 — Exportar asistencia (fase 5)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-06-1 | un evento terminado | toco «Exportar CSV» | el archivo incluye nombre, código, ingresó (sí/no), hora y origen (QR o manual) | ☐ |

Casos transversales: **CP-06-T1** modo avión · **CP-06-T2** cerrar y reabrir la app (persistencia) · **CP-06-T3** texto del sistema en tamaño grande.

## 4. Casos borde

| ID | Caso | Resultado esperado | Resultado |
|---|---|---|---|
| CB-01 | Celular sin cámara funcional: búsqueda manual como alternativa. | La app no se rompe; el mensaje o comportamiento es claro y no se pierden datos | ☐ |
| CB-02 | Dos celulares en la puerta del mismo evento: el servidor es la fuente de verdad; la app avisa si un código ya ingresó desde otro dispositivo al sincronizar. | La app no se rompe; el mensaje o comportamiento es claro y no se pierden datos | ☐ |
| CB-03 | Reloj del teléfono desajustado: mostrar advertencia y guardar la hora del dispositivo. | La app no se rompe; el mensaje o comportamiento es claro y no se pierden datos | ☐ |
| CB-04 | Lista actualizada durante el evento: refrescar sin perder ingresos ya registrados. | La app no se rompe; el mensaje o comportamiento es claro y no se pierden datos | ☐ |
| CB-05 | QR con formato distinto al esperado: se trata como inválido. | La app no se rompe; el mensaje o comportamiento es claro y no se pierden datos | ☐ |

## 5. Pruebas de datos y migraciones

| ID | Prueba | Esperado |
|---|---|---|
| CD-01 | Primer arranque en teléfono limpio | La base se crea con todas las tablas y `user_version` queda en 1 |
| CD-02 | Actualizar de una versión anterior con datos cargados | Las migraciones nuevas se aplican y no se pierde ningún registro |
| CD-03 | Borrar un registro padre | Los hijos se borran o se bloquea con mensaje, según el esquema |
| CD-04 | Intentar guardar con campos obligatorios vacíos | No se inserta nada y se marcan los campos |
| CD-05 | Cargar 1.000 registros de prueba | Las listas siguen fluidas (scroll sin trabas) y abren en menos de 2 s |

## 6. Regresión al cerrar cada fase

**Fase 1 — Estructura base y navegación**

- [ ] La app abre y navega sin errores

**Fase 2 — Importación de asistentes**

- [ ] Una lista de 2000 asistentes se importa en menos de 5 segundos.
- [ ] Los criterios de las fases anteriores siguen funcionando

**Fase 3 — Escaneo y validación**

- [ ] Validación en menos de 2 segundos sin conexión.
- [ ] Un segundo escaneo del mismo código nunca crea otro ingreso.
- [ ] Los criterios de las fases anteriores siguen funcionando

**Fase 4 — Sincronización con SystemInscripciones**

- [ ] Los ingresos se sincronizan sin pérdidas ni duplicados tras cortes de conexión simulados.
- [ ] Los criterios de las fases anteriores siguen funcionando

**Fase 5 — Exportación y reportes**

- [ ] El CSV coincide con los contadores de la app.
- [ ] Los criterios de las fases anteriores siguen funcionando

## 7. Matriz de dispositivos

| Dispositivo | Android | Qué se verifica |
|---|---|---|
| Gama baja (2–3 GB de RAM) | 8–10 | Fluidez de listas, tiempo de arranque |
| Gama media | 11–13 | Flujo completo y APK |
| Pantalla pequeña (5") | cualquiera | Que nada quede cortado ni oculto por el teclado |
| Texto del sistema grande | cualquiera | Que los formularios sigan siendo usables |

## 8. Lista de verificación para el lanzamiento (v1.0.0)

- [ ] Todas las historias ejecutadas en teléfono real (casos CP-)
- [ ] Todos los casos borde (CB-) ejecutados
- [ ] Pruebas de datos (CD-) ejecutadas
- [ ] APK instalado en un teléfono limpio sin Expo Go
- [ ] Probado en modo avión durante todo el flujo principal
- [ ] Rotación de pantalla no pierde datos del formulario
- [ ] Permisos solicitados solo cuando se necesitan y con explicación
- [ ] Sin errores en la consola durante un recorrido completo
- [ ] README con capturas y CHANGELOG al día

## 9. Informe de errores

```
Título: [Bug] <qué pasó>
ID de caso: CP-XX-X
Pasos: 1... 2... 3...
Esperado: ...
Obtenido: ...
Dispositivo / Android: ...
Captura o video: ...
```
