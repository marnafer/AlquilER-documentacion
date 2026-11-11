# Reglas de Negocio — AlquilER

> Documento de reglas de negocio del sistema AlquilER.

## 1. Usuarios y roles

| Regla | Descripción |
|---|---|
| RN-01 | El sistema tiene dos roles: `Usuario` (rol_id=1) y `Administrador` (rol_id=2). |
| RN-02 | El rol **no determina** si un usuario es propietario o inquilino. Un mismo usuario puede publicar propiedades, reservar, consultar y reseñar sin cuentas separadas. |
| RN-03 | Solo los administradores pueden acceder al panel de administración (`/admin/*`). |
| RN-04 | Un usuario debe verificar su email antes de poder iniciar sesión. |
| RN-05 | El token de verificación de email es válido por **60 minutos**. |
| RN-06 | El token de recuperación de contraseña es válido por **30 minutos**. |
| RN-07 | Un usuario solo puede tener **un refresh token activo** a la vez. Al iniciar sesión, se eliminan los refresh tokens anteriores. |
| RN-08 | El access token expira en **15 minutos**. El refresh token expira en **15 días**. |
| RN-09 | Al cerrar sesión, se invalida el refresh token. |
| RN-10 | Las contraseñas se almacenan hasheadas con bcrypt. |
| RN-11 | El sistema registra la actividad de los usuarios (login, registro, etc.) con IP y timestamp. |

## 2. Propiedades

| Regla | Descripción |
|---|---|
| RN-12 | Solo el propietario de una propiedad o un administrador puede editarla o eliminarla. |
| RN-13 | Las propiedades eliminadas usan **soft delete** (`deleted_at`). Solo administradores pueden restaurarlas. |
| RN-14 | Una propiedad puede tener múltiples imágenes, pero solo una puede ser la **principal** (`es_principal = 1`). |
| RN-15 | Las propiedades tienen estados: `disponible` (true/false) y `destacada` (true/false). |
| RN-16 | Solo administradores pueden marcar una propiedad como `destacada`. |
| RN-17 | Las propiedades se asocian a una categoría, localidad y propietario (usuario). |
| RN-18 | Los servicios se asocian a propiedades mediante una tabla pivote N:M (`propiedad_servicio`). |
| RN-19 | El propietario puede definir requisitos del interesado (mascotas, hijos, fumador, etc.) en formato JSON. |

## 3. Búsqueda y catálogo

| Regla | Descripción |
|---|---|
| RN-20 | El catálogo es de **lectura pública** (no requiere autenticación). |
| RN-21 | Los filtros disponibles: categoría, provincia, localidad, servicios, mascotas, hijos, precio (min/max), ambientes, dormitorios, baños, capacidad. |
| RN-22 | El ordenamiento por defecto es por fecha de publicación descendente (más recientes primero). |
| RN-23 | La paginación por defecto es de 10 resultados por página. |
| RN-24 | Las propiedades eliminadas (soft delete) no aparecen en el catálogo. |
| RN-25 | El propietario puede consultar sus propias propiedades (incluyendo no disponibles). |

## 4. Reservas

| Regla | Descripción |
|---|---|
| RN-26 | Solo usuarios autenticados pueden reservar propiedades. |
| RN-27 | Los estados de reserva son: `pendiente` → `confirmada` / `rechazada` / `cancelada` → `finalizada`. |
| RN-28 | Solo el **propietario** de la propiedad puede confirmar o rechazar una reserva. |
| RN-29 | Solo el **inquilino** (quien hizo la reserva) puede cancelarla. |
| RN-30 | Solo el **propietario** puede finalizar una reserva. |
| RN-31 | Al confirmar/rechazar/cancelar/finalizar, se envía una notificación al inquilino. |
| RN-32 | Al crear una reserva, se envía una notificación al propietario. |
| RN-33 | Las reservas eliminadas usan soft delete. Solo administradores pueden restaurarlas. |
| RN-34 | Un usuario puede reservar una propiedad propia (para testing). |

## 5. Consultas

| Regla | Descripción |
|---|---|
| RN-35 | Solo usuarios autenticados pueden consultar sobre una propiedad. |
| RN-36 | El propietario de la propiedad puede responder consultas. |
| RN-37 | Las consultas soportan mensajería bidireccional (consulta + respuestas). |
| RN-38 | Al crear una consulta, se envía una notificación al propietario. |
| RN-39 | Al responder una consulta, se envía una notificación al interesado. |
| RN-40 | Las consultas eliminadas usan soft delete. |

