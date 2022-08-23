Test Terminologie
=================

## Dummies

!!! note
    Dummies worden **buiten** de test implementatie om gedefinïeerd. Ze worden dus op zichzelf geïmplementeerd
    zonder dat de instantie weet in welke test deze gebruikt gaat worden.

Objecten welke aangemaakt worden en doorgegeven worden, maar niet binnen de geteste code worden
gebruikt. Een voorbeeld zou een vanuit een interface vereiste argument kunnen zijn waar de functie
niks mee doet.

## Fakes

!!! note
    Fakes worden **buiten** de test implementatie om gedefinïeerd. Ze worden dus op zichzelf geïmplementeerd
    zonder dat de instantie weet in welke test deze gebruikt gaat worden.

Objecten welke daadwerkelijk worden gebruikt, maar niet-productie bevatten. Bijvoorbeeld om informatie
weg te schrijven naar een in-memory database in plaats van de daadwerkelijke database.

## Stubs

!!! note
    Stubs worden **buiten** de test implementatie om gedefinïeerd. Ze worden dus op zichzelf geïmplementeerd
    zonder dat de instantie weet in welke test deze gebruikt gaat worden.

Objecten of resources welke vastgestelde waarden retourneert. Stubs worden vaak geheel afgesplitst van
de productie onderdelen waardoor ze alleen vanuit de tests benaderbaar en nuttig zijn.

Stubs en Mocks kunnen ingezet worden om te voorkomen dat er daadwerkelijke interactie met de rest van het
systeem plaatsvindt.

## Spies

!!! note
    Spies worden **buiten** de test implementatie om gedefinïeerd. Hierdoor kan dezelfde spy worden
    hergebruikt binnen verschillende tests.

Zijn [Stubs](#stubs) met een extra functionaliteit. Ze houden bij hoe ze worden aangeroepen. Een toevoeging
kan bijvoorbeeld zijn dat een service bijhoudt hoe vaak deze is aangeroepen en met welke waarden.

## Mocks

!!! warning
    Mocks worden **binnen** de test implementatie gedefinïeerd. Hierdoor kan dezelfde mock worden
    hergebruikt binnen verschillende tests.

Mocks zijn een laagje om daadwerkelijke implementaties heen. Deze laag bevat de verwachtingen voor de
wijze:

- Waarmee de implementatie wordt aangeroepen
- Hoe vaak de implementatie wordt aangeroepen
- Waarmee de implementatie reageert op de aanroepen

Mocks worden door hun eigenschappen gebruikt om in (unit) tests geïsoleerd te kunnen controleren wat
de reactie van code is op de werking van een implementatie waar het van afhankelijk is.

Mocks hebben net als Stubs het voordeel dat ermee voorkomen kan worden dat er veranderingen binnen het systeem
daadwerkelijk worden doorgevoerd.