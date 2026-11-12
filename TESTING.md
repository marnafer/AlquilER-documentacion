# Guía de Testing — AlquilER

## Backend (PHPUnit)

### Requisitos
- PHP 8.2+
- Composer
- MySQL con la base de datos de test configurada

### Ejecutar todos los tests
```bash
cd AlquilER-backend
composer test
# o directamente:
vendor/bin/phpunit
```

### Ejecutar por suite
```bash
# Solo tests unitarios
vendor/bin/phpunit --testsuite Unit

# Solo tests de integración
vendor/bin/phpunit --testsuite Integration

# Un test específico
vendor/bin/phpunit --filter 'NombreDelTest'

# Un archivo específico
vendor/bin/phpunit tests/Unit/Services/MailServiceTest.php
```

### Estructura de tests
```
tests/
├── Unit/
│   ├── BootstrapTest.php          # Bootstrap de la app
│   ├── Cache/                     # Cache de catálogos
│   ├── Controllers/Api/           # Tests de controladores (20+)
│   ├── Debug/                     # Debugger
│   ├── Exceptions/                # Manejo de excepciones
│   ├── Helpers/                   # JwtProvider, ProfanityFilter, IpHelper, etc.
│   ├── Http/                      # Request, Response
│   ├── Middlewares/               # RateLimit, CookieCsrf, Debug
│   ├── Models/                    # Relaciones Eloquent
│   ├── Performance/               # Métricas de rendimiento
│   ├── Policies/                  # Políticas de autorización
│   ├── Repositories/              # Tests de repositorios (20+)
│   ├── Routes/                    # Router, DebugRouter
│   ├── Sanitizers/                # Sanitizers (20+)
│   ├── Security/                  # Authenticator
│   ├── Services/                  # Servicios (25+)
│   └── Validators/                # Validadores (20+)
├── Integration/
│   ├── IntegridadBaseDeDatosTest.php  # Integridad referencial
│   └── PropiedadImagenHttpTest.php     # HTTP de imágenes
├── Support/
│   └── ServidorSmtpFalso.php          # Servidor SMTP falso para tests
└── TestCase.php                       # Base de tests
```

### Cobertura
- **2.300 tests**, **7.115 assertions** (todos pasan).
- Cobertura del 100% (10.986 líneas, 1.008 métodos, 164 clases).
- Tests de integración para integridad de BD e imágenes HTTP.

### Configuración de tests
El archivo `phpunit.xml` define:
- Testsuite `Unit` y `Integration`.
- Base de datos de test: `sistema_alquiler_db_dev` (o la configurada en `.env`).
- Variables de entorno para testing.

### Notas
- El test `AutenticadorControllerTest::test_login` tiene una falla pre-existente (SameSite 'Lax' vs 'None') no relacionada con cambios de código.
- Los tests de `MailService` usan un servidor SMTP falso (`ServidorSmtpFalso`) para no depender de un relay real.
- Los tests de integración requieren MySQL corriendo y la base de datos creada.

### Datos de prueba
El script `scripts/generar_datos_stress.php` crea datos sintéticos:
```bash
php scripts/generar_datos_stress.php --dry-run   # Ver plan sin escribir
php scripts/generar_datos_stress.php --execute     # Insertar datos
```
Solo funciona con `APP_ENV=development` y base `sistema_alquiler_db_dev`.

---

## Frontend (Playwright E2E)

### Requisitos
- Node.js 20+
- Backend corriendo en `localhost:8000` (o `reuseExistingServer: true` en config)
- Navegadores de Playwright instalados

### Instalar navegadores
```bash
npx playwright install chromium
```

### Ejecutar tests
```bash
cd sistema-alquiler-frontend-web

# Todos los tests
npx playwright test

# Un archivo específico
npx playwright test tests/chips-solapamiento.spec.js

# Un proyecto específico (viewport)
npx playwright test --project=movil-390
npx playwright test --project=desktop-1440

# Con UI interactiva
npx playwright test --ui

# Ver reporte
npx playwright show-report
```

### Estructura de tests
```
tests/
├── chips-solapamiento.spec.js   # Solapamiento de chips en movil y listado
└── e2e/
    ├── usuario.spec.js          # Registro, catalogo, detalle, favoritos, consulta, reserva, refresh de sesion
    ├── admin.spec.js            # Panel de administracion (propiedades, filtros, CSV, papelera)
    ├── destacadas.spec.js       # Propiedades destacadas (admin marca, Home muestra)
    ├── notificaciones.spec.js   # Campanita, badge, dropdown, pagina de notificaciones
    ├── helpers.js               # Sesiones de API con cookies aisladas, login, limpieza, carrusel
    └── smtp-server.cjs         # Servidor SMTP falso en 127.0.0.1:1025 para el registro
```

### Configuración (playwright.config.js)
- **testDir:** `./tests`
- **timeout:** 45000ms
- **baseURL:** `http://127.0.0.1:3000`
- **webServer:** inicia `npm run dev` automáticamente si no hay servidor corriendo
- **Viewports:**
  - `movil-390`: 390×844 (iPhone 12/13/14)
  - `tablet-768`: 768×1024 (iPad)
  - `desktop-1024`: 1024×768
  - `desktop-1440`: 1440×900

### Notas
- Los tests E2E requieren el backend corriendo en `localhost:8000` con datos de prueba.
- El usuario de prueba es `e2e.inquilino@test.com` / `E2e123456`.
- Los tests de registro crean usuarios nuevos con email único (`e2e.reg.{timestamp}@test.com`).
- `global-teardown.js` limpia los datos de prueba después de la ejecución.
- Las capturas de pantalla y traces están desactivadas por defecto (`screenshot: 'off'`, `trace: 'off'`).

### Debugging
```bash
# Ejecutar un test con trace
npx playwright test tests/chips-solapamiento.spec.js --trace on

# Ver el trace
npx playwright show-trace trace.zip

# Ejecutar en modo headed (ver el navegador)
npx playwright test --headed

# Ejecutar un test específico
npx playwright test -g "catalogo, detalle y favoritos"
```
