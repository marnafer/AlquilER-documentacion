# Changelog

Todos los cambios notables del proyecto AlquilER se documentan en este archivo.

El formato está basado en [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/),
y este proyecto adhiere a [Semantic Versioning](https://semver.org/lang/es/).

## [Unreleased]

### Added
- Backend: cobertura de tests al 100% (2.300 tests, 7.115 assertions).
- Backend: validación de complejidad de contraseña (mín. 8 chars, 1 mayúscula, 1 número).
- Backend: generación de versión WebP de imágenes procesadas.
- Backend: rotación de logs (5MB, 3 gen) en errores, debug y emails.
- Frontend: consentimiento explícito de política de privacidad en el registro.
- Frontend: SEO (title, description, OG tags) en todas las páginas, incluyendo admin con noindex.
- Documentación: actualización de OpenAPI spec, READMEs y API-CONTRACT (endpoints admin).
- Repositorio de documentación con guías de despliegue, variables de entorno, esquema de BD y troubleshooting.

## [1.0.0] - 2026-11-11

### Added
- **Frontend Web:** Modo oscuro/claro con toggle y persistencia en localStorage.
- **Frontend Web:** Mejoras de responsive (filtros de catálogo, categorías en grid, stats 2×2, barra de búsqueda apilada, touch targets 44px, carrusel 2 por fila en tablet).
- **Frontend Web:** `.htaccess` con fallback SPA para rutas de React Router (deep links ya no devuelven 404).
- **Frontend Web:** Forzado de charset UTF-8 en Apache (`AddDefaultCharset`, `AddCharset`) para evitar mojibake en acentos y ñ.
- **Frontend Web:** Mensaje mejorado y visible para error "Email no verificado" en login (scroll automático, ícono, role="alert").
- **Backend:** Falla visible (excepción) cuando SMTP no está configurado en producción, en vez de descartar emails silenciosamente.
- **Backend:** `enviar()` devuelve `Fallido` en producción sin SMTP configurado.
- **Backend:** Verificación de retorno de `enviar()` en `AutenticadorService`, `ContactoService` y `RecuperarContrasenaService`.
- **Backend:** `guardarEnLog()` persiste metadata en producción (sin MIME completo con tokens, por seguridad).
- **Documentación:** Repositorio de documentación con API-CONTRACT, OpenAPI, ERD, DEPLOY, ENV, TROUBLESHOOTING, schema.sql.

### Fixed
- **Frontend:** Fondo blanco del `body` en modo oscuro en el build de producción (Bootstrap sobreescribía el fondo oscuro por orden de CSS).
- **Frontend:** Desborde horizontal de filtros del catálogo entre 769px y 1200px.
- **Frontend:** Desborde de acciones del hero de admin en móvil.
- **Backend:** `php://input` no se leía en el servidor embebido de PHP (dev).

### Changed
- **Backend:** `MAIL_FROM` por defecto sigue siendo `no-responder@alquiler.local` (configurar email real en producción).
- **Backend:** `AUTH_COOKIE_SECURE` y `AUTH_COOKIE_SAMESITE` se leen de `APP_ENV` (no de las variables de entorno directamente).

## [0.9.0] - 2026-11-05

### Added
- **Frontend:** Migración a Vite 5.
- **Frontend:** Autenticación con cookies HttpOnly.
- **Frontend:** Contextos de tema (claro/oscuro) y UI.
- **Frontend:** Páginas nuevas: MisResenas, PerfilPublico, ReservaDetalle, VerificarEmail.
- **Frontend:** Hooks: useSEO, useScrollReveal.
- **Frontend:** Estilos separados por sección (19 archivos CSS).
- **Frontend:** Utilidades: categorias, imagenes, profanity, vistosRecientemente.

### Removed
- **Frontend:** Código legacy (`frontend/` y `legacy-php/`).

## [0.1.0] - 2026-08-12

### Added
- Repositorios iniciales del proyecto AlquilER (backend, frontend web, frontend mobile).
- Estructura base de la API REST en PHP.
- Base de datos con tablas principales.
- Autenticación JWT.
- CRUD de propiedades, usuarios, reservas, consultas, reseñas, favoritos.
- Panel de administración.
- Formulario de contacto.

---

[Unreleased]: https://github.com/JulianMP1997/AlquilER-documentacion/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/JulianMP1997/AlquilER-documentacion/releases/tag/v1.0.0
[0.9.0]: https://github.com/marnafer/AlquilER-frontend-web/compare/v0.1.0...v0.9.0
[0.1.0]: https://github.com/marnafer/AlquilER-backend/releases/tag/v0.1.0
