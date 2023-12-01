Installatie
===========

## Basis
PHPUnit is beschikbaar als een vendor via composer [^composer].
Daarbij zijn er voor PHPUnit verschillende onderdelen welke beschikbaar zijn.
Er is een basis package `phpunit/phpunit` welke we altijd nodig hebben.

Deze kan worden toegevoegd met het commando

``` shell
composer require --dev "phpunit/phpunit" ^9
```

## Frameworks
De standaard package is goed te gebruiken voor het testen van (op zichzelf staande)
code, maar het mist ondersteuning voor het makkelijk kunnen testen van onderdelen van
frameworks zoals Symfony, of services die met elkaar interacteren.

Wanneer je test binnen Symfony is het daarom aan te raden om het volgende op te nemen in
je composer middels require-dev:

``` shell
composer require --dev "symfony/test-pack"
```

Dit installeert PHPUnit en de benodigde onderdelen om binnen Symfony te kunnen testen.
Binnen de workshop code maken we gebruik van [symfony/test-pack](https://symfony.com/doc/current/testing.html){target="_blank""}.

## Workshop code
Binnen de workshop code is de require voor PHPUnit al opgenomen.

1. Begin daarom met het binnenhalen van het project vanuit de
[repository](https://github.com/wimulkeman/symfony-unit-test-workshop){target="_blank""}.
2. Maak na het binnenhalen van de repository een branch aan voor je eigen wijzigingen.
3. Draai het commando om de Docker container op te starten
```shell
docker-compose up -d
```
4. Draai het commando om de benodigde vendors op te halen
```shell
sh bin/composer install
```
5. Als je een versienummer van de PHPUnit package op kunt halen dan staat het project klaar
```shell
sh bin/php vendor/bin/phpunit --version
```

!!! note
    Als je gedurende de workshop je oplossing wilt controleren, dan kun je binnen
    de workshop repository switchen naar de `solutions` branch.

Je bent nu klaar voor je eerste test!

[^composer]: [Composer PHPUnit package](https://packagist.org/packages/phpunit/phpunit){target="_blank""}