## 6. Reseñas

| Regla | Descripción |
|---|---|
| RN-41 | Las reseñas solo se pueden crear después de una reserva **finalizada**. |
| RN-42 | Existen dos tipos de reseña: `propiedad` (el inquilino califica la propiedad) e `inquilino` (el propietario califica al inquilino). |
| RN-43 | La calificación es de 1 a 5 estrellas. |
| RN-44 | Un usuario puede calificar una reserva solo una vez por tipo. |
| RN-45 | El sistema muestra reseñas recibidas (como inquilino), de propiedades (de tus propiedades) y escritas (por ti). |
| RN-46 | El administrador puede gestionar reseñas (eliminar, restaurar). |

## 7. Favoritos

| Regla | Descripción |
|---|---|
| RN-47 | Solo usuarios autenticados pueden agregar/quitar favoritos. |
| RN-48 | Un usuario no puede agregar la misma propiedad dos veces a favoritos. |
| RN-49 | Los favoritos no usan soft delete (se eliminan físicamente). |

## 8. Notificaciones

| Regla | Descripción |
|---|---|
| RN-50 | Las notificaciones se generan automáticamente por eventos del sistema (nueva reserva, confirmación, rechazo, consulta, mensaje, reseña). |
| RN-51 | El usuario puede marcar notificaciones como leídas individualmente o todas a la vez. |
| RN-52 | El sistema muestra un contador de notificaciones no leídas. |
| RN-53 | Las notificaciones eliminadas usan soft delete. |

## 9. Contacto

| Regla | Descripción |
|---|---|
| RN-54 | El formulario de contacto requiere: nombre, email, asunto, mensaje. |
| RN-55 | El mensaje se envía por email al administrador (`MAIL_TO`). |
| RN-56 | Si `MAIL_TO` no está configurado, se lanza `BadRequestException`. |
| RN-57 | Si SMTP no está configurado en producción, se lanza `RuntimeException`. |

## 10. Administración

| Regla | Descripción |
|---|---|
| RN-58 | Solo administradores (rol_id=2) acceden al panel `/admin`. |
| RN-59 | El administrador puede gestionar: usuarios, categorías, provincias, localidades, roles, servicios, propiedades, reservas, consultas, reseñas, logs. |
| RN-60 | El administrador puede ver y restaurar registros eliminados (papelera). |
| RN-61 | La papelera (registros eliminados) solo es visible para administradores. |
| RN-62 | El administrador puede marcar propiedades como destacadas. |

## 11. Seguridad

| Regla | Descripción |
|---|---|
| RN-63 | Las contraseñas se almacenan hasheadas con bcrypt. |
| RN-64 | Los tokens JWT tienen expiración (15 min access, 15 días refresh). |
| RN-65 | Las credenciales se almacenan en cookies HttpOnly (no en localStorage). |
| RN-66 | El backend valida el `Origin` en operaciones que modifican datos (CSRF). |
| RN-67 | Rate limiting: 120 GET/min, 30 write/min, 5 login/min, 3 recuperación/min. |
| RN-68 | Los emails de verificación tienen token con expiración de 60 minutos. |
| RN-69 | Los tokens de reset de contraseña tienen expiración de 30 minutos. |
| RN-70 | CORS: solo orígenes configurados en `CORS_ALLOWED_ORIGINS` pueden acceder a la API. |
| RN-71 | En producción, las cookies usan `Secure=true` y `SameSite=None`. |

## 12. UI/UX

| Regla | Descripción |
|---|---|
| RN-72 | El sistema soporta modo claro y oscuro (persiste en localStorage). |
| RN-73 | El sistema es responsive (móvil, tablet, desktop). |
| RN-74 | Los mensajes de error deben ser claros y accionables. |
| RN-75 | El sistema muestra estados de carga (skeletons, spinners). |
| RN-76 | El sistema muestra estados vacíos y de error con acciones de reintento. |
| RN-77 | El sistema tiene SEO (title, meta description, OG tags por página). |
| RN-78 | Las animaciones de scroll reveal se desactivan si el usuario prefiere menos movimiento. |
