# Handoff: Nimma.codes redesign (A2 Blauwdruk)

## Doel

Een nieuwe one-pager voor Nimma.codes die bezoekers vooral doorstuurt naar de Meetup-groep. Daar staan de data, aanmeldingen en foto's. De site zelf laat zien wat Nimma.codes is en hoe een avond eruitziet.

Elke primaire knop gaat naar https://www.meetup.com/nimma-codes-meetup-group/. Discord, GitHub en "talk indienen" zijn secundair (tekstlinks en footer).

## Bronnen

- **Statische export:** `index.html` en `code-of-conduct.html` in dit pakket. Dit is de referentie voor markup en gedrag.
- **Ontwerpcanvas:** https://claude.ai/code/artifact/5599d030-6c95-45db-b835-f33d19ab7feb, pagina "A2 · Responsive" met desktop (1440), tablet (834), mobiel (390), stijlgids en Code of Conduct.
- **Huidige site:** Hugo met het eigen theme `nimma-codes` in https://github.com/nimma-codes/preview.nimma.codes.
- **Meetup-foto's:** 86 foto's uit de 15 meest recente albums, per album in een map, op Brams Mac in `~/Downloads/nimma-meetup-fotos/`. Open `overzicht.html` in die map om te kiezen.

## Pagina-opbouw

De markup gebruikt classes met de prefix `n-` (site) en `c-` (Code of Conduct). Hieronder de secties in volgorde, met een voorstel voor de Hugo-partial.

| Sectie | Root-class | Partial |
|---|---|---|
| Header met logo, menu en Meetup-knop | `.n-header` | `header.html` |
| Hero: kop, intro, knoppen, skyline, draaiende stempel | `.n-hero` | `hero.html` |
| Volgende meetup: titel, wanneer, waar, aanmelden | `.n-next` | `next-meetup.html` |
| Foto's: 4 vakken met link naar Meetup | `.n-photos` | `photos.html` |
| Waarom deze meetup bestaat: beeldmerk-constructie en tekst | `.n-over` | `about.html` |
| Wat we doen: drie kaarten met illustratie | `.n-cards` | `activities.html` |
| Slotblok: "Wil je meer weten? Check onze Meetup." | `.n-cta` | `cta.html` |
| Footer: merk, Community, Meedoen, Over, onderregel | `.n-footer` | `footer.html` |
| Vaste balk met volgende meetup (alleen mobiel) | `.n-sticky` | `sticky-next.html` |

Code of Conduct: hero met kruimelpad (`.c-hero`), inhoudsopgave plus meldblok links (`.c-aside`, sticky op desktop), tekst rechts (`.c-body`), lijsten wel/niet (`.c-lists`) en de handhavingsladder (`.c-ladder`).

## Design tokens

### Kleuren

| Naam | Waarde | Gebruik |
|---|---|---|
| Merkblauw | `#0077FF` | Logo, illustraties, lijnen en kaders, accentwoorden in grote koppen |
| Knopblauw | `#0066E0`, hover `#0055BD` | Vlakken met witte tekst (contrast 5,3:1) |
| Linkblauw | `#0062D6`, hover `#004FAD` | Links en labels op licht (5,6:1) |
| Inkt | `#0A1630` | Koppen en tekst, mobiele balk |
| Tekstgrijs | `#2B3A55` | Lead en lopende tekst |
| Cloud grey | `#FAFAFA` | Paginagrond |
| Wit | `#FFFFFF` | Meetup-blok, footer, meldblok |
| Tekenvel | `#EEF5FF` | Achtergrond achter de kaartillustraties |
| Foto-placeholder | `#DCEBFF` | Alleen in het ontwerp, verdwijnt met echte foto's |
| Raster | `#0077FF` op 7%, 32px | Hero, beeldmerk-constructie, slotblok |

