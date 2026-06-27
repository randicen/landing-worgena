# Worgena Landing Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Refactorizar el landing de Worgena a dirección editorial premium, manteniendo verbatim lo aprobado por el usuario y reformulando el resto con voz Steve Jobs.

**Architecture:** Vanilla HTML / CSS / JS sin frameworks. Refactor en lugar de `index.html`, `assets/css/tokens.css`, `assets/css/main.css`. Deploy target Cloudflare Pages sin cambios. Sin build step.

**Tech Stack:** HTML semántico, CSS custom (sin Tailwind), JavaScript vanilla, Google Fonts (Fraunces + Inter + JetBrains Mono).

## Global Constraints

- Stack: vanilla HTML / CSS / JS. NO React, NO Vite, NO Tailwind, NO build step.
- Tipografía: Fraunces display + Inter body + JetBrains Mono, vía Google Fonts.
- Color acento: `#0071E3` se mantiene como acento de marca (dosificado).
- Sin emojis en archivos. Sin lorem ipsum. Sin placeholders sin resolver.
- Hero, problema, solución (eyebrow + título + lead) y título features son VERBATIM del spec. Cualquier cambio requiere aprobación previa.
- Stats bar ELIMINADA por completo (sin números MIT NANDA / Thomson Reuters).
- Sin nombres de competidores en ninguna sección.
- Sin datos sin fuente verificable.
- Acentos en español correctos (pase completo al final).
- El proyecto NO es repo git, así que no hay `git commit`. Reemplazo por checkpoints de archivo (el implementador verifica que el archivo modificado carga sin errores antes de pasar al siguiente task).
- "Tests" en este plan = verificación visual en navegador + Lighthouse + grep checks. No hay unit tests porque no hay framework de tests.
- Deploy target sigue siendo Cloudflare Pages. No se toca `_redirects`, `_headers`, `robots.txt`, `sitemap.xml`, `public/*`.

---

## File Structure

| Archivo | Rol | Cambio |
|---|---|---|
| `index.html` | Página única | Refactor: head (fonts), todas las secciones |
| `assets/css/tokens.css` | Design tokens | Refactor completo |
| `assets/css/main.css` | Estilos por sección | Refactor: base, ritmo claro/oscuro, motion, accessibility |
| `assets/js/nav.js` | Sticky + mobile drawer | Sin cambios |
| `assets/js/scroll.js` | IntersectionObserver | Sin cambios |
| `README.md` | Documentación | Actualizar línea sobre fuentes |
| `docs/superpowers/specs/2026-06-26-worgena-landing-redesign.md` | Spec aprobado | Sin cambios (referencia) |

Archivos NO modificados: `robots.txt`, `sitemap.xml`, `_headers`, `_redirects`, `public/favicon.svg`, `public/og-image.svg`.

---

## Tasks

### Task 1: Cargar fuentes en el head del HTML

**Files:**
- Modify: `index.html:1-30` (head)

**Pasos:**

