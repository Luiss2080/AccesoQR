# Especificación del MVP — AccesoQR

> Documento vivo. Si una decisión cambia, se actualiza acá primero y después en el código.

## 1. Resumen

**Propuesta de valor:** Que en la puerta del evento se valide cada entrada en un segundo, aunque no haya internet, y que después todo se sincronice solo.

**Problema:** En eventos con muchos asistentes la señal falla justo en la puerta; se validan listas en papel, se cuelan entradas duplicadas y después hay que pasar todo a mano.

**Cómo se resuelve hoy:** Listas impresas, tachones y planillas pasadas a mano.

**Usuario objetivo:** Organizador de eventos o personal de puerta (1 a 3 personas) que controla desde 50 hasta 2000 asistentes con celulares.

**Métricas de éxito del MVP:**

- Validar una entrada en menos de 2 segundos sin conexión.
- Detectar el 100% de las entradas duplicadas.
- Sincronizar los ingresos sin pérdidas al recuperar conexión.

## 2. Alcance del MVP

### Incluido

- Escaneo QR offline de entradas en la puerta.
- Lista local de asistentes descargada antes del evento.
- Detección de duplicados y entradas inválidas.
- Contador en vivo de ingresos.
- Sincronización con SystemInscripciones al recuperar conexión.
- Exportación CSV de asistencia.

### Fuera del MVP

- Cuentas de usuario, inicio de sesión y multiusuario.
- Servidor propio, nube o sincronización entre dispositivos (excepto el envío de ingresos a SystemInscripciones).
- Pagos en línea o pasarelas de cobro.
- Publicación en App Store (iOS).
- Cualquier servicio de pago o con costo recurrente.

## 3. Reglas de negocio

Reglas que el código **debe** cumplir. Cada una se cubre con pruebas automáticas cuando es lógica pura.

- **RN-01** Cada código QR es único por asistente dentro de un evento.
- **RN-02** Un asistente solo puede ingresar una vez; el segundo escaneo se muestra como duplicado con la hora del primer ingreso.
- **RN-03** Un código que no está en la lista del evento es inválido y no registra nada.
- **RN-04** Cada ingreso válido se guarda con fecha y hora y se agrega a la cola de sincronización.
- **RN-05** La sincronización es idempotente: reenviar un ingreso ya enviado no lo duplica en el servidor.
- **RN-06** Los ingresos nunca se borran por una falla de sincronización; se reintentan.
- **RN-07** La lista de asistentes se importa antes del evento y se puede refrescar sin perder los ingresos ya registrados.

## 4. Historias de usuario y criterios de aceptación

Formato: **Dado** un contexto, **Cuando** el usuario hace algo, **Entonces** pasa algo verificable.

### HU-01 — Importar la lista de asistentes

**Como** organizador **quiero** cargar la lista de asistentes del evento antes de llegar **para** validar sin internet.

**Fase:** 2 — Importación de asistentes

**Criterios de aceptación:**

1. **Dado:** un archivo con 500 asistentes (nombre y código). **Cuando:** lo importo. **Entonces:** se crea el evento con 500 asistentes y un resumen de cuántos se importaron y cuántos tuvieron error.
2. **Dado:** una lista con códigos repetidos. **Cuando:** la importo. **Entonces:** la app informa los duplicados y conserva uno por código.

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

### HU-02 — Escanear y validar entradas

**Como** personal de puerta **quiero** escanear el QR y ver al instante si es válido **para** dejar pasar rápido.

**Fase:** 3 — Escaneo y validación

**Criterios de aceptación:**

1. **Dado:** un asistente que no ingresó. **Cuando:** escaneo su QR. **Entonces:** pantalla verde con su nombre y se registra el ingreso.
2. **Dado:** un QR que ya ingresó. **Cuando:** lo escaneo otra vez. **Entonces:** pantalla roja con «Ya ingresó a las 19:42» y no se registra nada.
3. **Dado:** un QR que no está en la lista. **Cuando:** lo escaneo. **Entonces:** pantalla roja «Entrada inválida».

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

### HU-03 — Ver el contador en vivo

**Como** organizador **quiero** ver cuántos ingresaron de cuántos esperados **para** controlar el aforo.

**Fase:** 3 — Escaneo y validación

**Criterios de aceptación:**

1. **Dado:** 100 ingresos de 400. **Cuando:** abro el evento. **Entonces:** veo «100 / 400» y el porcentaje, actualizado en cada escaneo.

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

### HU-04 — Buscar y validar a mano

**Como** personal de puerta **quiero** buscar por nombre cuando el QR no funciona **para** no dejar a nadie afuera.

