---
title: Project-URL-voorbeeld-verwijzing
titlesuffix: Azure Cognitive Services
description: Referentie voor het eindpunt van de Project-URL-voorbeeld.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: url-preview
ms.topic: reference
ms.date: 03/29/2018
ms.author: rosh
ms.openlocfilehash: da23d8037f37174826a2b9662c39bb507367a6ae
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57537804"
---
# <a name="project-url-preview-v7-reference"></a>Naslaggids voor project URL-voorbeeld voor Bing versie 7

URL-voorbeeld ondersteunt korte beschrijvingen van bronnen op het Web voor blogberichten, forumdiscussies, preview pagina's, enzovoort. De URL kan elk type resource van Internet zijn: Webpagina's, nieuws, afbeelding of video. De query moet een absolute URL met een schema http of https; relatieve URL's of andere schema's, zoals ftp: / / worden niet ondersteund.

Toepassingen die gebruikmaken van URL-voorbeeld verzenden webaanvragen naar het eindpunt met een URL naar het voorbeeld in een queryparameter. De aanvraag moet bevatten de *Ocp-Apim-Subscription-Key* header.

De JSON-antwoord voor de Preview-versie-informatie kan worden geparseerd: naam, beschrijving van de resource *isFamilyFriendly*, en koppelingen die toegang tot een representatieve installatiekopie en de volledige resource online bieden.

Alleen de gegevens van URL-voorbeeld moet u weergeven preview codefragmenten en miniatuurafbeeldingen hyperlink aan hun bronsites in de gebruiker geïnitieerde URL delen op sociale media, chatbot of vergelijkbare aanbiedingen. U moet niet kopiëren, opslaan of alle gegevens die u van het URL-voorbeeld-Project ontvangt in de cache. U moet alle aanvragen voor het uitschakelen van voorbeelden die u van de website of eigenaren van inhoud ontvangt mogelijk in acht neemt.

## <a name="endpoint"></a>Eindpunt
Om aan te vragen van URL-voorbeeld-resultaten, door een aanvraag te verzenden naar het volgende eindpunt. De kop- en URL-parameters gebruiken om te definiëren verder specificaties.

GET-eindpunt:
```
https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search?q=queryURL

```

De aanvraag moet de HTTPS-protocol gebruiken en omvatten queryparameter te volgen:

q - de query waarmee de URL om een voorbeeld van

De volgende secties vindt u technische gegevens over de antwoordobjecten, query-parameters en headers die invloed hebben op de lijst met zoekresultaten.

