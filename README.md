Unit Test Workshop Docs
=======================

*[Workshop repository](https://bitbucket.org/assuradeurengilde/unit-testing-workshop)*

Deze repository bevat de onderdelen welke uitgevoerd kunnen worden door de mensen
welke de Unit Test Workshop volgen.

De onderdelen zijn daarbij opgedeeld in verschillende categorie�n om de deelnemers
eerst bekend te maken met PHPUnit op zichzelf voordat het wordt getoond in
samenwerking met andere frameworks.

## Workshop documentatie
De pagina's voor de workshop zijn geplaatst onder `docs`. Daarbij is er middels
mappen een onderverdeling gemaakt over het hoofdonderwerp.

## Opmaak
De documentatie is opgemaakt met [MKDocs](https://www.mkdocs.org/) in combinatie
met [Material Theme](https://squidfunk.github.io/mkdocs-material/).

### Configuratie
De configuratie wordt opgenomen in het bestand `mkdocs.yml` in het root van het
project.

### Documentatie bouwen
Om de documentatie te kunnen bouwen moet je de `mkdocs` executable beschikbaar
hebben of de Docker image gebruiken.

Wanneer je de command line executable beschikbaar hebt dan kun je de volgende
opties gebruiken. Beide opties worden uitgevoerd in de root van het project.

**Installatie**

__Via Docker__

Draai het volgende commando in de root van het project om de documentatie
te kunnen bekijken via de browser http://127.0.0.1:8000:

```bash
docker run --rm -it -p 8000:8000 -v ${PWD}:/docs squidfunk/mkdocs-material
```

__Handmatige installatie__

Je bent de volgende onderdelen nodig om gebruik te maken van mkdocs en het]
gebruikte Material thema.
```bash
pip install mkdocs
pip install mkdocs-material
```

**Bouwen van een statische site**
```bash
mkdocs build
```

**Serveren van de huidige configuratie**
*Deze optie ververst zichzelf wanneer een configuratie wordt aangepast*
```bash
mkdocs serve
```
