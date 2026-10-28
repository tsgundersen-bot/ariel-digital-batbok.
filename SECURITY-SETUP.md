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

## Oppsett i Firebase-konsollen (utføres av eier)

1. Gå til Firebase-konsollen og opprett et prosjekt, for eksempel `ariel-batbok`.
2. Registrer en webapp i prosjektet. Ta vare på webappens **offentlige** konfigurasjonsverdier (`apiKey`, `authDomain`, `projectId`, `appId`). Firebase web-`apiKey` er en prosjektidentifikator, ikke en administratornøkkel.
3. I Authentication → Sign-in method: aktiver Google.
4. I Authentication → Settings → Authorized domains: legg til `tsgundersen-bot.github.io`.
5. Opprett Firestore i valgt europeisk region. Start med lukkede regler, og gjennomgå `firestore.rules` før publisering.
6. Opprett den første båten og eiermedlemskapet fra Firebase-konsollen etter at eierens Google-bruker-ID er kjent. Ikke tillat selvoppretting av eier via klienten.
7. Test med to ulike Google-kontoer og Firestore-emulator før eksisterende lokale data migreres.

**Ikke del:** service account JSON, private nøkler, tilgangstokener eller passord. Ikke endre deling på Google Disk-dokumenter for å få appen til å fungere.
