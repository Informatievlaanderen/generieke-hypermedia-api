Discussie: [#7](https://github.com/pietercolpaert/generieke-hypermedia-api/issues/7)

# Authenticatie, signing en authorisatie

Om gebruik te maken van bepaalde API methoden en operaties is het vaak noodzakelijk dat een client zich authenticeert. _Authenticatie_ laat de server toe om een client uniek te identificeren en bijkomend de authorisaties van de client op te halen. _Authorisaties_ bepalen welke methoden en operaties aangeboden worden door de API aan een client. _Signing_ laat een client toe elke request naar een server te ondertekenen om zo zijn identiteit te bewijzen.

Een server _MOET_ ondersteuning bieden voor volgende specificaties:

* [RFC 7235 - Hypertext Transfer Protocol (HTTP/1.1): Authentication](https://tools.ietf.org/html/rfc7235#section-4.1)

## Code voorbeelden

```
HTTP/1.1 401 Unauthorized  
WWW-Authenticate: Bearer realm="example.org"
```

## Algoritme voor herkennen van crud operaties

__TODO__

## Herbruikbare library

__TODO__