# Arquitectura del Sistema — AlquilER

> Documento de arquitectura: componentes, capas, flujos y despliegue del sistema AlquilER.

## 1. Visión general

AlquilER es un sistema de alquiler de propiedades con arquitectura **cliente-servidor**:

- **Frontend**: SPA en React + Vite (TypeScript), servida como sitio estático.
- **Backend**: API REST en PHP 8.2 (sin framework, arquitectura en capas), corriendo sobre Apache/Nginx + MySQL 8.
- **Comunicación**: HTTP/JSON con cookies HttpOnly para autenticación.

## 2. Diagrama de arquitectura de alto nivel

```mermaid
flowchart LR
    U[Usuario / Navegador] -->|HTTPS| F[Frontend SPA<br/>React + Vite<br/>alquilerr.alwaysdata.net]
    F -->|/api/* JSON + cookies| B[Backend API<br/>PHP 8.2<br/>alquiler.alwaysdata.net]
    B -->|PDO / Eloquent| D[(MySQL 8<br/>sistema_alquiler_db)]
    B -->|SMTP| M[Servidor de email<br/>Resend / SMTP]
    B -->|Filesystem| S[storage/<br/>uploads, logs, cache]
    F -.->|assets estáticos| CDN[CDN / Hosting estático]
```

## 3. Arquitectura en capas del backend

El backend sigue el patrón **Controller → Service → Repository → Model**:

```mermaid
flowchart TD
    R[Routes<br/>src/Routes/*.php] --> Mw[Middlewares<br/>CookieCsrf, RateLimit, Debug]
    Mw --> C[Controllers<br/>src/Controllers/Api/*.php]
    C --> V[Validators<br/>src/Validators/*.php]
    C --> S[Services<br/>src/Services/*.php]
    S --> P[Policies<br/>src/Policies/*.php]
    S --> Repo[Repositories<br/>src/Repositories/*.php]
    Repo --> Model[Models<br/>src/Models/*.php]
    Model --> DB[(MySQL)]
    S --> Ext[Servicios externos<br/>MailService, Cache, Security]
```

### Responsabilidades por capa

| Capa | Responsabilidad |
|---|---|
| **Routes** | Mapeo de URLs a controladores y middlewares. |
| **Middlewares** | CSRF (Origin/cookies), rate limiting, debug (solo dev). La autenticación JWT la maneja `Authenticator` en `src/Security/`. |
| **Controllers** | Recibir request, validar entrada, llamar al servicio, formatear respuesta HTTP. |
| **Validators** | Validación de campos (formato, tipo, rangos, obligatoriedad). |
| **Services** | Lógica de negocio, orquestación, reglas de dominio. |
| **Policies** | Autorización (¿puede este usuario hacer esta acción?). |
| **Repositories** | Acceso a datos (queries, persistencia). |
| **Models** | Entidades de dominio (Eloquent ORM). |

## 4. Estructura de directorios del backend

```
AlquilER/
├── public/
│   ├── index.php          # Front controller
│   ├── router-dev.php     # Router para PHP built-in server
│   └── .htaccess          # Rewrite rules
├── src/
│   ├── Cache/             # FileCache
│   ├── Controllers/Api/   # Controladores REST
│   ├── Debug/             # Herramientas de debug
│   ├── Docs/              # Documentación interna
│   ├── Exceptions/        # Excepciones personalizadas
│   ├── Helpers/           # Helpers (ResponseHelper, etc.)
│   ├── Http/              # Request/Response
│   ├── Middlewares/       # Middlewares HTTP
│   ├── Models/            # Modelos Eloquent
│   ├── Performance/       # Instrumentación
│   ├── Policies/          # Autorización
│   ├── Repositories/      # Acceso a datos
│   ├── Routes/            # Definición de rutas
│   ├── Sanitizers/        # Sanitización de inputs
│   ├── Security/          # CSRF, rate limiting, headers
│   ├── Services/          # Lógica de negocio
│   └── Validators/        # Validación de campos
├── config/
│   ├── config.php         # Configuración principal
│   └── database.php       # Conexión BD
├── scripts/
│   ├── migrations/        # Migraciones SQL
│   └── seeders/           # Seeders de datos
├── storage/
│   ├── uploads/           # Imágenes subidas
│   ├── cache/             # Cache de catálogos
│   ├── logs/              # Logs de la app
│   └── rate_limit/        # Contadores de rate limit
└── .env                   # Variables de entorno
```

