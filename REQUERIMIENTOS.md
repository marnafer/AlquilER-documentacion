# Requerimientos — AlquilER

> Documento de requerimientos funcionales y no funcionales del sistema AlquilER.

## 1. Requerimientos Funcionales

### RF-01: Autenticación y usuarios

| ID | Requerimiento | Prioridad |
|---|---|---|
| RF-01.1 | El sistema debe permitir el registro de nuevos usuarios con email, contraseña, nombre, apellido, teléfono y domicilio. | Alta |
| RF-01.2 | El sistema debe enviar un email de verificación al registrarse. | Alta |
| RF-01.3 | El usuario debe poder verificar su email mediante un enlace con token (válido 60 minutos). | Alta |
| RF-01.4 | El sistema debe permitir el inicio de sesión con email y contraseña. | Alta |
| RF-01.5 | El sistema debe bloquear el login si el email no está verificado. | Alta |
| RF-01.6 | El sistema debe permitir la recuperación de contraseña por email. | Alta |
| RF-01.7 | El sistema debe permitir el cambio de contraseña desde el perfil (con confirmación por email). | Media |
| RF-01.8 | El sistema debe mantener la sesión mediante cookies HttpOnly (access token 15 min, refresh token 15 días). | Alta |
| RF-01.9 | El sistema debe renovar automáticamente el access token usando el refresh token. | Alta |
| RF-01.10 | El sistema debe permitir el cierre de sesión (invalidar refresh token). | Alta |
| RF-01.11 | El sistema debe soportar dos roles: Usuario (rol_id=1) y Administrador (rol_id=2). | Alta |
| RF-01.12 | El administrador debe poder gestionar usuarios (CRUD, restaurar eliminados). | Alta |
| RF-01.13 | El sistema debe registrar la actividad de los usuarios (logs). | Media |

### RF-02: Propiedades

| ID | Requerimiento | Prioridad |
|---|---|---|
| RF-02.1 | El sistema debe permitir crear propiedades con: título, descripción, precio, expensas, dirección, ambientes, dormitorios, baños, capacidad, mascotas, hijos, categoría, localidad, servicios. | Alta |
| RF-02.2 | El sistema debe permitir subir múltiples imágenes por propiedad. | Alta |
| RF-02.3 | El sistema debe permitir marcar una imagen como principal. | Media |
| RF-02.4 | El sistema debe permitir editar y eliminar propiedades (soft delete). | Alta |
| RF-02.5 | El sistema debe permitir restaurar propiedades eliminadas (solo admin). | Media |
| RF-02.6 | El sistema debe permitir marcar propiedades como destacadas (admin). | Media |
| RF-02.7 | El sistema debe mostrar la disponibilidad de la propiedad. | Alta |
| RF-02.8 | El propietario debe poder ver sus propiedades publicadas. | Alta |
| RF-02.9 | El sistema debe soportar categorías, provincias, localidades y servicios como datos maestros. | Alta |

### RF-03: Búsqueda y catálogo

| ID | Requerimiento | Prioridad |
|---|---|---|
| RF-03.1 | El sistema debe mostrar un catálogo de propiedades con paginación. | Alta |
| RF-03.2 | El sistema debe permitir filtrar por: categoría, provincia, localidad, servicios, mascotas, hijos, precio (min/max), ambientes, dormitorios, baños, capacidad. | Alta |
| RF-03.3 | El sistema debe permitir ordenar resultados (más recientes, precio, etc.). | Media |
| RF-03.4 | El sistema debe mostrar propiedades destacadas en el home. | Media |
| RF-03.5 | El sistema debe mostrar un carrusel de propiedades vistas recientemente. | Baja |
| RF-03.6 | El sistema debe mostrar un detalle completo de cada propiedad (imágenes, servicios, ubicación, políticas). | Alta |

### RF-04: Reservas

| ID | Requerimiento | Prioridad |
|---|---|---|
| RF-04.1 | El usuario debe poder reservar una propiedad con fecha de inicio y fin. | Alta |
| RF-04.2 | El propietario debe poder confirmar o rechazar una reserva. | Alta |
| RF-04.3 | El usuario debe poder cancelar su reserva. | Alta |
| RF-04.4 | El propietario debe poder finalizar una reserva. | Alta |
| RF-04.5 | Las reservas tienen estados: pendiente, confirmada, rechazada, cancelada, finalizada. | Alta |
| RF-04.6 | El sistema debe enviar notificaciones por cambios de estado de reserva. | Media |
| RF-04.7 | El administrador debe poder gestionar reservas (CRUD). | Media |

### RF-05: Consultas

| ID | Requerimiento | Prioridad |
|---|---|---|
| RF-05.1 | El usuario debe poder consultar sobre una propiedad. | Alta |
| RF-05.2 | El propietario debe poder responder consultas. | Alta |
| RF-05.3 | El sistema debe soportar mensajería bidireccional en consultas. | Media |
| RF-05.4 | El administrador debe poder gestionar consultas. | Media |

### RF-06: Reseñas

| ID | Requerimiento | Prioridad |
|---|---|---|
| RF-06.1 | Después de una reserva finalizada, el inquilino puede reseñar la propiedad. | Media |
| RF-06.2 | Después de una reserva finalizada, el propietario puede reseñar al inquilino. | Media |
| RF-06.3 | Las reseñas tienen calificación (1-5) y comentario. | Media |
| RF-06.4 | El sistema debe mostrar reseñas recibidas, de propiedades y escritas. | Media |
| RF-06.5 | El administrador debe poder gestionar reseñas. | Baja |

