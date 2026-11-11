# Auditoría de Seguridad — AlquilER

> Auditoría de seguridad del proyecto AlquilER al 25/10/2026.
> Basada en revisión del código fuente, configuración y arquitectura.

---

## 1. Resumen ejecutivo

| Aspecto | Estado | Detalle |
|---|---|---|
| Autenticación | ✅ Buena | JWT (access 15 min, refresh 15 días) en cookies HttpOnly |
| Autorización | ✅ Buena | Policies por rol y ownership |
| CSRF | ✅ Buena | CookieCsrfMiddleware valida Origin en writes |
| CORS | ✅ Buena | Orígenes configurables por entorno |
| Rate limiting | ✅ Buena | 12 reglas por IP y endpoint |
| Contraseñas | ✅ Buena | bcrypt (PASSWORD_DEFAULT) |
| SQL Injection | ✅ Bajo riesgo | Eloquent ORM (parametrizado) |
| XSS | ✅ Bajo riesgo | React escapa automáticamente + sanitizers |
| Secrets en repo | ✅ Protegido | `.env` en `.gitignore` |
| Headers HTTP | ⚠️ Parcial | Solo `X-Content-Type-Options: nosniff` |
| Monitoreo | ❌ Falta | Sin monitoreo continuo ni alertas |
| CSP | ❌ Falta | Sin Content-Security-Policy |

---

## 2. Controles de seguridad implementados

### 2.1 Autenticación

| Control | Implementación | Archivo |
|---|---|---|
| Hash de contraseñas | bcrypt (`password_hash` con `PASSWORD_DEFAULT`) | `AutenticadorService.php` |
| Access token | JWT, 15 min de vida | `JwtProvider.php` |
| Refresh token | Token aleatorio, 15 días, hasheado con SHA-256 en BD | `AutenticadorService.php` |
| Cookies | HttpOnly, Secure (prod), SameSite (Lax/None) | `AuthCookies` |
| Renovación automática | Frontend renueva access token con refresh token | `autoRefresh.js` |
| Cierre de sesión | Invalida refresh token en BD | `AutenticadorService::logout()` |
| Solo 1 refresh token activo | Elimina tokens anteriores al login | `AutenticadorService::login()` |
| Verificación de email | Token con expiración 60 min | `AutenticadorService::register()` |
| Recuperación de contraseña | Token con expiración 60 min | `RecuperarContrasenaService` |

### 2.2 Autorización

| Control | Implementación | Archivo |
|---|---|---|
| Roles | Usuario (1) y Admin (2) | `Rol.php` (constantes) |
| Policies | Ver, crear, confirmar, rechazar, finalizar, cancelar, modificar, eliminar, restaurar | `src/Policies/*.php` |
| Ownership | Solo el propietario puede editar/eliminar su propiedad | `PropiedadPolicy` |
| Admin-only | Panel admin, papelera, logs, gestión de usuarios | `Authenticator::soloAdmin()` |
| Papelera | Solo admin puede ver/restaurar eliminados | `Authenticator::verificarSiPidePapelera()` |

### 2.3 CSRF (Cross-Site Request Forgery)

| Aspecto | Detalle |
|---|---|
| **Middleware** | `CookieCsrfMiddleware` |
| **Mecanismo** | Valida que el header `Origin` esté en la lista de orígenes permitidos |
| **Aplicación** | Solo en operaciones que modifican datos (POST, PUT, PATCH, DELETE) y que envían cookies de auth |
| **Rutas públicas** | Contacto no requiere CSRF (no envía cookies de auth) |
| **Configuración** | `CORS_ALLOWED_ORIGINS` en `.env` |

### 2.4 CORS (Cross-Origin Resource Sharing)

| Aspecto | Detalle |
|---|---|
| **Mecanismo** | `Access-Control-Allow-Origin` con origen específico (no `*`) |
| **Credentials** | `Access-Control-Allow-Credentials: true` |
| **Métodos** | GET, POST, PUT, PATCH, DELETE, OPTIONS |
| **Headers** | Content-Type, Authorization |
| **Exposed** | X-Request-ID |
| **Configuración** | `CORS_ALLOWED_ORIGINS` en `.env` |

### 2.5 Rate limiting

| Regla | Límite | Ventana |
|---|---|---|
| login | 5 | 60s |
| register | 5 | 600s (10 min) |
| recuperar | 3 | 300s (5 min) |
| restablecer | 5 | 300s (5 min) |
| refresh | 10 | 60s |
| contacto | 10 | 60s |
| crear_consulta | 10 | 60s |
| enviar_mensaje | 20 | 60s |
| admin | 30 | 60s |
| get | 120 | 60s |
| write | 30 | 60s |
| default | 60 | 60s |