- [ ] Agregar dentro de `<head>`, antes de los `<link rel="stylesheet">` existentes:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,500..700;1,9..144,500..700&family=Inter:wght@400;500;600&family=JetBrains+Mono:wght@400;500&display=swap">
```

**Verificación:**
- Abrir `index.html` en navegador. DevTools > Network > Fonts. Confirmar que `Fraunces`, `Inter`, `JetBrains Mono` se cargan sin error 404.

---

### Task 2: Refactorizar `tokens.css` con los tokens del spec

**Files:**
- Modify: `assets/css/tokens.css` (archivo completo)

**Pasos:**

- [ ] Reemplazar todo el contenido de `tokens.css` con los tokens definidos en el spec sección "Design tokens":

```css
:root {
  /* Tipografía */
  --font-display: 'Fraunces', ui-serif, Georgia, serif;
  --font-body: 'Inter', -apple-system, BlinkMacSystemFont, system-ui, sans-serif;
  --font-mono: 'JetBrains Mono', ui-monospace, 'SF Mono', Menlo, monospace;

  /* Color - light surfaces */
  --ink: #0A0A0A;
  --ink-2: #1D1D1F;
  --ink-3: #6E6E73;
  --paper: #FAF9F6;
  --paper-pure: #FFFFFF;
  --line: #E5E2DC;
  --accent: #0071E3;
  --accent-soft: rgba(0, 113, 227, 0.08);

  /* Color - dark surfaces */
  --ink-inv: #FAF9F6;
  --ink-inv-2: #C7C5BF;
  --ink-inv-3: #8A8780;
  --night: #0A0A0A;
  --night-2: #18181A;
  --line-inv: rgba(255, 255, 255, 0.08);
  --accent-inv: #4A9EFF;

  /* Spacing */
  --space-1: 8px;
  --space-2: 12px;
  --space-3: 16px;
  --space-4: 20px;
  --space-5: 24px;
  --space-6: 28px;
  --space-7: 32px;
  --space-8: 40px;
  --space-9: 44px;
  --space-10: 48px;
  --space-11: 52px;
  --space-12: 56px;
  --space-13: 72px;
  --space-14: 96px;
  --space-15: 120px;
  --space-16: 160px;
  --space-17: 200px;

  /* Layout */
  --container-max: 1262px;
  --container-pad-mobile: 20px;
  --container-pad-tablet: 32px;
  --container-pad-desktop: 40px;

  /* Type scale */
  --fs-eyebrow: 13px;
  --fs-small: 14px;
  --fs-body: 18px;
  --fs-link: 18px;
  --fs-button: 16px;
  --fs-mono: 14px;
  --fs-h-tertiary: 24px;
  --fs-h-secondary: 40px;
  --fs-h-primary: 56px;
  --fs-display: 80px;
  --fs-display-xl: 104px;

  --lh-eyebrow: 18px;
  --lh-small: 20px;
  --lh-body: 28px;
  --lh-link: 24px;
  --lh-button: 24px;
  --lh-mono: 22px;
  --lh-h-tertiary: 32px;
  --lh-h-secondary: 48px;
  --lh-h-primary: 60px;
  --lh-display: 84px;
  --lh-display-xl: 104px;

  /* Easing */
  --ease-out: cubic-bezier(0.16, 1, 0.3, 1);
  --ease-smooth: cubic-bezier(0.4, 0, 0.2, 1);

  /* Radius */
  --radius-button: 9999px;
  --radius-input: 8px;
  --radius-card: 12px;
  --radius-large: 24px;

  /* Shadows (mantener los del archivo actual) */
  --shadow-subtle: 0 1px 3px rgba(0, 0, 0, 0.12);
  --shadow-medium: 0 4px 12px rgba(0, 0, 0, 0.15);
  --shadow-deep: 0 12px 32px rgba(0, 0, 0, 0.16);
}
```

**Verificación:**
- Abrir `index.html` en navegador. Confirmar que la página no se rompe visiblemente. Si `main.css` aún referencia tokens viejos, el archivo se verá mal pero no debe dar error de consola. Los fixes están en Tasks siguientes.

---

### Task 3: Refactorizar base de `main.css`

**Files:**
- Modify: `assets/css/main.css` (selector `body`, headings, contenedores, eyebrows, `.section-title`, `.section-lead`, agregar `.section-dark`)

**Pasos:**

- [ ] Reemplazar `body` con tipografía Inter, color `--ink`, background `--paper-pure`.
- [ ] Aplicar `font-family: var(--font-display)` a `h1`, `h2`, `h3`.
- [ ] Definir estilos para `.eyebrow`:

```css
.eyebrow {
  font-family: var(--font-body);
  font-size: var(--fs-eyebrow);
  line-height: var(--lh-eyebrow);
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--accent);
  font-weight: 500;
  display: inline-block;
}
```

- [ ] Definir `.section-title`:

```css
.section-title {
  font-family: var(--font-display);
  font-size: var(--fs-h-primary);
  line-height: var(--lh-h-primary);
  letter-spacing: -0.02em;
  font-weight: 500;
  color: var(--ink);
  margin: 0 0 var(--space-5) 0;
}
```

- [ ] Definir `.section-lead`:

```css
.section-lead {
  font-family: var(--font-body);
  font-size: var(--fs-body);
  line-height: var(--lh-body);
  color: var(--ink-2);
  max-width: 62ch;
  margin: 0 0 var(--space-10) 0;
}
```

- [ ] Definir `.section-dark`:

```css
.section-dark {
  background: var(--night);
  color: var(--ink-inv);
}
.section-dark .section-title { color: var(--ink-inv); }
.section-dark .section-lead { color: var(--ink-inv-2); }
.section-dark .eyebrow { color: var(--accent-inv); }
```

- [ ] Refactorizar `.container` con `max-width: var(--container-max)` y padding lateral responsivo.

**Verificación:**
- Abrir en navegador. Tipografía base cambia a Inter. Headings a Fraunces. Secciones oscuras (todavía sin asignar) tienen fondo negro cuando se les aplica `.section-dark`.

---

### Task 4: Refactorizar NAV

**Files:**
- Modify: `index.html:58-99`
- Modify: estilos nav en `main.css`

**Pasos:**

- [ ] Reemplazar el contenido de `<nav class="nav">` con la estructura existente pero ajustando el brand:

```html
<a href="#" class="nav-brand" aria-label="Worgena, inicio">
  <span class="nav-brand-mark" aria-hidden="true">
    <svg viewBox="0 0 32 32" fill="none">
      <path d="M8 8 L12 22 L16 13 L20 22 L24 8" stroke="#FFFFFF" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round"/>
    </svg>
  </span>
  <span class="nav-brand-text">Worgena</span>
