# Backlog — AlquilER

> Backlog priorizado de tareas, mejoras y deuda técnica del proyecto AlquilER.
> Formato: `[ID] Título — Descripción — Prioridad — Estado`

---

## 1. Leyenda

| Símbolo | Significado |
|---|---|
| 🔴 | Crítico (bloquea producción) |
| 🟠 | Alto (importante, próximo sprint) |
| 🟡 | Medio (deseable) |
| 🟢 | Bajo (nice to have) |
| ✅ | Completado |
| 🔄 | En progreso |
| ⏳ | Pendiente |
| 🐛 | Bug |
| ✨ | Feature nueva |
| 🔧 | Mejora / refactor |
| 📋 | Documentación |

---

## 2. Funcionalidades pendientes

### 2.1 Alta prioridad

| ID | Tipo | Título | Descripción | Prioridad | Estado |
|---|---|---|---|---|---|
| BL-001 | ✨ | Configurar SMTP en producción | Sin SMTP, los emails de verificación y reset de contraseña no se envían. Configurar MAIL_HOST, MAIL_PORT, MAIL_USERNAME, MAIL_PASSWORD en .env de producción. | 🔴 | ✅ |
| BL-002 | ✨ | Configurar APP_ENV=production | Sin esto, las cookies no usan Secure=true ni SameSite=None, rompiendo la auth cross-origin. | 🔴 | ✅ |
| BL-003 | ✨ | Validación de complejidad de contraseña | Agregar validación en UsuarioValidator: mínimo 8 caracteres, 1 mayúscula, 1 número. | 🟠 | ✅ |
| BL-004 | ✨ | Política de privacidad | Página de política y consentimiento explícito en el registro. | 🟠 | ✅ |
| BL-005 | ✨ | Derecho al olvido | Endpoint para que el usuario solicite eliminación de su cuenta y datos. | 🟠 | ⏳ |

### 2.2 Media prioridad

| ID | Tipo | Título | Descripción | Prioridad | Estado |
|---|---|---|---|---|---|
| BL-006 | ✨ | Code splitting / lazy loading | Reducir bundle principal (~749KB) con React.lazy() y Suspense. | 🟡 | ✅ |
| BL-007 | ✨ | CI/CD con GitHub Actions | Automatizar tests y deploy. Actualmente deploy manual por FTP. | 🟡 | ✅ |
| BL-008 | ✨ | Rotación de logs | Extender rotación (5MB, 3 gen) a emails.log, errores.log, debug.log. | 🟡 | 🔄 |
| BL-009 | ✨ | Feature flags | Sistema para habilitar/deshabilitar funcionalidades sin deploy. | 🟡 | ⏳ |
| BL-010 | ✨ | Health check endpoint | GET /api/health que verifique BD, storage y servicios. | 🟡 | ⏳ |
| BL-011 | ✨ | Alertas de errores críticos | Email o webhook cuando ocurre un error 500. | 🟡 | ⏳ |
| BL-012 | ✨ | Almacenamiento externo (S3/Cloudinary) | Mover imágenes de storage/uploads a almacenamiento externo. | 🟡 | ⏳ |
| BL-013 | ✨ | Moderación de reseñas | Opción de moderación previa antes de publicar reseñas. | 🟡 | ⏳ |

### 2.3 Baja prioridad

| ID | Tipo | Título | Descripción | Prioridad | Estado |
|---|---|---|---|---|---|
| BL-014 | ✨ | Animaciones de scroll reveal | Ya implementadas, verificar prefers-reduced-motion. | 🟢 | ✅ |
| BL-015 | ✨ | SEO por página | Title, meta description, OG tags. Implementado con useSEO en todas las páginas (incluye admin con noindex). | 🟢 | ✅ |
| BL-016 | ✨ | Carrusel de propiedades vistas recientemente | Mostrar en home las últimas vistas. | 🟢 | ✅ |
| BL-017 | ✨ | Procesamiento de imágenes (WebP) | Generar versión WebP además de JPG/PNG. | 🟢 | ✅ |

---

## 3. Bugs conocidos

| ID | Título | Descripción | Prioridad | Estado |
|---|---|---|---|---|
| BUG-001 | Mojibake en producción | Caracteres rotos por falta de charset. Fix: `AddDefaultCharset utf-8` en `.htaccess`. | 🟠 | ✅ |
| BUG-002 | Emails silenciosos en producción | SMTP no configurado → emails descartados sin error. Fix: `MailService::enviar()` lanza excepción. | 🔴 | ✅ |
| BUG-003 | Dark mode: secciones blancas | Body background no cambiaba en dark mode. Fix: `:root[data-theme="dark"] body`. | 🟠 | ✅ |
| BUG-004 | SPA deep-link 404 | Rutas directas (ej: `/propiedad/1`) daban 404. Fix: `.htaccess` con fallback a `index.html`. | 🟠 | ✅ |
| BUG-005 | Login bloqueado sin feedback | Si el email no está verificado, el login falla sin mensaje claro. | 🟡 | ⏳ |
| BUG-006 | Rate limiting en NAT | Usuarios en redes compartidas pueden ser bloqueados. | 🟡 | ⏳ |

---

## 4. Deuda técnica