- **Almacenamiento**: Archivos JSON por IP en directorio temporal
- **Respuesta 429**: Incluye header `Retry-After` y `X-RateLimit-Limit` / `X-RateLimit-Remaining`
- **Fallback**: Si el directorio no es escribible, permite la solicitud (fail-open)

### 2.6 Sanitización de inputs

| Componente | Archivo | Detalle |
|---|---|---|
| Sanitizers | `src/Sanitizers/*.php` | Sanitización por entidad (HTML, XSS) |
| Validators | `src/Validators/*.php` | Validación de formato, tipo, rangos |
| Profanity filter | `src/utils/profanity.js` (frontend) | Filtro de profanidad en frontend |

### 2.7 Protección de datos sensibles

| Control | Implementación |
|---|---|
| Contraseñas | bcrypt con `PASSWORD_DEFAULT` (cost 10) |
| Tokens JWT | No almacenados en BD (stateless) |
| Refresh tokens | Hasheados con SHA-256 en BD |
| `.env` | En `.gitignore`, no commiteado |
| Logs | En producción, no persisten MIME completo (con tokens) |
| Uploads | Directorio `public/uploads/` bloqueado por `.htaccess` |
| Logs | Acceso directo bloqueado por `.htaccess` (`*.log`, `*.env`) |

---

## 3. Vulnerabilidades identificadas

### 3.1 Alta severidad

| ID | Vulnerabilidad | Descripción | Impacto | Mitigación recomendada |
|---|---|---|---|---|
| V-01 | **Sin Content-Security-Policy (CSP)** | No hay header CSP. Un atacante podría inyectar scripts. | Alto | Agregar `Content-Security-Policy` en `.htaccess` o `index.php` |
| V-02 | **Sin HSTS (Strict-Transport-Security)** | No fuerza HTTPS. Un atacante podría hacer downgrade a HTTP. | Alto | Agregar `Strict-Transport-Security: max-age=31536000` |
| V-03 | **Sin X-Frame-Options** | La app puede ser embebida en un iframe (clickjacking). | Medio | Agregar `X-Frame-Options: DENY` o `SAMEORIGIN` |
| V-04 | **Fuerza de contraseña no validada** | No se valida complejidad mínima en el registro. | Medio | Agregar validación en `UsuarioValidator` (min 8 chars, 1 mayúscula, 1 número) |

### 3.2 Media severidad

| ID | Vulnerabilidad | Descripción | Impacto | Mitigación recomendada |
|---|---|---|---|---|
| V-05 | **Rate limiting fail-open** | Si el directorio de storage no es escribible, permite todas las solicitudes. | Medio | Fail-closed o alerta cuando no se puede escribir |
| V-06 | **Sin monitoreo de seguridad** | No hay detección de intentos de brute force, anomalías, etc. | Medio | Implementar logging de seguridad + alertas |
| V-07 | **CORS con trailing slash** | Si `CORS_ALLOWED_ORIGINS` tiene trailing slash, el match falla. | Bajo | Normalizar orígenes (trim trailing slash) |
| V-08 | **Sin rotación de secrets** | JWT_KEY no tiene rotación. Si se compromete, todos los tokens son válidos. | Medio | Implementar rotación de claves con gracia |
| V-09 | **Debug endpoints en producción** | `DebugController` expone stats, logs, phpinfo. | Alto | Deshabilitar en producción (`APP_ENV=production`) |

### 3.3 Baja severidad

| ID | Vulnerabilidad | Descripción | Impacto | Mitigación recomendada |
|---|---|---|---|---|
| V-10 | **Sin protección contra enumeración de usuarios** | El login dice "credenciales inválidas" (no revela si el email existe). ✅ Ya protegido. | Bajo | Mantener mensaje genérico |
| V-11 | **Sin CAPTCHA en login/registro** | Posible automatización de registros. | Bajo | Agregar CAPTCHA (reCAPTCHA/hCaptcha) |
| V-12 | **Sin 2FA** | No hay autenticación de dos factores. | Bajo | Opcional: implementar TOTP |
| V-13 | **Imágenes sin validación de contenido** | Se valida mime type y dimensiones, pero no se verifica que sea una imagen real (magic bytes). | Bajo | Verificar magic bytes además de extensión |

---

## 4. OWASP Top 10 — Evaluación

