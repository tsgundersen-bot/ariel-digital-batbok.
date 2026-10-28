# ARIEL – plan for privat innlogging og skylagring

**Status:** Planleggingsfiler. Ingen skytilkobling eller innlogging er aktivert i appen.

## Foreslått arkitektur

- Firebase Authentication med Google-innlogging (Google-konto).
- Cloud Firestore med tilgangskontroll på serveren.
- Hver båt har egen `boats/{boatId}`-post og egne underregistre.
- Medlemskap gis eksplisitt via `boats/{boatId}/members/{uid}`.
- Roller: owner, editor, viewer. Eier styrer tilgang.
- Ingen private dokumenter eller brukertokener i det offentlige GitHub-repositoriet.
- Google Disk brukes foreløpig til manuell, kryptert sikkerhetskopi, **ikke** som automatisk database.

## Før produksjonssetting

1. Opprett Firebase-prosjekt i brukerens Google-konto.
2. Aktiver Google som innloggingsleverandør.
3. Opprett Firestore-database med lukkede regler fra `firestore.rules`.
4. Registrer GitHub Pages-domenet som autorisert domene.
5. Implementer og test innlogging og dataoperasjoner med Firebase SDK.
6. Test med to forskjellige Google-kontoer at en fremmed ikke kan lese, skrive eller invitere seg selv.
7. Importer eksisterende data fra lokal sikkerhetskopi **først etter** eksplisitt bekreftelse.
8. Først etter godkjente tester: aktiver synkronisering i appen.

## Viktig sikkerhetsmodell

GitHub Pages er offentlig. En skjult knapp eller klientbasert passord er ikke tilgangskontroll.
Firestore-regler håndheves på serveren, og ingen tilgang gis uten eksplisitt medlemskap.
Den første eieren må legges inn på en sikker måte fra Firebase-konsollen eller en kontrollert administrativ prosess, aldri ved selvregistrering fra nettleseren.

## Fremtidig datamodell

- `boats/{boatId}`: navn, type, modell, år.
- `boats/{boatId}/members/{uid}`: role.
- `boats/{boatId}/equipment/{id}`: utstyr.
- `boats/{boatId}/faults/{id}`: feilsaker.
- `boats/{boatId}/services/{id}`: servicehistorikk.
- `boats/{boatId}/tasks/{id}`: oppgaver.
- `boats/{boatId}/parts/{id}`: deler.
- `boats/{boatId}/documents/{id}`: dokumentmetadata/lenker.

**Merk:** Regelsettet må gjennomgås og testes i emulator før det brukes med virkelige data.
