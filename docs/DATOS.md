# Modelo de datos — AccesoQR

> Diseño detallado de la base SQLite: campos, validaciones, consultas y migraciones. Complementa la sección 6 de [SPEC.md](SPEC.md).

## 1. Convenciones

- Base local `SQLite` con `expo-sqlite`; versión del esquema en `PRAGMA user_version`.
- Claves primarias `INTEGER AUTOINCREMENT` llamadas `id`.
- Fechas como texto ISO `AAAA-MM-DD` (y `AAAA-MM-DDTHH:MM` cuando hay hora). Nunca se guardan textos como «hoy».
- Montos como `REAL` en bolivianos; se redondean a 2 decimales al guardar y se formatean al mostrar.
- Booleanos como `INTEGER` 0/1.
- Los textos se guardan sin espacios sobrantes (`trim()`).
- Todas las consultas usan parámetros (`?`), nunca concatenan texto del usuario.
- Las claves foráneas se activan con `PRAGMA foreign_keys = ON` al abrir la base.

## 2. Relaciones

| Campo | Apunta a | Comportamiento |
|---|---|---|
| `asistentes.evento_id` | `eventos` | Al borrar el padre se borran los hijos |
| `sync_pendiente.asistente_id` | `asistentes` | Al borrar el padre se borran los hijos |

```
eventos  ──<  asistentes
asistentes  ──<  sync_pendiente
sync_pendiente
```

## 3. Detalle por tabla

### 3.1 `eventos`

Cada evento con fecha e id de origen en SystemInscripciones.

| Campo | Tipo SQL → TS | Control en formulario | Validación | Mensaje de error |
|---|---|---|---|---|
| `id` | INTEGER | Automático | Clave primaria | — |
| `nombre` | TEXT → `string` | Texto | Obligatorio; Texto de 1 a 80 caracteres sin espacios sobrantes | «Escribí nombre» |
| `fecha` | TEXT → `string` | Fecha | Obligatorio; Fecha real en formato AAAA-MM-DD | «Fecha no válida» |
| `origen_id` | TEXT → `string \| null` | Texto | Opcional; Texto de 1 a 80 caracteres sin espacios sobrantes | «Escribí origen id» |

**Índices recomendados** (agregarlos en la migración si aún no existen):

```sql
CREATE INDEX IF NOT EXISTS idx_eventos_fecha ON eventos(fecha);
```

**Consultas base** (en `src/db/eventos.ts`):

```ts
listar(): Promise<Evento[]>            // SELECT * FROM eventos ORDER BY id DESC
obtener(id: number): Promise<Evento | null>   // SELECT * FROM eventos WHERE id = ?
crear(datos: NuevoEvento): Promise<number>   // INSERT INTO eventos (nombre, fecha, origen_id) VALUES (?, ?, ?)
actualizar(id: number, datos: NuevoEvento): Promise<void>   // UPDATE eventos SET nombre = ?, fecha = ?, origen_id = ? WHERE id = ?
eliminar(id: number): Promise<void>      // DELETE FROM eventos WHERE id = ?
```

### 3.2 `asistentes`

Lista local de asistentes con su código QR e instante de ingreso.

| Campo | Tipo SQL → TS | Control en formulario | Validación | Mensaje de error |
|---|---|---|---|---|
| `id` | INTEGER | Automático | Clave primaria | — |
| `evento_id` | INTEGER → `number` | Selector de eventos | Obligatorio; Debe existir en `eventos` | «Elegí un registro» |
| `nombre` | TEXT → `string` | Texto | Obligatorio; Texto de 1 a 80 caracteres sin espacios sobrantes | «Escribí nombre» |
| `codigo` | TEXT → `string` | Texto / escáner | Obligatorio; Sin espacios; máximo 64 caracteres | «Código no válido» |
| `ingreso_en` | TEXT → `string \| null` | Texto | Opcional; Texto de 1 a 80 caracteres sin espacios sobrantes | «Escribí ingreso en» |

**Restricciones de tabla:** `UNIQUE (evento_id, codigo)`.

**Índices recomendados** (agregarlos en la migración si aún no existen):

```sql
CREATE INDEX IF NOT EXISTS idx_asistentes_evento_id ON asistentes(evento_id);
CREATE INDEX IF NOT EXISTS idx_asistentes_codigo ON asistentes(codigo);
```

**Consultas base** (en `src/db/asistentes.ts`):

```ts
listar(): Promise<Asistente[]>            // SELECT * FROM asistentes ORDER BY id DESC
obtener(id: number): Promise<Asistente | null>   // SELECT * FROM asistentes WHERE id = ?
crear(datos: NuevoAsistente): Promise<number>   // INSERT INTO asistentes (evento_id, nombre, codigo, ingreso_en) VALUES (?, ?, ?, ?)
actualizar(id: number, datos: NuevoAsistente): Promise<void>   // UPDATE asistentes SET evento_id = ?, nombre = ?, codigo = ?, ingreso_en = ? WHERE id = ?
eliminar(id: number): Promise<void>      // DELETE FROM asistentes WHERE id = ?
listarPorEvento(evento_id: number): Promise<Asistente[]>   // WHERE evento_id = ?
```

### 3.3 `sync_pendiente`

Cola de ingresos aún no enviados al servidor.

