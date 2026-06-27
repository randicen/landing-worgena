# Worgena Landing Redesign — Design Spec

**Date:** 2026-06-26
**Status:** Draft, awaiting user approval
**Stack:** vanilla HTML / CSS / JS (no React, no Vite, no Framer builder)

---

## Goal

Refactorizar el landing actual de Worgena para llevarlo de "Apple-ish genérico" a "editorial premium" estilo Linear / Vercel. Mantener verbatim lo que el usuario aprobó. Reformular el resto con voz Steve Jobs: traducir lo técnico en valor para el socio dueño de una firma legal o contable en Colombia.

## Non-goals (out of scope)

- Cambiar de stack (sigue vanilla HTML / CSS / JS).
- Migrar a Framer builder, React, Vite o cualquier framework.
- Cambiar el deploy target (sigue Cloudflare Pages).
- Cambiar el azul de marca `#0071E3` como acento (se mantiene, se dosifica).
- Reescribir el código del formulario ni la integración `mailto:`.
- Agregar analytics, dark mode toggle, multi-página.

## Direction

**Editorial premium / Linear-Vercel aesthetic.**

- Tipografía grande (display 80-104px en desktop), jerarquía clara entre display, body y eyebrow.
- Paleta restringida: off-white + ink + un acento. Mucho whitespace.
- Alternancia rítmica de secciones claras y oscuras para crear tensión visual.
- Animaciones mínimas (scroll reveal + micro-hover). Sin nada decorativo.
- Lenguaje directo, sin jerga, sin buzzwords. Cada feature técnica traducida a su valor para el socio.

---

## Design tokens

### Tipografía

- **Display:** Fraunces (Google Fonts, OFL). Variable axes: opsz, wght, SOFT, WONK. Uso: headlines h1/h2 y los números grandes cuando queden. Pesos 500-700. Tiene itálica real (se usa para `<em>`).
- **Body:** Inter (Google Fonts, OFL). Pesos 400, 500, 600.
- **Mono:** JetBrains Mono (Google Fonts, OFL). Uso mínimo, solo donde haya dato o número en contexto técnico.

```css
--font-display: 'Fraunces', ui-serif, Georgia, serif;
--font-body: 'Inter', -apple-system, BlinkMacSystemFont, system-ui, sans-serif;
--font-mono: 'JetBrains Mono', ui-monospace, 'SF Mono', Menlo, monospace;
```

Preconnect en el `<head>`:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,500..700;1,9..144,500..700&family=Inter:wght@400;500;600&family=JetBrains+Mono:wght@400;500&display=swap">
```

### Color

```css
/* Light surfaces */
--ink: #0A0A0A;          /* primary text on light */
--ink-2: #1D1D1F;        /* secondary text on light */
--ink-3: #6E6E73;        /* tertiary text on light */
--paper: #FAF9F6;        /* off-white warm bg */
--paper-pure: #FFFFFF;   /* pure white for cards */
--line: #E5E2DC;         /* warm border */
--accent: #0071E3;       /* brand blue — usado con moderación */
--accent-soft: rgba(0, 113, 227, 0.08);

