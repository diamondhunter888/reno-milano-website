# Reno Milano Website Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Bouw een volledige Astro-website voor Reno Milano, een luxueus renovatiebedrijf in Antwerpen, met 6 pagina's, donker kleurpalet en contactformulier via Netlify Forms.

**Architecture:** Astro statische site generator met component-gebaseerde structuur. Elke pagina is een `.astro` bestand dat herbruikbare componenten samenstelt via een gedeelde BaseLayout. CSS is globaal in `global.css` met CSS custom properties voor het kleurpalet.

**Tech Stack:** Astro 4.x, HTML5, CSS3 (geen framework), Google Fonts (Cormorant Garamond + Inter), Netlify Forms

---

## Bestandsstructuur

```
C:/Users/WORK/RENO MIlANO/website/
├── src/
│   ├── layouts/
│   │   └── BaseLayout.astro          # HTML shell, fonts, nav, footer
│   ├── components/
│   │   ├── Nav.astro                 # Navigatiebalk
│   │   ├── Footer.astro              # Voettekst
│   │   ├── HeroSection.astro         # Volledig scherm hero
│   │   ├── DienstenSection.astro     # 4 diensten icoonkaarten
│   │   ├── RealisatiesPreview.astro  # 3 foto preview + link
│   │   ├── StatsSection.astro        # 3 statistieken
│   │   ├── CtaBanner.astro           # Offerte CTA banner
│   │   ├── ContactForm.astro         # Netlify formulier + honeypot
│   │   └── MapsEmbed.astro           # Click-to-load Google Maps
│   ├── pages/
│   │   ├── index.astro               # Home
│   │   ├── diensten.astro            # Diensten
│   │   ├── realisaties.astro         # Portfolio galerij
│   │   ├── over-ons.astro            # Over ons
│   │   ├── contact.astro             # Contact
│   │   └── 404.astro                 # Foutpagina
│   └── styles/
│       └── global.css                # CSS custom properties + reset + utilities
├── public/
│   ├── favicon.svg                   # SVG RM favicon
│   └── images/                       # Placeholder afbeeldingen
├── astro.config.mjs
└── package.json
```

---

## Task 1: Astro project initialiseren

**Files:**
- Create: `package.json`
- Create: `astro.config.mjs`
- Create: `src/styles/global.css`
- Create: `public/favicon.svg`

- [ ] **Stap 1: Astro installeren**

```bash
cd "C:/Users/WORK/RENO MIlANO/website"
npm create astro@latest . -- --template minimal --no-install --no-git
npm install
```

- [ ] **Stap 2: astro.config.mjs controleren**

Inhoud moet zijn:
```js
import { defineConfig } from 'astro/config';

export default defineConfig({});
```

- [ ] **Stap 3: Global CSS aanmaken**

Maak `src/styles/global.css`:
```css
/* ── Custom Properties ── */
:root {
  --clr-black:    #0d0d0d;
  --clr-dark:     #2a2a2a;
  --clr-mid:      #888888;
  --clr-light:    #f5f5f5;
  --clr-white:    #ffffff;

  --font-display: 'Cormorant Garamond', serif;
  --font-body:    'Inter', sans-serif;

  --spacing-xs: 0.5rem;
  --spacing-sm: 1rem;
  --spacing-md: 2rem;
  --spacing-lg: 4rem;
  --spacing-xl: 8rem;
}

/* ── Reset ── */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
body {
  font-family: var(--font-body);
  font-size: 1rem;
  color: var(--clr-dark);
  background: var(--clr-white);
  line-height: 1.6;
}
img { display: block; max-width: 100%; }
a { color: inherit; text-decoration: none; }

/* ── Utilities ── */
.container { max-width: 1200px; margin-inline: auto; padding-inline: 1.5rem; }
.label {
  font-size: 0.6875rem;
  font-weight: 600;
  letter-spacing: 0.25em;
  text-transform: uppercase;
  color: var(--clr-mid);
}
.btn-outline {
  display: inline-block;
  border: 1px solid currentColor;
  padding: 0.6rem 1.5rem;
  font-size: 0.6875rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  transition: background 0.2s, color 0.2s;
  cursor: pointer;
}
.btn-outline:hover { background: var(--clr-white); color: var(--clr-black); }
.btn-solid {
  display: inline-block;
  background: var(--clr-white);
  color: var(--clr-black);
  padding: 0.6rem 1.5rem;
  font-size: 0.6875rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  border: 1px solid var(--clr-white);
  transition: background 0.2s, color 0.2s;
  cursor: pointer;
}
.btn-solid:hover { background: transparent; color: var(--clr-white); }

/* ── Section spacing ── */
.section { padding: var(--spacing-lg) 0; }
.section--dark { background: var(--clr-black); color: var(--clr-white); }
.section--light { background: var(--clr-light); }
```

- [ ] **Stap 4: SVG favicon aanmaken**

Maak `public/favicon.svg`:
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32">
  <rect width="32" height="32" fill="#0d0d0d"/>
  <text x="50%" y="55%" dominant-baseline="middle" text-anchor="middle"
    font-family="serif" font-size="14" font-weight="700" fill="#ffffff">RM</text>
</svg>
```

- [ ] **Stap 5: Dev server testen**

```bash
npm run dev
```
Verwacht: server draait op `http://localhost:4321`

- [ ] **Stap 6: Commit**

```bash
git init
git add .
git commit -m "chore: initialiseer Astro project met global CSS en favicon"
```

---

## Task 2: BaseLayout en navigatie

