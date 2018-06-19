# Generieke Hypermedia API specificatie

We bouwen een specificatie van generieke bouwblokken voor API’s in Vlaanderen. Dit is deel van de [Werkgroep data-standaarden van het Stuurorgaan](https://overheid.vlaanderen.be/stuurorgaan-werkgroepen).

__Eerste draft gepland: juli 2018__

## Discussieer mee

Via de issue tracker kan je online mee discussiëren over wat in de specificatie moet en wat niet: https://github.com/informatievlaanderen/generieke-hypermedia-api/issues

Wil je fysiek mee aanwezig zijn? Er zijn nog publieke workshops gepland in juni:
 * 2018-06-11 van 13:00 tot 14:30 - Herman Teirlinck - 01.71 - Frans Breziers
 * 2018-06-22 van 14:00 tot 15:30 - Herman Teirlinck - 01.23 - Léon Stynen
 
 Inschrijven kan via https://overheid.vlaanderen.be/opleiding/publieke-werkgroepen-oslo-technische-standaard-voor-apis
 
## Scope

Met honderden lokale besturen en administraties die verschillende producten of diensten leveren aan burgers en ondernemingen met behulp van toepassingen van verschillende dienstenleveranciers is er naast semantische interoperabiliteit van data ook nood aan interoperabiliteit van API’s zelf, en de methoden en operaties die gebruikt worden om de data die erdoor ontsloten wordt te bevragen en bewerken.

Een Generieke Hypermedia API beschrijft in elke respons de verdere stappen die vanaf dat punt kunnen worden genomen met behulp van [Hypermedia Controls](https://martinfowler.com/articles/richardsonMaturityModel.html#level3). Een generieke client herkent op zijn beurt bepaalde bouwblokken en kan daar mee verder aan de slag. Op welke bouwblokken we kunnen rekenen binnen Vlaanderen, wordt gespecificeerd in deze werkgroep.

## Bouwblokken

* [Paginering](paginering.md)
* [CRUD operaties](crud-operaties.md)
* [Graph queries](graph-queries.md)
* [Full text search](full-text-search.md)
* [Geo-spatial features](geospatial.md)
* [Authenticatie, signing en authorisatie](authenticatie.md)
* [Taal-selectie/ontdekking](taal.md)
* [Versionering](versionering.md)
* [Metadatering](metadatering.md)
* [Tijdsannotatie](tijdsannotatie.md)
* ...

## Terminologie

| Term        | Omschrijving                                        |
| ----------- |:---------------------------------------------------:|
| API         | Een 'Application Programming Interface' is een set van methoden dat de communicatie tussen verschillende software componenten toelaat |
| Client      | Een persoon of machine die gebruik wil maken van een bepaalde service, aangeboden door een server via een API. Een client neemt het initiatief tot communicatie met de server. |
| Hypermedia  | Hypermedia wordt gebruikt als een medium en restrictie voor API's Binnen de REST applicatie architectuur laat hypermedia toe dat een client en server interageren louter op basis van de hypermedia die dynamisch door de server wordt meegegeven in de respons. |
| Server      | Biedt één of meerdere clients services aan die via een bepaalde communicatieprotocol ter beschikking gesteld worden. Een server beantwoord vragen van clients met een bepaalde respons. |
