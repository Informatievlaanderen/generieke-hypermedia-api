Discussie: [#3](https://github.com/pietercolpaert/generieke-hypermedia-api/issues/3)

# Full Text Search

APIs die entiteiten bevatten met tekstuele eigenschappen kunnen een zoekfunctionaliteit aanbieden die
gebruikers toelaten om, gegeven een (deel van een) tekst, alle entiteiten te tonen die deze tekst bevatten in hun eigenschappen.

Een API die personen bevat kan bijvoorbeeld een _full text search_ functionaliteit aanbieden
om personen te zoeken op verscheidene eigenschappen, zoals naam, woonplaats, ...

## Nakomingsniveaus

### HTTP

Een server _MOET_ een query toelaten via een HTTP GET [query string](https://en.wikipedia.org/wiki/Query_string) parameter.

### Semantisch

Een server _MOET_ ondersteuning bieden voor de Hydra [`freetextQuery`](http://www.hydra-cg.com/spec/latest/core/#hydra:freetextQuery).

Wanneer er fragmentaties van toepassing zijn op de API (zoals opzoekresultaten of [paginering](https://github.com/Informatievlaanderen/generieke-hypermedia-api/blob/master/paginering.md)),
dan _MOET_ de kanonieke API URI aangeduid worden via een [`void:subset`](https://www.w3.org/TR/void/#subset) vanaf de opgevraagde URL.

Daarnaast _MAG_ de server tegelijkertijd ondersteuning bieden voor de `filter` parameter
binnen [JSON API](http://jsonapi.org/format/#fetching-filtering)
door de variable naam van de `freetextQuery` `filter` te noemen.

Indien het antwoord van de zoekopdracht niet triviaal is,
of als er meerdere zoekparameters mogelijk zijn,
dan _MAG_ de server een [SHACL](https://www.w3.org/TR/shacl/)-gebaseerde uitbreiding
van de Hydra `freetextQuery` gebruiken om de
[structuur van de zoekresultaten declaratief te beschrijven](https://linkeddatafragments.github.io/Article-Declarative-Hypermedia-Responses/#approach-shacl).

## Code voorbeelden

### HTTP

Client request:
```
GET /personen?naam=Bob HTTP/1.1
Host: api.example.org
```

### Semantisch

Server response voor http://api.example.com/personen:
```
{
  "@context": "http://www.w3.org/ns/hydra/context.jsonld",
  "@type": "IriTemplate",
  "@id": "http://api.example.com/personen",
  "search": {
    "template": "http://api.example.com/personen{?filter}",
    "variableRepresentation": "BasicRepresentation",
    "mapping": [
      {
        "@type": "IriTemplateMapping",
        "variable": "filter",
        "property": "hydra:freetextQuery",
        "required": true
      }
    ]
  }
}
```

Server response voor http://api.example.com/personen?filter=Bob in geval van fragmentatie door zoeken:
```
{
  "@context": "http://www.w3.org/ns/hydra/context.jsonld",
  "@type": "IriTemplate",
  "@id": "http://api.example.com/personen",
  "http://rdfs.org/ns/void#subset": "http://api.example.com/personen?filter=Bob",
  "search": {
    "template": "http://api.example.com/personen{?filter}",
    "variableRepresentation": "BasicRepresentation",
    "mapping": [
      {
        "@type": "IriTemplateMapping",
        "variable": "filter",
        "property": "hydra:freetextQuery",
        "required": true
      }
    ]
  }
}
```

Server response voor http://api.example.com/personen?page=1 in geval van fragmentatie:
```
{
  "@context": "http://www.w3.org/ns/hydra/context.jsonld",
  "@type": "IriTemplate",
  "@id": "http://api.example.com/personen",
  "http://rdfs.org/ns/void#subset": "http://api.example.com/personen?page=1",
  "search": {
    "template": "http://api.example.com/personen{?filter}",
    "variableRepresentation": "BasicRepresentation",
    "mapping": [
      {
        "@type": "IriTemplateMapping",
        "variable": "filter",
        "property": "hydra:freetextQuery",
        "required": true
      }
    ]
  }
}
```

Server response voor http://api.example.com/personen met SHACL uitbreiding voor het beschrijven van de resultatenstructuur:
```
{
  "@context": [
    "http://www.w3.org/ns/hydra/context.jsonld",
    {
      "sh": "http://www.w3.org/ns/shacl#",
      "ex": "http://example.org/"
    }
  ],
  "@type": "IriTemplate",
  "@id": "http://api.example.com/personen",
  "search": {
    "template": "http://api.example.com/personen{?filter}",
    "variableRepresentation": "BasicRepresentation",
    "mapping": [
      {
        "@type": "IriTemplateMapping",
        "variable": "filter",
        "property": "hydra:freetextQuery",
        "required": true
      }
    ],
    "sh:parameter": { "sh:path": { "@id": "ex:filter" } },
    "sh:select": "SELECT ?filter WHERE { ?person http://xmlns.com/foaf/0.1/name ?filter }"
  }
}
```

## Algoritme voor herkennen van zoekfunctionaliteiten

Onderstaande reeks stappen definieert een algoritme die door een generieke client kan worden toegepast om gebruik te maken van full text search.

1. Is de relevante query string parameter voor de API al geweten?
    1. Ja: Bevraag de API met de gekende parameter als key en de query als value.
    2. Neen: bevraag de API, en herken het response formaat.
       1. RDF:
           1. Zoek de Hydra query template met de kanonieke API URL. Deze kanonieke API URL kan gevonden worden door inverse `void:subset` links te volgen.
           2. Interpreteer de Hydra query template.
           3. Vul de template URL in met de gewenste query.
           4. Bevraag de gematerialiseerde template URL.
       2. JSON: Bevraag de API met de `filter` parameter als key en de query als value.
       3. Anders: Error

## Herbruikbare library

__TODO__