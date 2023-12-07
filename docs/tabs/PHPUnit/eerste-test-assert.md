Asserts
=======

## Wat is een test

!!! cite
    __test__ _(de; m; meervoud: tests, testen)_ toesting van de kwaliteit van zaken

Onder de defintie van test staat in het Van Dale woordenboek[^test-definitie] boven staande definitie.
Wanneer onderdelen worden getest dan wordt er gekeken naar bevestiging van beweringen. Beweringen worden
in het Engels __Assertions__ genoemd. Binnen PHPUnit zijn deze __assertions__ de basis van je tests.

## Je eerste test

Binnen de workshop code staat de class `App\Mapper\BarMapper`. In deze mapper zit de functie `map`.

!!! warning
    Je test methoden moeten altijd met `test` beginnen. PHPUnit zoekt naar functies welke beginnen met `test
    en voert deze uit als test.

1. Maak voor deze functie een test aan. PHPUnit kan zowel met camelCase als snake_case methode namen omgaan.
   ![Test genereren in PHPStorm](/assets/screencasts/generate-test.gif)

2. Maak de test aan in de namespace `Tests\Unit\Mapper`
3. Schrijf voor de map functie een test welke kan bevestigen dat met de invoer `bar` de `map` functie een
`foo` waarde terug geeft door een `assert`[^test-assert-same] te gebruiken.
4. Controleer of je test werkt door het uitvoeren van het volgende commando
```shell
php vendor/bin/phpunit --testdox
```
Je zou daarbij een volgend overzicht moeten krijgen (de test regel is afhankelijk van je benaming van de test functie)
![Eerste test resultaat](/assets/screencasts/img/first-test-result.png)

!!! note
    De parameter `--testdox` is geen vereiste voor het draaien van de test, maar zorgt voor een
    leesbare output (het vinkje met de regel welke de test omschrijft). Zonder `--testdox` wordt
    elke testfunctie als een `.` getoond in het resultaat.

🎉 Gelukt! Je hebt je eerste test geschreven!

## Unhappy flow

Een belangrijk punt van het testen is niet alleen controleren wat je weet wat mis gaat, maar ook
controleren of je de situaties afdicht waarin het mis gaat in de code (unhappy flow).

1. Schrijf een test om te controleren dat de waarde `not-bar` geen `foo` terug geeft, maar een `NULL`. waarde
2. Controleer de `NULL` waarde met zijn eigen `assert` check[^test-assert-same]

!!! important
    Maak je tests zo klein en nadrukkelijk mogelijk. Dit maakt tests makkelijk leesbaar en daarmee begrijpelijk.
    Begrijpelijke testen geven een goed inzicht in over complexe code. Gebruik daarom zoveel mogelijk de assert[^test-assert-same]
    opties specifiek voor wat je wilt controleren.

[^test-definitie]: [Van Dale definitie voor "test"](https://www.vandale.nl/gratis-woordenboek/nederlands/betekenis/test#.Ywy-LuxByo0){target="_blank"}
[^test-assert-same]: [Assert checks in PHPUnit](https://phpunit.readthedocs.io/en/9.5/assertions.html){target="_blank"}