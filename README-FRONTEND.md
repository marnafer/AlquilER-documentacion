# AlquilER — Frontend Web

Frontend desacoplado del sistema AlquilER, construido con **React 19** y **Vite 5**. Consume la API REST del backend (PHP) y se despliega como SPA estática.

**URL de producción:** https://alquilerr.alwaysdata.net

## Tecnologías

- **React 19** — UI y enrutamiento (React Router 7)
- **Vite 5** — bundler y dev server
- **React Bootstrap 2** — componentes de UI
- **Phosphor Icons** — iconografía
- **CSS modular** — 19 archivos CSS por sección (base, home, catalogo, detalle, etc.)

## Requisitos

- Node.js 20 o superior
- npm 10 o superior

## Estructura del proyecto

```
sistema-alquiler-frontend-web/
├── public/              # Assets estáticos (copiados a dist/ en build)
│   ├── .htaccess        # Fallback SPA para rutas de React Router
│   ├── favicon*.png     # Favicons
│   ├── manifest.json    # PWA manifest
│   └── assets/img/      # Imágenes del hero, logo, etc.
├── src/
│   ├── components/      # Componentes reutilizables
│   │   ├── Header.jsx           # Navbar principal
│   │   ├── Footer.jsx           # Pie de página
│   │   ├── PropiedadCard.jsx    # Tarjeta de propiedad
│   │   ├── PropiedadDetalle.jsx # Detalle de propiedad (modal/página)
│   │   ├── PropiedadCardImage.jsx
│   │   ├── MultiSelect.jsx      # Selector múltiple (filtros)
│   │   ├── Icon.jsx             # Wrapper de Phosphor Icons
│   │   ├── ServicioIcono.jsx    # Icono de servicio
│   │   ├── EmptyState.jsx       # Estado vacío
│   │   ├── ErrorState.jsx       # Estado de error
│   │   ├── SkeletonCard.jsx     # Skeleton de carga
│   │   ├── Loader.jsx           # Spinner de carga
│   │   └── admin/               # Componentes de administración
│   │       └── PanelCrud.jsx    # Panel CRUD genérico
│   ├── context/
│   │   ├── AuthContext.jsx      # Contexto de autenticación
│   │   ├── ThemeContext.jsx     # Contexto de tema (claro/oscuro)
│   │   └── UIContext.jsx        # Contexto de UI
│   ├── hooks/
│   │   ├── useAuth.js           # Hook de autenticación
│   │   ├── useSEO.js            # SEO (title, meta, OG tags)
│   │   └── useScrollReveal.js   # Animaciones de scroll
│   ├── pages/                   # Páginas (rutas)
│   │   ├── Home.jsx             # Inicio (hero, destacadas, servicios, stats)
│   │   ├── Propiedades.jsx      # Catálogo con filtros
│   │   ├── PropiedadDetalle.jsx # Detalle de propiedad
│   │   ├── PropiedadForm.jsx    # Crear/editar propiedad
│   │   ├── Dashboard.jsx        # Panel del usuario
│   │   ├── Perfil.jsx           # Perfil del usuario
│   │   ├── PerfilPublico.jsx    # Perfil público de un usuario
│   │   ├── MisPropiedades.jsx   # Mis propiedades publicadas
│   │   ├── Favoritos.jsx        # Propiedades favoritas
│   │   ├── MisReservas.jsx      # Mis reservas
│   │   ├── ReservaDetalle.jsx   # Detalle de reserva
│   │   ├── MisConsultas.jsx     # Mis consultas
│   │   ├── MisResenas.jsx       # Mis reseñas
│   │   ├── Notificaciones.jsx   # Notificaciones
│   │   ├── Login.jsx            # Inicio de sesión
│   │   ├── Register.jsx         # Registro
│   │   ├── RecuperarContrasena.jsx  # Recuperar contraseña
│   │   ├── RestablecerContrasena.jsx # Restablecer contraseña
│   │   ├── VerificarEmail.jsx   # Verificación de email
│   │   ├── Contacto.jsx         # Contacto
│   │   ├── Servicios.jsx        # Catálogo de servicios
│   │   ├── PreguntasFrecuentes.jsx
│   │   ├── Terminos.jsx
│   │   ├── Privacidad.jsx
│   │   ├── NotFound.jsx         # 404
│   │   └── admin/               # Páginas de administración
│   │       ├── AdminHome.jsx
│   │       ├── UsuariosAdmin.jsx
│   │       ├── CategoriasAdmin.jsx
│   │       ├── ProvinciasAdmin.jsx
│   │       ├── LocalidadesAdmin.jsx
│   │       ├── RolesAdmin.jsx
│   │       ├── ServiciosAdmin.jsx
│   │       ├── ResenasAdmin.jsx
│   │       ├── ReservasAdmin.jsx
│   │       ├── ConsultasAdmin.jsx
│   │       ├── LogsAdmin.jsx
│   │       └── PropiedadesAdmin.jsx
│   ├── services/
│   │   ├── api.js               # Todas las llamadas a la API
│   │   └── autoRefresh.js       # Interceptor de 401 + refresh de tokens
│   ├── styles/                  # 19 archivos CSS por sección
│   │   ├── styles.css           # Entry point (@imports)
│   │   ├── base.css             # Variables CSS, reset, modo oscuro
│   │   ├── home.css             # Hero, destacadas, servicios, stats
│   │   ├── catalogo.css         # Catálogo de propiedades y filtros
│   │   ├── detalle.css          # Detalle de propiedad
│   │   ├── cuenta.css           # Dashboard, perfil, favoritos, mis propiedades
│   │   ├── formporp.css         # Formulario de propiedad
│   │   ├── dashboard.css        # Panel de usuario
│   │   ├── admin.css            # Panel de administración
│   │   ├── header.css           # Header/navbar
│   │   ├── footer.css           # Footer
│   │   ├── forms.css            # Formularios de auth
│   │   ├── responsive.css       # Media queries (768px, 480px, etc.)
│   │   ├── overlay.css          # Modales y overlays
│   │   ├── paginas.css          # Páginas estáticas
│   │   ├── perfil-publico.css   # Perfil público
│   │   ├── feedback.css         # Alertas y feedback
│   │   ├── iconos.css           # Iconos
│   │   └── estilos.css          # (legacy)
│   ├── utils/
│   │   ├── categorias.js        # Iconos y helpers de categorías
│   │   ├── imagenes.js          # Rutas de imágenes
│   │   ├── servicios.js         # Helpers de servicios
│   │   ├── profanity.js         # Filtro de profanidad
│   │   └── vistosRecientemente.js # Historial de vistos
│   ├── App.jsx                  # Shell de la app (BrowserRouter, Header, Footer)
│   ├── AppRouter.jsx            # Definición de rutas
│   └── main.jsx                 # Entry point (imports, ReactDOM)
├── dist/                        # Build de producción (generado)
├── .env                         # Variables de entorno (dev)
├── .env.example                 # Template de .env
├── .env.production              # Variables de producción
├── vite.config.js               # Configuración de Vite (proxy, purgeCSS, etc.)
├── package.json                 # Dependencias y scripts
├── .htaccess                    # Rewrite para Apache (raíz del proyecto)
└── public/.htaccess             # Fallback SPA para rutas de React Router
```

