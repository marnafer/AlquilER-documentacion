# Variables de Entorno — AlquilER Backend

Referencia completa de las variables de entorno del backend (`.env`).

## Base de datos

| Variable | Requerida | Default | Descripción |
|---|---|---|---|
| `DB_HOST` | ✅ | — | Host de MySQL (ej: `localhost`) |
| `DB_DATABASE` | ✅ | — | Nombre de la base de datos |
| `DB_USERNAME` | ✅ | — | Usuario de MySQL |
| `DB_PASSWORD` | — | (vacío) | Contraseña de MySQL |
| `DB_PORT` | — | 3306 | Puerto de MySQL |

## JWT

| Variable | Requerida | Default | Descripción |
|---|---|---|---|
| `JWT_KEY` | ✅ | — | Clave secreta para firmar JWT (mín. 32 caracteres) |
| `JWT_EXP` | — | 3600 | Expiración del access token en segundos |

## App

| Variable | Requerida | Default | Descripción |
|---|---|---|---|
| `APP_ENV` | ✅ | `development` | Entorno: `development` o `production` |
| `APP_DEBUG` | — | `false` | Mostrar errores detallados (solo dev) |
| `APP_URL` | ✅ | — | URL base del backend (ej: `http://localhost:8000`) |
| `FRONTEND_URL` | — | `http://localhost:3000` | URL del frontend (para enlaces en emails) |

## Cookies de autenticación

| Variable | Requerida | Default | Descripción |
|---|---|---|---|
| `AUTH_COOKIE_SECURE` | — | (derivado de APP_ENV) | `true` en producción (HTTPS). El código usa `APP_ENV` para determinar esto. |
| `AUTH_COOKIE_SAMESITE` | — | (derivado de APP_ENV) | `Lax` en dev, `None` en producción. El código usa `APP_ENV`. |

> **Nota:** Estas variables están en `.env.example` pero el código actualmente usa `APP_ENV` para determinar `secure` y `samesite`. Asegurate de que `APP_ENV=production` en producción.

## CORS

| Variable | Requerida | Default | Descripción |
|---|---|---|---|
| `CORS_ALLOWED_ORIGINS` | — | (vacío) | Orígenes permitidos, separados por coma (ej: `https://alquilerr.alwaysdata.net`) |
| `TRUSTED_PROXIES` | — | (vacío) | IPs de proxies confiables (separadas por coma) |

## Rate Limiting

| Variable | Requerida | Default | Descripción |
|---|---|---|---|
| `RATE_LIMIT_STORAGE_PATH` | — | (temp del sistema) | Directorio para archivos de rate limiting |

## Correo (SMTP)

| Variable | Requerida | Default | Descripción |
|---|---|---|---|
| `MAIL_HOST` | — | (vacío → Mailpit en dev) | Host SMTP. Si vacío en dev, usa Mailpit (127.0.0.1:1025). En producción, **debe configurarse** o los emails fallan. |
| `MAIL_PORT` | — | 587 | Puerto SMTP |
| `MAIL_USERNAME` | — | (vacío) | Usuario SMTP |
| `MAIL_PASSWORD` | — | (vacío) | Contraseña SMTP |
| `MAIL_FROM` | — | `no-responder@alquiler.local` | Email remitente (debe ser válido en producción) |
| `MAIL_FROM_NAME` | — | `AlquilER` | Nombre del remitente |
| `MAIL_ENCRYPTION` | — | `tls` | Cifrado: `tls`, `ssl`, o `none` |
| `MAIL_TO` | — | (vacío) | Email que recibe los mensajes de contacto |

## Métricas de rendimiento (solo desarrollo)

| Variable | Requerida | Default | Descripción |
|---|---|---|---|
| `PERFORMANCE_METRICS_ENABLED` | — | `false` | Activar instrumentación de rendimiento |
| `PERFORMANCE_METRICS_SLOW_SQL_MS` | — | 100 | Umbral de consultas lentas (ms) |

## Ejemplo completo (.env de desarrollo)

```env
DB_HOST=localhost
DB_DATABASE=sistema_alquiler_db_dev
DB_USERNAME=root
DB_PASSWORD=

JWT_KEY=clave_aleatoria_de_al_menos_32_caracteres
JWT_EXP=3600
AUTH_COOKIE_SECURE=false
AUTH_COOKIE_SAMESITE=Lax

APP_ENV=development
APP_DEBUG=true
APP_URL=http://localhost:8000
FRONTEND_URL=http://localhost:3000

CORS_ALLOWED_ORIGINS=http://localhost,http://127.0.0.1,http://localhost:3000,http://127.0.0.1:3000
TRUSTED_PROXIES=
RATE_LIMIT_STORAGE_PATH=

MAIL_HOST=
MAIL_PORT=587
MAIL_USERNAME=
MAIL_PASSWORD=
MAIL_FROM=no-responder@alquiler.local
MAIL_FROM_NAME=AlquilER
MAIL_ENCRYPTION=tls
MAIL_TO=contacto@alquiler.com.ar

PERFORMANCE_METRICS_ENABLED=false
PERFORMANCE_METRICS_SLOW_SQL_MS=100
```

## Ejemplo completo (.env de producción)

```env
DB_HOST=localhost
DB_DATABASE=sistema_alquiler_db
DB_USERNAME=<usuario_mysql>
DB_PASSWORD=<password_mysql>

JWT_KEY=<clave_aleatoria_de_al_menos_32_caracteres>
JWT_EXP=3600
AUTH_COOKIE_SECURE=true
AUTH_COOKIE_SAMESITE=None

APP_ENV=production
APP_DEBUG=false
APP_URL=https://alquiler.alwaysdata.net
FRONTEND_URL=https://alquilerr.alwaysdata.net

CORS_ALLOWED_ORIGINS=https://alquilerr.alwaysdata.net
TRUSTED_PROXIES=
RATE_LIMIT_STORAGE_PATH=

MAIL_HOST=smtp.tuproveedor.com
MAIL_PORT=587
MAIL_USERNAME=tu_usuario
MAIL_PASSWORD=tu_password
MAIL_FROM=no-responder@tudominio.com
MAIL_FROM_NAME=AlquilER
MAIL_ENCRYPTION=tls
MAIL_TO=contacto@tudominio.com

PERFORMANCE_METRICS_ENABLED=false
PERFORMANCE_METRICS_SLOW_SQL_MS=100
```
