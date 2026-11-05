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

## Slik er en innsending bygd opp

En innsending gjelder en arbeidsgiver og dekker tidsrommet fra `registrertFra` til og med `registrertTilOgMed`. Neste
innsending for samme arbeidsgiver starter der den forrige sluttet.

Innsendingen har en liste med personer. For hver person sendes alt som er endret og ikke sendt før.

- **Tidslinjen** gjelder fra og med datoen i `gjelderFraOgMed`. SPK beholder det som ligger før denne datoen og
  erstatter alt fra og med datoen med periodene i innsendingen. En tom periodeliste fjerner alt fra og med datoen.
- **Periodene** er tidsrom der stillingene, lønnen og permisjonene er uendret. En periode uten `tilOgMed` er løpende.
  Et opphold mellom to perioder betyr at personen ikke var ansatt i oppholdet.
- **Stillingsforholdene** i en periode er stillingene slik de står i lønnssystemet, hver med sin `stillingsid`,
  stilling, arbeidsinntekt og eventuell permisjon. Flere stillinger hos samme arbeidsgiver sendes hver for seg.
- **Timer** sendes per stilling og måned for timelønnede og for ekstratimer i en månedslønnet stilling.
- **Stillingsidendringer** brukes når en stilling får ny ID, for eksempel når arbeidsgiveren bytter lønnssystem.
  Historikken på den gamle ID-en flyttes da over på den nye.

SPK behandler hver person for seg. En feil hos en person stopper ikke de andre.

## Eksempler

[`eksempler/`](eksempler) har innsendinger for vanlige situasjoner, som ny ansatt, permisjon, timelønn, sluttårsaker,
endring tilbake i tid og bytte av lønnssystem. Hvert eksempel har kvitteringene SPK svarer med. Personene og
arbeidsgiverne er syntetiske testdata fra Tenor, og alle Tenor-personene og -virksomhetene vi bruker, står i
[`testdata/tenor/`](testdata/tenor).

## Gangen i en innsending

1. Lønnssystemet sender innsendingen. SPK lagrer den og svarer med kvitteringen og hvor den kan hentes.
2. Lønnssystemet henter kvitteringen igjen til status er `FERDIG` eller `AVVIST`. Mens den er `MOTTATT` eller
   `BEHANDLES`, sier `Retry-After` hvor mange sekunder det bør ventes.
3. Når status er `FERDIG`, har kvitteringen resultatet for hver person, med feilkoder der noe er galt. Personene
   identifiseres med plassen sin i innsendingen.

Lønnssystemet sender bare en innsending om gangen per arbeidsgiver og venter på endelig kvittering før det sender neste.
`innsendingsId` er idempotensnøkkelen. Sendes samme innsending på nytt, for eksempel etter tidsavbrudd, får
lønnssystemet kvitteringen fra første gang.

## Verdier

- Beløp oppgis i øre som heltall. 100 betyr 1 krone.
- Prosenter oppgis i basispunkter som heltall. 10 000 betyr 100 % og 8 694 betyr 86,94 %.
- Datoer skrives `YYYY-MM-DD`. Tidspunkter skrives etter RFC 3339 i UTC, for eksempel `2026-11-05T12:00:00Z`.
- Fødselsnummer og D-nummer har 11 sifre. Kontrollsifrene sjekkes av SPK etter skjemavalideringen.

Beløp og prosenter er heltall slik at de aldri blir flyttall.

## Validering

Skjemaene refererer til hverandre med `$id` under `https://spk.no/schemas/spk-medlemsdata/json/`. Adressene er
identifikatorer og kan ikke hentes over nett. En validator må derfor mappe prefikset til katalogen `json/` lokalt.

Skjemaet sjekker formen på innsendingen. SPK sjekker i tillegg regler som skjemaet ikke kan uttrykke, for eksempel
kontrollsiffer, overlappende perioder og samme person flere ganger i en innsending. Bruddene kommer som feilkoder i
kvitteringen.

## Versjonering

Formatet bruker [semantisk versjonering](https://semver.org). Versjonen står i feltet `versjon` i både innsending og
kvittering, og gyldige verdier står i `json/felles.json`.

Så lenge versjonen er `0.x.y-SNAPSHOT`, kan formatet endres uten at det er bakoverkompatibelt. Fra versjon 1.0.0 skal
en fil som validerte før, fortsatt validere innenfor samme majorversjon. Nye valgfrie felter og nye enum-verdier kan
komme i en minorversjon. Eksisterende verdier fjernes bare i en ny majorversjon.

## Henvendelser

Formatet eies og forvaltes av team-kundedata i Statens pensjonskasse.
