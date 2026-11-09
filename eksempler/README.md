# Eksempler

Hvert eksempel er en katalog med innsendinger som sendes i rekkefølge for samme arbeidsgiver, `1_start.json`,
`2_….json` og så videre. Ved siden av hver innsending ligger kvitteringen SPK svarer med når den er ferdig behandlet,
for eksempel `1_start.kvittering.json`. README i katalogen forklarer situasjonen og hvert steg.

| Eksempel | Viser |
|---|---|
| [01_ny_ansatt](01_ny_ansatt) | Start, lønnsoppgjør, deltid og slutt |
| [02_flere_stillinger](02_flere_stillinger) | To stillinger hos samme arbeidsgiver, den ene med særaldersgrense |
| [03_permisjon](03_permisjon) | Hel og delvis permisjon uten lønn, for en person med D-nummer |
| [04_timeloennet](04_timeloennet) | Timelønnet stilling og timer per måned, også rettede timer |
| [05_opphold](05_opphold) | Slutter og blir ansatt på nytt etter et opphold |
| [06_sluttaarsaker](06_sluttaarsaker) | Alle sluttårsakene i samme innsending |
| [07_tilbake_i_tid](07_tilbake_i_tid) | Lønnsoppgjør som gjelder fra før forrige innsending |
| [08_individuell_avloenning](08_individuell_avloenning) | Stillingsbetegnelse, tillegg og midlertidig stilling med høyere lønn |
| [09_apotek](09_apotek) | Regulativ 105 og apotekenes egne permisjonsavtaler |
| [10_bytte_av_loennssystem](10_bytte_av_loennssystem) | Ny stillingsid når arbeidsgiveren bytter lønnssystem |
| [11_feil_per_person](11_feil_per_person) | Feil hos enkeltpersoner, mens resten går gjennom |
| [12_avvist_innsending](12_avvist_innsending) | Innsending som avvises fordi den ikke følger skjemaet |
| [13_annullering](13_annullering) | Person registrert med feil fødselsnummer, fjernet med tom tidslinje og sendt på det riktige |

Innsendingen i `12_avvist_innsending/1_sluttaarsak_uten_sluttdato.json` er med vilje ugyldig. Alle de andre følger
skjemaet.

## Testdata

Personene og arbeidsgiverne er syntetiske testdata fra [Tenor](https://www.skatteetaten.no/testdata/), Skatteetatens
testdatasøk. Fødselsnumrene har måneden lagt til 80, så de kan aldri tilhøre en ekte person, og kontrollsifrene
stemmer. Alle Tenor-personene og -virksomhetene vi bruker, står i [`testdata/tenor`](../testdata/tenor).

| Fødselsnummer | Navn | Født | Brukes i |
|---|---|---|---|
| 22860299708 | Oppjaget Fiskebolle | 2002 | 01 |
| 24897297651 | Skamfull Ungdom | 1972 | 02, 11 |
| 71888500566 | Nær Journal | 1985 (D-nummer) | 03 |
| 08840897533 | Livstrett Gullmynt | 2008 | 04, 12 |
| 11879399752 | Justerbar Fluktstol | 1993 | 05, 09 |
| 06838799319 | Urokkelig Brudgom | 1987 | 06, 11, 12 |
| 07835797877 | Klartenkt Mandagsbil | 1957 | 06 (alderspensjon) |
| 22827399160 | Enkel Plekter | 1973 | 06, 10 |
| 08818795980 | Rik Nær Gullmynt | 1987 (død i Folkeregisteret) | 06 (død) |
| 22829097072 | Akseptabel Lyselilla Breiflabb | 1990 | 07, 09 |
| 03816098555 | Unormal Stasjon | 1960 | 08 |
| 01818599745 | Tilfeldig Bever | 1985 | 13 (feil fødselsnummer) |
| 01898799898 | Bevisst Robåt | 1987 | 13 |

`22860299709` i eksempel 11 er fødselsnummeret fra eksempel 01 med feil kontrollsiffer.

| Organisasjonsnummer | Navn | Virksomhet | Brukes i |
|---|---|---|---|
| 313683257 | Utgått Autonom Piggsvin | Organisasjonsledd i staten, forsvarsaktiviteter | Alle unntatt 09 |
| 312705230 | Samarbeidsvillig Pratsom Kaiman | Apotek | 09 |

## Slik er kvitteringene laget

Kvitteringene er ikke skrevet for hånd. De er svaret fra SPKs prototype når innsendingene i eksemplet behandles i
rekkefølge, og en test sjekker at de fortsatt stemmer. `mottaksreferanse` og `mottakstidspunkt` er satt fast for
eksemplene. Ellers er kvitteringene slik tjenesten svarer.
