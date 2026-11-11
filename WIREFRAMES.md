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

## 16. Favoritos

```text
┌──────────────────────────────────────────────────────────────┐
│  Mi perfil > Favoritos                                       │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌────────┐ ┌────────┐ ┌────────┐                           │
│  │ [img]  │ │ [img]  │ │ [img]  │                           │
│  │ Título │ │ Título │ │ Título │                           │
│  │ $1500  │ │ $2000  │ │ $1200  │                           │
│  │ 2 hab  │ │ 3 hab  │ │ 1 hab  │                           │
│  │ [♥ Quitar] │ [♥ Quitar] │ [♥ Quitar] │                    │
│  └────────┘ └────────┘ └────────┘                           │
│                                                              │
│  (Estado vacío si no hay favoritos)                          │
│  ┌──────────────────────────────────────────────────────┐    │
│  │  ❤️ No tenés favoritos todavía                       │    │
│  │  Explorá el catálogo y guardá tus propiedades        │    │
│  │  preferidas.                                          │    │
│  │  [Ir al catálogo]                                    │    │
│  └──────────────────────────────────────────────────────┘    │
└──────────────────────────────────────────────────────────────┘
```

---

## 17. Mis consultas

```text
┌──────────────────────────────────────────────────────────────┐
│  Mi perfil > Consultas                                       │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  [Enviadas] [Recibidas]                                      │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐    │
│  │  🏠 Departamento céntrico                            │    │
│  │  "Hola, ¿acepta mascotas grandes?"                   │    │
│  │  Estado: Respondida ✅                                │    │
│  │  Última respuesta: Hace 2 días                        │    │
│  │  [Ver conversación]                                  │    │
│  └──────────────────────────────────────────────────────┘    │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐    │
│  │  🏠 Casa con pileta                                  │    │
│  │  "¿El precio incluye expensas?"                      │    │
│  │  Estado: Sin respuesta ⏳                             │    │
│  │  Enviada: Hace 5 días                                │    │
│  │  [Ver conversación]                                  │    │
│  └──────────────────────────────────────────────────────┘    │
└──────────────────────────────────────────────────────────────┘
```

---

## 18. Detalle de consulta (mensajería bidireccional)

```text
┌──────────────────────────────────────────────────────────────┐
│  ← Volver a mis consultas                                    │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐    │
│  │  Consulta sobre: Departamento céntrico                │    │
│  │  Propietario: María García                            │    │
│  ├──────────────────────────────────────────────────────┤    │
│  │                                                      │    │
│  │  ┌──────────────────────────────────────────────┐    │    │
│  │  │ 👤 Interesado (tú)                           │    │    │
│  │  │ "Hola, ¿acepta mascotas grandes?"            │    │    │
│  │  │ Hace 5 días                                  │    │    │
│  │  └──────────────────────────────────────────────┘    │    │
│  │                                                      │    │
│  │          ┌──────────────────────────────────────┐    │    │
│  │          │ 👤 Propietario                       │    │    │
│  │          │ "Sí, aceptamos mascotas de hasta     │    │    │
│  │          │  20kg."                              │    │    │
│  │          │ Hace 2 días                          │    │    │
│  │          └──────────────────────────────────────┘    │    │
│  │                                                      │    │
│  │  ┌──────────────────────────────────────────────┐    │    │
│  │  │ 👤 Interesado (tú)                           │    │    │
│  │  │ "Perfecto, gracias!"                         │    │    │
│  │  │ Hace 1 día                                   │    │    │
│  │  └──────────────────────────────────────────────┘    │    │
│  │                                                      │    │
│  ├──────────────────────────────────────────────────────┤    │
│  │  ┌──────────────────────────────────────────────┐    │    │
│  │  │ Escribir respuesta...                        │    │    │
│  │  └──────────────────────────────────────────────┘    │    │
│  │  [Enviar]                                            │    │
│  └──────────────────────────────────────────────────────┘    │
└──────────────────────────────────────────────────────────────┘
```

