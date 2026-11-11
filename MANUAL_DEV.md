# Manual del Desarrollador — AlquilER

> Guía completa para desarrolladores que trabajan en el proyecto AlquilER.
> Cubre instalación, configuración, desarrollo, testing y despliegue.

---

## 1. Requisitos previos

### 1.1 Software necesario

| Herramienta | Versión mínima | Verificar con |
|---|---|---|
| PHP | 8.2+ | `php -v` |
| Composer | 2.x | `composer --version` |
| Node.js | 18+ | `node -v` |
| npm | 9+ | `npm -v` |
| MySQL | 8.0+ | `mysql --version` |
| Git | 2.x | `git --version` |
| Apache/Nginx | (opcional) | — |

### 1.2 Extensiones PHP requeridas

- `pdo`, `pdo_mysql`
- `gd` (procesamiento de imágenes)
- `mbstring`
- `xml`
- `json`
- `fileinfo`

### 1.3 Herramientas recomendadas

- **IDE**: VS Code con extensiones PHP Intelephense, ESLint, Prettier
- **Cliente MySQL**: phpMyAdmin, DBeaver, o MySQL Workbench
- **Cliente API**: Postman o Insomnia
- **Git GUI**: GitKraken, SourceTree, o GitHub Desktop

---

## 2. Instalación del backend

### 2.1 Clonar el repositorio

```bash
git clone https://github.com/marnafer/AlquilER-backend.git
cd AlquilER-backend
git checkout dev
```

### 2.2 Instalar dependencias

```bash
composer install
```

### 2.3 Configurar variables de entorno

```bash
cp .env.example .env
```

Editar `.env` con los valores locales:

```env
DB_HOST=127.0.0.1
DB_DATABASE=sistema_alquiler_db
DB_USERNAME=root
DB_PASSWORD=

JWT_KEY=una_clave_aleatoria_larga_de_al_menos_32_caracteres
JWT_EXP=3600

APP_ENV=development
APP_DEBUG=true
APP_URL=http://localhost:8000

CORS_ALLOWED_ORIGINS=http://localhost,http://127.0.0.1,http://localhost:3000,http://127.0.0.1:3000

MAIL_HOST=
MAIL_PORT=587
MAIL_USERNAME=
MAIL_PASSWORD=
MAIL_FROM=no-responder@alquiler.local
MAIL_FROM_NAME=AlquilER
MAIL_ENCRYPTION=tls
MAIL_TO=contacto@alquiler.com.ar

FRONTEND_URL=http://localhost:3000

PERFORMANCE_METRICS_ENABLED=false
```

### 2.4 Crear la base de datos

```sql
CREATE DATABASE sistema_alquiler_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### 2.5 Ejecutar el esquema

```bash
mysql -u root -p sistema_alquiler_db < create_database.sql
```

O importar `create_database.sql` desde phpMyAdmin.

### 2.6 Ejecutar migraciones adicionales

```bash
mysql -u root -p sistema_alquiler_db < scripts/migrations/20261010_precalificacion_interesados.sql
```

### 2.7 (Opcional) Seed de datos de prueba

```bash
php scripts/seeders/seed_dev.php
```

O ejecutar manualmente los seeders disponibles.

### 2.8 Iniciar el servidor de desarrollo

```bash
php -S 127.0.0.1:8000 -t public public/router-dev.php
```

El backend estará disponible en `http://127.0.0.1:8000`.

---

## 3. Instalación del frontend

### 3.1 Clonar el repositorio

```bash
git clone https://github.com/marnafer/AlquilER-frontend-web.git
cd AlquilER-frontend-web
git checkout dev
```

### 3.2 Instalar dependencias

```bash
npm install
```

### 3.3 Configurar variables de entorno

Crear archivo `.env` en la raíz del proyecto frontend:

```env
VITE_API_URL=http://127.0.0.1:8000
```

### 3.4 Iniciar el servidor de desarrollo

```bash
npm run dev
```

El frontend estará disponible en `http://localhost:3000`. El proxy de Vite redirige `/api` al backend.

### 3.5 Build de producción

```bash
npm run build
```

Genera la carpeta `dist/` con los archivos estáticos.

---

## 4. Estructura del proyecto

### 4.1 Backend

