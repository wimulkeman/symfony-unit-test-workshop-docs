Mocking
=======
Een belangrijk aspect van unit tests is dat ze geïsoleerd draaien. Dat betekend dat het soms nodig is om code
afhankelijkheden te controleren binnen je test. Hierbij komt de term [mocking][Mocking] om de hoek kijken.

Mocks kun je in verschillende benamingen tegen komen waarbij de implementatie per stuk verschilt. Deze benamingen zijn
opgenomen in de [Terminologie](/tabs/Referenties/terminologie) onder het Referenties tabblad.

In dit onderdeel maken we gebruik van __Mocks__.

## Wat is een mock

Mocks zijn afhankelijk van je code waarbij je zelf de afhankelijkheid test binnen je test code. Mocks worden vaak
samen met __Dependency Injection__[^dependency-injection][^inversion-of-control] ingezet.

Een goede indicatie van het moeten gebruiken van een [Mock][Mocking], [Stub][Stubs], [Spy][Spies] of [Dummy][Dummies] is wanneer je class een `__construct`
heeft waarin andere objecten worden doorgegeven.

In het geval van de volgende code hebben we repository welke wordt doorgegeven aan onze class.

```php
<?php

namespace App\Provider;

use App\Exception\Model\Shop\ItemDoesNotSupportStockingException;
use App\Interface\Model\Shop\Stockable;
use App\Model\Shop\Product;
use App\Repository\ProductRepository;

class QuantityProvider
{
    private ProductRepository $productRepository;
    
    public function __construct(ProductRepository $productRepository)
    {
        $this->productRepository = $productRepository;
    }
    
    public function getTotalStockForSku(string $sku): int
    {
        $product = $this->productRepository->findBySku($sku);
        
        if (!$product instanceof Stockable) {
            throw new ItemDoesNotSupportStockingException($sku);
        }
        
        return $product->getLocalStock() + $product->getRemoteStock();
    }
}
```

Hierin hebben we twee afhankelijkheden:

1. We hebben de product repository
2. En we hebben de product model welke wordt opgehaald via de repository

We zouden voor deze onderdelen bestaande onderdelen uit een applicatie kunnen pakken, maar dan zouden we ook een
werkende database tot onze beschikking moeten hebben. Voor een unit test is dit niet vaak wenselijk omdat we dan
een afhankelijkheid hebben die buiten de test en de te testen code om kan wijzigen.

Om dit tegen te gaan kunnen we dit onderdeel gaan mocken.

```php
<?php

namespace Test\Unit\Provider;

use App\Provider\QuantityProvider;
use PHPUnit\Framework\TestCase;
use Test\Stub\Model\Product;

class QuantityProviderTest extends TestCase
{
    public function test_it_uses_the_local_and_remote_stock_to_calculate_the_total_stock()
    {
        $repo = $this->createMock(App\Repository\ProductRepository::class);
    }
}
```

We hebben nu een kale mock van de repository. Deze kunnen we nu gebruiken om aan de constructor vereisten te voldoen.

```php
<?php

namespace Test\Unit\Provider;

use App\Provider\QuantityProvider;
use PHPUnit\Framework\TestCase;
use Test\Stub\Model\Product;

class QuantityProviderTest extends TestCase
{
    public function test_it_uses_the_local_and_remote_stock_to_calculate_the_total_stock()
    {
        $repo = $this->createMock(App\Repository\ProductRepository::class);
        
        $provider = new QuantityProvider($repo);
    }
}
```

Door de mock uit te breiden kunnen we aangeven dat we verwachten dat er 1x een call word gedaan op de `findBySku`
methode met de opgegeven `$sku` waarde.

Door het gebruik van de `createMock` functie krijgen we een `MockObject` terug voor het object. Het `MockObject`
heeft een aantal functies beschikbaar[^mock-object-phpunit][^mock-object-mockery] welke we kunnen gebruiken om de
toepassing van de afhankelijkheid te testen.

```php
<?php

namespace Test\Unit\Provider;

use App\Provider\QuantityProvider;
use PHPUnit\Framework\TestCase;
use Test\Stub\Model\Product;

class QuantityProviderTest extends TestCase
{
    public function test_it_uses_the_local_and_remote_stock_to_calculate_the_total_stock()
    {
        $sku = 'test-123';
        $product = new Product();
        $product->setLocalStock(5);
        $product->setRemoteStock(1332);
    
        $repo = $this->createMock(App\Repository\ProductRepository::class);
        $repo->expects($this->once)
            ->method('findBySku')
            ->with($sku)
            ->willReturn($product);
        
        $provider = new QuantityProvider($repo);
    }
}
```

!!! note
    De `expects` toevoeging in de mock maakt van de Mock ook gelijk een `assertion`. Het controleert of de mock
    ook daadwerkelijk wordt aangeroepen. Op deze manier kun je controleren of de code ook gebruik maakt van zijn
    afhankelijkheden, en met de verwachte waarden.

