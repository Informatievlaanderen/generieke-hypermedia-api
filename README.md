# Generieke Hypermedia API specificatie

We bouwen een specificatie van generieke bouwblokken voor API’s in Vlaanderen. Dit is deel van de [Werkgroep data-standaarden van het Stuurorgaan](https://overheid.vlaanderen.be/stuurorgaan-werkgroepen).

__Eerste draft gepland: juli 2018__

## Discussieer mee

Via de issue tracker kan je online mee discussiëren over wat in de specificatie moet en wat niet: https://github.com/informatievlaanderen/generieke-hypermedia-api/issues

Wil je fysiek mee aanwezig zijn? Er is nog één publieke workshop gepland in juni:
 * 2018-06-22 van 14:00 tot 15:30 - Herman Teirlinck - 01.23 - Léon Stynen
 
 Inschrijven kan via https://overheid.vlaanderen.be/opleiding/publieke-werkgroepen-oslo-technische-standaard-voor-apis
 
## Scope

Met honderden lokale besturen en administraties die verschillende producten of diensten leveren aan burgers en ondernemingen met behulp van toepassingen van verschillende dienstenleveranciers is er naast semantische interoperabiliteit van data ook nood aan interoperabiliteit van API’s zelf, en de methoden en operaties die gebruikt worden om de data die erdoor ontsloten wordt te bevragen en bewerken.

Een Generieke Hypermedia API beschrijft in elke respons de verdere stappen die vanaf dat punt kunnen worden genomen met behulp van [Hypermedia Controls](https://martinfowler.com/articles/richardsonMaturityModel.html#level3). Een client kan vervolgens een generieke afhandeling voorzien voor elk van deze bouwblokken. Op deze manier kan een strikte koppeling tussen client en server vermeden worden.

Op welke bouwblokken we kunnen rekenen binnen Vlaanderen, wordt gespecificeerd in deze specificatie.

## Toepassingsgebied

Deze specificatie is van toepassing op alle API's die een RESTful architectuur gebruiken. De afspraken per bouwblok zijn applicatie-neutraal. In een applicatie-context is het noodzakelijk om uit de verschillende bouwblokken en de ondersteunde standaarden per bouwblok een coherente keuze te maken.

## Nakomingsniveaus

Momenteel zijn twee nakomingsniveaus voorzien voor deze standaard: HTTP en Semantisch. Het HTTP nakomingsniveau veronderstelt dat alle Hypermedia Controls meegegeven worden als onderdeel van de HTTP request en response headers. Dit laat bestaande API's toe de bouwblokken uit deze specificatie te ondersteunen zonder wijzigingen door te voeren in de bestaande API payloads.

Voor alle nieuw te ontwikkelen API's wordt sterk aangeraden om het semantisch nakomingsniveau te repecteren. Dit nakomingsniveau verwacht dat alle hypermedia controls semantisch beschreven worden als onderdeel van de API payload, gebruik makend van het Resource Description Framework. Bijgevolg _MOET_ elke API die ervoor kiest het semantisch nakomingsniveau te implementeren één of meerdere Linked Data serialisaties ondersteunen. De keuze voor een bepaalde serialisatie is afhankelijk van de concrete toepassing. Onderstaande tabel geeft een prioriteiten-lijst mee voor de implementatie van content-negotiation:

q | serialisatie  | Linked Data? | Quads 
------------ | -------------|--------------| -----
1 | application/ld+json | Ja | Ja 
1 | application/trig | Ja | Ja
1 | application/n-quads | Ja | Ja
0.9 | text/html | Kan (RDFa of JSON-LD snippets) | kan (JSON-LD snippets) 
0.7 | application/n-triples | Ja | Nee
0.7 | text/turtle  | Ja | Nee 
0.7 | application/rdf+xml | Ja (weinig gebruikt) | Nee
0.6 | non-linked data specificatie (json, xml, csv...) | Nee | Nee


## Bouwblokken

* [Paginering](paginering.md)
* [CRUD operaties](crud-operaties.md)
* [Graph queries](graph-queries.md)
* [Full text search](full-text-search.md)
* [Geo-spatial features](geospatial.md)
* [Taal-selectie/ontdekking](taal.md)
* [Versionering](versionering.md)
* [Metadatering](metadatering.md)
* [Tijdsannotatie](tijdsannotatie.md)

## Terminologie

| Term        | Omschrijving                                        |
| ----------- |:---------------------------------------------------:|
| API         | Een 'Application Programming Interface' is een set van methoden dat de communicatie tussen verschillende software componenten toelaat |
| Client      | Een persoon of machine die gebruik wil maken van een bepaalde service, aangeboden door een server via een API. Een client neemt het initiatief tot communicatie met de server. |
| Hypermedia  | Hypermedia wordt gebruikt als een medium en restrictie voor API's Binnen de REST applicatie architectuur laat hypermedia toe dat een client en server interageren louter op basis van de hypermedia die dynamisch door de server wordt meegegeven in de respons. |
| Linked Data | Een methode voor het publiceren van gestructureerde manier op een bevraagbare manier via onder meer inferentie en gebaseerd op vocabularia. Linked Data verwijst typisch naar een collectie van Semantic Web technologiën waaronder RDF, OWL, SKOS, SPARQL... |
| RDF         | Het Resource Description Framework is een canonisch model voor data-uitwisseling op het Web, gepubliceerd als standaard door het World Wide Web Consortium (W3C). |
| Server      | Biedt één of meerdere clients services aan die via een bepaalde communicatieprotocol ter beschikking gesteld worden. Een server beantwoord vragen van clients met een bepaalde respons. |
