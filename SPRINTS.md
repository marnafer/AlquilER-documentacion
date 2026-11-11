# Sprints — AlquilER

> Planificación de sprints del proyecto AlquilER.
> Sprints S0–S3: completados (datos reales del dashboard al 25/10/2026).
> Sprints S4+: planificados para el próximo ciclo (backlog pendiente).

---

## 1. Resumen de sprints

| Sprint | Nombre | Fechas | Días | Commits | PRs | Estado |
|---|---|---|---|---|---|---|
| S0 | Inicio | 11/08 → 30/08 | 20 | 46 | 0 | ✅ Completado |
| S1 | Núcleo | 31/08 → 20/09 | 21 | 219 | 21 | ✅ Completado |
| S2 | Producto | 21/09 → 11/10 | 21 | 335 | 6 | ✅ Completado |
| S3 | Calidad | 12/10 → 23/10 | 12 | 162 | 4 | ⚠️ Con observaciones |
| **Total S0–S3** | | **11/08 → 23/10** | **74** | **762** | **31** | **RC** |
| S4 | Producción estable | Sem 1-2 (post 23/10) | 10 | — | — | ⏳ Planificado |
| S5 | Seguridad y compliance | Sem 3-4 | 10 | — | — | ⏳ Planificado |
| S6 | Calidad y performance | Sem 5-6 | 10 | — | — | ⏳ Planificado |
| S7 | Features avanzadas | Sem 7-8 | 10 | — | — | ⏳ Planificado |
| S8 | Pulido y lanzamiento | Sem 9-10 | 10 | — | — | ⏳ Planificado |

---

## 2. Sprints completados (S0–S3)

### Sprint S0 — Inicio (11/08 → 30/08, 20 días)

| Campo | Detalle |
|---|---|
| **Objetivo** | Contrato de API, repositorio, arquitectura base y rama `dev`. |
| **Commits** | 46 |
| **PRs** | 0 |
| **Resultado** | API-CONTRACT cerrado; backend y frontend web inicializados. |
| **Estado** | ✅ Completado |

**Actividades principales:**
- Definición del contrato de API (endpoints, request/response).
- Inicialización de los repositorios backend y frontend web.
- Configuración de la arquitectura base (estructura de carpetas, config, database).
- Creación de la rama `dev` para desarrollo.
- Setup del entorno de desarrollo local (PHP built-in server, Vite).

---

### Sprint S1 — Núcleo (31/08 → 20/09, 21 días)

| Campo | Detalle |
|---|---|
| **Objetivo** | Refactor por capas (Controller → Service → Repository), JWT, seguridad y base de datos. |
| **Commits** | 219 |
| **PRs** | 21 |
| **Resultado** | 14 módulos del backend, rate limit, tests de integridad y `create_database.sql`. |
| **Estado** | ✅ Completado |

**Actividades principales:**
- Refactor completo: separación en capas (Controllers, Services, Repositories, Models).
- Implementación de JWT (access token 15 min, refresh token 15 días).
- Cookies HttpOnly para autenticación.
- Middleware de autenticación, CSRF (Origin), CORS.
- Rate limiting (120 GET/min, 30 write/min, 5 login/min).
- 14 módulos del backend (usuarios, propiedades, reservas, consultas, reseñas, etc.).
- Tests de integridad de base de datos.
- `create_database.sql` con esquema completo (17 tablas + 1 vista).
- Validadores y sanitizers por entidad.
- Policies de autorización (ownership, roles).

---

### Sprint S2 — Producto (21/09 → 11/10, 21 días)

| Campo | Detalle |
|---|---|
| **Objetivo** | App móvil, reservas, reseñas, notificaciones, mensajería y precalificación. |
| **Commits** | 335 |
| **PRs** | 6 |
| **Resultado** | Web y mobile integrados; flujo completo de alquiler y reputación. |
| **Estado** | ✅ Completado |

**Actividades principales:**
- Desarrollo de la app móvil (React Native / Expo).
- Flujo completo de reservas (crear, confirmar, rechazar, cancelar, finalizar).
- Sistema de reseñas (propiedad e inquilino, calificación 1-5).
- Notificaciones automáticas por eventos (nueva reserva, confirmación, etc.).
- Mensajería bidireccional en consultas.
- Favoritos.
- Integración web + mobile con el backend.
- Precalificación de interesados (mascotas, hijos, etc.).
- Características destacadas de propiedades.

---

### Sprint S3 — Calidad (12/10 → 23/10, 12 días)

