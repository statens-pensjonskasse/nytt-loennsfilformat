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
- Slutt er siste dag i en periode, med sluttårsak. Sluttårsak oppgis bare når perioden ikke følges direkte av en
  ny periode. Et opphold mellom to perioder betyr at personen ikke var ansatt. Bare perioden som starter sist,
  kan være løpende.
- En tom periodeliste fjerner alt SPK har fra og med `gjelderFraOgMed`. Er personen rapportert ved en feil, sendes
  tom periodeliste fra første dag SPK har for personen, og timene rettes til 0.

Spørsmål:

1. Kan lønnssystemet hente ut hele tidslinjen for en person fra en vilkårlig dato, med de verdiene som
   gjelder etter senere rettinger?
2. Hvor langt tilbake i tid gjør dere endringer, og hvor ofte skjer det?
3. Registrerer dere sluttårsak for alle som slutter? Hvis ikke, hvilke sluttårsaker mangler?

**Stillingene**

- Hver stilling sendes for seg med en `stillingsid` fra lønnssystemet. Lønnssystemet slår ikke
  sammen stillinger med lik aldersgrense og justerer ikke ned til 100 %.
- Ved bytte av lønnssystem sender det nye systemet `stillingsidendringer` med den gamle og den nye ID-en. Da
  beholder stillingen historikken sin hos SPK.

Reglene for `stillingsid`:

1. ID-en er stabil fra stillingen begynner til den slutter, og den er lik i alle innsendinger.
2. Endringer i stillingen, som stillingsprosent, stillingskode eller lønn, meldes på samme ID og gir ikke ny
   stilling.
3. En ID gjenbrukes aldri. En ansatt som slutter og begynner igjen, også i samme jobb, får ny stilling med ny ID.
   En stilling som mangler i en periode eller over et opphold, kan ikke komme tilbake med samme ID.
4. En stilling hører til ett organisasjonsnummer. Arbeid under et annet organisasjonsnummer er en ny stilling med
   ny ID.
5. Samme ID kan bare være med en gang per periode.
6. ID-en endres bare med `stillingsidendringer`. Den nye ID-en kan ikke ha vært brukt for personen før, heller
   ikke for en stilling som er avsluttet.

Brudd på regel 3 avvises med `STILLINGSID_GJENBRUKT` i kvitteringen.
- `gruppe` knytter stillingen til riktig avtale hos arbeidsgivere med flere avtaler. Da slipper SPK å gjette
  på avtalen.
- `pensjonsordning` oppgir TP-nummeret, så et felles mottak kan ta imot for flere ordninger.
- `ansettelsesform` sier om personen er fast eller midlertidig ansatt.

Spørsmål:

1. Hvilket felt i lønnssystemet kan brukes som `stillingsid`? Beholder det samme verdi når stillingsprosent,
   stillingskode eller lønn endres?
2. Kan lønnssystemet følge reglene for `stillingsid` over? Hvis ikke, hvilke regler er vanskelige, og hvorfor?
3. Kan lønnssystemet gi stillingen ny `stillingsid` når personen flytter til et annet organisasjonsnummer?
4. Når en arbeidsgiver bytter lønnssystem, får det nye systemet med seg stillingsidene fra det gamle, så det kan
   sende `stillingsidendringer`?
5. Registrerer dere avtalen (`gruppe`) og TP-nummeret? Hvis ja, ligger de på stillingen eller på arbeidsgiveren?
6. `ansettelsesform` er fast eller midlertidig ansettelse, som feltet ansettelsesform i a-meldingen. Avlønningstype
   er et annet felt. Har dere opplysningen om fast eller midlertidig ansettelse?

**Timer og permisjon**

- Timelønnede og ekstratimer meldes som faktiske timer per stilling og måned. SPK regner ut
  stillingsstørrelsen, og arbeidsgiveren slipper å regne gjennomsnitt.
- Full stilling oppgis som timer per uke og arbeidstidsordning, på samme måte som i a-meldingen. Timene
  gjelder måneden de ble arbeidet. A-meldingen bruker utbetalingsmåneden og sender rettelser som etterbetaling.
- Timer for samme stilling og måned i en senere innsending erstatter de forrige. En rettelse sender derfor hele
  det nye timetallet for måneden. Er 80 timer i mars rettet til 95, sendes 95.
- Overtid og hviletid er ikke med, og 0 timer betyr at personen ikke jobbet. En måned kan ha høyst 300 timer, som
  for timelønn i a-meldingen.
- Delvis permisjon oppgis med hvor stor del av stillingen som er uten lønn. Permisjon med full lønn og permisjon
  under 30 dager rapporteres ikke.

Spørsmål:

1. Kan dere skille timer per stilling når personen har flere stillinger hos samme arbeidsgiver?
2. Kan dere oppgi timene for måneden de ble arbeidet? Hvis ikke, hvilken måned kan dere oppgi dem for?
3. Når timer rettes, har dere det nye timetallet for hele måneden, eller bare timene som kommer i tillegg?
4. Er timer per uke for full stilling og arbeidstidsordning registrert per stilling?
5. Hvordan registrerer dere delvis permisjon i dag? Er det prosenten uten lønn eller prosenten med lønn som
   lagres? Gjelder en fraværsprosent bare permisjon uten lønn?

**Svaret fra SPK**

- SPK svarer med en kvittering per person. En feil hos en person stopper ikke de andre.
- Feilene har faste koder og peker på feltet de gjelder, så lønnssystemet kan vise dem til brukeren.
- Lønnssystemet sender bare en innsending om gangen per arbeidsgiver, og venter på endelig kvittering (`FERDIG`
  eller `AVVIST`) før neste. `MOTTATT` og `BEHANDLES` er foreløpige, og kvitteringen hentes på nytt etter
  `Retry-After`.
- `innsendingsId` gjør det trygt å sende på nytt etter tidsavbrudd. Samme ID med samme innhold gir kvitteringen
  fra første gang. Samme ID med annet innhold avvises med `INNSENDINGSID_GJENBRUKT`. ID-en er brukt når
  innsendingen er tatt imot, også om den avvises i behandlingen. Avvises innsendingen ved mottak, kan samme ID
  brukes igjen.

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

Så lenge versjonen slutter på `-SNAPSHOT`, kan formatet endres uten at det er bakoverkompatibelt. Fra første versjon
uten `-SNAPSHOT` skal en fil som validerte før, fortsatt validere innenfor samme majorversjon. Eksisterende verdier fjernes bare i en ny majorversjon. Feilkodene i kvitteringen er en åpen liste, og avsender må tåle koder den ikke kjenner.

## Henvendelser

Formatet eies og forvaltes av team-kundedata i Statens pensjonskasse.