## 5. Arquitectura del frontend

```mermaid
flowchart TD
    E[Entry: main.tsx] --> R[Router: App.tsx<br/>React Router]
    R --> L[Layout<br/>Header, Footer, Main]
    L --> P[Pages<br/>Home, Catalogo, Detalle, Perfil, Admin...]
    P --> C[Components<br/>reutilizables]
    P --> H[Hooks<br/>useAuth, useApi, useTheme...]
    H --> API[API Layer<br/>fetch + cookies]
    C --> CTX[Contexts<br/>Auth, Theme, Toast]
    CTX --> API
```

### Estructura de directorios del frontend

```
sistema-alquiler-frontend-web/
├── src/
│   ├── api/               # Llamadas a la API backend
│   ├── components/        # Componentes reutilizables
│   ├── contexts/          # Contextos (Auth, Theme, Toast)
│   ├── hooks/             # Custom hooks
│   ├── pages/             # Páginas (rutas)
│   ├── styles/            # CSS (base, responsive, tokens)
│   ├── types/             # Tipos TypeScript
│   ├── utils/             # Utilidades
│   └── assets/            # Imágenes, iconos
├── public/
│   ├── .htaccess          # SPA fallback + charset
│   └── ...
├── vite.config.ts         # Configuración de Vite
└── package.json
```

## 6. Flujo de autenticación

```mermaid
sequenceDiagram
    participant U as Usuario
    participant F as Frontend
    participant B as Backend
    participant DB as MySQL

    U->>F: Ingresar email + contraseña
    F->>B: POST /api/auth/login
    B->>DB: Buscar usuario por email
    DB-->>B: Usuario (con hash de contraseña)
    B->>B: Verificar contraseña (bcrypt)
    B->>B: Generar access token (15 min) + refresh token (15 días)
    B->>DB: Guardar refresh token
    B-->>F: 200 OK + Set-Cookie (access, refresh) HttpOnly
    F-->>U: Redirigir a dashboard

    Note over F,B: Requests posteriores incluyen cookies automáticamente

    F->>B: GET /api/propiedades (con cookies)
    B->>B: Validar access token (JWT)
    alt Token válido
        B-->>F: 200 OK + datos
    else Token expirado
        F->>B: POST /api/auth/refresh (con refresh cookie)
        B->>DB: Validar refresh token
        B-->>F: 200 OK + nuevo access token (cookie)
        F->>B: GET /api/propiedades (reintentar)
        B-->>F: 200 OK + datos
    end
```

## 7. Flujo de una reserva

```mermaid
sequenceDiagram
    participant I as Inquilino
    participant F as Frontend
    participant B as Backend
    participant P as Propietario

    I->>F: Seleccionar propiedad + fechas
    F->>B: POST /api/reservas
    B->>B: Validar fechas, propiedad disponible
    B->>B: Crear reserva (estado: pendiente)
    B->>B: Notificar al propietario
    B-->>F: 201 Created
    F-->>I: Reserva creada

    P->>F: Ver reserva pendiente
    F->>B: PUT /api/reservas/{id}/confirmar
    B->>B: Verificar que es el propietario
    B->>B: Cambiar estado a confirmada
    B->>B: Notificar al inquilino
    B-->>F: 200 OK

    Note over I,P: Al finalizar el alquiler

    P->>F: Finalizar reserva
    F->>B: PUT /api/reservas/{id}/finalizar
    B->>B: Cambiar estado a finalizada
    B-->>F: 200 OK

    I->>F: Crear reseña
    F->>B: POST /api/resenas
    B->>B: Verificar reserva finalizada
    B-->>F: 201 Created
```

## 8. Despliegue (Deployment)

