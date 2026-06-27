# Worgena — Landing Page

One-pager estático para Worgena. Sin build step, sin dependencias de runtime, performance máxima.

## Stack

- HTML semántico
- CSS custom (variables en `assets/css/tokens.css` + estilos en `assets/css/main.css`)
- JavaScript vanilla (`assets/js/nav.js`, `assets/js/scroll.js`)
- Sin Tailwind, sin frameworks, sin tracking de terceros

## Deploy a Cloudflare Pages (sin Git)

1. Entrá a https://dash.cloudflare.com → Pages.
2. Click en **Create application** → **Pages** → **Upload assets** (drag & drop).
3. Seleccioná toda la carpeta `landing-worgena/` (no la carpeta padre).
4. Project name: `worgena`. Subdomain será `worgena.pages.dev`.
5. Click **Deploy site**. En ~30 segundos tenés la URL pública.

## Deploy a Cloudflare Pages (con Git, recomendado)

1. Subí esta carpeta a un repo de GitHub/GitLab (ej. `worgena-landing`).
2. En Cloudflare Pages → **Create application** → **Connect to Git**.
3. Seleccioná el repo. Build command: vacío. Build output: `/` (raíz).
4. Cada push a `main` redeplega automáticamente.

## Custom domain (cuando compres worgena.com)

1. Comprá el dominio (Cloudflare Registrar es lo más simple).
2. Pages → tu proyecto → **Custom domains** → **Set up a custom domain**.
3. Agregá `worgena.com` y `www.worgena.com`.
4. El archivo `_redirects` ya está configurado para redirigir `www` → apex.

## Verificación post-deploy

- [ ] Abrir la URL pública en Chrome y Safari (desktop y mobile)
- [ ] Lighthouse: Performance ≥ 95, Accessibility ≥ 95, Best Practices ≥ 95, SEO = 100
- [ ] Probar el form: completar campos → click "Reservar llamada" → debe abrir el cliente de correo con los datos pre-cargados
- [ ] Verificar sticky nav: scrollear y comprobar que aparece frosted glass

## Estructura

```
landing-worgena/
├── index.html              # Página única
├── robots.txt
├── sitemap.xml
├── _headers                # Security headers + cache
├── _redirects              # www → apex, dominio custom → pages.dev
├── public/
│   ├── favicon.svg         # W geométrica en azul Apple
│   └── og-image.svg        # 1200×630 social preview (placeholder)
├── assets/
│   ├── css/
│   │   ├── tokens.css      # Variables del design system
│   │   └── main.css        # Estilos
│   └── js/
│       ├── scroll.js       # IntersectionObserver
│       └── nav.js          # Sticky + frosted glass + mobile drawer
└── README.md
```

## Próximos pasos (fuera de v1)

- Reemplazar og-image.svg por un PNG 1200×630 real (algunas redes sociales no aceptan SVG en og:image).
- Configurar analytics cuando se decida el stack (Plausible, Cloudflare Analytics, etc.).
- Reemplazar el form mailto por un Cloudflare Worker cuando haya > 5 leads por semana.
- Agregar dominio custom cuando se compre.
- Multi-página cuando haga falta (/precios público, /seguridad enterprise, /blog).