</a>
```

- [ ] Aplicar `.nav-brand-text { font-family: var(--font-display); font-weight: 500; }`.
- [ ] Links del menú en Inter 14-15px (`font-size: 14-15px; font-weight: 500;`).
- [ ] Mantener sticky + frosted glass + mobile drawer existentes.
- [ ] CTA button "Agendar demo" mantiene color azul actual.

**Verificación:**
- Scroll en navegador: nav aparece con frosted glass al hacer scroll.
- Mobile (DevTools responsive 375px): drawer se abre al tocar hamburguesa.
- Brand "Worgena" se ve en serif Fraunces.

---

### Task 5: HERO (verbatim)

**Files:**
- Modify: `index.html:101-200`
- Modify: estilos hero en `main.css`

**Pasos:**

- [ ] Verificar que el texto del hero coincide verbatim con el spec. **No cambiar**:
  - Eyebrow: `Sistema operativo de trabajo para firmas`
  - H1: `El sistema operativo de <em>tu firma</em>.`
  - Sub: `Worgena automatiza el trabajo repetitivo para que tu equipo se quede con la estrategia y las relaciones con los clientes. Un espacio donde tu firma produce, edita y deja que el asistente trabaje también cuando no está.`
  - CTA primario: `Agendar demo de 20 min`
  - CTA secundario: `Cómo funciona`
- [ ] Aplicar al `.hero-headline`:

```css
.hero-headline {
  font-family: var(--font-display);
  font-size: var(--fs-display-xl);
  line-height: var(--lh-display-xl);
  letter-spacing: -0.03em;
  font-weight: 500;
  color: var(--ink);
  margin: 0 0 var(--space-6) 0;
}
.hero-headline em {
  font-style: italic;
  color: var(--accent);
}
```

- [ ] Aplicar al `.hero-sub`:

```css
.hero-sub {
  font-family: var(--font-body);
  font-size: var(--fs-body);
  line-height: var(--lh-body);
  color: var(--ink-2);
  max-width: 56ch;
  margin: 0 0 var(--space-10) 0;
}
```

- [ ] Responsive: en mobile (`<768px`), `.hero-headline` pasa a `--fs-display` y `line-height: var(--lh-display)`.
- [ ] Mockup SVG: ajustar container con `border-radius: var(--radius-large)` y `box-shadow: var(--shadow-medium)`.

**Verificación:**
- DevTools > Inspector: confirmar que `<em>` tiene `font-family: Fraunces` y `font-style: italic` (no `oblique`).
- Comparar texto del hero en navegador con el spec palabra por palabra (debe coincidir exactamente).
- Mobile (375px): headline no se desborda, mockup se ve.

---

### Task 6: PARA QUIÉN ES (reformular)

**Files:**
- Modify: `index.html:227-250`
- Modify: estilos `.easy-grid`, `.easy-card` en `main.css`

**Pasos:**

- [ ] Mantener estructura HTML, reemplazar contenido. Usar esta sección exacta:

```html
<section class="section" id="para-quien" aria-labelledby="para-quien-title">
  <div class="container">
    <span class="eyebrow">Empezar</span>
    <h2 class="section-title" id="para-quien-title">No necesitás saber de IA para aprovecharla.</h2>
    <p class="section-lead">Tu equipo carga un documento y recibe un primer borrador listo para firmar. No traduce lo que necesita a prompt engineering, no hace cursos, no lee manuales. El asistente trabaja en el idioma de la práctica, no en el idioma de la tecnología.</p>
    <div class="reveal-stagger easy-grid">
      <article class="easy-card">
        <div class="easy-num">01</div>
        <h3 class="easy-title">Hecho para Colombia, no para "el mundo".</h3>
        <p class="easy-body">Cuando la herramienta entiende el marco normativo colombiano, tu equipo no pierde tiempo traduciéndole el contexto. Worgena trabaja sobre la ley que ya está vigente acá — Estatuto Tributario, CST, jurisprudencia nacional, formatos DIAN.</p>
      </article>
      <article class="easy-card">
        <div class="easy-num">02</div>
        <h3 class="easy-title">Tu equipo produce desde el día uno.</h3>
        <p class="easy-body">Sin manuales, sin cursos, sin onboarding de tres semanas. La primera tarea sale al rato de empezar a usarlo. Lo único que tu equipo necesita saber es el caso que está trabajando.</p>
      </article>
      <article class="easy-card">
        <div class="easy-num">03</div>
        <h3 class="easy-title">Lo que aprende la firma, queda en la firma.</h3>
        <p class="easy-body">Cada criterio, validación o workflow que tu equipo diseña queda registrado y se ejecuta siempre. El conocimiento no se pierde cuando alguien renuncia, se jubila, o se cambia de bufete.</p>
      </article>
    </div>
  </div>