```mermaid
flowchart TD
    subgraph Producción
        A[alquilerr.alwaysdata.net<br/>Frontend estático] -->|/api/*<|end|> B[alquiler.alwaysdata.net<br/>Backend PHP]
        B --> C[(MySQL alwaysdata)]
        B --> D[storage/<br/>uploads, logs, cache]
    end

    subgraph Desarrollo local
        E[localhost:3000<br/>Vite dev server] -->|proxy /api| F[127.0.0.1:8000<br/>PHP built-in server]
        F --> G[(MySQL local<br/>sistema_alquiler_db)]
    end

    subgraph Repositorios
        H[GitHub: AlquilER-frontend-web] -->|build + deploy| A
        I[GitHub: AlquilER-backend] -->|deploy manual FTP| B
    end
```

## 9. Patrones de diseño utilizados

| Patrón | Dónde | Descripción |
|---|---|---|
| **Repository** | `src/Repositories/` | Abstracción del acceso a datos (interfaces). |
| **Service Layer** | `src/Services/` | Lógica de negocio desacoplada de HTTP. |
| **Middleware** | `src/Middlewares/` | Pipeline de procesamiento de requests. |
| **Policy** | `src/Policies/` | Autorización basada en roles y ownership. |
| **DTO / Validator** | `src/Validators/` | Validación de entrada desacoplada. |
| **Factory** | `src/Helpers/` | Creación de respuestas HTTP estandarizadas. |
| **Singleton** | `config/database.php` | Conexión única a la base de datos. |
| **Observer (notificaciones)** | `src/Services/NotificacionService.php` | Generación automática de notificaciones por eventos. |

## 10. Seguridad en la arquitectura

```mermaid
flowchart TD
    Req[Request HTTP] --> RL{Rate Limit<br/>dentro del límite?}
    RL -->|No| R2[429 Too Many Requests]
    RL -->|Sí| CSRF{CSRF<br/>Origin/Cookie válido?}
    CSRF -->|No y método write| R4[403 Forbidden]
    CSRF -->|Sí| Auth{Autenticado?<br/>JWT válido?}
    Auth -->|No y ruta protegida| R3[401 Unauthorized]
    Auth -->|Sí o ruta pública| Ctrl[Controller]
    Ctrl --> Policy{Policy<br/>Autorizado?}
    Policy -->|No| R5[403 Forbidden]
    Policy -->|Sí| Svc[Service → Repository → DB]
```

## 11. Cache y performance

```mermaid
flowchart LR
    Req[Request catálogo] --> Cache{FileCache<br/>hit?}
    Cache -->|Hit| Resp[Respuesta cacheada]
    Cache -->|Miss| DB[(MySQL query)]
    DB --> Proc[Procesar + serializar]
    Proc --> Save[Guardar en cache<br/>TTL configurable]
    Save --> Resp
```

- **FileCache**: cache de catálogos de baja rotación (categorías, provincias, localidades, servicios).
- **Instrumentación**: `PerformanceMiddleware` registra tiempos de respuesta en `storage/logs/performance.jsonl`.
- **Rotación de logs**: 5MB por archivo, 3 generaciones.
- **Eloquent eager loading**: `with()` para evitar N+1 en listados con imágenes y servicios.

---

## 12. Máquina de estados: Reservas

```mermaid
stateDiagram-v2
    [*] --> Pendiente: Usuario crea reserva
    Pendiente --> Confirmada: Propietario confirma
    Pendiente --> Rechazada: Propietario rechaza
    Pendiente --> Cancelada: Inquilino cancela
    Confirmada --> Finalizada: Propietario finaliza
    Confirmada --> Cancelada: Inquilino cancela
    Rechazada --> [*]
    Cancelada --> [*]
    Finalizada --> [*]
```

### Transiciones válidas

| Estado actual | Acción | Nuevo estado | Quién ejecuta |
|---|---|---|---|
| Pendiente | Confirmar | Confirmada | Propietario |
| Pendiente | Rechazar | Rechazada | Propietario |
| Pendiente | Cancelar | Cancelada | Inquilino |
| Confirmada | Finalizar | Finalizada | Propietario |
| Confirmada | Cancelar | Cancelada | Inquilino |