| Campo | Detalle |
|---|---|
| **Objetivo** | Cobertura, rate limiting, verificación de email, imágenes y hardening de despliegue. |
| **Commits** | 162 |
| **PRs** | 4 |
| **Resultado** | 97,83% de cobertura, deploy en alwaysdata y último commit de seguridad. |
| **Estado** | ⚠️ Con observaciones |

**Actividades principales:**
- Cobertura de tests: 97,83% líneas (9.714/9.929), 98,71% métodos, 95,10% clases.
- 2.123 tests unitarios + 10 pruebas E2E (Playwright).
- Verificación de email (token 60 min, envío por SMTP/Resend).
- Procesamiento de imágenes (thumbnail, mediana, grande).
- Rate limiting refinado.
- Hardening de seguridad (CSRF, CORS, headers).
- Deploy en alwaysdata (frontend + backend).
- Último commit de seguridad (hardening de logs).

**Observaciones (pendientes para S4):**
- Cobertura E2E solo al 50% (4/8 flujos críticos).
- Sin pipeline de CI/CD (deploy manual por FTP).
- Sin monitoreo continuo (solo chequeo manual).
- 3 issues reportados, todos cerrados (promedio 63,5h).

---

## 3. Decisión post-S3 (D-01, D-02, D-03)

| ID | Decisión | Descripción | Responsable | Fecha |
|---|---|---|---|---|
| D-01 | Congelar alcance | Etiquetar `v1.0.0-rc1` y no sumar módulos hasta cerrar CI y E2E. | Mariano | 25/10 |
| D-02 | Priorizar calidad | Próximo sprint: 60% CI/E2E/monitoreo + 40% correcciones. Cero features nuevas. | Ambos | Semana del 26/10 |
| D-03 | Publicar evidencia | Subir dashboard + PDF y usar link de alwaysdata en la entrega. | Julián | 25/10 |

---

## 4. Sprints planificados (S4–S8)

### Sprint S4 — Producción estable

**Duración:** Semanas 1-2 post 23/10

| ID | Tarea | Prioridad | Estimación | Responsable | Estado |
|---|---|---|---|---|---|
| BL-001 | Configurar SMTP en producción (Resend) | 🔴 Crítico | 2h | Backend | ⏳ |
| BL-002 | Configurar APP_ENV=production | 🔴 Crítico | 1h | Backend | ⏳ |
| BL-006 | Backups automáticos de BD en alwaysdata | 🟠 Alto | 3h | DevOps | ⏳ |
| BL-007 | Monitoreo de uptime (UptimeRobot) | 🟠 Alto | 1h | DevOps | ⏳ |
| BUG-005 | Login feedback (email no verificado) | 🟡 Medio | 2h | Frontend | ⏳ |
| BUG-006 | Rate limiting en redes compartidas (NAT) | 🟡 Medio | 4h | Backend | ⏳ |
| BL-014 | Health check endpoint (`/api/health`) | 🟡 Medio | 2h | Backend | ⏳ |
| BL-015 | Alertas de errores críticos | 🟡 Medio | 4h | Backend | ⏳ |

**Criterios de aceptación:**
- [ ] Emails de verificación se envían en producción.
- [ ] Cookies funcionan cross-origin.
- [ ] Backups diarios configurados y verificados.
- [ ] Monitoreo de uptime activo con alertas.
- [ ] Health check endpoint responde 200 OK.

---

### Sprint S5 — Seguridad y compliance

**Duración:** Semanas 3-4

| ID | Tarea | Prioridad | Estimación | Responsable | Estado |
|---|---|---|---|---|---|
| BL-003 | Validación de complejidad de contraseña | 🟠 Alto | 4h | Backend | ⏳ |
| BL-004 | Política de privacidad + consentimiento | 🟠 Alto | 8h | Frontend + Backend | ⏳ |
| BL-005 | Derecho al olvido (eliminación de cuenta) | 🟠 Alto | 8h | Backend | ⏳ |
| UX-005 | Indicador de fuerza de contraseña | 🟢 Bajo | 3h | Frontend | ⏳ |
| BL-017 | Moderación de reseñas | 🟡 Medio | 6h | Backend + Frontend | ⏳ |

**Criterios de aceptación:**
- [ ] Contraseñas requieren min 8 chars + mayúscula + número.
- [ ] Página de política de privacidad accesible.
- [ ] Registro incluye checkbox de consentimiento.
- [ ] Usuario puede solicitar eliminación de cuenta.

---

### Sprint S6 — Calidad y performance

**Duración:** Semanas 5-6 (60% CI/E2E/monitoreo, 40% correcciones — D-02)

