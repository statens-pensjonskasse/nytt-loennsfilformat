# Nytt lønnsfilformat

Her ligger formatet lønnssystemene bruker når de rapporterer stillinger og lønn til Statens pensjonskasse (SPK).
Formatet er definert i [JSON Schema](https://json-schema.org/) (draft 2020-12) i katalogen `json/`, og skjemaene er
kilden til sannhet.

Formatet er et utkast og kan endres uten varsel. Versjonen er `0.1.0-SNAPSHOT`.

## Skjemaene

| Fil | Innhold |
|---|---|
| [`json/tidslinje.json`](json/tidslinje.json) | Innsendingen fra lønnssystemet |
| [`json/kvittering.json`](json/kvittering.json) | SPKs svar på en innsending |
| [`json/felles.json`](json/felles.json) | Typer som brukes i begge, som versjon, dato, fødselsnummer og beløp |

Hvert felt har tittel og beskrivelse i skjemaet. Les dem der, så slipper du at README og skjema sier forskjellige
ting.

## Eksempler

[`eksempler/`](eksempler) har innsendinger for vanlige situasjoner, som ny ansatt, permisjon, timelønn, sluttårsaker,
endring tilbake i tid og bytte av lønnssystem. Hvert eksempel har kvitteringene SPK svarer med. Personene og
arbeidsgiverne er syntetiske testdata fra Tenor, og alle Tenor-personene og -virksomhetene vi bruker, står i
[`testdata/tenor/`](testdata/tenor).

## Versjonering

Formatet bruker [semantisk versjonering](https://semver.org). Versjonen står i feltet `versjon` i både innsending og
kvittering, og gyldige verdier står i `json/felles.json`.

Så lenge versjonen er `0.x.y-SNAPSHOT`, kan formatet endres uten at det er bakoverkompatibelt. Fra versjon 1.0.0 skal
en fil som validerte før, fortsatt validere innenfor samme majorversjon. Nye valgfrie felter og nye enum-verdier kan
komme i en minorversjon. Eksisterende verdier fjernes bare i en ny majorversjon.

## Henvendelser

Formatet eies og forvaltes av team-kundedata i Statens pensjonskasse.
