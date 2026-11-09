# @ygdigital.nl: automatisch plaatsen (YG Digital HQ)

Sinds 9 november 2026 is het Instagram-account @ygdigital.nl van **YG Digital HQ**, niet meer van Manon
(besluit Andries, 9 nov: *"nee de yg-digital.nl instagram word van het yg-digital HQ en niet meer van manon"*).
Deze map is de eigen automaat van het account. De techniek komt uit `yg-instagram` (Manon), die daar sinds
september werkt; hier staat alleen wat @ygdigital.nl nodig heeft.

Het account is het Nederlandstalige account van het bedrijf: zie het merkplan in `yg-digital HQ/concept/merkplan-2026-11-09.md`.

## Hoe het werkt

Elk uur kijkt GitHub Actions in `planning.json` wat er aan de beurt is, plaatst dat via de Instagram API
(`graph.instagram.com`, Instagram-login) en schrijft het weg in `gedaan.json`, zodat niets twee keer gebeurt.
Je computer mag uit. Kosten: geen (Meta rekent niets, Actions is gratis voor openbare mappen).

| | |
|---|---|
| `planning.json` | Wat er wanneer geplaatst wordt. **Leeg zolang het account op pauze staat.** |
| `beeld/` | De platen als JPEG. Instagram haalt ze hier zelf op. |
| `pakketten/` | Goedgekeurde pakketten: platen, `bijschrift.txt`, `beeldbeschrijvingen.txt`. |
| `gedaan.json` | Wat al geplaatst is. Wordt vanzelf bijgewerkt. |
| `sleutel.json` | Vingerafdruk en datum van de sleutel, om op tijd te waarschuwen (verschijnt na de eerste run). |
| `src/plaatsen.cjs` | Het script dat het werk doet. |
| `src/instagram.cjs` | De koppeling met Instagram. |
| `gereedschap/plan-carrousel.cjs` | Zet één goedgekeurde carrousel uit een pakket in de planning. |
| `gereedschap/controleer.cjs` | Toetst of sleutel en accountnummer werken. |
| `gereedschap/vernieuw-token.cjs` | Elke twee maanden: vernieuwt de sleutel. |

De map is **openbaar**, want Instagram haalt de platen op van een openbaar adres. De sleutel staat in GitHub Secrets en nooit in de bestanden.

## Eenmalig instellen (Andries, ongeveer 15 minuten)

1. **Map op GitHub.** `SteinClaude/yg-hq-instagram`, openbaar. (Claude maakt hem aan na je go.)
2. **Nieuwe sleutel voor @ygdigital.nl.** In de bestaande Meta-app *YG Digital plaatser* (ontwikkelmodus;
   @ygdigital.nl heeft daar al de rol Instagram Tester): *Instagram* → *API setup with Instagram login* →
   bij @ygdigital.nl **Generate token**. Kopieer de sleutel en het accountnummer (Instagram user ID).
   De oude sleutel (in yg-instagram) is van 16 september en verloopt rond 15 november.
3. **Twee geheimen zetten** in deze map op GitHub: *Settings → Secrets and variables → Actions → New repository secret*:
   - `IG_TOKEN`: de sleutel
   - `IG_USER_ID`: het accountnummer
4. **Proef:** *Actions → Plaatsen op Instagram (@ygdigital.nl) → Run workflow*. Met een lege planning
   meldt hij "Sleutel werkt, account @ygdigital.nl" en "Niets te plaatsen". Er wordt niets geplaatst.

Pas daarna haalt Eva @ygdigital.nl uit de automaat van Manon (geheimen `IG_TOKEN` en `IG_USER_ID` daar weg),
zodat er nooit twee automaten voor hetzelfde account draaien.

## Regels

- **Niets in de planning zonder "pakket OK <code>" van Andries** voor dat pakket. Elk OK letterlijk,
  met datum en tijd, in de commit die erop volgt.
- Prijs alleen in de "zo werkt het"-post. Geen echte klanten of prospects in beeld of tekst zonder hun toestemming.
- Woordkeus bij een weggeefactie: "weggeefactie/gekozen", nooit "winactie/winnen/kans".
- Claude logt nergens in en plaatst nooit buiten de automaat.

## Sleutel vernieuwen (elke 60 dagen)

De automaat opent zelf een melding "Sleutel vernieuwen" op GitHub, twaalf dagen voordat de sleutel verloopt.
Dan op je eigen computer: `node gereedschap/vernieuw-token.cjs HUIDIGE_SLEUTEL` en de nieuwe sleutel bij
`IG_TOKEN` zetten. Of in de Meta-app een nieuwe genereren (stap 2).
