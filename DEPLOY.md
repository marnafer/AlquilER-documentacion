# Guía de Despliegue — AlquilER

## Arquitectura de producción

| Componente | URL | Servidor |
|---|---|---|
| Frontend Web | https://alquilerr.alwaysdata.net | alwaysdata (cuenta alquilerr) |
| Backend API | https://alquiler.alwaysdata.net | alwaysdata (cuenta alquiler) |
| Base de datos | MySQL en alwaysdata | alwaysdata |

---

## Frontend (React + Vite)

### Build
```bash
cd sistema-alquiler-frontend-web
npm install
npm run build
```
El resultado se genera en `dist/`.

### Deploy por FTP
1. Conectar por FTPS a `ftp-alquilerr.alwaysdata.net` (puerto 21).
   - Usuario: `alquilerr`
   - Contraseña: (ver gestor de credenciales)
2. Subir todo el contenido de `dist/` a `/www/`.
3. Subir `dist/.htaccess` a `/www/.htaccess` (fallback SPA para rutas de React Router + charset UTF-8).
4. Limpiar assets viejos en `/www/assets/` que no estén en el nuevo build.

### Variables de entorno del frontend (.env.production)
```env
VITE_API_URL=https://alquiler.alwaysdata.net
VITE_APP_NAME=Sistema de Alquiler
```

---

## Backend (PHP)

### Despliegue

El backend se despliega en alwaysdata. El código se obtiene del repositorio `AlquilER-backend`.

#### Opción A: Deploy por Git (recomendado)
Si el servidor tiene acceso a Git:
```bash
cd /home/<usuario>/www
git pull origin dev  # o master, según la rama de producción
composer install --no-dev --optimize-autoloader
```

#### Opción B: Deploy manual por FTP/SFTP
1. Conectar por SFTP/FTP a `ftp-alquiler.alwaysdata.net` (puerto 21, TLS).
   - Usuario: `alquiler` (cuenta del backend)
   - Contraseña: (ver gestor de credenciales)
2. Subir todos los archivos del proyecto (excepto `vendor/`, `storage/`, `.env`).
3. Ejecutar `composer install --no-dev --optimize-autoloader` en el servidor (via SSH o panel de alwaysdata).
4. Configurar el `.env` de producción (ver sección de variables de entorno).

### Variables de entorno (.env en producción)

Ver `ENV.md` para la lista completa. Las críticas son:

```env
APP_ENV=production
APP_DEBUG=false
AUTH_COOKIE_SECURE=true
AUTH_COOKIE_SAMESITE=None

DB_HOST=localhost
DB_DATABASE=sistema_alquiler_db
DB_USERNAME=<usuario_mysql>
DB_PASSWORD=<password_mysql>

JWT_KEY=<clave_aleatoria_de_al_menos_32_caracteres>
JWT_EXP=3600

APP_URL=https://alquiler.alwaysdata.net
FRONTEND_URL=https://alquilerr.alwaysdata.net
CORS_ALLOWED_ORIGINS=https://alquilerr.alwaysdata.net

MAIL_HOST=<smtp.tuproveedor.com>
MAIL_PORT=587
MAIL_USERNAME=<tu_usuario>
MAIL_PASSWORD=<tu_password>
MAIL_FROM=no-responder@tudominio.com
MAIL_FROM_NAME=AlquilER
MAIL_ENCRYPTION=tls
MAIL_TO=contacto@tudominio.com
```

**Importante:** `APP_ENV=production` es necesario para que las cookies de autenticación funcionen cross-origin (frontend en `alquilerr.alwaysdata.net`, backend en `alquiler.alwaysdata.net`). Sin esto, las cookies tienen `SameSite=Lax` y no se envían en requests cross-origin.

### Correo SMTP

Si `MAIL_HOST` está vacío, el backend falla visiblemente (excepción) en vez de silenciosamente descartar el email. Configurar con un proveedor real (Resend, SendGrid, Gmail, etc.).

### Base de datos
- Importar `create_database.sql` en la base de datos de producción.
- Ejecutar migraciones en `scripts/migrations/` si la base ya existía.

### Permisos
- `storage/` debe tener permisos de escritura (755 o 775).
- `storage/logs/`, `storage/emails.log`, `storage/stress-tests/` deben ser escribibles.

### Verificación post-deploy
1. `https://alquiler.alwaysdata.net/api/propiedades/destacadas` → debe retornar 200 con JSON.
2. `https://alquilerr.alwaysdata.net/` → debe cargar el frontend.
3. Login con usuario verificado → debe establecer cookies `access_token` y `refresh_token`.
4. Verificar que las cookies se envían en requests cross-origin (DevTools → Network → Request Headers → Cookie).

---

## Notas generales

- El frontend es una SPA. Todas las rutas son manejadas por React Router en el cliente.
- El `.htaccess` en `/www/` redirige todas las rutas no-archivo a `index.html` para que React Router las resuelva.
- El charset UTF-8 se fuerza en Apache via `AddDefaultCharset utf-8` y `AddCharset utf-8` en `.htaccess`.
- PurgeCSS se ejecuta solo en build (frontend) para eliminar CSS no utilizado.
- Las imágenes del hero y logo están en `public/assets/img/` y se copian a `dist/assets/img/` en el build.