---

## 19. Crear reseña

```text
┌──────────────────────────────────────────────┐
│  ← Volver a mis reservas                     │
│                                              │
│  ┌────────────────────────────────────────┐  │
│  │       Dejar reseña                     │  │
│  │                                        │  │
│  │  Propiedad: Departamento céntrico      │  │
│  │  Reserva finalizada: 31/05/2026        │  │
│  │                                        │  │
│  │  Calificación                          │  │
│  │  ★★★★☆  (4 de 5)                      │  │
│  │  [1] [2] [3] [4] [5]                  │  │
│  │                                        │  │
│  │  Comentario                            │  │
│  │  ┌──────────────────────────────┐      │  │
│  │  │                              │      │  │
│  │  │                              │      │  │
│  │  │                              │      │  │
│  │  └──────────────────────────────┘      │  │
│  │                                        │  │
│  │  ┌──────────────────────────────┐      │  │
│  │  │       Enviar reseña          │      │  │
│  │  └──────────────────────────────┘      │  │
│  └────────────────────────────────────────┘  │
└──────────────────────────────────────────────┘
```

---

## 20. Ver reseñas

```text
┌──────────────────────────────────────────────────────────────┐
│  Mi perfil > Reseñas                                         │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  [Recibidas] [De mis propiedades] [Escritas]                 │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐    │
│  │  ⭐ Reseña de: Juan Pérez                            │    │
│  │  Propiedad: Departamento céntrico                     │    │
│  │  ★★★★☆ 4/5                                           │    │
│  │  "Excelente ubicación, muy limpio y cómodo."         │    │
│  │  Fecha: 02/06/2026                                    │    │
│  └──────────────────────────────────────────────────────┘    │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐    │
│  │  ⭐ Reseña de: Ana Martínez                          │    │
│  │  Propiedad: Departamento céntrico                     │    │
│  │  ★★★★★ 5/5                                           │    │
│  │  "Perfecto para una estadía corta."                  │    │
│  │  Fecha: 15/06/2026                                    │    │
│  └──────────────────────────────────────────────────────┘    │
└──────────────────────────────────────────────────────────────┘
```

---

## 21. Recuperar contraseña

```text
┌──────────────────────────────────────────┐
│                                          │
│           [Logo] AlquilER                │
│                                          │
│  ┌────────────────────────────────────┐  │
│  │    Recuperar contraseña            │  │
│  │                                    │  │
│  │  Ingresá tu email y te enviaremos  │  │
│  │  un enlace para restablecer tu     │  │
│  │  contraseña.                       │  │
│  │                                    │  │
│  │  Email                             │  │
│  │  ┌──────────────────────────────┐  │  │
│  │  │ usuario@email.com            │  │  │
│  │  └──────────────────────────────┘  │  │
│  │                                    │  │
│  │  ┌──────────────────────────────┐  │  │
│  │  │   Enviar enlace              │  │  │
│  │  └──────────────────────────────┘  │  │
│  │                                    │  │
│  │  ← Volver al login                 │  │
│  └────────────────────────────────────┘  │
│                                          │
└──────────────────────────────────────────┘
```

---

## 22. Restablecer contraseña (con token)

```text
┌──────────────────────────────────────────┐
│                                          │
│           [Logo] AlquilER                │
│                                          │
│  ┌────────────────────────────────────┐  │
│  │    Nueva contraseña                │  │
│  │                                    │  │
│  │  Nueva contraseña                  │  │
│  │  ┌──────────────────────────────┐  │  │
│  │  │ ••••••••                     │  │  │
│  │  └──────────────────────────────┘  │  │
│  │                                    │  │
│  │  Confirmar contraseña              │  │
│  │  ┌──────────────────────────────┐  │  │
│  │  │ ••••••••                     │  │  │
│  │  └──────────────────────────────┘  │  │
│  │                                    │  │
│  │  ┌──────────────────────────────┐  │  │
│  │  │   Restablecer contraseña      │  │  │
│  │  └──────────────────────────────┘  │  │
│  │                                    │  │
│  │  (Token expirado → mostrar error)  │  │
│  └────────────────────────────────────┘  │
│                                          │
└──────────────────────────────────────────┘
```

