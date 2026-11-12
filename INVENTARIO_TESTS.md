# Inventario de Tests - AlquilER

> Listado completo de tests existentes y faltantes del proyecto AlquilER.
> Para guiar la creacion de nuevos tests.

---

## 1. Backend (PHPUnit)

### 1.1 Tests existentes (2.300 tests, 7.115 assertions, todos pasan)

#### Unit Tests (tests/Unit/)

| Carpeta | Cantidad | Cobertura |
|---|---|---|
| `Controllers/Api/` | 20 | Controladores REST |
| `Services/` | 22 | Logica de negocio |
| `Repositories/` | 21 | Acceso a datos |
| `Validators/` | 20 | Validacion de campos |
| `Sanitizers/` | 18 | Sanitizacion de inputs |
| `Policies/` | 7 | Autorizacion |
| `Models/` | 1 | Relaciones de modelos |
| `Middlewares/` | 3 | CSRF, Debug, RateLimit |
| `Helpers/` | 4 | JwtProvider, Response, etc. |
| `Http/` | 2 | Request, Response |
| `Routes/` | 2 | Router |
| `Security/` | 1 | Authenticator |
| `Cache/` | 1 | FileCache |
| `Exceptions/` | 1 | GlobalExceptionHandler |
| `Debug/` | 1 | Debugger |
| `Docs/` | 1 | OpenApiSpec |
| `Performance/` | 8 | Metricas de performance |
| `Controllers/` | 0 | (vacio, tests en Controllers/Api) |
| **Total Unit** | **~113** | |

#### Integration Tests (tests/Integration/)

| Archivo | Descripcion | Estado |
|---|---|---|
| `IntegridadBaseDeDatosTest` | Verifica integridad de BD (FKs, constraints) | OK. Pasa |
| `PropiedadImagenHttpTest` | Subida HTTP real de imagenes | OK. Pasa (3 tests) |

### 1.2 Cobertura actual del backend

| Metrica | Valor |
|---|---|
| Lineas | 100.00% (10.986/10.986) |
| Metodos | 100.00% (1.008/1.008) |
| Clases | 100.00% (164/164) |

PHPUnit 11.5.56, 2.300 tests, 7.115 assertions. Todos los tests pasan.

### 1.3 Tests faltantes en el backend

Ninguno critico: la cobertura del backend es 100%. La deuda restante es de calidad (N+1, validacion inconsistente) y no de cobertura.

---

## 2. Frontend (Playwright E2E)

### 2.1 Tests existentes (92 tests = 23 x 4 viewports, 72 pasan, 20 saltados)

| Archivo | Flujos cubiertos |
|---|---|
| `tests/e2e/admin.spec.js` | Login admin, menu de administracion, panel de propiedades (carga, filtros, exportacion CSV, papelera), acceso denegado a usuario comun |
| `tests/e2e/destacadas.spec.js` | El admin marca una propiedad destacada y el Home la muestra; un usuario comun no puede destacar por API |
| `tests/e2e/notificaciones.spec.js` | Campanita, badge, dropdown y pagina de notificaciones |
| `tests/e2e/usuario.spec.js` | Registro con cuenta nueva, catalogo, detalle, favoritos, consulta y reserva desde el detalle, refresh automatico de sesion |
| `tests/chips-solapamiento.spec.js` | Solapamiento de chips en detalle y listado (regresion) |
| `tests/e2e/helpers.js` | Sesiones de API con cookies aisladas, login browser, limpieza de datos, carrusel del Home |
| `tests/e2e/smtp-server.cjs` | Servidor SMTP falso en 127.0.0.1:1025 para el flujo de registro |

### 2.2 Flujos E2E cubiertos (8/8)

| Flujo | Estado | Test |
|---|---|---|
| Login | OK | `usuario.spec.js`, `admin.spec.js` |
| Registro | OK | `usuario.spec.js` |
| Catalogo / detalle / favoritos | OK | `usuario.spec.js` |
| Consulta y reserva | OK | `usuario.spec.js` |
| Refresh automatico de sesion | OK | `usuario.spec.js` |
| Panel de administracion | OK | `admin.spec.js` |
| Propiedades destacadas | OK | `destacadas.spec.js` |
| Notificaciones | OK | `notificaciones.spec.js` |
| Chips (regresion visual) | OK | `chips-solapamiento.spec.js` |
| Responsive (4 viewports) | OK | todos los specs x movil-390, tablet-768, desktop-1024, desktop-1440 |

### 2.3 Tests unitarios del frontend