/* Dark surfaces */
--ink-inv: #FAF9F6;      /* primary text on dark */
--ink-inv-2: #C7C5BF;    /* secondary text on dark */
--ink-inv-3: #8A8780;    /* tertiary text on dark */
--night: #0A0A0A;        /* deep black bg */
--night-2: #18181A;      /* slightly lifted black */
--line-inv: rgba(255, 255, 255, 0.08);
--accent-inv: #4A9EFF;   /* brighter blue on dark, para legibilidad */
```

### Spacing

Mantengo la escala de 8px. Agrego dos stops arriba para que el hero respire.

```css
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
--space-16: 160px;   /* hero top padding */
--space-17: 200px;   /* hero display breathing */
```

### Type scale (editorial)

```css
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
```

Eyebrow va con `letter-spacing: 0.08em; text-transform: uppercase;`.

### Layout

```css
--container-max: 1262px;
--container-pad-mobile: 20px;
--container-pad-tablet: 32px;
--container-pad-desktop: 40px;
```

### Easing

```css
--ease-out: cubic-bezier(0.16, 1, 0.3, 1);
--ease-smooth: cubic-bezier(0.4, 0, 0.2, 1);
```

### Radius

```css
--radius-button: 9999px;
--radius-input: 8px;
--radius-card: 12px;     /* nuevo: cards con radius sutil */
--radius-large: 24px;    /* nuevo: cards grandes, hero mockup */
```

---

## Sections (canonical order)

### 1. NAV

Mantener estructura. Ajustes:
- Tipografía Inter 14-15px en links.
- Brand mark con Fraunces (en vez de Inter).
- Sticky frosted glass se mantiene.
- Mobile drawer se mantiene.

### 2. HERO — verbatim

**Texto exacto (no modificar):**
- Eyebrow: `Sistema operativo de trabajo para firmas`
- H1: `El sistema operativo de <em>tu firma</em>.`
- Sub: `Worgena automatiza el trabajo repetitivo para que tu equipo se quede con la estrategia y las relaciones con los clientes. Un espacio donde tu firma produce, edita y deja que el asistente trabaje también cuando no está.`
- CTA primario: `Agendar demo de 20 min`
- CTA secundario: `Cómo funciona`

**Tratamiento visual:**
- Background `--paper` o `--paper-pure`.
- H1 en Fraunces, weight 500, size `--fs-display-xl` desktop, `--fs-display` mobile.
- `<em>` "tu firma" en cursiva real de Fraunces, color `--accent`.
- Sub a `--fs-body`, color `--ink-2`, max-width 56ch.
- Mockup SVG: bordes más suaves, sombra sutil, escala ligeramente mayor. Sin cambios funcionales al SVG actual.

### 3. PARA QUIÉN ES — reformular

**Título:** `No necesitás saber de IA para aprovecharla.`
**Sub:** `Tu equipo carga un documento y recibe un primer borrador listo para firmar. No traduce lo que necesita a prompt engineering, no hace cursos, no lee manuales. El asistente trabaja en el idioma de la práctica, no en el idioma de la tecnología.`

**3 cards (reformuladas):**

**01 · Hecho para Colombia, no para "el mundo".**
Cuando la herramienta entiende el marco normativo colombiano, tu equipo no pierde tiempo traduciéndole el contexto. Worgena trabaja sobre la ley que ya está vigente acá — Estatuto Tributario, CST, jurisprudencia nacional, formatos DIAN.

**02 · Tu equipo produce desde el día uno.**
Sin manuales, sin cursos, sin onboarding de tres semanas. La primera tarea sale al rato de empezar a usarlo. Lo único que tu equipo necesita saber es el caso que está trabajando.

**03 · Lo que aprende la firma, queda en la firma.**
Cada criterio, validación o workflow que tu equipo diseña queda registrado y se ejecuta siempre. El conocimiento no se pierde cuando alguien renuncia, se jubila, o se cambia de bufete.

### 4. PROBLEMA — verbatim

**Texto exacto (no modificar):**

- Eyebrow: `El problema`
- H2: `La IA ya está en tu bufete. La pregunta es cómo se integra al negocio.`
- Párrafo 1: `El 92% de los profesionales colombianos ya usa IA en su trabajo diario — chat por chat, prompt por prompt. La pregunta no es si tu firma adopta IA, sino si la está capturando como ventaja o se le está escapando como costo oculto.`
- Párrafo 2: `Las firmas que lo hacen bien rediseñan el trabajo alrededor de la IA: cada output queda trazado, cada decisión queda justificada, cada profesional pasa de hacer el trabajo mecánico a firmarlo. Las que lo hacen mal — y son la mayoría, según MIT NANDA 2025 — enchufan la IA al proceso viejo y descubren, meses después, que compraron la herramienta equivocada.`
- Párrafo 3: `La diferencia entre el 5% que escala y el 95% que no, no es la herramienta. Es el sistema alrededor de la herramienta.`

**Tratamiento visual:**
- **Fondo `--night`**, texto `--ink-inv`. Contraste dramático.
- Display Fraunces color `--ink-inv`.
- Párrafos en Inter 18-20px, color `--ink-inv-2`, max-width 62ch.
- Highlight sutil en la frase clave del segundo párrafo (background `--accent-inv` con 8% alpha).

### 5. SOLUCIÓN — partial verbatim + reformular

**Mantener verbatim:**
- Eyebrow: `Cómo Worgena cambia tu trabajo`
- H2: `No es un chat que responde preguntas. Es el sistema operativo de tu firma.`
- Lead: `Worgena no improvisa. Cada respuesta es un primer borrador listo para que un abogado o contador lo refine, cada cita es verificable contra la fuente, y cada decisión queda registrada en un audit log que se puede mostrar al cliente o regulador.`

**3 pilares (reformulados):**

**01 · La IA hace el primer borrador. Tu equipo firma.**
El trabajo mecánico — buscar la cláusula, compararla con la norma, identificar la inconsistencia — lo hace la IA en minutos. Tu equipo pasa el tiempo donde agrega valor: criterio, estrategia, firma.

**02 · Otro agente revisa antes que vos.**
Antes de que tu equipo vea una respuesta, ya pasó por un verificador independiente que la cruza con la fuente. Es como tener un segundo par de ojos trabajando en otra sesión. Si la respuesta no se sostiene, no llega a tu escritorio.

**03 · Lo que tu firma aprende una vez, lo ejecuta siempre.**
Un workflow que tu equipo diseñó una vez — cómo revisar contratos, cómo cruzar declaraciones, cómo armar minutas — se ejecuta cada vez que alguien lo necesita. La IA no parte de cero en cada caso.

**3 principios (reformulados):**

1. **El humano decide. La IA propone.**
   Ningún output sale sin que un profesional de tu firma lo revise y apruebe. La IA hace el primer borrador. Tu equipo firma.

2. **La fuente manda. La cita se puede verificar.**
   Cada afirmación está vinculada a su fuente. Si no tiene fuente, no aparece. Si tiene fuente, se puede auditar.

3. **Lo que carga tu firma, se queda en tu firma.**
   Tus datos, tus criterios y el contexto de tu equipo no se usan para entrenar nada fuera de tu control. Tu trabajo no se convierte en producto de otro.

### 6. EL SISTEMA — reformular

**Título:** `Lo que hace un sistema operativo de firma.`
**Sub:** `Una app de chat responde preguntas. Un sistema operativo de firma hace el trabajo, lo registra y aprende de él. Estas son las cuatro capacidades que diferencian a Worgena.`

**4 cards (reformuladas):**

**Memoria compartida de la firma.**
Cuando un profesional aprueba un criterio o descubre algo nuevo, queda visible para los demás. La firma comparte contexto, decisiones y aprendizaje. Nadie duplica el trabajo que otro ya hizo.

**Trabaja también cuando tu equipo no está.**
Producir y editar documentos desde donde se esté. Y mientras el equipo no está, el asistente sigue trabajando: revisa contratos, monitorea leyes nacionales, vigila el avance del equipo y alerta cuando algo cambia. La firma no se detiene cuando el equipo se detiene.

**Lee los documentos que tu firma ya tiene, no solo la ley pública.**
Worgena conoce la jurisprudencia, los estatutos y las plantillas que tu firma ya usa. La próxima vez que tu equipo enfrente un caso parecido, no arranca de cero. Aplica lo que ya aprendió sobre cómo trabaja tu firma.

**Cada decisión queda registrada. Para responderte a vos y al cliente.**
Quién hizo qué, con qué modelo, cuándo y por qué. Cuando un cliente pregunta cómo se llegó a una conclusión, no hay que reconstruir el razonamiento — está trazado.

### 7. FEATURES — partial verbatim + reformular

**Mantener verbatim:** H2 `Lo que Worgena hace por tu firma.`

**6 features (reformuladas):**

1. **Análisis de cláusulas con cita a la fuente.**
   Cada cambio propuesto viene con la norma colombiana que lo respalda. Tu equipo verifica antes de firmar, no después.

2. **Revisión masiva para due diligence.**
   Cientos de contratos en una vista. Diferencias marcadas, riesgo por fila, exportación a Excel. Lo que antes llevaba semanas, en horas.

3. **Workflows que se acumulan.**
   Cada tarea queda registrada con su flujo. Lo que tu firma aprende una vez, lo ejecuta cien veces sin repetir el prompt.

4. **Memoria de la firma entre sesiones.**
   El criterio de tu firma se mantiene de un día para otro. El abogado que vuelve mañana no tiene que re-explicar el contexto del caso.

5. **Editor en lienzo.**
   Cambios manuales y cambios asistidos conviven en el mismo documento. Tu equipo edita, la IA sugiere, todo en un mismo lugar.

6. **Audit log inmutable.**
   Quién hizo qué, cuándo, con qué modelo. Cada decisión queda registrada para responder ante clientes y reguladores.

### 8. SEGURIDAD — reformular

**Título:** `Las preguntas que tu cliente te va a hacer. Con respuesta.`
**Sub:** `Cuando un cliente pregunta cómo se protege la información de su caso, necesita una respuesta, no una promesa. Estas son las preguntas más comunes y cómo las responde Worgena.`

**4 QA (reformuladas):**

**¿Dónde viven los datos?**
En infraestructura propia, con respaldo en Colombia. Lo que carga tu firma no se mezcla con lo de otra firma. Cumplimos con Habeas Data conforme a la Ley 1581 de 2012.

**¿Worgena usa lo que mi firma carga para entrenar otros modelos?**
No. Los modelos que Worgena usa no entrenan con tus datos. Tu trabajo no se convierte en producto de otro.

**¿Quién puede entrar a la cuenta de mi firma?**
Autenticación de dos factores. Cada sesión queda registrada con quién entró, cuándo y desde dónde.

**¿Qué pasa si Worgena se equivoca?**
Tu equipo es quien firma. Worgena propone, verifica y traza. La responsabilidad final es del profesional. Y el audit log permite reconstruir cada paso si algo se cuestiona.

### 9. CASOS — reformular

**Título:** `Aplicado a la práctica que tu firma ya hace.`
**Sub:** `Worgena se adapta al trabajo que tu firma ya hace. No te pedimos cambiar cómo trabajás.`

**3 use cases (reformulados):**

**Abogado o bufete.**
Cincuenta contratos en una mañana. Revisión masiva de contratos laborales, marcación de cláusulas de riesgo y propuesta de cambios con cita al CST y la jurisprudencia aplicable.

**Contador o firma contable.**
Cruzar declaraciones con el Estatuto Tributario actualizado. Análisis de obligaciones tributarias vigentes, alertas sobre cambios recientes en normativa DIAN y consistencia entre lo que tu firma declara y lo que el cliente declara.

**Notaría o administración.**
Minutas con citas a los estatutos, autenticaciones trazadas en segundos. Redacción de actas, contratos comerciales y documentos societarios con verificación contra los estatutos y el Código de Comercio.

### 10. PRICING — reformular

**Título:** `El precio se mide en capacidad, no en horas.`
**Sub:** `Cobramos por capacidad de negocio generada, no por horas trabajadas ni por cantidad de documentos. Una llamada de veinte minutos nos alcanza para armar un plan a la medida de tu firma.`
**Nota:** `Si no genera valor para tu equipo, no seguís. Sin cláusulas de permanencia abusivas.`

**Tratamiento visual:**
- Fondo claro, generous whitespace.
- Sin tabla de precios (es a medida).
- Una sola línea de garantía al final, en Inter 16px color `--ink-3`.

### 11. CTA FINAL — reformular

**Título:** `Veinte minutos para entender tu caso.`
**Sub:** `Te contactamos en menos de 24 horas hábiles. Sin compromiso, sin venta forzada. Si Worgena no aplica para tu firma, te lo decimos.`

**Canales:** WhatsApp, teléfono, email — sin cambios.

**Form:** campos sin cambios. Botón: `Reservar llamada de 20 min`.

**Qué pasa después:**

1. Te contactamos en menos de 24 horas hábiles para confirmar horario.
2. En la llamada entendemos tu práctica y vemos si Worgena aplica.
3. Si sí, configuramos un piloto de 30 días con tus primeros tres workflows.

**Tratamiento visual:**
- **Fondo `--night`**, texto claro. Cierre dramático.
- Display Fraunces grande.

### 12. FOOTER — mantener con pase de acentos

- Marca con Fraunces.
- Tagline: `El sistema operativo de tu firma. Automatiza el trabajo repetitivo. Hecho en Colombia.`
- Columnas sin cambios estructurales.
- Bottom bar con `© 2026 Worgena · Medellín, Colombia` y la línea opcional.

---

## Section rhythm (claro / oscuro)

| # | sección | fondo |
|---|---|---|
| 1 | nav | paper con frosted |
| 2 | hero | paper-pure |
| 3 | para quién es | paper |
| 4 | problema | **night** |
| 5 | solución | paper |
| 6 | el sistema | paper |
| 7 | features | paper |
| 8 | seguridad | paper-pure |
| 9 | casos | paper |
| 10 | pricing | paper |
| 11 | CTA final | **night** |
| 12 | footer | night |

Tres pausas oscuras: `problema` (drama), `CTA final` (cierre). El nav usa paper. Seguridad queda clara para no duplicar el efecto dramático tan cerca del final.

---

## Motion

- Mantener clases `reveal` y `reveal-stagger` actuales.
- Easing único: `--ease-out` (cubic-bezier(0.16, 1, 0.3, 1)).
- Sin animaciones decorativas, sin parallax, sin marquees.
- Hover de cards: `transform: translateY(-2px)` + `box-shadow` ligeramente mayor. Transición 200ms.
- Hover de botones: cambio de fondo + `translateY(-1px)`. Transición 150ms.
- **Reduced motion:** si `prefers-reduced-motion: reduce`, desactivo reveal y staggers.

## Accessibility

- `lang="es"` se mantiene.
- `aria-label` en nav y secciones, roles correctos.
- Contraste mínimo AA en todas las combinaciones (especialmente revisar texto sobre `--night`).
- Focus visible en todos los elementos interactivos: outline `--accent` 2px sólido, offset 2px.
- Texto del form con `label` asociado y `aria-describedby` cuando hay ayuda.
- Tabla de contenidos invisible para screen readers si la agrego después (no en este sprint).

## SEO

- Title, description, og:\* se mantienen.
- Schema.org Organization se mantiene.
- Canonical, robots, sitemap sin cambios.
- Performance de fuentes: preconnect a Google Fonts.

---

## File structure (post-refactor)

```
landing-worgena/
├── index.html                     # refactor
├── robots.txt                     # sin cambios
├── sitemap.xml                    # sin cambios
├── _headers                       # sin cambios
├── _redirects                     # sin cambios
├── public/
│   ├── favicon.svg                # sin cambios
│   └── og-image.svg               # sin cambios
├── assets/
│   ├── css/
│   │   ├── tokens.css             # refactor — nuevos tokens
│   │   └── main.css               # refactor — secciones, ritmo, motion
│   └── js/
│       ├── nav.js                 # sin cambios
│       └── scroll.js              # sin cambios
├── docs/
│   └── superpowers/
│       └── specs/
│           └── 2026-06-26-worgena-landing-redesign.md   # este doc
└── README.md                      # actualizar línea de fuentes
```

---

## Verification checklist (pre-entrega)

Per AGENTS.md revisión pre-entrega:

- [ ] `index.html` valida sin errores de sintaxis.
- [ ] Sin emojis en el archivo.
- [ ] Sin lorem ipsum ni placeholders sin resolver.
- [ ] Cada sección tiene contenido visible (ninguna vacía).
- [ ] Diseño se ve profesional en desktop y mobile (test en navegador).
- [ ] Animaciones `reveal` se ejecutan (no quedan elementos invisibles al cargar).
- [ ] Easings usan `--ease-out` en todos los casos.
- [ ] Contraste AA en todas las combinaciones (especialmente sobre `--night`).
- [ ] Sin acentos faltantes en español (pase completo).
- [ ] Hero, problema, solución (eyebrow + título + lead) coinciden verbatim con lo aprobado.
- [ ] Stats bar eliminada por completo (sin números MIT NANDA / Thomson Reuters).
- [ ] Sin datos inventados en casos, seguridad, features.
- [ ] Sin nombres de competidores en ninguna sección.
- [ ] Lighthouse ≥ 95 perf/a11y/best, 100 SEO (objetivo).
- [ ] `<em>` "tu firma" del hero renderiza en cursiva real de Fraunces, no en italic falso.

---

## Out of scope (explícito)

- No se reemplaza el form mailto por Cloudflare Worker.
- No se cambia el deploy target (sigue Cloudflare Pages).
- No se agrega analytics.
- No se migra el sitio a React, Vite o Framer builder.
- No se cambia el favicon ni og-image.
- No se hace dark mode toggle (la paleta usa secciones oscuras intencionalmente, no es un toggle de tema).
- No se agrega multi-página (/precios, /seguridad, /blog).
- No se hace pase de i18n (queda solo en español).