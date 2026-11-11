# Inventario de Tests — AlquilER

> Listado completo de tests existentes y faltantes del proyecto AlquilER.
> Para guiar la creación de nuevos tests.

---

## 1. Backend (PHPUnit)

### 1.1 Tests existentes (2.193 tests, 2.190 pasan, 3 fallan)

#### Unit Tests (tests/Unit/)

| Carpeta | Cantidad | Cobertura |
|---|---|---|
| `Controllers/Api/` | 20 | Controladores REST |
| `Services/` | 22 | Lógica de negocio |
| `Repositories/` | 21 | Acceso a datos |
| `Validators/` | 20 | Validación de campos |
| `Sanitizers/` | 18 | Sanitización de inputs |
| `Policies/` | 7 | Autorización |
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
| `Performance/` | 8 | Métricas de performance |
| `Controllers/` | 0 | (vacío, tests en Controllers/Api) |
| **Total Unit** | **~113** | |

#### Integration Tests (tests/Integration/)

| Archivo | Descripción | Estado |
|---|---|---|
| `IntegridadBaseDeDatosTest` | Verifica integridad de BD (FKs, constraints) | ✅ Pasa |
| `PropiedadImagenHttpTest` | Subida HTTP real de imágenes | ❌ 3 fallos |

#### Tests que fallan (3)

| Test | Causa probable | Solución |
|---|---|---|
| `test_subida_http_real_de_imagen_de_propiedad` | Requiere servidor web con GD y límites de upload | Configurar php.ini o usar mock |
| `test_rechaza_imagen_http_que_supera_15_mb` | Requiere límite de upload de 15MB en php.ini | Ajustar `upload_max_filesize` |
| `test_rechaza_imagen_http_con_dimension_excesiva` | Requiere validación de dimensiones en upload | Verificar configuración de GD |

---

### 1.2 Cobertura actual del backend

| Métrica | Valor |
|---|---|
| Líneas | 56,40% (10.493/18.606) |
| Métodos | 89,75% (876/976) |
| Clases | 81,10% (133/164) |

### 1.3 Tests faltantes en el backend

#### Servicios sin tests (o con baja cobertura)

| Servicio | Tests existentes | Faltante |
|---|---|---|
| `AutenticadorService` | ✅ AutenticadorServiceTest | — |
| `CategoriaService` | ✅ CategoriaServiceTest | — |
| `ConsultaService` | ✅ ConsultaServiceTest | — |
| `ContactoService` | ✅ ContactoServiceTest | — |
| `FavoritoService` | ✅ FavoritoServiceTest | — |
| `LocalidadService` | ✅ LocalidadServiceTest | — |
| `LogActividadService` | ✅ LogActividadServiceTest | — |
| `MailService` | ✅ MailServiceTest | — |
| `MensajeConsultaService` | ✅ MensajeConsultaServiceTest | — |
| `NotificacionService` | ✅ NotificacionServiceTest | — |
| `PropiedadImagenService` | ✅ PropiedadImagenServiceTest | — |
| `PropiedadService` | ✅ PropiedadServiceTest | — |
| `PropiedadServicioService` | ✅ PropiedadServicioServiceTest | — |
| `ProvinciaService` | ✅ ProvinciaServiceTest | — |
| `RecuperarContrasenaService` | ✅ RecuperarContrasenaServiceTest | — |
| `ResenaService` | ✅ ResenaServiceTest | — |
| `ReservaService` | ✅ ReservaServiceTest | — |
| `RolService` | ✅ RolServiceTest | — |
| `ServicioService` | ✅ ServicioServiceTest | — |
| `UsuarioService` | ✅ UsuarioServiceTest | — |

> **Nota**: Todos los servicios tienen tests. La baja cobertura (56,40%) se debe a que los tests de integración (que requieren BD) no se ejecutan en CI, y a que hay código en `src/` que no está cubierto (Helpers, Performance, Debug, etc.).

#### Código sin cobertura (estimado)

| Módulo | Cobertura estimada | Prioridad |
|---|---|---|
| `src/Helpers/` (JwtProvider, ResponseHelper) | Parcial | Alta |
| `src/Performance/` (métricas) | Parcial | Baja |
| `src/Debug/` (debugger) | Parcial | Baja |
| `src/Http/` (Request, Response) | Parcial | Media |
| `src/Cache/` (FileCache) | Parcial | Media |
| `src/Security/` (Authenticator) | Parcial | Alta |
| `src/Middlewares/` | Parcial | Media |
| `src/Repositories/` (Eloquent) | Parcial | Alta |
| `src/Sanitizers/` | Parcial | Media |
| `src/Validators/` | Parcial | Media |

---

## 2. Frontend (Playwright E2E)

### 2.1 Tests existentes (10 tests, 4 flujos críticos)

