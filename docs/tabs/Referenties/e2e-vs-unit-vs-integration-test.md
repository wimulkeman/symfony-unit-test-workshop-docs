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

Een voorbeeld van een unit test kan zijn om te controleren of een mapper functie op een gegeven waarde de
juiste waarde terug geeft. Of om te controleren of een save functie daadwerkelijk uiteindelijk data naar
een database probeert te sturen.

### Kenmerken

- Test de werking van 1 methode (class method of algemene functie)
- Afhankelijkheden worden gedicteerd door de test (mogelijk via [Mocking][Mocking])
- Draait snel vergeleken met de andere tests

## Integration Tests


### Kenmerken

- Test de werking van een enkele functionaliteit
- Afhankelijkheden worden vaak niet afgevangen om implementaties daadwerkelijk te kunnen testen.

## E2E

### Kenmerken

- Test een flow binnen de applicatie

## Lagen
De verschillende testvormen kunnen goed als aanvulling naast elkaar
worden gebruikt. De verdeling daarvan is goed te visualiseren als een
gelaagde opzet. Hoe dieper je in de lagen komt, hoe geïsoleerder de
tests zijn.

<figure markdown>
  ![Test vormen piramide](https://miro.medium.com/max/996/1*lqWygfNJqWQ4VCyjecQ6Eg.png){ width="100%" }
  <figcaption>Test volgorde voor de verschillende vormen</figcaption>
</figure>

[Mocking]: /tabs/Referenties/terminologie/#mocks "Bekijk hier uitleg over Mocking en Stubbing"

[^1]: [Lawrence Tan - Unit Tests, UI Tests, Integration Tests & End-To-End Tests](https://lawrey.medium.com/unit-tests-ui-tests-integration-tests-end-to-end-tests-c0d98e0218a6)