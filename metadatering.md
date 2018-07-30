Discussie: [#9](https://github.com/pietercolpaert/generieke-hypermedia-api/issues/9)

__Opmerking: in het OSLO traject DCAT-AP Vlaanderen wordt momenteel gewerkt aan een metadata profiel op Vlaams niveau, de verplichte en optioneel op te nemen metadata attributen zullen hiermee afgestemd worden__

# Metadatering van API's

Het metadateren van API's biedt waardevolle technische en organisatorisch informatie aan afnemers die met een bestaande API aan de slag willen gaan, door een overzicht te geven van de structuur en de mogelijkheden die een bepaalde API biedt. Daarnaast zorgt gestructureerde metadata ervoor dat informatie automatisch kan worden opgehaald en getoond door bijvoorbeeld een web service catalogus. 

Deze specificatie beschrijft welke metadata kan opgenomen worden op niveau van de API en welke metadatastandaarden gevolgd dienen te worden. Volgende types van metadata worden onderscheiden:
* API identificatie (versiedatum, titel...)
* Contactgegevens m.b.t. de API
* Temporele begrenzing
* Geografische begrenzing
* Beschikbare resources

## Nakomingsniveaus

### HTTP

Een server _MOET_ ondersteuning bieden voor volgende methodes:

+ De response van een GET request op het entry point van de API bevat een Link Header met rel="http://www.w3.org/ns/hydra/core#apiDocumentation"

### Semantisch

Een server die ondersteuning wil bieden voor Linked Data _MOET_ de metadata van de API in-band beschikbaar maken door de implementatie van onderstaande metadata specificaties:

* API identificatie: [DCMI Metadata Terms](http://dublincore.org/documents/dcmi-terms/) en [Hydra Core Vocabulary](http://www.hydra-cg.com/spec/latest/core/)
* Contactgegevens m.b.t. de API: [OSLO implementatie van schema:ContactPoint](https://data.vlaanderen.be/doc/applicatieprofiel/generiek#Contactpunt)
* Temporele begrenzing: [DCMI Metadata Terms](http://dublincore.org/documents/dcmi-terms/), [dct:temporal](	http://purl.org/dc/terms/temporal)
* Geografische begrenzing: [DCMI Metadata Terms](http://dublincore.org/documents/dcmi-terms/), [dct:spatial](http://purl.org/dc/terms/spatial)
* Beschikbare resources: [Hydra Core Vocabulary](http://www.hydra-cg.com/spec/latest/core/)

## Code voorbeelden

### HTTP

```http
GET https://hostname/api
```

```http
HTTP 200 OK
Link: <https://hostname/api/documentation>; rel="http://www.w3.org/ns/hydra/core#apiDocumentation"
```

```http
GET https://hostname/api/documentation
```

```http
HTTP 200 OK
```

### Semantisch

```http
GET https://hostname/api
```

```http
HTTP 200 OK
```

```json
{
  "@context": [
  	"http://www.w3.org/ns/hydra/context.jsonld",
  	"https://raw.githubusercontent.com/SEMICeu/DCAT-AP/master/releases/1.1/dcat-ap_1.1.jsonld"
  ],
  "@id": "/api",
  "@type": "EntryPoint, Distribution",
  "apiDocumentation": "/api/documentation",
  "issued": "2016-01-10",
  "modified": "2018-07-24",
  "accessURL": "https://hostname/api",
  "mediaType": [ "ld+json" ],
}
```

```http
GET https://hostname/api/documentation
```

```http
HTTP 200 OK
```

```json
{
  "@context": [
  	"http://www.w3.org/ns/hydra/context.jsonld",
  	"https://data.vlaanderen.be/context/generiek.jsonld",
  	"https://raw.githubusercontent.com/SEMICeu/DCAT-AP/master/releases/1.1/dcat-ap_1.1.jsonld",
  	{ 
  		"schema": "https://schema.org/",
  		"distributionOf": { "@reverse": "http://www.w3.org/ns/dcat#distribution" },
  		"geometry": { 
  			"@id": "http://www.w3.org/ns/locn#geometry", 
  			"@type": "http://www.opengis.net/ont/geosparql#wktLiteral" 
  		}
  	}
  ],
  "@id": "https://hostname/api/documentation",
  "@type": "ApiDocumentation, Distribution",
  "title": "Voorbeeld Event API",
  "description": "Lorem ipsum dolor sit amet",
  "issued": "2016-01-10",
  "modified": "2018-07-24",
  "license": "https://overheid.vlaanderen.be/sites/default/files/documenten/ict-egov/licenties/hergebruik/modellicentie_gratis_hergebruik_v1_0.html",
  "entrypoint": "https://hostname/api",
  "schema:contactPoint": {
  	"contactnaam": "Helpdesk",
  	"email": "info@example.org",
  	"website": "https://example.org/helpdesk"
  },
  "temporal": {
	"@type": "PeriodOfTime",
	"startDate": "2010-01-01",
	"endDate": "2016-12-31"
  },
  "spatial": {
	"@type": "Location",
	"geometry": "POLYGON((-10.58 70.09,34.59 70.09,34.59 34.56,-10.5834.56))"
  },
  "supportedClass": [
      {
          "@id": "schema:Event",
          "name": "An Event",
          "label": "Event",
          "supportedProperty": [
              {
                  "@type": "SupportedProperty",
                  "property": "schema:eventName"
              },
              {
                  "@type": "SupportedProperty",
                  "property": "schema:eventDescription"
              },
              {
                  "@type": "SupportedProperty",
                  "property": "schema:startDate"
              },
              {
                  "@type": "SupportedProperty",
                  "property": "schema:endDate"
              }
          ]
      }
  ]
}
```

Een "supportedProperty" kan ook nog verder gespecifieerd worden door gebruik te maken van de [Shapes Constraint Language (SHACL)](https://www.w3.org/TR/shacl/):

```json
{
	"@context": {
		"hydra": "http://www.w3.org/ns/hydra/context.jsonld",
		"sh": "http://www.w3.org/ns/shacl#",
		"schema": "https://schema.org/"
	}
	...,
	"supportedProperty": [
     {
         "@type": "hydra:SupportedProperty, sh:PropertyShape",
		 "sh:name": "eventName",
		 "sh:description": "Een naam voor het event",
		 "sh:path": "schema:eventName",
		 "sh:minCount": "1",
		 "sh:maxCount": "1"         	
     }
 	],
 	...
}
```

## Gebruik van de OpenAPI 2.0 specificatie

De meeste van de metadata velden in bovestaande voorbeelden wordt vandaag in veel API's reeds opgenomen in lijn met de OpenAPI specificatie.

Onderstaande JSON-LD context mapped enkele belangrijke JSON velden uit het OpenAPI schema op de vermelde Metadata vocabularia.

```json
{
	"@context": {
		"hydra": "http://www.w3.org/ns/hydra/context.jsonld",
		"sh": "http://www.w3.org/ns/shacl#",
		"schema": "https://schema.org/",
		"dct": "http://purl.org/dc/terms/",
		"owl": "http://www.w3.org/2002/07/owl#",
		"foaf": "http://xmlns.com/foaf/0.1/",

		"info": "hydra:apiDocumentation",
		"basePath": "dcat:accessURL",
		"consumes": "dcat:mediaType",
		"produces": "dcat:mediaType",
		"paths": "hydra:supportedClass",

		"title": "dct:title",
		"description": "dct:description",
		"termsOfService": "schema:termsOfService",
		"contact": "schema:contactPoint",
		"license": "dct:license",
		"version": "owl:versionInfo",

		"name": "foaf:name",
		"url": "foaf:page",
		"email": "schema:email"
	}
}
```

Verder is het mogelijk om vanuit de Hydra `ApiDocumentation` een OpenAPI specificatie `Paths Object` te genereren.

Voorbeeld met Hydra en SHACL:
```json
{
  "@context": {
  	"hydra": "http://www.w3.org/ns/hydra/core#",
	"sh": "http://www.w3.org/ns/shacl#",
	"schema": "https://schema.org/",
    "rdfs": "http://www.w3.org/2000/01/rdf-schema#"
  },
  "@id": "https://hostname/api/documentation",
  "@type": "hydra:ApiDocumentation",
  "hydra:title": "Voorbeeld Event API",
  "hydra:description": "Lorem ipsum dolor sit amet",
  "hydra:supportedClass": [
      {
          "@id": "schema:Event",
          "rdfs:label": "Event",
          "hydra:supportedProperty": [
            {
            	"@id": "schema:eventName",
                "@type": "hydra:SupportedProperty, sh:PropertyShape",
				"sh:name": "eventName",
				"sh:description": "Een naam voor het event",
				"sh:path": "schema:eventName",
				"sh:minCount": "1",
				"sh:maxCount": "1"         	
			}
          ]
      }
  ],
  "hydra:supportedOperation": [
  	{
      "@type": "hydra:Operation",
      "hydra:method": "GET",
      "hydra:description": "Haal de informatie over een bepaald event op.",
      "hydra:expects": {
        "@id": "schema:Event",
        "sh:targetClass": "schema:Event",
        "sh:property": [
          {
             "sh:name": "eventName",
             "sh:description": "Een naam voor het event",
             "sh:path": "schema:eventName",
             "sh:minCount": "1",
             "sh:maxCount": "1",
             "sh:datatype": "http://www.w3.org/2001/XMLSchema#string"            
          }
        ]
      },
      "hydra:returns": "schema:Event",
      "hydra:possibleStatus": [
      	{
      		"statusCode": "200",
      		"title": "Succesvol",
      		"description": "Succesvolle event response"
      	}
      ]	
  	}
  ]
}
```

Voorbeeld van zelfde informatie omgezet in OpenAPI 2.0 specificatie:
```json
{
  "title": "Voorbeeld Event API",
  "description": "Lorem ipsum dolor sit amet",
  "paths": [
  	{
  		"/event": {
  			"get": {
  				"description": "Haal de informatie over een bepaald event op.",
  				"produces": "application/json",
  				"responses": {
  					"200": {
  						"description": "Succesvolle vent response",
			      		"schema": {
			      			"type": "array",
			      			"items": {
			      				"$ref": "https://schema.org/Event"
			      			}
			      		}
  					}
  				},
  				"parameters": {
  					"name": "eventName",
  					"in": "body",
  					"description": "Een naam voor het event",
  					"required": "true",
  					"schema": {
  						"type": "string"
  					}
  				}
  			}
  		}
  	}
  ]
}
```

Enkel assumpties werden gemaakt om de omzetting uit te voeren:
* `/event > get > responses > schema > type`: de transformatie verondersteld dat de response een array van informatie-velden bevat. De OpenAPI specificatie voorziet ook in "string", "number", "integer", "boolean" en "file" als alternatieven.
* `/event > get > parameters > in`: de transformatie verondersteld dat deze parameter in de body van de request wordt meegegeven. De OpenAPI specificatie voorziet ook in 'query', 'header', 'path' en 'formData' als alternatieven. 
* `/event > get > parameters > required`: is een boolean veld in de OpenAPI specificatie en moet worden afgeleid uit de `sh:minCount`.

## Algoritme voor herkennen van API metadata.

__TODO__

## Herbruikbare library

__TODO__