## Desarrollo

### Requisitos
- Node.js 20 o superior
- npm 10 o superior

### Instalación
```bash
npm install
```

### Servidor de desarrollo
```bash
npm run dev
```
La aplicación inicia en `http://localhost:3000`. El dev server de Vite redirige `/api` y `/uploads` al backend (`localhost:8000`).

### Variables de entorno (desarrollo)
Crear `.env` a partir de `.env.example`:
```env
VITE_API_URL=
VITE_APP_NAME=Sistema de Alquiler
```
Con `VITE_API_URL` vacío, las llamadas a la API salen por `localhost:3000` y Vite las redirige al backend.

## Build de producción

```bash
npm run build
```
El resultado se genera en `dist/` y es la carpeta optimizada que debe publicarse.

Para previsualizar el build:
```bash
npm run preview
```

## Variables de entorno (producción)

Archivo `.env.production`:
```env
VITE_API_URL=https://alquiler.alwaysdata.net
VITE_APP_NAME=Sistema de Alquiler
```

## Autenticación

La autenticación es **basada en cookies HttpOnly** administradas por el backend:
- El frontend envía `credentials: 'include'` en todas las llamadas a la API.
- El backend establece cookies `access_token` (path `/`) y `refresh_token` (path `/api/autenticador`).
- En producción: `Secure=true`, `SameSite=None` (cross-origin).
- `autoRefresh.js` intercepta respuestas 401 y renueva el token usando la cookie de refresh.

El contexto de autenticación (`AuthContext`) maneja el estado del usuario y expone `login`, `logout`, `isAuthenticated`, `usuario`, `token`.

## Tema (modo claro/oscuro)

El tema se maneja con `ThemeContext` y se persiste en `localStorage` (clave `alquiler-theme`). Se aplica como atributo `data-theme` en `<html>`. Los estilos usan variables CSS que cambian según el tema.

## Rutas

### Públicas
| Ruta | Página | Descripción |
|---|---|---|
| `/` | Home | Inicio: hero, destacadas, servicios, stats |
| `/propiedades` | Propiedades | Catálogo con filtros avanzados |
| `/propiedades/:id` | PropiedadDetalle | Detalle de una propiedad |
| `/servicios` | Servicios | Catálogo de servicios |
| `/contacto` | Contacto | Formulario de contacto |
| `/preguntas-frecuentes` | PreguntasFrecuentes | FAQ |
| `/terminos` | Terminos | Términos y condiciones |
| `/privacidad` | Privacidad | Política de privacidad |