Zie voor meer informatie over kopteksten die aanvragen moeten [Headers](#headers).

Zie voor meer informatie over queryparameters die aanvragen moeten bevatten [queryparameters](#query-parameters).

Zie voor meer informatie over de JSON-objecten dat het antwoord bevat, [antwoordobjecten](#response-objects).

De maximale URL-lengte is 2048 tekens. Om ervoor te zorgen dat uw URL-lengte de limiet niet overschrijdt, moet de maximale lengte van de queryparameters maximaal 1500 tekens. Als de URL is langer dan 2048 tekens, retourneert de server 404 niet gevonden.

Zie voor meer informatie over het toegestane gebruik en weergave van resultaten [gebruiken en weergavevereisten](use-display-requirements.md).

> [!NOTE]
> Sommige aanvraagheaders die relevant zijn voor andere search API's zijn niet van invloed op URL-voorbeeld
> - Pragma – de oproepende functie heeft geen controle over of URL-voorbeeld maakt gebruik van cache
> - Gebruikersagent – voorlopig Url Preview-API biedt geen verschillende reacties voor aanroepen afkomstig van pc's, Laptop of Mobile.

> Bovendien sommige parameters zijn niet op dit moment zinvol is voor de Preview-API-URL, maar kunnen worden gebruikt in de toekomst bevindt voor verbeterde globalisatie.

## <a name="headers"></a>Headers
Hier volgen de headers die bijvoorbeeld een aanvraag en antwoord bevatten.

|Header|Description|
|------------|-----------------|
|<a name="market" />BingAPIs-Market|Antwoordheader.<br /><br /> De markt die wordt gebruikt door de aanvraag. De notatie is \<languageCode\>-\<countryCode\>. Bijvoorbeeld: nl-NL.|
|<a name="traceid" />BingAPIs-TraceId|Antwoordheader.<br /><br /> De id van de logboekvermelding die de details van de aanvraag bevat. Registreer deze id wanneer er een fout optreedt. Als u het probleem niet kunt vaststellen en oplossen, neemt u deze id op bij de andere informatie die u aan het ondersteuningsteam verstrekt.|
|<a name="subscriptionkey" />Ocp-Apim-Subscription-Key|Vereiste aanvraagheader.<br /><br /> De abonnementssleutel die u hebt ontvangen toen u zich hebt geregistreerd voor deze service in [Cognitive Services](https://www.microsoft.com/cognitive-services/).|
|<a name="clientid" />X-MSEdge-ClientID|Optionele aanvraag- en antwoord-header.<br /><br /> Bing gebruikt deze header om gebruikers consistent gedrag te bieden bij Bing API-aanroepen. Bing introduceert nieuwe functies en verbeteringen vaak in flights, en gebruikt de client-ID als sleutel voor het toewijzen van verkeer aan verschillende flights. Als u niet dezelfde client-id gebruikt voor een gebruiker voor meerdere aanvragen, kan Bing de gebruiker toewijzen aan meerdere conflicterende flights. Toewijzing aan meerdere conflicterende flights kan leiden tot een inconsistente gebruikerservaring. Als de tweede aanvraag bijvoorbeeld een andere flighttoewijzing heeft dan de eerste, kan de ervaring onverwacht zijn. Bing kan de client-id ook gebruiken om webresultaten aan te passen aan de zoekgeschiedenis van die client-id, waardoor de gebruiker een rijkere ervaring krijgt.<br /><br /> Bing gebruikt deze header ook om de rangschikking van resultaten te verbeteren door de activiteit te analyseren die wordt gegenereerd door een client-id. De relevantie-verbeteringen helpen de kwaliteit van de resultaten die door Bing-API's worden geleverd te verbeteren, en maken op hun beurt hogere doorklikpercentages mogelijk voor de API-consument.<br /><br />Hieronder volgen de basisgebruiksregels die van toepassing zijn op deze header.<br /><ul><li>Elke gebruiker die uw toepassing op het apparaat gebruikt, moet een unieke door Bing gegenereerde client-id hebben.<br /><br/>Als u deze header niet in de aanvraag opneemt, genereert Bing een id en retourneert deze in de X-MSEdge-ClientID-antwoordheader. De enige keer dat u deze header NIET in een aanvraag moet opnemen, is de eerste keer dat de gebruiker uw app op dat apparaat gebruikt.<br /><br/></li><li>Gebruik de client-id voor elke Bing-API-aanvraag die uw app voor deze gebruiker op het apparaat doet.<br /><br/></li><li>**LET OP:** U moet ervoor zorgen dat deze Client-ID niet gerelateerd aan een authenticatable gebruikersaccountgegevens is.</li><br/><li>Maak de client-id persistent. Gebruik in een browser-app een persistent HTTP-cookie om ervoor te zorgen dat de id in alle sessies wordt gebruikt. Gebruik geen sessiecookie. Voor andere apps, zoals mobiele apps, gebruikt u de persistente opslag van het apparaat om de id persistent te maken.<br /><br/>De volgende keer dat de gebruiker uw app op dat apparaat gebruikt, haalt u de client-id op die u persistent hebt gemaakt.</li></ul><br /> **OPMERKING:** Bing-antwoorden kunnen mogelijk niet bevatten of deze header. Als het antwoord deze header bevat, registreert u de client-id en gebruikt u deze voor alle volgende Bing-aanvragen voor de gebruiker op dat apparaat.<br /><br /> **OPMERKING:** Als u de X-MSEdge-ClientID opneemt, moet u cookies niet opnemen in de aanvraag.|
|<a name="clientip" />X-MSEdge-ClientIP|Optionele aanvraagheader.<br /><br /> Het IPv4- of IPv6-adres van het clientapparaat. Het IP-adres wordt gebruikt voor het detecteren van de locatie van de gebruiker. Bing gebruikt de locatie-informatie om het gedrag van Veilig Zoeken te bepalen.<br /><br /> Verdoezel het adres niet (bijvoorbeeld door het laatste octet te wijzigen in 0). Wanneer u het adres verdoezelt, is de locatie totaal niet in de buurt van de werkelijke locatie van het apparaat, wat ertoe kan leiden dat Bing onjuiste resultaten geeft.|

## <a name="query-parameters"></a>Queryparameters
De aanvraag kan de volgende queryparameters bevatten. Zie de vereiste kolom voor vereiste parameters. Moet u de URL de queryparameters coderen. De query moet een absolute URL met een schema http of https; We bieden geen ondersteuning voor relatieve URL's of andere schema's, zoals ftp: / /

|Name|Value|Type|Vereist|
|----------|-----------|----------|--------------|
|<a name="mkt" />mkt|De markt waaruit de resultaten afkomstig zijn. <br /><br />Zie voor een lijst van mogelijke waarden van de markt, markt-Codes.<br /><br /> **OPMERKING:** De URL van Preview-API ondersteunt momenteel alleen Amerikaanse Geografie en Engelse taal.<br /><br />|String|Ja|
|<a name="query" />q|De URL om een voorbeeld van|String|Ja|
|<a name="responseformat" />responseFormat|Het mediatype dat als u wilt gebruiken voor het antwoord. De volgende zijn waarden mogelijk niet hoofdlettergevoelig.<br /><ul><li>JSON</li><li>JSONLD</li></ul><br /> De standaardwaarde is JSON. Zie voor meer informatie over de JSON-objecten dat het antwoord bevat, [Antwoordobjecten](#response-objects).<br /><br />Als u JsonLd opgeeft, bevat de antwoordtekst JSON-LD objecten met de lijst met zoekresultaten. Zie voor meer informatie over de JSON-LD [JSON-LD](https://json-ld.org/).|String|Nee|
|<a name="safesearch"/>safeSearch|Ongeldige inhoud voor volwassenen, of illegale inhoud, is geblokkeerd met foutcode 400, en de *isFamilyFriendly* vlag wordt niet geretourneerd. <p>Voor juridische inhoud voor volwassenen, ziet hieronder u het gedrag. Statuscode 200 wordt geretourneerd en de *isFamilyFriendly* vlag is ingesteld op false.<ul><li>safeSearch=strict: Titel, beschrijving, URL en afbeelding wordt niet geretourneerd.</li><li>veilig zoeken = gemiddeld; Titel, URL en beschrijving, maar niet de beschrijvende afbeelding ophalen</li><li>veilig zoeken = uit; Krijg alle antwoord objecten/elementen: titel, URL, beschrijving en afbeelding.</li></ul> |String|Niet vereist. </br> Standaard ingesteld op safeSearch = strikte.|

## <a name="response-objects"></a>Antwoordobjecten
Het antwoordschema is ofwel een [webpagina] of ErrorResponse, zoals in de webzoekopdrachten-API. Als de aanvraag mislukt, wordt het object op het hoogste niveau is het [ErrorResponse](#errorresponse) object.

|Object|Description|
|------------|-----------------|
|[WebPage](#webpage)|Bovenste niveau JSON-object met de kenmerken van de Preview-versie.|

### <a name="error"></a>Fout
Hiermee definieert u de fout is opgetreden.

|Element|Description|Type|
|-------------|-----------------|----------|
|<a name="error-code" />code|De foutcode die aangeeft van de categorie van de fout. Zie voor een lijst van mogelijke codes, [foutcodes](#error-codes).|String|
|<a name="error-message" />Bericht|Een beschrijving van de fout.|String|
|<a name="error-moredetails" />moreDetails|Een beschrijving met aanvullende informatie over de fout.|String|
|<a name="error-parameter" />parameter|De queryparameter in de aanvraag die de fout heeft veroorzaakt.|String|
|<a name="error-subcode" />subCode|De foutcode die aangeeft van de fout. Bijvoorbeeld, als `code` InvalidRequest, is `subCode` mogelijk ParameterInvalid of ParameterInvalidValue. |String|
|<a name="error-value" />Waarde|De queryparameter-waarde die niet geldig is.|String|

### <a name="errorresponse"></a>ErrorResponse
Het object van het type op het hoogste niveau dat het antwoord bevat als de aanvraag is mislukt.

|Name|Value|Type|
|----------|-----------|----------|
|_type|Type hint.|String|
|<a name="errors" />Fouten|Een lijst van fouten die worden beschreven van de redenen waarom de aanvraag is mislukt.|[Fout](#error)]|

### <a name="webpage"></a>WebPage
Informatie over definieert een de webpagina wordt weergegeven in de Preview-versie.

|Name|Value|Type|
|----------|-----------|----------|
|naam|De titel van de pagina, niet per se de HTML-titel|String|
|url|De URL die daadwerkelijk is verkend (aanvraag kan hebt gevolgd omleidingen)|String|
|description|Korte beschrijving van de pagina en de inhoud|String|
|isFamilyFriendly|Meest nauwkeurige voor items in de web-index; realtime haalt doen deze detectie die uitsluitend zijn gebaseerd op de URL en niet op de pagina-inhoud|booleaans|
|primaryImageOfPage/contentUrl|De URL naar een representatieve afbeelding wilt opnemen in de Preview-versie|String|

### <a name="identifiable"></a>Identificeerbare
|Name|Value|Type|
|-------------|-----------------|----------|
|id|Een resource-id|String|

## <a name="error-codes"></a>Foutcodes

Hier volgen de mogelijke HTTP-statuscodes die een aanvraag retourneert.

|Statuscode|Description|
|-----------------|-----------------|
|200|Geslaagd.|
|400|Een van de queryparameters is ontbreekt of is ongeldig.|
|400|ServerError, ResourceError subCode: De aangevraagde URL kan niet worden bereikt.|
|400|ServerError, ResourceError subCode: De aangevraagde URL heeft geen een succescode (inclusief, indien deze geretourneerd HTTP 404) geretourneerd.|
|400|InvalidRequest, subCode geblokkeerd: De aangevraagde URL bevatten inhoud voor volwassen en is geblokkeerd|
|401|De abonnementssleutel ontbreekt of is niet geldig.|
|403|De gebruiker is geverifieerd (bijvoorbeeld deze sleutel hebt gebruikt een geldig abonnement), maar zij geen machtiging voor de aangevraagde resource.<br /><br /> Bing mogelijk deze status ook geretourneerd als de oproepende functie hun query's per maand quotum overschreden.|
|410|De aanvraag gebruikt HTTP in plaats van het HTTPS-protocol. HTTPS is het enige ondersteunde protocol.|
|429|De oproepende functie heeft de query's per seconde quotum overschreden.|
|500|Onverwachte serverfout opgetreden.|

Als de aanvraag mislukt, wordt het antwoord bevat een [ErrorResponse](#errorresponse) object, dat een lijst met bevat [fout](#error) objecten die wordt beschreven wat de oorzaak van de fout. Als de fout is gerelateerd aan een parameter, de `parameter` veld geeft de parameter die het probleem. En als de fout is gerelateerd aan een parameterwaarde, de `value` veld geeft de waarde die is niet geldig.

```json
{
  "_type": "ErrorResponse",
  "errors": [
    {
      "code": "InvalidRequest",
      "subCode": "ParameterMissing",
      "message": "Required parameter is missing.",
      "parameter": "q"
    }
  ]
}

{
  "_type": "ErrorResponse",
  "errors": [
    {
      "code": "InvalidAuthorization",
      "subCode": "AuthorizationMissing",
      "message": "Authorization is required.",
      "moreDetails": "Subscription key is not recognized."
    }
  ]
}
```

Hier volgen de mogelijke fout code en de onderliggende fout code-waarden.

|Code|SubCode|Description
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>Niet geïmplementeerd|HTTP-statuscode is 500.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Geblokkeerd|Bing retourneert InvalidRequest wanneer er een deel van de aanvraag niet geldig is. Bijvoorbeeld, een vereiste parameter ontbreekt of een parameterwaarde is niet geldig.<br/><br/>Als de fout ParameterMissing of ParameterInvalidValue is, is de HTTP-statuscode 400.<br/><br/>Als u het HTTP-protocol in plaats van HTTPS, Bing retourneert HttpNotAllowed en de HTTP-statuscode 410.
|RateLimitExceeded|Er is geen onderliggende codes|Bing retourneert RateLimitExceeded wanneer u uw query's per seconde (QPS) of query's per maand (QPM) quotum overschrijdt.<br/><br/>Als u QPS overschrijdt, Bing retourneert HTTP-statuscode 429 en als u QPM overschrijdt, Bing 403 retourneert.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing retourneert InvalidAuthorization wanneer Bing de oproepende functie kan niet worden geverifieerd. Bijvoorbeeld, de `Ocp-Apim-Subscription-Key` koptekst ontbreekt of de abonnementssleutel is niet geldig.<br/><br/>Redundantie treedt op als u meer dan één verificatiemethode opgeven.<br/><br/>Als de fout InvalidAuthorization is, is de HTTP-statuscode 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing retourneert InsufficientAuthorization wanneer de oproepende functie heeft geen machtigingen voor toegang tot de resource. Dit kan gebeuren als de abonnementssleutel is uitgeschakeld of is verlopen. <br/><br/>Als de fout InsufficientAuthorization is, is de HTTP-statuscode 403.

## <a name="next-steps"></a>Volgende stappen
- [Snelstart voor C#](csharp.md)
- [Snelstart voor Java](java-quickstart.md)
- [Snelstart voor JavaScript](javascript.md)
- [Snelstart voor Node](node-quickstart.md)
- [Snelstartgids voor Python](python-quickstart.md)
