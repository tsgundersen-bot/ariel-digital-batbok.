# ARIEL – Digital Båtbok

Mobiltilpasset, statisk båtbok for Ariel (Dominator 34 Flybridge, 1997).

## Status
Versjon 2.6. Servicehistorikk, sjekklister, planlagte oppgaver, feilregister, reservedeler, utstyr og dokumentlenker. Installasjonsmanifest og service worker gir grunnleggende offline-støtte etter første vellykkede besøk.

## Databehandling
Alle brukerregistreringer lagres i nettleserens localStorage på den enkelte enheten. Ingen konto, serverdatabase eller automatisk synkronisering. Eksporter JSON-sikkerhetskopi regelmessig, spesielt før bytte av telefon eller sletting av nettleserdata. Import erstatter lokal registrert informasjon. Nettstedet og kildekoden er offentlig; ikke legg private dokumenter i repositoryet.

## Begrensninger
Servicepunkter og intervaller er ikke verifisert mot Volvo Penta-serienummer og gjeldende manual. Appen har ingen automatiske pushvarsler. Offline-funksjon og installasjonsflyt må testes på faktisk mobil og i publisert GitHub Pages-miljø. Dokumentregisteret lagrer URL-er, ikke filene selv. Utstyrskoblinger bruker foreløpig navn/plassering og bør senere erstattes med stabile ID-er.

## Før produksjonsklar
1. Verifisere delenummer og serviceintervaller mot motor- og drevserienummer.
2. Autentisering og privat datalagring med sikker synkronisering mellom enheter.
3. Import/eksport med skjemavalidering og testdekning.
4. Bilder og dokumentfiler i privat lagring.
5. Testing av alle arbeidsflyter på mobil og PC, samt datamigrering.

Dette er en prototype under utvikling, ikke et komplett vedlikeholdssystem med verifiserte fabrikantdata.