**Files:**
- Create: `src/layouts/BaseLayout.astro`
- Create: `src/components/Nav.astro`
- Create: `src/components/Footer.astro`

- [ ] **Stap 1: Nav.astro aanmaken**

```astro
---
const navLinks = [
  { href: '/', label: 'Home' },
  { href: '/diensten', label: 'Diensten' },
  { href: '/realisaties', label: 'Realisaties' },
  { href: '/over-ons', label: 'Over ons' },
];
const currentPath = Astro.url.pathname;
---

<nav class="nav">
  <div class="container nav__inner">
    <a href="/" class="nav__logo">RENO MILANO</a>

    <button class="nav__toggle" aria-label="Menu openen" aria-expanded="false">
      <span></span><span></span><span></span>
    </button>

    <ul class="nav__links" role="list">
      {navLinks.map(link => (
        <li>
          <a
            href={link.href}
            class={`nav__link ${currentPath === link.href ? 'nav__link--active' : ''}`}
          >
            {link.label}
          </a>
        </li>
      ))}
      <li>
        <a href="/contact" class="btn-outline nav__cta">Contact</a>
      </li>
    </ul>
  </div>
</nav>

<style>
.nav {
  position: fixed;
  top: 0; left: 0; right: 0;
  z-index: 100;
  background: var(--clr-black);
  border-bottom: 1px solid var(--clr-dark);
}
.nav__inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 64px;
}
.nav__logo {
  font-family: var(--font-display);
  font-size: 1rem;
  font-weight: 700;
  letter-spacing: 0.3em;
  color: var(--clr-white);
}
.nav__links {
  display: flex;
  align-items: center;
  gap: 2rem;
  list-style: none;
}
.nav__link {
  font-size: 0.6875rem;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--clr-mid);
  transition: color 0.2s;
}
.nav__link:hover, .nav__link--active { color: var(--clr-white); }
.nav__cta { color: var(--clr-mid); }
.nav__cta:hover { background: var(--clr-white); color: var(--clr-black); }
.nav__toggle {
  display: none;
  flex-direction: column;
  gap: 5px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 4px;
}
.nav__toggle span {
  display: block;
  width: 22px;
  height: 1px;
  background: var(--clr-white);
  transition: transform 0.2s;
}

/* Mobiel */
@media (max-width: 768px) {
  .nav__toggle { display: flex; }
  .nav__links {
    display: none;
    position: absolute;
    top: 64px; left: 0; right: 0;
    background: var(--clr-black);
    flex-direction: column;
    padding: 1.5rem;
    gap: 1.5rem;
    border-top: 1px solid var(--clr-dark);
  }
  .nav__links.is-open { display: flex; }
}
</style>

<script>
  const toggle = document.querySelector('.nav__toggle');
  const links = document.querySelector('.nav__links');
  toggle?.addEventListener('click', () => {
    const isOpen = links?.classList.toggle('is-open');
    toggle.setAttribute('aria-expanded', String(isOpen));
  });
</script>
```

- [ ] **Stap 2: Footer.astro aanmaken**

```astro
---
const year = new Date().getFullYear();
---
<footer class="footer">
  <div class="container footer__inner">
    <span class="nav__logo" style="font-family:var(--font-display);font-weight:700;letter-spacing:0.3em;color:var(--clr-white);">RENO MILANO</span>
    <p class="label" style="color:var(--clr-mid);">© {year} Reno Milano · Antwerpen, België</p>
    <ul class="footer__links" role="list">
      <li><a href="/diensten" class="footer__link">Diensten</a></li>
      <li><a href="/realisaties" class="footer__link">Realisaties</a></li>
      <li><a href="/contact" class="footer__link">Contact</a></li>
    </ul>
  </div>
</footer>

<style>
.footer {
  background: var(--clr-black);
  border-top: 1px solid var(--clr-dark);
  padding: 2rem 0;
}
.footer__inner {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
}
.footer__links {
  display: flex;
  gap: 1.5rem;
  list-style: none;
}
.footer__link {
  font-size: 0.6875rem;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--clr-mid);
  transition: color 0.2s;
}
.footer__link:hover { color: var(--clr-white); }
</style>
```

- [ ] **Stap 3: BaseLayout.astro aanmaken**

```astro
---
import Nav from '../components/Nav.astro';
import Footer from '../components/Footer.astro';
import '../styles/global.css';

interface Props {
  title: string;
  description?: string;
}
const { title, description = 'Reno Milano — Renovatiebedrijf in Antwerpen. Specialist in woningrenovatie, dakwerken, badkamer en keuken.' } = Astro.props;
---

<!doctype html>
<html lang="nl">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content={description} />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@300;400;700&family=Inter:wght@400;600&display=swap" rel="stylesheet" />
    <title>{title} · Reno Milano</title>
  </head>
  <body>
    <Nav />
    <main style="padding-top: 64px;">
      <slot />
    </main>
    <Footer />
  </body>
</html>
```

- [ ] **Stap 4: Tijdelijke index.astro aanmaken om nav te testen**

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
---
<BaseLayout title="Home">
  <div style="min-height:80vh;display:flex;align-items:center;justify-content:center;color:#888;">
    <p>Placeholder</p>
  </div>
</BaseLayout>
```

- [ ] **Stap 5: Visueel testen in browser**

`http://localhost:4321` — nav moet zichtbaar zijn, hamburger werkt op mobiel.

- [ ] **Stap 6: Commit**

```bash
git add .
git commit -m "feat: voeg BaseLayout, Nav en Footer toe"
```

---

