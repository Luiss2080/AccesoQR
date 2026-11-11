<div align="center">

<img src="docs/assets/logo.svg" alt="AccesoQR" width="96" height="96"/>

# AccesoQR

**Check-in de eventos con QR sin conexión. Cliente móvil de SystemInscripciones: valida entradas en la puerta y sincroniza después.**

<p>
<img src="https://img.shields.io/badge/Expo-000020?style=for-the-badge&logo=expo&logoColor=white" alt="Expo"/>
<img src="https://img.shields.io/badge/React_Native-20232A?style=for-the-badge&logo=react&logoColor=61DAFB" alt="React Native"/>
<img src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript"/>
<img src="https://img.shields.io/badge/SQLite-003B57?style=for-the-badge&logo=sqlite&logoColor=white" alt="SQLite"/>
<img src="https://img.shields.io/badge/Licencia-MIT-green?style=for-the-badge" alt="MIT"/>
</p>

[🚀 Características](#-características) · [🧰 Stack](#-stack) · [📂 Estructura](#-estructura) · [🔧 Configuración](#-configuración) · [🧪 Pruebas](#-pruebas) · [🤝 Contribuir](#-contribuir) · [📄 Licencia](#-licencia)

</div>

---

## 🚀 Características

- **Escaneo QR offline** de entradas en la puerta.
- **Lista local** de asistentes descargada antes del evento.
- **Detección de duplicados** y entradas inválidas.
- **Contador en vivo** de ingresos.
- **Sincronización** con [SystemInscripciones](https://github.com/Luiss2080/SystemInscripciones) al recuperar conexión.
- **Exportación CSV** de asistencia.

> Sin servidores, sin suscripciones y sin servicios de pago: todos los datos viven en el dispositivo (SQLite).

## 🧰 Stack

| Capa | Tecnología |
| --- | --- |
| App | React Native + Expo (TypeScript) |
| Datos | SQLite local (`expo-sqlite`) |
| Navegación | Expo Router |
| Distribución | APK / EAS Build local |

### Modelo de datos

- `eventos (id, nombre, fecha)`
- `asistentes (id, evento_id, nombre, codigo, ingreso_en)`
- `sync_pendiente (id, asistente_id, fecha_hora)`

## 🗺️ Roadmap

- [ ] 1. Estructura base y navegación
- [ ] 2. Importación de asistentes
- [ ] 3. Escaneo y validación
- [ ] 4. Sincronización con SystemInscripciones
- [ ] 5. Exportación y reportes

> Detalle fase por fase: [docs/ROADMAP.md](docs/ROADMAP.md) · [docs/SPEC.md](docs/SPEC.md) · [docs/PLANNING.md](docs/PLANNING.md) · [docs/DATOS.md](docs/DATOS.md) · [docs/PANTALLAS.md](docs/PANTALLAS.md) · [docs/PRUEBAS.md](docs/PRUEBAS.md) · [docs/BACKLOG.md](docs/BACKLOG.md)

## 📂 Estructura

```text
AccesoQR/
├── app/            # Pantallas y rutas (Expo Router)
├── src/
│   ├── db/         # Esquema y consultas SQLite
│   ├── components/ # Componentes reutilizables
│   └── lib/        # Utilidades
├── docs/assets/    # Logo y recursos del README
└── .github/        # Plantillas, CI y Dependabot
```

## 🔧 Configuración

Requisitos: Node.js 20+ y la app Expo Go (o un emulador).

```bash
git clone https://github.com/Luiss2080/AccesoQR.git
cd AccesoQR
npm install
npx expo start
```

> El repositorio está en fase inicial: la estructura y la documentación están listas y el código de la app se construye siguiendo el roadmap.

## 🧪 Pruebas

```bash
npm test
npx tsc --noEmit
```

## 🤝 Contribuir

Lee la [guía de contribución](CONTRIBUTING.md) y el [código de conducta](CODE_OF_CONDUCT.md). Para reportar vulnerabilidades consulta la [política de seguridad](SECURITY.md); para dudas, [SUPPORT.md](SUPPORT.md).

## 📄 Licencia

Distribuido bajo licencia MIT. Consulta [LICENSE](LICENSE).

---

<div align="center">

Hecho por [Luiss2080](https://github.com/Luiss2080)

</div>
