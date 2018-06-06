Discussie: #1
Open: Hoe omgaang met gepagineerde CSV downoads? (#12)

# Server-side paginering

Server-side paginering wordt gebruikt om grote datasets die ontsloten worden via API's op te delen in kleinere result sets om zo een lagere initiële laadtijd te bekomen voor afnemers.

Deze sectie beschrijft een abstract algoritme dat een generieke client toelaat volgende methodes voor paginering te herkennen en gebruiken:

+ [RFC 5988 - Web Linking](https://tools.ietf.org/html/rfc5988)
+ [Draft Hydra specificatie voor paginering](https://github.com/HydraCG/Specifications/blob/master/drafts/use-cases/3.2.pagination.md)
+ [Hypertext Application Language (HAL) Internet Draft](http://stateless.co/hal_specification.html)

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
        },
        ...
      },
      {
		"_links": {
        	"self": { "href": "/api/resource/6" }
        },
        ...
      }
    ]
}
```

## Algoritme voor paginering

Onderstaande reeks stappen definieert een algoritme die door een generieke client kan worden toegepast om gebruik te maken van paginering.

1. Als de Link headers een `rel` attribuut bevatten met `next`, `last`, `first`, `previous` of `prev`, gebruik de correspondere links om de paginering te initialiseren.
2. Anders, als de response een `_links` node bevat met als child nodes `next`, `last`, `first` of `prev`, gebruik de corresponderende `href` child nodes om de paginering te initialiseren.
3. Anders, als de response op root-level een `@type` key (JSON-LD) of `http://www.w3.org/1999/02/22-rdf-syntax-ns#type` predikaat (RDF Triples) heeft met als waarde `PartialCollection` of `http://www.w3.org/ns/hydra/core#PartialCollection`, gebruik de `next`, `last`, `first` of `previous` attributen om de paginering te initialiseren.

## Herbruikbare library

https://github.com/pietercolpaert/extract-page-controls
