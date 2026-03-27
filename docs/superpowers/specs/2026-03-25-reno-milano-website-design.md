# Reno Milano Website — Design Spec

**Datum:** 2026-03-25
**Project:** Reno Milano — Renovatiebedrijf Antwerpen
**Tech stack:** Astro (statische site, geen server vereist)

---

## 1. Overzicht

Een luxueuze, professionele brochure-website voor Reno Milano, een renovatiebedrijf gevestigd in Antwerpen, België. De site is in het Nederlands, met meertaligheid (FR, EN) als toekomstige uitbreiding buiten de scope van v1.

---

## 2. Diensten

- Volledige woningrenovatie (vloeren, muren, plafonds)
- Dakwerken (isolatie & herstelling)
- Badkamerrenovatie (totaalrenovatie)
- Keukenrenovatie (maatwerk & plaatsing)

---

## 3. Pagina's

| Pagina       | Doel                                              |
|--------------|---------------------------------------------------|
| Home         | Eerste indruk, hero, diensten, realisaties, CTA   |
| Diensten     | Overzicht van alle 4 diensten met beschrijving    |
| Realisaties  | Portfolio van uitgevoerde werken (fotogalerij)    |
| Over ons     | Verhaal, waarden, certificeringen                 |
| Contact      | Formulier + gegevens + Google Maps                |
| 404          | Foutpagina met link terug naar home               |

---

## 4. Lay-out & Visuele stijl

### Stijl
- **Luxueus, minimalistisch, donker**
- Geïnspireerd op high-end interieur- en architectuurbureaus
- Geen onnodige ornamenten — witruimte en typografie dragen het design

### Kleurpalet

| Naam          | Hex       | Gebruik                          |
|---------------|-----------|----------------------------------|
| Diepzwart     | `#0d0d0d` | Achtergrond hero, navbar, CTA    |
| Donkergrijs   | `#2a2a2a` | Secties, kaarten donker          |
| Middengrijs   | `#888888` | Subtitels, accenten, lijnen      |
| Lichtgrijs    | `#f5f5f5` | Lichte secties achtergrond       |
| Wit           | `#ffffff` | Tekst op donker, kaarten licht   |

### Typografie
- **Font:** Cormorant Garamond (koppen) + Inter (bodytekst) — beide via Google Fonts
- **Koppen (H1):** 48–72px, font-weight 300 of 800, letter-spacing: 0.1em–0.3em, uppercase
- **Koppen (H2):** 28–36px, font-weight 700, letter-spacing: 0.05em
- **Bodytekst:** 15–16px, font-weight 400, Inter, kleur `#888` op donker / `#444` op licht
- **Labels:** 10–11px, font-weight 600, uppercase, letter-spacing: 0.25em

### Navigatie
- Horizontaal, minimalistisch
- Logo links: `RENO MILANO` in kapitalen (letter-spacing: 0.3em)
- Links rechts: Home · Diensten · Realisaties · Over ons · `[CONTACT]` (outline-knop)
- Mobiel: hamburger-menu

### Favicon
- SVG text-based favicon: `RM` in wit op zwarte achtergrond

---

## 5. Homepage secties

1. **Hero** — Volledig scherm donkere achtergrond, gecentreerde tekst (`RENO MILANO` + slogan), CTA-knop `BEKIJK ONZE REALISATIES`
2. **Diensten** — 4 icoonkaarten (renovatie, dak, bad, keuken) op lichte achtergrond
3. **Realisaties preview** — 3 foto-placeholders op donkere achtergrond, link naar volledige pagina
4. **Statistieken** — 3 cijfers: `10+ jaar ervaring` / `200+ tevreden klanten` / `100% kwaliteitsgarantie`
5. **CTA-banner** — Donkere achtergrond, tekst "Vraag vandaag uw gratis offerte aan", knop naar contact

---

## 6. Diensten pagina

Per dienst een sectie met:
- Titel + korte beschrijving (2–3 zinnen)
- Placeholder-foto
- Diensten: Woningrenovatie, Dakwerken, Badkamer, Keuken

---

## 7. Realisaties pagina

- **Layout:** Responsive grid, 3 kolommen (desktop), 2 (tablet), 1 (mobiel)
- **Minimaal 6 projectkaarten** als placeholder
- Per kaart: placeholder-foto, projectnaam, korte omschrijving (1 zin)
- Geen filtering in v1

---

## 8. Over ons pagina

- **Bedrijfsverhaal:** 1 alinea over Reno Milano, missie en vakmanschap (placeholder tekst)
- **Waarden:** 3 punten (bijv. Kwaliteit, Betrouwbaarheid, Vakmanschap)
- **Certificeringen:** Sectie voor erkend aannemer / labels (placeholder)
- Geen teamfoto's in v1

---

## 9. Contactpagina

- **Contactformulier:** naam, e-mail, bericht, honeypot (spam), verzendknop
- **Backend:** Netlify Forms (HTML `data-netlify="true"` attribuut, geen API key vereist)
- **Contactgegevens:** adres (Antwerpen), telefoonnummer, e-mailadres
- **Google Maps:** Click-to-load overlay (GDPR-conform) — gebruiker klikt op knop om kaart te laden

---

## 10. GDPR

- Google Maps wordt pas geladen na expliciete klik van de gebruiker (click-to-load)
- Geen tracking cookies of analytics in v1
- Geen cookiebanner nodig in v1 (geen cookies bij normale gebruik)

---

## 11. Assets

- Alle afbeeldingen zijn voorlopig **placeholders** (grijze blokken)
- Geen logo aanwezig — tijdelijke tekstversie `RENO MILANO`
- Foto's worden later aangeleverd door klant

---

## 12. Technische vereisten

### Framework
- **Astro** — statische site generator
- Output: pure HTML/CSS/JS, geen server vereist
- Hosting: Netlify (aanbevolen), Vercel, GitHub Pages of FTP-upload

### Projectstructuur
```
src/
  pages/
    index.astro           # Home
    diensten.astro        # Diensten
    realisaties.astro     # Portfolio
    over-ons.astro        # Over ons
    contact.astro         # Contact
    404.astro             # Foutpagina
  components/
    Nav.astro
    Footer.astro
    HeroSection.astro
    DienstenSection.astro
    RealisatiesPreview.astro
    StatsSection.astro
    CtaBanner.astro
    ContactForm.astro
    MapsEmbed.astro       # Click-to-load Google Maps
  layouts/
    BaseLayout.astro
  styles/
    global.css
public/
  favicon.svg
  images/                 # Placeholder afbeeldingen
```

### Responsiviteit
- Mobile-first CSS
- Breakpoints: 640px (sm), 768px (md), 1024px (lg), 1280px (xl)

---

## 13. Niet in scope (v1)

- Blog / nieuws
- CMS / content management systeem
- Animaties / parallax
- Meertaligheid (toekomstige uitbreiding)
- Klantportaal / login
- Analytics

---

## 14. Succescriteria

- [ ] Alle 6 pagina's volledig gebouwd en navigeerbaar (incl. 404)
- [ ] Responsief (mobiel, tablet, desktop)
- [ ] Contactformulier functioneel via Netlify Forms + honeypot spam-bescherming
- [ ] Google Maps click-to-load (GDPR-conform)
- [ ] Kleurpalet en typografie consistent toegepast
- [ ] SVG favicon aanwezig
- [ ] Klaar voor deployment op Netlify
