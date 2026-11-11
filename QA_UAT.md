# QA y UAT — AlquilER

> Documento de aseguramiento de calidad (QA) y pruebas de aceptación del usuario (UAT) del proyecto AlquilER.

---

## 1. Introducción

### 1.1 ¿Qué es QA?

**QA (Quality Assurance)** es el proceso de asegurar que el software cumple con los estándares de calidad definidos. Incluye pruebas automatizadas, revisiones de código, y verificación de que el sistema funciona correctamente.

### 1.2 ¿Qué es UAT?

**UAT (User Acceptance Testing)** son las pruebas de aceptación realizadas por el usuario final (o stakeholder) para validar que el sistema cumple con los requisitos de negocio. Es la última etapa antes del release a producción.

### 1.3 Diferencias

| | QA | UAT |
|---|---|---|
| **Quién** | Developers / QA engineers | Usuarios / clientes / stakeholders |
| **Cuándo** | Continuo (cada commit/PR) | Al final de un sprint/release |
| **Qué verifica** | Que el código funcione técnicamente | Que el sistema resuelva el problema del usuario |
| **Criterio de éxito** | Tests pasan, sin bugs críticos | El usuario acepta el sistema |
| **En AlquilER** | PHPUnit, Playwright, ESLint, PHP Lint | Validación por el profesor/cliente |

---

## 2. Estrategia de QA

### 2.1 Niveles de testing

| Nivel | Descripción | Herramienta | Cobertura actual |
|---|---|---|---|
| **Unitarios** | Prueban funciones/métodos individuales | PHPUnit 11.5 | 2.123 tests, 97,83% líneas |
| **Integración** | Prueban interacción entre componentes (BD real) | PHPUnit (suite Integration) | Tests de integridad de BD |
| **E2E** | Prueban flujos completos del usuario | Playwright | 10 tests, 4/8 flujos críticos |
| **Linting** | Verifican estilo y calidad del código | ESLint (frontend), PHP Lint (backend) | Automático en CI |
| **Code Review** | Revisión manual por pares | GitHub PRs | 31 PRs mergeados |

### 2.2 Pirámide de testing

```
         /\
        /  \   E2E (Playwright) — 10 tests, 4/8 flujos
       /----\
      /      \  Integración (PHPUnit) — BD real
     /--------\
    /          \ Unitarios (PHPUnit) — 2.123 tests, 97,83%
   /------------\
```

### 2.3 Herramientas

| Herramienta | Tipo | Uso en AlquilER |
|---|---|---|
| **PHPUnit 11.5** | Unit/Integration | Backend PHP — 2.123 tests unitarios |
| **Playwright** | E2E | Frontend — 10 pruebas, 4 flujos críticos |
| **Xdebug** | Coverage | Cobertura de código (97,83% líneas) |
| **ESLint** | Linting | Frontend React/JS |
| **PHP Lint** | Linting | Backend PHP (sintaxis) |
| **GitHub Actions** | CI | Ejecución automática de tests en PRs |

### 2.4 Cobertura actual

| Métrica | Valor | Meta |
|---|---|---|
| Líneas | 97,83% (9.714/9.929) | ≥ 85% |
| Métodos | 98,71% (843/854) | ≥ 90% |
| Clases | 95,10% | ≥ 90% |
| Tests unitarios | 2.123 | — |
| Tests E2E | 10 (4/8 flujos) | ≥ 80% (8/8) |

### 2.5 Flujos E2E cubiertos

| Flujo | Estado |
|---|---|
| Login | ✅ Cubierto |
| Registro | ✅ Cubierto |
| Publicación de propiedad | ✅ Cubierto |
| Reserva | ✅ Cubierto |
| Reseña | ⏳ Pendiente |
| Mensajería (consulta) | ⏳ Pendiente |
| Favoritos | ⏳ Pendiente |
| Notificaciones | ⏳ Pendiente |

---

## 3. Estrategia de UAT

### 3.1 Alcance