> Cada transición genera una notificación al inquilino.

## 13. Flujo de publicación de propiedad

```mermaid
sequenceDiagram
    participant U as Usuario
    participant F as Frontend
    participant B as Backend
    participant S as Storage (uploads)

    U->>F: Completar formulario de propiedad
    F->>B: POST /api/propiedades (datos)
    B->>B: Validar datos (PropiedadValidator)
    B->>B: Verificar permisos (Policy)
    B->>B: Crear propiedad (estado: disponible)
    B-->>F: 201 Created + propiedad_id

    U->>F: Subir imágenes
    F->>B: POST /api/propiedades/{id}/imagenes (multipart)
    B->>B: Validar imagen (tipo, tamaño)
    B->>S: Guardar archivo original
    B->>S: Generar thumbnail, mediana, grande
    B->>B: Crear registro en BD (imagen_propiedad)
    B-->>F: 201 Created + URLs

    U->>F: Marcar imagen principal
    F->>B: PUT /api/propiedades/{id}/imagenes/{img}/principal
    B->>B: Desmarcar anterior principal
    B->>B: Marcar nueva principal
    B-->>F: 200 OK

    U->>F: Asociar servicios
    F->>B: POST /api/propiedades/{id}/servicios
    B->>B: Validar servicios
    B->>B: Crear registros en pivote
    B-->>F: 200 OK

    F-->>U: Propiedad publicada
```

## 14. Flujo de notificaciones

```mermaid
flowchart TD
    E[Evento del sistema] --> D{Tipo de evento}
    D -->|Nueva reserva| N1[Notificar al propietario]
    D -->|Reserva confirmada/rechazada/cancelada/finalizada| N2[Notificar al inquilino]
    D -->|Nueva consulta| N3[Notificar al propietario]
    D -->|Respuesta a consulta| N4[Notificar al interesado]
    D -->|Nueva reseña| N5[Notificar al reseñado]
    N1 --> S[NotificacionService::crear()]
    N2 --> S
    N3 --> S
    N4 --> S
    N5 --> S
    S --> DB[(Tabla notificaciones)]
    S --> C[Incrementar contador no leídas]
    DB --> F[Frontend consulta notificaciones]
    F --> U[Usuario ve notificación]
```

## 15. Pipeline de despliegue

```mermaid
flowchart LR
    subgraph Frontend
        A[git push dev] --> B[GitHub Actions / manual]
        B --> C[npm run build]
        C --> D[dist/ generado]
        D --> E[Deploy a alwaysdata<br/>FTP / rsync]
        E --> F[alquilerr.alwaysdata.net]
    end

    subgraph Backend
        G[git push dev] --> H[Deploy manual FTP]
        H --> I[Ejecutar migraciones SQL]
        I --> J[Verificar .env de producción]
        J --> K[alquiler.alwaysdata.net]
    end

    subgraph Verificación
        F --> L[Smoke test: login, catálogo, detalle]
        K --> L
        L --> M{OK?}
        M -->|Sí| N[Deploy completo]
        M -->|No| O[Rollback / fix]
    end
```

## 16. Diagrama de componentes del frontend (detalle)

```mermaid
flowchart TD
    subgraph Pages
        P1[Home]
        P2[Catalogo]
        P3[DetallePropiedad]
        P4[Perfil]
        P5[MisPropiedades]
        P6[Admin*]
        P7[Login / Registro]
    end

    subgraph Shared Components
        C1[Header]
        C2[Footer]
        C3[PropertyCard]
        C4[Modal]
        C5[Toast]
        C6[Skeleton]
        C7[EmptyState]
        C8[ErrorState]
    end

    subgraph Contexts
        X1[AuthContext]
        X2[ThemeContext]
        X3[ToastContext]
    end

    subgraph Hooks
        H1[useAuth]
        H2[useApi]
        H3[useTheme]
        H4[useDebounce]
    end

    subgraph API Layer
        A1[authApi]
        A2[propiedadesApi]
        A3[reservasApi]
        A4[consultasApi]
        A5[resenasApi]
    end

    P1 --> C1 & C2 & C3
    P2 --> C1 & C2 & C3 & C6 & C7 & C8
    P3 --> C1 & C2 & C4
    X1 --> H1
    H1 --> A1
    H2 --> A2 & A3 & A4 & A5
    X2 --> H3
    X3 --> C5
```