</section>
```

- [ ] Aplicar `.easy-card { border-radius: var(--radius-card); }`.
- [ ] Hover: `.easy-card:hover { transform: translateY(-2px); box-shadow: var(--shadow-medium); transition: all 200ms var(--ease-out); }`.

**Verificación:**
- Confirmar visualmente que las 3 cards tienen el texto del spec.
- Hover en navegador: la card se eleva 2px.

---

### Task 7: PROBLEMA (verbatim + dark theme)

**Files:**
- Modify: `index.html:253-263`
- Modify: estilos `.prose-block` en `main.css`

**Pasos:**

- [ ] Reemplazar la sección completa con:

```html
<section class="section section-dark" id="problema" aria-labelledby="problema-title">
  <div class="container">
    <span class="eyebrow">El problema</span>
    <h2 class="section-title" id="problema-title">La IA ya está en tu bufete. La pregunta es cómo se integra al negocio.</h2>
    <div class="prose-block reveal">
      <p>El 92% de los profesionales colombianos ya usa IA en su trabajo diario — chat por chat, prompt por prompt. La pregunta no es si tu firma adopta IA, sino si la está capturando como ventaja o se le está escapando como costo oculto.</p>
      <p><mark class="highlight">Las firmas que lo hacen bien rediseñan el trabajo alrededor de la IA: cada output queda trazado, cada decisión queda justificada, cada profesional pasa de hacer el trabajo mecánico a firmarlo.</mark> Las que lo hacen mal — y son la mayoría, según MIT NANDA 2025 — enchufan la IA al proceso viejo y descubren, meses después, que compraron la herramienta equivocada.</p>
      <p>La diferencia entre el 5% que escala y el 95% que no, no es la herramienta. Es el sistema alrededor de la herramienta.</p>
    </div>
  </div>
</section>
```

- [ ] Aplicar `.prose-block p { max-width: 62ch; color: var(--ink-inv-2); margin: 0 0 var(--space-6) 0; font-size: 19px; line-height: 32px; }`.
- [ ] Aplicar `.highlight { background: rgba(74, 158, 255, 0.08); color: inherit; padding: 2px 6px; border-radius: 4px; }`.

**Verificación:**
- Confirmar fondo `--night` en la sección.
- Confirmar texto claro legible (contraste AA con axe DevTools).
- Highlight visible sobre la frase marcada.

---

### Task 8: SOLUCIÓN (partial verbatim + reformular)

**Files:**
- Modify: `index.html:266-308`
- Modify: estilos `.pillars`, `.principles` en `main.css`

**Pasos:**

- [ ] Reemplazar la sección con:

```html
<section class="section" id="solucion" aria-labelledby="solucion-title">
  <div class="container">
    <span class="eyebrow">Cómo Worgena cambia tu trabajo</span>
    <h2 class="section-title" id="solucion-title">No es un chat que responde preguntas. Es el sistema operativo de tu firma.</h2>
    <p class="section-lead">Worgena no improvisa. Cada respuesta es un primer borrador listo para que un abogado o contador lo refine, cada cita es verificable contra la fuente, y cada decisión queda registrada en un audit log que se puede mostrar al cliente o regulador.</p>

    <div class="pillars reveal-stagger">
      <article class="pillar">
        <div class="pillar-num">01</div>
        <h3 class="pillar-title">La IA hace el primer borrador. Tu equipo firma.</h3>
        <p class="pillar-body">El trabajo mecánico — buscar la cláusula, compararla con la norma, identificar la inconsistencia — lo hace la IA en minutos. Tu equipo pasa el tiempo donde agrega valor: criterio, estrategia, firma.</p>
      </article>
      <article class="pillar">
        <div class="pillar-num">02</div>
        <h3 class="pillar-title">Otro agente revisa antes que vos.</h3>
        <p class="pillar-body">Antes de que tu equipo vea una respuesta, ya pasó por un verificador independiente que la cruza con la fuente. Es como tener un segundo par de ojos trabajando en otra sesión. Si la respuesta no se sostiene, no llega a tu escritorio.</p>
      </article>
      <article class="pillar">
        <div class="pillar-num">03</div>
        <h3 class="pillar-title">Lo que tu firma aprende una vez, lo ejecuta siempre.</h3>
        <p class="pillar-body">Un workflow que tu equipo diseñó una vez — cómo revisar contratos, cómo cruzar declaraciones, cómo armar minutas — se ejecuta cada vez que alguien lo necesita. La IA no parte de cero en cada caso.</p>
      </article>
    </div>

    <div class="principles reveal">
      <h3 class="principles-title">Tres principios que rigen todo lo demás.</h3>
      <ol class="principles-list">
        <li>
          <strong>El humano decide. La IA propone.</strong>
          <span>Ningún output sale sin que un profesional de tu firma lo revise y apruebe. La IA hace el primer borrador. Tu equipo firma.</span>
        </li>
        <li>
          <strong>La fuente manda. La cita se puede verificar.</strong>
          <span>Cada afirmación está vinculada a su fuente. Si no tiene fuente, no aparece. Si tiene fuente, se puede auditar.</span>
        </li>
        <li>
          <strong>Lo que carga tu firma, se queda en tu firma.</strong>
          <span>Tus datos, tus criterios y el contexto de tu equipo no se usan para entrenar nada fuera de tu control. Tu trabajo no se convierte en producto de otro.</span>
        </li>
      </ol>
    </div>
  </div>
