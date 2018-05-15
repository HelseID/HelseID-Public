Forespørsel om tilgang til ressurs
===================================

En klient må få konfigurert en eksplisitt tillatelse til å få utstedt et token som kan brukes mot et spesifikt API. Denne tillatelsen uttrykker vi med det vi kaller et scope.
Et tenkt eksempel på et scope i OAuth 2.0 sammenheng kunne ha vært "https://www.minehelsedata.no/patient/read" - som f.eks kunne ha gitt en klient tilgang til å lese pasientdata fra et API.
Når klienten ber om et Access Token må den altså angi ønsket scope i forespørselen, og dersom klientens konfigurasjon i HelseID tilsier at den har tilgang til dette scopet kan HelseID generere og utstede et nytt token. Når APIet så mottar forespørselen må det verifisere at det riktige scopet er angitt i Access Tokenet, og at sikkerhetsbilletten er gyldig. 

Scope
=====
Et scope indikerer hva slags ressurser et token skal få tilgang til. Det være seg et sett med claims eller et tjenestegrensenitt. Fordi et scope kan bety tilgang til et tjenestegrensesnitt, kan et scope formuleres som en autorisasjon. Det er dog viktig å merke seg at dette da blir en autorisasjon på klientnivå, og ikke for en bruker. Et token kan inneholde flere scopes. I HelseID konfigureres et scope som minimum ett underelement av en ressurs, som typisk vil bety grunnleggende tilgang til ressursen. Flere scopes konfigurert for samme ressurs vil bety ulike former for tilgang til samme ressurs.

Tilgang til ett API (Ressurs)
=============================

API-ressurs
^^^^^^^^^^^
En ressurs er noe du ønsker å beskytte med HelseID, enten identitetsdata for dine brukere eller API'er. Enhver ressurs er identitfisert med et unikt navn, og klienter benytter dette navnet når de spesifiserer hvilke resursser de ønkser tilgang til. API-ressurser representerer funksjonalitet som en klient ønsker å påberope seg - typisk modellert som web-APIer, men ikke nødvendigvis. En tilgangstoken tillater tilgang til en API ressurs. Klienter ber om tilgangsfunksjoner og send dem til API-en. Tilgangstokenetter inneholder informasjon om klienten og brukeren (hvis den er til stede). APIer bruker denne informasjonen til å tillate tilgang til dataene sine.


Scope
^^^^^
I det enkleste tilfellet har ett API nøyaktig ett scope. Men det er tilfeller hvor man kanskje vil dele opp funksjonaliteten til ett API, og gi forskjellige klienter tilgang til ulike deler. Dette løses ved å definerer forskjellige scopes for dette API-et.

Audience
^^^^^^^^
Det unike navnet som identifiserer et API. Dette navnet vil bli satt i audience for access tokenet når det inneholder scopes relatert til API-et.

Tilgang til flere API (Ressurser)
=================================
Tenk senarioet hvor en client trenger å be om tilgang til flere ressurser. Dette kan oppnåes på to forskjellige måter. Enten ber klienten om et access token som inneholder alle scopes den trenger, eller så ber den om et access token for hvert scope. Begge handlingene kan være riktig basert på ulike situasjoner. Ber man om et access token som inneholder flere scopes og flere audiences utsetter man seg for risiko ved at en av mottakerne kan benytte seg av tokenet for å impersonere deg mot enn annen audience i samme token. Det er derfor vanlig å snakke om tillitssirkel (circle of trust) i denne sammenheng. Hvis det er gjensidig tillit mellom clienten og ressursene i samme access token er det i utgangspunktet problemfritt å velge denne framgangsmåten. Men hvis det ikke er tillit mellom ressursene gir det heller ingen mening å utsette seg for den nevnte risikoen for impersonering og derfor anbefaler man at man kun ber om scopes innen for èn tillitssirkel for hvert access token.

Alt i ett token (Circle of trust)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Fordeler: enkelt, slipper å eksponsere tilgangsinformasjon til andre ressurser, Bakdeler: risiko for impersonering.

Ett token for hver tilgang
^^^^^^^^^^^^^^^^^^^^^^^^^^
Fordeler: Mindre risiko for misbruk, bakdeler: mer kompleksitet rundt håndtering av flere access tokens, flere kall til HelseID.

Tilgang på veiene av bruker (Token exchange)
============================================
Tenk på følgende scenario: En front-end-klient kaller et API ved hjelp av et token utstedt via en interaktiv flyt (for hybrid flow). Dette API-et (API 1) vil nå kalle et back-end-API (API 2) på vegne av den interaktive brukeren. Med andre ord trenger API-et (API 1) et access token som inneholder brukerens identitet, men med scopet til back-end-APIet (API 2). Du har kanskje hørt om begrepet fattig manns delegasjon der access token fra forsiden bare videresendes til bakenden. Dette har noen mangler, f.eks. API 2 må nå akseptere API 1-scopet som vil tillate brukeren å ringe API 2 direkte. Også - du vil kanskje legge til noen delegeringsspesifikke krav i tokenet, f.eks. det faktum at anropsbanen er via API 1.

Impersonering
^^^^^^^^^^^^^
Når aktør A etterligner aktør B, er A gitt alle rettighetene B har innenfor noen definerte rettighetssammenheng og er skiller seg fra B i den sammenhengen. Når aktør A etterligner aktør B, så er det i den grad noen enhet som mottar et slikt token, faktisk handler med B. Det er sant at enkelte medlemmer av identitetssystemet kan ha bevissthet om at etterligning skjer, men det er ikke et krav. For all hensikt og når A er etterligner B, er A B.

Delegering
^^^^^^^^^^
Delegasjonssemantikk er annerledes enn etterligningssemantikk, selv om de to er nært beslektede. Med delegasjonssemantikk har prinsipper A fortsatt sin egen identitet skilt fra B, og det er eksplisitt forstått at mens B kan ha delegert noen av sine rettigheter til A, blir alle handlinger tatt av A som representerer B. På en måte er A en an agent for B. Delegasjon og etterligning er ikke gjeldende i alle situasjoner. Når en aktør opptrer direkte på egne vegne, er det verken delegasjon eller etterligning som er i spill. De er imidlertid de vanligste semantikkene som gjelder for tokenutveksling.


