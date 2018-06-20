Discussie: [#6](https://github.com/pietercolpaert/generieke-hypermedia-api/issues/6)

# CRUD operaties

CRUD of Create, Read, Update & Delete zijn de vier basisoperaties die op resources kunnen worden uitgevoerd.
* Create: aanmaken van een nieuwe resource
* Read: opvragen van gegevens over een specifieke resource
* Update: wijzigen van gegevens over een specifieke resource
* Delete: verwijderen van een resource

Deze sectie beschrijft een abstract algoritme dat een generieke client toelaat CRUD acties te herkennen die toegelaten zijn op een bepaalde resource.

## Nakomingsniveaus

### HTTP

Een server _MOET_ ondersteuning bieden voor volgende methodes:

+ [Linked Data Platform](https://www.w3.org/TR/ldp/)

### Semantisch

Een server die ondersteuning wil bieden voor Linked Data _MOET_ bijkomend volgende methodes ondersteunen:

+ [Hydra](http://www.hydra-cg.com/spec/latest/core/)

## Code voorbeelden

### Linked Data Platform

De toegelaten operaties worden meegegeven in de HTTP `Allow` response header.

```
GET /api/resource/1 HTTP/1.1
Host: example.org
Accept: application/ld+json
```

```
HTTP/1.1 200 OK 
Content-Type: application/ld+json; charset=UTF-8
Link: <http://www.w3.org/ns/ldp#Resource>; rel="type"
Allow: GET,PUT,DELETE
```

### Hydra

De toegelaten operaties worden meegegeven in de response body via de eigenschap [hydra:operation](http://www.w3.org/ns/hydra/core#operation).

```json
{
  "@context": "http://www.w3.org/ns/hydra/context.jsonld",
  "@id": "/api/resource/1",
  "title": "Een voorbeeld resource",
  "description": "Deze resource kan verwijderd worden met een HTTP DELETE request of aangepast worden met een HTTP PUT request",
  "operation": [
    {
      "@type": "Operation",
      "method": "DELETE"
    },
    {
      "@type": "Operation",
      "method": "PUT"
    }
  ]
}
```

## Algoritme voor herkennen van crud operaties

Onderstaande reeks stappen definieert een algoritme die door een generieke client kan worden toegepast om CRUD operaties te herkennen.

__TODO__

## Herbruikbare library

__TODO__