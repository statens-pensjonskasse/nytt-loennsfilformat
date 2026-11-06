# Annullering

En lønnsmedarbeider registrerer en ny ansatt med fødselsnummeret til en annen person. Feilen oppdages etter at
innsendingen er behandlet.

- `1_start`: Tilfeldig Bever (01818599745) starter 1. juni 2026 i full stilling, men det er Bevisst Robåt
  (01898799898) som er ansatt.
- `2_rett_foedselsnummer`: Lønnssystemet annullerer personen med feil fødselsnummer og sender hele tidslinjen på det
  riktige i samme innsending. Tilfeldig Bever har ingenting igjen hos arbeidsgiveren, og stillingsid 1301 kan ikke
  brukes igjen for personen. Bevisst Robåt får stillingen med samme stillingsid.
- `3_annullert_igjen`: Tilfeldig Bever annulleres en gang til. SPK har ingen stillinger fra lønnssystemet for personen,
  så personen avvises med `ANNULLERING_UKJENT_PERSON`.
