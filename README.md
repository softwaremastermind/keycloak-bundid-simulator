# eid-bundid-idpmock
hde, 12/23

## Mission

Für die Durchführung von Authentifizierungen via BundID in den Dev-Umgebungen kann die Integrationsumgebung
von *BundID* nur eingeschränkt verwendet werden. Als Ersatz wird in den DEV-Umgebungen (ausser *main*-Instanz) 
der `eid-bundid-idpmock` konfiguriert.

## Funktion

Nach dem Start der Authentifizierung leitet der *Keycloak* via *SAML* an den `eid-bundid-idpmock`. Diese 
Anwendung zeigt ein Formular (ähnlich Mock in `Eid-Support`) zur Erfassung der Benutzerdaten an. Nach dem 
Formpost wird an den Keycloak zurückgeleitet. Dort werden die Userdaten verarbeitet.

## Lokale Entwicklung

Aufruf der lokalen Anwendung:

    http://localhost:8443/saml

## Architektur

Der **BundID-Simulator** ist eine Java-Anwendung auf Basis des Frameworks *Springboot*. Für die Web-UI basiert auf dem  
CSS-Framework *Bootstrap*. In *Springboot* werden die HTML-Seiten mit *Thymeleaf* gerendert (siehe 
`src/main/resources/views`)..  

## Funktionsweise

Die Webanwendung wird per *POST* über den Endpunkt `/saml` aufgerufen. Als Parmeter im Post-Request sind u.a. das 
angeforderte Vertrauensniveau und die Rücksprung-Url (*Assertion Consumer Service Url*) enthalten. Der **BundID-Simulator** 
verhält sich fachlich ähnlich der externen BundID-Anwendung. 

Nach Bestätigung der Dateneingabe wird ein *Saml-Response* nach BundID-Spezifikation erstellt (siehe 
`saml_response_template.xml`). Zur Simulation von Fehlern oder Abbrüchen wird das Template `saml_response_error_template.xml`
verwendet. Die Weiterleitung als POST_Request an den Aufrufer erfolgt über `auth_response.html`. 

### Datenbasis

Alle Attribute für den Response sind in `src/main/resources/field_definitions.yml` definiert. Die Liste mit Personen, 
die in der Web-UI dargestellt wird, ist in der Datei `src/main/resources/users.xml` abgelegt. Jeder Datensatz hat folgende 
Form:

    id: "U01"
    surname: "Neumann"
    givenname: "Maria"
    mail: "maria.neumenn@ba-online.de"
    postalAddress: "Thomas-Mann-Straße 3"
    postalCode: "10409"
    localityName: "Berlin"
    country: "DE"
    personalTitle: ""
    gender: "2"
    birthdate: "1964-08-12"
    placeOfBirth: "Berlin"
    birthName: "Winter"
    bpk2: "BUNDIDSIM-U01"

Personendaten können in der Definitionsdatei geändert, gelöscht und ergänzt werden.

    Endpunkte: 
    http://localhost:8080/livez
    http://localhost:8080/readyz

    select_view SelectViewController
    edit_view EditViewController