| Campo | Tipo SQL → TS | Control en formulario | Validación | Mensaje de error |
|---|---|---|---|---|
| `id` | INTEGER | Automático | Clave primaria | — |
| `asistente_id` | INTEGER → `number` | Selector de asistentes | Obligatorio; Debe existir en `asistentes` | «Elegí un registro» |
| `fecha_hora` | TEXT → `string` | Hora | Obligatorio; Formato HH:MM (00:00–23:59) | «Hora no válida» |
| `enviado` | INTEGER → `number` | Numérico entero | Obligatorio; Entero mayor o igual a 0; Valor inicial `0` | «Ingresá un número entero» |

**Índices recomendados** (agregarlos en la migración si aún no existen):

```sql
CREATE INDEX IF NOT EXISTS idx_sync_pendiente_asistente_id ON sync_pendiente(asistente_id);
CREATE INDEX IF NOT EXISTS idx_sync_pendiente_fecha_hora ON sync_pendiente(fecha_hora);
```

**Consultas base** (en `src/db/sync_pendiente.ts`):

```ts
listar(): Promise<Sync_pendiente[]>            // SELECT * FROM sync_pendiente ORDER BY id DESC
obtener(id: number): Promise<Sync_pendiente | null>   // SELECT * FROM sync_pendiente WHERE id = ?
crear(datos: NuevoSync_pendiente): Promise<number>   // INSERT INTO sync_pendiente (asistente_id, fecha_hora, enviado) VALUES (?, ?, ?)
actualizar(id: number, datos: NuevoSync_pendiente): Promise<void>   // UPDATE sync_pendiente SET asistente_id = ?, fecha_hora = ?, enviado = ? WHERE id = ?
eliminar(id: number): Promise<void>      // DELETE FROM sync_pendiente WHERE id = ?
listarPorAsistente(asistente_id: number): Promise<Sync_pendiente[]>   // WHERE asistente_id = ?
```

## 4. Tipos TypeScript

Copiar a `src/db/tipos.ts`:

```ts
export interface Evento {
  id: number;
  nombre: string;
  fecha: string;
  origen_id: string | null;
}
export type NuevoEvento = Omit<Evento, 'id'>;

export interface Asistente {
  id: number;
  evento_id: number;
  nombre: string;
  codigo: string;
  ingreso_en: string | null;
}
export type NuevoAsistente = Omit<Asistente, 'id'>;

export interface Sync_pendiente {
  id: number;
  asistente_id: number;
  fecha_hora: string;
  enviado: number;
}
export type NuevoSync_pendiente = Omit<Sync_pendiente, 'id'>;

```

## 5. Valores permitidos (enumeraciones)

Esta app no usa columnas con valores cerrados.

## 6. Migraciones

Migración 1 = esquema inicial (ya en `src/db/schema.ts` tras la fase 1). Cada cambio posterior es una migración nueva al final de la lista; nunca se edita una ya publicada.

| Versión | Fase | Motivo |
|---|---|---|
| 2 | 3 | Distinguir ingresos validados a mano de los leídos por QR. |
| 3 | 4 | URL y token de SystemInscripciones. |

**Migración 2** (fase 3)

```sql
ALTER TABLE asistentes ADD COLUMN manual INTEGER NOT NULL DEFAULT 0;
```

**Migración 3** (fase 4)

```sql
CREATE TABLE ajustes (clave TEXT PRIMARY KEY, valor TEXT NOT NULL);
```

## 7. Esquema inicial completo

```sql
CREATE TABLE eventos (id INTEGER PRIMARY KEY AUTOINCREMENT, nombre TEXT NOT NULL, fecha TEXT NOT NULL, origen_id TEXT);
CREATE TABLE asistentes (id INTEGER PRIMARY KEY AUTOINCREMENT, evento_id INTEGER NOT NULL REFERENCES eventos(id) ON DELETE CASCADE, nombre TEXT NOT NULL, codigo TEXT NOT NULL, ingreso_en TEXT, UNIQUE (evento_id, codigo));
CREATE TABLE sync_pendiente (id INTEGER PRIMARY KEY AUTOINCREMENT, asistente_id INTEGER NOT NULL REFERENCES asistentes(id) ON DELETE CASCADE, fecha_hora TEXT NOT NULL, enviado INTEGER NOT NULL DEFAULT 0);
```

## 8. Reglas de integridad que NO delega a la base

- **RN-01** Cada código QR es único por asistente dentro de un evento.
- **RN-02** Un asistente solo puede ingresar una vez; el segundo escaneo se muestra como duplicado con la hora del primer ingreso.
- **RN-03** Un código que no está en la lista del evento es inválido y no registra nada.
- **RN-04** Cada ingreso válido se guarda con fecha y hora y se agrega a la cola de sincronización.
- **RN-05** La sincronización es idempotente: reenviar un ingreso ya enviado no lo duplica en el servidor.
- **RN-06** Los ingresos nunca se borran por una falla de sincronización; se reintentan.
- **RN-07** La lista de asistentes se importa antes del evento y se puede refrescar sin perder los ingresos ya registrados.

Estas reglas se implementan en `src/lib/` como funciones puras y se prueban de forma automática (ver [PRUEBAS.md](PRUEBAS.md)).
