# Sprints — AlquilER

> Planificación de sprints del proyecto AlquilER.
> Cada sprint tiene duración de 2 semanas. Incluye objetivo, tareas, criterios de aceptación y métricas.

---

## 1. Resumen de sprints

| Sprint | Nombre | Duración | Objetivo principal | Items | Estado |
|---|---|---|---|---|---|
| Sprint 1 | Producción estable | 2 semanas | Poner el sistema en producción sin errores críticos | 7 | ⏳ Pendiente |
| Sprint 2 | Seguridad y compliance | 2 semanas | Asegurar la app y cumplir con normativas | 8 | ⏳ Pendiente |
| Sprint 3 | Calidad y performance | 2 semanas | Mejorar código, tests y performance | 12 | ⏳ Pendiente |
| Sprint 4 | Features avanzadas | 2 semanas | Funcionalidades nuevas y mejoras UX | 12 | ⏳ Pendiente |
| Sprint 5 | Pulido y lanzamiento | 2 semanas | Pulir detalles y preparar lanzamiento | 13 | ⏳ Pendiente |
| **Total** | | **10 semanas** | | **52** | |

---

## 2. Sprint 1: Producción estable

**Objetivo:** Poner el sistema en producción sin errores críticos de configuración.

**Duración:** Semanas 1-2

### Tareas

| ID | Tarea | Prioridad | Estimación | Responsable | Estado |
|---|---|---|---|---|---|
| BL-001 | Configurar SMTP en producción (Resend) | 🔴 Crítico | 2h | Backend | ⏳ |
| BL-002 | Configurar APP_ENV=production | 🔴 Crítico | 1h | Backend | ⏳ |
| BL-006 | Configurar backups automáticos de BD en alwaysdata | 🟠 Alto | 3h | DevOps | ⏳ |
| BL-007 | Configurar monitoreo de uptime (UptimeRobot) | 🟠 Alto | 1h | DevOps | ⏳ |
| BUG-001 | Verificar fix de mojibake en producción | 🟠 Alto | 1h | Frontend | ✅ |
| BUG-002 | Verificar fix de emails silenciosos | 🔴 Crítico | 1h | Backend | ✅ |
| BUG-003 | Verificar fix de dark mode | 🟠 Alto | 1h | Frontend | ✅ |
| BUG-004 | Verificar fix de SPA deep-link | 🟠 Alto | 1h | Frontend | ✅ |

### Criterios de aceptación

- [ ] Los emails de verificación se envían correctamente en producción.
- [ ] Las cookies funcionan cross-origin (login persiste).
- [ ] Los backups diarios están configurados y verificados.
- [ ] El monitoreo de uptime está activo y envía alertas.
- [ ] No hay errores de encoding (mojibake) en producción.
- [ ] Las rutas directas (deep links) funcionan sin 404.

### Métricas de éxito

- 0 errores críticos en producción.
- 100% de emails de verificación entregados.
- Tiempo de respuesta p95 < 500ms.

---

## 3. Sprint 2: Seguridad y compliance

**Objetivo:** Asegurar la aplicación y cumplir con normativas de protección de datos.

**Duración:** Semanas 3-4

### Tareas

| ID | Tarea | Prioridad | Estimación | Responsable | Estado |
|---|---|---|---|---|---|
| BL-003 | Validación de complejidad de contraseña | 🟠 Alto | 4h | Backend | ⏳ |
| BL-004 | Política de privacidad + consentimiento | 🟠 Alto | 8h | Frontend + Backend | ⏳ |
| BL-005 | Derecho al olvido (eliminación de cuenta) | 🟠 Alto | 8h | Backend | ⏳ |
| BUG-005 | Login feedback (email no verificado) | 🟡 Medio | 2h | Frontend | ⏳ |
| BUG-006 | Rate limiting en redes compartidas (NAT) | 🟡 Medio | 4h | Backend | ⏳ |
| UX-005 | Indicador de fuerza de contraseña | 🟢 Bajo | 3h | Frontend | ⏳ |
| RS-009 | Revisar fuerza de contraseña en backend | 🟠 Alto | 2h | Backend | ⏳ |
| RS-010 | Anonimización de datos eliminados | 🟠 Alto | 4h | Backend | ⏳ |

### Criterios de aceptación

- [ ] Las contraseñas requieren mínimo 8 caracteres con mayúscula y número.
- [ ] Existe una página de política de privacidad accesible.
- [ ] El registro incluye checkbox de consentimiento.
- [ ] El usuario puede solicitar eliminación de su cuenta.
- [ ] El login muestra mensaje claro si el email no está verificado.
- [ ] El rate limiting no bloquea usuarios legítimos en redes compartidas.

