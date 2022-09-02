(Native) Functies mocken
========================
In sommige gevallen werkte je code met (native php) functie afhankelijkheid welke niet
via een Dependecy Injection of Service Container wordt aangeleverd.

In zulke gevallen is het mogelijk om een functie ook te mocken, maar hier zitten wel wat haken en ogen aan:

- Je bent de package `php-mock/php-mock-phpunit` nodig
- Vervolgens kun je de trait `PHPMock` gebruiken. Deze maakt de functie `getFunctionMock` beschikbaar
- De functie moet gekoppeld zijn aan de namespace (niet voorzien zijn van een `\`)
- De functie mag niet al eerder door een andere test gebruikt zijn

Wanneer je aan beide punten kunt voldoen, dan kun je de functie testen via onderstaande code.

```php
<?php
$reflection = new ReflectionClass($etlCsvImport);

$fopen = $this->getFunctionMock($reflection->getNamespaceName(), 'fopen');
        $fopen->expects($this->once())
            ->withAnyParameters()
            ->willReturn(false);
```

Voor de functie `getFunctionMock` is een tweetal parameters nodig. De functie welke je wilt mocken,
en de namespace waarin deze is geregistreerd. Dit is normaal gesproken de namespace van de code welke je aan
het testen bent.

In het voorbeeld wordt de namespace via een reflection class opgehaald zodat een verplaatsing van de class
bij een refactoring niet gelijk de test doet breken.