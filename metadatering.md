Discussie: [#9](https://github.com/pietercolpaert/generieke-hypermedia-api/issues/9)

# Metadatering van API's

Het metadateren van API's biedt waardevolle technische en organisatorisch informatie aan ontwikkelaars die met een bestaande API aan de slag willen gaan. Daarnaast zorgt gestructureerde metadata ervoor dat informatie automatisch kan worden opgehaald en getoond door bijvoorbeeld een web service catalogus. 

Deze specificatie beschrijft welke metadata kan opgenomen worden en welke metadatastandaarden gevolgd dienen te worden. Volgende types van metadata worden onderscheiden:
* API identificatie (versiedatum, titel...)
* Contactgegevens m.b.t. de API
* Temporele begrenzing
* Geografische begrenzing
* Beschikbare resources

## Nakomingsniveaus

### HTTP

Een server _MOET_ ondersteuning bieden voor volgende methodes:

+ [RFC 5988 - Web Linking](https://tools.ietf.org/html/rfc5988), door gebruik te maken van de `describedby` en/of `help` link headers als response op een GET request op niveau van het entry point van de API.
+ [RFC 7231, section 4.3.7: OPTIONS](https://tools.ietf.org/html/rfc7231#section-4.3.7), specifiek voor metadata met betrekking tot de communicatie opties tussen de client en de server voor een bepaalde resource.

### Semantisch

Een server die ondersteuning wil bieden voor Linked Data _MOET_ de metadata van de API in-band beschikbaar maken door de implementatie van onderstaande metadata specificaties:

* API identificatie: [DCMI Metadata Terms](http://dublincore.org/documents/dcmi-terms/) en [Hydra Core Vocabulary](http://www.hydra-cg.com/spec/latest/core/)
* Contactgegevens m.b.t. de API: [OSLO implementatie van schema:ContactPoint](https://data.vlaanderen.be/doc/applicatieprofiel/generiek#Contactpunt)
* Temporele begrenzing: [DCMI Metadata Terms](http://dublincore.org/documents/dcmi-terms/), [dct:temporal](	http://purl.org/dc/terms/temporal)
* Geografische begrenzing: [DCMI Metadata Terms](http://dublincore.org/documents/dcmi-terms/), [dct:spatial](http://purl.org/dc/terms/spatial)
* Beschikbare resources: [Hydra Core Vocabulary](http://www.hydra-cg.com/spec/latest/core/)

## Code voorbeelden

__TODO__

## Algoritme voor herkennen van crud operaties

__TODO__

## Herbruikbare library

__TODO__