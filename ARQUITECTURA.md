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
    R[Routes<br/>src/Routes/*.php] --> Mw[Middlewares<br/>Auth, CSRF, CORS, RateLimit]
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
| **Middlewares** | Autenticación (JWT), CSRF (Origin), CORS, rate limiting, logging. |
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
    Req[Request HTTP] --> CORS{CORS<br/>Origin permitido?}
    CORS -->|No| R1[403 Forbidden]
    CORS -->|Sí| RL{Rate Limit<br/>dentro del límite?}
    RL -->|No| R2[429 Too Many Requests]
    RL -->|Sí| Auth{Autenticado?<br/>JWT válido?}
    Auth -->|No y ruta protegida| R3[401 Unauthorized]
    Auth -->|Sí o ruta pública| CSRF{CSRF<br/>Origin válido?}
    CSRF -->|No y método write| R4[403 Forbidden]
    CSRF -->|Sí| Ctrl[Controller]
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