---

## 17. Diagrama de flujo de datos (DFD) — Nivel 1

```mermaid
flowchart LR
    U[Usuario] -->|Credenciales, datos de propiedad, reservas| F[Frontend SPA]
    F -->|JSON + cookies| B[Backend API]
    B -->|Queries SQL| D[(MySQL)]
    B -->|Archivos| S[Storage<br/>uploads, logs, cache]
    B -->|Emails<|end|> M[Servidor SMTP]
    D -->|Datos| B
    S -->|Archivos| B
    M -->|Confirmación| B
    B -->|JSON responses| F
    F -->|HTML/CSS/JS<|end|> U
```

### Flujos de datos principales

| Flujo | Origen | Destino | Datos |
|---|---|---|---|
| Autenticación | Usuario → Frontend → Backend → BD | Tokens en cookies | email, contraseña |
| Publicación de propiedad | Usuario → Frontend → Backend → BD + Storage | Propiedad + imágenes | título, precio, fotos, servicios |
| Reserva | Inquilino → Frontend → Backend → BD | Reserva + notificación | fechas, propiedad_id |
| Búsqueda | Usuario → Frontend → Backend → BD (cache) | Listado paginado | filtros, orden, página |
| Consulta | Usuario → Frontend → Backend → BD | Consulta + notificación | mensaje, propiedad_id |
| Reseña | Usuario → Frontend → Backend → BD | Reseña + notificación | calificación, comentario |

## 18. Pipeline de procesamiento de imágenes

```mermaid
flowchart TD
    U[Usuario sube imagen] --> V{Validación<br/>tipo, tamaño, dimensiones}
    V -->|Error| E[Rechazar con mensaje]
    V -->|OK| O[Guardar original<br/>storage/uploads/original/]
    O --> R1[Generar thumbnail<br/>150x150, crop center]
    O --> R2[Generar mediana<br/>400x300, fit]
    O --> R3[Generar grande<br/>1200x900, fit]
    R1 --> S1[storage/uploads/thumb/]
    R2 --> S2[storage/uploads/medium/]
    R3 --> S3[storage/uploads/large/]
    S1 --> DB[Guardar registro en BD<br/>imagen_propiedad]
    S2 --> DB
    S3 --> DB
    DB --> CDN[Servir vía URL pública]
```

### Formatos y tamaños soportados

| Tamaño | Dimensiones | Uso |
|---|---|---|
| Thumbnail | 150×150 | Listados, carruseles |
| Mediana | 400×300 | Cards de catálogo |
| Grande | 1200×900 | Detalle de propiedad |
| Original | Sin modificar | Backup, descarga |

## 19. Flujo de manejo de errores

```mermaid
flowchart TD
    Req[Request] --> Ctrl[Controller]
    Ctrl --> Val{Validación OK?}
    Val -->|No| VErr[422 Validation Error<br/>detalles por campo]
    Val -->|Sí| Svc[Service]
    Svc --> Biz{Regla de negocio OK?}
    Biz -->|No| BErr[400/403/409 Business Error<br/>mensaje descriptivo]
    Biz -->|Sí| Repo[Repository]
    Repo --> DB{Query OK?}
    DB -->|No| DBErr[500 Database Error<br/>log interno, mensaje genérico]
    DB -->|Sí| OK[200/201 Success]
    Ctrl -->|Excepción no capturada| Global[Global Exception Handler]
    Global --> Log[Log en storage/logs/errores.log]
    Global --> Resp[500 Internal Server Error<br/>mensaje genérico al cliente]
```

### Tipos de error y códigos HTTP