</section>
```

- [ ] `.pillar-num` en Fraunces weight 500, `--fs-h-secondary`.
- [ ] `.pillar-title` en Fraunces weight 500, `--fs-h-tertiary`.

**Verificación:**
- Comparar texto verbatim del spec palabra por palabra (eyebrow + h2 + lead).
- Confirmar pilares y principios coinciden con el spec.

---

### Task 9: EL SISTEMA (reformular)

**Files:**
- Modify: `index.html:311-366`
- Modify: estilos `.sistema-grid`, `.sistema-card` en `main.css`

**Pasos:**

- [ ] Reemplazar sección con:

```html
<section class="section sistema" id="sistema" aria-labelledby="sistema-title">
  <div class="container">
    <span class="eyebrow">El sistema</span>
    <h2 class="section-title" id="sistema-title">Lo que hace un sistema operativo de firma.</h2>
    <p class="section-lead">Una app de chat responde preguntas. Un sistema operativo de firma hace el trabajo, lo registra y aprende de él. Estas son las cuatro capacidades que diferencian a Worgena.</p>

    <div class="sistema-grid reveal-stagger">
      <article class="sistema-card">
        <div class="sistema-icon">[mantener SVG actual de users]</div>
        <h3 class="sistema-title">Memoria compartida de la firma.</h3>
        <p class="sistema-body">Cuando un profesional aprueba un criterio o descubre algo nuevo, queda visible para los demás. La firma comparte contexto, decisiones y aprendizaje. Nadie duplica el trabajo que otro ya hizo.</p>
      </article>
      <article class="sistema-card">
        <div class="sistema-icon">[mantener SVG actual de clock]</div>
        <h3 class="sistema-title">Trabaja también cuando tu equipo no está.</h3>
        <p class="sistema-body">Producir y editar documentos desde donde se esté. Y mientras el equipo no está, el asistente sigue trabajando: revisa contratos, monitorea leyes nacionales, vigila el avance del equipo y alerta cuando algo cambia. La firma no se detiene cuando el equipo se detiene.</p>
      </article>
      <article class="sistema-card">
        <div class="sistema-icon">[mantener SVG actual de book]</div>
        <h3 class="sistema-title">Lee los documentos que tu firma ya tiene, no solo la ley pública.</h3>
        <p class="sistema-body">Worgena conoce la jurisprudencia, los estatutos y las plantillas que tu firma ya usa. La próxima vez que tu equipo enfrente un caso parecido, no arranca de cero. Aplica lo que ya aprendió sobre cómo trabaja tu firma.</p>
      </article>
      <article class="sistema-card">
        <div class="sistema-icon">[mantener SVG actual de layers]</div>
        <h3 class="sistema-title">Cada decisión queda registrada. Para responderte a vos y al cliente.</h3>
        <p class="sistema-body">Quién hizo qué, con qué modelo, cuándo y por qué. Cuando un cliente pregunta cómo se llegó a una conclusión, no hay que reconstruir el razonamiento — está trazado.</p>
      </article>
    </div>
  </div>
</section>
```

- [ ] `.sistema-card { border-radius: var(--radius-card); border: 1px solid var(--line); padding: var(--space-7); background: var(--paper-pure); }`.
- [ ] `.sistema-icon svg { width: 32px; height: 32px; color: var(--accent); }`.
- [ ] Hover sutil: `translateY(-2px)` + shadow.

**Verificación:**
- Confirmar las 4 cards con copy del spec.
- Hover: la card se eleva.

---

### Task 10: FEATURES (partial verbatim + reformular)

**Files:**
- Modify: `index.html:369-440`
- Modify: estilos `.features` en `main.css`

**Pasos:**

- [ ] Reemplazar sección con (mantener SVGs existentes, solo cambiar texto de las cards):

```html
<section class="section" id="features" aria-labelledby="features-title">
  <div class="container">
    <span class="eyebrow">Funcionalidades</span>
    <h2 class="section-title" id="features-title">Lo que Worgena hace por tu firma.</h2>
    <div class="features reveal-stagger">
      <article class="feature">
        <div class="feature-icon">[mantener SVG actual de file-text]</div>
        <h3 class="feature-title">Análisis de cláusulas con cita a la fuente.</h3>
        <p class="feature-body">Cada cambio propuesto viene con la norma colombiana que lo respalda. Tu equipo verifica antes de firmar, no después.</p>
      </article>
      <article class="feature">
        <div class="feature-icon">[mantener SVG actual de grid]</div>
        <h3 class="feature-title">Revisión masiva para due diligence.</h3>
        <p class="feature-body">Cientos de contratos en una vista. Diferencias marcadas, riesgo por fila, exportación a Excel. Lo que antes llevaba semanas, en horas.</p>
      </article>
      <article class="feature">
        <div class="feature-icon">[mantener SVG actual de trending-up]</div>
        <h3 class="feature-title">Workflows que se acumulan.</h3>
        <p class="feature-body">Cada tarea queda registrada con su flujo. Lo que tu firma aprende una vez, lo ejecuta cien veces sin repetir el prompt.</p>
      </article>
      <article class="feature">
        <div class="feature-icon">[mantener SVG actual de layers]</div>
        <h3 class="feature-title">Memoria de la firma entre sesiones.</h3>
        <p class="feature-body">El criterio de tu firma se mantiene de un día para otro. El abogado que vuelve mañana no tiene que re-explicar el contexto del caso.</p>
      </article>
      <article class="feature">
        <div class="feature-icon">[mantener SVG actual de edit]</div>
        <h3 class="feature-title">Editor en lienzo.</h3>
        <p class="feature-body">Cambios manuales y cambios asistidos conviven en el mismo documento. Tu equipo edita, la IA sugiere, todo en un mismo lugar.</p>
      </article>
      <article class="feature">
        <div class="feature-icon">[mantener SVG actual de check-square]</div>
        <h3 class="feature-title">Audit log inmutable.</h3>
        <p class="feature-body">Quién hizo qué, cuándo, con qué modelo. Cada decisión queda registrada para responder ante clientes y reguladores.</p>
      </article>
    </div>
  </div>
