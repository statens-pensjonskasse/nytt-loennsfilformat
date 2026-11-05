# Testdata fra Tenor

Personene og virksomhetene her er syntetiske testdata fra [Tenor](https://www.skatteetaten.no/testdata/),
Skatteetatens testdatasøk. Bruk dem i eksempler og tester i stedet for å finne på fødselsnumre, organisasjonsnumre og
navn.

| Fil | Innhold |
|---|---|
| [`personer.json`](personer.json) | Personer fra Folkeregisteret i Tenor med fødselsnummer, navn, fødselsdato, kjønn og personstatus |
| [`virksomheter.json`](virksomheter.json) | Virksomheter fra Enhetsregisteret i Tenor med organisasjonsnummer, navn, organisasjonsform og næringskode |

Fødselsnumrene har måneden lagt til 80, så de kan aldri tilhøre en ekte person. Kontrollsifrene stemmer. Listen har
også en person med D-nummer (`71888500566`), en død person (`08818795980`) og en utflyttet person (`26813547111`).
Navnene er syntetiske navn fra Tenor, skrevet med stor forbokstav.

Virksomhetene er to organisasjonsledd i staten og et apotek med underenheten sin. Apoteket brukes for regulativ 105.

## Slik hentes flere

Søk etter personer under **Avansert søk → Person** i Tenor, for eksempel

```
foedselsdato:[1960-01-01 to 2005-12-31] and personstatus:bosatt
```

og bruk **Eksporter resultat** over trefflisten. Legg nye personer til i `personer.json` med de samme feltene, sortert
etter fødselsnummer.