| Código | Tipo | Cuándo |
|---|---|---|
| 400 | Bad Request | Datos malformados |
| 401 | Unauthorized | No autenticado o token inválido |
| 403 | Forbidden | Autenticado pero sin permisos |
| 404 | Not Found | Recurso no existe |
| 409 | Conflict | Conflicto de estado (ej: email duplicado) |
| 422 | Unprocessable Entity | Validación de campos fallida |
| 429 | Too Many Requests | Rate limit excedido |
| 500 | Internal Server Error | Error inesperado del servidor |

## 20. Modelo de dominio (Class Diagram)

```mermaid
classDiagram
    class Usuario {
        +int id
        +string nombre
        +string apellido
        +string email
        +string telefono
        +string domicilio
        +string contrasena
        +int rol_id
        +timestamp email_verified_at
        +publicarPropiedad()
        +reservar()
        +favorito()
    }

    class Propiedad {
        +int id
        +string titulo
        +string descripcion
        +decimal precio
        +decimal expensas
        +string direccion
        +int categoria_id
        +int localidad_id
        +int propietario_id
        +bool disponible
        +bool destacada
        +agregarImagen()
        +agregarServicio()
    }

    class Reserva {
        +int id
        +int propiedad_id
        +int inquilino_id
        +date fecha_inicio
        +date fecha_fin
        +enum estado
        +confirmar()
        +rechazar()
        +cancelar()
        +finalizar()
    }

    class Resena {
        +int id
        +int reserva_id
        +int autor_id
        +int destinatario_id
        +enum tipo
        +int calificacion
        +string comentario
    }

    class Consulta {
        +int id
        +int propiedad_id
        +int usuario_id
        +string mensaje
        +responder()
    }

    class Notificacion {
        +int id
        +int usuario_id
        +string tipo
        +string mensaje
        +bool leida
        +marcarLeida()
    }

    class Favorito {
        +int id
        +int usuario_id
        +int propiedad_id
    }

    class ImagenPropiedad {
        +int id
        +int propiedad_id
        +string url
        +bool es_principal
    }

    Usuario "1" --> "*" Propiedad : publica
    Usuario "1" --> "*" Reserva : realiza
    Usuario "1" --> "*" Consulta : crea
    Usuario "1" --> "*" Resena : escribe
    Usuario "1" --> "*" Notificacion : recibe
    Usuario "1" --> "*" Favorito : tiene
    Propiedad "1" --> "*" ImagenPropiedad : tiene
    Propiedad "1" --> "*" Reserva : recibe
    Propiedad "1" --> "*" Consulta : recibe
    Reserva "1" --> "*" Resena : genera
    Reserva "1" --> "1" Propiedad : es de
```

---

## 21. Matriz de roles y permisos (RBAC)

```mermaid
flowchart TD
    subgraph Visitante["Visitante (no autenticado)"]
        V1[Ver catálogo de propiedades]
        V2[Ver detalle de propiedad]
        V3[Ver servicios, categorías]
        V4[Enviar mensaje de contacto]
        V5[Ver FAQ]
    end

    subgraph Usuario["Usuario (rol_id=1)"]
        U1[Todo lo del Visitante]
        U2[Registrarse y verificar email]
        U3[Iniciar/cerrar sesión]
        U4[Publicar propiedades]
        U5[Reservar propiedades]
        U6[Agregar/quitar favoritos]
        U7[Consultar sobre propiedades]
        U8[Responder consultas (si es propietario)]
        U9[Crear reseñas (post-reserva finalizada)]
        U10[Editar perfil y contraseña]
        U11[Ver notificaciones]
    end

    subgraph Propietario["Propietario (implícito)"]
        P1[Todo lo del Usuario]
        P2[Editar/eliminar sus propiedades]
        P3[Confirmar/rechazar reservas]
        P4[Finalizar reservas]
        P5[Responder consultas de sus propiedades]
        P6[Reseñar inquilinos]
        P7[Marcar imagen principal]
    end

    subgraph Admin["Administrador (rol_id=2)"]
        A1[Todo lo del Usuario]
        A2[Panel de administración]
        A3[Gestionar usuarios (CRUD + restore)]
        A4[Gestionar categorías, provincias, localidades, servicios, roles]
        A5[Gestionar propiedades (todas, incluyendo eliminadas)]
        A6[Marcar propiedades como destacadas]
        A7[Gestionar reservas, consultas, reseñas]
        A8[Ver y restaurar registros eliminados (papelera)]
        A9[Ver logs de actividad]
        A10[Restaurar registros soft-deleted]
    end
```

