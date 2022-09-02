Testen in Laravel
=================

Vaak is code met alleen PHPUnit goed te testen, maar soms moet je ook Laravel
specifieke onderdelen testen.

Laravel ondersteund het testen van haar eigen onderdelen via verschillende manieren.
Deze manieren zijn onder andere terug te lezen in de documentatie[^laravel-test-documentatie]. 
Laravel ondersteund zowel Unit[^Unit Test] als Feature tests. De Feature tests kunnen daarbij
zowel dienen als Integration Test[^Integration Test] of als E2E Test[^E2E Test]. Van daaruit
kan er dan ook onder de `tests` map een `Unit` en `Feature` map worden gevonden.

Laten we beginnen met het testen via Laravel!

## Het Test command
Draai het volgende commando om de tests via het Laravel test command uit te voeren.

```shell
sh bin/php artisan test
```

Je ziet nu de tests welke je net hebt geschreven, maar dan aangeboden vanuit Laravel.

Laravel biedt ook command's om testen aan te kunnen maken. Deze zullen we in de komende
opdrachten gebruiken om tests te genereren.

[^laravel-test-documentatie]: [Laravel Documentatie - Testing: Getting Started](https://laravel.com/docs/9.x/testing){target=_blank}

[^Unit Test]: [Unit Tests](/tabs/Referenties/e2e-vs-unit-vs-integration-test/#unit-tests)
[^Integration Test]: [Integration Tests](/tabs/Referenties/e2e-vs-unit-vs-integration-test/#integration-tests)
[^E2E Test]: [E2E Tests](/tabs/Referenties/e2e-vs-unit-vs-integration-test/#e2e-tests)