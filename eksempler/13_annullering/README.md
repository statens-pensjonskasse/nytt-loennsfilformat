# Annullering

En lønnsmedarbeider registrerer en ny ansatt med fødselsnummeret til en annen person. Feilen oppdages etter at
innsendingen er behandlet.

- `1_start`: Tilfeldig Bever (01818599745) starter 1. juni 2026 i full stilling, men det er Bevisst Robåt
  (01898799898) som er ansatt.
- `2_rett_foedselsnummer`: Lønnssystemet sender en tom tidslinje for personen med feil fødselsnummer, fra 1. juni 2026,
  den første dagen SPK har for personen. Det fjerner alt SPK har for Tilfeldig Bever hos arbeidsgiveren. I samme
  innsending sendes hele tidslinjen på det riktige fødselsnummeret, og Bevisst Robåt får stillingen med samme
  stillingsid.

Det trengs ingen egen operasjon for å annullere. En tom tidslinje fjerner alt fra og med `gjelderFraOgMed`, på samme måte
som en tidslinje med perioder erstatter alt fra og med den datoen. Hadde personen hatt timer, ville de blitt sendt på
nytt med 0 timer.
