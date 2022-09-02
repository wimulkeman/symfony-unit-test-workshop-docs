Facade Mocking
==============
Één van de handige onderdelen binnen Laravel is de ondersteuning voor het makkelijk kunnen
mocken van [Facades][^Laravel Facades]. Voorbeelden van Facades in Laravel zijn de Storage, Database (DB)
en Caching onderdelen.

Laravel heeft binnen deze Facades de functie `shouldReceive` beschikbaar gemaakt waarmee je ze kunt mocken
binnen je tests. Laten we deze eens uitproberen. De `shouldReceive` functie krijgt als argument de functie
welke je van de Facade wilt aanroepen (de functie achter de `::`). Een aanroep voor `Storage::disk('local')`
wordt daarmee gemocked via:

```php
<?php
$mock = Storage::shouldRecieve('disk')
            ->with('local')
            ->...
```

De `shouldReceive` retourneert een Mock Expectation[^Mock Expectation]. Omdat er een `Expectation` word
teruggegeven geldt de definitie ook gelijk als een `assertion`. De syntax ervoor is net iets anders dan
je tijdens de [Mock](/tabs/PHPUnit/mocking) opdrachten hebt gebruikt, maar dezelfde opties zijn beschikbaar.

Laten we dit nu in de praktijk gaan brengen.

1. Genereer een test via het commando
```shell
sh bin/php artisan make:test Client/SupportedBanks/Test --unit
```
2. Er is nu een test aangemaakt onder `tests/Unit/Client/SupportedBanks.php`
3. Verwijder de `test_example` functie
4. Schrijf een test om zeker te zijn dat bij een eerste aanroep van de functie `App\Client\SupportedBanks::getList`
de opgehaalde lijst in de `Cache` via de `put` methode wordt weggeschreven
5. Schrijf een test om te controleren dat bij een tweede aanroep de `get` methode wordt gebruikt om de lijst op te halen 

Zoals je ziet zijn Facades makkelijk te vervangen met een mock binnen Laravel, maar wat als je een container
wilt vervangen? Daarover meer in de volgende stap.

[^Laravel Facades]: [Laravel Docs - Facades](https://laravel.com/docs/9.x/facades#main-content){taget="_blank"}
[^Mock Expectation]: [PHPUnit Docs - Epectation Declarations](https://docs.mockery.io/en/latest/reference/expectations.html#){target="_blank"}