## Task 3: Homepage — Hero en Diensten secties

**Files:**
- Create: `src/components/HeroSection.astro`
- Create: `src/components/DienstenSection.astro`
- Modify: `src/pages/index.astro`

- [ ] **Stap 1: HeroSection.astro aanmaken**

```astro
---
---
<section class="hero">
  <div class="hero__inner">
    <p class="label hero__label">Renovatie · Antwerpen · België</p>
    <h1 class="hero__title">
      <span class="hero__thin">RENO</span>
      <span class="hero__bold">MILANO</span>
    </h1>
    <div class="hero__divider"></div>
    <p class="hero__sub">Vakmanschap dat spreekt voor zich</p>
    <a href="/realisaties" class="btn-outline hero__cta">Bekijk onze realisaties</a>
  </div>
</section>

<style>
.hero {
  min-height: 100vh;
  background: var(--clr-black);
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: var(--spacing-lg) var(--spacing-sm);
}
.hero__inner { display: flex; flex-direction: column; align-items: center; gap: 1rem; }
.hero__label { color: var(--clr-mid); margin-bottom: 0.5rem; }
.hero__title {
  font-family: var(--font-display);
  display: flex;
  gap: 0.5em;
  letter-spacing: 0.2em;
  line-height: 1;
}
.hero__thin { font-size: clamp(2.5rem, 8vw, 5rem); font-weight: 300; color: var(--clr-white); }
.hero__bold { font-size: clamp(2.5rem, 8vw, 5rem); font-weight: 800; color: var(--clr-white); }
.hero__divider { width: 48px; height: 1px; background: var(--clr-mid); margin: 0.5rem 0; }
.hero__sub {
  font-size: 0.8125rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--clr-mid);
}
.hero__cta { margin-top: 1.5rem; color: var(--clr-mid); }
.hero__cta:hover { background: var(--clr-white); color: var(--clr-black); }
</style>
```

- [ ] **Stap 2: DienstenSection.astro aanmaken**

```astro
---
const diensten = [
  { icon: '🏠', titel: 'Woningrenovatie', omschrijving: 'Vloeren, muren en plafonds — volledig vernieuwd met oog voor detail.' },
  { icon: '🏚️', titel: 'Dakwerken', omschrijving: 'Isolatie, herstelling en vervanging van daken voor elk type woning.' },
  { icon: '🛁', titel: 'Badkamer', omschrijving: 'Totaalrenovatie van uw badkamer: van ontwerp tot volledige afwerking.' },
  { icon: '🍳', titel: 'Keuken', omschrijving: 'Keukens op maat, perfect geplaatst en afgewerkt tot in de puntjes.' },
];
---
<section class="section section--light diensten">
  <div class="container">
    <p class="label" style="text-align:center;margin-bottom:0.75rem;">Wat wij doen</p>
    <h2 class="diensten__title">Onze Diensten</h2>
    <div class="diensten__grid">
      {diensten.map(d => (
        <div class="diensten__card">
          <span class="diensten__icon">{d.icon}</span>
          <h3 class="diensten__naam">{d.titel}</h3>
          <p class="diensten__omschrijving">{d.omschrijving}</p>
        </div>
      ))}
    </div>
  </div>
</section>

<style>
.diensten__title {
  font-family: var(--font-display);
  font-size: clamp(1.75rem, 4vw, 2.5rem);
  font-weight: 700;
  letter-spacing: 0.05em;
  text-align: center;
  color: var(--clr-black);
  margin-bottom: var(--spacing-md);
}
.diensten__grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 1.5rem;
}
.diensten__card {
  background: var(--clr-white);
  border: 1px solid #e0e0e0;
  padding: 2rem 1.5rem;
  text-align: center;
  transition: box-shadow 0.2s;
}
.diensten__card:hover { box-shadow: 0 4px 24px rgba(0,0,0,0.08); }
.diensten__icon { font-size: 2rem; display: block; margin-bottom: 1rem; }
.diensten__naam {
  font-size: 0.75rem;
  font-weight: 700;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: var(--clr-black);
  margin-bottom: 0.75rem;
}
.diensten__omschrijving { font-size: 0.875rem; color: var(--clr-mid); line-height: 1.7; }
</style>
```

- [ ] **Stap 3: index.astro bijwerken met Hero en Diensten**

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
import HeroSection from '../components/HeroSection.astro';
import DienstenSection from '../components/DienstenSection.astro';
---
<BaseLayout title="Home">
  <HeroSection />
  <DienstenSection />
</BaseLayout>
```

- [ ] **Stap 4: Testen in browser** — hero en 4 dienstenkaarten zichtbaar op `http://localhost:4321`

- [ ] **Stap 5: Commit**

```bash
git add .
git commit -m "feat: voeg Hero en DienstenSection toe aan homepage"
```

---

## Task 4: Homepage — Realisaties preview, Stats en CTA

**Files:**
- Create: `src/components/RealisatiesPreview.astro`
- Create: `src/components/StatsSection.astro`
- Create: `src/components/CtaBanner.astro`
- Modify: `src/pages/index.astro`
- Create: `public/images/placeholder.svg`

- [ ] **Stap 1: Placeholder SVG aanmaken**

Maak `public/images/placeholder.svg`:
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 800 600">
  <rect width="800" height="600" fill="#2a2a2a"/>
  <text x="50%" y="50%" dominant-baseline="middle" text-anchor="middle"
    font-family="sans-serif" font-size="24" fill="#555555">Foto komt binnenkort</text>
