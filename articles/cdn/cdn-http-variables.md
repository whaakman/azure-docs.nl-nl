---
title: Variabelen voor Azure CDN regelengine HTTP | Microsoft Docs
description: HTTP-variabelen kunt u voor het ophalen van metagegevens van HTTP-aanvraag en -antwoord.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: v-deasim
ms.openlocfilehash: ea7469b1d1c3d1c20beca9b1fb3bef0d4dac9492
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="http-variables-for-azure-cdn-rules-engine"></a>HTTP-variabelen voor de engine van Azure CDN-regels
HTTP-variabelen bieden de mogelijkheid waarmee u HTTP-aanvraag en -antwoord metagegevens kunt ophalen. Deze metagegevens kan vervolgens worden gebruikt voor het dynamisch een aanvraag of antwoord wijzigen. Het gebruik van HTTP-variabelen is beperkt tot de volgende regels engine functies:

- [Cache-sleutel opnieuw schrijven](cdn-rules-engine-reference-features.md#cache-key-rewrite)
- [De aanvraagheader Client wijzigen](cdn-rules-engine-reference-features.md#modify-client-request-header)
- [Client-antwoordheader wijzigen](cdn-rules-engine-reference-features.md#modify-client-response-header)
- [URL-omleiding](cdn-rules-engine-reference-features.md#url-redirect)
- [Herschrijven van URL](cdn-rules-engine-reference-features.md#url-rewrite)

## <a name="definitions"></a>Definities
De volgende tabel beschrijft de ondersteunde HTTP-variabelen. Een lege waarde wordt geretourneerd wanneer GEO metagegevens (bijvoorbeeld postcode) niet beschikbaar voor een bepaald verzoek is.


| Naam | Variabele | Beschrijving | Voorbeeldwaarde |
| ---- | -------- | ----------- | ------------ |
| ASN (aanvrager) | % {geo_asnum} | Hiermee wordt aangegeven van de aanvrager AS-nummer. <br /><br />**Afgeschafte:** % {virt_dst_asnum}. <br />Deze variabele is vervangen door % {geo_asnum}. Hoewel een regel die gebruikmaakt van deze afgeschaft variabele werken blijven, moet u het gebruik van de nieuwe variabele bijwerken. | AS15133 |
| Plaats (aanvrager) | % {geo_city} | Hiermee wordt aangegeven in plaats van de aanvrager. | Los Angeles |
| Continent (aanvrager) | % {geo_continent} | Hiermee wordt aangegeven van de aanvrager continent via de afkorting ervan. <br />Geldige waarden zijn: <br />AF: Afrika<br />AS: Azië<br />EU: Europa<br />N.V.T.: Noord-Amerika<br />Overheadkosten: Oceanië<br />SA: Zuid-Amerika<br /><br />**Afgeschafte:** % {virt_dst_continent}. <ber />Deze variabele is vervangen door % {geo_continent}. <br />Hoewel een regel die gebruikmaakt van deze afgeschaft variabele werken blijven, moet u het gebruik van de nieuwe variabele bijwerken.| N/A |
| Cookiewaarde | % {cookie_Cookie} | Retourneert de waarde die overeenkomt met de cookie-sleutel die is geïdentificeerd door de term Cookie. | Voorbeeldgebruik: <br />% {cookie__utma}<br /><br />Voorbeeldwaarde:<br />111662281.2.10.1222100123 |
| Land (aanvrager) | % {geo_country} | Hiermee wordt aangegeven van de aanvrager land van oorsprong via de landcode. <br />**Afgeschafte:** % {virt_dst_country}. <br /><br />Deze variabele is vervangen door % {geo_country}. Hoewel een regel die gebruikmaakt van deze afgeschaft variabele werken blijven, moet u het gebruik van de nieuwe variabele bijwerken. | VS |
| Aangewezen marktgebied (aanvrager) | % {geo_dma_code} |Hiermee wordt aangegeven van de aanvrager media markt aan de regiocode. <br /><br />Dit veld is alleen van toepassing op aanvragen die afkomstig van de Verenigde Staten zijn.| 745 |
| HTTP-aanvraagmethode | % {request_method} | Hiermee geeft u de HTTP-aanvraagmethode. | GET |
| HTTP-statuscode | % {status} | Hiermee wordt aangegeven in de HTTP-statuscode voor het antwoord. | 200 |
| IP-adres (aanvrager) | % {virt_dst_addr} | Hiermee wordt aangegeven van de aanvrager IP-adres. | 192.168.1.1 |
| Breedtegraad (aanvrager) | % {geo_latitude} | Geeft de breedte van de aanvrager. | 34.0995 |
| Lengtegraad (aanvrager) | % {geo_longitude} | Hiermee wordt aangegeven van de aanvrager lengtegraad. | -118.4143 |
| Grootstedelijke statistische gebied (aanvrager) | % {geo_metro_code} | Geeft het grootstedelijke gebied van de aanvrager. <br /><br />Dit veld is alleen van toepassing op aanvragen die afkomstig van de Verenigde Staten zijn.<br />| 745 |
| Poort (aanvrager) | % {virt_dst_port} | Hiermee wordt aangegeven van de aanvrager tijdelijke poort. | 55885 |
| Postcode (aanvrager) | % {geo_postal_code} | Hiermee wordt aangegeven van de aanvrager postcode. | 90210 |
| Queryreeks gevonden | % {is_args} | De waarde voor deze variabele is afhankelijk van of de aanvraag een queryreeks bevat.<br /><br />-Querytekenreeks gevonden:?<br />-Er is geen querytekenreeks: NULL | ? |
| Querytekenreeksparameter gevonden | % {is_amp} | De waarde voor deze variabele is afhankelijk van of de aanvraag ten minste één queryreeksparameter bevat.<br /><br />-De parameter die is gevonden: &<br />-Er zijn geen Parameters: NULL | & |
| De waarde voor Parameter queryreeks | % {arg_&lt;parameter&gt;} | Retourneert de waarde die overeenkomt met de querytekenreeksparameter geïdentificeerd door de &lt;parameter&gt; term. | Voorbeeldgebruik: <br />% {arg_language}<br /><br />Voorbeeld queryreeksparameter opgeven: <br />? taal = nl<br /><br />Voorbeeld van de waarde: nl |
| De waarde van de query-tekenreeks | % {query_string} | Hiermee wordt aangegeven in de aanvraag-URL opgegeven waarde voor de hele queryreeks. |Key1 = waarde1 & key2 = waarde2 & key3 val3 = |
| Domein van verwijzende site | % {referring_domain} | Hiermee geeft u het domein dat is gedefinieerd in de aanvraagheader verwijzende site. | www.google.com |
| Regio (aanvrager) | % {geo_region} | Hiermee wordt aangegeven van de aanvrager regio (bijvoorbeeld staat of provincie) via de alfanumerieke afkorting ervan. | CA |
| Waarde van de aanvraag-Header | % {http_RequestHeader} | Retourneert de waarde die overeenkomt met de aanvraagheader geïdentificeerd door de term RequestHeader. <br /><br />Als de naam van de aanvraagheader bevat een streepje (bijvoorbeeld gebruiker-Agent), kunt u deze met een onderstrepingsteken (bijvoorbeeld User_Agent) vervangen.| Voorbeeldgebruik: % {http_Connection}<br /><br />Voorbeeldwaarde: Keepalive | 
| Aanvraag-Host | % {host} | Hiermee geeft u de host die is gedefinieerd in de aanvraag-URL. | www.mydomain.com |
| Aanvraagprotocol | % {request_protocol} | Geeft de aanvraag. | HTTP/1.1 |
| Aanvraag-schema | % {schema} | Hiermee geeft u het schema van de aanvraag. |http |
| Aanvraag-URI (relatieve) | % {request_uri} | Hiermee geeft u het relatieve pad, met inbegrip van de querytekenreeks is gedefinieerd in de aanvraag-URI. | /Marketing/foo.js?loggedin=True |
| Aanvraag-URI (relatieve zonder query-tekenreeks) | % {uri} | Hiermee geeft u het relatieve pad naar de aangevraagde inhoud. <br /><br/>Belangrijke informatie:<br />-Deze relatieve pad worden uitgesloten van de queryreeks.<br />-Deze relatieve pad weerspiegelt URL regeneraties. Een URL herschreven onder de volgende voorwaarden:<br />  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-Functie voor het herschrijven van URL: Deze functie herschrijft het relatieve pad dat is gedefinieerd in de aanvraag-URI.<br />    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-URL van de CNAME edge: Dit type aanvraag wordt herschreven voor de bijbehorende CDN-URL. |/800001/corigin/rewrittendir/foo.js |
| Aanvraag-URI | % {aanvraag} | Hierin wordt beschreven in de aanvraag. <br />Syntaxis: `HTTPMethod RelativePath Protocol` | /Marketing/foo.js?loggedin=true HTTP/1.1 ophalen |
| Antwoord headerwaarde | % {resp_&lt;ResponseHeader&gt;} | Retourneert de waarde die overeenkomt met de antwoordheader geïdentificeerd door de &lt;ResponseHeader&rt; term. <br /><br />Als de naam van de antwoordheader bevat een streepje (bijvoorbeeld gebruiker-Agent), kunt u deze met een onderstrepingsteken (bijvoorbeeld User_Agent) vervangen. | Voorbeeldgebruik: % {resp_Content_Length}<br /><br />Voorbeeld van de waarde: 100 |

## <a name="usage"></a>Gebruik
De volgende tabel beschrijft de juiste syntaxis voor het opgeven van een HTTP-variabele.


| Syntaxis | Voorbeeld | Beschrijving |
| ------ | -------- | ---------- |
| %{&lt;HTTPVariable&gt;} | % {host} | Gebruik de volgende syntaxis om de gehele waarde die overeenkomt met het opgegeven &lt;HTTPVariable&gt;. |
| %{&lt;HTTPVariableDelimiter&gt;} | % {host} | Gebruik deze syntaxis voor het instellen van de aanvraag voor de gehele waarde die overeenkomt met het opgegeven &lt;HTTPVariableDelimiter&gt;. |
| %{&lt;HTTPVariableDelimiterExpression&gt;} | %{Host/=^www\.([^\.] +)\.([^\.:] +) /cdn.$2.$3:80} | Gebruik een reguliere expressie voor &lt;HTTPVariableDelimiterExpression&gt; als u wilt vervangen, verwijderen of de waarde van een HTTP-variabele te manipuleren. |

Namen van variabelen HTTP ondersteunen alleen alfabetische tekens en onderstrepingstekens bevatten. Niet-ondersteunde tekens niet converteren naar onderstrepingstekens bevatten.

## <a name="delimiter-reference"></a>Scheidingsteken-verwijzing
Een scheidingsteken kan worden opgegeven na een HTTP-variabele voor het bereiken van de volgende gevolgen:

- De waarde die is gekoppeld aan de variabele worden getransformeerd.

     Voorbeeld: De gehele waarde converteren naar kleine letters.

- De waarde die is gekoppeld aan de variabele verwijderen.

- De waarde die is gekoppeld aan de variabele instellen.

     Voorbeeld: Gebruik reguliere expressies de waarde die is gekoppeld aan de HTTP-variabele te wijzigen.

De scheidingstekens worden in de volgende tabel beschreven.

| Scheidingsteken | Beschrijving |
| --------- | ----------- |
| := | Hiermee wordt aangegeven dat een standaardwaarde zal worden toegewezen aan de variabele wanneer het is: <br />-Ontbreekt <br />-Instellen op NULL. |
| :+ | Hiermee wordt aangegeven dat een standaardwaarde zal worden toegewezen aan de variabele wanneer er een waarde aan is toegewezen. |
| : | Hiermee wordt aangegeven dat een subtekenreeks van de waarde die is toegewezen aan de variabele zal worden uitgebreid. |
| # | Hiermee wordt aangegeven dat het patroon opgegeven na deze scheidingsteken moet worden verwijderd wanneer deze aan het begin van de waarde die is gekoppeld aan de variabele kan worden gevonden. |
| % | Hiermee wordt aangegeven dat het patroon opgegeven na deze scheidingsteken moet worden verwijderd wanneer deze aan het einde van de waarde die is gekoppeld aan de variabele kan worden gevonden. <br />Deze definitie is alleen van toepassing als het symbool % wordt gebruikt als een scheidingsteken. |
| / | Een HTTP-variabele of een patroon begrenst. |
| // | Zoeken en vervangen van alle exemplaren van het opgegeven patroon. |
| /= | Zoeken, kopiëren en herschrijven van alle instanties van het opgegeven patroon. |
| , | De waarde die is gekoppeld aan de HTTP-variabele in kleine letters worden geconverteerd. |
| ^ | De waarde die is gekoppeld aan de HTTP-variabele in hoofdletters worden geconverteerd. |
| ,, | Converteren van alle exemplaren van het opgegeven teken in de waarde die is gekoppeld aan de HTTP-variabele in kleine letters. |
| ^^ | Alle exemplaren van het opgegeven teken in de waarde die is gekoppeld aan de HTTP-variabele in hoofdletters worden geconverteerd. |

## <a name="exceptions"></a>Uitzonderingen
De volgende tabel beschrijft de omstandigheden waaronder de opgegeven tekst wordt niet beschouwd als een HTTP-variabele.

| Voorwaarde | Beschrijving | Voorbeeld |
| --------- | ----------- | --------|
| Aanhalingstekens symbool % | Het percentagesymbool kan worden voorafgegaan door het gebruik van een backslash. <br />De Voorbeeldwaarde naar rechts beschouwd als een letterlijke waarde en niet als een HTTP-variabele.| \%{host} |
| Onbekende variabelen | Een lege tekenreeks geretourneerd altijd voor onbekende variabelen. | % {unknownvariable} |
| Ongeldige tekens of syntaxis | Variabelen die ongeldige tekens of syntaxis bevatten worden behandeld als letterlijke waarden. <br /><br />Voorbeeld #1: De opgegeven waarde bevat een ongeldig teken (bijvoorbeeld-). <br /><br />Voorbeeld #2: De opgegeven waarde bevat een dubbele reeks accolades. <br /><br />Voorbeeld #3: De opgegeven waarde ontbreekt een sluitaccolade.<br /> | Voorbeeld #1: % {resp_user-agent} <br /><br />Bijvoorbeeld #2: % {{host}} <br /><br />Voorbeeld #3: % {host |
| Variabele naam ontbreekt | Een NULL-waarde wordt altijd geretourneerd wanneer een variabele is niet opgegeven. | %{} |
| Volgtekens | Tekens die de audittrail een variabele worden behandeld als letterlijke waarden. <br />De Voorbeeldwaarde naar rechts bevat een afsluitende accolade die zal worden behandeld als een letterlijke waarde. | % {host}} |

## <a name="setting-default-header-values"></a>Standaardwaarden voor de koptekst instellen
Een standaardwaarde kan worden toegewezen aan een koptekst als deze voldoet aan een van de volgende voorwaarden:
- Ontbrekende/uitschakelen
- Ingesteld op NULL.

De volgende tabel beschrijft hoe een standaardwaarde definiëren.

| Voorwaarde | Syntaxis | Voorbeeld | Beschrijving |
| --------- | ------ | --------| ----------- |
| Een koptekst ingesteld op een standaardwaarde als deze voldoet aan een van de volgende voorwaarden: <br /><br />-Koptekst ontbreekt <br /><br />-Header-waarde is ingesteld op NULL.| % {Variabele: = waarde} | % {http_referer: = niet-gespecificeerde} | De header verwijzende site wordt alleen worden ingesteld op *niet nader omschreven* wanneer het zich ontbreekt of is ingesteld op NULL. Er is geen actie worden uitgevoerd als deze eenmaal is ingesteld. |
| Een koptekst ingesteld op een standaardwaarde wanneer deze ontbreekt. | % {Variabele = waarde} | % {http_referer = niet-gespecificeerde} | De header verwijzende site wordt alleen worden ingesteld op *niet nader omschreven* wanneer deze ontbreekt. Er is geen actie worden uitgevoerd als deze eenmaal is ingesteld. |
| De header ingesteld op een standaardwaarde wanneer deze niet voldoet aan een van de volgende voorwaarden: <br /><br />-Ontbreekt<br /><br /> -Instellen op NULL. | % {Variabele: + waarde} | % {http_referer: + niet nader omschreven} | De header verwijzende site wordt alleen worden ingesteld op *niet nader omschreven* wanneer een waarde is toegewezen aan deze. Er is geen actie worden uitgevoerd als deze ontbreekt of is ingesteld op NULL. |

## <a name="manipulating-variables"></a>Variabelen bewerken
Variabelen kunnen worden bewerkt in de volgende manieren:

- Uitbreidbare subtekenreeksen
- Verwijderen van patronen

### <a name="substring-expansion"></a>Subtekenreeks-uitbreiding
Standaard wordt een variabele uitbreiden naar de volledige waarde. Gebruik de volgende syntaxis om uit te breiden alleen een subtekenreeks van de variabele waarde.

`%<Variable>:<Offset>:<Length>}`

Belangrijke informatie:

- De waarde die is toegewezen aan de term Offset bepaalt het eerste teken van de subtekenreeks:

     - Positief: Het eerste teken van de subtekenreeks wordt berekend op basis van het eerste teken in de tekenreeks.
     - Nul: Het eerste teken van de subtekenreeks is het eerste teken in de tekenreeks.
     - Negatieve: Het eerste teken van de subtekenreeks wordt berekend op basis van het laatste teken in de tekenreeks.

- De lengte van de subtekenreeks wordt bepaald door de *lengte* term:

     - Weggelaten: Als de term lengte weggelaten kunt de subtekenreeks om op te nemen van alle tekens tussen het eerste teken en het einde van de tekenreeks.
     - Positief: Bepaalt de lengte van de subtekenreeks in het eerste teken naar rechts.
     - Negatieve: Bepaalt de lengte van de subtekenreeks in het eerste teken aan de linkerkant.

#### <a name="examples"></a>Voorbeelden:

Het volgende voorbeeld is afhankelijk van de volgende voorbeeldquery aanvraag-URL:

`https://cdn.mydomain.com/folder/marketing/myconsultant/proposal.html`

De volgende tekenreeks wordt getoond hoe verschillende methoden voor het manipuleren van variabelen:

`https://www%{http_host:3}/mobile/%{request_uri:7:10}/%{request_uri:-5:-8}.htm`

Op basis van de voorbeeld-aanvraag-URL, produceren de bovenstaande variabele manipulatie de volgende waarde:

`https://www.mydomain.com/mobile/marketing/proposal.htm`


### <a name="pattern-removal"></a>Verwijderen van het patroon
Tekst die overeenkomt met een specifiek patroon kan vanaf het begin of het einde van een variabele waarde worden verwijderd.

| Syntaxis | Bewerking |
| ------ | ------ |
| % {Variabele #Pattern} | Verwijder de tekst als het opgegeven patroon wordt gevonden aan het begin van de waarde van een variabele. |
| % {Variabele % patroon} | Verwijder de tekst als het opgegeven patroon wordt gevonden aan het einde van de waarde van een variabele. |

#### <a name="example"></a>Voorbeeld:

In dit voorbeeldscenario wordt de variabele request_uri ingesteld op:

`/800001/myorigin/marketing/product.html?language=en-US`

De volgende tabel laat zien hoe deze syntaxis werkt.

| De syntaxis van de steekproef | Resultaten |
| ------------- | ------- |
| %{request_uri#/800001}/customerorigin | /customerorigin/myorigin/marketing/product.HTML?Language=en-us | Omdat de variabele met het patroon begint, is het vervangen. |
| htm % {request_uri % html} | /800001/myorigin/marketing/product.HTML?Language=en-us | Omdat de variabele niet met het patroon eindigt, moet u er geen wijziging is.|

### <a name="find-and-replace"></a>Zoeken en vervangen
De syntaxis van zoeken en vervangen wordt in de volgende tabel beschreven.

| Syntaxis | Bewerking |
| ------ | ------ |
| %{Variable/Find/Replace} | Zoeken en vervangen eerste exemplaar van het opgegeven patroon. |
| %{Variable//Find/Replace} | Zoeken en vervangen van alle instanties van het opgegeven patroon. |
| % {Variabele ^} |De volledige waarde converteren naar hoofdletters. |
| % {Variabele ^ vinden} | Het eerste exemplaar van het opgegeven patroon converteren naar hoofdletters. |
| % {Variabele} | De volledige waarde converteren naar kleine letters. |
| % {Variabele zoeken} | Het eerste exemplaar van het opgegeven patroon converteren naar kleine letters. |

### <a name="find-and-rewrite"></a>Zoeken en schrijven
Voor een variant van zoeken en vervangen, gebruikt u de tekst die overeenkomt met het opgegeven patroon wanneer opnieuw te schrijven. De syntaxis van zoeken en schrijven wordt beschreven in de volgende tabel.

| Syntaxis | Bewerking |
| ------ | ------ |
| %{Variable/=Find/Rewrite} | Zoeken, kopiëren en herschrijven van alle instanties van het opgegeven patroon. |
| %{Variable/^Find/Rewrite} | Zoeken, kopiëren en het opgegeven patroon herschrijven wanneer deze aan het begin van de variabele optreedt. |
| %{Variable/$Find/Rewrite} | Zoeken, kopiëren en het opgegeven patroon herschrijven wanneer deze aan het einde van de variabele optreedt. |
| %{Variable/Find} | Zoek en verwijder alle instanties van het opgegeven patroon. |

Belangrijke informatie:

- Vouw de tekst die overeenkomt met het opgegeven patroon door te geven van een dollarteken gevolgd door een gehele getal (bijvoorbeeld $1).

- Meerdere patronen kunnen worden opgegeven. De volgorde waarin het patroon is opgegeven, bepaalt het gehele getal dat wordt toegewezen aan deze. In het volgende voorbeeld wordt het eerste patroon overeenkomt met 'www-.,' het tweede patroon overeenkomt met het tweede niveau domein en het derde patroon overeenkomt met het domein op het hoogste niveau:

    `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$2.$3:80}`

- De waarde herschreven kan bestaan uit een combinatie van tekst en deze tijdelijke aanduidingen.

    In het vorige voorbeeld wordt de hostnaam van de herschreven naar `cdn.$2.$3:80` (bijvoorbeeld cdn.mydomain.com:80).

- Het geval van een patroon tijdelijke aanduiding (bijvoorbeeld $1) kan worden gewijzigd via de volgende vlaggen:
     - U: de uitgevouwen waarde hoofdletters.

         Voorbeeldsyntaxis:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$U2.$3:80}`

     - V: kleine de uitgevouwen waarde.

         Voorbeeldsyntaxis:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$L2.$3:80}`

- Een operator moet worden opgegeven voordat het patroon. De opgegeven operator bepaalt het gedrag voor het vastleggen van patroon:

     - `=`: Dit geeft aan dat alle instanties van het opgegeven patroon moeten worden vastgelegd en herschreven.
     - `^`: Dit geeft aan dat alleen de tekst die met het opgegeven patroon begint wordt vastgelegd.
     - `$`: Geeft aan dat alleen tekst die met het opgegeven patroon eindigt vastleggen.
 
- Als u weglaat de */herschrijven* waarde, de tekst die overeenkomt met het patroon wordt verwijderd.


