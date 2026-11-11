# Guía de Despliegue — AlquilER

## Arquitectura de producción

| Componente | URL | Servidor |
|---|---|---|
| Frontend Web | https://alquilerr.alwaysdata.net | alwaysdata (cuenta alquilerr) |
| Backend API | https://alquiler.alwaysdata.net | alwaysdata (cuenta alquiler) |
| Base de datos | MySQL en alwaysdata | alwaysdata |

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
3. Subir `dist/.htaccess` a `/www/.htaccess` (fallback SPA para rutas de React Router).
4. Limpiar assets viejos en `/www/assets/` que no estén en el nuevo build.

### Variables de entorno del frontend (.env.production)
```env
VITE_API_URL=https://alquiler.alwaysdata.net
VITE_APP_NAME=Sistema de Alquiler
```

## Backend (PHP)

### Despliegue
El backend se despliega en alwaysdata. El código se obtiene del repositorio `AlquilER-backend`.

### Variables de entorno (.env en producción)
Ver `ENV.md` para la lista completa.

### Configuración crítica para producción
```env
APP_ENV=production
APP_DEBUG=false
AUTH_COOKIE_SECURE=true
AUTH_COOKIE_SAMESITE=None
```

**Importante:** `APP_ENV=production` es necesario para que las cookies de autenticación funcionen cross-origin (frontend en `alquilerr.alwaysdata.net`, backend en `alquiler.alwaysdata.net`). Sin esto, las cookies tienen `SameSite=Lax` y no se envían en requests cross-origin.

### Correo SMTP
Para que los emails de verificación y reset de contraseña funcionen:
```env
MAIL_HOST=smtp.tuproveedor.com
MAIL_PORT=587
MAIL_USERNAME=tu_usuario
MAIL_PASSWORD=tu_password
MAIL_FROM=no-responder@tudominio.com
MAIL_FROM_NAME=AlquilER
MAIL_ENCRYPTION=tls
MAIL_TO=contacto@tudominio.com
FRONTEND_URL=https://alquilerr.alwaysdata.net
```

Si `MAIL_HOST` está vacío, el backend falla visiblemente (excepción) en vez de silenciosamente descartar el email.

### Base de datos
- Importar `create_database.sql` en la base de datos de producción.
- Ejecutar migraciones en `scripts/migrations/` si la base ya existía.

### Permisos
- `storage/` debe tener permisos de escritura (755 o 775).
- `storage/logs/`, `storage/emails.log`, `storage/stress-tests/` deben ser escribibles.