</section>
```

**Verificación:**
- Confirmar título verbatim: "Lo que Worgena hace por tu firma."
- Confirmar 6 features con copy del spec.

---

### Task 11: SEGURIDAD (reformular)

**Files:**
- Modify: `index.html:443-468`
- Modify: estilos `.qa-list`, `.qa` en `main.css`

**Pasos:**

- [ ] Reemplazar sección con:

```html
<section class="section compliance" id="seguridad" aria-labelledby="seguridad-title">
  <div class="container">
    <span class="eyebrow">Seguridad y datos</span>
    <h2 class="section-title" id="seguridad-title">Las preguntas que tu cliente te va a hacer. Con respuesta.</h2>
    <p class="section-lead">Cuando un cliente pregunta cómo se protege la información de su caso, necesita una respuesta, no una promesa. Estas son las preguntas más comunes y cómo las responde Worgena.</p>

    <div class="qa-list reveal-stagger">
      <article class="qa">
        <h3 class="qa-q">¿Dónde viven los datos?</h3>
        <p class="qa-a">En infraestructura propia, con respaldo en Colombia. Lo que carga tu firma no se mezcla con lo de otra firma. Cumplimos con Habeas Data conforme a la Ley 1581 de 2012.</p>
      </article>
      <article class="qa">
        <h3 class="qa-q">¿Worgena usa lo que mi firma carga para entrenar otros modelos?</h3>
        <p class="qa-a">No. Los modelos que Worgena usa no entrenan con tus datos. Tu trabajo no se convierte en producto de otro.</p>
      </article>
      <article class="qa">
        <h3 class="qa-q">¿Quién puede entrar a la cuenta de mi firma?</h3>
        <p class="qa-a">Autenticación de dos factores. Cada sesión queda registrada con quién entró, cuándo y desde dónde.</p>
      </article>
      <article class="qa">
        <h3 class="qa-q">¿Qué pasa si Worgena se equivoca?</h3>
        <p class="qa-a">Tu equipo es quien firma. Worgena propone, verifica y traza. La responsabilidad final es del profesional. Y el audit log permite reconstruir cada paso si algo se cuestiona.</p>
      </article>
    </div>
  </div>
</section>
```

- [ ] `.qa-q { font-family: var(--font-display); font-weight: 500; font-size: var(--fs-h-tertiary); color: var(--ink); }`.
- [ ] `.qa-a { color: var(--ink-2); }`.

**Verificación:**
- Confirmar las 4 QA con copy del spec.

---

### Task 12: CASOS (reformular)

**Files:**
- Modify: `index.html:471-494`
- Modify: estilos `.use-cases` en `main.css`

**Pasos:**

- [ ] Reemplazar sección con:

```html
<section class="section" id="casos" aria-labelledby="casos-title">
  <div class="container">
    <span class="eyebrow">Por tipo de firma</span>
    <h2 class="section-title" id="casos-title">Aplicado a la práctica que tu firma ya hace.</h2>
    <p class="section-lead">Worgena se adapta al trabajo que tu firma ya hace. No te pedimos cambiar cómo trabajás.</p>
    <div class="use-cases reveal-stagger">
      <article class="use-case">
        <div class="use-case-role">Abogado o bufete</div>
        <h3 class="use-case-title">Cincuenta contratos en una mañana.</h3>
        <p class="use-case-body">Revisión masiva de contratos laborales, marcación de cláusulas de riesgo y propuesta de cambios con cita al CST y la jurisprudencia aplicable.</p>
      </article>
      <article class="use-case">
        <div class="use-case-role">Contador o firma contable</div>
        <h3 class="use-case-title">Cruzar declaraciones con el Estatuto Tributario actualizado.</h3>
        <p class="use-case-body">Análisis de obligaciones tributarias vigentes, alertas sobre cambios recientes en normativa DIAN y consistencia entre lo que tu firma declara y lo que el cliente declara.</p>
      </article>
      <article class="use-case">
        <div class="use-case-role">Notaría o administración</div>
        <h3 class="use-case-title">Minutas con citas a los estatutos, autenticaciones trazadas en segundos.</h3>
        <p class="use-case-body">Redacción de actas, contratos comerciales y documentos societarios con verificación de cláusulas contra los estatutos y el Código de Comercio.</p>
      </article>
    </div>
  </div>
