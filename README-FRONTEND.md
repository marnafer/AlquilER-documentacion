# Alquiler Frontend — Guía de Desarrollo

Frontend desacoplado del sistema AlquilER, construido con **React 19** y **Vite 5**. Consume la API REST del backend (PHP) y se despliega como SPA estática.

**URL de producción:** https://alquilerr.alwaysdata.net

---

## Tecnologías

| Tecnología | Versión | Uso |
|---|---|---|
| React | 19.3 | UI y enrutamiento |
| React Router | 7.18 | Enrutamiento SPA |
| Vite | 5.4 | Bundler y dev server |
| React Bootstrap | 2.10 | Componentes de UI (navbar, modal, etc.) |
| Bootstrap | 5.3 | Estilos base y grid |
| Phosphor Icons | 2.1 | Iconografía |
| Playwright | 1.63 | Tests E2E |

## Requisitos

- Node.js 20 o superior
- npm 10 o superior
- Backend corriendo en `localhost:8000` (para desarrollo)

## Instalación

```bash
npm install
```

## Desarrollo

```bash
npm run dev
```

La aplicación inicia en `http://localhost:3000`. El dev server de Vite redirige `/api` y `/uploads` al backend (`localhost:8000`) mediante proxy.

### Variables de entorno (desarrollo)

Crear `.env` a partir de `.env.example`:
```env
VITE_API_URL=
VITE_APP_NAME=Sistema de Alquiler
```
Con `VITE_API_URL` vacío, las llamadas a la API salen por `localhost:3000` y Vite las redirige al backend.

### Variables de entorno (producción)

Archivo `.env.production`:
```env
VITE_API_URL=https://alquiler.alwaysdata.net
VITE_APP_NAME=Sistema de Alquiler
```

## Estructura del proyecto

```
src/
├── components/          # Componentes reutilizables
│   ├── Header.jsx       # Navbar principal (colapsa en móvil)
│   ├── Footer.jsx       # Pie de página
│   ├── PropiedadCard.jsx # Tarjeta de propiedad (catálogo y destacadas)
│   ├── PropiedadDetalle.jsx # Detalle de propiedad (modal)
│   ├── PropiedadCardImage.jsx # Imagen con fallback
│   ├── MultiSelect.jsx  # Selector múltiple (filtros del catálogo)
│   ├── Icon.jsx         # Wrapper de Phosphor Icons
│   ├── ServicioIcono.jsx # Icono de servicio
│   ├── EmptyState.jsx   # Estado vacío
│   ├── ErrorState.jsx   # Estado de error con reintento
│   ├── SkeletonCard.jsx # Skeleton de carga
│   ├── Loader.jsx       # Spinner de carga
│   └── admin/
│       └── PanelCrud.jsx # Panel CRUD genérico para admin
├── context/
│   ├── AuthContext.jsx  # Autenticación (usuario, token, login, logout)
│   ├── ThemeContext.jsx # Tema claro/oscuro (persiste en localStorage)
│   └── UIContext.jsx    # Estado de UI (modales, toasts, etc.)
├── hooks/
│   ├── useAuth.js       # Hook para acceder a AuthContext
│   ├── useSEO.js        # SEO (title, meta, OG tags por página)
│   └── useScrollReveal.js # Animaciones de entrada al hacer scroll
├── pages/               # Páginas (una por ruta)
│   ├── Home.jsx         # Inicio: hero, búsqueda, destacadas, servicios, stats
│   ├── Propiedades.jsx  # Catálogo con filtros avanzados y paginación
│   ├── PropiedadDetalle.jsx # Detalle de propiedad
│   ├── PropiedadForm.jsx # Crear/editar propiedad
│   ├── Dashboard.jsx    # Panel con resumen del usuario
│   ├── Perfil.jsx       # Perfil y configuración
│   ├── PerfilPublico.jsx # Perfil público de un usuario
│   ├── MisPropiedades.jsx # Mis propiedades publicadas
│   ├── Favoritos.jsx    # Propiedades favoritas
│   ├── MisReservas.jsx  # Mis reservas
│   ├── ReservaDetalle.jsx # Detalle de reserva
│   ├── MisConsultas.jsx # Mis consultas
│   ├── MisResenas.jsx   # Mis reseñas
│   ├── Notificaciones.jsx # Notificaciones
│   ├── Login.jsx        # Inicio de sesión
│   ├── Register.jsx     # Registro
│   ├── RecuperarContrasena.jsx # Recuperar contraseña
│   ├── RestablecerContrasena.jsx # Restablecer contraseña
│   ├── VerificarEmail.jsx # Verificación de email
│   ├── Contacto.jsx     # Contacto
│   ├── Servicios.jsx    # Catálogo de servicios
│   ├── PreguntasFrecuentes.jsx
│   ├── Terminos.jsx
│   ├── Privacidad.jsx
│   ├── NotFound.jsx     # 404
│   └── admin/           # Páginas de administración (12 páginas)
├── services/
│   ├── api.js           # Todas las llamadas a la API (90+ funciones)
│   └── autoRefresh.js   # Interceptor de 401 + refresh de tokens
├── styles/              # 19 archivos CSS por sección
├── utils/
│   ├── categorias.js    # Iconos y helpers de categorías
│   ├── imagenes.js      # Rutas de imágenes
│   ├── servicios.js     # Helpers de servicios
│   ├── profanity.js     # Filtro de profanidad
│   └── vistosRecientemente.js # Historial de vistos
├── App.jsx              # Shell: BrowserRouter, Header, Footer, main
├── AppRouter.jsx        # Definición de todas las rutas
└── main.jsx             # Entry point
```

