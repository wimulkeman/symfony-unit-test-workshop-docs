Meer Assertions
===============
Nu we onze eerste tests hebben geschreven is het tijd om er wat handigheid in te krijgen.
Naast string return waarden kun je veel verschillende andere soorten return waarden controleren.

In dit onderdeel gaan we verschillende return waarden controleren.

## Testen van booleans
1. Maak een test aan voor de `support` functie van de class `App\Handler\PageHandler`
2. Test dat de aanroep met de waarde `page` een `TRUE` waarde terug geeft
3. Test dat de aanroep met de waarde `header` een `FALSE` waarde terug geeft
4. Schrijf de tests met de specifieke `assert` checks[^test-assert-same]
5. Controleer of je test werkt met het commando
```shell
sh bin/php vendor/bin/phpunit --testdox
```

## Testen van objects en arrays
1. Maak een test aan voor de `handle` functie van de class  `App\Handler\PageHandler`
2. Test dat je een `Illuminate\Support\Collection` terug krijgt uit de functie
3. Test dat je kunt tellen hoeveel items er in collectie zitten door te controleren op de interface `\Countable`
4. Test dat er in de collection 4 items zitten
5. Test dat er een item gezocht kan worden in de response van de functie door te controleren of de `Iterable` is 
6. Test middels een array assert[^test-assert-same] dat de waarde foo voorkomt in de teruggegeven collectie

!!! important
    We hebben net een test geschreven voor het controleren of de return waarde een Collection is. Dit is
    overbodig omdat de return waarde een Collection is. Dit zouden we normaal gesproken afdwingen middels
    een interface (code abstraction). In het geval van een dergelijke return waarde is het testen erop vaak
    overbodig. __Voorkom dat je overbodige tests schrijft welke al door return types en typehints worden
    afgevangen.__

[^test-assert-same]: [Assert checks in PHPUnit](https://phpunit.readthedocs.io/en/9.5/assertions.html){target="_blank"}