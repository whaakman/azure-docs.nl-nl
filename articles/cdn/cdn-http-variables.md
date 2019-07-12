---
title: HTTP-variabelen voor regels-engine van Azure CDN | Microsoft Docs
description: HTTP-variabelen kunnen u HTTP-aanvraag en respons metagegevens ophalen.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: magattus
ms.openlocfilehash: 53ad0c516547e17801bd57c2fd6b0d1704383797
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593821"
---
# <a name="http-variables-for-azure-cdn-rules-engine"></a>HTTP-variabelen voor regels-engine van Azure CDN
HTTP-variabelen bieden de mogelijkheid waarmee u HTTP-aanvraag en respons metagegevens kunt ophalen. Deze metagegevens kunt vervolgens worden gebruikt om een aanvraag of antwoord dynamisch wijzigen. Het gebruik van HTTP-variabelen is beperkt tot de volgende regels-engine-functies:

- [Cache-sleutel opnieuw schrijven](cdn-verizon-premium-rules-engine-reference-features.md#cache-key-rewrite)
- [Client-aanvraagheader wijzigen](cdn-verizon-premium-rules-engine-reference-features.md#modify-client-request-header)
- [Client-Reactieheader wijzigen](cdn-verizon-premium-rules-engine-reference-features.md#modify-client-response-header)
- [URL-omleiding](cdn-verizon-premium-rules-engine-reference-features.md#url-redirect)
- [Herschrijven van URL](cdn-verizon-premium-rules-engine-reference-features.md#url-rewrite)

## <a name="definitions"></a>Definities
De volgende tabel beschrijft de ondersteunde HTTP-variabelen. Een lege waarde wordt geretourneerd wanneer GEO-metagegevens (bijvoorbeeld postcode) niet beschikbaar voor een bepaalde aanvraag is.


| Name | Variabele | Description | Voorbeeldwaarde |
| ---- | -------- | ----------- | ------------ |
| ASN (aanvrager) | %{geo_asnum} | Geeft aan van de aanvrager als getal. <br /><br />**Afgeschafte:** % {virt_dst_asnum}. <br />Deze variabele is afgeschaft en vervangen door % {geo_asnum}. Hoewel een regel die gebruikmaakt van deze afgeschaft variabele werken blijven, moet u deze voor het gebruik van de nieuwe variabele bijwerken. | AS15133 |
| Plaats (aanvrager) | %{geo_city} | Hiermee wordt aangegeven in plaats van de aanvrager. | Los Angeles |
| Continent (aanvrager) | %{geo_continent} | Geeft aan van de aanvrager continent via de afkorting van. <br />Geldige waarden zijn: <br />AF: Afrika<br />AS: Azië<br />EU: Europa<br />NA: Noord-Amerika<br />OC: Oceanië<br />SA: Zuid-Amerika<br /><br />**Afgeschafte:** % {virt_dst_continent}. <br />Deze variabele is afgeschaft en vervangen door % {geo_continent}. <br />Hoewel een regel die gebruikmaakt van deze afgeschaft variabele werken blijven, moet u deze voor het gebruik van de nieuwe variabele bijwerken.| N/A |
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
| Verwijzende domein | %{referring_domain} | Geeft aan dat het domein dat is gedefinieerd in de aanvraagheader verwijzende site. | <www.google.com> |
| Regio (aanvrager) | %{geo_region} | Geeft aan van de aanvrager regio (bijvoorbeeld staat of provincie) via de afkorting van alfanumerieke tekens. | CA |
| Waarde van aanvraagheader | %{http_RequestHeader} | Retourneert de waarde die overeenkomt met de aanvraagheader geïdentificeerd door de term RequestHeader. <br /><br />Als de naam van de header van de aanvraag bevat een streepje (bijvoorbeeld gebruiker-Agent), vervangt u deze met een onderstrepingsteken (bijvoorbeeld User_Agent).| Voorbeeldgebruik: % {http_Connection}<br /><br />Voorbeeldwaarde: Keep-Alive | 
| Host van aanvraag | % {host} | Geeft aan dat de host die is gedefinieerd in de aanvraag-URL. | <www.mydomain.com> |
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
| , | De waarde die is gekoppeld aan de variabele HTTP naar kleine letters geconverteerd. |
| ^ | De waarde die is gekoppeld aan de HTTP-variabele in hoofdletters omzetten. |
| ,, | Converteert alle exemplaren van het opgegeven teken in de waarde die is gekoppeld aan de variabele HTTP naar kleine letters. |
| ^^ | Alle exemplaren van het opgegeven teken in de waarde die is gekoppeld aan de HTTP-variabele in hoofdletters worden geconverteerd. |

## <a name="exceptions"></a>Uitzonderingen
De volgende tabel beschrijft de omstandigheden waaronder de opgegeven tekst wordt niet beschouwd als een HTTP-variabele.

| Voorwaarde | Description | Voorbeeld |
| --------- | ----------- | --------|
| Aanhalingstekens % symbool | Het percentagesymbool kan worden voorafgegaan door het gebruik van een backslash. <br />De Voorbeeldwaarde aan de rechterkant wordt beschouwd als een letterlijke waarde en niet als een HTTP-variabele.| \%{host} |
| Unknown variables | An empty string is always returned for unknown variables. | %{unknown_variable} |
| Invalid characters or syntax | Variables that contain invalid characters or syntax are treated as literal values. <br /><br />Example #1: The specified value contains an invalid character (for example, -). <br /><br />Example #2: The specified value contains a double set of curly braces. <br /><br />Example #3: The specified value is missing a closing curly brace.<br /> | Example #1: %{resp_user-agent} <br /><br />Example #2: %{{host}} <br /><br />Example #3: %{host |
| Missing variable name | A NULL value is always returned when a variable is not specified. | %{} |
| Trailing characters | Characters that trail a variable are treated as literal values. <br />The sample value to the right contains a trailing curly brace that will be treated as a literal value. | %{host}} |

## Setting default header values
A default value can be assigned to a header when it meets any of the following conditions:
- Missing/unset
- Set to NULL.

The following table describes how to define a default value.

| Condition | Syntax | Example | Description |
| --------- | ------ | --------| ----------- |
| Set a header to a default value when it meets any of the following conditions: <br /><br />- Missing Header <br /><br />- Header value is set to NULL.| %{Variable:=Value} | %{http_referrer:=unspecified} | The Referrer header will only be set to *unspecified* when it is either missing or set to NULL. No action will take place if it has been set. |
| Set a header to a default value when it is missing. | %{Variable=Value} | %{http_referrer=unspecified} | The Referrer header will only be set to *unspecified* when it is missing. No action will take place if it has been set. |
| Set the header to a default value when it does not meet any of the following conditions: <br /><br />- Missing<br /><br /> - Set to NULL. | %{Variable:+Value} | %{http_referrer:+unspecified} | The Referrer header will only be set to *unspecified* when a value has been assigned to it. No action will take place if it is either missing or set to NULL. |

## Manipulating variables
Variables can be manipulated in the following ways:

- Expanding substrings
- Removing patterns

### Substring expansion
By default, a variable will expand to its full value. Use the following syntax to only expand a substring of the variable's value.

`%<Variable>:<Offset>:<Length>}`

Key information:

- The value assigned to the Offset term determines the starting character of the substring:

     - Positive: The starting character of the substring is calculated from the first character in the string.
     - Zero: The starting character of the substring is the first character in the string.
     - Negative: The starting character of the substring is calculated from the last character in the string.

- The length of the substring is determined by the *Length* term:

     - Omitted: Omitting the Length term allows the substring to include all characters between the starting character and the end of the string.
     - Positive: Determines the length of the substring from the starting character to the right.
     - Negative: Determines the length of the substring from the starting character to the left.

#### Example:

The following example relies on the following sample request URL:

https:\//cdn.mydomain.com/folder/marketing/myconsultant/proposal.html

The following string demonstrates various methods for manipulating variables:

https:\//www%{http_host:3}/mobile/%{request_uri:7:10}/%{request_uri:-5:-8}.htm

Based on the sample request URL, the above variable manipulation will produce the following value:

https:\//www.mydomain.com/mobile/marketing/proposal.htm


### Pattern removal
Text that matches a specific pattern can be removed from either the beginning or the end of a variable's value.

| Syntax | Action |
| ------ | ------ |
| %{Variable#Pattern} | Remove text when the specified pattern is found at the beginning of a variable's value. |
| %{Variable%Pattern} | Remove text when the specified pattern is found at the end of a variable's value. |

#### Example:

In this sample scenario, the *request_uri* variable is set to:

`/800001/myorigin/marketing/product.html?language=en-US`

The following table demonstrates how this syntax works.

| Sample syntax | Results | |
| ------------- | ------- | --- |
| %{request_uri#/800001}/customerorigin | /customerorigin/myorigin/marketing/product.html?language=en-US | Because the variable starts with the pattern, it was replaced. |
| %{request_uri%html}htm | /800001/myorigin/marketing/product.html?language=en-US | Because the variable doesn't end with the pattern, there was no change.|

### Find and replace
The find and replace syntax is described in the following table.

| Syntax | Action |
| ------ | ------ |
| %{Variable/Find/Replace} | Find and replace first occurrence of the specified pattern. |
| %{Variable//Find/Replace} | Find and replace all occurrences of the specified pattern. |
| %{Variable^} |Convert the entire value to uppercase. |
| %{Variable^Find} | Convert the first occurrence of the specified pattern to uppercase. |
| %{Variable,} | Convert the entire value to lowercase. |
| %{Variable,Find} | Convert the first occurrence of the specified pattern to lowercase. |

### Find and rewrite
For a variation on find and replace, use the text that matches the specified pattern when rewriting it. The find and rewrite syntax is described in the following table.

| Syntax | Action |
| ------ | ------ |
| %{Variable/=Find/Rewrite} | Find, copy, and rewrite all occurrences of the specified pattern. |
| %{Variable/^Find/Rewrite} | Find, copy, and rewrite the specified pattern when it occurs at the start of the variable. |
| %{Variable/$Find/Rewrite} | Find, copy, and rewrite the specified pattern when it occurs at the end of the variable. |
| %{Variable/Find} | Find and delete all occurrences of the specified pattern. |

Key information:

- Expand text that matches the specified pattern by specifying a dollar sign followed by a whole integer (for example, $1).

- Multiple patterns can be specified. The order in which the pattern is specified determines the integer that will be assigned to it. In the following example, the first pattern matches "www.," the second pattern matches the second-level domain, and the third pattern matches the top-level domain:

    `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$2.$3:80}`

- The rewritten value can consist of any combination of text and these placeholders.

    In the previous example, the hostname is rewritten to `cdn.$2.$3:80` (for example, cdn.mydomain.com:80).

- The case of a pattern placeholder (for example, $1) can be modified through the following flags:
     - U: Uppercase the expanded value.

         Sample syntax:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$U2.$3:80}`

     - L: Lowercase the expanded value.

         Sample syntax:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$L2.$3:80}`

- An operator must be specified before the pattern. The specified operator determines the pattern-capturing behavior:

     - `=`: Indicates that all occurrences of the specified pattern must be captured and rewritten.
     - `^`: Indicates that only text that starts with the specified pattern will be captured.
     - `$`: Indicates that only text that ends with the specified pattern will be capture.
 
- If you omit the */Rewrite* value, the text that matches the pattern is deleted.

\`{host}le: HTTP variables for Azure CDN rules engine | Microsoft Docs
description: HTTP variables allow you to retrieve HTTP request and response metadata.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''

ms.assetid: 
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: magattus


---
# HTTP variables for Azure CDN rules engine
HTTP variables provide the means through which you can retrieve HTTP request and response metadata. This metadata can then be used to dynamically alter a request or a response. The use of HTTP variables is restricted to the following rules engine features:

- [Cache-Key Rewrite](cdn-verizon-premium-rules-engine-reference-features.md#cache-key-rewrite)
- [Modify Client Request Header](cdn-verizon-premium-rules-engine-reference-features.md#modify-client-request-header)
- [Modify Client Response Header](cdn-verizon-premium-rules-engine-reference-features.md#modify-client-response-header)
- [URL Redirect](cdn-verizon-premium-rules-engine-reference-features.md#url-redirect)
- [URL Rewrite](cdn-verizon-premium-rules-engine-reference-features.md#url-rewrite)

## Definitions
The following table describes the supported HTTP variables. A blank value is returned when GEO metadata (for example, postal code) is unavailable for a particular request.


| Name | Variable | Description | Sample value |
| ---- | -------- | ----------- | ------------ |
| ASN (Requester) | %{geo_asnum} | Indicates the requester's AS number. <br /><br />**Deprecated:** %{virt_dst_asnum}. <br />This variable has been deprecated in favor of %{geo_asnum}. Although a rule that uses this deprecated variable will continue to work, you should update it to use the new variable. | AS15133 |
| City (Requester) | %{geo_city} | Indicates the requester's city. | Los Angeles |
| Continent (Requester) | %{geo_continent} | Indicates the requester's continent through its abbreviation. <br />Valid values are: <br />AF: Africa<br />AS: Asia<br />EU: Europe<br />NA: North America<br />OC: Oceania<br />SA: South America<br /><br />**Deprecated:** %{virt_dst_continent}. <br />This variable has been deprecated in favor of %{geo_continent}. <br />Although a rule that uses this deprecated variable will continue to work, you should update it to use the new variable.| N/A |
| Cookie Value | %{cookie_Cookie} | Returns the value corresponding to the cookie key identified by the Cookie term. | Sample Usage: <br />%{cookie__utma}<br /><br />Sample Value:<br />111662281.2.10.1222100123 |
| Country (Requester) | %{geo_country} | Indicates the requester's country of origin through its country code. <br />**Deprecated:** %{virt_dst_country}. <br /><br />This variable has been deprecated in favor of %{geo_country}. Although a rule that uses this deprecated variable will continue to work, you should update it to use the new variable. | US |
| Designated Market Area (Requester) | %{geo_dma_code} |Indicates the requester's media market by its region code. <br /><br />This field is only applicable to requests that originate from the United States.| 745 |
| HTTP Request Method | %{request_method} | Indicates the HTTP request method. | GET |
| HTTP Status Code | %{status} | Indicates the HTTP status code for the response. | 200 |
| IP Address (Requester) | %{virt_dst_addr} | Indicates the requester's IP address. | 192.168.1.1 |
| Latitude (Requester) | %{geo_latitude} | Indicates the requester's latitude. | 34.0995 |
| Longitude (Requester) | %{geo_longitude} | Indicates the requester's longitude. | -118.4143 |
| Metropolitan Statistical Area (Requester) | %{geo_metro_code} | Indicates the requester's metropolitan area. <br /><br />This field is only applicable to requests that originate from the United States.<br />| 745 |
| Port (Requester) | %{virt_dst_port} | Indicates the requester's ephemeral port. | 55885 |
| Postal Code (Requester) | %{geo_postal_code} | Indicates the requester's postal code. | 90210 |
| Query String Found | %{is_args} | The value for this variable varies according to whether the request contains a query string.<br /><br />- Query String Found: ?<br />- No Query String: NULL | ? |
| Query String Parameter Found | %{is_amp} | The value for this variable varies according to whether the request contains at least one query string parameter.<br /><br />- Parameter Found: &<br />- No Parameters: NULL | & |
| Query String Parameter Value | %{arg_&lt;parameter&gt;} | Returns the value corresponding to the query string parameter identified by the &lt;parameter&gt; term. | Sample Usage: <br />%{arg_language}<br /><br />Sample Query String Parameter: <br />?language=en<br /><br />Sample Value: en |
| Query String Value | %{query_string} | Indicates the entire query string value defined in the request URL. |key1=val1&key2=val2&key3=val3 |
| Referrer Domain | %{referring_domain} | Indicates the domain defined in the Referrer request header. | <www.google.com> |
| Region (Requester) | %{geo_region} | Indicates the requester's region (for example, state or province) through its alphanumeric abbreviation. | CA |
| Request Header Value | %{http_RequestHeader} | Returns the value corresponding to the request header identified by the RequestHeader term. <br /><br />If the name of the request header contains a dash (for example, User-Agent), replace it with an underscore (for example, User_Agent).| Sample Usage: %{http_Connection}<br /><br />Sample Value: Keep-Alive | 
| Request Host | %{host} | Indicates the host defined in the request URL. | <www.mydomain.com> |
| Request Protocol | %{request_protocol} | Indicates the request protocol. | HTTP/1.1 |
| Request Scheme | %{scheme} | Indicates the request scheme. |http |
| Request URI (Relative) | %{request_uri} | Indicates the relative path, including the query string, defined in the request URI. | /marketing/foo.js?loggedin=true |
| Request URI (Relative without query string) | %{uri} | Indicates the relative path to the requested content. <br /><br/>Key information:<br />- This relative path excludes the query string.<br />- This relative path reflects URL rewrites. A URL will be rewritten under the following conditions:<br />  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- URL Rewrite Feature: This feature rewrites the relative path defined in the request URI.<br />    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Edge CNAME URL: This type of request is rewritten to the corresponding CDN URL. |/800001/corigin/rewrittendir/foo.js |
| Request URI | %{request} | Describes the request. <br />Syntax: &lt;HTTP method&gt; &lt;relative path&gt; &lt;HTTP protocol&gt; | GET /marketing/foo.js?loggedin=true HTTP/1.1 |
| Response Header Value | %{resp_&lt;ResponseHeader&gt;} | Returns the value corresponding to the response header identified by the &lt;ResponseHeader&gt; term. <br /><br />If the name of the response header contains a dash (for example, User-Agent), replace it with an underscore (for example, User_Agent). | Sample Usage: %{resp_Content_Length}<br /><br />Sample Value: 100 |

## Usage
The following table describes the proper syntax for specifying an HTTP variable.


| Syntax | Example | Description |
| ------ | -------- | ---------- |
| %{&lt;HTTPVariable&gt;} | %{host} | Use this syntax to get the entire value corresponding to the specified &lt;HTTPVariable&gt;. |
| %{&lt;HTTPVariableDelimiter&gt;} | %{host,} | Use this syntax to set the case for the entire value corresponding to the specified  &lt;HTTPVariableDelimiter&gt;. |
| %{&lt;HTTPVariableDelimiterExpression&gt;} | %{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$2.$3:80} | Use a regular expression for &lt;HTTPVariableDelimiterExpression&gt; to replace, delete, or manipulate an HTTP variable's value. |

HTTP variable names only support alphabetic characters and underscores. Convert unsupported characters to underscores.

## Delimiter reference
A delimiter can be specified after an HTTP variable to achieve any of the following effects:

- Transform the value associated with the variable.

     Example: Convert the entire value to lowercase.

- Delete the value associated with the variable.

- Manipulate the value associated with the variable.

     Example: Use regular expressions to change the value associated with the HTTP variable.

The delimiters are described in the following table.

| Delimiter | Description |
| --------- | ----------- |
| := | Indicates that a default value will be assigned to the variable when it is either: <br />- Missing <br />- Set to NULL. |
| :+ | Indicates that a default value will be assigned to the variable when a value has been assigned to it. |
| : | Indicates that a substring of the value assigned to the variable will be expanded. |
| # | Indicates that the pattern specified after this delimiter should be deleted when it is found at the beginning of the value associated with the variable. |
| % | Indicates that the pattern specified after this delimiter should be deleted when it is found at the end of the value associated with the variable. <br />This definition is only applicable when the % symbol is used as a delimiter. |
| / | Delimits an HTTP variable or a pattern. |
| // | Find and replace all instances of the specified pattern. |
| /= | Find, copy, and rewrite all occurrences of the specified pattern. |
| , | Convert the value associated with the HTTP variable to lowercase. |
| ^ | Convert the value associated with the HTTP variable to uppercase. |
| ,, | Convert all instances of the specified character in the value associated with the HTTP variable to lowercase. |
| ^^ | Convert all instances of the specified character in the value associated with the HTTP variable to uppercase. |

## Exceptions
The following table describes circumstances under which the specified text isn't treated as an HTTP variable.

| Condition | Description | Example |
| --------- | ----------- | --------|
| Escaping % symbol | The percentage symbol can be escaped through the use of a backslash. <br />The sample value to the right will be treated as a literal value and not as an HTTP variable.| \%{host} |
| Onbekende variabelen | Een lege tekenreeks geretourneerd altijd voor onbekende variabelen. | %{unknown_variable} |
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
| Een header ingesteld op een standaardwaarde wanneer deze voldoet aan een van de volgende voorwaarden: <br /><br />-Koptekst ontbreekt <br /><br />-Header-waarde is ingesteld op NULL.| %{Variable:=Value} | %{http_referrer:=unspecified} | De verwijzende koptekst wordt alleen worden ingesteld op *niet nader omschreven* wanneer het zich in ontbreekt of is ingesteld op NULL. Er is geen actie vindt plaats als deze eenmaal is ingesteld. |
| Een header ingesteld op een standaardwaarde als deze ontbreekt. | %{Variable=Value} | %{http_referrer=unspecified} | De verwijzende koptekst wordt alleen worden ingesteld op *niet nader omschreven* als deze ontbreekt. Er is geen actie vindt plaats als deze eenmaal is ingesteld. |
| De header ingesteld op een standaardwaarde wanneer deze niet voldoet aan een van de volgende voorwaarden: <br /><br />-Ontbreekt<br /><br /> -Instellen op NULL. | % {Variabele: + waarde} | %{http_referrer:+unspecified} | De verwijzende koptekst wordt alleen worden ingesteld op *niet nader omschreven* wanneer een waarde is toegewezen aan deze. Er is geen actie vindt plaats als deze ontbreekt of is ingesteld op NULL. |

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

| Syntaxis | Action |
| ------ | ------ |
| %{Variable#Pattern} | Verwijder tekst als het opgegeven patroon wordt gevonden aan het begin van de waarde van een variabele. |
| %{Variable%Pattern} | Verwijder tekst als het opgegeven patroon wordt gevonden aan het einde van de waarde van een variabele. |

#### <a name="example"></a>Voorbeeld:

In dit voorbeeldscenario; de *request_uri* variabele is ingesteld op:

`/800001/myorigin/marketing/product.html?language=en-US`

De volgende tabel laat zien hoe deze syntaxis werkt.

| Voorbeeldsyntaxis | Resultaten | |
| ------------- | ------- | --- |
| %{request_uri#/800001}/customerorigin | /customerorigin/myorigin/marketing/product.html?language=en-US | Omdat de variabele wordt gestart met het patroon, is het vervangen. |
| %{request_uri%html}htm | /800001/myorigin/marketing/product.html?language=en-US | Omdat de variabele niet met het patroon eindigt, is er geen wijziging.|

### <a name="find-and-replace"></a>Zoeken en vervangen
De syntaxis van zoeken en vervangen wordt in de volgende tabel beschreven.

| Syntaxis | Action |
| ------ | ------ |
| %{Variable/Find/Replace} | Zoeken en vervangen eerste exemplaar van het opgegeven patroon. |
| %{Variable//Find/Replace} | Zoeken en vervangen van alle instanties van het opgegeven patroon. |
| %{Variable^} |De volledige waarde converteren naar hoofdletters. |
| %{Variable^Find} | Het eerste exemplaar van het opgegeven patroon converteren naar hoofdletters. |
| % {Variabele} | De volledige waarde converteren naar kleine letters. |
| % {Variabele, zoeken naar} | Het eerste exemplaar van het opgegeven patroon converteren naar kleine letters. |

### <a name="find-and-rewrite"></a>Zoeken en schrijven
Voor een variant van zoeken en vervangen, gebruikt u de tekst die overeenkomt met het opgegeven patroon wanneer opnieuw te schrijven. De syntaxis van de zoek- en herschrijven wordt in de volgende tabel beschreven.

| Syntaxis | Action |
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