### RF-07: Favoritos

| ID | Requerimiento | Prioridad |
|---|---|---|
| RF-07.1 | El usuario debe poder agregar/quitar propiedades de favoritos. | Media |
| RF-07.2 | El usuario debe poder ver su lista de favoritos. | Media |

### RF-08: Notificaciones

| ID | Requerimiento | Prioridad |
|---|---|---|
| RF-08.1 | El sistema debe notificar sobre nuevas reservas, confirmaciones, rechazos, consultas y reseñas. | Media |
| RF-08.2 | El usuario debe poder marcar notificaciones como leídas. | Media |
| RF-08.3 | El sistema debe mostrar un contador de notificaciones no leídas. | Media |

### RF-09: Contacto

| ID | Requerimiento | Prioridad |
|---|---|---|
| RF-09.1 | El sistema debe permitir enviar mensajes de contacto al administrador. | Media |
| RF-09.2 | El mensaje de contacto se envía por email al administrador. | Media |

### RF-10: Administración

| ID | Requerimiento | Prioridad |
|---|---|---|
| RF-10.1 | El administrador debe tener un panel con métricas y accesos rápidos. | Media |
| RF-10.2 | El administrador debe poder gestionar: usuarios, categorías, provincias, localidades, roles, servicios, propiedades, reservas, consultas, reseñas, logs. | Alta |
| RF-10.3 | El administrador debe poder ver logs de actividad. | Media |
| RF-10.4 | El administrador debe poder restaurar registros eliminados (soft delete). | Media |

### RF-11: UI/UX

| ID | Requerimiento | Prioridad |
|---|---|---|
| RF-11.1 | El sistema debe soportar modo claro y oscuro. | Media |
| RF-11.2 | El sistema debe ser responsive (móvil, tablet, desktop). | Alta |
| RF-11.3 | El sistema debe tener animaciones de scroll reveal. | Baja |
| RF-11.4 | El sistema debe mostrar estados de carga (skeletons, spinners). | Media |
| RF-11.5 | El sistema debe mostrar estados vacíos y de error con acciones. | Media |
| RF-11.6 | El sistema debe tener SEO (title, meta description, OG tags por página). | Media |

---

## 2. Requerimientos No Funcionales

### RNF-01: Rendimiento
- El backend debe responder a requests GET en menos de 500ms (p95).
- El frontend debe cargar en menos de 3 segundos en conexión 3G.
- Las consultas a la base de datos deben estar indexadas.

### RNF-02: Seguridad
- Las contraseñas se almacenan hasheadas (bcrypt).
- Los tokens JWT tienen expiración (15 min access, 15 días refresh).
- Las credenciales de autenticación se almacenan en cookies HttpOnly.
- El backend valida el Origin en operaciones que modifican datos (CSRF).
- Rate limiting: 120 GET/min, 30 write/min, 5 login/min.
- Los emails de verificación tienen token con expiración (60 min).
- Los tokens de reset de contraseña tienen expiración (30 min).

### RNF-03: Usabilidad
- La interfaz debe ser intuitiva y responsive.
- Los mensajes de error deben ser claros y accionables.
- El sistema debe mostrar feedback visual (loading, success, error).

### RNF-04: Mantenibilidad
- Arquitectura en capas: Controller → Service → Repository → Model.
- Uso de interfaces para desacoplamiento (RepositoryInterface, TokenProviderInterface).
- Tests unitarios y de integración (PHPUnit).
- Tests E2E (Playwright) para flujos críticos.

### RNF-05: Escalabilidad
- El backend es stateless (JWT en cookies, no sesiones en servidor).
- El frontend es una SPA estática (se puede servir desde CDN).
- Cache de catálogos (FileCache) para consultas de baja rotación.

### RNF-06: Compatibilidad
- Navegadores: Chrome, Firefox, Safari, Edge (últimas 2 versiones).
- Dispositivos: móvil (320px+), tablet (768px+), desktop (1024px+).
- Backend: PHP 8.2+, MySQL 8.0+, Apache/Nginx.

### RNF-07: Disponibilidad
- El sistema debe estar disponible 24/7 en producción.
- Backups de base de datos recomendados.

---

## 3. Actores del sistema

| Actor | Descripción | Permisos |
|---|---|---|
| **Visitante** | Usuario no autenticado | Ver catálogo, detalle de propiedades, servicios, contacto, FAQ |
| **Usuario** | Usuario autenticado (rol_id=1) | Todo lo anterior + reservar, favoritos, consultas, perfil, publicar propiedades |
| **Administrador** | Usuario autenticado (rol_id=2) | Todo lo anterior + panel admin, gestión de usuarios/categorías/servicios/etc., logs |
| **Propietario** | Rol implícito (usuario que publicó propiedades) | Gestionar sus propiedades, confirmar/rechazar reservas, responder consultas |
| **Inquilino** | Rol implícito (usuario que reservó) | Reservar, cancelar, calificar propiedad |

> Nota: El rol no determina si es propietario o inquilino. Un mismo usuario puede publicar, alquilar, reservar y reseñar sin necesidad de cuentas separadas.
