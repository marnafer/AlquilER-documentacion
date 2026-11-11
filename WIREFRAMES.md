# Wireframes — AlquilER

> Wireframes de baja fidelidad de las pantallas principales del sistema AlquilER.
> Formato: ASCII para layout + Mermaid para flujos de navegación.

---

## 1. Home / Catálogo principal

```text
┌──────────────────────────────────────────────────────────────┐
│  [Logo] AlquilER          [Buscar...]   [Login] [Registrarse]│
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────────────────────────────────────────────┐    │
│  │  🔍 Buscar propiedades...                            │    │
│  │  [Categoría ▾] [Provincia ▾] [Precio min] [Precio max]│    │
│  │  [Mascotas ☐] [Hijos ☐] [Ambientes ▾] [Buscar]      │    │
│  └──────────────────────────────────────────────────────┘    │
│                                                              │
│  ★ Propiedades destacadas                                    │
│  ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐              │
│  │ [img]  │ │ [img]  │ │ [img]  │ │ [img]  │              │
│  │ Título │ │ Título │ │ Título │ │ Título │              │
│  │ $1500  │ │ $2000  │ │ $1200  │ │ $1800  │              │
│  │ ★★★★☆ │ │ ★★★★★ │ │ ★★★☆☆ │ │ ★★★★☆ │              │
│  └────────┘ └────────┘ └────────┘ └────────┘              │
│                                                              │
│  Todas las propiedades                                        │
│  ┌────────┐ ┌────────┐ ┌────────┐                           │
│  │ [img]  │ │ [img]  │ │ [img]  │                           │
│  │ Título │ │ Título │ │ Título │                           │
│  │ $1500  │ │ $2000  │ │ $1200  │                           │
│  │ 2 hab  │ │ 3 hab  │ │ 1 hab  │                           │
│  └────────┘ └────────┘ └────────┘                           │
│                                                              │
│  [← Anterior]  Página 1 de 5  [Siguiente →]                │
│                                                              │
│  ────────────────────────────────────────────────────────     │
│  Footer: © AlquilER | Contacto | FAQ | Términos             │
└──────────────────────────────────────────────────────────────┘
```

### Flujo de navegación

```mermaid
flowchart TD
    Home[Home] --> Detalle[Detalle de propiedad]
    Home --> Login[Login]
    Home --> Registro[Registro]
    Home --> Catalogo[Catálogo filtrado]
    Catalogo --> Detalle
    Detalle --> Reservar[Formulario de reserva]
    Detalle --> Favorito[Agregar a favoritos]
    Detalle --> Consultar[Enviar consulta]
```

---

## 2. Detalle de propiedad

```text
┌──────────────────────────────────────────────────────────────┐
│  [Logo] AlquilER          [Buscar...]   [🔔] [👤 Mi perfil] │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  ← Volver al catálogo                                        │
│                                                              │
│  ┌────────────────────────────┐  ┌──────────────────────┐    │
│  │                            │  │ Departamento céntrico │    │
│  │   [Imagen principal]       │  │                      │    │
│  │                            │  │ $1.500 / mes         │    │
│  │  [◀] [img1][img2][img3] [▶]│  │ Expensas: $300       │    │
│  │                            │  │                      │    │
│  └────────────────────────────┘  │ ★★★★☆ 4.5 (12 reseñas)│    │
│                                  │                      │    │
│  ┌────────────────────────────┐  │ 📍 Av. San Martín 1234│    │
│  │ Descripción                │  │    Córdoba, Argentina │    │
│  │                            │  │                      │    │
│  │ Amplio departamento de 2   │  │ 🛏️ 2 dormitorios     │    │
│  │ ambientes con balcón y     │  │ 🚿 1 baño            │    │
│  │ vista al parque...         │  │ 📐 45 m²             │    │
│  │                            │  │ 👥 Capacidad: 3      │    │
│  └────────────────────────────┘  │                      │    │
│                                  │ ✅ Acepta mascotas    │    │
│  ┌────────────────────────────┐  │ ✅ Acepta hijos      │    │
│  │ Servicios                  │  │                      │    │
│  │ ☑ WiFi  ☑ Estacionamiento  │  │ ┌────────────────┐  │    │
│  │ ☑ Pileta  ☑ Aire acond.    │  │ │  [Reservar]    │  │    │
│  │ ☑ Amoblado                 │  │ └────────────────┘  │    │
│  └────────────────────────────┘  │ [♥ Favorito]       │    │
│                                  │ [✉ Consultar]      │    │
│  ┌────────────────────────────┐  └──────────────────────┘    │
│  │ Reseñas                    │                              │
│  │ ★★★★★ "Excelente..." - Juan│                              │
│  │ ★★★★☆ "Muy bueno..." - Ana │                              │
│  │ [Ver todas las reseñas]    │                              │
│  └────────────────────────────┘                              │
└──────────────────────────────────────────────────────────────┘
```