| Estado | Detalle |
|---|---|
| No implementados | No hay Vitest/Jest configurado |
| No implementados | No hay tests de componentes |
| No implementados | No hay tests de hooks |

---

## 3. Tests faltantes (priorizados)

### Prioridad Alta (criticos para release)

| ID | Test | Tipo | Descripcion |
|---|---|---|---|
| T-01 | E2E: Flujo de resena | E2E | Crear resena despues de reserva finalizada |
| T-02 | E2E: Mensajeria de consulta | E2E | Enviar consulta -> responder -> conversacion |
| T-03 | E2E: Favoritos | E2E | Ya cubierto en usuario.spec.js |
| T-04 | Fix: PropiedadImagenHttpTest | Integracion | Resuelto (3 tests pasan) |
| T-05 | Unit: JwtProvider | Unit | Cubierto por backend al 100% |
| T-06 | Unit: Authenticator | Unit | Cubierto por backend al 100% |

### Prioridad Media (mejora de calidad)

| ID | Test | Tipo | Descripcion |
|---|---|---|---|
| T-07 | Unit: PropiedadService | Unit | Cubierto por backend al 100% |
| T-08 | Unit: ReservaService | Unit | Cubierto por backend al 100% |
| T-09 | Unit: AutenticadorService | Unit | Cubierto por backend al 100% |
| T-10 | Integration: API endpoints | Integration | Cubierto (PropiedadImagenHttpTest + suite) |
| T-11 | Unit: FileCache | Unit | Cubierto por backend al 100% |
| T-12 | Unit: RateLimitMiddleware | Unit | Cubierto por backend al 100% |
| T-13 | Frontend: Tests unitarios | Unit | Configurar Vitest + tests de componentes |

### Prioridad Baja (nice to have)

| ID | Test | Tipo | Descripcion |
|---|---|---|---|
| T-14 | Unit: Performance metrics | Unit | Cubierto por backend al 100% |
| T-15 | Unit: Debug tools | Unit | Cubierto por backend al 100% |
| T-16 | E2E: Panel admin completo | E2E | CRUD de todas las entidades en admin |
| T-17 | E2E: Responsive | E2E | Cubierto (4 viewports en todos los specs) |
| T-18 | E2E: Dark mode | E2E | Verificar cambio de tema |

---

## 4. Comandos para ejecutar tests

### Backend

```bash
# Todos los tests
composer test

# Solo unitarios (no requiere BD)
composer test:unit

# Solo integracion (requiere MySQL)
composer test:integration

# Un test especifico
vendor/bin/phpunit --filter NombreDelTest

# Con cobertura
vendor/bin/phpunit --coverage-text

# Con cobertura HTML
vendor/bin/phpunit --coverage-html coverage/
```

### Frontend

```bash
# E2E con Playwright (levanta backend, SMTP y Vite solo)
npx playwright test

# Un archivo especifico
npx playwright test tests/e2e/usuario.spec.js

# Un proyecto especifico (viewport)
npx playwright test --project=movil-390
npx playwright test --project=desktop-1440

# Modo UI (debug)
npx playwright test --ui

# Con reporte
npx playwright test --reporter=html
```

---

## 5. Configuracion de CI

Los tests se ejecutan automaticamente en GitHub Actions:

| Workflow | Archivo | Jobs |
|---|---|---|
| Backend CI | `.github/workflows/ci.yml` (backend repo) | PHPUnit (unit), PHP Lint |
| Frontend CI | `.github/workflows/ci.yml` (frontend repo) | Vite Build, ESLint |

> Nota: Los tests de integracion (que requieren BD) no se ejecutan en CI actualmente.

---

## 6. Notas

- La cobertura del backend es 100% (10.986 lineas, 1.008 metodos, 164 clases).
- El dashboard reportaba 97,83% pero eso era del 25/10/2026 con una configuracion diferente de coverage.
- Los tests E2E requieren MySQL con la base de desarrollo y los usuarios de prueba (`e2e.admin@test.com`, `e2e.inquilino@test.com`, contrasena `E2e123456`): `php scripts/seed_e2e_users.php`.
- Playwright levanta el backend en 127.0.0.1:8000 con APP_ENV=development y DISABLE_RATE_LIMIT=1, un SMTP falso en 127.0.0.1:1025 y Vite en 127.0.0.1:3000.
- Para mejorar la cobertura del frontend, priorizar tests unitarios (Vitest).
- Ver `BACKLOG.md` items TEST-001 a TEST-008 (completados) y `QA_UAT.md` para la estrategia de QA y UAT.