Discussie: [#1](https://github.com/pietercolpaert/generieke-hypermedia-api/issues/1)

# Server-side paginering

Server-side paginering wordt gebruikt om datasets die ontsloten worden via API's op te delen in kleinere result sets om zo een lagere initiële laadtijd te bekomen voor afnemers.

Deze sectie beschrijft een abstract algoritme dat een generieke client toelaat een aantal methodes voor paginering te herkennen en gebruiken.

Een server _MOET_ ondersteuning bieden voor volgende methodes:
+ [RFC 5988 - Web Linking](https://tools.ietf.org/html/rfc5988)

Daarnaast _MAG_ een server volgende methodes ondersteunen:
+ [Draft Hydra specificatie voor paginering](https://github.com/HydraCG/Specifications/blob/master/drafts/use-cases/3.2.pagination.md)
+ [Hypertext Application Language (HAL) Internet Draft](http://stateless.co/hal_specification.html)
+ [JSON API Pagination](http://jsonapi.org/format/#fetching-pagination)

Een server die ondersteuning wil bieden voor Linked Data _MOET_ bijkomend volgende methodes ondersteunen:
+ [Draft Hydra specificatie voor paginering](https://github.com/HydraCG/Specifications/blob/master/drafts/use-cases/3.2.pagination.md)

Deze specificatie doet geen uitspraken over de paginering strategie van een server. Zo kunnen onder meer cursor-, offset- of pagina gebaseerde paginering strategieën gebruikt worden.

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

__TODO__

## HAL

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

## JSON API

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

1. Als de Link headers een `rel` attribuut bevatten met `next`, `last`, `first`, `previous` of `prev`, gebruik de correspondere links om de paginering te initialiseren.
2. Anders, als de response een `_links` node bevat met als keys `next`, `last`, `first` of `prev`, gebruik de corresponderende waarde om de paginering te initialiseren.
3. Anders, als de response een `links` node bevat met als child nodes `next`, `last`, `first` of `prev`, gebruik de corresponderende `href` child nodes om de paginering te initialiseren.
4. Anders, als de response op root-level een `@type` key (JSON-LD) of `http://www.w3.org/1999/02/22-rdf-syntax-ns#type` predikaat (RDF Triples) heeft met als waarde `PartialCollection` of `http://www.w3.org/ns/hydra/core#PartialCollection`, gebruik de `next`, `last`, `first` of `previous` attributen om de paginering te initialiseren.

## Herbruikbare library

- Voor clients: https://github.com/pietercolpaert/extract-page-controls
