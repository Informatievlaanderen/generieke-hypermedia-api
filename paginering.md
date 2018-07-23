Discussie: [#1](https://github.com/pietercolpaert/generieke-hypermedia-api/issues/1)

# Server-side paginering

Server-side paginering wordt gebruikt om datasets die ontsloten worden via API's op te delen in kleinere result sets om zo een lagere initiële laadtijd te bekomen voor afnemers.

Deze specificatie beschrijft een abstract algoritme dat een generieke client toelaat een aantal methodes voor paginering te herkennen en gebruiken.

**Opmerking**: deze specificatie doet geen uitspraken over de paginering strategie van een server. Zo kunnen onder meer cursor-, offset- of pagina gebaseerde paginering strategieën gebruikt worden.

## Nakomingsniveaus

### HTTP

Een server _MOET_ ondersteuning bieden voor volgende methodes:

+ [RFC 5988 - Web Linking](https://tools.ietf.org/html/rfc5988)

### Semantisch

Een server die ondersteuning wil bieden voor Linked Data _MOET_ bijkomend volgende methodes ondersteunen:

+ [Hydra Core Vocabulary](https://www.hydra-cg.com/spec/latest/core/)

Daarnaast _MAG_ een server volgende specificaties ondersteunen:

+ [Hypertext Application Language (HAL) Internet Draft](http://stateless.co/hal_specification.html)
+ [JSON API Pagination](http://jsonapi.org/format/#fetching-pagination)

In dit laatste geval _MOET_ de JSON payload geannoteerd te worden met een JSON-LD context om de HAL of JSON API notatie voor paginering te normaliseren naar de Hydra vocabulary. Onderstaande JSON-LD context is gebaseerd op de JSON-LD 1.1 specificatie en laat toe hypermedia controls voor paginering in HAL of JSON API te interpreteren volgens de Hydra vocabulary.

```json
{
  "@context": {
     "hydra": "http://www.w3.org/ns/hydra/core#",
     "first": { "@id": "hydra:first", "@type": "@id" },
     "last": { "@id": "hydra:last", "@type": "@id" },
     "next": { "@id": "hydra:next", "@type": "@id" },
     "prev": { "@id": "hydra:previous", "@type": "@id" },
     "_links": "@nest",
     "links": "@nest", 
     "href": "@id",
  }
}
```

## Code voorbeelden

### RFC 5988 - Web Linking

De HTTP Link header bevat informatie voor paginering.

```
Link: 	<https://hostname/api/resource?page=4&limit=100>; rel="next",
  		<https://hostname/api/resource?page=50&limit=100>; rel="last",
  		<https://hostname/api/resource?page=1&limit=100>; rel="first",
  		<https://hostname/api/resource?page=2&limit=100>; rel="prev"
```

Volgende relaties worden ondersteund:

| Naam        | Omschrijving                                        |
| ----------- |:---------------------------------------------------:|
| `next`      | Link relatie voor de volgende pagina met resultaten |
| `last`      | Link relatie voor de laatste pagina met resultaten  |
| `first`     | Link relatie voor de eerste pagina met resultaten   |
| `prev`      | Link relatie voor de vorige pagina met resulaten    |
| `previous`  | Synoniem voor `prev`                                |

### Hydra

De JSON(-LD) respons bevat informatie voor paginering op basis van het Hydra vocabularium.

```json
{
  "@context": "http://www.w3.org/ns/hydra/context.jsonld",
  "@id": "https://hostname/api/resource?page=3",
  "@type": "PartialCollection",
  "next": "/api/resource?page=4",
  "last": "/api/resource?page=50",
  "first": "/api/resource",
  "previous": "/api/resource?page=2",
  "totalItems": 2,
  "member": [
      {
          "@id": "/api/resource/5"
      },
      {
          "@id": "/api/resource/6"
      }
  ]
}
```

### HAL

De JSON of XML respons bevat informatie voor paginering in een `_links` node.

```json
{
    "_links": {
        "self": {
            "href": "/api/resource?page=3"
        },
        "first": {
            "href": "/api/resource"
        },
        "prev": {
            "href": "/api/resource?page=2"
        },
        "next": {
            "href": "/api/resource?page=4"
        },
        "last": {
            "href": "/api/resource?page=5"
        }
    },
    "count": 2,
    "total": 100,
    "_embedded": [
      {
		    "_links": {
        	"self": { "href": "/api/resource/5" }
        }
      },
      {
		    "_links": {
        	"self": { "href": "/api/resource/6" }
        }
      }
    ]
}
```

### JSON API

De JSON respons bevat informatie voor paginering in een `links` node.

```json
{
    "links": {
        "self": "/api/resource?page=3",
        "first": "/api/resource",
        "prev": "/api/resource?page=2",
        "next": "/api/resource?page=4",
        "last": "/api/resource?page=5"
    },
    "meta": {
      "total-pages": 100
    },
    "data": [
      {
        "id": "/api/resource/5"
      },
      {
        "id": "/api/resource/6"
      }
    ]
}
```

## Client algoritme voor paginering

Onderstaande reeks stappen definieert een algoritme die door een generieke client kan worden toegepast om gebruik te maken van paginering.

Namespace prefix rdfs: http://www.w3.org/1999/02/22-rdf-syntax-ns#
Namespace prefix hydra: http://www.w3.org/ns/hydra/core#

1. Als de Link headers een `rel` attribuut bevatten met `next`, `last`, `first`, `previous` of `prev`, gebruik de correspondere links om de paginering te initialiseren.
2. Anders, als de response header `application/ld+json` als Content-Type bevat, gebruik het [JSON-LD 1.1 processing algoritme](https://json-ld.org/spec/FCGS/json-ld-api/20180607/#expansion-algorithms) om de response om te zetten in zijn geëxpandeerde vorm.
3. Als de response een object bevat met als attribuut `rdfs:type`  met de waarde `hydra:PartialCollection`, gebruik de `hydra:next`, `hydra:last`, `hydra:first` of `hydra:previous` attributen om de paginering te initialiseren.

## Herbruikbare library

- Voor clients: https://github.com/pietercolpaert/extract-page-controls