### Tabla de permisos por recurso

| Recurso | Visitante | Usuario | Propietario | Admin |
|---|---|---|---|---|
| Ver catálogo | ✅ | ✅ | ✅ | ✅ |
| Ver detalle propiedad | ✅ | ✅ | ✅ | ✅ |
| Publicar propiedad | ❌ | ✅ | ✅ | ✅ |
| Editar propiedad propia | ❌ | ❌ | ✅ | ✅ |
| Editar propiedad ajena | ❌ | ❌ | ❌ | ✅ |
| Reservar | ❌ | ✅ | ✅ | ✅ |
| Confirmar/rechazar reserva | ❌ | ❌ | ✅ (suya) | ✅ |
| Cancelar reserva | ❌ | ✅ (propia) | ✅ (propia) | ✅ |
| Finalizar reserva | ❌ | ❌ | ✅ (suya) | ✅ |
| Favoritos | ❌ | ✅ | ✅ | ✅ |
| Consultas | ❌ | ✅ | ✅ | ✅ |
| Reseñas | ❌ | ✅ (post-finalizada) | ✅ | ✅ |
| Panel admin | ❌ | ❌ | ❌ | ✅ |
| Papelera (restore) | ❌ | ❌ | ❌ | ✅ |
| Logs de actividad | ❌ | ❌ | ❌ | ✅ |

## 22. Flujo de búsqueda y filtrado

```mermaid
sequenceDiagram
    participant U as Usuario
    participant F as Frontend
    participant C as FileCache
    participant B as Backend
    participant DB as MySQL

    U->>F: Ingresar filtros (categoría, precio, etc.)
    F->>F: Debounce (300ms)
    F->>B: GET /api/propiedades?categoria_id=1&precio_min=1000&...

    B->>B: Validar filtros (PropiedadValidator)
    B->>C: ¿Catálogos de referencia en cache?
    alt Cache hit
        C-->>B: Categorías, provincias, etc.
    else Cache miss
        B->>DB: SELECT categorías, provincias...
        DB-->>B: Datos
        B->>C: Guardar en cache (TTL)
    end

    B->>DB: SELECT propiedades WHERE filtros + paginación
    DB-->>B: Resultados + total
    B->>B: Serializar + incluir imágenes principales
    B-->>F: 200 OK {data, meta: {total, page, per_page}}

    F->>F: Renderizar cards + paginación
    F-->>U: Mostrar resultados

    Note over F: Si no hay resultados → EmptyState con sugerencias
```

### Filtros disponibles

| Filtro | Tipo | Descripción |
|---|---|---|
| `categoria_id` | int[] | Categorías (múltiples) |
| `provincia_id` | int | Provincia |
| `localidad_id` | int | Localidad |
| `servicio_id` | int[] | Servicios (múltiples) |
| `acepta_mascotas` | bool | Acepta mascotas |
| `acepta_hijos` | bool | Acepta hijos |
| `precio_min` / `precio_max` | decimal | Rango de precio |
| `cantidad_ambientes` | int | Ambientes |
| `cantidad_dormitorios` | int | Dormitorios |
| `cantidad_banos` | int | Baños |
| `capacidad` | int | Capacidad de personas |
| `page` | int | Página (default: 1) |
| `per_page` | int | Resultados por página (default: 10) |
| `orden` | string | Ordenamiento (recientes, precio_asc, precio_desc) |

## 23. Flujo de consultas y mensajería

