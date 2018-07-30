Discussie: [#8](https://github.com/pietercolpaert/generieke-hypermedia-api/issues/8)

# Taal-ontdekking/selectie

De **detectie** van de taal van een bepaalde resource is belangrijk voor clients met taal-afhankelijke functionaliteiten,
zoals bijvoorbeeld het samenvatten en beschrijven van bepaalde resources afhankelijk van de taal van de gebruiker.

Anderzijds gaat de **selectie** van een taal een stap verder,
hierbij kunnen protocollen zoals [_HTTP language negotiation_](https://www.w3.org/International/questions/qa-when-lang-neg)
door clients gebruikt worden om een bepaalde resource in een gegeven taal op te vragen.

Een API _MOET_ minimaal ondersteuning bieden voor één van de volgende taalcodes:
* nl-BE
* nl

**Opmerking**: We spreken hier over de taal van de _data_, niet van de _controle_.
Hier zijn bijvoorbeeld de taal van de naam en beschrijving van resources van belang.
De taal die (eventueel) gebruikt wordt binnen URLs of andere API-specifieke benamingen is irrelevant,
aangezien clients hier geen interpretatie van (zouden) moeten doen.

## Nakomingsniveaus

### HTTP

Voor het toelaten van de **detectie** van de taal van een resource _MOET_
een [`Content-Language`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Language)
HTTP header beschikbaar te zijn voor de resource binnen een server response.

Voor het toelaten van de **selectie** van de taal van een resource _MOET_
een server de [`Accept-Language`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Language)
in de client request correct behandelen, zodat [_HTTP language negotiation_](https://www.w3.org/International/questions/qa-when-lang-neg)
kan plaats vinden.

### Semantisch

Voor het toelaten van de **detectie** van de taal van een resource _MOET_
tekst geannoteerd te zijn met een taal door middel van [RDF language tagged strings](https://www.w3.org/TR/rdf11-concepts/#dfn-language-tagged-string).

Voor de selectie van een taal schrijven we op semantisch niveau geen vereisten voor.
Hiervoor kan ofwel enkel selectie door middel van [_HTTP language negotiation_](https://www.w3.org/International/questions/qa-when-lang-neg)
gebruikt worden, ofwel kan een eigen hypermedia operatie beschreven worden met een taal-parameter.

**Opmerking**: De HTTP en Semantische nakomingsniveaus moeten compatibel zijn met elkaar.
Dit wilt zeggen dat wanneer een `Content-Language` header beschikbaar binnen de (RDF) response voor een resource,
dat _minstens_ __language tagged strings__ in die taal beschikbaar moeten zijn.

## Code voorbeelden

### HTTP

Client request:
```
GET /api/resource/1 HTTP/1.1
Host: example.org
Accept-Language: nl-be,nl-nl;q=0.7
```

Server response:
```
HTTP/1.1 200 OK 
Content-Language: nl-be
...
```

### Semantisch

RDF response:
```
<http://example.org/gent> rdfs:label "Gent"@nl-be
                                     "Ghent"@en-us.
```

JSON-LD response:
```
{
  "@context": {
  	"label": { "@id": "http://www.w3.org/2000/01/rdf-schema#label", "@container": "@language" }
  },
  "@id": "/api/resource/1",
  "label": {
  	"nl-be": "Gent",
  	"en-us": "Ghent"
  }
}
```

## Algoritme voor language negotiation

https://www.ietf.org/rfc/rfc4647.txt

## Herbruikbare library

* https://www.npmjs.com/package/accept-language-negotiator