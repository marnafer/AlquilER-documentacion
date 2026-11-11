# Troubleshooting — AlquilER

Problemas comunes encontrados durante el desarrollo y su solución.

## 1. Mojibake (acentos y ñ rotos en el navegador)

**Síntoma:** Los acentos y la ñ se ven como `Ã³`, `Ã±`, `�`, etc.

**Causa:** Apache servía los archivos JS y CSS con `Content-Type: text/javascript` / `text/css` sin `charset=utf-8`. Algunos navegadores usaban la codificación del sistema (Windows-1252) en vez de UTF-8.

**Solución:** Se agregó `AddDefaultCharset utf-8` y `AddCharset utf-8 .js .css` en `public/.htaccess`. Verificar que el header HTTP incluya `charset=utf-8`:
```bash
curl -I https://alquilerr.alwaysdata.net/assets/index-*.js
# Debe mostrar: content-type: text/javascript; charset=utf-8
```

## 2. Cookies no se guardan/envían en producción

**Síntoma:** El login retorna 200 pero las requests autenticadas fallan con 401. Las cookies no aparecen en DevTools.

**Causas posibles:**
- `APP_ENV` no está en `production` → las cookies se setean con `SameSite=Lax` y `Secure=false`, que no funcionan cross-origin.
- El frontend no envía `credentials: 'include'` en el fetch (el interceptor `autoRefresh.js` lo agrega para requests a la API, pero el login debe incluirlo).

**Solución:**
1. Verificar que `APP_ENV=production` en el `.env` del backend.
2. Verificar que el frontend envíe `credentials: 'include'` (ya incluido en `autoRefresh.js`).
3. Verificar que el backend retorne `Access-Control-Allow-Credentials: true` y `Access-Control-Allow-Origin: https://alquilerr.alwaysdata.net`.

## 3. Email de verificación no llega

**Síntoma:** El usuario se registra pero no recibe el email de verificación. En dev tools se ve la respuesta 401 "Email no verificado".

**Causa:** `MAIL_HOST` no está configurado en producción. El backend guardaba el email en el log (solo metadata, sin el link) y devolvía éxito silenciosamente.

**Solución:**
1. Configurar `MAIL_HOST`, `MAIL_PORT`, `MAIL_USERNAME`, `MAIL_PASSWORD`, `MAIL_FROM` en el `.env` de producción.
2. El código ahora falla visiblemente (excepción) si SMTP no está configurado, en vez de silenciosamente.
3. Si usás Resend, verificá el dominio en Resend y usá `onboarding@resend.dev` para pruebas.

## 4. Rutas profundas devuelven 404

**Síntoma:** Entrar directo a `/propiedades`, `/login`, etc. devuelve 404.

**Causa:** El servidor no tiene fallback a `index.html` para rutas de SPA.

**Solución:** El `.htaccess` en `/www/` incluye:
```apache
RewriteCond %{REQUEST_FILENAME} -f [OR]
RewriteCond %{REQUEST_FILENAME} -d
RewriteRule ^ - [L]
RewriteRule ^ index.html [L]
```

## 5. Rate limiting bloquea requests

**Síntoma:** Respuestas 429 (Too Many Requests) en desarrollo.

**Causa:** El backend tiene rate limiting (120 GET/min, 30 write/min, 5 login/min). En desarrollo, múltiples requests rápidos (HMR, pruebas) pueden exceder el límite.

**Solución:** El storage del rate limit está en `storage/` o temp. Limpiar la carpeta `alquiler-backend-rate-limit` en temp para resetear. O configurar `RATE_LIMIT_STORAGE_PATH` en el `.env`.

## 6. Desborde horizontal en móvil

**Síntoma:** Scroll horizontal no deseado en pantallas pequeñas.

**Causa común:** Grids con `1fr` (que es `minmax(auto, 1fr)`) donde el contenido fuerza un ancho mínimo.

**Solución:** Usar `minmax(0, 1fr)` en los grids y `min-width: 0` en los contenedores flex.

## 7. Servidor de desarrollo no inicia (puerto en uso)

**Síntoma:** `Port 3000 is in use, trying another one...`

**Solución:** Matar los procesos node anteriores o usar el puerto alternativo que Vite asigna automáticamente.

## 8. Base de datos no conecta

**Síntoma:** `SQLSTATE[HY000] [2002] No se puede establecer una conexión`.

**Solución:**
- Verificar que MySQL esté corriendo (XAMPP → MySQL → Start).
- Verificar `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, `DB_PASSWORD` en `.env`.
- En desarrollo, la base es `sistema_alquiler_db_dev`.

## 9. php://input vacío (servidor embebido de PHP)

**Síntoma:** `El JSON enviado no es válido` en desarrollo con `php -S`.

**Causa:** El servidor embebido de PHP consume `php://input` en el router script.

**Solución:** Usar `public/router-dev.php` como router (captura el body antes de que se consuma) y agregar el fallback en `Request::fromGlobals()`. O usar Apache/XAMPP que no tiene este problema.

## 10. Tests fallan por SameSite

**Síntoma:** `Failed asserting that 'Lax' is identical to 'None'` en `AutenticadorControllerTest`.

**Causa:** Pre-existente, no relacionado con cambios de código. El test espera `'Lax'` pero el código devuelve `'None'` (porque APP_ENV es 'production' en el test).

**Solución:** Ajustar el test o la configuración de APP_ENV en el entorno de testing.