**Fase:** 3 — Escaneo y validación

**Criterios de aceptación:**

1. **Dado:** un asistente sin QR legible. **Cuando:** lo busco por nombre y confirmo. **Entonces:** se registra el ingreso igual, marcado como manual.

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

### HU-05 — Sincronizar con SystemInscripciones

**Como** organizador **quiero** enviar los ingresos al sistema al tener conexión **para** tener la asistencia oficial.

**Fase:** 4 — Sincronización con SystemInscripciones

**Criterios de aceptación:**

1. **Dado:** 20 ingresos pendientes y conexión disponible. **Cuando:** toco «Sincronizar». **Entonces:** se envían y quedan marcados como enviados.
2. **Dado:** falla la conexión a mitad de envío. **Cuando:** reintento. **Entonces:** solo se reenvían los pendientes, sin duplicar.

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

### HU-06 — Exportar asistencia

**Como** organizador **quiero** exportar la asistencia en CSV **para** entregar el reporte.

**Fase:** 5 — Exportación y reportes

**Criterios de aceptación:**

1. **Dado:** un evento terminado. **Cuando:** toco «Exportar CSV». **Entonces:** el archivo incluye nombre, código, ingresó (sí/no), hora y origen (QR o manual).

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

## 5. Pantallas y navegación

| Pantalla | Qué muestra | Acciones principales |
|---|---|---|
| Inicio | Lista de eventos con ingresos y pendientes de sincronizar | Importar evento |
| Importar | Selector de archivo y resumen de importación | Elegir archivo, confirmar |
| Evento | Contador de ingresos, accesos a escáner, lista y sincronización | Escanear, buscar, sincronizar, exportar |
| Escáner | Cámara con resultado grande en verde, rojo o amarillo, con sonido y vibración | Escanear, linterna, búsqueda manual |
| Lista de asistentes | Lista con filtro (todos, ingresaron, faltan) y búsqueda | Validar a mano |
| Sincronización | Pendientes, último envío y errores | Sincronizar, reintentar |
| Ajustes | URL y token de SystemInscripciones | Guardar, probar conexión |

**Navegación:** la pantalla de inicio es el punto de partida; desde ahí se llega a cada sección con un toque y se vuelve con el botón atrás. Las acciones frecuentes (nuevo registro) están siempre a la vista. La navegación se implementa con Expo Router (rutas por archivo en `app/`).

## 6. Modelo de datos

Base SQLite local versionada con `PRAGMA user_version`. Las fechas se guardan como texto ISO 8601 y los montos como números reales (en bolivianos).

### `eventos`

Cada evento con fecha e id de origen en SystemInscripciones.

| Campo | Tipo | Reglas |
|---|---|---|
| `id` | INTEGER | Clave primaria autoincremental |
| `nombre` | TEXT | Obligatorio |
| `fecha` | TEXT | Obligatorio |
| `origen_id` | TEXT | Opcional |

### `asistentes`

Lista local de asistentes con su código QR e instante de ingreso.

| Campo | Tipo | Reglas |
|---|---|---|
| `id` | INTEGER | Clave primaria autoincremental |
| `evento_id` | INTEGER | Obligatorio; Referencia a eventos (se borra en cascada) |
| `nombre` | TEXT | Obligatorio |
| `codigo` | TEXT | Obligatorio |
| `ingreso_en` | TEXT | Opcional |

Restricciones: `UNIQUE (evento_id, codigo)`.

### `sync_pendiente`

Cola de ingresos aún no enviados al servidor.

| Campo | Tipo | Reglas |
|---|---|---|
| `id` | INTEGER | Clave primaria autoincremental |
| `asistente_id` | INTEGER | Obligatorio; Referencia a asistentes (se borra en cascada) |
| `fecha_hora` | TEXT | Obligatorio |
| `enviado` | INTEGER | Obligatorio; Por defecto 0 |

Detalle de la política de migraciones: nunca editar una migración publicada; agregar una nueva al final de la lista en `src/db/schema.ts`.

## 7. Casos borde a contemplar

- Celular sin cámara funcional: búsqueda manual como alternativa.
- Dos celulares en la puerta del mismo evento: el servidor es la fuente de verdad; la app avisa si un código ya ingresó desde otro dispositivo al sincronizar.
- Reloj del teléfono desajustado: mostrar advertencia y guardar la hora del dispositivo.
- Lista actualizada durante el evento: refrescar sin perder ingresos ya registrados.
- QR con formato distinto al esperado: se trata como inválido.