</svg>
```

- [ ] **Stap 2: RealisatiesPreview.astro aanmaken**

```astro
---
const previews = [
  { alt: 'Woningrenovatie project 1' },
  { alt: 'Badkamerrenovatie project 2' },
  { alt: 'Keukenrenovatie project 3' },
];
---
<section class="section section--dark realisaties-preview">
  <div class="container">
    <p class="label" style="text-align:center;margin-bottom:0.75rem;color:var(--clr-mid);">Ons werk</p>
    <h2 class="rp__title">Realisaties</h2>
    <div class="rp__grid">
      {previews.map(p => (
        <div class="rp__item">
          <img src="/images/placeholder.svg" alt={p.alt} class="rp__img" />
        </div>
      ))}
    </div>
    <div style="text-align:center;margin-top:2rem;">
      <a href="/realisaties" class="btn-outline">Bekijk alle realisaties</a>
    </div>
  </div>
</section>

<style>
.rp__title {
  font-family: var(--font-display);
  font-size: clamp(1.75rem, 4vw, 2.5rem);
  font-weight: 700;
  letter-spacing: 0.05em;
  text-align: center;
  color: var(--clr-white);
  margin-bottom: var(--spacing-md);
}
.rp__grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 1rem;
}
.rp__img { width: 100%; height: 220px; object-fit: cover; display: block; }
</style>
```

- [ ] **Stap 3: StatsSection.astro aanmaken**

```astro
---
const stats = [
  { cijfer: '10+', label: 'Jaar ervaring' },
  { cijfer: '200+', label: 'Tevreden klanten' },
  { cijfer: '100%', label: 'Kwaliteitsgarantie' },
];
---
<section class="section section--light stats">
  <div class="container stats__grid">
    {stats.map(s => (
      <div class="stats__item">
        <span class="stats__cijfer">{s.cijfer}</span>
        <span class="label">{s.label}</span>
      </div>
    ))}
  </div>
</section>

<style>
.stats__grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  gap: 2rem;
  text-align: center;
}
.stats__item { display: flex; flex-direction: column; align-items: center; gap: 0.5rem; }
.stats__cijfer {
  font-family: var(--font-display);
  font-size: clamp(2rem, 5vw, 3.5rem);
  font-weight: 800;
  color: var(--clr-black);
  line-height: 1;
}
</style>
```

- [ ] **Stap 4: CtaBanner.astro aanmaken**

```astro
---
---
<section class="section section--dark cta">
  <div class="container" style="text-align:center;">
    <p class="label" style="margin-bottom:1rem;">Klaar voor uw renovatie?</p>
    <h2 class="cta__title">Vraag vandaag uw gratis offerte aan</h2>
    <a href="/contact" class="btn-outline cta__btn">Neem contact op</a>
  </div>
</section>

<style>
.cta__title {
  font-family: var(--font-display);
  font-size: clamp(1.5rem, 3vw, 2.25rem);
  font-weight: 300;
  letter-spacing: 0.08em;
  color: var(--clr-white);
  margin-bottom: 2rem;
}
.cta__btn { color: var(--clr-mid); }
.cta__btn:hover { background: var(--clr-white); color: var(--clr-black); }
</style>
```

- [ ] **Stap 5: index.astro finaliseren**

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
import HeroSection from '../components/HeroSection.astro';
import DienstenSection from '../components/DienstenSection.astro';
import RealisatiesPreview from '../components/RealisatiesPreview.astro';
import StatsSection from '../components/StatsSection.astro';
import CtaBanner from '../components/CtaBanner.astro';
---
<BaseLayout title="Home">
  <HeroSection />
  <DienstenSection />
  <RealisatiesPreview />
  <StatsSection />
  <CtaBanner />
</BaseLayout>
```

- [ ] **Stap 6: Testen** — volledige homepage zichtbaar, alle 5 secties aanwezig

- [ ] **Stap 7: Commit**

```bash
git add .
git commit -m "feat: volledige homepage met alle secties"
```

---

## Task 5: Dienstenpagina

**Files:**
- Create: `src/pages/diensten.astro`

- [ ] **Stap 1: diensten.astro aanmaken**

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';