| ID | Tarea | Prioridad | Estimación | Responsable | Estado |
|---|---|---|---|---|---|
| BL-010 | Code splitting / lazy loading (React.lazy) | 🟡 Medio | 6h | Frontend | ⏳ |
| BL-011 | CI/CD con GitHub Actions | 🟡 Medio | 8h | DevOps | ⏳ |
| BL-012 | Rotación de logs (emails, errores, debug) | 🟡 Medio | 3h | Backend | ⏳ |
| DT-001 | Revisar queries N+1 (eager loading) | 🟡 Medio | 6h | Backend | ⏳ |
| DT-003 | Validación de entrada consistente | 🟡 Medio | 6h | Backend | ⏳ |
| DT-004 | Manejo de errores inconsistente | 🟡 Medio | 4h | Backend | ⏳ |
| TEST-001 | Tests E2E: flujo de login | 🟠 Alto | 4h | QA | ⏳ |
| TEST-002 | Tests E2E: flujo de reserva | 🟠 Alto | 6h | QA | ⏳ |
| TEST-003 | Tests E2E: publicación de propiedad | 🟡 Medio | 6h | QA | ⏳ |
| UX-001 | Skeletons en carga (catálogo, detalle) | 🟡 Medio | 4h | Frontend | ⏳ |
| UX-002 | Toast de éxito/error consistente | 🟡 Medio | 3h | Frontend | ⏳ |
| DOC-001 | Actualizar OpenAPI spec | 🟡 Medio | 4h | Backend | ⏳ |
| DOC-004 | Documentar endpoints de admin | 🟡 Medio | 3h | Backend | ⏳ |

**Criterios de aceptación:**
- [ ] Bundle principal < 400KB (gzip).
- [ ] CI/CD ejecuta tests en cada PR.
- [ ] E2E cubre login, reserva y publicación.
- [ ] No hay queries N+1 en endpoints principales.
- [ ] Cobertura de tests ≥ 70%.

---

### Sprint S7 — Features avanzadas

**Duración:** Semanas 7-8

| ID | Tarea | Prioridad | Estimación | Responsable | Estado |
|---|---|---|---|---|---|
| BL-008 | Internacionalización (i18n) | 🟡 Medio | 16h | Frontend | ⏳ |
| BL-009 | Accesibilidad (a11y) WCAG 2.1 AA | 🟡 Medio | 12h | Frontend | ⏳ |
| BL-013 | Feature flags | 🟡 Medio | 6h | Backend + Frontend | ⏳ |
| BL-016 | Almacenamiento externo (S3/Cloudinary) | 🟡 Medio | 8h | Backend | ⏳ |
| BL-021 | Procesamiento de imágenes (WebP) | 🟢 Bajo | 4h | Backend | ⏳ |
| UX-003 | Empty states con acciones | 🟢 Bajo | 3h | Frontend | ⏳ |
| UX-004 | Confirmación de acciones destructivas | 🟢 Bajo | 3h | Frontend | ⏳ |
| UX-006 | Paginación infinita (opcional) | 🟢 Bajo | 6h | Frontend | ⏳ |
| BL-020 | Carrusel de propiedades vistas recientemente | 🟢 Bajo | 4h | Frontend | ⏳ |

**Criterios de aceptación:**
- [ ] Sistema soporta al menos 2 idiomas.
- [ ] Interfaz cumple WCAG 2.1 AA.
- [ ] Imágenes en almacenamiento externo.
- [ ] Lighthouse accessibility ≥ 90.

---

### Sprint S8 — Pulido y lanzamiento

**Duración:** Semanas 9-10

| ID | Tarea | Prioridad | Estimación | Responsable | Estado |
|---|---|---|---|---|---|
| BL-019 | SEO por página (title, meta, OG) | 🟢 Bajo | 6h | Frontend | ⏳ |
| BL-018 | Verificar prefers-reduced-motion | 🟢 Bajo | 2h | Frontend | ✅ |
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
| DT-002 | Cobertura de tests ≥ 70% | 🟡 Medio | 16h | Backend | ⏳ |

**Criterios de aceptación:**
- [ ] Lighthouse SEO ≥ 90.
- [ ] 0 vulnerabilidades críticas en dependencias.
- [ ] Documentación completa y actualizada.
- [ ] Cobertura de tests ≥ 70% verificada.
- [ ] Sistema listo para lanzamiento público.

---

## 5. Evolución semana a semana (datos reales)