| Archivo | Flujos cubiertos |
|---|---|
| `e2e/admin.spec.js` | Panel de administración |
| `e2e/destacadas.spec.js` | Propiedades destacadas |
| `e2e/notificaciones.spec.js` | Notificaciones |
| `e2e/usuario.spec.js` | Flujo de usuario (login, reserva, etc.) |
| `tests/chips-solapamiento.spec.js` | Chips de filtros |

### 2.2 Flujos E2E cubiertos (4/8)

| Flujo | Estado | Test |
|---|---|---|
| Login | ✅ | `usuario.spec.js` |
| Registro | ✅ | `usuario.spec.js` |
| Publicación de propiedad | ✅ | `usuario.spec.js` |
| Reserva | ✅ | `usuario.spec.js` |
| Reseña | ⏳ Pendiente | — |
| Mensajería (consulta) | ⏳ Pendiente | — |
| Favoritos | ⏳ Pendiente | — |
| Notificaciones | ✅ | `notificaciones.spec.js` |

### 2.3 Tests unitarios del frontend

| Estado | Detalle |
|---|---|
| ❌ No implementados | No hay Jest/Vitest configurado |
| ❌ No implementados | No hay tests de componentes |
| ❌ No implementados | No hay tests de hooks |

---

## 3. Tests faltantes (priorizados)

### Prioridad Alta (críticos para release)

| ID | Test | Tipo | Descripción |
|---|---|---|---|
| T-01 | E2E: Flujo de reseña | E2E | Crear reseña después de reserva finalizada |
| T-02 | E2E: Mensajería de consulta | E2E | Enviar consulta → responder → conversación |
| T-03 | E2E: Favoritos | E2E | Agregar/quitar favorito, ver lista |
| T-04 | Fix: PropiedadImagenHttpTest | Integración | Corregir 3 fallos de subida de imágenes |
| T-05 | Unit: JwtProvider | Unit | Generar/validar access y refresh tokens |
| T-06 | Unit: Authenticator | Unit | Verificar autenticación, soloAdmin, papelera |

### Prioridad Media (mejora de calidad)

| ID | Test | Tipo | Descripción |
|---|---|---|---|
| T-07 | Unit: PropiedadService | Unit | CRUD, imágenes, servicios, soft delete |
| T-08 | Unit: ReservaService | Unit | Crear, confirmar, rechazar, cancelar, finalizar |
| T-09 | Unit: AutenticadorService | Unit | Login, refresh, logout, verificación |
| T-10 | Integration: API endpoints | Integration | Request/response de todos los endpoints |
| T-11 | Unit: FileCache | Unit | Cache hit/miss, TTL, invalidación |
| T-12 | Unit: RateLimitMiddleware | Unit | Límites por IP y endpoint |
| T-13 | Frontend: Tests unitarios | Unit | Configurar Vitest + tests de componentes |

### Prioridad Baja (nice to have)

| ID | Test | Tipo | Descripción |
|---|---|---|---|
| T-14 | Unit: Performance metrics | Unit | Métricas de performance |
| T-15 | Unit: Debug tools | Unit | Debugger, stats |
| T-16 | E2E: Panel admin completo | E2E | CRUD de todas las entidades en admin |
| T-17 | E2E: Responsive | E2E | Verificar layout en mobile/tablet/desktop |
| T-18 | E2E: Dark mode | E2E | Verificar cambio de tema |

---

## 4. Comandos para ejecutar tests

### Backend

```bash
# Todos los tests
composer test

# Solo unitarios (no requiere BD)
composer test:unit

# Solo integración (requiere MySQL)
composer test:integration

# Un test específico
vendor/bin/phpunit --filter NombreDelTest

# Con cobertura
vendor/bin/phpunit --coverage-text

# Con cobertura HTML
vendor/bin/phpunit --coverage-html coverage/
```

### Frontend

```bash
# E2E con Playwright
npx playwright test

# Un test específico
npx playwright test e2e/usuario.spec.js

# Modo UI (debug)
npx playwright test --ui

# Con reporte
npx playwright test --reporter=html
```

---

## 5. Configuración de CI

Los tests se ejecutan automáticamente en GitHub Actions:

| Workflow | Archivo | Jobs |
|---|---|---|
| Backend CI | `.github/workflows/ci.yml` (backend repo) | PHPUnit (unit), PHP Lint |
| Frontend CI | `.github/workflows/ci.yml` (frontend repo) | Vite Build, ESLint |

> **Nota**: Los tests de integración (que requieren BD) no se ejecutan en CI actualmente.

---

## 6. Notas

- La cobertura del backend (56,40%) es baja porque muchos tests de integración no se ejecutan en CI.
- El dashboard reportaba 97,83% pero eso era del 25/10/2026 con una configuración diferente de coverage.
- Los tests unitarios pasan todos (2.190/2.193). Los 3 fallos son de integración.
- Para mejorar la cobertura, priorizar tests de servicios y repositories (alta prioridad).
- Ver `BACKLOG.md` items TEST-001 a TEST-008 para el plan de testing.
- Ver `QA_UAT.md` para la estrategia de QA y UAT.
