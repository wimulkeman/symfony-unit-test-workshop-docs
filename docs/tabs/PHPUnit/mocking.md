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

Een goede indicatie van het moeten gebruiken van een Mock, Stub, Spy of Dummy is wanneer je class een `__construct`
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
    
        $repo = $this->createMock(App\Repository\ProductRepository::class)
            ->expects($this->once)
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
    
        $repo = $this->createMock(App\Repository\ProductRepository::class)
            ->expects($this->once)
            ->with($sku)
            ->willReturn($product);
        
        $provider = new QuantityProvider($repo);
        
        $this->assertSame(1337, $provider->getTotalStockForSku($sku));
    }
}
```

## Meerdere calls

[Mocking]: /tabs/Referenties/terminologie/#mocks "Mocks zijn in test beschreven gedragingen van afhankelijkheden"
[Stubs]: /tabs/Referenties/terminologie/#stubs 'Stubs zijn "Domme implementaties" welke een productie implementatie vervangen maar altijd dezelfde state terug geven'

[^dependency-injection]: [PHP-DI - Undestanding Dependency Injection](https://php-di.org/doc/understanding-di.html){target="_blank"}
[^inversion-of-control]: [Martin Fowler - Inversion of Control Containers and the Dependency Injection pattern](https://www.martinfowler.com/articles/injection.html){target="_blank"}