</section>
```

- [ ] `.use-case-role { font-family: var(--font-mono); font-size: 12px; text-transform: uppercase; letter-spacing: 0.08em; color: var(--accent); }`.
- [ ] `.use-case-title { font-family: var(--font-display); font-weight: 500; font-size: var(--fs-h-tertiary); }`.

**Verificación:**
- Confirmar los 3 use cases con copy del spec.

---

### Task 13: PRICING (reformular)

**Files:**
- Modify: `index.html:497-504`
- Modify: estilos `.pricing` en `main.css`

**Pasos:**

- [ ] Reemplazar sección con:

```html
<section class="section pricing" id="precios" aria-labelledby="precios-title">
  <div class="container">
    <span class="eyebrow">Pricing</span>
    <h2 class="section-title" id="precios-title">El precio se mide en capacidad, no en horas.</h2>
    <p class="section-lead">Cobramos por capacidad de negocio generada, no por horas trabajadas ni por cantidad de documentos. Una llamada de veinte minutos nos alcanza para armar un plan a la medida de tu firma.</p>
    <p class="pricing-note">Si no genera valor para tu equipo, no seguís. Sin cláusulas de permanencia abusivas.</p>
  </div>
</section>
```

**Verificación:**
- Confirmar copy del spec.

---

### Task 14: CTA FINAL (reformular + dark theme)

**Files:**
- Modify: `index.html:507-590`
- Modify: estilos `.cta-final` en `main.css`

**Pasos:**

- [ ] Reemplazar la sección con (mantener estructura de canales + form, cambiar copy):

```html
<section class="cta-final section-dark" id="contacto" aria-labelledby="contacto-title">
  <div class="container">
    <div class="cta-final-inner">
      <span class="eyebrow">Empezá</span>
      <h2 class="section-title" id="contacto-title">Veinte minutos para entender tu caso.</h2>
      <p class="section-lead">Te contactamos en menos de 24 horas hábiles. Sin compromiso, sin venta forzada. Si Worgena no aplica para tu firma, te lo decimos.</p>

      <div class="contact-channels reveal">
        <a href="https://wa.me/573008667362?text=Hola%2C%20me%20interesa%20agendar%20una%20demo%20de%20Worgena" class="contact-channel" target="_blank" rel="noopener noreferrer">
          <span class="contact-channel-icon">[mantener SVG WhatsApp]</span>
          <span class="contact-channel-text">
            <strong>WhatsApp</strong>
            <small>+57 300 866 7362 — respuesta rápida</small>
          </span>
        </a>
        <a href="tel:+573008667362" class="contact-channel">
          <span class="contact-channel-icon">[mantener SVG teléfono]</span>
          <span class="contact-channel-text">
            <strong>Teléfono</strong>
            <small>+57 300 866 7362</small>
          </span>
        </a>
        <a href="mailto:japabontorres@gmail.com?subject=Interesado%20en%20Worgena" class="contact-channel">
          <span class="contact-channel-icon">[mantener SVG email]</span>
          <span class="contact-channel-text">
            <strong>Email</strong>
            <small>japabontorres@gmail.com</small>
          </span>
        </a>
      </div>

      <div class="or-divider"><span>o completa el formulario</span></div>

      <form class="lead-form" id="lead-form" novalidate>
        <div class="field">
          <label for="nombre">Tu nombre</label>
          <input type="text" id="nombre" name="nombre" placeholder="Ana Ramírez" required autocomplete="name" />
        </div>
        <div class="field">
          <label for="email">Email corporativo</label>
          <input type="email" id="email" name="email" placeholder="ana@bufete.co" required autocomplete="email" />
        </div>
        <div class="field field-full">
          <label for="firma">Nombre de la firma</label>
          <input type="text" id="firma" name="firma" placeholder="Ramírez &amp; Asociados" required autocomplete="organization" />
        </div>
        <div class="field field-full">
          <label for="tamano">Tamaño de la firma</label>
          <select id="tamano" name="tamano" required>
            <option value="" disabled selected>Elegí una opción</option>
            <option value="1-5">1 a 5 profesionales</option>
            <option value="6-20">6 a 20 profesionales</option>
            <option value="21-50">21 a 50 profesionales</option>
            <option value="50+">Más de 50 profesionales</option>
          </select>
        </div>
        <div class="submit">
          <button type="submit" class="btn btn-primary">Reservar llamada de 20 min</button>
        </div>
        <p class="fineprint">Al enviar este formulario se abre tu cliente de correo con los datos pre-cargados. No almacenamos nada en el navegador.</p>
      </form>

      <div class="what-next reveal">
        <h3 class="what-next-title">Qué pasa después</h3>
        <ol class="what-next-list">
          <li>Te contactamos en menos de 24 horas hábiles para confirmar horario.</li>
          <li>En la llamada entendemos tu práctica y vemos si Worgena aplica.</li>
          <li>Si sí, configuramos un piloto de 30 días con tus primeros tres workflows.</li>
        </ol>
      </div>
    </div>
  </div>