| # | Vulnerabilidad OWASP | Estado en AlquilER | Detalle |
|---|---|---|---|
| A01 | Broken Access Control | ✅ Mitigado | Policies por rol y ownership. Admin-only endpoints protegidos. |
| A02 | Cryptographic Failures | ✅ Mitigado | bcrypt para contraseñas, JWT firmado, refresh tokens hasheados. |
| A03 | Injection | ✅ Mitigado | Eloquent ORM (parametrizado), sanitizers en inputs. |
| A04 | Insecure Design | ⚠️ Parcial | Falta CSP, HSTS, X-Frame-Options. |
| A05 | Security Misconfiguration | ⚠️ Parcial | Debug endpoints, falta de headers de seguridad. |
| A06 | Vulnerable Components | ⚠️ Sin auditoría | No se ejecuta `npm audit` ni `composer audit` automáticamente. |
| A07 | Auth Failures | ✅ Mitigado | Rate limiting en login, tokens con expiración, cookies HttpOnly. |
| A08 | Data Integrity Failures | ✅ Mitigado | CSRF protection, validación de Origin. |
| A09 | Logging Failures | ⚠️ Parcial | Logs existen pero sin monitoreo ni alertas. |
| A10 | SSRF | ✅ N/A | No hay requests a URLs proporcionadas por el usuario. |

---

## 5. Recomendaciones prioritarias

### Prioridad 1 (Crítico — implementar antes del release)

1. **Deshabilitar DebugController en producción** (V-09)
   - Verificar `APP_ENV=production` deshabilita rutas de debug
   - O agregar middleware que bloquee `/api/debug/*` en producción

2. **Agregar headers de seguridad** (V-01, V-02, V-03)
   ```apache
   # En .htaccess o index.php
   Header set X-Content-Type-Options "nosniff"
   Header set X-Frame-Options "SAMEORIGIN"
   Header set Strict-Transport-Security "max-age=31536000; includeSubDomains"
   Header set Content-Security-Policy "default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'"
   ```

3. **Validar complejidad de contraseña** (V-04)
   - Mínimo 8 caracteres, 1 mayúscula, 1 número
   - En `UsuarioValidator` y frontend

### Prioridad 2 (Importante — próximo sprint)

4. **Auditar dependencias** (V-06)
   - `npm audit` y `composer audit` en CI
   - Actualizar paquetes con vulnerabilidades

5. **Monitoreo de seguridad** (V-06)
   - Logging de intentos de login fallidos
   - Alertas por múltiples 401/403/429

6. **Fail-closed en rate limiting** (V-05)
   - Si no se puede escribir, bloquear en lugar de permitir

### Prioridad 3 (Mejora — largo plazo)

7. **CAPTCHA en login/registro** (V-11)
8. **2FA opcional** (V-12)
9. **Rotación de JWT_KEY** (V-08)
10. **Validación de magic bytes en imágenes** (V-13)

---

## 6. Checklist de seguridad para deploy

- [ ] `APP_ENV=production` configurado
- [ ] `APP_DEBUG=false`
- [ ] `JWT_KEY` con al menos 32 caracteres aleatorios
- [ ] `AUTH_COOKIE_SECURE=true`
- [ ] `AUTH_COOKIE_SAMESITE=None` (para cross-origin)
- [ ] `CORS_ALLOWED_ORIGINS` con dominio específico (no `*`)
- [ ] `MAIL_HOST` configurado (SMTP real)
- [ ] `.env` no commiteado en el repo
- [ ] Debug endpoints deshabilitados
- [ ] Headers de seguridad configurados
- [ ] Backups de BD configurados
- [ ] Monitoreo de uptime activo

---

## 7. Referencias cruzadas

- `RIESGOS.md` — Riesgos de seguridad (RS-01 a RS-10)
- `REGLAS_NEGOCIO.md` — Reglas de seguridad (RN-63 a RN-71)
- `REQUERIMIENTOS.md` — RNF-02 (Seguridad), RNF-08 (Rate limiting)
- `ARQUITECTURA.md` — Sección 10: Seguridad en la arquitectura
- `BACKLOG.md` — Items de seguridad (BL-003, BL-004, BL-005)
- `CI_CD.md` — Pipeline de CI/CD

---

## 8. Notas

- Esta auditoría se basa en la revisión del código fuente al 25/10/2026.
- Las vulnerabilidades se clasifican según OWASP y severidad (Crítica, Alta, Media, Baja).
- Las recomendaciones de Prioridad 1 deben implementarse antes del release a producción.
- Verificar periódicamente con `npm audit` y `composer audit`.
- Considerar una auditoría externa antes del lanzamiento público.