## Autenticación

La autenticación es **basada en cookies HttpOnly** administradas por el backend:

1. El frontend envía `credentials: 'include'` en todas las llamadas a la API (via `autoRefresh.js`).
2. El backend establece cookies `access_token` (path `/`) y `refresh_token` (path `/api/autenticador`).
3. En producción: `Secure=true`, `SameSite=None` (cross-origin entre `alquilerr.alwaysdata.net` y `alquiler.alwaysdata.net`).
4. `autoRefresh.js` intercepta respuestas 401 y renueva el token usando la cookie de refresh. Si el refresh falla, dispara el evento `auth:session-expired`.

El contexto de autenticación (`AuthContext`) maneja el estado del usuario y expone:
- `token`, `usuario`, `loading`
- `login()`, `logout()`, `isAuthenticated`, `refreshUser()`

## Tema (modo claro/oscuro)

El tema se maneja con `ThemeContext` y se persiste en `localStorage` (clave `alquiler-theme`). Se aplica como atributo `data-theme` en `<html>`. Los estilos usan variables CSS que cambian automáticamente según el tema.

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
- `AddDefaultCharset utf-8` en `.htaccess` para evitar mojibake.

## API (services/api.js)

Todas las llamadas a la API están centralizadas en `services/api.js` (90+ funciones). Cada función:
- Usa `fetch` con `credentials: 'include'` (via autoRefresh).
- Retorna `{ success, data, error, ... }`.
- Maneja errores de red con `errorDeConexion()`.

Grupos de funciones:
- **Autenticación:** `login`, `register`, `logout`, `getPerfil`, `actualizarPerfil`.
- **Propiedades:** `getPropiedades`, `getPropiedad`, `getPropiedadesDestacadas`, `crearPropiedad`, `actualizarPropiedad`, `eliminarPropiedad`.
- **Reservas:** `getReservas`, `crearReserva`, `confirmarReserva`, `rechazarReserva`, `cancelarReserva`.
- **Consultas:** `getConsultas`, `crearConsulta`, `responderConsulta`.
- **Reseñas:** `getResenas`, `crearResena`.
- **Favoritos:** `getFavoritos`, `agregarFavorito`, `eliminarFavorito`.
- **Servicios:** `getServicios`, `getServicio`.
- **Categorías:** `getCategorias`.
- **Ubicación:** `getProvincias`, `getLocalidades`.
- **Notificaciones:** `getNotificaciones`, `marcarLeida`, `marcarTodasLeidas`.
- **Admin:** `getUsuarios`, `getCategoriasAdmin`, etc.

## Tests E2E (Playwright)

```bash
npx playwright test
```

Tests en `tests/` y `e2e/`:
- `tests/chips-solapamiento.spec.js` — Solapamiento de chips en móvil.
- `e2e/usuario.spec.js` — Flujo de usuario (registro, catálogo, detalle, favoritos).
- `e2e/admin.spec.js` — Panel de administración.
- `e2e/destacadas.spec.js` — Propiedades destacadas.
- `e2e/notificaciones.spec.js` — Notificaciones.

Configuración en `playwright.config.js`:
- Viewports: 390px (móvil), 768px (tablet), 1024px (desktop), 1440px (wide).
- baseURL: `http://127.0.0.1:3000`.
- webServer: inicia `npm run dev` automáticamente.

## Build de producción

```bash
npm run build
```
Genera `dist/` con los archivos optimizados.

Para previsualizar:
```bash
npm run preview
```

## Despliegue

1. Ejecutar `npm run build`.
2. Subir todo el contenido de `dist/` a `/www/` del servidor FTP (`ftp-alquilerr.alwaysdata.net`, puerto 21, TLS).
3. Asegurarse de que `dist/.htaccess` se suba a `/www/.htaccess`.
4. Limpiar assets viejos en `/www/assets/` que no estén en el nuevo build.

**Producción:** https://alquilerr.alwaysdata.net

## SEO

Cada página usa el hook `useSEO` para establecer:
- `<title>` y `<meta name="description">`.
- Open Graph tags (og:title, og:description, og:type).
- Twitter Card tags.
- `noindex` para páginas privadas (login, dashboard, etc.).

## Notas

- El frontend es una SPA. Todas las rutas son manejadas por React Router en el cliente.
- El `.htaccess` en `/www/` redirige todas las rutas no-archivo a `index.html`.
- PurgeCSS se ejecuta solo en build para eliminar CSS no utilizado.
- El charset UTF-8 se fuerza en Apache via `AddDefaultCharset utf-8` y `AddCharset utf-8` en `.htaccess`.
- Las imágenes del hero y logo están en `public/assets/img/` y se copian a `dist/assets/img/` en el build.
- El historial de "vistos recientemente" se guarda en `localStorage` (clave `alquiler_vistos_recientemente`).
- El tema (claro/oscuro) se persiste en `localStorage` (clave `alquiler-theme`).
- El token de autenticación se maneja via cookies HttpOnly (no localStorage).
