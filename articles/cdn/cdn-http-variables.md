---
title: HTTP-variabelen voor regels-engine van Azure CDN | Microsoft Docs
description: HTTP-variabelen kunnen u HTTP-aanvraag en respons metagegevens ophalen.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: magattus
ms.openlocfilehash: 70154a50086fabc84671e7e540d8496490ab5b2d
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57436626"
---
# <a name="http-variables-for-azure-cdn-rules-engine"></a>HTTP-variabelen voor regels-engine van Azure CDN
HTTP-variabelen bieden de mogelijkheid waarmee u HTTP-aanvraag en respons metagegevens kunt ophalen. Deze metagegevens kunt vervolgens worden gebruikt om een aanvraag of antwoord dynamisch wijzigen. Het gebruik van HTTP-variabelen is beperkt tot de volgende regels-engine-functies:

- [Cache-sleutel opnieuw schrijven](cdn-rules-engine-reference-features.md#cache-key-rewrite)
- [Client-aanvraagheader wijzigen](cdn-rules-engine-reference-features.md#modify-client-request-header)
- [Client-Reactieheader wijzigen](cdn-rules-engine-reference-features.md#modify-client-response-header)
- [URL-omleiding](cdn-rules-engine-reference-features.md#url-redirect)
- [Herschrijven van URL](cdn-rules-engine-reference-features.md#url-rewrite)

## <a name="definitions"></a>Definities
De volgende tabel beschrijft de ondersteunde HTTP-variabelen. Een lege waarde wordt geretourneerd wanneer GEO-metagegevens (bijvoorbeeld postcode) niet beschikbaar voor een bepaalde aanvraag is.


| Name | Variabele | Description | Voorbeeldwaarde |
| ---- | -------- | ----------- | ------------ |
| ASN (aanvrager) | %{geo_asnum} | Geeft aan van de aanvrager als getal. <br /><br />**Afgeschafte:** % {virt_dst_asnum}. <br />Deze variabele is afgeschaft en vervangen door % {geo_asnum}. Hoewel een regel die gebruikmaakt van deze afgeschaft variabele werken blijven, moet u deze voor het gebruik van de nieuwe variabele bijwerken. | AS15133 |
| Plaats (aanvrager) | %{geo_city} | Hiermee wordt aangegeven in plaats van de aanvrager. | Los Angeles |
| Continent (aanvrager) | %{geo_continent} | Geeft aan van de aanvrager continent via de afkorting van. <br />Geldige waarden zijn: <br />AF: Afrika<br />AS: Azië<br />EU: Europa<br />NA: Noord-Amerika<br />OC: Oceanië<br />SA: Zuid-Amerika<br /><br />**Afgeschafte:** % {virt_dst_continent}. <ber />Deze variabele is afgeschaft en vervangen door % {geo_continent}. <br />Hoewel een regel die gebruikmaakt van deze afgeschaft variabele werken blijven, moet u deze voor het gebruik van de nieuwe variabele bijwerken.| N/A |
| Cookiewaarde | %{cookie_Cookie} | Retourneert de waarde die overeenkomt met de cookiesleutel aangeduid met de term Cookie. | Voorbeeldgebruik: <br />%{cookie__utma}<br /><br />Voorbeeldwaarde:<br />111662281.2.10.1222100123 |
| Land (aanvrager) | %{geo_country} | Geeft aan van de aanvrager land van oorsprong via de landcode. <br />**Afgeschafte:** % {virt_dst_country}. <br /><br />Deze variabele is afgeschaft en vervangen door % {geo_country}. Hoewel een regel die gebruikmaakt van deze afgeschaft variabele werken blijven, moet u deze voor het gebruik van de nieuwe variabele bijwerken. | VS |
| Aangewezen marktgebied (aanvrager) | %{geo_dma_code} |Geeft aan van de aanvrager media markt door de regiocode. <br /><br />Dit veld is alleen van toepassing op aanvragen die afkomstig uit de Verenigde Staten zijn.| 745 |
| HTTP-aanvraagmethode | % {request_method} | Geeft aan dat de HTTP-aanvraagmethode. | GET |
| HTTP-statuscode | % {status} | Geeft aan dat de HTTP-statuscode voor het antwoord. | 200 |
| IP-adres (aanvrager) | %{virt_dst_addr} | Geeft aan dat de IP-adres van de aanvrager. | 192.168.1.1 |
| Breedte (aanvrager) | %{geo_latitude} | Geeft aan dat de breedtegraad van de aanvrager. | 34.0995 |
| Lengtegraad (aanvrager) | %{geo_longitude} | Geeft aan dat de lengtegraad van de aanvrager. | -118.4143 |
| Statistische gebied rond Tokio (aanvrager) | %{geo_metro_code} | Geeft aan dat gebied rond Tokio van de aanvrager. <br /><br />Dit veld is alleen van toepassing op aanvragen die afkomstig uit de Verenigde Staten zijn.<br />| 745 |
| Poort (aanvrager) | %{virt_dst_port} | Geeft aan dat de tijdelijke poort van de aanvrager. | 55885 |
| Postcode (aanvrager) | %{geo_postal_code} | Geeft aan dat de postcode van de aanvrager. | 90210 |
| Querytekenreeks gevonden | % {is_args} | De waarde voor deze variabele is afhankelijk van of de aanvraag een queryreeks bevat.<br /><br />-Querytekenreeks gevonden:?<br />-Er is geen querytekenreeks: NULL | ? |
| Queryreeks-Parameter gevonden | %{is_amp} | De waarde voor deze variabele is afhankelijk van of de aanvraag ten minste één queryreeks-parameter bevat.<br /><br />-De parameter die is gevonden: &<br />-Er zijn geen Parameters: NULL | & |
| Waarde voor Parameter de QueryString | %{arg_&lt;parameter&gt;} | Retourneert de waarde die overeenkomt met de queryreeks-parameter geïdentificeerd door de &lt;parameter&gt; term. | Voorbeeldgebruik: <br />%{arg_language}<br /><br />Voorbeeld van queryreeks-Parameter: <br />?language=en<br /><br />Voorbeeld van een waarde: nl |
| Waarde voor de QueryString | %{query_string} | Geeft aan dat de hele queryreekswaarde gedefinieerd in de aanvraag-URL. |key1=val1&key2=val2&key3=val3 |
| Verwijzende domein | %{referring_domain} | Geeft aan dat het domein dat is gedefinieerd in de aanvraagheader verwijzende site. | www.google.com |
| Regio (aanvrager) | %{geo_region} | Geeft aan van de aanvrager regio (bijvoorbeeld staat of provincie) via de afkorting van alfanumerieke tekens. | CA |
| Waarde van aanvraagheader | %{http_RequestHeader} | Retourneert de waarde die overeenkomt met de aanvraagheader geïdentificeerd door de term RequestHeader. <br /><br />Als de naam van de header van de aanvraag bevat een streepje (bijvoorbeeld gebruiker-Agent), vervangt u deze met een onderstrepingsteken (bijvoorbeeld User_Agent).| Voorbeeldgebruik: % {http_Connection}<br /><br />Voorbeeldwaarde: Keep-Alive | 
| Host van aanvraag | % {host} | Geeft aan dat de host die is gedefinieerd in de aanvraag-URL. | www.mydomain.com |
| Aanvraagprotocol | %{request_protocol} | Geeft aan dat de aanvraagprotocol. | HTTP/1.1 |
| Aanvraag-schema | % {scheme} | Geeft aan dat het schema van de aanvraag. |http |
| Aanvraag-URI (relatief) | %{request_uri} | Geeft het relatieve pad, met inbegrip van de query-tekenreeks, gedefinieerd in de aanvraag-URI. | /marketing/foo.js?loggedin=true |
| Aanvraag-URI (relatief zonder query-tekenreeks) | % {uri} | Geeft aan dat het relatieve pad naar de aangevraagde inhoud. <br /><br/>Belangrijke informatie:<br />-Dit relatieve pad niet van toepassing op de query-tekenreeks.<br />-Dit relatieve pad weerspiegelt URL regeneraties. Een URL wordt worden herschreven onder de volgende voorwaarden:<br />  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-Herschrijf de functie URL: Deze functie herschrijft het relatieve pad dat is gedefinieerd in de aanvraag-URI.<br />    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-URL van de CNAME edge: Dit type aanvraag opnieuw wordt geschreven naar de bijbehorende CDN-URL. |/800001/corigin/rewrittendir/foo.js |
| Aanvraag-URI | % {aanvraag} | Hierin wordt beschreven in de aanvraag. <br />Syntaxis: &lt;HTTP-methode&gt; &lt;relatief pad&gt; &lt;HTTP-protocol&gt; | /Marketing/foo.js?loggedin=true HTTP/1.1 ophalen |
| Antwoord-Header-waarde | % {resp_&lt;ResponseHeader&gt;} | Retourneert de waarde die overeenkomt met de reactieheader geïdentificeerd door de &lt;ResponseHeader&gt; term. <br /><br />Als de naam van de response-header bevat een streepje (bijvoorbeeld gebruiker-Agent), vervangt u deze met een onderstrepingsteken (bijvoorbeeld User_Agent). | Voorbeeldgebruik: % {resp_Content_Length}<br /><br />Voorbeeldwaarde: 100 |

## <a name="usage"></a>Gebruik
De volgende tabel beschrijft de juiste syntaxis voor het opgeven van een HTTP-variabele.


| Syntaxis | Voorbeeld | Description |
| ------ | -------- | ---------- |
| %{&lt;HTTPVariable&gt;} | % {host} | Gebruik de volgende syntaxis om de gehele waarde die overeenkomt met de opgegeven &lt;HTTPVariable&gt;. |
| %{&lt;HTTPVariableDelimiter&gt;} | % {host} | Gebruik deze syntaxis voor het instellen van de aanvraag voor de gehele waarde die overeenkomt met de opgegeven &lt;HTTPVariableDelimiter&gt;. |
| %{&lt;HTTPVariableDelimiterExpression&gt;} | %{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$2.$3:80} | Gebruik een reguliere expressie voor &lt;HTTPVariableDelimiterExpression&gt; als u wilt vervangen, verwijderen of bewerken van de waarde van een HTTP-variabele. |

Namen van variabelen HTTP ondersteunen alleen alfabetische tekens en onderstrepingstekens bevatten. Niet-ondersteunde tekens converteren naar onderstrepingstekens bevatten.

## <a name="delimiter-reference"></a>Scheidingsteken-verwijzing
Een scheidingsteken kan worden opgegeven nadat een HTTP-variabele aan het bereiken van de volgende gevolgen:

- Transformeer de waarde die is gekoppeld aan de variabele.

     Voorbeeld: De volledige waarde converteren naar kleine letters.

- De waarde die is gekoppeld aan de variabele verwijderen.

- De waarde die is gekoppeld aan de variabele te manipuleren.

     Voorbeeld: Reguliere expressies gebruiken om te wijzigen van de waarde die is gekoppeld aan de HTTP-variabele.

De scheidingstekens worden beschreven in de volgende tabel.

| Scheidingsteken | Description |
| --------- | ----------- |
| := | Geeft aan dat een standaardwaarde zal worden toegewezen aan de variabele wanneer het is: <br />-Ontbreekt <br />-Instellen op NULL. |
| :+ | Geeft aan dat een standaardwaarde zal worden toegewezen aan de variabele als een waarde is toegewezen aan deze. |
| : | Geeft aan dat een subtekenreeks van de waarde die is toegewezen aan de variabele zal worden uitgebreid. |
| # | Geeft aan dat het patroon dat is opgegeven na deze scheidingsteken moet worden verwijderd wanneer deze aan het begin van de waarde die is gekoppeld aan de variabele is aangetroffen. |
| % | Geeft aan dat het patroon dat is opgegeven na deze scheidingsteken moet worden verwijderd wanneer deze aan het einde van de waarde die is gekoppeld aan de variabele is aangetroffen. <br />Deze definitie is alleen van toepassing wanneer het symbool % wordt gebruikt als een scheidingsteken. |
| / | Een HTTP-variabele of een patroon begrenst. |
| // | Zoeken en vervangen van alle exemplaren van het opgegeven patroon. |
| /= | Zoeken, kopiëren en opnieuw schrijven van alle instanties van het opgegeven patroon. |
| ,  | De waarde die is gekoppeld aan de variabele HTTP naar kleine letters geconverteerd. |
| ^ | De waarde die is gekoppeld aan de HTTP-variabele in hoofdletters omzetten. |
| ,, | Converteert alle exemplaren van het opgegeven teken in de waarde die is gekoppeld aan de variabele HTTP naar kleine letters. |
| ^^ | Alle exemplaren van het opgegeven teken in de waarde die is gekoppeld aan de HTTP-variabele in hoofdletters worden geconverteerd. |

## <a name="exceptions"></a>Uitzonderingen
De volgende tabel beschrijft de omstandigheden waaronder de opgegeven tekst wordt niet beschouwd als een HTTP-variabele.

| Voorwaarde | Description | Voorbeeld |
| --------- | ----------- | --------|
| Aanhalingstekens % symbool | Het percentagesymbool kan worden voorafgegaan door het gebruik van een backslash. <br />De Voorbeeldwaarde aan de rechterkant wordt beschouwd als een letterlijke waarde en niet als een HTTP-variabele.| \%{host} |
| Onbekende variabelen | Een lege tekenreeks geretourneerd altijd voor onbekende variabelen. | % {unknownvariable} |
| Ongeldige tekens of syntaxis | Variabelen die ongeldige tekens of syntaxis bevatten worden behandeld als letterlijke waarden. <br /><br /># 1 De opgegeven waarde bevat een ongeldig teken (bijvoorbeeld-). <br /><br />Voorbeeld #2: De opgegeven waarde bevat een dubbele set accolades. <br /><br />Voorbeeld #3: De opgegeven waarde is een sluitaccolade ontbreekt.<br /> | Voorbeeld #1: % {resp_user-agent} <br /><br />Voorbeeld #2: % {{host}} <br /><br />Voorbeeld #3: % {host |
| De naam van variabele ontbreekt | Een NULL-waarde wordt altijd geretourneerd wanneer een variabele is niet opgegeven. | %{} |
| Volgtekens | Tekens die doorlopen van een variabele worden behandeld als letterlijke waarden. <br />De Voorbeeldwaarde aan de rechterkant bevat een afsluitende accolade die zullen worden behandeld als een letterlijke waarde. | % {host}} |

## <a name="setting-default-header-values"></a>Instellen van standaardwaarden koptekst
Een standaardwaarde kan worden toegewezen aan een koptekst, als deze voldoet aan een van de volgende voorwaarden:
- Missing/unset
- Ingesteld op NULL.

De volgende tabel wordt beschreven hoe u een standaardwaarde definiëren.

| Voorwaarde | Syntaxis | Voorbeeld | Description |
| --------- | ------ | --------| ----------- |
| Een header ingesteld op een standaardwaarde wanneer deze voldoet aan een van de volgende voorwaarden: <br /><br />-Koptekst ontbreekt <br /><br />-Header-waarde is ingesteld op NULL.| %{Variable:=Value} | %{http_referer:=unspecified} | De verwijzende koptekst wordt alleen worden ingesteld op *niet nader omschreven* wanneer het zich in ontbreekt of is ingesteld op NULL. Er is geen actie vindt plaats als deze eenmaal is ingesteld. |
| Een header ingesteld op een standaardwaarde als deze ontbreekt. | %{Variable=Value} | %{http_referer=unspecified} | De verwijzende koptekst wordt alleen worden ingesteld op *niet nader omschreven* als deze ontbreekt. Er is geen actie vindt plaats als deze eenmaal is ingesteld. |
| De header ingesteld op een standaardwaarde wanneer deze niet voldoet aan een van de volgende voorwaarden: <br /><br />-Ontbreekt<br /><br /> -Instellen op NULL. | % {Variabele: + waarde} | %{http_referer:+unspecified} | De verwijzende koptekst wordt alleen worden ingesteld op *niet nader omschreven* wanneer een waarde is toegewezen aan deze. Er is geen actie vindt plaats als deze ontbreekt of is ingesteld op NULL. |

## <a name="manipulating-variables"></a>Variabelen bewerken
Variabelen kunnen worden bewerkt in de volgende manieren:

- Subtekenreeksen uitbreiden
- Verwijderen van patronen

### <a name="substring-expansion"></a>De subtekenreeks-uitbreiding
Standaard wordt een variabele uitbreiden naar de volledige waarde. Gebruik de volgende syntaxis om uit te breiden alleen een subtekenreeks van de waarde van de variabele.

`%<Variable>:<Offset>:<Length>}`

Belangrijke informatie:

- De waarde die is toegewezen aan de term Offset bepaalt het eerste teken van de subtekenreeks:

     - Positieve: Het eerste teken van de subtekenreeks wordt berekend op basis van het eerste teken in de tekenreeks.
     - Zero: Het eerste teken van de subtekenreeks is het eerste teken in de tekenreeks.
     - Negatieve: Het eerste teken van de subtekenreeks wordt berekend op basis van het laatste teken in de tekenreeks.

- De lengte van de subtekenreeks wordt bepaald door de *lengte* term:

     - Weggelaten: Als de lengte van de termijn kunt de subtekenreeks om op te nemen van alle tekens tussen het eerste teken en het einde van de tekenreeks.
     - Positieve: Bepaalt de lengte van de subtekenreeks uit het eerste teken naar rechts.
     - Negatieve: Bepaalt de lengte van de subtekenreeks uit het eerste teken naar links.

#### <a name="example"></a>Voorbeeld:

Het volgende voorbeeld is afhankelijk van het volgende voorbeeld aanvraag-URL:

https:\//cdn.mydomain.com/folder/marketing/myconsultant/proposal.html

De volgende tekenreeks ziet u verschillende methoden voor het werken met variabelen:

https:\//www%{http_host:3}/mobile/%{request_uri:7:10}/%{request_uri:-5:-8}.htm

Op basis van de voorbeeld-aanvraag-URL, maken de bovenstaande variabele manipulatie de volgende waarde:

https:\//www.mydomain.com/mobile/marketing/proposal.htm


### <a name="pattern-removal"></a>Verwijderen van het patroon
Tekst die overeenkomt met een specifieke patroon kan worden verwijderd uit het begin of het einde van de waarde van een variabele.

| Syntaxis | Bewerking |
| ------ | ------ |
| %{Variable#Pattern} | Verwijder tekst als het opgegeven patroon wordt gevonden aan het begin van de waarde van een variabele. |
| %{Variable%Pattern} | Verwijder tekst als het opgegeven patroon wordt gevonden aan het einde van de waarde van een variabele. |

#### <a name="example"></a>Voorbeeld:

In dit voorbeeldscenario; de *request_uri* variabele is ingesteld op:

`/800001/myorigin/marketing/product.html?language=en-US`

De volgende tabel laat zien hoe deze syntaxis werkt.

| Voorbeeldsyntaxis | Resultaten |
| ------------- | ------- |
| %{request_uri#/800001}/customerorigin | /customerorigin/myorigin/marketing/product.html?language=en-US | Omdat de variabele wordt gestart met het patroon, is het vervangen. |
| %{request_uri%html}htm | /800001/myorigin/marketing/product.html?language=en-US | Omdat de variabele niet met het patroon eindigt, is er geen wijziging.|

### <a name="find-and-replace"></a>Zoeken en vervangen
De syntaxis van zoeken en vervangen wordt in de volgende tabel beschreven.

| Syntaxis | Bewerking |
| ------ | ------ |
| %{Variable/Find/Replace} | Zoeken en vervangen eerste exemplaar van het opgegeven patroon. |
| %{Variable//Find/Replace} | Zoeken en vervangen van alle instanties van het opgegeven patroon. |
| %{Variable^} |De volledige waarde converteren naar hoofdletters. |
| %{Variable^Find} | Het eerste exemplaar van het opgegeven patroon converteren naar hoofdletters. |
| % {Variabele} | De volledige waarde converteren naar kleine letters. |
| % {Variabele, zoeken naar} | Het eerste exemplaar van het opgegeven patroon converteren naar kleine letters. |

### <a name="find-and-rewrite"></a>Zoeken en schrijven
Voor een variant van zoeken en vervangen, gebruikt u de tekst die overeenkomt met het opgegeven patroon wanneer opnieuw te schrijven. De syntaxis van de zoek- en herschrijven wordt in de volgende tabel beschreven.

| Syntaxis | Bewerking |
| ------ | ------ |
| %{Variable/=Find/Rewrite} | Zoeken, kopiëren en opnieuw schrijven van alle instanties van het opgegeven patroon. |
| %{Variable/^Find/Rewrite} | Zoeken, kopiëren en opnieuw schrijven van het opgegeven patroon wanneer hiervan aan het begin van de variabele. |
| %{Variable/$Find/Rewrite} | Zoeken, kopiëren en opnieuw schrijven van het opgegeven patroon wanneer hiervan aan het einde van de variabele. |
| %{Variable/Find} | Zoeken en verwijderen van alle instanties van het opgegeven patroon. |

Belangrijke informatie:

- Vouw de tekst die overeenkomt met het opgegeven patroon door een dollarteken gevolgd door een hele geheel getal zijn (bijvoorbeeld $1) op te geven.

- Meerdere patronen kunnen worden opgegeven. De volgorde waarin het patroon is opgegeven, bepaalt het gehele getal dat wordt toegewezen aan deze. In het volgende voorbeeld wordt de eerste patroon komt overeen met 'www-.,' de tweede patroon overeenkomt met het domein op het tweede niveau en het derde patroon overeenkomt met het domein op het hoogste niveau:

    `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$2.$3:80}`

- De rewritten waarde kan bestaan uit een combinatie van tekst en deze tijdelijke aanduidingen.

    In het vorige voorbeeld wordt de hostnaam herschreven naar `cdn.$2.$3:80` (bijvoorbeeld cdn.mydomain.com:80).

- Het geval van een patroon tijdelijke aanduiding (bijvoorbeeld $1) kan worden gewijzigd in de volgende vlaggen:
     - U: De uitgebreide waarde in hoofdletters.

         Van de voorbeeldsyntaxis:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$U2.$3:80}`

     - L: Kleine letters de uitgevouwen waarde.

         Van de voorbeeldsyntaxis:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$L2.$3:80}`

- Een operator moet worden opgegeven voordat het patroon. De opgegeven operator bepaalt het gedrag voor het vastleggen van een patroon:

     - `=`: Geeft aan dat alle instanties van het opgegeven patroon moeten worden vastgelegd en herschreven.
     - `^`: Geeft aan dat alleen de tekst die met het opgegeven patroon begint wordt vastgelegd.
     - `$`: Geeft aan dat alleen tekst die met het opgegeven patroon eindigt vastleggen zal zijn.
 
- Als u weglaat de */herschrijven* waarde, de tekst die overeenkomt met het patroon wordt verwijderd.


