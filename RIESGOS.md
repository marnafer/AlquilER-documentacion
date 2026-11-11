# Análisis de Riesgos — AlquilER

> Identificación y mitigación de riesgos del proyecto AlquilER.

## 1. Riesgos técnicos

| ID | Riesgo | Probabilidad | Impacto | Mitigación |
|---|---|---|---|---|
| RT-01 | **Falla del servicio de email (SMTP)** — Sin SMTP configurado, los emails de verificación y reset de contraseña se descartan silenciosamente. | Alta | Alto | Código ahora falla visiblemente (excepción) si SMTP no está configurado en producción. Configurar MAIL_HOST en `.env`. |
| RT-02 | **Expiración de tokens JWT** — Access token expira en 15 min. Si el refresh token falla, el usuario queda deslogueado. | Media | Medio | Auto-refresh implementado en `autoRefresh.js`. Refresh token válido 15 días. |
| RT-03 | **Rate limiting bloquea usuarios legítimos** — Límites muy estrictos (5 login/min) pueden bloquear usuarios en redes compartidas (NAT). | Media | Medio | Límites configurables. Respuestas 429 incluyen `Retry-After`. |
| RT-04 | **Dependencia de servicios externos** — Resend, alwaysdata, etc. Si caen, el sistema pierde funcionalidad. | Baja | Alto | Fallback a log local en dev. En prod, fail-fast con error visible. |
| RT-05 | **Mojibake (encoding)** — Caracteres rotos en producción por falta de charset en headers HTTP. | Media | Medio | `AddDefaultCharset utf-8` + `AddCharset utf-8` en `.htaccess`. Archivos fuente en UTF-8. |
| RT-06 | **Cookies cross-origin** — `SameSite=None` + `Secure` requiere HTTPS. Si el backend no tiene HTTPS, las cookies no se guardan. | Media | Alto | Asegurar `APP_ENV=production` y HTTPS en producción. |
| RT-07 | **Inyección SQL** — Aunque se usa Eloquent ORM (parametrizado), consultas raw podrían ser vulnerables. | Baja | Alto | Usar siempre Eloquent/query builder. Revisar queries raw en repositorios. |
| RT-08 | **XSS (Cross-Site Scripting)** — Inputs no sanitizados en el frontend. | Media | Alto | Sanitización en backend (sanitizers). Escapado en frontend (React escapa por defecto). |
| RT-09 | **Dependencias desactualizadas** — Vulnerabilidades en paquetes npm/composer. | Media | Medio | Ejecutar `npm audit` y `composer audit` periódicamente. |
| RT-10 | **Performance de consultas** — Queries N+1 en listados de propiedades con imágenes y servicios. | Media | Medio | Eloquent eager loading (`with()`). Cache de catálogos (FileCache). Instrumentación de performance. |
| RT-11 | **Tamaño del bundle** — El bundle principal es ~749KB (JS). Puede ser lento en conexiones lentas. | Media | Medio | Code splitting (lazy loading de rutas). Considerar manualChunks en Vite. |
| RT-12 | **Almacenamiento de archivos** — Las imágenes se guardan en `storage/uploads`. Si el disco se llena, falla la subida. | Baja | Medio | Monitorear espacio en disco. Considerar almacenamiento externo (S3, Cloudinary). |

## 2. Riesgos operacionales

| ID | Riesgo | Probabilidad | Impacto | Mitigación |
|---|---|---|---|---|
| RO-01 | **Pérdida de datos** — Sin backups automáticos de la base de datos. | Media | Alto | Configurar backups automáticos en alwaysdata. Ejecutar `mysqldump` periódicamente. |
| RO-02 | **Downtime del servidor** — alwaysdata puede tener caídas. | Baja | Alto | Monitorear con herramientas externas (UptimeRobot, etc.). |
| RO-03 | **Deploy manual** — El deploy es manual (FTP). Puede haber errores humanos. | Alta | Medio | Documentar el proceso de deploy. Considerar CI/CD (GitHub Actions). |
| RO-04 | **Configuración de entorno** — Un `.env` mal configurado puede romper la app en producción. | Media | Alto | Validación de variables requeridas en `config.php`. Fallar visiblemente (no silenciosamente). |
| RO-05 | **Migraciones de base de datos** — Si se olvida ejecutar una migración, la app falla. | Media | Alto | Documentar migraciones pendientes. Ejecutar `scripts/migrations/` en deploy. |
| RO-06 | **Rate limit storage** — Si el directorio de rate limit no es escribible, falla. | Baja | Medio | Usar directorio temporal del sistema como fallback. |
| RO-07 | **Logs crecen sin límite** — `storage/emails.log`, `storage/errores.log`, `debug.log` pueden crecer indefinidamente. | Media | Bajo | Rotación de logs (ya implementada para performance.jsonl: 5MB, 3 generaciones). Extender a otros logs. |
| RO-08 | **Bus factor** — El proyecto depende de 2 desarrolladores. Si uno no está disponible, el mantenimiento se ralentiza. | Media | Medio | Documentación completa (este repo). Código con comentarios. Tests automatizados. |

## 3. Riesgos de seguridad