### Métricas de éxito

- 0 vulnerabilidades críticas en auditoría.
- 100% de usuarios con consentimiento registrado.

---

## 4. Sprint 3: Calidad y performance

**Objetivo:** Mejorar la calidad del código, agregar tests y optimizar performance.

**Duración:** Semanas 5-6

### Tareas

| ID | Tarea | Prioridad | Estimación | Responsable | Estado |
|---|---|---|---|---|---|
| BL-010 | Code splitting / lazy loading (React.lazy) | 🟡 Medio | 6h | Frontend | ⏳ |
| BL-011 | CI/CD con GitHub Actions | 🟡 Medio | 8h | DevOps | ⏳ |
| BL-012 | Rotación de logs (emails, errores, debug) | 🟡 Medio | 3h | Backend | ⏳ |
| DT-001 | Revisar queries N+1 (eager loading) | 🟡 Medio | 6h | Backend | ⏳ |
| DT-002 | Cobertura de tests ≥ 70% | 🟡 Medio | 16h | Backend | ⏳ |
| DT-003 | Validación de entrada consistente | 🟡 Medio | 6h | Backend | ⏳ |
| DT-004 | Manejo de errores inconsistente | 🟡 Medio | 4h | Backend | ⏳ |
| TEST-001 | Tests E2E: flujo de login | 🟠 Alto | 4h | QA | ⏳ |
| TEST-002 | Tests E2E: flujo de reserva | 🟠 Alto | 6h | QA | ⏳ |
| TEST-003 | Tests E2E: publicación de propiedad | 🟡 Medio | 6h | QA | ⏳ |
| DOC-001 | Actualizar OpenAPI spec | 🟡 Medio | 4h | Backend | ⏳ |
| DOC-004 | Documentar endpoints de admin | 🟡 Medio | 3h | Backend | ⏳ |
| UX-001 | Skeletons en carga (catálogo, detalle) | 🟡 Medio | 4h | Frontend | ⏳ |
| UX-002 | Toast de éxito/error consistente | 🟡 Medio | 3h | Frontend | ⏳ |

### Criterios de aceptación

- [ ] El bundle principal se reduce a < 400KB.
- [ ] CI/CD ejecuta tests en cada PR.
- [ ] Cobertura de tests ≥ 70% en servicios.
- [ ] No hay queries N+1 en listados principales.
- [ ] Todos los endpoints devuelven códigos HTTP correctos.
- [ ] Los tests E2E pasan para login, reserva y publicación.

### Métricas de éxito

- Bundle size < 400KB (gzip).
- Cobertura de tests ≥ 70%.
- 0 queries N+1 en endpoints principales.
- Tiempo de carga < 2s en 3G.

---

## 5. Sprint 4: Features avanzadas

**Objetivo:** Implementar funcionalidades nuevas y mejorar la experiencia de usuario.

**Duración:** Semanas 7-8

### Tareas

| ID | Tarea | Prioridad | Estimación | Responsable | Estado |
|---|---|---|---|---|---|
| BL-008 | Internacionalización (i18n) | 🟡 Medio | 16h | Frontend | ⏳ |
| BL-009 | Accesibilidad (a11y) WCAG 2.1 AA | 🟡 Medio | 12h | Frontend | ⏳ |
| BL-013 | Feature flags | 🟡 Medio | 6h | Backend + Frontend | ⏳ |
| BL-014 | Health check endpoint | 🟡 Medio | 2h | Backend | ⏳ |
| BL-015 | Alertas de errores críticos | 🟡 Medio | 4h | Backend | ⏳ |
| BL-016 | Almacenamiento externo (S3/Cloudinary) | 🟡 Medio | 8h | Backend | ⏳ |
| BL-017 | Moderación de reseñas | 🟡 Medio | 6h | Backend + Frontend | ⏳ |
| BL-021 | Procesamiento de imágenes (WebP) | 🟢 Bajo | 4h | Backend | ⏳ |
| UX-003 | Empty states con acciones | 🟢 Bajo | 3h | Frontend | ⏳ |
| UX-004 | Confirmación de acciones destructivas | 🟢 Bajo | 3h | Frontend | ⏳ |
| UX-006 | Paginación infinita (opcional) | 🟢 Bajo | 6h | Frontend | ⏳ |
| BL-020 | Carrusel de propiedades vistas recientemente | 🟢 Bajo | 4h | Frontend | ⏳ |

### Criterios de aceptación

- [ ] El sistema soporta al menos 2 idiomas (español + inglés).
- [ ] La interfaz cumple WCAG 2.1 AA (contraste, teclado, ARIA).
- [ ] Las imágenes se almacenan en servicio externo.
- [ ] El health check endpoint responde correctamente.
- [ ] Las alertas de error crítico funcionan.

