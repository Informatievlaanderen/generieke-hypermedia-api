Discussie: [#10](https://github.com/pietercolpaert/generieke-hypermedia-api/issues/10)

# Versionering

Data blijft zelfden statisch,
daarom is het belangrijk dat resources gewijzigd kunnen worden
wanneer de achterliggende data wijzigt.
Daarbovenop is het soms ook belangrijk dat _eerdere versies_
van een resource opgevraagd kunnen worden.
Dit principe van het beheren van meerdere versies noemen we _versionering_.

In veel gevallen kunnen versies geïdentificeerd worden door middel van tijdstippen.
Dit is echter niet altijd het geval, zoals bijvoorbeeld [git commits](https://git-scm.com/docs/git-commit)
die versies van een project aanduiden via een alfanumerieke _hash_.

**Opmerking**: We spreken hier over versionering van de _data_, niet van de _controle_ (API).
Deze specificatie maakt geen uitspraak over hoe API's geversioneerd moeten worden,
aangezien hier verschillende werkbare technieken voor bestaan.
In bouwblok-gebaseerde hypermedia API's is versionering van de API voor clients niet noodzakelijk.
Generieke clients dienen controle die beschreven staat in server responses automatisch te herkennen,
en op basis daarvan operaties op de API uit te voeren.
Indien een API wijzigt, dient de beschrijving van de controle ook gewijzigd te worden,
waardoor clients automatisch kunnen werken met deze nieuwe versie.
Versionering van API's is hier echter orthogonaal mee,
wat wilt zeggen dat API's _mogen_ geversioneerd worden,
zolang de generieke beschrijvingen correct gewijzigd worden bij aanpassingen.

## Nakomingsniveaus

### HTTP

Indien temporele versionering van toepassing is
_MOET_ het [Memento raamwerk](https://tools.ietf.org/html/rfc7089) gebruikt te worden.
Hiermee kunnen resources via HTTP headers aanduiden in welk temporele versie ze zich bevinden,
door middel van een tijdsannotatie.
Verder laat Memento clients toe om een bepaalde versie van een resource op te vragen
met [_datetime negotiation_](https://mementoweb.org/guide/rfc/#overview-datetime-conneg).

Indien versies _atemporeel_ gedefinieerd zijn, bestaan er geen standaarden.
Hiervoor _MAG_ bijvoorbeeld [`alternate` link headers](https://www.w3.org/Protocols/9707-link-header.html)
gebruikt worden om verschillende versies van een resource aan te duiden,
waarbij de _laatste_ versie van de resource op een vaste link gezet kan worden,
en clients via een [HTTP 307 redirect](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/307)
naar de geversioneerde URL gestuurd kan worden.

### Semantisch

Op HTTP niveau kunnen we enkel de versie van _volledige resources_ beschrijven.
Op semantisch niveau kunnen we RDF statements op meer granulair annoteren.

Indien de versionering temporeel is _MOET_ het [`prov:generatedAtTime`](https://www.w3.org/TR/prov-o/#generatedAtTime)
predikaat gebruikt te worden.
Indien een atemporele versionering van toepassing is _MAG_ een niet gestandardiseerde ontologie gebruikt worden,
zoals bijvoorbeeld [`ver:relatedVersion`](http://semweb.datasciencelab.be/ns/version/#relatedVersion).

## Code voorbeelden

### HTTP

Client request:
```
GET /api/resource/1 HTTP/1.1
Host: example.org
Accept-Datetime: Mon, 18 Jun 2018 11:49:40 GMT
```

Server response:
```
HTTP/1.1 200 OK 
Memento-Datetime: Mon, 18 Jun 2018 11:49:40 GMT
...
```

### Semantisch

```
<#observation1> {
   # mijn data binnen observation1
}
<#observation1> prov:generatedAtTime "2018-06-01T12:00Z"^^xsd:dateTime .
```

## Algoritme voor datetime negotiation

https://mementoweb.org/guide/rfc/#overview-datetime-conneg

## Herbruikbare library

* https://www.npmjs.com/package/wayback-machine
* https://www.npmjs.com/package/memento-client