const diensten = [
  {
    titel: 'Woningrenovatie',
    omschrijving: 'Van vloeren tot plafonds: wij renoveren uw woning van A tot Z. Met oog voor kwaliteit en detail zorgen wij voor een perfect resultaat dat jaren meegaat.',
    details: ['Vloerwerken (parket, tegels, vinyl)', 'Schilderwerk en behang', 'Plafondafwerking', 'Elektriciteit en sanitair (coördinatie)'],
  },
  {
    titel: 'Dakwerken',
    omschrijving: 'Een solide dak is de basis van elk huis. Wij verzorgen isolatie, herstelling en volledige vervanging van daken voor elk type woning in Antwerpen en omgeving.',
    details: ['Dakinspectie en diagnose', 'Isolatie en ventilatie', 'Herstelling van lekken', 'Volledige dakrenovatie'],
  },
  {
    titel: 'Badkamerrenovatie',
    omschrijving: 'Uw badkamer verdient een frisse start. Van ontwerp tot volledige realisatie: wij maken van uw badkamer een luxueuze en functionele ruimte.',
    details: ['Volledige sloop en opbouw', 'Tegels en vloerwerken', 'Sanitair en kranen', 'Douche, bad en meubels'],
  },
  {
    titel: 'Keukenrenovatie',
    omschrijving: 'Een nieuwe keuken op maat, perfect aangepast aan uw stijl en ruimte. Wij begeleiden u van het eerste ontwerp tot de laatste schroef.',
    details: ['Keukenontwerp op maat', 'Plaatsing en afwerking', 'Aanrechtbladen en achterwanden', 'Verlichting en elektrische aansluitingen'],
  },
];
---
<BaseLayout title="Diensten" description="Ontdek alle renovatiediensten van Reno Milano: woningrenovatie, dakwerken, badkamer en keuken in Antwerpen.">
  <section class="section section--dark" style="min-height:40vh;display:flex;align-items:center;">
    <div class="container" style="text-align:center;">
      <p class="label" style="margin-bottom:0.75rem;">Wat wij doen</p>
      <h1 style="font-family:var(--font-display);font-size:clamp(2rem,5vw,3.5rem);font-weight:300;letter-spacing:0.15em;color:var(--clr-white);">ONZE DIENSTEN</h1>
    </div>
  </section>

  {diensten.map((d, i) => (
    <section class={`section ${i % 2 === 0 ? 'section--light' : ''}`}>
      <div class="container dienst">
        <div class="dienst__tekst">
          <p class="label" style="margin-bottom:0.5rem;">{`0${i + 1}`}</p>
          <h2 class="dienst__titel">{d.titel}</h2>
          <p class="dienst__omschrijving">{d.omschrijving}</p>
          <ul class="dienst__lijst">
            {d.details.map(item => <li>{item}</li>)}
          </ul>
        </div>
        <div class="dienst__foto">
          <img src="/images/placeholder.svg" alt={d.titel} />
        </div>
      </div>
    </section>
  ))}

  <section class="section section--dark" style="text-align:center;">
    <div class="container">
      <p class="label" style="margin-bottom:1rem;">Interesse?</p>
      <h2 style="font-family:var(--font-display);font-size:clamp(1.5rem,3vw,2.25rem);font-weight:300;letter-spacing:0.08em;color:var(--clr-white);margin-bottom:2rem;">Vraag een vrijblijvende offerte aan</h2>
      <a href="/contact" class="btn-outline" style="color:var(--clr-mid);">Contact opnemen</a>
    </div>
  </section>
</BaseLayout>

