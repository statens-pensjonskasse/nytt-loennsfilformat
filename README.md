# Nytt lønnsfilformat

Her ligger forslag til formatet lønnssystemene kan bruke når de rapporterer stillinger og lønn til Statens pensjonskasse (SPK).
Formatet er definert i [JSON Schema](https://json-schema.org/) (draft 2020-12) i katalogen `json/`, og skjemaene er
kilden til sannhet.

Formatet er et utkast og kan endres uten varsel. Versjonen er `3.0.0-SNAPSHOT`.

## Forslag og tilbakemeldinger

Skjemaene er et forslag. Vi vil gjerne høre hva dere mener om det som er nytt sammenlignet med
kravspesifikasjonen for v2.7.

**Tidslinjer i stedet for meldinger**

- Lønnssystemet sender hele tidslinjen for personen fra datoen endringen gjelder fra (`gjelderFraOgMed`).
  Status- og endringsmeldingene faller bort, og SPK finner selv ut hva som er endret.
- Slutt er siste dag i en periode, med sluttårsak. Et opphold mellom to perioder betyr at personen ikke var
  ansatt.

**Stillingene**

- Hver stilling sendes for seg med en stabil `stillingsid` fra lønnssystemet. Lønnssystemet slår ikke
  sammen stillinger med lik aldersgrense og justerer ikke ned til 100 %.
- Ved bytte av lønnssystem sender det nye systemet en `stillingsidendring`. Da beholder stillingen
  historikken sin hos SPK.
- `gruppe` knytter stillingen til riktig avtale hos arbeidsgivere med flere avtaler. Da slipper SPK å gjette
  på avtalen.
- `pensjonsordning` oppgir TP-nummeret, så et felles mottak kan ta imot for flere ordninger.
- `ansettelsesform` sier om personen er fast eller midlertidig ansatt.

**Timer og permisjon**

- Timelønnede og ekstratimer meldes som faktiske timer per stilling og måned. SPK regner ut
  stillingsstørrelsen, og arbeidsgiveren slipper å regne gjennomsnitt.
- Delvis permisjon oppgis med hvor stor del av stillingen som er uten lønn.

**Svaret fra SPK**

- SPK svarer med en kvittering per person. En feil hos en person stopper ikke de andre.
- Feilene har faste koder og peker på feltet de gjelder, så lønnssystemet kan vise dem til brukeren.
- `innsendingsId` gjør det trygt å sende på nytt etter tidsavbrudd. `registrertFra` og `registrertTilOgMed`
  viser hvilket tidsrom innsendingen dekker, så ingenting faller mellom to innsendinger.

## Skjemaene

| Fil | Innhold |
|---|---|
| [`json/tidslinje.json`](json/tidslinje.json) | Innsendingen fra lønnssystemet |
| [`json/kvittering.json`](json/kvittering.json) | SPKs svar på en innsending |
| [`json/felles.json`](json/felles.json) | Typer som brukes i begge, som versjon, dato, fødselsnummer og beløp |

## Eksempler

[`eksempler/`](eksempler) har innsendinger for vanlige situasjoner, som ny ansatt, permisjon, timelønn, sluttårsaker,
endring tilbake i tid og bytte av lønnssystem. Hvert eksempel har kvitteringene SPK svarer med. Personene og
arbeidsgiverne er syntetiske testdata fra Tenor, og alle Tenor-personene og -virksomhetene vi bruker, står i
[`testdata/tenor/`](testdata/tenor).

## Versjonering

Formatet bruker [semantisk versjonering](https://semver.org). Versjonen står i feltet `versjon` i både innsending og
kvittering, og gyldige verdier står i `json/felles.json`.

Så lenge versjonen er `0.x.y-SNAPSHOT`, kan formatet endres uten at det er bakoverkompatibelt. Fra versjon 1.0.0 skal
en fil som validerte før, fortsatt validere innenfor samme majorversjon. Eksisterende verdier fjernes bare i en ny
majorversjon. Feilkodene i kvitteringen er en åpen liste: avsender må tåle koder den ikke kjenner.

## Henvendelser

Formatet eies og forvaltes av team-kundedata i Statens pensjonskasse.
