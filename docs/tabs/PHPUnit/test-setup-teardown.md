setUp / tearDown
================
We weten nu hoe we de return waarden van functies kunnen testen,
en hoe we afhankelijkheden kunnen beheersen vanuit onze test. Maar wat
als veel tests nu hetzelfde gebruiken? Moeten we dan elke keer dezelfde code
schrijven? Dat schendt toch het DRY[^dry-principle] principe?

Dat klopt, en gelukkig is daarvoor wat beschikbaar binnen PHPUnit.

## setUp

PHPUnit biedt een tweetal functies aan om onderdelen klaar te zetten voor
een test:

- __setUp__: Deze functie wordt voor elke test methode aangeroepen
- __setUpBeforeClass__: Deze functie wordt eenmalig aangeroepen voor de gehele test class 
voordat er een test methode wordt aangeroepen.

Op eenzelfde manier zijn er ook functies beschikbaar voor het herstellen of verwijderen van
variabelen aan het einde van een test of test class.

- __tearDown__: Deze functie wordt na elke test methode aangeroepen
- __tearDownAfterClass__: Deze functie wordt aangeroepen nadat alle test methodes in een class
zijn behandeld

!!! important
    PHPUnit doet veel van zijn opschoonwerkzaamheden voor en na het uitvoeren van een enkele test
    methode. Het is daarom belangrijk om niet alle tests in 1 test methode te stoppen. Als je dit
    wel doet dan zal een status van een object worden onthouden en de onderlinge tests binnen
    dezelfde methode elkaar beïnvloeden.

In de praktijk kun je dit gebruiken voor het klaarzetten van algemene gegevens of het herstellen van
een staat in services die je wilt testen (bijvoorbeeld omdat deze statische onderdelen heeft).

1. Draai de tests. De test `Tests\Unit\Handler\StatefullHandlerTest` gaat mis omdat het te testen object een status vasthoudt
2. Voeg in de test een `setUp` functie toe welke ervoor zorgt dat de status in het object gereset wordt
3. Draai de test opnieuw zodat deze nu slaagt

[^dry-principle]: [Wikipedia - Don't Repeat Yourself](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself){target="_blank"}
[^setup-teardown]: [PHPUnit - Fixtures > setUp / tearDown](https://phpunit.readthedocs.io/en/9.5/fixtures.html#more-setup-than-teardown){target="_blank"}