# ADR 0001: Bij Hugo blijven en de volgende meetup bij het bouwen ophalen

Status: aanvaard
Datum: 2026-10-29

## Context

De nieuwe site is een one-pager die mensen naar Meetup stuurt. Bovenaan staat een blok met de volgende meetup: titel, datum, tijd, host en plaats. Als iemand dat met de hand moet bijhouden, raakt het verouderd.

De huidige site is gebouwd met Hugo en een eigen theme (`nimma-codes`), met de broncode in `nimma-codes/preview.nimma.codes`. Bij het nieuwe ontwerp is de broncode verhuisd naar `nimma-codes/www.nimma.codes`, zodat bron en hosting in één repo staan. Het nieuwe ontwerp is gewone HTML en CSS zonder framework of client-side JavaScript, op een paar animaties na.

## Beslissing

1. We blijven bij Hugo. Het ontwerp wordt een nieuw theme, of een vervanging van het huidige, opgeknipt in partials zoals in `docs/HANDOFF.md` staat.
2. De volgende meetup halen we op tijdens de build met `resources.GetRemote`, uit de openbare iCal-feed van de groep (`/events/ical/`). De RSS-feed geeft alleen de publicatiedatum, niet de datum van de avond. De API vraagt om OAuth. iCal heeft titel, starttijd en link, maar geen locatie: de host halen we uit de titel (het deel na `@`).
3. Een geplande GitHub Action bouwt en deployt de site elke dag opnieuw, zodat het blok actueel blijft zonder handwerk. De site heeft zo geen redactioneel onderhoud nodig.
4. Lukt het ophalen niet, of staat er geen meetup gepland, dan toont het blok "Volgende meetup wordt binnenkort aangekondigd" met de knop naar Meetup. De build faalt daar niet op.
5. Twee vangnetten houden het onderhoudsvrij. Een script in de pagina toont de knop naar Meetup zodra de getoonde avond voorbij is, ook als de build is stilgevallen. Een keepalive-job zet een lege commit op `main` na 50 dagen stilte, zodat GitHub de geplande build niet uitzet. De bekende action `keepalive-workflow` is door GitHub geblokkeerd, daarom staat de stap in de workflow zelf.

## Redenering

- Hugo staat er al en is snel. Voor een one-pager is een JavaScript-framework overbodig.
- Ophalen tijdens de build houdt de site statisch: geen API-sleutels in de browser, geen laadvertraging, geen afhankelijkheid van Meetup op het moment dat iemand de site bezoekt.
- Eén keer per dag is vaak genoeg voor meetups die maandelijks plaatsvinden. Na het aankondigen van een avond kan de Action ook met de hand worden gestart.

## Gevolgen

- Beide deploy-workflows hebben een dagelijkse planning, plus documentatie in `docs/SETUP.md`.
- Wat op `main` staat, staat binnen een dag op www. Er is geen preview-site meer; review gebeurt in de PR.
- De feed vraagt geen sleutel.
- De Hugo-versie staat vast op 0.168.0 in de workflows, omdat de template `try` gebruikt en eerder `latest` gold.
- Werkt Meetup zijn feed bij, dan kan het blok terugvallen op de tekst zonder datum. Dat is zichtbaar maar niet kapot.