| ID | Riesgo | Probabilidad | Impacto | Mitigación |
|---|---|---|---|---|
| RS-01 | **Tokens JWT robados** — Si un atacante obtiene un access token, tiene acceso por 15 min. | Media | Alto | Tokens de corta duración (15 min). Refresh token en cookie HttpOnly con path restringido. |
| RS-02 | **CSRF (Cross-Site Request Forgery)** — Un sitio malicioso podría enviar requests con las cookies del usuario. | Media | Alto | `CookieCsrfMiddleware` valida el `Origin` en operaciones que modifican datos. |
| RS-03 | **XSS (Cross-Site Scripting)** — Inyección de scripts en el frontend. | Baja | Alto | React escapa automáticamente. Sanitización en backend. `X-Content-Type-Options: nosniff`. |
| RS-04 | **Brute force en login** — Intentos masivos de login. | Media | Medio | Rate limiting: 5 intentos/min por IP. |
| RS-05 | **Exposición de datos sensibles** — Tokens, contraseñas en logs. | Baja | Alto | En producción, `guardarEnLog()` no persiste el MIME completo (con tokens). Logs de performance no incluyen credenciales. |
| RS-06 | **CORS mal configurado** — Si `CORS_ALLOWED_ORIGINS` es `*`, cualquier sitio puede hacer requests. | Baja | Alto | Configurar orígenes específicos en producción. |
| RS-07 | **SQL Injection** — Aunque se usa Eloquent (parametrizado), queries raw podrían ser vulnerables. | Baja | Alto | Revisar queries raw en repositorios. Usar siempre query builder. |
| RS-08 | **Secrets en el repositorio** — `.env` con credenciales commiteado por error. | Baja | Alto | `.env` está en `.gitignore`. Verificar con `git status` antes de commit. |
| RS-09 | **Fuerza de contraseña** — No se valida complejidad mínima (mayúsculas, números, símbolos) en el registro. | Media | Medio | Agregar validación de complejidad en `UsuarioValidator`. Requerir mínimo 8 caracteres con al menos 1 mayúscula, 1 número. |
| RS-10 | **Cumplimiento legal (GDPR/protección de datos)** — El sistema almacena datos personales (nombre, email, teléfono, domicilio) sin política de privacidad explícita ni mecanismo de eliminación de cuenta. | Baja | Alto | Implementar política de privacidad, opción de eliminación de cuenta (derecho al olvido), y consentimiento explícito. |

## 4. Riesgos de negocio

| ID | Riesgo | Probabilidad | Impacto | Mitigación |
|---|---|---|---|---|
| RN-01 | **Falta de verificación de email** — Usuarios que no verifican su email no pueden usar el sistema. | Alta | Medio | Mensaje claro en el frontend. Código ahora falla visiblemente si SMTP no está configurado. |
| RN-02 | **Spam/abuso del formulario de contacto** — Sin rate limiting específico, puede ser abusado. | Media | Bajo | Rate limiting: 10 contacto/min. Validación de campos. |
| RN-03 | **Contenido inapropiado** — Usuarios publican propiedades con contenido ofensivo. | Baja | Medio | Filtro de profanidad (`profanity.js`). Moderación manual por admin. |
| RN-04 | **Dependencia de un solo proveedor** — alwaysdata para hosting, Resend para emails. | Baja | Alto | Considerar proveedores alternativos. Backups regulares. |
| RN-05 | **Escalabilidad de la base de datos** — Sin caché de consultas, la BD puede ser cuello de botella. | Media | Medio | FileCache para catálogos de baja rotación. Instrumentación de performance. |
| RN-06 | **Falta de moderación de reseñas** — Reseñas falsas o maliciosas. | Baja | Medio | Admin puede eliminar reseñas. Considerar moderación previa. |

## 5. Matriz de riesgos (probabilidad vs impacto)

```
                    Impacto
                    Bajo    Medio    Alto
Probabilidad  Alta  |  RN-02  | RT-03   | RT-01  |
              Media | RO-07   | RT-05   | RT-04  |
                    |         | RT-08   | RT-07  |
                    |         | RT-10   | RT-12  |
                    |         | RT-11   | RO-01  |
                    |         | RT-09   | RO-03  |
                    |         | RS-04   | RO-04  |
                    |         | RN-05   | RS-01  |
                    |         | RN-03   | RS-02  |
                    |         | RN-06   | RS-06  |
                    |         |         | RS-07  |
                    |         |         | RS-08  |
              Baja  |         | RT-02   | RT-04  |
                    |         | RT-06   | RT-07  |
                    |         | RT-09   | RT-12  |
                    |         | RN-03   | RO-01  |
                    |         | RN-06   | RO-04  |
                    |         |         | RS-01  |
                    |         |         | RS-02  |
                    |         |         | RS-06  |
                    |         |         | RS-07  |
                    |         |         | RS-08  |
```

## 6. Plan de mitigación prioritario

1. **Configurar SMTP en producción** (RT-01, RN-01) — Crítico. Sin esto, los usuarios no pueden verificar su email.
2. **Configurar APP_ENV=production** (RT-06) — Crítico. Sin esto, las cookies no funcionan cross-origin.
3. **Configurar backups automáticos** (RO-01) — Alto. Previene pérdida de datos.
4. **Implementar CI/CD** (RO-03) — Medio. Reduce errores en deploy manual.
5. **Monitoreo de uptime** (RO-02) — Medio. Detecta caídas del servidor.
6. **Auditar dependencias** (RT-09) — Medio. Detecta vulnerabilidades.
7. **Rotación de logs** (RO-07) — Bajo. Previene crecimiento indefinido.