---

## 23. Cambiar contraseña (desde perfil)

```text
┌──────────────────────────────────────────────┐
│  Mi perfil > Cambiar contraseña               │
├──────────────────────────────────────────────┤
│                                              │
│  ┌────────────────────────────────────────┐  │
│  │                                        │  │
│  │  Contraseña actual                     │  │
│  │  ┌──────────────────────────────┐      │  │
│  │  │ ••••••••                     │      │  │
│  │  └──────────────────────────────┘      │  │
│  │                                        │  │
│  │  Nueva contraseña                      │  │
│  │  ┌──────────────────────────────┐      │  │
│  │  │ ••••••••                     │      │  │
│  │  └──────────────────────────────┘      │  │
│  │                                        │  │
│  │  Confirmar nueva contraseña             │  │
│  │  ┌──────────────────────────────┐      │  │
│  │  │ ••••••••                     │      │  │
│  │  └──────────────────────────────┘      │  │
│  │                                        │  │
│  │  Se enviará un email de confirmación   │  │
│  │  a tu correo.                          │  │
│  │                                        │  │
│  │  ┌──────────────────────────────┐      │  │
│  │  │   Cambiar contraseña          │      │  │
│  │  └──────────────────────────────┘      │  │
│  └────────────────────────────────────────┘  │
└──────────────────────────────────────────────┘
```

---

## 24. Contacto

```text
┌──────────────────────────────────────────────┐
│  Contacto                                    │
├──────────────────────────────────────────────┤
│                                              │
│  ┌────────────────────────────────────────┐  │
│  │                                        │  │
│  │  Nombre                                │  │
│  │  ┌──────────────────────────────┐      │  │
│  │  │                              │      │  │
│  │  └──────────────────────────────┘      │  │
│  │                                        │  │
│  │  Email                                 │  │
│  │  ┌──────────────────────────────┐      │  │
│  │  │                              │      │  │
│  │  └──────────────────────────────┘      │  │
│  │                                        │  │
│  │  Asunto                                │  │
│  │  ┌──────────────────────────────┐      │  │
│  │  │                              │      │  │
│  │  └──────────────────────────────┘      │  │
│  │                                        │  │
│  │  Mensaje                               │  │
│  │  ┌──────────────────────────────┐      │  │
│  │  │                              │      │  │
│  │  │                              │      │  │
│  │  └──────────────────────────────┘      │  │
│  │                                        │  │
│  │  ┌──────────────────────────────┐      │  │
│  │  │   Enviar mensaje             │      │  │
│  │  └──────────────────────────────┘      │  │
│  └────────────────────────────────────────┘  │
└──────────────────────────────────────────────┘
```

---

## 25. FAQ

```text
┌──────────────────────────────────────────────────────────────┐
│  Preguntas frecuentes                                        │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  ▸ ¿Cómo publico una propiedad?                             │
│  ▸ ¿Cómo reservo una propiedad?                              │
│  ▸ ¿Puedo cancelar una reserva?                              │
│  ▸ ¿Cómo verifico mi email?                                  │
│  ▸ ¿Cómo recupero mi contraseña?                             │
│  ▸ ¿Qué significa "destacada"?                               │
│  ▸ ¿Cómo dejo una reseña?                                    │
│  ▸ ¿Cómo contacto al administrador?                          │
│                                                              │
│  (Click en cada pregunta para expandir la respuesta)         │
└──────────────────────────────────────────────────────────────┘
```

---

## 26. Verificar email