```
AlquilER/
├── public/                    # Document root
│   ├── index.php              # Front controller
│   ├── router-dev.php         # Router para PHP built-in server
│   └── .htaccess              # Rewrite rules + charset
├── src/
│   ├── Cache/                 # FileCache
│   ├── Controllers/Api/       # 20 controladores REST
│   ├── Exceptions/            # Excepciones personalizadas
│   ├── Helpers/               # JwtProvider, ResponseHelper
│   ├── Http/                  # Request, Response
│   ├── Middlewares/           # CookieCsrf, Debug, RateLimit
│   ├── Models/                # 18 modelos Eloquent
│   ├── Performance/           # Instrumentación
│   ├── Policies/              # Autorización (ownership, roles)
│   ├── Repositories/          # Acceso a datos + interfaces
│   ├── Routes/                # api.php (113 rutas)
│   ├── Sanitizers/            # Sanitización de inputs
│   ├── Security/              # Authenticator (JWT)
│   ├── Services/              # 23 servicios de negocio
│   └── Validators/            # 19 validadores
├── config/                    # Configuración principal
├── scripts/                   # Migraciones, seeders, utilidades
├── storage/                   # Uploads, logs, cache, rate_limit
├── tests/                     # 100+ tests (Unit, Integration)
├── create_database.sql        # Esquema completo de BD
├── composer.json              # Dependencias PHP
├── phpunit.xml                # Configuración de tests
└── .env                       # Variables de entorno
```

### 4.2 Frontend

```
sistema-alquiler-frontend-web/
├── src/
│   ├── api.js                 # Todas las llamadas HTTP al backend
│   ├── autoRefresh.js         # Renovación automática de token
│   ├── components/            # 13 componentes reutilizables
│   ├── context/               # AuthContext, ThemeContext, UIContext
│   ├── hooks/                 # useAuth, useScrollReveal, useSEO
│   ├── pages/                 # 35 páginas
│   ├── pages/admin/           # 12 páginas de administración
│   ├── services/              # api.js, autoRefresh.js
│   ├── styles/                # CSS (base, responsive, tokens)
│   ├── utils/                 # Utilidades (profanity, vistos, etc.)
│   └── assets/                # Imágenes, iconos
├── public/
│   ├── .htaccess              # SPA fallback + UTF-8 charset
│   └── ...
├── vite.config.ts             # Configuración de Vite
└── package.json               # Dependencias y scripts
```

---

## 5. Convenciones de código

### 5.1 Backend (PHP)

- **Estilo**: PSR-12 (estándar de PHP)
- **Namespaces**: `App\...` (PSR-4 autoload)
- **Arquitectura**: Controller → Service → Repository → Model
- **Interfaces**: Todas las dependencias usan interfaces (RepositoryInterface, TokenProviderInterface)
- **Validación**: Validators retornan arrays con `success` y `errors`
- **Excepciones**: Usar excepciones personalizadas (BadRequestException, ForbiddenException, etc.)
- **Soft delete**: Modelos con `SoftDeletes` trait, filtro automático en queries
- **Nomenclatura**: camelCase para métodos, snake_case para columnas de BD

### 5.2 Frontend (React)

- **Estilo**: ESLint con configuración estándar
- **Componentes**: Funcionales con hooks
- **Estado**: Context API (AuthContext, ThemeContext, UIContext)
- **Rutas**: React Router v6 con PrivateRoute, GuestRoute, AdminRoute
- **API**: Un solo archivo `services/api.js` con todas las llamadas
- **CSS**: CSS variables + responsive.css con media queries
- **Nomenclatura**: PascalCase para componentes, camelCase para funciones

### 5.3 Git

- **Rama principal**: `master`
- **Rama de desarrollo**: `dev`
- **Feature branches**: `feature/nombre-descriptivo`
- **Commits**: Mensajes descriptivos en español
- **PRs**: A `dev`, luego merge a `master` para release

---

## 6. Desarrollo

### 6.1 Agregar un nuevo endpoint (backend)

1. Crear el método en el Controller correspondiente
2. Agregar la ruta en `src/Routes/api.php`
3. Crear/actualizar el Validator si es necesario
4. Crear/actualizar el Service con la lógica de negocio
5. Crear/actualizar el Repository si necesita acceso a BD
6. Agregar Policy si requiere autorización
7. Agregar tests unitarios e integración
8. Actualizar `openapi.json` y `API-CONTRACT.md`

### 6.2 Agregar una nueva página (frontend)

1. Crear el componente en `src/pages/NombrePagina.jsx`
2. Agregar la ruta en `src/AppRouter.jsx`
3. Agregar link en `Header.jsx` o `Footer.jsx` si es necesario
4. Agregar estilos en `src/styles/` si es necesario
5. Actualizar `WIREFRAMES.md` si es una pantalla nueva

### 6.3 Agregar una nueva tabla (BD)