### Métricas de éxito

- Lighthouse accessibility score ≥ 90.
- Lighthouse performance score ≥ 85.
- 0 imágenes servidas desde storage local en producción.

---

## 6. Sprint 5: Pulido y lanzamiento

**Objetivo:** Pulir detalles finales y preparar el lanzamiento oficial.

**Duración:** Semanas 9-10

### Tareas

| ID | Tarea | Prioridad | Estimación | Responsable | Estado |
|---|---|---|---|---|---|
| BL-019 | SEO por página (title, meta, OG) | 🟢 Bajo | 6h | Frontend | ⏳ |
| BL-018 | Verificar prefers-reduced-motion | 🟢 Bajo | 2h | Frontend | ✅ |
| UX-005 | Indicador de fuerza de contraseña | 🟢 Bajo | 3h | Frontend | ⏳ |
| DT-005 | Actualizar OpenAPI spec | 🟢 Bajo | 4h | Backend | ⏳ |
| DT-006 | Auditar dependencias (npm + composer) | 🟢 Bajo | 2h | DevOps | ⏳ |
| DOC-002 | Actualizar README-BACKEND | 🟢 Bajo | 2h | Backend | ⏳ |
| DOC-003 | Actualizar README-FRONTEND | 🟢 Bajo | 2h | Frontend | ⏳ |
| DOC-005 | Changelog actualizado | 🟢 Bajo | 1h | Todos | ⏳ |
| TEST-004 | Tests unitarios: AuthService | 🟡 Medio | 4h | Backend | ⏳ |
| TEST-005 | Tests unitarios: ReservaService | 🟡 Medio | 6h | Backend | ⏳ |
| TEST-006 | Tests unitarios: PropiedadService | 🟡 Medio | 6h | Backend | ⏳ |
| TEST-007 | Tests de integración: API endpoints | 🟡 Medio | 8h | Backend | ⏳ |
| TEST-008 | Cobertura ≥ 70% (verificación final) | 🟡 Medio | 2h | Backend | ⏳ |

### Criterios de aceptación

- [ ] Lighthouse SEO score ≥ 90.
- [ ] Todas las dependencias están actualizadas y sin vulnerabilidades críticas.
- [ ] La documentación está completa y actualizada.
- [ ] Cobertura de tests ≥ 70% verificada.
- [ ] El sistema está listo para lanzamiento público.

### Métricas de éxito

- Lighthouse: Performance ≥ 85, Accessibility ≥ 90, SEO ≥ 90, Best Practices ≥ 90.
- 0 vulnerabilidades críticas en dependencias.
- Cobertura de tests ≥ 70%.

---

## 7. Ceremonias por sprint

| Ceremonia | Frecuencia | Duración | Participantes |
|---|---|---|---|
| Sprint Planning | Inicio de sprint | 2h | Todo el equipo |
| Daily Standup | Diario | 15 min | Todo el equipo |
| Sprint Review | Fin de sprint | 1h | Todo el equipo + stakeholders |
| Sprint Retrospective | Fin de sprint | 1h | Todo el equipo |
| Backlog Refinement | Mid-sprint | 1h | Todo el equipo |

---

## 8. Definición de "Done"

Una tarea se considera completada cuando:

- [ ] El código está escrito y funcional.
- [ ] Los tests unitarios/integración pasan.
- [ ] El código fue revisado (code review).
- [ ] La documentación está actualizada.
- [ ] No hay regresiones en funcionalidad existente.
- [ ] Se verificó en entorno de desarrollo.
- [ ] Se desplegó en producción (si aplica).

---

## 9. Riesgos por sprint

| Sprint | Riesgo principal | Mitigación |
|---|---|---|
| Sprint 1 | SMTP no disponible en producción | Tener fallback a log local, configurar antes del deploy |
| Sprint 2 | Cambios de schema en BD | Migraciones incrementales, backups antes de cada cambio |
| Sprint 3 | Tests E2E inestables (flaky) | Usar selectores robustos, reintentos, datos de prueba fijos |
| Sprint 4 | i18n rompe layouts | Probar con textos largos, usar CSS flexible |
| Sprint 5 | Scope creep (agregar más features) | Congelar scope, mover items nuevos al backlog |

---

## 10. Notas

- Los sprints son de 2 semanas (10 días hábiles).
- La velocidad estimada es de ~40 story points por sprint.
- Los items pueden moverse entre sprints según prioridad.
- Los sprints 1 y 2 son críticos para la estabilidad en producción.
- Los sprints 3-5 son de mejora continua.
- Este plan es flexible y se ajusta según feedback.