---

## 3. Login

```text
┌──────────────────────────────────────────┐
│                                          │
│           [Logo] AlquilER                │
│                                          │
│  ┌────────────────────────────────────┐  │
│  │         Iniciar sesión             │  │
│  │                                    │  │
│  │  Email                             │  │
│  │  ┌──────────────────────────────┐  │  │
│  │  │ usuario@email.com            │  │  │
│  │  └──────────────────────────────┘  │  │
│  │                                    │  │
│  │  Contraseña                        │  │
│  │  ┌──────────────────────────────┐  │  │
│  │  │ ••••••••                     │  │  │
│  │  └──────────────────────────────┘  │  │
│  │                                    │  │
│  │  [ ] Recordarme                    │  │
│  │                                    │  │
│  │  ┌──────────────────────────────┐  │  │
│  │  │       Iniciar sesión         │  │  │
│  │  └──────────────────────────────┘  │  │
│  │                                    │  │
│  │  ¿Olvidaste tu contraseña?         │  │
│  │                                    │  │
│  │  ¿No tenés cuenta? Registrarse     │  │
│  └────────────────────────────────────┘  │
│                                          │
└──────────────────────────────────────────┘
```

---

## 4. Registro

```text
┌──────────────────────────────────────────┐
│                                          │
│           [Logo] AlquilER                │
│                                          │
│  ┌────────────────────────────────────┐  │
│  │         Crear cuenta               │  │
│  │                                    │  │
│  │  Nombre    │ Apellido              │  │
│  │  ┌──────┐  │ ┌──────────────────┐ │  │
│  │  │      │  │ │                  │ │  │
│  │  └──────┘  │ └──────────────────┘ │  │
│  │                                    │  │
│  │  Email                             │  │
│  │  ┌──────────────────────────────┐  │  │
│  │  │                              │  │  │
│  │  └──────────────────────────────┘  │  │
│  │                                    │  │
│  │  Teléfono        │ Domicilio       │  │
│  │  ┌────────────┐  │ ┌────────────┐ │  │
│  │  │            │  │ │            │ │  │
│  │  └────────────┘  │ └────────────┘ │  │
│  │                                    │  │
│  │  Contraseña                        │  │
│  │  ┌──────────────────────────────┐  │  │
│  │  │ ••••••••                     │  │  │
│  │  └──────────────────────────────┘  │  │
│  │                                    │  │
│  │  Confirmar contraseña              │  │
│  │  ┌──────────────────────────────┐  │  │
│  │  │ ••••••••                     │  │  │
│  │  └──────────────────────────────┘  │  │
│  │                                    │  │
│  │  [ ] Acepto términos y condiciones │  │
│  │                                    │  │
│  │  ┌──────────────────────────────┐  │  │
│  │  │       Registrarse            │  │  │
│  │  └──────────────────────────────┘  │  │
│  │                                    │  │
│  │  ¿Ya tenés cuenta? Iniciar sesión  │  │
│  └────────────────────────────────────┘  │
│                                          │
└──────────────────────────────────────────┘
```

---

## 5. Formulario de reserva

