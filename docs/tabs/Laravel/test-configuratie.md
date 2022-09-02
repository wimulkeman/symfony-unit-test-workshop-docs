Testen van configuraties
========================
Het kan voorkomen dat je een onderdeel wil testen welke gebruik maakt van configuratie.
Gezien configuratie kan wijzigen kan het handig zijn om de configuratie te mocken. Een
voordeel van Laravel is dat de configuratie, net als vertalingen, tijdens run-time
in het geheugen wordt opgeslagen.

Doordat de informatie in het geheugen staat kan deze tijdens een test worden aangepast
door gebruik te maken van de Facade classes voor bijvoorbeeld de Configuratie.

1. Maak een test aan voor `App\Providers\SegmentProvider`
2. Overschrijf in de test de configuratie via de `Config` facade
3. Controleer of de functie `getActive` hetzelfde resultaat terug geeft als welke nu is ingesteld in de configuratie 