```text
┌──────────────────────────────────────────┐
│                                          │
│           [Logo] AlquilER                │
│                                          │
│  ┌────────────────────────────────────┐  │
│  │                                    │  │
│  │         ✅ Email verificado         │  │
│  │                                    │  │
│  │  Tu email ha sido verificado       │  │
│  │  correctamente. Ya podés iniciar    │  │
│  │  sesión.                           │  │
│  │                                    │  │
│  │  ┌──────────────────────────────┐  │  │
│  │  │   Ir al login                │  │  │
│  │  └──────────────────────────────┘  │  │
│  │                                    │  │
│  └────────────────────────────────────┘  │
│                                          │
│  (Estado error: token inválido/expirado) │
│  ┌────────────────────────────────────┐  │
│  │         ❌ Error                   │  │
│  │                                    │  │
│  │  El enlace de verificación es      │  │
│  │  inválido o ha expirado.           │  │
│  │                                    │  │
│  │  ┌──────────────────────────────┐  │  │
│  │  │   Reenviar verificación      │  │  │
│  │  └──────────────────────────────┘  │  │
│  └────────────────────────────────────┘  │
│                                          │
└──────────────────────────────────────────┘
```

---

## 27. Estados vacíos y error

```text
┌──────────────────────────────────────────────┐
│  (EmptyState - sin resultados)               │
│                                              │
│  ┌────────────────────────────────────────┐  │
│  │                                        │  │
│  │         🔍 No se encontraron           │  │
│  │         propiedades                     │  │
│  │                                        │  │
│  │  Intentá con otros filtros o           │  │
│  │  explorá todas las propiedades.        │  │
│  │                                        │  │
│  │  [Limpiar filtros] [Ver todas]          │  │
│  └────────────────────────────────────────┘  │
│                                              │
│  (ErrorState - error de conexión)            │
│  ┌────────────────────────────────────────┐  │
│  │                                        │  │
│  │         ⚠️ Algo salió mal               │  │
│  │                                        │  │
│  │  No pudimos cargar los datos.          │  │
│  │  Verificá tu conexión e intentá        │  │
│  │  de nuevo.                             │  │
│  │                                        │  │
│  │  [Reintentar]                          │  │
│  └────────────────────────────────────────┘  │
│                                              │
│  (404 - Página no encontrada)                │
│  ┌────────────────────────────────────────┐  │
│  │                                        │  │
│  │         404                            │  │
│  │                                        │  │
│  │  La página que buscás no existe.       │  │
│  │                                        │  │
│  │  [Volver al inicio]                    │  │
│  └────────────────────────────────────────┘  │
└──────────────────────────────────────────────┘
```

---

## 28. Gestión admin: Propiedades

```text
┌──────────────────────────────────────────────────────────────┐
│  Panel Admin > Propiedades                                   │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  🔍 Buscar propiedad...    [Filtrar por categoría ▾]         │
│  [Todas] [Disponibles] [No disponibles] [Destacadas] [Eliminadas]│
│                                                              │
│  ┌────┬──────────────────┬────────┬──────┬──────────────┐    │
│  │ ID │ Título           │ Precio │ Disp │ Acciones     │    │
│  ├────┼──────────────────┼────────┼──────┼──────────────┤    │
│  │ 1  │ Depto céntrico   │ $1500  │ ✅   │ ✏️ 🗑️ ⭐    │    │
│  │ 2  │ Casa con pileta  │ $2000  │ ✅   │ ✏️ 🗑️ ⭐    │    │
│  │ 3  │ Monoambiente     │ $800   │ ❌   │ ✏️ 🗑️       │    │
│  └────┴──────────────────┴────────┴──────┴──────────────┘    │
│                                                              │
│  [← Anterior]  Página 1 de 5  [Siguiente →]                 │
└──────────────────────────────────────────────────────────────┘
```

---

## 29. Gestión admin: Categorías / Provincias / Localidades / Servicios / Roles