<style>
.dienst {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem;
  align-items: center;
}
.dienst__titel {
  font-family: var(--font-display);
  font-size: clamp(1.5rem, 3vw, 2rem);
  font-weight: 700;
  letter-spacing: 0.05em;
  color: var(--clr-black);
  margin-bottom: 1rem;
}
.dienst__omschrijving { color: #555; line-height: 1.8; margin-bottom: 1.5rem; }
.dienst__lijst { list-style: none; display: flex; flex-direction: column; gap: 0.5rem; }
.dienst__lijst li {
  font-size: 0.875rem;
  color: var(--clr-mid);
  padding-left: 1rem;
  border-left: 1px solid var(--clr-mid);
}
.dienst__foto img { width: 100%; height: 320px; object-fit: cover; }
@media (max-width: 768px) {
  .dienst { grid-template-columns: 1fr; gap: 2rem; }
}
</style>
```

- [ ] **Stap 2: Testen** — `http://localhost:4321/diensten` — 4 diensten zichtbaar

- [ ] **Stap 3: Commit**

```bash
git add .
git commit -m "feat: voeg dienstenpagina toe met 4 diensten"
```

---

## Task 6: Realisatiespagina (portfolio)

**Files:**
- Create: `src/pages/realisaties.astro`

- [ ] **Stap 1: realisaties.astro aanmaken**

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';

const projecten = [
  { naam: 'Totaalrenovatie — Borgerhout', categorie: 'Woningrenovatie', omschrijving: 'Volledige renovatie van een rijwoning inclusief vloeren, schilderwerk en badkamer.' },
  { naam: 'Dakrenovatie — Merksem', categorie: 'Dakwerken', omschrijving: 'Volledige vervanging van het dak met nieuwe isolatie en ventilatie.' },
  { naam: 'Luxe badkamer — Berchem', categorie: 'Badkamer', omschrijving: 'Moderne inloopdouche, vrijstaand bad en maatwerk meubels.' },
  { naam: 'Open keuken — Wilrijk', categorie: 'Keuken', omschrijving: 'Keuken op maat met eiland, inductie en natuurstenen werkblad.' },
  { naam: 'Appartementrenovatie — Antwerpen centrum', categorie: 'Woningrenovatie', omschrijving: 'Renovatie van een appartement inclusief nieuwe vloeren en plafonds.' },
  { naam: 'Badkamer & keuken — Deurne', categorie: 'Badkamer & Keuken', omschrijving: 'Gecombineerde renovatie van badkamer en keuken in één project.' },
];
---
<BaseLayout title="Realisaties" description="Bekijk de realisaties van Reno Milano: renovatieprojecten in Antwerpen en omgeving.">
  <section class="section section--dark" style="min-height:40vh;display:flex;align-items:center;">
    <div class="container" style="text-align:center;">
      <p class="label" style="margin-bottom:0.75rem;">Ons werk</p>
      <h1 style="font-family:var(--font-display);font-size:clamp(2rem,5vw,3.5rem);font-weight:300;letter-spacing:0.15em;color:var(--clr-white);">REALISATIES</h1>
    </div>
  </section>

  <section class="section section--light">
    <div class="container">
      <div class="portfolio__grid">
        {projecten.map(p => (
          <article class="portfolio__kaart">
            <img src="/images/placeholder.svg" alt={p.naam} class="portfolio__foto" />
            <div class="portfolio__info">
              <p class="label" style="margin-bottom:0.25rem;">{p.categorie}</p>
              <h3 class="portfolio__naam">{p.naam}</h3>
              <p class="portfolio__omschrijving">{p.omschrijving}</p>
            </div>
          </article>
        ))}
      </div>
    </div>
  </section>

  <section class="section section--dark" style="text-align:center;">
    <div class="container">
      <p style="font-family:var(--font-display);font-size:clamp(1.5rem,3vw,2rem);font-weight:300;color:var(--clr-white);letter-spacing:0.08em;margin-bottom:2rem;">Wil u uw woning ook laten renoveren?</p>
      <a href="/contact" class="btn-outline" style="color:var(--clr-mid);">Offerte aanvragen</a>
    </div>
  </section>
</BaseLayout>

<style>
.portfolio__grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1.5rem;
}
.portfolio__kaart { background: var(--clr-white); border: 1px solid #e0e0e0; overflow: hidden; }
.portfolio__foto { width: 100%; height: 220px; object-fit: cover; }
.portfolio__info { padding: 1.25rem; }
.portfolio__naam {
  font-family: var(--font-display);
  font-size: 1.1rem;
  font-weight: 700;
  color: var(--clr-black);
  margin: 0.25rem 0 0.5rem;
}
.portfolio__omschrijving { font-size: 0.875rem; color: var(--clr-mid); line-height: 1.6; }
@media (max-width: 1024px) { .portfolio__grid { grid-template-columns: repeat(2, 1fr); } }
@media (max-width: 640px) { .portfolio__grid { grid-template-columns: 1fr; } }
</style>
```

- [ ] **Stap 2: Testen** — `http://localhost:4321/realisaties` — 6 projectkaarten zichtbaar

- [ ] **Stap 3: Commit**

```bash
git add .
git commit -m "feat: voeg realisatiespagina toe met 6 projectkaarten"
```

---

## Task 7: Over ons pagina

**Files:**
- Create: `src/pages/over-ons.astro`

- [ ] **Stap 1: over-ons.astro aanmaken**

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';

const waarden = [
  { titel: 'Kwaliteit', tekst: 'Wij werken uitsluitend met kwalitatieve materialen en bewezen technieken. Elk project wordt met dezelfde zorg afgewerkt, groot of klein.' },
  { titel: 'Betrouwbaarheid', tekst: 'Duidelijke communicatie, eerlijke prijzen en afspraken nakomen. Dat is de basis van elke samenwerking bij Reno Milano.' },
  { titel: 'Vakmanschap', tekst: 'Onze ervaren vaklieden brengen jarenlange kennis mee. Van dakwerken tot badkamerafwerking: het detail maakt het verschil.' },
];
---
<BaseLayout title="Over ons" description="Leer meer over Reno Milano, uw renovatiepartner in Antwerpen.">
  <section class="section section--dark" style="min-height:40vh;display:flex;align-items:center;">
    <div class="container" style="text-align:center;">
      <p class="label" style="margin-bottom:0.75rem;">Wie zijn wij</p>
      <h1 style="font-family:var(--font-display);font-size:clamp(2rem,5vw,3.5rem);font-weight:300;letter-spacing:0.15em;color:var(--clr-white);">OVER ONS</h1>
    </div>
  </section>

  <section class="section section--light">
    <div class="container verhaal">
      <div>
        <p class="label" style="margin-bottom:0.75rem;">Ons verhaal</p>
        <h2 style="font-family:var(--font-display);font-size:clamp(1.5rem,3vw,2.25rem);font-weight:700;color:var(--clr-black);margin-bottom:1.5rem;">Renovatie met passie en precisie</h2>
        <p style="color:#555;line-height:1.9;margin-bottom:1rem;">
          Reno Milano is een renovatiebedrijf gevestigd in het hart van Antwerpen. Met meer dan tien jaar ervaring in woningrenovatie, dakwerken, badkamers en keukens hebben wij honderden Antwerpse gezinnen een mooier, comfortabeler thuis gegeven.
        </p>
        <p style="color:#555;line-height:1.9;">
          Wij geloven dat een geslaagde renovatie begint bij goed luisteren. Uw wensen, uw stijl, uw budget — dat is ons vertrekpunt. Van het eerste gesprek tot de laatste afwerking staan wij voor u klaar met eerlijk advies en vakkundige uitvoering.
        </p>
      </div>
      <div>
        <img src="/images/placeholder.svg" alt="Reno Milano team" style="width:100%;height:360px;object-fit:cover;" />
      </div>
    </div>
  </section>

  <section class="section section--dark">
    <div class="container">
      <p class="label" style="text-align:center;margin-bottom:0.75rem;">Waar wij voor staan</p>
      <h2 style="font-family:var(--font-display);font-size:clamp(1.5rem,3vw,2.25rem);font-weight:700;letter-spacing:0.05em;color:var(--clr-white);text-align:center;margin-bottom:var(--spacing-md);">Onze Waarden</h2>
      <div class="waarden__grid">
        {waarden.map(w => (
          <div class="waarde">
            <div class="waarde__divider"></div>
            <h3 class="waarde__titel">{w.titel}</h3>
            <p class="waarde__tekst">{w.tekst}</p>
          </div>
        ))}
      </div>
    </div>
  </section>

  <section class="section section--light" style="text-align:center;">
    <div class="container">
      <p class="label" style="margin-bottom:0.75rem;">Erkend aannemer</p>
      <p style="color:#555;max-width:600px;margin:0 auto 2rem;line-height:1.8;">
        Reno Milano is een geregistreerde aannemer en voldoet aan alle wettelijke vereisten voor renovatiewerken in België. Wij werken conform de Belgische bouwregelgeving en bieden volledige garantie op al onze werken.
      </p>
      <a href="/contact" class="btn-outline" style="border-color:var(--clr-black);color:var(--clr-black);">Neem contact op</a>
    </div>
  </section>
</BaseLayout>

<style>
.verhaal {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem;
  align-items: center;
}
.waarden__grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  gap: 2.5rem;
}
.waarde__divider { width: 32px; height: 1px; background: var(--clr-mid); margin-bottom: 1rem; }
.waarde__titel {
  font-size: 0.75rem;
  font-weight: 700;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--clr-white);
  margin-bottom: 0.75rem;
}
.waarde__tekst { font-size: 0.875rem; color: var(--clr-mid); line-height: 1.8; }
@media (max-width: 768px) { .verhaal { grid-template-columns: 1fr; gap: 2rem; } }
</style>
```

- [ ] **Stap 2: Testen** — `http://localhost:4321/over-ons`

