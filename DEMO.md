# Demo — AlquilER

> Guía de demostración del sistema AlquilER.
> Incluye script de presentación, flujo de demo, credenciales de prueba y checklist.

---

## 1. Información general

| Aspecto | Detalle |
|---|---|
| **URL de la app** | https://alquilerr.alwaysdata.net |
| **API** | https://alquiler.alwaysdata.net |
| **Health check** | https://alquiler.alwaysdata.net/api/health |
| **Dashboard** | https://github.com/JulianMP1997/alquiler-dashboard |
| **Repositorios** | Backend: marnafer/AlquilER-backend · Frontend: marnafer/AlquilER-frontend-web · Docs: marnafer/AlquilER-documentacion |

---

## 2. Credenciales de prueba

| Usuario | Email | Contraseña | Rol | Descripción |
|---|---|---|---|---|
| Admin | prop@gmail.com | Test1234! | Administrador | Acceso completo al panel admin |
| Usuario 1 | (ver BD) | Test1234! | Usuario | Publicó propiedades |
| Usuario 2 | (ver BD) | Test1234! | Usuario | Reservó propiedades |

> Todos los usuarios tienen la contraseña `Test1234!` y están verificados.

### Datos de prueba en la BD

- **Propiedades**: Varias propiedades publicadas con imágenes, servicios, categorías
- **Reservas**: Reservas en distintos estados (pendiente, confirmada, finalizada, cancelada)
- **Consultas**: Consultas con mensajería bidireccional
- **Reseñas**: Reseñas de propiedades e inquilinos
- **Favoritos**: Propiedades marcadas como favoritas
- **Notificaciones**: Notificaciones de distintos tipos

---

## 3. Script de demo (10 minutos)

### 3.1 Introducción (1 min)

> "AlquilER es una plataforma web para buscar, publicar y reservar propiedades en alquiler. Desarrollada con React + Vite en el frontend y PHP 8.2 en el backend, con MySQL como base de datos."

**Mostrar**: Home page con catálogo y propiedades destacadas.

### 3.2 Búsqueda y filtros (1.5 min)

1. Mostrar el catálogo de propiedades
2. Aplicar filtros: categoría, provincia, precio, mascotas
3. Mostrar ordenamiento (más recientes, precio asc/desc)
4. Mostrar paginación

> "El catálogo soporta 13 filtros combinables, paginación y ordenamiento. Las propiedades destacadas aparecen primero."

### 3.3 Detalle de propiedad (1.5 min)

1. Click en una propiedad
2. Mostrar galería de imágenes (carrusel)
3. Mostrar características (ambientes, dormitorios, baños, capacidad, superficie)
4. Mostrar servicios (WiFi, parking, pileta, etc.)
5. Mostrar reseñas
6. Mostrar botones: Reservar, Favorito, Consultar

> "Cada propiedad tiene un detalle completo con imágenes, servicios, ubicación, políticas y reseñas."

### 3.4 Reserva (1.5 min)

1. Click en "Reservar"
2. Completar fechas y mensaje
3. Enviar reserva
4. Mostrar estado "Pendiente"
5. (Como propietario) Confirmar la reserva
6. Mostrar estado "Confirmada"

> "El flujo de reserva incluye: crear, confirmar/rechazar, cancelar, finalizar. Cada cambio genera una notificación."

### 3.5 Publicar propiedad (1.5 min)

1. Login como usuario
2. Ir a "Mi perfil" → "Mis propiedades"
3. Click en "Publicar nueva propiedad"
4. Completar formulario (título, descripción, precio, categoría, localidad, etc.)
5. Subir imágenes
6. Marcar imagen principal
7. Asociar servicios
8. Publicar

> "El propietario puede publicar propiedades con múltiples imágenes, servicios y requisitos del interesado."

### 3.6 Consultas y mensajería (1 min)

1. En el detalle de una propiedad, click en "Consultar"
2. Enviar mensaje
3. (Como propietario) Ver la consulta y responder
4. Mostrar la conversación bidireccional

> "Las consultas soportan mensajería bidireccional con notificaciones automáticas."

### 3.7 Reseñas (1 min)

1. Mostrar una reserva finalizada
2. Click en "Reseñar"
3. Seleccionar calificación (1-5 estrellas)
4. Escribir comentario
5. Enviar
6. Mostrar la reseña en el detalle de la propiedad