```text
┌──────────────────────────────────────────────┐
│  ← Volver a detalle de propiedad             │
│                                              │
│  ┌────────────────────────────────────────┐  │
│  │       Reservar propiedad               │  │
│  │                                        │  │
│  │  Propiedad: Departamento céntrico      │  │
│  │  Precio: $1.500 / mes                  │  │
│  │                                        │  │
│  │  Fecha de inicio                       │  │
│  │  ┌──────────────────────────────┐      │  │
│  │  │ 📅 2026-05-01                │      │  │
│  │  └──────────────────────────────┘      │  │
│  │                                        │  │
│  │  Fecha de fin                          │  │
│  │  ┌──────────────────────────────┐      │  │
│  │  │ 📅 2026-05-31                │      │  │
│  │  └──────────────────────────────┘      │  │
│  │                                        │  │
│  │  Duración: 30 días                     │  │
│  │  Total estimado: $1.500                │  │
│  │                                        │  │
│  │  Mensaje al propietario (opcional)     │  │
│  │  ┌──────────────────────────────┐      │  │
│  │  │                              │      │  │
│  │  │                              │      │  │
│  │  └──────────────────────────────┘      │  │
│  │                                        │  │
│  │  ┌──────────────────────────────┐      │  │
│  │  │       Enviar reserva         │      │  │
│  │  └──────────────────────────────┘      │  │
│  └────────────────────────────────────────┘  │
└──────────────────────────────────────────────┘
```

---

## 6. Publicar / Editar propiedad

```text
┌──────────────────────────────────────────────────────────────┐
│  [Logo] AlquilER          [🔔] [👤 Mi perfil]               │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  ← Volver a mis propiedades                                  │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐    │
│  │       Publicar nueva propiedad                        │    │
│  │                                                      │    │
│  │  Título                                              │    │
│  │  ┌────────────────────────────────────────────────┐  │    │
│  │  │                                                │  │    │
│  │  └────────────────────────────────────────────────┘  │    │
│  │                                                      │    │
│  │  Descripción                                         │    │
│  │  ┌────────────────────────────────────────────────┐  │    │
│  │  │                                                │  │    │
│  │  │                                                │  │    │
│  │  └────────────────────────────────────────────────┘  │    │
│  │                                                      │    │
│  │  Precio ($/mes)  │  Expensas ($)                    │    │
│  │  ┌────────────┐  │  ┌────────────┐                 │    │
│  │  │            │  │  │            │                 │    │
│  │  └────────────┘  │  └────────────┘                 │    │
│  │                                                      │    │
│  │  Categoría ▾     │  Localidad ▾                    │    │
│  │  ┌────────────┐  │  ┌────────────┐                 │    │
│  │  │            │  │  │            │                 │    │
│  │  └────────────┘  │  └────────────┘                 │    │
│  │                                                      │    │
│  │  Dirección                                           │    │
│  │  ┌────────────────────────────────────────────────┐  │    │
│  │  │                                                │  │    │
│  │  └────────────────────────────────────────────────┘  │    │
│  │                                                      │    │
│  │  Ambientes: [2]  Dormitorios: [1]  Baños: [1]       │    │
│  │  Capacidad: [3]  Superficie: [45] m²                 │    │
│  │                                                      │    │
│  │  ☑ Acepta mascotas   ☑ Acepta hijos                 │    │
│  │                                                      │    │
│  │  Servicios                                           │    │
│  │  ☑ WiFi  ☑ Parking  ☑ Pileta  ☑ Aire acondicionado  │    │
│  │  ☑ Amoblado  ☑ Seguridad 24h                        │    │
│  │                                                      │    │
│  │  ─── Imágenes ───                                    │    │
│  │  ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐        │    │
│  │  │ [img]  │ │ [img]  │ │ [+]    │ │        │        │    │
│  │  │ Principal│ │        │ │ Agregar│ │        │        │    │
│  │  └────────┘ └────────┘ └────────┘ └────────┘        │    │
│  │                                                      │    │
│  │  ┌────────────────────────────────────────────────┐  │    │
│  │  │         Publicar propiedad                    │  │    │
│  │  └────────────────────────────────────────────────┘  │    │
│  └──────────────────────────────────────────────────────┘    │
└──────────────────────────────────────────────────────────────┘
```