1. Crear migración SQL en `scripts/migrations/`
2. Actualizar `create_database.sql`
3. Crear modelo en `src/Models/`
4. Crear repository + interface en `src/Repositories/`
5. Crear service en `src/Services/`
6. Crear controller en `src/Controllers/Api/`
7. Agregar rutas en `src/Routes/api.php`
8. Actualizar `ERD.md` y `schema.sql`

---

## 7. Testing

### 7.1 Backend (PHPUnit)

```bash
# Todos los tests
composer test

# Solo unitarios (no requiere BD)
composer test:unit

# Solo integración (requiere MySQL corriendo)
composer test:integration

# Un test específico
vendor/bin/phpunit --filter NombreDelTest
```

### 7.2 Frontend (Playwright)

```bash
# Instalar browsers (primera vez)
npx playwright install

# Ejecutar tests E2E
npx playwright test

# Ejecutar un test específico
npx playwright test tests/login.spec.js

# Modo UI (debug)
npx playwright test --ui
```

### 7.3 Linting

```bash
# Backend: PHP Lint
find src -name "*.php" -print0 | xargs -0 -n1 php -l

# Frontend: ESLint
npx eslint src --ext .js,.jsx
```

### 7.4 Cobertura

```bash
# Backend: PHPUnit con coverage
vendor/bin/phpunit --coverage-html coverage/
# Abrir coverage/index.html en el navegador
```

---

## 8. Despliegue

### 8.1 Backend (alwaysdata)

1. Hacer push a `master` (o `dev` para staging)
2. Conectar por FTP/SFTP a alwaysdata
3. Subir archivos modificados (excluir `.env`, `vendor/`, `storage/`)
4. Ejecutar migraciones SQL pendientes en la BD de producción
5. Verificar `.env` de producción (APP_ENV=production, MAIL_HOST configurado)
6. Verificar health check: `https://alquiler.alwaysdata.net/api/health`

### 8.2 Frontend (alwaysdata)

1. Ejecutar `npm run build`
2. Subir contenido de `dist/` a `~/www/` en alwaysdata
3. Verificar que `.htaccess` esté en la raíz del deploy
4. Verificar la app: `https://alquilerr.alwaysdata.net`

### 8.3 Variables de entorno de producción

```env
APP_ENV=production
APP_DEBUG=false
APP_URL=https://alquilerr.alwaysdata.net
API_URL=https://alquiler.alwaysdata.net

DB_HOST=mysql-alquiler.alwaysdata.net
DB_DATABASE=alquiler_db
DB_USERNAME=alquiler
DB_PASSWORD=***

JWT_KEY=clave_aleatoria_larga_de_produccion
AUTH_COOKIE_SECURE=true
AUTH_COOKIE_SAMESITE=None

CORS_ALLOWED_ORIGINS=https://alquilerr.alwaysdata.net

MAIL_HOST=smtp.resend.com
MAIL_PORT=587
MAIL_USERNAME=resend
MAIL_PASSWORD=re_***
MAIL_FROM=no-responder@alquiler.com.ar
MAIL_FROM_NAME=AlquilER
MAIL_ENCRYPTION=tls
MAIL_TO=contacto@alquiler.com.ar

FRONTEND_URL=https://alquilerr.alwaysdata.net
```

---

## 9. Debugging

### 9.1 Backend

- **Logs**: `storage/logs/errores.log`, `storage/logs/emails.log`
- **Performance**: `storage/logs/performance.jsonl` (solo en dev con `PERFORMANCE_METRICS_ENABLED=true`)
- **Debug endpoints**: `/api/debug/stats`, `/api/debug/logs`, `/api/debug/testDB` (solo en dev)
- **PhpStorm/Xdebug**: Configurar Xdebug en `php.ini` y usar breakpoints

### 9.2 Frontend

- **Console**: Errores en la consola del navegador (F12)
- **Network**: Pestaña Network para ver requests/responses
- **React DevTools**: Extensión de Chrome para inspeccionar componentes
- **Vite**: Logs en la terminal donde corre `npm run dev`

---

## 10. Referencias

- `README-BACKEND.md` — Guía rápida de instalación del backend
- `README-FRONTEND.md` — Guía rápida de desarrollo del frontend
- `ARQUITECTURA.md` — Arquitectura del sistema
- `ERD.md` — Diagrama entidad-relación
- `API-CONTRACT.md` — Contrato de la API
- `DEPLOY.md` — Guía de despliegue
- `ENV.md` — Referencia de variables de entorno
- `TROUBLESHOOTING.md` — Problemas comunes y soluciones
- `CI_CD.md` — Configuración de CI/CD