El UAT valida que el sistema cumple con los requerimientos de negocio definidos en `REQUERIMIENTOS.md`. Se ejecuta en el entorno de producción (https://alquilerr.alwaysdata.net) con datos reales.

### 3.2 Checklist de UAT

#### Autenticación y usuarios
- [ ] Registro de nuevo usuario con email, contraseña, nombre, apellido, teléfono, domicilio
- [ ] Recepción de email de verificación
- [ ] Verificación de email mediante enlace (token válido 60 min)
- [ ] Login con email y contraseña (email verificado)
- [ ] Bloqueo de login si email no verificado
- [ ] Recuperación de contraseña por email
- [ ] Cambio de contraseña desde perfil
- [ ] Cierre de sesión
- [ ] Renovación automática de access token

#### Propiedades
- [ ] Crear propiedad con todos los campos
- [ ] Subir múltiples imágenes
- [ ] Marcar imagen principal
- [ ] Editar propiedad
- [ ] Eliminar propiedad (soft delete)
- [ ] Restaurar propiedad (admin)
- [ ] Marcar propiedad como destacada (admin)
- [ ] Ver propiedades propias

#### Búsqueda y catálogo
- [ ] Ver catálogo con paginación
- [ ] Filtrar por categoría, provincia, localidad, servicios, mascotas, hijos, precio, ambientes, dormitorios, baños, capacidad
- [ ] Ordenar resultados
- [ ] Ver propiedades destacadas en home
- [ ] Ver detalle completo de propiedad
- [ ] Carrusel de propiedades vistas recientemente

#### Reservas
- [ ] Crear reserva con fecha de inicio y fin
- [ ] Confirmar reserva (propietario)
- [ ] Rechazar reserva (propietario)
- [ ] Cancelar reserva (inquilino)
- [ ] Finalizar reserva (propietario)
- [ ] Notificaciones por cambios de estado

#### Consultas
- [ ] Crear consulta sobre propiedad
- [ ] Responder consulta (propietario)
- [ ] Mensajería bidireccional
- [ ] Notificaciones por consulta/respuesta

#### Reseñas
- [ ] Crear reseña después de reserva finalizada (inquilino → propiedad)
- [ ] Crear reseña después de reserva finalizada (propietario → inquilino)
- [ ] Ver reseñas recibidas, de propiedades, escritas

#### Favoritos
- [ ] Agregar/quitar propiedad de favoritos
- [ ] Ver lista de favoritos

#### Notificaciones
- [ ] Ver notificaciones
- [ ] Ver contador de no leídas
- [ ] Marcar como leída individualmente
- [ ] Marcar todas como leídas

#### Contacto
- [ ] Enviar mensaje de contacto
- [ ] Recepción del email por el admin

#### Administración (solo admin)
- [ ] Acceder al panel de administración
- [ ] Gestionar usuarios (CRUD + restore)
- [ ] Gestionar categorías, provincias, localidades, roles, servicios
- [ ] Gestionar propiedades (todas, incluyendo eliminadas)
- [ ] Gestionar reservas, consultas, reseñas
- [ ] Ver logs de actividad
- [ ] Restaurar registros eliminados (papelera)

#### UI/UX
- [ ] Modo claro y oscuro
- [ ] Responsive (móvil, tablet, desktop)
- [ ] Estados de carga (skeletons, spinners)
- [ ] Estados vacíos y de error con acciones
- [ ] SEO (title, meta description, OG tags)

### 3.3 Criterios de aceptación

Una funcionalidad se considera **aceptada** cuando:
1. El usuario puede completar el flujo sin errores.
2. El resultado es el esperado según los requerimientos.
3. No hay errores visibles en la consola del navegador.
4. La experiencia es fluida en móvil y desktop.

### 3.4 Entorno de UAT

| Aspecto | Detalle |
|---|---|
| **URL** | https://alquilerr.alwaysdata.net |
| **API** | https://alquiler.alwaysdata.net |
| **Usuario de prueba** | prop@gmail.com / Test1234! (admin, verificado) |
| **Navegadores** | Chrome, Firefox, Safari, Edge |
| **Dispositivos** | Móvil (320px+), Tablet (768px+), Desktop (1024px+) |

---

## 4. Defectos y tracking

### 4.1 Severidades

| Severidad | Descripción | Ejemplo |
|---|---|---|
| **Crítica** | Bloquea el uso del sistema | No se puede hacer login |
| **Alta** | Funcionalidad principal no funciona | No se pueden reservar propiedades |
| **Media** | Funcionalidad secundaria afectada | Filtros no funcionan correctamente |
| **Baja** | Problema cosmético o menor | Texto mal alineado |

### 4.2 Estados de un defecto

```
Abierto → En progreso → Resuelto → Verificado → Cerrado
                                    ↓
                              Reabierto (si falla de nuevo)
```

### 4.3 Historial de defectos (del dashboard)

| Issue | Título | Severidad | Estado | Tiempo de resolución |
|---|---|---|---|---|
| #1 | (issue cerrado) | — | Cerrado | ~63,5h promedio |
| #2 | (issue cerrado) | — | Cerrado | ~63,5h promedio |
| #3 | (issue cerrado) | — | Cerrado | ~63,5h promedio |

> 3 issues reportados, 3 cerrados. Promedio de resolución: 63,5 horas.

---

## 5. Métricas de calidad

### 5.1 Métricas actuales (dashboard al 25/10/2026)

| Métrica | Valor | Meta | Estado |
|---|---|---|---|
| Cobertura backend (líneas) | 97,83% | ≥ 85% | ✅ En meta |
| Cobertura backend (métodos) | 98,71% | ≥ 90% | ✅ En meta |
| Cobertura backend (clases) | 95,10% | ≥ 90% | ✅ En meta |
| Tests unitarios | 2.123 | — | ✅ |
| Tests E2E | 10 (4/8 flujos) | ≥ 80% (8/8) | ⚠️ Atención |
| Issues abiertos | 0 | ≤ 2 | ✅ En meta |
| PRs mergeados | 31 (2,9/sem) | ≥ 2,0/sem | ✅ En meta |

### 5.2 Objetivos de calidad (Sprint S4+)

| Objetivo | Meta | Plazo |
|---|---|---|
| Cobertura E2E | ≥ 80% (8/8 flujos) | 15/11/2026 |
| Cobertura unitaria | ≥ 70% (mantener) | Continuo |
| Tiempo de respuesta API | < 500ms (p95) | 30/11/2026 |
| Defectos críticos abiertos | 0 | Continuo |
| UAT aprobado | 100% checklist | Antes del release |

---

## 6. Roles y responsabilidades

| Rol | Responsabilidad en QA/UAT |
|---|---|
| **Developer** | Escribir código, tests unitarios, code review |
| **QA / Tester** | Ejecutar tests E2E, verificar cobertura, reportar bugs |
| **Product Owner / Profesor** | Ejecutar UAT, validar requerimientos, aceptar/rechazar |
| **DevOps** | Configurar CI/CD, monitoreo, backups |

---

## 7. Notas

- El QA es continuo y automatizado (GitHub Actions).
- El UAT se ejecuta manualmente por el stakeholder.
- Los defectos encontrados en UAT se registran en GitHub Issues.
- Ver `BACKLOG.md` para el listado de items pendientes de testing.
- Ver `SPRINTS.md` para la planificación de sprints de calidad.
- Ver `CI_CD.md` para la configuración de integración continua.
