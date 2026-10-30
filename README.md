# Nimma.codes Website

Deze repository bevat de sources voor https://www.nimma.codes. 
Het is een statische website gegenereerd met Hugo.

## De website lokaal draaien

Het enige wat je nodig hebt om deze website te bouwen is [Hugo 0.168+ (extended)](https://gohugo.io/getting-started/installing/).
Vervolgens run je `hugo server` in the root van deze repository.
De website is nu live op http://localhost:1313/.

### Met Docker

Als je Docker gebruikt, kan je eenvoudig Hugo draaien met dit command:

```shell
docker run --rm -it -p "1313:1313" -v "$PWD:/project" hugomods/hugo:exts server --bind 0.0.0.0
```

## Wijzigingen publiceren

Maak een PR op deze repo. Zodra die gemerged is naar `main`, bouwt de workflow `deploy.yml` de site en zet hem in de branch `gh-pages`. GitHub Pages serveert die op https://www.nimma.codes.

De workflow draait ook elke ochtend, zodat de volgende meetup vanzelf bijgewerkt wordt. Zie [docs/SETUP.md](docs/SETUP.md).

## Documentatie

- [docs/SETUP.md](docs/SETUP.md): lokaal draaien, deploy, externe afhankelijkheden en testen.
- [docs/design/](docs/design/): het ontwerp (A2 Blauwdruk) met de spec in `HANDOFF.md`.
- [docs/adr/](docs/adr/): vastgelegde beslissingen.