---

## 7. Perfil de usuario

```text
┌──────────────────────────────────────────────────────────────┐
│  [Logo] AlquilER          [🔔 3] [👤 Mi perfil]             │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────────────────────────────────────────────┐    │
│  │  👤 Juan Pérez                                        │    │
│  │  juan@email.com                                       │    │
│  │  Miembro desde: Enero 2026                            │    │
│  │  [Editar perfil] [Cambiar contraseña]                 │    │
│  └──────────────────────────────────────────────────────┘    │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐    │
│  │  Tabs: [Mis propiedades] [Mis reservas] [Favoritos]   │    │
│  │        [Mis consultas] [Mis reseñas]                  │    │
│  ├──────────────────────────────────────────────────────┤    │
│  │                                                      │    │
│  │  ┌────────┐ ┌────────┐ ┌────────┐                   │    │
│  │  │ [img]  │ │ [img]  │ │ [img]  │                   │    │
│  │  │ Título │ │ Título │ │ Título │                   │    │
│  │  │ $1500  │ │ $2000  │ │ $1200  │                   │    │
│  │  │ [Editar]│ │ [Editar]│ │ [Editar]│                   │    │
│  │  └────────┘ └────────┘ └────────┘                   │    │
│  │                                                      │    │
│  │  [+ Publicar nueva propiedad]                        │    │
│  └──────────────────────────────────────────────────────┘    │
└──────────────────────────────────────────────────────────────┘
```

---

## 8. Panel de administración

```text
┌──────────────────────────────────────────────────────────────┐
│  [Logo] AlquilER Admin    [🔔] [👤 Admin]                    │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌────────────┐  ┌──────────────────────────────────────┐    │
│  │            │  │  Panel de administración              │    │
│  │  Sidebar   │  │                                      │    │
│  │            │  │  ┌────┐ ┌────┐ ┌────┐ ┌────┐        │    │
│  │  📊 Dashboard│  │  │ 42 │ │ 15 │ │ 8  │ │ 3  │        │    │
│  │  👥 Usuarios │  │  │Users│ │Props│ │Res.│ │Pend│        │    │
│  │  🏠 Propiedades│ │  └────┘ └────┘ └────┘ └────┘        │    │
│  │  📋 Reservas  │  │                                      │    │
│  │  💬 Consultas │  │  Accesos rápidos                     │    │
│  │  ⭐ Reseñas   │  │  [Gestionar usuarios]                │    │
│  │  📂 Categorías│  │  [Gestionar propiedades]             │    │
│  │  🌍 Provincias│  │  [Ver papelera]                      │    │
│  │  🏙️ Localidades│ │  [Ver logs]                          │    │
│  │  🔧 Servicios │  │                                      │    │
│  │  👤 Roles     │  │  ─── Última actividad ───            │    │
│  │  📝 Logs      │  │  • Usuario X reservó propiedad Y     │    │
│  │  🗑️ Papelera  │  │  • Admin Z marcó propiedad destacada  │    │
│  │            │  │  • Usuario W creó consulta            │    │
│  └────────────┘  └──────────────────────────────────────┘    │
└──────────────────────────────────────────────────────────────┘
```

---

## 9. Gestión de usuarios (Admin)