Wit op `#0077FF` haalt geen 4,5:1. Gebruik voor vlakken met kleine witte tekst daarom altijd Knopblauw, en voor kleine blauwe tekst op licht Linkblauw. Merkblauw is voor grafische elementen en koppen van 24px of groter.

### Typografie

Google Fonts: Poppins (500, 600, 700) en IBM Plex Sans (400, 500, 600). Poppins sluit aan op het woordmerk, Plex Sans was al de bodyfont.

| Stijl | Font | Desktop | Mobiel |
|---|---|---|---|
| H1 | Poppins 600, letter-spacing -0.02em | clamp(42px, 6vw, 76px) / 1.05 | 42px / 1.06 |
| H2 | Poppins 600 | clamp(30px, 4vw, 48px) / 1.15 | 30px |
| H3 | Poppins 600 | 22px | 20px |
| Lead | Plex Sans 400 | 20px | 17px |
| Body | Plex Sans 400 | 18px / 1.7 | 16px / 1.65 |
| Label | Plex Sans 500 | 14px, Linkblauw | 13 tot 14px |

### Maten

- Container: max 1240px, zijmarge 24px (16px op mobiel).
- Ruimte tussen secties: 128px desktop, 104px tablet, 72px mobiel.
- Hoeken: overal recht (radius 0), ook bij knoppen. Uitzondering: de stempel is een cirkel.
- Lijnen: 1px Merkblauw voor kaders en scheidingen, 2px voor het Meetup-blok en het slotblok.
- Klikgebied minimaal 44px, op mobiel 52px voor de hoofdknoppen.

## Breakpoints

| | Desktop, vanaf 1024 | Tablet, 768 tot 1023 | Mobiel, tot 767 |
|---|---|---|---|
| Header | Logo, 3 links, Meetup-knop | Gelijk aan desktop | Alleen logo en Meetup-knop |
| Hero | 2 kolommen, stempel half over de tekstkolom | Gestapeld, skyline gecentreerd (max 480px), stempel linksboven | Gestapeld, knoppen volle breedte |
| Volgende meetup | 1 rij: titel, wanneer, waar, aanmelden | 2x2, aanmelden over de volle breedte | Gelijk aan tablet, compacter |
| Foto's | 4 kolommen: groot 2x2, 2 klein, 1 breed | 2 kolommen | 2 kolommen, rijen van 150px |
| Over | 2 kolommen | Gestapeld | Gestapeld |
| Kaarten | 3 naast elkaar | Liggend: illustratie links, tekst rechts | Gestapeld |
| Footer | 4 kolommen | Merk bovenaan, 3 kolommen eronder | 1 kolom |
| Vaste balk | Verborgen | Verborgen | Zichtbaar onderin |

## Animaties

Alles staat in `assets/site.css`. Bij `prefers-reduced-motion: reduce` staat alles stil en is alles direct zichtbaar.

| Wat | Hoe | Timing |
|---|---|---|
| Skyline | Tekent zich van links naar rechts (`clip-path`), met een blauwe scanlijn | 2,8s, cubic-bezier(.65,0,.35,1), 0,2s vertraging |
| Stempel "Aanmelden via Meetup" | Tekst draait rond het beeldmerk, schaalt iets op bij hover | 24s lineair, oneindig |
| Beeldmerk-constructie | Lijn wordt overgetrokken (`stroke-dashoffset`), daarna vult het vlak tot 12% | 3s, dan 1s |
| Knoppen | Kleur donkerder, 2px omhoog | 0,2s |

In de export starten de animaties bij het laden. In de echte build is het beter om de beeldmerk-animatie te starten zodra de sectie in beeld komt (IntersectionObserver).

## Afbeeldingen

### SVG (in `assets/img/`)

