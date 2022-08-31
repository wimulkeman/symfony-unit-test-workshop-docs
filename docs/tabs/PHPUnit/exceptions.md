Exceptions
==========
We hebben nu al veel verschillende opties toegepast. Deze opties helpen ons
om de reacties van de stuk code te testen, afhankelijkheden te isoleren en
om stateful objects te kunnen beheren.

Maar wat als we graag zeker willen weten dat de unhappy flow ook
goed activeert? Wat als we willen weten of onze code ook daadwerkelijk
zichzelf afbreekt wanneer we dat beoogd hebben? Oftewel, hoe testen we
een gegooide __Exception__?

PHPUnit heeft een methode[^exception-check] beschikbaar waarmee je kunt testen dat in de
stappen erna een exception zal worden gegooid.

1. Schrijf een test voor `App\Handler\StateHandler`
2. Test dat er een exception wordt gegooid wanneer de handler gelijk de `stopRunning`
methode aanroept

[^exception-check]: [PHPUnit Docs - Assert Exception Is Thrown](https://phpunit.readthedocs.io/en/9.5/writing-tests-for-phpunit.html?highlight=exception#testing-exceptions){target=_blank}