```text
┌──────────────────────────────────────────────────────────────┐
│  Panel Admin > Usuarios                                      │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  🔍 Buscar usuario...          [Filtrar por rol ▾]  [+ Nuevo]│
│                                                              │
│  ┌────┬──────────────┬──────────────────┬──────┬──────────┐  │
│  │ ID │ Nombre       │ Email            │ Rol  │ Acciones │  │
│  ├────┼──────────────┼──────────────────┼──────┼──────────┤  │
│  │ 1  │ Juan Pérez   │ juan@email.com   │ User │ ✏️ 🗑️   │  │
│  │ 2  │ María García │ maria@email.com  │ User │ ✏️ 🗑️   │  │
│  │ 3  │ Admin        │ admin@email.com  │ Admin│ ✏️       │  │
│  └────┴──────────────┴──────────────────┴──────┴──────────┘  │
│                                                              │
│  [← Anterior]  Página 1 de 3  [Siguiente →]                │
│                                                              │
│  [Ver eliminados (papelera)]                                 │
└──────────────────────────────────────────────────────────────┘
```

---

## 10. Papelera (registros eliminados)

```text
┌──────────────────────────────────────────────────────────────┐
│  Panel Admin > Papelera                                      │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  [Usuarios] [Propiedades] [Reservas] [Consultas] [Reseñas]   │
│                                                              │
│  ┌────┬──────────────┬──────────────────┬──────────────┐     │
│  │ ID │ Nombre       │ Email            │ Eliminado el │     │
│  ├────┼──────────────┼──────────────────┼──────────────┤     │
│  │ 5  │ Pedro López  │ pedro@email.com  │ 2026-04-10   │     │
│  │ 8  │ Ana Martínez │ ana@email.com    │ 2026-04-15   │     │
│  └────┴──────────────┴──────────────────┴──────────────┘     │
│                                                              │
│  [Restaurar seleccionados]                                   │
└──────────────────────────────────────────────────────────────┘
```

---

## 11. Mis reservas (inquilino)

```text
┌──────────────────────────────────────────────────────────────┐
│  Mi perfil > Mis reservas                                    │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  [Todas] [Pendientes] [Confirmadas] [Finalizadas] [Canceladas]│
│                                                              │
│  ┌──────────────────────────────────────────────────────┐    │
│  │  🏠 Departamento céntrico                            │    │
│  │  📅 01/05/2026 → 31/05/2026 (30 días)               │    │
│  │  Estado: ✅ Confirmada                                │    │
│  │  Propietario: María García                            │    │
│  │  [Ver detalle] [Cancelar] [Reseñar] (si finalizada)  │    │
│  └──────────────────────────────────────────────────────┘    │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐    │
│  │  🏠 Casa con pileta                                  │    │
│  │  📅 15/06/2026 → 20/06/2026 (5 días)                │    │
│  │  Estado: ⏳ Pendiente                                 │    │
│  │  Propietario: Juan Pérez                              │    │
│  │  [Ver detalle] [Cancelar]                            │    │
│  └──────────────────────────────────────────────────────┘    │
└──────────────────────────────────────────────────────────────┘
```

---

## 12. Gestión de reservas (propietario)

```text
┌──────────────────────────────────────────────────────────────┐
│  Mi perfil > Reservas recibidas                              │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  [Todas] [Pendientes] [Confirmadas] [Finalizadas]           │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐    │
│  │  🏠 Departamento céntrico                            │    │
│  │  Inquilino: Pedro López                              │    │
│  │  📅 01/05/2026 → 31/05/2026                         │    │
│  │  Estado: ⏳ Pendiente                                 │    │
│  │  [Confirmar] [Rechazar] [Ver detalle]                │    │
│  └──────────────────────────────────────────────────────┘    │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐    │
│  │  🏠 Casa con pileta                                  │    │
│  │  Inquilino: Ana Martínez                             │    │
│  │  📅 15/06/2026 → 20/06/2026                         │    │
│  │  Estado: ✅ Confirmada                                │    │
│  │  [Finalizar] [Ver detalle]                           │    │
│  └──────────────────────────────────────────────────────┘    │
└──────────────────────────────────────────────────────────────┘
```

---

## 13. Notificaciones

