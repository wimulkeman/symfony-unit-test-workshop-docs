Testen van de applicatie
========================
Applicaties welke veel gebruik maken van Dependency Injections werken weinig tot niet met het aanroepen
van de Service Container. Maar wat als je het wel een keer nodig bent?

Voor dergelijke situaties is het mogelijk om de applicatie tot een dieper niveau te beïnvloeden binnen Laravel.

Om een afhankelijkheid van de Service Container af te vangen kan er gewerkt worden met een combinatie van 3
onderdelen:

1. Het gebruik maken van de Laravel `TestCase`
2. Het mocken van de in te laden service
2. Het overschrijven van de service registratie in de container

Een voorbeeld hiervan zou het overschrijven van de MountManager kunnen zijn om te testen of de code deze wel goed
aanroept.

## Stap 1 - Gebruik maken van de Laravel TestCase

```php
<?php

namespace Tests\Unit\Handler;

use App\Handler\WriteFile;
use Illuminate\Foundation\Testing\TestCase;

class WriteFileTest extends TestCase
{

}
```

Van deze stap wordt de Laravel `TestCase` ingeladen. Deze is bij een Laravel project vaak al beschikbaar
vanuit de Abstract Class `Tests\TestCase`, waardoor we de volgende opzet zouden krijgen.

```php
<?php

namespace Tests\Unit\Handler;

use App\Handler\WriteFile;
use Tests\TestCase;

class WriteFileTest extends TestCase
{

}
```

Binnen de `setUp` van de Laravel `TestCase` wordt de applicatie opgestart. Daarmee wordt dus ook de service
container opgestart en zijn de services beschikbaar.

## Stap 2 - Mocken van de service

Hierna kunnen we de definitie van de service waar we van afhankelijk zijn opzetten.

```php
<?php

namespace Tests\Unit\Handler;

use App\Handler\WriteFile;
use Tests\TestCase;

class WriteFileTest extends TestCase
{
    public function test_it_writes_the_file_to_the_storage(): void
    {
        $mountManager = $this->createMock(MountManager::class);
        
        $mountManager->expects($this->once())
            ->method('put')
            ->with(
                'local://foo.txt',
                'bar-content'
            )
            ->willReturn(true);
    }
}
```

## Stap 3 - Overschrijven van de service

Wanneer we vervolgens een service willen overschrijven, dan kan dit vanaf dat moment via de applicatie welke
beschikbaar is via `$this->app`.

```php
<?php

namespace Tests\Unit\Handler;

use App\Handler\WriteFile;
use Tests\TestCase;

class WriteFileTest extends TestCase
{
    public function test_it_writes_the_file_to_the_storage(): void
    {
        $mountManager = $this->createMock(MountManager::class);
        
        $mountManager->expects($this->once())
            ->method('put')
            ->with(
                'local://foo.txt',
                'bar-content'
            )
            ->willReturn(true);
            
        $this->app->instance(MountManager::class, $mountManager);
        
        WriteFile::putContent('foo.txt', 'bar-content');
    }
}
```