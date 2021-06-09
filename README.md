[![Build Status](https://github.com/navikt/syfotjenester/workflows/Release%20(Github%20Package%20Registry)/badge.svg)](https://github.com/navikt/syfotjenester/actions?query=workflow%3A%22Release+%28Github+Package+Registry%29%22)

# Syfotjenester

Dette prosjektet inneholder spesifikasjoner for de tjenestene som SYFO tilbyr internt.
De er maskinlesbare i form av WSDL/XSD/JSON-filer, og disse brukes som utgangspunkt for å
generere Javakode. Denne autogenererte koden blir kompilert og siden publisert, slik at konsumenter
kan bruke dem til å kommunisere med tjenestene.

## Bygging

Java 8 trenges pga `javax.xml.bind.*` bygg virker ikke lenger med Java versjon >= 9.
https://stackoverflow.com/questions/43574426/how-to-resolve-java-lang-noclassdeffounderror-javax-xml-bind-jaxbexception-in-j

`mvn clean install`

## Oppdatering av WSDL fra NHN

WSDLer fra NHN må først konverteres til noe som kan brukes av NAV. Disse NAV-
versjonene av WSDLene hentes fra pakker generert av Team Mellomvare. WSDLene
må hentes ut fra maven-pakker og legges inn manuelt her i syfotjenester.
Pakkene ligger på intern NAV Nexus her: http://maven.adeo.no/

* no.nav.tjenester:ekstern-helse-fastlegeinformasjon-tjenestespesifikasjon
* no.nav.tjenester:ekstern-helse-adresseregisteret-tjenestespesifikasjon

For å generere disse nye interne NAV-WSDLene ta kontakt med Team Mellomvare.
Nye WSDLer fra NHN ligger her:

* https://register-at.test.nhn.no/docs/arkitektur/register/flr.html
* https://register-at.test.nhn.no/docs/arkitektur/register/ar.html

NHN har fire hovedleveranser i året, WSDLene oppdateres med endringer relevante
for NAV på omtrent en av disse fire, altså en gang i året. Følg med
på #ext-nav-grunndata og
https://www.nhn.no/samhandlingsplattform/grunndata/leveranseplaner-for-grunndata
for å få oppdateringer på når  endringene er i testmiljø og når de skal
prodsettes.

## Gjøre endringer, release

For å endre spesifikasjoner, lag en branch. Kjør bygget lokalt, da vil du
få siste endringer med `0-SNAPSHOT` som versjon. Test med en konsument at
endringene fungerer (sett versjon av tjenestespesifikasjoner til `0-SNAPSHOT` i konsumenten.)
Når testingen er ferdig, send en pull request til dette repoet.

Hver branch og pull request vil gå gjennom et CI-bygg.
Etter at en pull request er merget til master-branchen, vil
CI automatisk gjøre en release av hele repoet til Maven Central.
Alle modulene i dette repoet får samme versjonsnummer.
Versjonsnummeret til releasen blir `1.YYYY.MM.DD-HH-MM-commithash`.