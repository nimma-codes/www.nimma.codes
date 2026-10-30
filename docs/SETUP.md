# Setup

## Lokaal draaien

1. Installeer Hugo extended, versie 0.168.0 (dezelfde als in `.github/workflows/`). Op macOS: `brew install hugo`.
2. Draai `hugo server` in de root van de repo.
3. Open http://localhost:1313/.

Met Docker kan het ook. Dit image volgt de nieuwste Hugo-versie, niet de vaste versie uit de workflows:

```shell
docker run --rm -it -p "1313:1313" -v "$PWD:/project" hugomods/hugo:exts server --bind 0.0.0.0
```

## Environment-variabelen

Er zijn geen secrets nodig. De Meetup-feed is openbaar. Wil je lokaal een andere feed testen, zet dan `HUGO_PARAMS_MEETUP_ICAL`:

```shell
HUGO_PARAMS_MEETUP_ICAL=https://example.com/feed.ics hugo server
```

## Testdata

Geen database of seeds. Het blok "Volgende meetup" komt bij elke build uit de iCal-feed van de Meetup-groep (`params.meetup.ical` in `config.toml`). Staat er niets gepland of is de feed onbereikbaar, dan toont het blok "Wordt binnenkort aangekondigd" en geeft Hugo een waarschuwing. De build faalt niet.

Valt de dagelijkse build stil, dan kan er een avond op de site blijven staan die al geweest is. Een klein script in `partials/home/next-meetup.html` vangt dat op: is het vier uur na de start, dan toont het blok in de browser de knop naar Meetup en verdwijnt de mobiele balk.

## Externe afhankelijkheden

| Systeem | Hoe gekoppeld |
|---|---|
| Meetup | iCal-feed tijdens de build, zie `docs/adr/0001-hugo-en-meetup-feed.md` |
| Google Fonts | Poppins en IBM Plex Sans via `<link>` in `partials/head.html` |
| GitHub Pages | Hosting van de branch `gh-pages`, gevuld via `peaceiris/actions-gh-pages` |

## Deploy

`.github/workflows/deploy.yml` bouwt de site en zet het resultaat in de branch `gh-pages` van deze repo. GitHub Pages serveert die branch op https://www.nimma.codes. De workflow draait:

- bij elke push naar `main`;
- elke dag om 05:15 UTC, voor het blok "Volgende meetup";
- met de hand via Actions, "Run workflow".

Er is geen aparte preview-site meer. Bekijk wijzigingen lokaal met `hugo server` of in de PR, en merge pas als het klaar is: wat op `main` staat, staat live.

De branch `gh-pages` bevat alleen gegenereerde bestanden. Pas daar niets met de hand aan, de volgende build overschrijft het.

GitHub zet geplande workflows in publieke repo's uit als er 60 dagen niets in de repo gebeurt. De job `keepalive` in dezelfde workflow voorkomt dat: staat `main` 50 dagen stil, dan zet hij er een lege commit op. Staat de build toch uit, zet hem dan weer aan onder Actions.

## Testen

1. Draai `hugo server` en open de homepage.
2. Maak het venster smaller om tablet (768 tot 1023) en mobiel (tot 767) te zien. Op mobiel verschijnt onderin de vaste balk met de volgende meetup.
3. Test de terugval van het meetup-blok met een kapotte feed-URL via `HUGO_PARAMS_MEETUP_ICAL`.

## Opbouw van het theme

- `themes/nimma-codes/layouts/index.html` zet de homepage samen uit `partials/home/*`.
- `partials/meetup/next.html` leest de feed en geeft titel, host, starttijd en link terug.
- `_default/single.html` is de layout voor losse pagina's. Met `report: true` in de front matter komt er een inhoudsopgave en meldblok bij, zoals op de Code of Conduct.
- Foto's: zie `data/photos.yaml`.
- Het ontwerp en de spec staan in `docs/design/`.