```text
┌──────────────────────────────────────────────┐
│  🔔 Notificaciones                            │
├──────────────────────────────────────────────┤
│                                              │
│  ┌────────────────────────────────────────┐  │
│  │ 📌 Nueva reserva en tu propiedad       │  │
│  │    "Departamento céntrico"             │  │
│  │    Hace 2 horas                        │  │
│  │    [Marcar como leída]                 │  │
│  ├────────────────────────────────────────┤  │
│  │ 📌 Tu reserva fue confirmada           │  │
│  │    "Casa con pileta"                   │  │
│  │    Hace 1 día                          │  │
│  │    [Marcar como leída]                 │  │
│  ├────────────────────────────────────────┤  │
│  │ 📌 Nueva consulta en tu propiedad      │  │
│  │    "Departamento céntrico"             │  │
│  │    Hace 3 días                         │  │
│  │    [Marcar como leída]                 │  │
│  └────────────────────────────────────────┘  │
│                                              │
│  [Marcar todas como leídas]                  │
└──────────────────────────────────────────────┘
```

---

## 14. Mapa de navegación (sitemap)

```mermaid
flowchart TD
    Root["/ (Home)"] --> Catalogo["/catalogo (Catálogo)"]
    Root --> Detalle["/propiedad/:id (Detalle)"]
    Root --> Login["/login"]
    Root --> Registro["/registro"]
    Root --> Contacto["/contacto"]
    Root --> FAQ["/faq"]

    Catalogo --> Detalle
    Detalle --> Reservar["/reservar/:id"]
    Detalle --> Consultar["Modal: Consulta"]
    Detalle --> Favorito["Toggle favorito"]

    Login --> Perfil["/perfil"]
    Registro --> Verificar["/verificar-email/:token"]
    Registro --> Login

    Perfil --> MisProps["/perfil/propiedades"]
    Perfil --> MisReservas["/perfil/reservas"]
    Perfil --> Favoritos["/perfil/favoritos"]
    Perfil --> MisConsultas["/perfil/consultas"]
    Perfil --> MisResenas["/perfil/resenas"]
    Perfil --> EditarPerfil["/perfil/editar"]

    MisProps --> Publicar["/propiedades/nueva"]
    MisProps --> EditarProp["/propiedades/:id/editar"]

    Perfil --> Admin["/admin (solo admin)"]
    Admin --> AdminUsers["/admin/usuarios"]
    Admin --> AdminProps["/admin/propiedades"]
    Admin --> AdminReservas["/admin/reservas"]
    Admin --> AdminConsultas["/admin/consultas"]
    Admin --> AdminResenas["/admin/resenas"]
    Admin --> AdminCats["/admin/categorias"]
    Admin --> AdminProv["/admin/provincias"]
    Admin --> AdminLoc["/admin/localidades"]
    Admin --> AdminServ["/admin/servicios"]
    Admin --> AdminRoles["/admin/roles"]
    Admin --> AdminLogs["/admin/logs"]
    Admin --> AdminTrash["/admin/papelera"]
```

---

## 15. Responsive: breakpoints

```mermaid
flowchart LR
    M["📱 Mobile<br/>320px - 767px"] --> T["📱 Tablet<br/>768px - 1023px"]
    T --> D["🖥️ Desktop<br/>1024px+"]
```

### Adaptaciones por breakpoint

| Componente | Mobile | Tablet | Desktop |
|---|---|---|---|
| Header | Hamburguesa | Hamburguesa | Links horizontales |
| Catálogo | 1 columna | 2 columnas | 3-4 columnas |
| Filtros | Accordion colapsable | Sidebar | Sidebar fijo |
| Detalle propiedad | Stacked (vertical) | 2 columnas | 2 columnas + sidebar |
| Formulario reserva | Full width | Full width | Modal o sidebar |
| Admin sidebar | Oculto (toggle) | Oculto (toggle) | Visible fijo |
| Cards de propiedad | Full width | 2 por fila | 3-4 por fila |

---

## Notas

- Los wireframes son de **baja fidelidad** (estructura, no diseño visual).
- Los colores, tipografías e imágenes reales se definen en la fase de diseño UI.
- Los wireframes sirven como guía para el desarrollo frontend, no como especificación final.
- Para wireframes de alta fidelidad, usar Figma o similar.