| Semana | Fechas | Sprint | Backend | Web | Mobile | Total |
|---|---|---|---|---|---|---|
| W33 | 11-17 ago | S0 | 12 | 1 | 0 | 13 |
| W34 | 18-24 ago | S0 | 14 | 6 | 0 | 20 |
| W35 | 25-31 ago | S0 | 12 | 1 | 0 | 13 |
| W36 | 1-7 sep | S1 | 18 | 1 | 0 | 19 |
| W37 | 8-14 sep | S1 | 81 | 23 | 0 | 104 |
| W38 | 15-21 sep | S1 | 90 | 6 | 0 | 96 |
| W39 | 22-28 sep | S2 | 22 | 49 | 21 | 92 |
| W40 | 29 sep-5 oct | S2 | 29 | 11 | 17 | 57 |
| W41 | 6-12 oct | S2 | 37 | 79 | 70 | 186 |
| W42 | 13-19 oct | S3 | 47 | 8 | 45 | 100 |
| W43 | 20-26 oct | S3 | 43 | 16 | 3 | 62 |
| **Total** | | | **405** | **201** | **156** | **762** |

---

## 6. KPIs del proyecto (datos reales al 25/10/2026)

| KPI | Valor | Meta | Estado |
|---|---|---|---|
| Alcance MVP | 100% (14/14 módulos) | ≥ 90% | ✅ En meta |
| Cobertura backend | 97,83% (9.714/9.929 líneas) | ≥ 85% | ✅ En meta |
| Velocidad de integración | 2,9 PRs/semana (31 PRs) | ≥ 2,0/sem | ✅ En meta |
| Defectos abiertos | 0 (3/3 cerrados) | ≤ 2 | ✅ En meta |
| Cobertura E2E frontend | 50% (4/8 flujos) | ≥ 80% | ⚠️ Atención |
| Latencia API (mediana) | ≈ 500 ms | < 500 ms | ⚠️ Atención |
| Pruebas automatizadas | 2.127 (2.123 unit + 10 E2E) | — | ✅ |

---

## 7. Equipo y ritmo

| Métrica | Valor |
|---|---|
| Autores | Julián Pretz (385 commits, 50,5%) · Mariano Fernández (377, 49,5%) |
| Período | 11/08/2026 → 23/10/2026 (74 días, 10,6 semanas) |
| Commits/semana | 69 promedio |
| PRs/semana | 2,9 promedio |
| Issues | 3 reportados, 3 cerrados (promedio 63,5h) |
| Deploy | https://alquilerr.alwaysdata.net · API: https://alquiler.alwaysdata.net/api/health (200 OK) |

---

## 8. Riesgos (del dashboard)

| ID | Riesgo | Severidad | Mitigación | Fecha objetivo |
|---|---|---|---|---|
| R-01 | Cobertura E2E al 50% (meta 80%) | Alta | Sumar E2E de login, publicación, reserva, reseña, mensajería y favoritos | 15/11/2026 |
| R-02 | Sin pipeline de CI/CD | Alta | Implementar GitHub Actions para tests y deploy | 15/11/2026 |
| R-03 | Conocimiento concentrado (2 devs) | Media | Documentación completa, code review cruzado | Continuo |
| R-04 | Sin monitoreo continuo | Media | Configurar UptimeRobot + alertas de error | 15/11/2026 |
| R-05 | Latencia API ~500ms (frío 1.018ms) | Baja | Optimizar queries, cache, warm-up | 30/11/2026 |

---

## 9. Ceremonias por sprint

| Ceremonia | Frecuencia | Duración | Participantes |
|---|---|---|---|
| Sprint Planning | Inicio de sprint | 2h | Todo el equipo |
| Daily Standup | Diario | 15 min | Todo el equipo |
| Sprint Review | Fin de sprint | 1h | Todo el equipo + stakeholders |
| Sprint Retrospective | Fin de sprint | 1h | Todo el equipo |
| Backlog Refinement | Mid-sprint | 1h | Todo el equipo |

---

## 10. Definición de "Done"

Una tarea se considera completada cuando:

- [ ] El código está escrito y funcional.
- [ ] Los tests unitarios/integración pasan.
- [ ] El código fue revisado (code review).
- [ ] La documentación está actualizada.
- [ ] No hay regresiones en funcionalidad existente.
- [ ] Se verificó en entorno de desarrollo.
- [ ] Se desplegó en producción (si aplica).

---

## 11. Notas

- Sprints S0–S3: datos reales del dashboard (762 commits, 31 PRs, 74 días).
- Sprints S4–S8: planificados para el próximo ciclo (backlog pendiente).
- El próximo sprint (S4) es 60% CI/E2E/monitoreo y 40% correcciones (D-02).
- Cero features nuevas hasta cerrar brechas de calidad (D-01).
- Este plan es flexible y se ajusta según feedback.