> "Después de una reserva finalizada, inquilino y propietario pueden dejarse reseñas mutuamente."

### 3.8 Notificaciones (0.5 min)

1. Click en el ícono 🔔
2. Mostrar lista de notificaciones
3. Mostrar contador de no leídas
4. Marcar como leída

> "El sistema genera notificaciones automáticas para reservas, consultas, mensajes y reseñas."

### 3.9 Panel de administración (1 min)

1. Login como admin (prop@gmail.com)
2. Ir a `/admin`
3. Mostrar dashboard con métricas
4. Mostrar gestión de usuarios, propiedades, reservas
5. Mostrar papelera (registros eliminados)
6. Mostrar logs de actividad

> "El admin tiene un panel completo para gestionar todas las entidades del sistema, ver logs y restaurar registros eliminados."

### 3.10 Responsive y dark mode (0.5 min)

1. Mostrar la app en modo oscuro
2. Mostrar la app en móvil (responsive)
3. Mostrar estados de carga (skeletons)
4. Mostrar estados vacíos

> "La app es responsive (móvil, tablet, desktop) y soporta modo claro/oscuro."

---

## 4. Flujo de demo recomendado (orden de pantallas)

```
1. Home (catálogo + destacadas)
   ↓
2. Detalle de propiedad (galería, características, servicios, reseñas)
   ↓
3. Reservar (formulario → estado pendiente)
   ↓
4. Login como propietario → confirmar reserva
   ↓
5. Login como inquilino → ver reserva confirmada → reseñar
   ↓
6. Publicar propiedad (formulario completo + imágenes + servicios)
   ↓
7. Consultas (enviar → responder → conversación)
   ↓
8. Favoritos (agregar → ver lista)
   ↓
9. Notificaciones (panel + contador)
   ↓
10. Perfil (editar, cambiar contraseña)
    ↓
11. Panel admin (dashboard, gestión, papelera, logs)
    ↓
12. Responsive + dark mode
```

---

## 5. Checklist de demo

### Antes de la demo

- [ ] Verificar que la app esté online: https://alquilerr.alwaysdata.net
- [ ] Verificar API health: https://alquiler.alwaysdata.net/api/health
- [ ] Tener credenciales listas (prop@gmail.com / Test1234!)
- [ ] Tener datos de prueba en la BD (propiedades, reservas, consultas, reseñas)
- [ ] Probar el flujo completo al menos una vez
- [ ] Tener el dashboard abierto en otra pestaña (opcional)
- [ ] Tener el repo de documentación abierto (opcional)

### Durante la demo

- [ ] Mostrar home y catálogo
- [ ] Aplicar filtros de búsqueda
- [ ] Ver detalle de propiedad
- [ ] Crear una reserva
- [ ] Confirmar/rechazar reserva (como propietario)
- [ ] Publicar una propiedad
- [ ] Enviar y responder consulta
- [ ] Dejar una reseña
- [ ] Ver notificaciones
- [ ] Mostrar panel de admin
- [ ] Mostrar responsive y dark mode

### Después de la demo

- [ ] Responder preguntas
- [ ] Mostrar documentación (repo, ERD, arquitectura)
- [ ] Mostrar tests y cobertura (dashboard)

---

## 6. Preguntas frecuentes de la demo

### ¿Qué tecnologías usan?
- Frontend: React 19 + Vite 5 + React Router
- Backend: PHP 8.2 (sin framework, arquitectura en capas)
- BD: MySQL 8.0
- ORM: Eloquent (Illuminate)
- Auth: JWT (access 15 min, refresh 15 días) en cookies HttpOnly
- Deploy: alwaysdata (hosting compartido)
- CI/CD: GitHub Actions

### ¿Cuántos commits/tests tienen?
- 762 commits en 74 días
- 31 PRs mergeados
- 2.127 pruebas automatizadas
- 97,83% cobertura backend
- 3 repositorios (backend, frontend web, frontend mobile)

### ¿Qué falta para producción?
- Configurar SMTP en producción (emails)
- Configurar APP_ENV=production (cookies cross-origin)
- Backups automáticos de BD
- Monitoreo de uptime
- CI/CD completo (deploy automatizado)
- Cobertura E2E al 80% (actualmente 50%)