| ID | Título | Descripción | Prioridad | Estado |
|---|---|---|---|---|
| DT-001 | Queries N+1 | Algunos listados no usan eager loading (`with()`). Revisar repositorios. | 🟡 | ⏳ |
| DT-002 | Cobertura de tests | Tests unitarios al 100% (backend AlquilER). | 🟡 | ✅ |
| DT-003 | Validación de entrada inconsistente | Algunos controllers no validan antes de llamar al servicio. | 🟡 | ⏳ |
| DT-004 | Manejo de errores inconsistente | Algunos endpoints devuelven 500 en lugar de 422/400. | 🟡 | ⏳ |
| DT-005 | Sin documentación de API actualizada | OpenAPI spec puede estar desactualizada respecto al código. | 🟢 | ⏳ |
| DT-006 | Dependencias desactualizadas | Ejecutar `npm audit` y `composer audit` periódicamente. | 🟢 | ⏳ |

---

## 5. Testing

| ID | Tipo | Título | Descripción | Prioridad | Estado |
|---|---|---|---|---|---|
| TEST-001 | 🧪 | Tests E2E: flujo de login | Playwright: registro → verificación → login → logout. | 🟠 | ⏳ |
| TEST-002 | 🧪 | Tests E2E: flujo de reserva | Playwright: buscar → reservar → confirmar → finalizar → reseñar. | 🟠 | ⏳ |
| TEST-003 | 🧪 | Tests E2E: publicación de propiedad | Playwright: crear propiedad → subir imágenes → publicar → editar. | 🟡 | ⏳ |
| TEST-004 | 🧪 | Tests unitarios: AuthService | Cubrir login, refresh, logout, verificación de email. | 🟡 | ✅ |
| TEST-005 | 🧪 | Tests unitarios: ReservaService | Cobertura al 100%. | 🟡 | ✅ |
| TEST-006 | 🧪 | Tests unitarios: PropiedadService | Cobertura al 100%. | 🟡 | ✅ |
| TEST-007 | 🧪 | Tests de integración: API endpoints | Verificar request/response de todos los endpoints. | 🟡 | ⏳ |
| TEST-008 | 🧪 | Cobertura ≥ 70% | Cobertura al 100% en backend AlquilER. | 🟡 | ✅ |

---

## 6. Documentación

| ID | Tipo | Título | Descripción | Prioridad | Estado |
|---|---|---|---|---|---|
| DOC-001 | 📋 | Actualizar OpenAPI spec | Sincronizar openapi.json con los endpoints actuales del código. | 🟡 | ⏳ |
| DOC-002 | 📋 | Actualizar README-BACKEND | Incluir instrucciones de migraciones y seeders. | 🟢 | ⏳ |
| DOC-003 | 📋 | Actualizar README-FRONTEND | Incluir instrucciones de variables de entorno y build. | 🟢 | ⏳ |
| DOC-004 | 📋 | Documentar endpoints de admin | Agregar al API-CONTRACT los endpoints `/admin/*`. | 🟡 | ⏳ |
| DOC-005 | 📋 | Changelog actualizado | Mantener CHANGELOG.md con cada release. | 🟢 | ⏳ |

---

## 7. Mejoras de UX/UI

| ID | Título | Descripción | Prioridad | Estado |
|---|---|---|---|---|
| UX-001 | Skeletons en carga | Agregar skeletons en catálogo y detalle. | 🟡 | 🔄 |
| UX-002 | Toast de éxito/error | Feedback visual consistente en todas las acciones. | 🟡 | ✅ |
| UX-003 | Empty states con acciones | Estados vacíos con botones de acción (ej: "Ir al catálogo"). | 🟢 | ✅ |
| UX-004 | Confirmación de acciones destructivas | Modal de confirmación antes de eliminar. | 🟢 | ⏳ |
| UX-005 | Indicador de fuerza de contraseña | Mostrar barra de fuerza en registro. | 🟢 | ⏳ |
| UX-006 | Paginación infinita (opcional) | Scroll infinito en catálogo como alternativa a paginación. | 🟢 | ⏳ |

---

## 8. Sprints sugeridos

### Sprint 1: Producción estable (crítico)
- BL-001: Configurar SMTP
- BL-002: Configurar APP_ENV=production

### Sprint 2: Seguridad y compliance
- BL-003: Validación de contraseña
- BL-004: Política de privacidad
- BL-005: Derecho al olvido
- BUG-005: Login feedback
- BUG-006: Rate limiting NAT

### Sprint 3: Calidad y performance
- BL-010: Code splitting
- BL-011: CI/CD
- BL-012: Rotación de logs
- DT-001: Queries N+1
- DT-002: Cobertura de tests
- TEST-001 a TEST-008: Tests E2E y unitarios
- DOC-001: Actualizar OpenAPI spec
- DOC-004: Documentar endpoints de admin
- UX-001: Skeletons
- UX-002: Toast

### Sprint 4: Features avanzadas
- BL-008: i18n
- BL-009: a11y
- BL-013: Feature flags
- BL-014: Health check
- BL-015: Alertas
- BL-016: Almacenamiento externo
- BL-017: Moderación de reseñas

---

## 9. Métricas de progreso

| Categoría | Total | Completados | Pendientes |
|---|---|---|---|
| Funcionalidades | 19 | 2 | 17 |
| Bugs | 6 | 4 | 2 |
| Deuda técnica | 6 | 0 | 6 |
| Testing | 8 | 0 | 8 |
| Documentación | 5 | 0 | 5 |
| UX/UI | 6 | 2 | 4 |
| **Total** | **50** | **15** | **35** |

---

## 8. Notas

- El backlog se actualiza en cada sprint planning.
- Los IDs son secuenciales y no se reutilizan.
- Las prioridades pueden cambiar según feedback del cliente.
- Los items completados se mantienen para historial.
