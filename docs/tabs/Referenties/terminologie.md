Test Terminologie
=================

## Dummies

!!! note
    Dummies worden **buiten** de test implementatie om gedefinieerd. Ze worden dus op zichzelf geïmplementeerd
    zonder dat de instantie weet in welke test deze gebruikt gaat worden.

Objecten welke aangemaakt worden en doorgegeven worden, maar niet binnen de geteste code worden
gebruikt. Een voorbeeld zou een vanuit een interface vereiste argument kunnen zijn waar de functie
niks mee doet.

## Fakes

!!! note
    Fakes worden **buiten** de test implementatie om gedefinieerd. Ze worden dus op zichzelf geïmplementeerd
    zonder dat de instantie weet in welke test deze gebruikt gaat worden.

Objecten welke daadwerkelijk worden gebruikt, maar niet-productie bevatten. Bijvoorbeeld om informatie
weg te schrijven naar een in-memory database in plaats van de daadwerkelijke database.


## Mocks

!!! warning
    Mocks worden **binnen** de test implementatie gedefinieerd. Hierdoor kan dezelfde mock worden
    hergebruikt binnen verschillende tests.

Mocks zijn een laagje om daadwerkelijke implementaties heen. Deze laag bevat de verwachtingen voor de
wijze:

- Waarmee de implementatie wordt aangeroepen
- Hoe vaak de implementatie wordt aangeroepen
- Waarmee de implementatie reageert op de aanroepen

Mocks worden door hun eigenschappen gebruikt om in (unit) tests geïsoleerd te kunnen controleren wat
de reactie van code is op de werking van een implementatie waar het van afhankelijk is.

Mocks hebben net als [Stubs](#stubs) het voordeel dat ermee voorkomen kan worden dat er veranderingen binnen het systeem
daadwerkelijk worden doorgevoerd.

## Stubs

!!! note
    Stubs worden **buiten** de test implementatie om gedefinieerd. Ze worden dus op zichzelf geïmplementeerd
    zonder dat de instantie weet in welke test deze gebruikt gaat worden.

Objecten of resources welke vastgestelde waarden retourneert. Stubs worden vaak geheel afgesplitst van
de productie onderdelen waardoor ze alleen vanuit de tests benaderbaar en nuttig zijn.

Stubs en [Mocks](#mocks) kunnen ingezet worden om te voorkomen dat er daadwerkelijke interactie met de rest van het
systeem plaatsvindt.

## Spies

!!! note
    Spies worden **buiten** de test implementatie om gedefinieerd. Hierdoor kan dezelfde spy worden
    hergebruikt binnen verschillende tests.

Zijn [Stubs](#stubs) met een extra functionaliteit. Ze houden bij hoe ze worden aangeroepen. Een toevoeging
kan bijvoorbeeld zijn dat een service bijhoudt hoe vaak deze is aangeroepen en met welke waarden.

## Test Doubles

Wanneer je code gaat testen welke afhankelijkheden heeft dan probeer je deze afhankelijkheden te isoleren. Door de
isolatie weet je zeker dat je test altijd hetzelfde draait. __Test Doubles__[^1] is een overkoepelende benaming voor de
verschillende beschikbare opties voor het isoleren van afhankelijkheden. Onder Test Doubles vallen [Dummies](#dummies-1),
[Fakes](#fakes), [Mocks](#mocks), [Stubs](#stubs) en [Spies](#spies).

[^1]: [Martin Fowler - Mocks aren't Strubs #The Difference Between Mocks and Stubs](https://martinfowler.com/articles/mocksArentStubs.html#TheDifferenceBetweenMocksAndStubs){target="_blank""}