### Protegidas (requieren autenticación)
| Ruta | Página | Descripción |
|---|---|---|
| `/perfil` | Perfil | Perfil del usuario |
| `/dashboard` | Dashboard | Panel con resumen |
| `/favoritos` | Favoritos | Propiedades favoritas |
| `/reservas` | MisReservas | Mis reservas |
| `/reservas/:id` | ReservaDetalle | Detalle de reserva |
| `/consultas` | MisConsultas | Mis consultas |
| `/mis-resenas` | MisResenas | Mis reseñas |
| `/notificaciones` | Notificaciones | Notificaciones |
| `/mis-propiedades` | MisPropiedades | Mis propiedades publicadas |
| `/propiedades/crear` | PropiedadForm | Crear propiedad |
| `/propiedades/:id/editar` | PropiedadForm | Editar propiedad |
| `/usuarios/:id` | PerfilPublico | Perfil público |

### Administración (requieren rol admin)
| Ruta | Página |
|---|---|
| `/admin` | AdminHome |
| `/admin/usuarios` | UsuariosAdmin |
| `/admin/categorias` | CategoriasAdmin |
| `/admin/provincias` | ProvinciasAdmin |
| `/admin/localidades` | LocalidadesAdmin |
| `/admin/roles` | RolesAdmin |
| `/admin/servicios` | ServiciosAdmin |
| `/admin/resenas` | ResenasAdmin |
| `/admin/reservas` | ReservasAdmin |
| `/admin/consultas` | ConsultasAdmin |
| `/admin/logs` | LogsAdmin |
| `/admin/propiedades` | PropiedadesAdmin |

### Invitados (solo no logueados)
| Ruta | Página |
|---|---|
| `/login` | Login |
| `/register` | Register |
| `/recuperar-contrasena` | RecuperarContrasena |
| `/restablecer-contrasena` | RestablecerContrasena |
| `/verificar-email` | VerificarEmail |

## Estilos

Los estilos se organizan en 19 archivos CSS importados en orden de cascada desde `styles.css`:

1. `base.css` — Variables CSS (paleta, tipografía, sombras), reset, modo oscuro.
2. `iconos.css` — Iconos.
3. `header.css` — Navbar y menú.
4. `home.css` — Hero, destacadas, servicios, stats, categorías.
5. `footer.css` — Footer.
6. `feedback.css` — Alertas y feedback.
7. `forms.css` — Formularios de auth.
8. `catalogo.css` — Catálogo y filtros.
9. `detalle.css` — Detalle de propiedad.
10. `estados.css` — Estados (loading, error, vacío).
11. `dashboard.css` — Panel de usuario.
12. `cuenta.css` — Perfil, favoritos, mis propiedades, reservas.
13. `perfil-publico.css` — Perfil público.
14. `formprop.css` — Formulario de propiedad.
15. `responsive.css` — Media queries (1200px, 1024px, 992px, 768px, 576px, 480px, 360px).
16. `admin.css` — Panel de administración.
17. `paginas.css` — Páginas estáticas.
18. `overlay.css` — Modales y overlays.

### Modo oscuro
El tema se maneja con `data-theme="dark"` en `<html>`. Las variables CSS cambian automáticamente. El toggle está en el header.

### Responsive
- **Mobile-first** con breakpoints en 1200px, 1024px, 992px, 768px, 576px, 480px, 360px.
- Grids se adaptan: 5→4→3→2→1 columnas en catálogo.
- Navegación colapsa a hamburguesa en ≤991px.
- Formularios se apilan en móvil.
- Touch targets mínimos de 44px en móvil.

## Despliegue

### Build
```bash
npm run build
```
Genera `dist/` con los archivos optimizados.

### Publicación
1. Subir todo el contenido de `dist/` a `/www/` del servidor FTP (`ftp-alquilerr.alwaysdata.net`).
2. Asegurarse de que `dist/.htaccess` se suba a `/www/.htaccess` (fallback SPA).
3. Limpiar assets viejos en `/www/assets/` que no estén en el nuevo build.

### Servidor
- **Producción:** https://alquilerr.alwaysdata.net
- **API:** https://alquiler.alwaysdata.net
- **FTP:** ftp-alquilerr.alwaysdata.net (puerto 21, TLS)

## Variables de entorno

| Variable | Desarrollo | Producción | Descripción |
|---|---|---|---|
| `VITE_API_URL` | (vacío) | `https://alquiler.alwaysdata.net` | URL base de la API. Vacío = usa el proxy de Vite (localhost:3000). |
| `VITE_APP_NAME` | `Sistema de Alquiler` | `Sistema de Alquiler` | Nombre de la aplicación. |

## Notas

- El frontend es una SPA (Single Page Application). Todas las rutas son manejadas por React Router en el cliente.
- El `.htaccess` en `/www/` redirige todas las rutas no-archivo a `index.html` para que React Router las resuelva.
- Las imágenes del hero y logo están en `public/assets/img/` y se copian a `dist/assets/img/` en el build.
- PurgeCSS se ejecuta solo en build para eliminar CSS no utilizado.
- El charset UTF-8 se fuerza en Apache via `AddDefaultCharset utf-8` y `AddCharset utf-8` en `.htaccess`.