```text
┌──────────────────────────────────────────────────────────────┐
│  Panel Admin > Categorías                                    │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  [+ Nueva categoría]                                         │
│                                                              │
│  ┌────┬──────────────────┬──────────────┐                   │
│  │ ID │ Nombre           │ Acciones     │                   │
│  ├────┼──────────────────┼──────────────┤                   │
│  │ 1  │ Departamento     │ ✏️ 🗑️        │                   │
│  │ 2  │ Casa             │ ✏️ 🗑️        │                   │
│  │ 3  │ Monoambiente     │ ✏️ 🗑️        │                   │
│  └────┴──────────────────┴──────────────┘                   │
│                                                              │
│  (Misma estructura para Provincias, Localidades, Servicios,  │
│   Roles: tabla con ID, nombre, acciones)                     │
└──────────────────────────────────────────────────────────────┘
```

---

## 30. Gestión admin: Consultas

```text
┌──────────────────────────────────────────────────────────────┐
│  Panel Admin > Consultas                                     │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  [Todas] [Abiertas] [Respondidas] [Eliminadas]               │
│                                                              │
│  ┌────┬──────────────────┬──────────────┬──────────┬──────┐  │
│  │ ID │ Propiedad        │ Usuario      │ Estado   │ Acc. │  │
│  ├────┼──────────────────┼──────────────┼──────────┼──────┤  │
│  │ 1  │ Depto céntrico   │ Juan Pérez   │ Abierta  │ 👁️ 🗑️│  │
│  │ 2  │ Casa con pileta  │ Ana García   │ Respond. │ 👁️ 🗑️│  │
│  └────┴──────────────────┴──────────────┴──────────┴──────┘  │
│                                                              │
│  [← Anterior]  Página 1 de 2  [Siguiente →]                 │
└──────────────────────────────────────────────────────────────┘
```

---

## 31. Gestión admin: Reseñas

```text
┌──────────────────────────────────────────────────────────────┐
│  Panel Admin > Reseñas                                       │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  [Todas] [De propiedades] [De inquilinos] [Eliminadas]       │
│                                                              │
│  ┌────┬────────┬──────────────┬──────┬──────────┬──────┐     │
│  │ ID │ Autor  │ Propiedad    │ Cal. │ Fecha    │ Acc. │     │
│  ├────┼────────┼──────────────┼──────┼──────────┼──────┤     │
│  │ 1  │ Juan   │ Depto cen.   │ ★★★★☆│ 02/06    │ 👁️ 🗑️│     │
│  │ 2  │ Ana    │ Depto cen.   │ ★★★★★│ 15/06    │ 👁️ 🗑️│     │
│  └────┴────────┴──────────────┴──────┴──────────┴──────┘     │
│                                                              │
│  [← Anterior]  Página 1 de 1  [Siguiente →]                 │
└──────────────────────────────────────────────────────────────┘
```

---

## 32. Gestión admin: Logs de actividad

```text
┌──────────────────────────────────────────────────────────────┐
│  Panel Admin > Logs de actividad                             │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  🔍 Buscar...    [Filtrar por acción ▾]    [Filtrar por fecha]│
│                                                              │
│  ┌──────────┬──────────────┬──────────────┬──────────────┐   │
│  │ Fecha    │ Usuario      │ Acción       │ IP           │   │
│  ├──────────┼──────────────┼──────────────┼──────────────┤   │
│  │ 11/11 10:│ Juan Pérez   │ Login        │ 192.168.1.5  │   │
│  │ 11/11 09:│ Admin        │ Marcó destac.│ 192.168.1.1  │   │
│  │ 10/11 18:│ María García │ Publicó prop.│ 192.168.1.8  │   │
│  └──────────┴──────────────┴──────────────┴──────────────┘   │
│                                                              │
│  [← Anterior]  Página 1 de 20  [Siguiente →]                │
└──────────────────────────────────────────────────────────────┘
```

---

## Notas

- Los wireframes son de **baja fidelidad** (estructura, no diseño visual).
- Los colores, tipografías e imágenes reales se definen en la fase de diseño UI.
- Los wireframes sirven como guía para el desarrollo frontend, no como especificación final.
- Para wireframes de alta fidelidad, usar Figma o similar.
