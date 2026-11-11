# Diagrama Entidad-Relación — AlquilER

> Generado a partir del esquema de la base de datos `sistema_alquiler_db`.

## Diagrama (Mermaid)

```mermaid
erDiagram
    roles {
        int id PK
        varchar nombre
        timestamp deleted_at
    }

    usuarios {
        int id PK
        varchar nombre
        varchar apellido
        varchar email
        varchar telefono
        varchar domicilio
        varchar contrasena
        int rol_id FK
        timestamp email_verified_at
        varchar email_verification_token
        timestamp email_verification_expires_at
        timestamp deleted_at
    }

    categorias {
        int id PK
        varchar nombre
        timestamp deleted_at
    }

    provincias {
        int id PK
        varchar nombre
        timestamp deleted_at
    }

    localidades {
        int id PK
        varchar nombre
        varchar codigo_postal
        int provincia_id FK
        timestamp deleted_at
    }

    servicios {
        int id PK
        varchar nombre
        timestamp deleted_at
    }

    propiedades {
        int id PK
        varchar titulo
        text descripcion
        decimal precio
        decimal expensas
        varchar direccion
        tinyint cantidad_ambientes
        tinyint cantidad_dormitorios
        tinyint cantidad_banos
        tinyint capacidad
        tinyint acepta_mascotas
        tinyint acepta_hijos
        tinyint disponible
        tinyint destacada
        int categoria_id FK
        int usuario_id FK
        int localidad_id FK
        timestamp deleted_at
        datetime fecha_publicacion
        longtext requisitos_interesados
    }

    propiedad_imagenes {
        int id PK
        int propiedad_id FK
        varchar ruta
        varchar descripcion
        tinyint es_principal
    }

    propiedad_servicio {
        int id PK
        int propiedad_id FK
        int servicio_id FK
    }

    reservas {
        int id PK
        int propiedad_id FK
        int usuario_id FK
        date fecha_inicio_alquiler
        date fecha_fin_alquiler
        enum estado
        timestamp fecha_reserva
        timestamp fecha_confirmacion
        timestamp deleted_at
    }

    consultas {
        int id PK
        int propiedad_id FK
        int usuario_id FK
        longtext perfil_interesado
        timestamp fecha_consulta
        timestamp deleted_at
    }

    mensajes_consultas {
        int id PK
        int consulta_id FK
        int usuario_id FK
        text mensaje
        timestamp fecha_mensaje
        timestamp deleted_at
    }

    favoritos {
        int id PK
        int usuario_id FK
        int propiedad_id FK
    }

    resenas {
        int id PK
        int reserva_id FK
        enum tipo
        int calificador_id FK
        tinyint calificacion
        text comentario
        timestamp fecha_publicacion
        timestamp deleted_at
    }

    notificaciones {
        int id PK
        int usuario_id FK
        varchar tipo
        varchar titulo
        text mensaje
        int referencia_id
        tinyint leida
        timestamp fecha_notificacion
        timestamp deleted_at
    }

    logs_actividad {
        int id PK
        int usuario_id FK
        varchar accion
        varchar ip_address
        timestamp fecha
    }

    password_resets {
        int id PK
        varchar email
        varchar token
        datetime expiracion
        tinyint usado
        varchar contrasena_temporal
    }

    refresh_tokens {
        int id PK
        int usuario_id FK
        varchar token
        datetime expires_at
        timestamp created_at
    }

    %% Relaciones
    roles ||--o{ usuarios : "tiene"
    usuarios ||--o{ propiedades : "publica"
    usuarios ||--o{ reservas : "realiza"
    usuarios ||--o{ consultas : "realiza"
    usuarios ||--o{ favoritos : "guarda"
    usuarios ||--o{ notificaciones : "recibe"
    usuarios ||--o{ logs_actividad : "genera"
    usuarios ||--o{ refresh_tokens : "tiene"
    usuarios ||--o{ mensajes_consultas : "envia"
    usuarios ||--o{ resenas : "califica (calificador_id)"

    categorias ||--o{ propiedades : "clasifica"
    provincias ||--o{ localidades : "contiene"
    localidades ||--o{ propiedades : "ubicada en"

    propiedades ||--o{ propiedad_imagenes : "tiene"
    propiedades ||--o{ propiedad_servicio : "ofrece"
    propiedades ||--o{ reservas : "es reservada"
    propiedades ||--o{ consultas : "recibe"
    propiedades ||--o{ favoritos : "es favorita de"

    servicios ||--o{ propiedad_servicio : "asociado a"

    reservas ||--o{ resenas : "genera"
    consultas ||--o{ mensajes_consultas : "tiene"
```

## Relaciones principales

| Entidad A | Relación | Entidad B | Descripción |
|---|---|---|---|
| `roles` | 1:N | `usuarios` | Un rol tiene muchos usuarios |
| `usuarios` | 1:N | `propiedades` | Un usuario publica muchas propiedades |
| `usuarios` | 1:N | `reservas` | Un usuario (inquilino) realiza reservas |
| `usuarios` | 1:N | `consultas` | Un usuario realiza consultas |
| `usuarios` | 1:N | `favoritos` | Un usuario guarda favoritos |
| `usuarios` | 1:N | `notificaciones` | Un usuario recibe notificaciones |
| `usuarios` | 1:N | `logs_actividad` | Un usuario genera logs |
| `usuarios` | 1:N | `refresh_tokens` | Un usuario tiene refresh tokens |
| `usuarios` | 1:N | `mensajes_consultas` | Un usuario envía mensajes |
| `usuarios` | 1:N | `resenas` | Un usuario califica (calificador_id) |
| `categorias` | 1:N | `propiedades` | Una categoría clasifica propiedades |
| `provincias` | 1:N | `localidades` | Una provincia contiene localidades |
| `localidades` | 1:N | `propiedades` | Una localidad tiene propiedades |
| `propiedades` | 1:N | `propiedad_imagenes` | Una propiedad tiene imágenes |
| `propiedades` | N:M | `servicios` (via `propiedad_servicio`) | Propiedades ofrecen servicios |
| `propiedades` | 1:N | `reservas` | Una propiedad es reservada |
| `propiedades` | 1:N | `consultas` | Una propiedad recibe consultas |
| `propiedades` | 1:N | `favoritos` | Una propiedad es favorita |
| `reservas` | 1:N | `resenas` | Una reserva genera reseñas |
| `consultas` | 1:N | `mensajes_consultas` | Una consulta tiene mensajes |

## Notas

- Todas las tablas principales usan **soft delete** (`deleted_at` nullable) excepto `favoritos`, `password_resets`, `refresh_tokens`, `propiedad_servicio` y `mensajes_consultas` (estas últimas también tienen `deleted_at`).
- `resenas.tipo` es un enum: `'propiedad'` (reseña de la propiedad por el inquilino) o `'inquilino'` (reseña del inquilino por el propietario).
- `propiedad_servicio` es una tabla pivote N:M entre `propiedades` y `servicios`.
- `favoritos` es una tabla N:M entre `usuarios` y `propiedades`.
- `notificaciones.referencia_id` es una referencia polimórfica (puede apuntar a reservas, consultas, etc. según el `tipo`).
- `password_resets` y `refresh_tokens` no tienen FK a `usuarios` (se vinculan por email/usuario_id respectivamente, pero sin constraint).
- `logs_actividad` registra acciones de usuarios (login, registro, etc.) con IP y timestamp.