### ¿Cómo se maneja la seguridad?
- Contraseñas con bcrypt
- JWT con expiración corta (15 min)
- Cookies HttpOnly (no accesibles desde JS)
- CSRF protection (validación de Origin)
- Rate limiting (12 reglas por IP y endpoint)
- CORS con orígenes específicos
- Soft delete (no se borran datos físicamente)

---

## 7. Links útiles

| Recurso | URL |
|---|---|
| App en producción | https://alquilerr.alwaysdata.net |
| API en producción | https://alquiler.alwaysdata.net |
| Health check | https://alquiler.alwaysdata.net/api/health |
| Dashboard del proyecto | https://github.com/JulianMP1997/alquiler-dashboard |
| Repo backend | https://github.com/marnafer/AlquilER-backend |
| Repo frontend | https://github.com/marnafer/AlquilER-frontend-web |
| Repo documentación | https://github.com/marnafer/AlquilER-documentacion |
| Documentación (README) | https://github.com/marnafer/AlquilER-documentacion/blob/main/README.md |

---

## 8. Plan B (si algo falla durante la demo)

| Problema | Solución |
|---|---|
| La app no carga | Verificar https://alquilerr.alwaysdata.net. Si está caída, mostrar el dashboard (https://github.com/JulianMP1997/alquiler-dashboard) o capturas de `capturas/` |
| Login falla | Verificar credenciales (prop@gmail.com / Test1234!). Probar con otro navegador. |
| No se envían emails | Es esperado en producción si SMTP no está configurado. Mostrar que el código maneja el error correctamente. |
| Imagen no carga | Verificar conexión. Las imágenes se sirven desde `storage/uploads/`. |
| Error 429 (rate limit) | Esperar 1 minuto. El rate limiting es por IP. |
| La reserva no se crea | Verificar que el usuario esté logueado y que la propiedad esté disponible. |
| El admin no puede acceder | Verificar que el usuario tenga rol_id=2 (admin). |

---

## 9. Mostrar métricas del proyecto (opcional, 2 min)

Si hay tiempo, mostrar el dashboard del proyecto:

1. Abrir https://github.com/JulianMP1997/alquiler-dashboard
2. Mostrar los 5 KPIs:
   - Alcance MVP: 100% (14/14 módulos)
   - Cobertura backend: 97,83% (9.714/9.929 líneas)
   - Velocidad de integración: 2,9 PRs/semana
   - Defectos abiertos: 0
   - Cobertura E2E: 50% (4/8 flujos)
3. Mostrar la evolución de commits (762 en 74 días)
4. Mostrar los 4 sprints (S0-S3)
5. Mostrar el equipo (2 devs, 50,5/49,5%)

---

## 10. Mostrar código y arquitectura (opcional, 2 min)

Si hay tiempo, mostrar la arquitectura:

1. Abrir el repo de documentación: https://github.com/marnafer/AlquilER-documentacion
2. Mostrar `ARQUITECTURA.md` con los diagramas de capas
3. Mostrar `ERD.md` con el diagrama entidad-relación (17 tablas + 1 vista)
4. Mostrar `WIREFRAMES.md` con los 39 wireframes
5. Mostrar la estructura de carpetas del backend (`src/`)
6. Mostrar un controlador y un servicio como ejemplo

---

## 11. Cambiar de cuenta durante la demo

Durante la demo necesitás cambiar entre usuarios:

| Paso | Acción |
|---|---|
| 1 | Login como usuario normal (para reservar, consultar, favoritos) |
| 2 | Logout |
| 3 | Login como propietario (para confirmar/rechazar/finalizar reservas) |
| 4 | Logout |
| 5 | Login como admin (prop@gmail.com) para el panel admin |

> **Tip**: Tené las credenciales anotadas para cambiar rápidamente.

---

## 12. Notas

- El demo debe durar aproximadamente 10 minutos.
- Tener las credenciales listas antes de empezar.
- Verificar que la app esté online antes de la demo.
- El dashboard (https://github.com/JulianMP1997/alquiler-dashboard) tiene un script de presentación de 5 minutos.
- La documentación completa está en el repo de docs (36 archivos).
- Si algo falla, usar el Plan B (sección 8).
- Si hay tiempo extra, mostrar métricas (sección 9) o código (sección 10).
