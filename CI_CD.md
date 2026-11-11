# CI/CD — AlquilER

> Configuración de integración y despliegue continuo para el proyecto AlquilER.

---

## 1. Resumen

| Componente | Herramienta | Trigger | Jobs |
|---|---|---|---|
| Backend (PHP) | GitHub Actions | push/PR a `dev` o `master` | PHPUnit (unit), PHP Lint |
| Frontend (React) | GitHub Actions | push/PR a `dev` o `master` | Vite Build, ESLint |

---

## 2. Backend — `.github/workflows/ci.yml`

### Jobs

| Job | Descripción | Comando |
|---|---|---|
| **test** | Ejecuta PHPUnit (suite Unit) | `composer test:unit` |
| **lint** | Verifica sintaxis PHP de todos los archivos en `src/` | `php -l` por archivo |

### Configuración

- **PHP**: 8.2
- **Extensiones**: pdo, pdo_mysql, gd, mbstring, xml
- **Dependencias**: `composer install --prefer-dist --no-progress`
- **Suite de tests**: Solo Unit (no requiere BD). Integration requiere MySQL corriendo.

### Archivo

```yaml
name: CI - Backend

on:
  push:
    branches: [dev, master]
  pull_request:
    branches: [dev, master]

jobs:
  test:
    name: PHPUnit (Unit)
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: shivammathur/setup-php@v2
        with:
          php-version: '8.2'
          extensions: pdo, pdo_mysql, gd, mbstring, xml
      - run: composer install --prefer-dist --no-progress
      - run: composer test:unit

  lint:
    name: PHP Lint
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: shivammathur/setup-php@v2
        with:
          php-version: '8.2'
      - run: find src -name "*.php" -print0 | xargs -0 -n1 php -l
```

---

## 3. Frontend — `.github/workflows/ci.yml`

### Jobs

| Job | Descripción | Comando |
|---|---|---|
| **build** | Compila el proyecto con Vite | `npm run build` |
| **lint** | Ejecuta ESLint sobre `src/` | `npx eslint src` |

### Configuración

- **Node.js**: 20
- **Cache**: npm (automático con `actions/setup-node`)
- **Dependencias**: `npm ci` (instalación limpia desde lockfile)
- **Build**: `vite build` → genera `dist/`

### Archivo

```yaml
name: CI - Frontend

on:
  push:
    branches: [dev, master]
  pull_request:
    branches: [dev, master]

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run build

  lint:
    name: Lint
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npx eslint src --ext .js,.jsx
```

---

## 4. Deploy (manual)

El deploy sigue siendo manual (ver `DEPLOY.md`). Para automatizar:

### Opción A: GitHub Actions + FTP

```yaml
# .github/workflows/deploy.yml (futuro)
name: Deploy
on:
  push:
    branches: [master]
jobs:
  deploy-frontend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '20' }
      - run: npm ci && npm run build
      - uses: SamKirkland/FTP-Deploy-Action@v4.3.5
        with:
          server: ${{ secrets.FTP_SERVER }}
          username: ${{ secrets.FTP_USERNAME }}
          password: ${{ secrets.FTP_PASSWORD }}
          local-dir: ./dist/
          server-dir: /www/
```

### Opción B: Netlify / Vercel (frontend)

Conectar el repo a Netlify o Vercel para deploy automático en cada push a `master`.

---

## 5. Secrets necesarios (para deploy automatizado)

| Secret | Descripción | Dónde configurar |
|---|---|---|
| `FTP_SERVER` | Servidor FTP de alwaysdata | GitHub → Settings → Secrets |
| `FTP_USERNAME` | Usuario FTP | GitHub → Settings → Secrets |
| `FTP_PASSWORD` | Contraseña FTP | GitHub → Settings → Secrets |

---

## 6. Próximos pasos

- [ ] Agregar tests de integración con MySQL (service container en GitHub Actions).
- [ ] Agregar deploy automático a alwaysdata via FTP.
- [ ] Agregar notificación de Slack/Discord en fallos de CI.
- [ ] Agregar badge de CI en README.

---

## 7. Notas

- Los workflows se ejecutan en cada push y PR a `dev` o `master`.
- Los tests de integración (que requieren BD) no se ejecutan en CI por ahora.
- El deploy sigue siendo manual hasta configurar los secrets de FTP.
- Ver `DEPLOY.md` para el proceso de deploy manual.
