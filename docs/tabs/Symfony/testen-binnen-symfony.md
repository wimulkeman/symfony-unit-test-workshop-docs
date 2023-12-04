Testen in Symfony
=================

Vaak is code met alleen PHPUnit goed te testen, maar soms moet je ook Symfony
specifieke onderdelen testen.

Symfony ondersteund het testen van haar eigen onderdelen via verschillende manieren.
Deze manieren zijn onder andere terug te lezen in de documentatie[^symfony-test-documentatie]. 
Symfony ondersteund zowel Unit[^Unit Test], Integration als Application tests.

Om in Symfony onderdelen te testen waarbij je de rest van de applicatie nodig bent, kun je uitbreiden
van de `KernelTestCase` class. Deze class zorgt ervoor dat de applicatie wordt opgestart en dat je
toegang hebt tot de container.

[^symfony-test-documentatie]: [Symfony Documentatie - Testing](https://symfony.com/doc/current/testing.html){target=_blank}

[^Unit Test]: [Unit Tests](/tabs/Referenties/e2e-vs-unit-vs-integration-test/#unit-tests)
[^Integration Test]: [Integration Tests](/tabs/Referenties/e2e-vs-unit-vs-integration-test/#integration-tests)
[^E2E Test]: [E2E Tests](/tabs/Referenties/e2e-vs-unit-vs-integration-test/#e2e-tests)