</section>
```

- [ ] Confirmar que `.cta-final` tiene fondo `--night` (heredado por `.section-dark`).
- [ ] `.contact-channel` con borde `--line-inv` y hover sutil.
- [ ] Inputs del form con `background: transparent; border: 1px solid var(--line-inv); color: var(--ink-inv);`.

**Verificación:**
- Confirmar copy del spec.
- Confirmar fondo oscuro y contraste legible.
- Testear form: completar campos → click submit → debe abrir cliente de correo.

---

### Task 15: FOOTER (mantener + pase de acentos)

**Files:**
- Modify: `index.html:592-639`
- Modify: estilos `.footer` en `main.css`

**Pasos:**

- [ ] Brand "Worgena" en Fraunces (`.footer .nav-brand-text`).
- [ ] Tagline reemplazar con: `El sistema operativo de tu firma. Automatiza el trabajo repetitivo. Hecho en Colombia.`
- [ ] Mantener columnas y links existentes.
- [ ] Bottom bar: `© 2026 Worgena · Medellín, Colombia` (mantener).

**Verificación:**
- Confirmar tagline y brand en serif Fraunces.

---

### Task 16: Motion y accesibilidad

**Files:**
- Modify: `assets/css/main.css`

**Pasos:**

- [ ] Verificar que `.reveal` y `.reveal-stagger` usan `--ease-out`:

```css
.reveal,
.reveal-stagger > * {
  opacity: 0;
  transform: translateY(8px);
  transition: opacity 600ms var(--ease-out), transform 600ms var(--ease-out);
}
.reveal.is-visible,
.reveal-stagger.is-visible > * {
  opacity: 1;
  transform: translateY(0);
}
```

- [ ] Stagger delay: `.reveal-stagger.is-visible > *:nth-child(1) { transition-delay: 0ms; } .reveal-stagger.is-visible > *:nth-child(2) { transition-delay: 80ms; } ...` hasta `nth-child(6)`.

- [ ] Agregar reduced-motion:

```css
@media (prefers-reduced-motion: reduce) {
  .reveal,
  .reveal-stagger > * {
    opacity: 1;
    transform: none;
    transition: none;
  }
}
```

- [ ] Focus visible global:

```css
:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 2px;
}
```

**Verificación:**
- DevTools > Rendering > Emulate "prefers-reduced-motion: reduce". Confirmar que no hay animaciones.
- Tab en navegador. Confirmar outline visible en todos los elementos focuseables.

---

### Task 17: Pase de acentos completo

**Files:**
- Modify: `index.html`

**Pasos:**

- [ ] Búsqueda manual en `index.html` de las siguientes palabras mal escritas (deben tener acento donde corresponda): solución, sección, navegación, información, auditoría, práctica, también, después, día,apié,apié.
- [ ] Corregir cualquier acento faltante que aparezca.
- [ ] Verificar especialmente:
  - `Worgena no improvisa` (no "Worgena no improviza" — pero el spec dice "improvisa" sin tilde, mantener verbatim)
  - `podés` no debe aparecer (es voseo argentino)
  - `click` está OK (es anglicismo aceptado)

**Verificación:**
- Búsqueda con grep o DevTools console: palabras clave renderizan correctamente.

---

### Task 18: Verificación final

**Files:**
- todos

**Pasos:**

- [ ] Lighthouse audit en navegador (DevTools > Lighthouse). Meta: Performance ≥95, Accessibility ≥95, Best Practices ≥95, SEO = 100.
- [ ] Verificación visual desktop (1440px) y mobile (375px). Capturar screenshot mental de cada sección.
- [ ] Ejecutar checklist del spec (14 puntos).
- [ ] Stats bar eliminada. Verificar con grep que `stat-number`, `stats-grid`, `eyebrow-stats` no aparecen en HTML final.
- [ ] Sin nombres de competidores en HTML. Verificar con grep que `Harvey`, `Legora`, `Dynamics` no aparecen.
- [ ] Sin datos sin fuente. Verificar con grep que `MIT NANDA`, `Thomson Reuters`, `US$19k`, `5% de las empresas` no aparecen en HTML.
- [ ] Confirmar 0 errores críticos en consola del navegador.
- [ ] Confirmar que todos los SVGs (hero mockup, icons) renderizan.
- [ ] Verificar que el `<em>` "tu firma" del hero está en cursiva real de Fraunces (font-style: italic, no synthetic oblique).
- [ ] Verificar `<html lang="es">`.
- [ ] Verificar Schema.org Organization en el JSON-LD.
- [ ] Verificar que las 4 secciones verbatim (hero, problema, solución partial, features title) coinciden con el spec.

**Verificación:**
- Checklist completa con tildes verdes en cada ítem.

---

## Out of scope (recordatorio)

- No migrar a React, Vite, Tailwind o Framer builder.
- No reemplazar el form mailto por Cloudflare Worker.
- No agregar analytics, dark mode toggle, multi-página.
- No tocar `robots.txt`, `sitemap.xml`, `_headers`, `_redirects`, `public/*`.
- No cambiar el deploy target (Cloudflare Pages).