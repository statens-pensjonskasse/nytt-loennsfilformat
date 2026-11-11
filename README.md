# Nytt lønnsfilformat

Her ligger forslag til formatet lønnssystemene kan bruke når de rapporterer stillinger og lønn til Statens pensjonskasse (SPK).
Formatet er definert i [JSON Schema](https://json-schema.org/) (draft 2020-12) i katalogen `json/`, og skjemaene er
kilden til sannhet.

Formatet er et utkast og kan endres uten varsel. Versjonen er `3.0.0-SNAPSHOT`.

## Forslag og tilbakemeldinger

Skjemaene er et forslag. Under står det som er nytt sammenlignet med kravspesifikasjonen for v2.7, og
spørsmålene vi trenger svar på for hvert punkt. Svar gjerne med hvordan lønnssystemet gjør det i dag, også
når svaret er at det ikke går.

**Tidslinjer i stedet for meldinger**

- Lønnssystemet sender hele tidslinjen for personen fra datoen endringen gjelder fra (`gjelderFraOgMed`).
  Status- og endringsmeldingene faller bort, og SPK finner selv ut hva som er endret.
- Slutt er siste dag i en periode, med sluttårsak. Et opphold mellom to perioder betyr at personen ikke var
  ansatt.

Spørsmål:

1. Kan lønnssystemet hente ut hele tidslinjen for en person fra en vilkårlig dato, med de verdiene som
   gjelder etter senere rettinger?
2. Hvor langt tilbake i tid gjør dere endringer, og hvor ofte skjer det?
3. Registrerer dere sluttårsak for alle som slutter? Hvis ikke, hvilke sluttårsaker mangler?

**Stillingene**

- Hver stilling sendes for seg med en stabil `stillingsid` fra lønnssystemet. Lønnssystemet slår ikke
  sammen stillinger med lik aldersgrense og justerer ikke ned til 100 %.
- Ved bytte av lønnssystem sender det nye systemet en `stillingsidendring`. Da beholder stillingen
  historikken sin hos SPK.
- `gruppe` knytter stillingen til riktig avtale hos arbeidsgivere med flere avtaler. Da slipper SPK å gjette
  på avtalen.
- `pensjonsordning` oppgir TP-nummeret, så et felles mottak kan ta imot for flere ordninger.
- `ansettelsesform` sier om personen er fast eller midlertidig ansatt.

Spørsmål:

1. Hvilket felt i lønnssystemet kan brukes som `stillingsid`? Beholder det samme verdi når stillingsprosent,
   stillingskode eller lønn endres?
2. Får stillingen ny id når personen flytter til en annen enhet eller et annet organisasjonsnummer hos samme
   arbeidsgiver?
3. Når en arbeidsgiver bytter lønnssystem, får det nye systemet med seg stillingsidene fra det gamle, så det kan
   sende `stillingsidendring`?
4. Registrerer dere avtalen (`gruppe`) og TP-nummeret? Hvis ja, ligger de på stillingen eller på arbeidsgiveren?
5. Er `ansettelsesform` den samme verdien som dere sender i a-meldingen?

**Timer og permisjon**

- Timelønnede og ekstratimer meldes som faktiske timer per stilling og måned. SPK regner ut
  stillingsstørrelsen, og arbeidsgiveren slipper å regne gjennomsnitt.
- Full stilling oppgis som timer per uke og arbeidstidsordning, på samme måte som i a-meldingen. Timene
  gjelder måneden de ble arbeidet, og en rettelse sender hele timetallet for måneden på nytt. A-meldingen
  bruker utbetalingsmåneden og sender rettelser som etterbetaling.
- Delvis permisjon oppgis med hvor stor del av stillingen som er uten lønn.

Spørsmål:

1. Kan dere skille timer per stilling når personen har flere stillinger hos samme arbeidsgiver?
2. Kan dere oppgi timene for måneden de ble arbeidet? Hvis ikke, hvilken måned kan dere oppgi dem for?
3. Når timer rettes, har dere det nye timetallet for hele måneden? Hvis ikke, hva har dere?
4. Er timer per uke for full stilling og arbeidstidsordning registrert per stilling?
5. Hvordan registrerer dere delvis permisjon i dag? Er det prosenten uten lønn eller prosenten med lønn som
   lagres?

**Svaret fra SPK**

- SPK svarer med en kvittering per person. En feil hos en person stopper ikke de andre.
- Feilene har faste koder og peker på feltet de gjelder, så lønnssystemet kan vise dem til brukeren.
- `innsendingsId` gjør det trygt å sende på nytt etter tidsavbrudd. `registrertFra` og `registrertTilOgMed`
  viser hvilket tidsrom innsendingen dekker, så ingenting faller mellom to innsendinger.

Spørsmål:

1. Kan lønnssystemet ta imot svar per person og sende på nytt bare for personene som feilet?
2. Hvem retter feilene hos dere, og hvor i lønnssystemet ser de dem?
3. Kan lønnssystemet lagre `innsendingsId` og bruke samme verdi når det sender på nytt etter tidsavbrudd?
4. Mangler det noe i kvitteringen for at brukeren skal kunne rette feilen uten å kontakte SPK?

**Ellers**

1. Bruker dere noe fra v2.7 som ikke finnes i dette formatet?

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
majorversjon. Feilkodene i kvitteringen er en åpen liste, og avsender må tåle koder den ikke kjenner.

## Henvendelser

Formatet eies og forvaltes av team-kundedata i Statens pensjonskasse.