- [ ] **Stap 3: Commit**

```bash
git add .
git commit -m "feat: voeg over ons pagina toe"
```

---

## Task 8: Contactpagina

**Files:**
- Create: `src/components/ContactForm.astro`
- Create: `src/components/MapsEmbed.astro`
- Create: `src/pages/contact.astro`

- [ ] **Stap 1: ContactForm.astro aanmaken (Netlify Forms + honeypot)**

```astro
---
---
<form
  name="contact"
  method="POST"
  data-netlify="true"
  netlify-honeypot="bot-field"
  class="contact-form"
>
  <input type="hidden" name="form-name" value="contact" />
  <!-- Honeypot: verborgen voor mensen, zichtbaar voor bots -->
  <p style="display:none;">
    <label>Niet invullen: <input name="bot-field" /></label>
  </p>

  <div class="contact-form__group">
    <label class="label contact-form__label" for="naam">Naam</label>
    <input type="text" id="naam" name="naam" required class="contact-form__input" placeholder="Uw volledige naam" />
  </div>

  <div class="contact-form__group">
    <label class="label contact-form__label" for="email">E-mailadres</label>
    <input type="email" id="email" name="email" required class="contact-form__input" placeholder="uw@email.be" />
  </div>

  <div class="contact-form__group">
    <label class="label contact-form__label" for="bericht">Bericht</label>
    <textarea id="bericht" name="bericht" required rows="5" class="contact-form__input contact-form__textarea" placeholder="Beschrijf uw renovatieproject..."></textarea>
  </div>

  <button type="submit" class="btn-solid contact-form__btn">Verstuur bericht</button>
</form>

<style>
.contact-form { display: flex; flex-direction: column; gap: 1.5rem; }
.contact-form__group { display: flex; flex-direction: column; gap: 0.5rem; }
.contact-form__label { color: var(--clr-mid); }
.contact-form__input {
  background: var(--clr-light);
  border: 1px solid #ddd;
  padding: 0.75rem 1rem;
  font-family: var(--font-body);
  font-size: 0.9375rem;
  color: var(--clr-black);
  outline: none;
  transition: border-color 0.2s;
  width: 100%;
}
.contact-form__input:focus { border-color: var(--clr-mid); }
.contact-form__textarea { resize: vertical; min-height: 120px; }
.contact-form__btn {
  background: var(--clr-black);
  color: var(--clr-white);
  border-color: var(--clr-black);
  align-self: flex-start;
  padding: 0.75rem 2rem;
}
.contact-form__btn:hover { background: transparent; color: var(--clr-black); }
</style>
```

- [ ] **Stap 2: MapsEmbed.astro aanmaken (GDPR click-to-load)**

```astro
---
---
<div class="maps-wrap" id="maps-wrap">
  <div class="maps-overlay" id="maps-overlay">
    <p class="label" style="margin-bottom:0.75rem;color:var(--clr-mid);">Google Maps</p>
    <p style="font-size:0.875rem;color:#888;margin-bottom:1.25rem;max-width:280px;text-align:center;">
      Door op de knop te klikken laadt u Google Maps. Google kan dan gegevens verzamelen.
    </p>
    <button class="btn-outline" id="maps-load-btn" style="color:var(--clr-mid);">Kaart laden</button>
  </div>
  <iframe
    id="maps-iframe"
    src=""
    data-src="https://maps.google.com/maps?q=Antwerpen&output=embed"
    width="100%"
    height="100%"
    style="border:0;display:none;"
    allowfullscreen
    loading="lazy"
    title="Reno Milano locatie op Google Maps"
  ></iframe>
</div>

<style>
.maps-wrap {
  position: relative;
  width: 100%;
  height: 400px;
  background: var(--clr-light);
  border: 1px solid #ddd;
  overflow: hidden;
}
.maps-overlay {
  position: absolute;
  inset: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: var(--clr-light);
  z-index: 2;
}
</style>

<script>
  document.getElementById('maps-load-btn')?.addEventListener('click', () => {
    const iframe = document.getElementById('maps-iframe') as HTMLIFrameElement;
    const overlay = document.getElementById('maps-overlay');
    if (iframe && overlay) {
      iframe.src = iframe.dataset.src ?? '';
      iframe.style.display = 'block';
      overlay.style.display = 'none';
    }
  });
</script>
```