## 8. Requisitos no funcionales

- **Offline total:** ninguna función del MVP requiere internet (salvo el envío de la sincronización, que se reintenta cuando hay conexión).
- **Rendimiento:** la app abre en menos de 2 segundos; las listas responden de inmediato con cientos de registros (consultas con índices y listas virtualizadas).
- **Privacidad:** los datos nunca salen del teléfono, salvo lo que el usuario comparte a propósito.
- **Idioma y formato:** español de Bolivia, montos en bolivianos (Bs) con dos decimales, fechas día/mes/año.
- **Usabilidad:** botones grandes, formularios cortos y flujos de pocos toques; pensado para usarse de pie y con una mano.
- **Accesibilidad básica:** contraste suficiente, etiquetas de accesibilidad en botones y tamaño de texto que respeta el ajuste del sistema.
- **Calidad:** `npm run lint`, `npx tsc --noEmit` y `npm test` pasan en cada commit (CI en GitHub Actions).
- **Compatibilidad:** Android 8 o superior (probado con Expo Go y con el APK).

### Capacidades de Expo que usa

| Paquete | Para qué | Permiso |
|---|---|---|
| `expo-camera` | Escanear códigos de barras o QR | Cámara |
| `expo-document-picker` | Elegir archivos para importar | — |
| `expo-network` | Detectar si hay conexión | — |
| `expo-haptics` | Vibración al validar | — |
| `expo-file-system` | Guardar y leer archivos locales (CSV, respaldos) | — |
| `expo-sharing` | Compartir PDF, CSV o respaldos con otras apps | — |

## 9. Riesgos y mitigaciones

| Riesgo | Mitigación |
|---|---|
| Dependencia de la API de SystemInscripciones | Congelar el contrato antes de la fase 4 y usar un servidor falso para desarrollar. |
| Colas de gente por escaneo lento | Pruebas de rendimiento con 2000 registros y feedback inmediato. |
| Cambios de esquema que rompan datos | Migraciones con `PRAGMA user_version`; nunca editar una migración publicada |
| El alcance crece | Todo lo que no esté en la sección 2 va a «Después del MVP» en el [ROADMAP](ROADMAP.md) |

## 10. Criterios para lanzar el MVP (v1.0.0)

- [ ] Todas las historias de la sección 4 cumplen sus criterios de aceptación.
- [ ] Todas las reglas de negocio de la sección 3 tienen pruebas o verificación manual.
- [ ] Probado en un teléfono Android real, en modo avión.
- [ ] APK generado con `npm run build:apk` e instalado en un teléfono limpio.
- [ ] README con capturas de pantalla y CHANGELOG actualizado.
- [ ] Release v1.0.0 publicado con el APK adjunto.

## 11. Glosario

| Término | Significado en AccesoQR |
|---|---|
| `eventos` | Cada evento con fecha e id de origen en SystemInscripciones. |
| `asistentes` | Lista local de asistentes con su código QR e instante de ingreso. |
| `sync_pendiente` | Cola de ingresos aún no enviados al servidor. |

## 12. Mensajes estándar

| Situación | Mensaje |
|---|---|
| Campo obligatorio vacío | Completá este campo |
| Número inválido | Ingresá un número válido |
| Monto cero o negativo | El monto debe ser mayor a 0 |
| Fecha inválida | Fecha no válida |
| Guardado correcto | Guardado |
| Error al guardar | No se pudo guardar. Intentá de nuevo |
| Confirmar borrado | ¿Borrar «{nombre}»? No se puede deshacer |
| Lista vacía | Todavía no hay nada acá. Tocá «+» para empezar |

## 13. Trazabilidad

| Historia | Fase | Casos de prueba |
|---|---|---|
| HU-01 Importar la lista de asistentes | 2 | CP-01-1, CP-01-2 |
| HU-02 Escanear y validar entradas | 3 | CP-02-1, CP-02-2, CP-02-3 |
| HU-03 Ver el contador en vivo | 3 | CP-03-1 |
| HU-04 Buscar y validar a mano | 3 | CP-04-1 |
| HU-05 Sincronizar con SystemInscripciones | 4 | CP-05-1, CP-05-2 |
| HU-06 Exportar asistencia | 5 | CP-06-1 |

## 14. Documentos relacionados

- [DATOS.md](DATOS.md) · [PANTALLAS.md](PANTALLAS.md) · [PRUEBAS.md](PRUEBAS.md) · [BACKLOG.md](BACKLOG.md) · [ROADMAP.md](ROADMAP.md) · [PLANNING.md](PLANNING.md)