```mermaid
sequenceDiagram
    participant I as Interesado
    participant F as Frontend
    participant B as Backend
    participant P as Propietario

    I->>F: Ver detalle de propiedad
    I->>F: Click "Consultar"
    F->>B: POST /api/consultas {propiedad_id, mensaje}
    B->>B: Validar mensaje (no vacío, max 1000 chars)
    B->>B: Crear consulta (estado: abierta)
    B->>B: Notificar al propietario
    B-->>F: 201 Created
    F-->>I: Consulta enviada

    P->>F: Ver notificación de nueva consulta
    P->>F: Abrir consulta
    F->>B: GET /api/consultas/{id}
    B-->>F: Consulta + mensajes
    P->>F: Escribir respuesta
    F->>B: POST /api/consultas/{id}/respuestas {mensaje}
    B->>B: Crear respuesta
    B->>B: Notificar al interesado
    B-->>F: 201 Created

    I->>F: Ver notificación de respuesta
    I->>F: Abrir consulta
    F->>B: GET /api/consultas/{id}
    B-->>F: Consulta + mensajes (bidireccional)
    I->>F: Responder
    F->>B: POST /api/consultas/{id}/respuestas {mensaje}
    B->>B: Crear respuesta
    B->>B: Notificar al propietario
    B-->>F: 201 Created
```

### Reglas de la mensajería

| Regla | Descripción |
|---|---|
| RN-35 | Solo usuarios autenticados pueden consultar. |
| RN-36 | Solo el propietario de la propiedad puede responder. |
| RN-37 | La mensajería es bidireccional (interesado ↔ propietario). |
| RN-38 | Al crear consulta, se notifica al propietario. |
| RN-39 | Al responder, se notifica al interesado. |
| RN-40 | Las consultas usan soft delete. |

## 24. Topología de red e infraestructura

```mermaid
flowchart TD
    subgraph Internet
        DNS[DNS: alquilerr.alwaysdata.net<br/>alquiler.alwaysdata.net]
    end

    subgraph alwaysdata["alwaysdata (Hosting compartido)"]
        subgraph Frontend["Servidor Frontend"]
            Apache1[Apache<br/>Sirve archivos estáticos]
            HT1[.htaccess<br/>SPA fallback + UTF-8]
        end
        subgraph Backend["Servidor Backend"]
            Apache2[Apache + mod_php<br/>PHP 8.2]
            HT2[.htaccess<br/>Rewrite + headers]
            PHP[PHP App<br/>src/ + config/]
            Storage[storage/<br/>uploads, logs, cache]
        end
        subgraph Database["Base de datos"]
            MySQL[(MySQL 8.0<br/>sistema_alquiler_db)]
        end
    end

    subgraph Externo["Servicios externos"]
        Resend[Resend / SMTP<br/>Envío de emails]
        CDN[CDN (opcional)<br/>Assets estáticos]
    end

    DNS --> Apache1
    DNS --> Apache2
    Apache1 --> HT1
    Apache2 --> HT2
    HT2 --> PHP
    PHP --> Storage
    PHP --> MySQL
    PHP --> Resend
    Apache1 -.-> CDN
```

### Puertos y protocolos

| Servicio | Puerto | Protocolo | Descripción |
|---|---|---|---|
| Frontend (producción) | 443 | HTTPS | Sitio estático (React build) |
| Backend (producción) | 443 | HTTPS | API REST (PHP) |
| MySQL (producción) | 3306 | TCP | Base de datos (interna) |
| Frontend (desarrollo) | 3000 | HTTP | Vite dev server |
| Backend (desarrollo) | 8000 | HTTP | PHP built-in server |
| MySQL (desarrollo) | 3306 | TCP | MySQL local |

### Variables de entorno por entorno

| Variable | Desarrollo | Producción |
|---|---|---|
| `APP_ENV` | `development` | `production` |
| `APP_URL` | `http://localhost:3000` | `https://alquilerr.alwaysdata.net` |
| `API_URL` | `http://127.0.0.1:8000` | `https://alquiler.alwaysdata.net` |
| `DB_HOST` | `127.0.0.1` | `mysql-alquiler.alwaysdata.net` |
| `MAIL_HOST` | (vacío = log local) | `smtp.resend.com` |
| `CORS_ALLOWED_ORIGINS` | `http://localhost:3000` | `https://alquilerr.alwaysdata.net` |