!!! note
    Je ziet dat er als return waarde voor de aanvraag op de repository een Stub[^Stubs] wordt teruggegeven. Dit wordt
    gedaan zodat we niet alles hoeven te mocken, maar wel controle hebben over de waarden

Nu we een mock hebben met een return waarde kunnen we controleren of de flow in de code klopt.

```php
<?php

namespace Test\Unit\Provider;

use App\Provider\QuantityProvider;
use PHPUnit\Framework\TestCase;
use Test\Stub\Model\Product;

class QuantityProviderTest extends TestCase
{
    public function test_it_uses_the_local_and_remote_stock_to_calculate_the_total_stock()
    {
        $sku = 'test-123';
        $product = new Product();
        $product->setLocalStock(5);
        $product->setRemoteStock(1332);
    
        $repo = $this->createMock(App\Repository\ProductRepository::class);
        $repo->expects($this->once)
            ->method('findBySku')
            ->with($sku)
            ->willReturn($product);
        
        $provider = new QuantityProvider($repo);
        
        $this->assertSame(1337, $provider->getTotalStockForSku($sku));
    }
}
```

## Eerste Mock test

Nu wordt het tijd om de theorie in de praktijk te gaan brengen!

1. Maak een test aan voor de methode `App\Providers\UserProvider::getUserName`
2. Maak in de test een Mock[^mock-object-phpunit] aan voor de dependency UserRepository
3. Controleer via de Mock dat de findById methode 1 keer wordt aangeroepen
4. Schrijf een test waarbij er een User stub wordt terug gegeven vanuit de repository
5. Schrijf een test waarbij er geen User stub (NULL) wordt terug gegeven vanuit de repository
6. Kijk wat er gebeurd als je in een test 2 keer de `getUserName` methode uitvoert. Dit zou vanuit de `expect` van de
Mock een gefaalde testuitslag moeten geven 

Gelukt! Je hebt nu 2 belangrijke onderdelen in PHPUnit getackeld. Je weet hoe je waarden kunt controleren via
__asserst__ en je kunt nu je afhankelijkheden controleren via __[test doubles][Test-doubles]__ zoals __[mocks][Mocking]__.

## Meerdere calls

Maar... wat nu als je meerdere calls doet naar dezelfde afhankelijkheid? Er zijn situaties
waarin je meerdere calls doet naar een afhankelijkheid, maar de reactie per call verschilt.

Je kunt in dergelijke situaties gebruik maken van de `consecutive` functies[^mock-consecutive] bij de mock.

Probeer dit uit in de praktijk.

1. Schrijf een test voor de functie `App\Providers\UserProvider::getUserNames`
2. Maak in de test een Mock[^mock-object-phpunit] aan voor de dependency UserRepository
3. Controleer via de Mock dat de findById methode 3 keer wordt aangeroepen
4. Richt de mock in om meerdere calls te ontvangen
5. Gebruik voor de functie `findById` als invoerargumenten `123`, `456` en `789`
6. Geef als return waarden voor `findById` een `User model`, een `NULL` en een `User model` terug

Gelukt? Goed bezig. Je begint nu al een behoorlijke PHPUnit professional te worden!

[Mocking]: /tabs/Referenties/terminologie/#mocks "Mocks zijn in test beschreven gedragingen van afhankelijkheden"
[Fakers]: /tabs/Referenties/terminologie/#fakers "Fakers zijn uitvoerbare implementaties welke productie implementaties vervangen"
[Spies]: /tabs/Referenties/terminologie/#spies "Spies zijn Stubs welke extra informatie bijhouden over hoe ze benaderd zijn"
[Stubs]: /tabs/Referenties/terminologie/#stubs 'Stubs zijn "Domme implementaties" welke een productie implementatie vervangen maar altijd dezelfde state terug geven'
[Dummies]: /tabs/Referenties/terminologie/#dummies "Dummies zijn vervangende instanties welke niet een rol spelen in de uitgevoerde (test) code"
[Test-doubles]: /tabs/Referenties/terminologie/#test-doubles "Test doubles is een overkoepelende term voor het vervangen van code afhankelijkheden met controleerbare onderdelen"

[^dependency-injection]: [PHP-DI - Undestanding Dependency Injection](https://php-di.org/doc/understanding-di.html){target="_blank"}
[^inversion-of-control]: [Martin Fowler - Inversion of Control Containers and the Dependency Injection pattern](https://www.martinfowler.com/articles/injection.html){target="_blank"}
[^mock-object-phpunit]: [PHPUnit Docs - Mock Objects](https://phpunit.readthedocs.io/en/9.5/test-doubles.html?highlight=mock#mock-objects){target="_blank"}
[^mock-object-mockery]: [Mockery Docs - Getting Started](https://docs.mockery.io/en/latest/getting_started/index.html){target="_blank"}
[^mock-consecutive]: [PHPUnit Docs - Mocking Consecutive Calls](https://phpunit.readthedocs.io/en/9.5/test-doubles.html?highlight=mock#test-doubles-stubs-examples-stubtest7-php){target="_blank"}