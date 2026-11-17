# Annullering

En lønnsmedarbeider sender inn feil person. Feilen oppdages etter at innsendingen er behandlet, og det som er sendt
inn for personen, må annulleres.

- `1_start`: Tilfeldig Bever (01818599745) sendes inn med start 1. juni 2026 i full stilling, men personen er ikke
  ansatt hos arbeidsgiveren.
- `2_annullering`: Lønnssystemet sender en tom tidslinje for personen fra 1. juni 2026, den første dagen SPK har for
  personen. Det fjerner alt SPK har for Tilfeldig Bever hos arbeidsgiveren.

Det trengs ingen egen operasjon for å annullere. En tom tidslinje fjerner alt fra og med `gjelderFraOgMed`, på samme måte
som en tidslinje med perioder erstatter alt fra og med den datoen. Hadde personen hatt timer, ville de blitt sendt på
nytt med 0 timer.