- [ ] **Stap 3: contact.astro aanmaken**

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
import ContactForm from '../components/ContactForm.astro';
import MapsEmbed from '../components/MapsEmbed.astro';
---
<BaseLayout title="Contact" description="Neem contact op met Reno Milano voor een vrijblijvende offerte voor uw renovatieproject in Antwerpen.">
  <section class="section section--dark" style="min-height:40vh;display:flex;align-items:center;">
    <div class="container" style="text-align:center;">
      <p class="label" style="margin-bottom:0.75rem;">Vrijblijvend contact</p>
      <h1 style="font-family:var(--font-display);font-size:clamp(2rem,5vw,3.5rem);font-weight:300;letter-spacing:0.15em;color:var(--clr-white);">CONTACT</h1>
    </div>
  </section>

  <section class="section section--light">
    <div class="container contact-layout">
      <div class="contact-links">
        <div>
          <p class="label" style="margin-bottom:1.5rem;">Stuur een bericht</p>
          <ContactForm />
        </div>
      </div>
      <div class="contact-info">
        <div class="contact-gegevens">
          <p class="label" style="margin-bottom:1.5rem;">Contactgegevens</p>
          <ul class="gegevens-lijst">
            <li>
              <span class="label">Adres</span>
              <p>Antwerpen, België</p>
            </li>
            <li>
              <span class="label">Telefoon</span>
              <p><a href="tel:+32000000000">+32 (0)0 000 00 00</a></p>
            </li>
            <li>
              <span class="label">E-mail</span>
              <p><a href="mailto:info@renomilano.be">info@renomilano.be</a></p>
            </li>
            <li>
              <span class="label">Openingsuren</span>
              <p>Ma–Vr: 8:00–18:00</p>
            </li>
          </ul>
        </div>
        <MapsEmbed />
      </div>
    </div>
  </section>
</BaseLayout>

<style>
.contact-layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem;
  align-items: start;
}
.gegevens-lijst {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
  margin-bottom: 2rem;
}
.gegevens-lijst li { display: flex; flex-direction: column; gap: 0.25rem; }
.gegevens-lijst p { color: #555; font-size: 0.9375rem; }
.gegevens-lijst a:hover { text-decoration: underline; }
@media (max-width: 768px) {
  .contact-layout { grid-template-columns: 1fr; gap: 2rem; }
}
</style>
```

- [ ] **Stap 4: Testen**
  - `http://localhost:4321/contact` — formulier en kaart-overlay zichtbaar
  - Klik op "Kaart laden" — Google Maps laadt in
  - Formulier indienen toont Netlify-bevestiging na deployment (lokaal geen backend)

- [ ] **Stap 5: Commit**

```bash
git add .
git commit -m "feat: voeg contactpagina toe met Netlify Forms en GDPR-conforme kaart"
```

---

## Task 9: 404 pagina

**Files:**
- Create: `src/pages/404.astro`

- [ ] **Stap 1: 404.astro aanmaken**

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
---
<BaseLayout title="Pagina niet gevonden">
  <section class="section section--dark" style="min-height:80vh;display:flex;align-items:center;justify-content:center;text-align:center;">
    <div>
      <p class="label" style="margin-bottom:1rem;">Fout 404</p>
      <h1 style="font-family:var(--font-display);font-size:clamp(3rem,8vw,6rem);font-weight:800;color:var(--clr-white);letter-spacing:0.1em;margin-bottom:1rem;">OEPS</h1>
      <p style="color:var(--clr-mid);margin-bottom:2.5rem;font-size:1rem;">Deze pagina bestaat niet (meer).</p>
      <a href="/" class="btn-outline" style="color:var(--clr-mid);">Terug naar home</a>
    </div>
  </section>
</BaseLayout>
```

- [ ] **Stap 2: Testen** — `http://localhost:4321/bestaat-niet` — 404 pagina zichtbaar

- [ ] **Stap 3: Commit**

```bash
git add .
git commit -m "feat: voeg 404 pagina toe"
```

---

## Task 10: Responsiviteit en finale controle

**Files:**
- Modify: `src/styles/global.css` (eventueel extra media queries)

- [ ] **Stap 1: Mobiel testen** — open browser DevTools, test 375px (iPhone), 768px (tablet), 1280px (desktop)

  Controleer per pagina:
  - [ ] Nav hamburger werkt op mobiel
  - [ ] Dienstenkaarten stapelen verticaal
  - [ ] Contactformulier past volledig op scherm
  - [ ] Portfolio grid schakelt naar 1 kolom

- [ ] **Stap 2: Netlify config aanmaken**

Maak `netlify.toml` in de **root** van het project (niet in `public/`):
```toml
[build]
  command = "npm run build"
  publish = "dist"
```

> Geen redirect-regels nodig: Astro genereert statische HTML per pagina, dus Netlify vindt elke pagina automatisch.

- [ ] **Stap 3: Productie build testen**

```bash
npm run build
npm run preview
```
Verwacht: site draait op `http://localhost:4321`, alle pagina's bereikbaar

- [ ] **Stap 4: Finale commit**

```bash
git add .
git commit -m "chore: netlify config en finale responsiviteitscontrole"
```

---

## Deployment naar Netlify

Na de finale commit:

1. Maak een account op [netlify.com](https://netlify.com) (gratis)
2. Klik **"Add new site" → "Deploy manually"**
3. Sleep de `dist/` map naar Netlify
4. Klaar — site is live op een `*.netlify.app` URL

Of via Git (aanbevolen voor updates):
1. Push repo naar GitHub
2. Netlify koppelen aan GitHub repo
3. Build command: `npm run build`, publish dir: `dist`
4. Elke push naar main deployt automatisch
