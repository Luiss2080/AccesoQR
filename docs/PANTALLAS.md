# Pantallas y flujos — AccesoQR

> Guía de interfaz para construir cada pantalla: ruta, contenido, estados y bosquejo. Complementa la sección 5 de [SPEC.md](SPEC.md).

## 1. Mapa de rutas

| Pantalla | Archivo (Expo Router) | Tipo |
|---|---|---|
| Inicio | `app/index.tsx` | Inicio |
| Importar | `app/importar.tsx` | Lista / vista |
| Evento | `app/evento.tsx` | Lista / vista |
| Escáner | `app/escaner.tsx` | Lista / vista |
| Lista de asistentes | `app/lista-de-asistentes.tsx` | Lista / vista |
| Sincronización | `app/sincronizacion.tsx` | Lista / vista |
| Ajustes | `app/ajustes.tsx` | Lista / vista |

**Convenciones de interfaz**

- Una acción principal por pantalla, en un botón grande abajo o en un botón flotante.
- Los formularios se abren como pantalla completa, validan al enviar y muestran el error junto al campo.
- Toda lista tiene estado vacío con una frase y un botón que lleva a la acción.
- Las acciones que borran piden confirmación explícita con el nombre del registro.
- El botón atrás nunca pierde datos sin avisar: si el formulario tiene cambios, se pregunta antes de salir.
- Textos en español de Bolivia (voseo opcional, siempre claros); montos como `Bs 1.250,00`.

## 2. Detalle por pantalla

### 2.1 Inicio

- **Archivo:** `app/index.tsx`
- **Qué muestra:** Lista de eventos con ingresos y pendientes de sincronizar
- **Acciones:** Importar evento

**Bosquejo**

```
┌──────────────────────────────┐
│ AccesoQR                     │
├──────────────────────────────┤
│ [ Resumen y alertas ]        │
│                              │
│  ▸ Importar                 │
│  ▸ Evento                   │
│  ▸ Escáner                  │
│  ▸ Lista de asistentes      │
│                              │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay información para mostrar. |
| Con datos | Lista de eventos con ingresos y pendientes de sincronizar |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.2 Importar

- **Archivo:** `app/importar.tsx`
- **Qué muestra:** Selector de archivo y resumen de importación
- **Acciones:** Elegir archivo, confirmar
- **Historias relacionadas:** HU-01 (sugerido por coincidencia de temas; confirmar al construir).

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Importar                    │
├──────────────────────────────┤
│ nombre · fecha               │
│ nombre · fecha               │
│ nombre · fecha               │
│                              │
│                         (+)  │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay información para mostrar. |
| Con datos | Selector de archivo y resumen de importación |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.3 Evento

- **Archivo:** `app/evento.tsx`
- **Qué muestra:** Contador de ingresos, accesos a escáner, lista y sincronización
- **Acciones:** Escanear, buscar, sincronizar, exportar
- **Historias relacionadas:** HU-01, HU-02, HU-03, HU-04, HU-05, HU-06 (sugerido por coincidencia de temas; confirmar al construir).

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Evento                      │
├──────────────────────────────┤
│ nombre · fecha               │
│ nombre · fecha               │
│ nombre · fecha               │
│                              │
│                         (+)  │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay eventos. Tocá «Escanear» para empezar. |
| Con datos | Contador de ingresos, accesos a escáner, lista y sincronización |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.4 Escáner

- **Archivo:** `app/escaner.tsx`
- **Qué muestra:** Cámara con resultado grande en verde, rojo o amarillo, con sonido y vibración
- **Acciones:** Escanear, linterna, búsqueda manual
- **Historias relacionadas:** HU-02 (sugerido por coincidencia de temas; confirmar al construir).

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Escáner                     │
├──────────────────────────────┤
│ nombre · fecha               │
│ nombre · fecha               │
│ nombre · fecha               │
│                              │
│                         (+)  │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay información para mostrar. |
| Con datos | Cámara con resultado grande en verde, rojo o amarillo, con sonido y vibración |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.5 Lista de asistentes

- **Archivo:** `app/lista-de-asistentes.tsx`
- **Qué muestra:** Lista con filtro (todos, ingresaron, faltan) y búsqueda
- **Acciones:** Validar a mano
- **Historias relacionadas:** HU-01, HU-02, HU-03, HU-04, HU-05, HU-06 (sugerido por coincidencia de temas; confirmar al construir).

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Lista de asistentes         │
├──────────────────────────────┤
│ nombre · evento_id           │
│ nombre · evento_id           │
│ nombre · evento_id           │
│                              │
│                         (+)  │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay asistentes. Tocá «Validar a mano» para empezar. |
| Con datos | Lista con filtro (todos, ingresaron, faltan) y búsqueda |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.6 Sincronización

- **Archivo:** `app/sincronizacion.tsx`
- **Qué muestra:** Pendientes, último envío y errores
- **Acciones:** Sincronizar, reintentar
- **Historias relacionadas:** HU-05 (sugerido por coincidencia de temas; confirmar al construir).

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Sincronización              │
├──────────────────────────────┤
│ nombre · fecha               │
│ nombre · fecha               │
│ nombre · fecha               │
│                              │
│                         (+)  │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay información para mostrar. |
| Con datos | Pendientes, último envío y errores |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.7 Ajustes

- **Archivo:** `app/ajustes.tsx`
- **Qué muestra:** URL y token de SystemInscripciones
- **Acciones:** Guardar, probar conexión
- **Historias relacionadas:** HU-05 (sugerido por coincidencia de temas; confirmar al construir).

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Ajustes                     │
├──────────────────────────────┤
│ nombre · fecha               │
│ nombre · fecha               │
│ nombre · fecha               │
│                              │
│                         (+)  │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay información para mostrar. |
| Con datos | URL y token de SystemInscripciones |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

## 3. Flujos principales

**HU-01 — Importar la lista de asistentes**

```
lo importo  →  se crea el evento con 500 asistentes y un resumen de cuántos se importaron y cuántos tuvieron error
```

**HU-02 — Escanear y validar entradas**

```
escaneo su QR  →  pantalla verde con su nombre y se registra el ingreso
```

**HU-03 — Ver el contador en vivo**

```
abro el evento  →  veo «100 / 400» y el porcentaje, actualizado en cada escaneo
```

**HU-04 — Buscar y validar a mano**

```
lo busco por nombre y confirmo  →  se registra el ingreso igual, marcado como manual
```

**HU-05 — Sincronizar con SystemInscripciones**

```
toco «Sincronizar»  →  se envían y quedan marcados como enviados
```

**HU-06 — Exportar asistencia**

```
toco «Exportar CSV»  →  el archivo incluye nombre, código, ingresó (sí/no), hora y origen (QR o manual)
```