- `logo.svg` en `logo-wit.svg`: woordmerk met beeldmerk, zonder `<style>`-blok (fill staat direct op de paden).
- `skyline.svg`: de bestaande Nijmegen-skyline uit het huidige theme.
- `illu-praatje.svg`, `illu-samen-coderen.svg`, `illu-op-bezoek.svg`, `illu-na-afloop.svg`: nieuw getekend in dezelfde lijnstijl (1px `#0077FF`, geen vulling). `illu-praatje.svg` heeft de hoofden ingekleurd met `#EEF5FF` zodat ze het statief afdekken; `illu-praatje-licht.svg` is dezelfde tekening met `#FAFAFA`, voor op de gewone achtergrond.
- `illu-dojo.svg`: de bestaande dojo-tekening, nu niet gebruikt.
- Het beeldmerk staat inline in de HTML (stempel en constructie), zodat het geanimeerd kan worden.

### Foto's

De vier vakken in `.n-photos` zijn nog placeholders (`<figure class="n-ph">`). Vervang ze door een `<img>` met `object-fit: cover` die het vak vult, en laat de `figcaption` staan.

- Kies 4 foto's uit `~/Downloads/nimma-meetup-fotos/overzicht.html`: een volle zaal (groot vak), een spreker, mensen samen achter een laptop en de borrel na afloop.
- Lever ze aan als WebP, rond 1600px breed voor het grote vak en 800px voor de kleine, met `srcset` en `loading="lazy"` behalve voor de bovenste.
- Let op: `2025-03-25_soft-skills-monads_01.png` is een PNG en `2026-10-16_meetup-group-photo-album_05.avif` is AVIF. De rest is JPEG, ongeveer 5 MB per stuk.
- Vraag bij herkenbare mensen even na of ze op de site mogen. Op Meetup staan ze al, maar een website is iets anders.

## Content die nog ingevuld moet worden

- Volgende meetup: `[TITEL VAN DE AVOND]`, `[DATUM]`, `[TIJD]`, `[HOST]`, `[STAD]`. Bij voorkeur automatisch, zie de ADR.
- Foto's, zie hierboven.

## Links

| Doel | URL |
|---|---|
| Meetup-groep | https://www.meetup.com/nimma-codes-meetup-group/ |
| Meetup-foto's | https://www.meetup.com/nimma-codes-meetup-group/photos/ |
| Discord | https://discord.gg/XN6P9X3WKj |
| GitHub-organisatie | https://github.com/nimma-codes |
| Talk indienen | https://github.com/nimma-codes/talks/issues |
| Broncode site | https://github.com/nimma-codes/preview.nimma.codes |
| Contact en melden | mailto:info@nimma.codes |

## Toegankelijkheid

- Echte `<a>`- en `<button>`-elementen, `aria-label` op de stempel en het logo, decoratieve SVG's met `aria-hidden="true"`.
- Alle tekst haalt 4,5:1, zie de kleurentabel.
- Alt-teksten staan in de export en beschrijven de illustraties; de illustratie in het slotblok op mobiel is decoratief.
- Volgorde van de headings: één H1 per pagina, daarna H2 per sectie.

## Open punten voor de code-sessie

1. **Volgende meetup automatisch tonen.** Zie `docs/adr/0001-hugo-en-meetup-feed.md`. Eerst uitzoeken welke feed Meetup voor de groep aanbiedt.
2. **Hosting en deploy.** Nagaan waar nimma.codes nu draait en hoe het nu gedeployed wordt, en dat vastleggen in `docs/SETUP.md`.
3. **Code of Conduct.** De tekst is overgenomen van de huidige pagina. Er kwam twee keer de kop "Handhaving" in voor; de tweede heet in het ontwerp nu "Melden". Even akkoord geven.
4. **OG-afbeelding en favicon.** De huidige `og_img.png` en favicons kunnen blijven, of opnieuw gemaakt worden met de skyline.
5. **Projectdocumentatie.** Maak of werk bij in de repo: `docs/SETUP.md` (lokaal draaien met Hugo, deploy, eventuele env-variabelen voor de feed) en deze ADR.
