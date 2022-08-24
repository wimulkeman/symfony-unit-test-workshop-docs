E2E vs Unit vs Integration Tests
===============================

Bij het testen van een applicatie kom je vaak verschillende termen [^1] tegen:

* Unit Tests
* Integration Tests
* E2E (End-to-End) Tests

Binnen deze workshop hebben wij de focus gelegd op __unit tests__,
maar wat is het verschil tussen deze test vormen?

## Unit Tests
Unit Tests zijn de laagste vorm van tests. Deze tests worden geschreven om een enkele methode geïsoleerd te testen.
Binnen deze tests worden afhankelijkheden van andere onderdelen of verwachtingen van andere onderdelen vaak afgevangen
via [Mocking][Mocking].

Unit Tests kunnen op zowel server code (backend) als op opmaak code (frontend) worden toegepast.

Een voorbeeld van een unit test kan zijn om te controleren of een mapper functie op een gegeven waarde de
juiste waarde terug geeft. Of om te controleren of een save functie daadwerkelijk uiteindelijk data naar
een database probeert te sturen.

### Kenmerken

- Test de werking van 1 methode (class method of algemene functie)
- Afhankelijkheden worden gedicteerd door de test (mogelijk via [Mocking][Mocking])
- [Stubs][Stubs] en [Dummies][Dummies] worden gebruikt als placeholder vulling van objecten.
Bijvoorbeeld voor de return waarde van een mocked object.
- [Fakers][Fakers] worden niet gebruikt omdat de test geïsoleerd moet blijven tot zijn eigen
functie/methode.
- Een test bestand beslaat een class
- Draait snel vergeleken met de andere tests

## Integration Tests
Integration Tests zitten een niveau boven [Unit Tests](#unit-tests). Ze testen een collectie van methoden
welke een onderlinge afhankelijkheid hebben.

Een integratie test kan bijvoorbeeld worden gebruikt om te
controleren of bij het bijwerken van een gebruiker zijn wachtwoord eerst wordt gekeken of de gebruiker
bestaat om vervolgens een record daadwerkelijk in de database te updated.

### Kenmerken

- Test de werking van een enkele functionaliteit
- Afhankelijkheden worden niet door [mocks][Mocking] afgevangen.
- Wordt vaak gebruikt in combinatie met [Fakers](Fakers) om bijvoorbeeld database verbindingen
om database calls te vervangen.
- [Stubs][Stubs] en [Spies][Spies] worden niet gebruikt omdat hiermee implementatie gedrag wordt
gedefinieerd.
- Een test bestand beslaat een groep van aan elkaar verwante functionaliteiten

## E2E Tests
Boven de [Integration Tests](#integration-tests) liggen E2E (End-To-End) Tests. Bij een E2E test worden meer
functionaliteiten gecombineerd vanuit een flow getest.

Een voorbeeld van een E2E test is het controleren van het inloggen van een gebruiker. Daarbij worden tijdens
het testen van de inlog flow op de achtergrond de functionaliteiten getest om een gebruiker te valideren, bij te houden
dat deze nu actief is ingelogd en door te verwijzen naar de juiste pagina. Voor de E2E test is het daarbij alleen van
belang dat de gebruiker zijn gegevens kan opgeven, daarna succesvol is ingelogd en op de juiste pagina is terecht
gekomen. Hoe dit tot stand is gekomen is voor de E2E test niet van belang.

### Kenmerken

- Test een flow binnen de applicatie
- De E2E is erop gericht om op hoog niveau een handeling te controleren (gebruiker kan inloggen en komt op zijn
landingspagina terecht)
- Vereist een geheel draaiende applicatie om de test in uit te kunnen voeren
- Gebruikt mogelijk de [Fakers](Fakers) en de [Spies][Spies] om gedragingen te controleren binnen de applicatie
- Een test bestand beslaat een groep van aan elkaar verwante flows

## Lagen
De verschillende testvormen kunnen goed als aanvulling naast elkaar
worden gebruikt. De verdeling daarvan is goed te visualiseren als een
gelaagde opzet. Hoe dieper je in de lagen komt, hoe geïsoleerder de
tests zijn.

<figure markdown>
  ![Test vormen piramide](https://miro.medium.com/max/996/1*lqWygfNJqWQ4VCyjecQ6Eg.png){ width="100%" }
  <figcaption>Test volgorde voor de verschillende vormen</figcaption>
</figure>

[Mocking]: /tabs/Referenties/terminologie/#mocks "Mocks zijn in test beschreven gedragingen van afhankelijkheden"
[Fakers]: /tabs/Referenties/terminologie/#fakers "Fakers zijn uitvoerbare implementaties welke productie implementaties vervangen"
[Spies]: /tabs/Referenties/terminologie/#spies "Spies zijn Stubs welke extra informatie bijhouden over hoe ze benaderd zijn"
[Stubs]: /tabs/Referenties/terminologie/#stubs 'Stubs zijn "Domme implementaties" welke een productie implementatie vervangen maar altijd dezelfde state terug geven'
[Dummies]: /tabs/Referenties/terminologie/#dummies "Dummies zijn vervangende instanties welke niet een rol spelen in de uitgevoerde (test) code"

[^1]: [Lawrence Tan - Unit Tests, UI Tests, Integration Tests & End-To-End Tests](https://lawrey.medium.com/unit-tests-ui-tests-integration-tests-end-to-end-tests-c0d98e0218a6)