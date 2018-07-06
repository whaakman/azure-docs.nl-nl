---
title: Naslaginformatie over URL-voorbeeld - Microsoft Cognitive Services-project | Microsoft Docs
description: Referentie voor het eindpunt van de Project-URL-voorbeeld.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/29/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 46c011d62b6ae51f5f7d292345e6ece0e27a8541
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37865872"
---
# <a name="project-url-preview-v7-reference"></a>Naslaggids voor project URL-voorbeeld voor Bing versie 7

URL-voorbeeld ondersteunt korte beschrijvingen van bronnen op het Web voor blogberichten, forumdiscussies, preview pagina's, enzovoort.  De URL kan elk type resource van Internet worden: webpagina's, nieuws, afbeelding of video. De query moet een absolute URL met een schema http of https; relatieve URL's of andere schema's, zoals ftp: / / worden niet ondersteund.

Toepassingen die gebruikmaken van URL-voorbeeld verzenden webaanvragen naar het eindpunt met een URL naar het voorbeeld in een queryparameter.  De aanvraag moet bevatten de *Ocp-Apim-Subscription-Key* header.   

De JSON-antwoord voor de Preview-versie-informatie kan worden geparseerd: naam, beschrijving van de resource *isFamilyFriendly*, en koppelingen die toegang tot een representatieve installatiekopie en de volledige resource online bieden.

Alleen de gegevens van URL-voorbeeld moet u weergeven preview codefragmenten en miniatuurafbeeldingen hyperlink aan hun bronsites in de gebruiker geïnitieerde URL delen op sociale media, chatbot of vergelijkbare aanbiedingen. U moet niet kopiëren, opslaan of alle gegevens die u van het URL-voorbeeld-Project ontvangt in de cache. U moet alle aanvragen voor het uitschakelen van voorbeelden die u van de website of eigenaren van inhoud ontvangt mogelijk in acht neemt.

## <a name="endpoint"></a>Eindpunt
Om aan te vragen van URL-voorbeeld-resultaten, door een aanvraag te verzenden naar het volgende eindpunt. De kop- en URL-parameters gebruiken om te definiëren verder specificaties.

GET-eindpunt: 
````
https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search?q=queryURL

````

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
  
|Koptekst|Beschrijving|  
|------------|-----------------|   
|<a name="market" />BingAPIs-market te versnellen|Antwoordheader.<br /><br /> De markt die worden gebruikt door de aanvraag. Het formulier is \<languageCode\>-\<countryCode\>. Bijvoorbeeld: nl-nl.|  
|<a name="traceid" />BingAPIs TraceId|Antwoordheader.<br /><br /> De ID van de vermelding die de details van de aanvraag bevat. Wanneer er een fout optreedt, vastleggen van deze ID. Als u niet kunt bepalen en los het probleem, zijn deze ID samen met de andere informatie die het ondersteuningsteam op te geven.|  
|<a name="subscriptionkey" />OCP-Apim-Subscription-Key|De aanvraagheader is vereist.<br /><br /> De abonnementssleutel die u hebt ontvangen toen u zich hebt geregistreerd voor deze service in [Cognitive Services](https://www.microsoft.com/cognitive-services/).|  
|<a name="clientid" />X-MSEdge-ClientID|Optionele aanvraag en antwoord-header.<br /><br /> Bing maakt gebruik van deze header voor gebruikers met een consistent gedrag voor Bing-API-aanroepen. Bing vluchten vaak nieuwe functies en verbeteringen en gebruikt de client-ID als een sleutel voor het toewijzen van verkeer op verschillende vluchten. Als u gebruik niet dezelfde client-ID voor een gebruiker met meerdere aanvragen, kunnen Bing kan de gebruiker toewijzen aan meerdere conflicterende vluchten. Wordt toegewezen aan meerdere conflicterende vluchten kan leiden tot een inconsistente gebruikerservaring. Bijvoorbeeld, als de tweede aanvraag de toewijzing van een andere vlucht dan de eerste heeft, de ervaring mogelijk onverwachte. Bing kunt ook de client-ID gebruiken om aan te passen webresultaten voor deze client id zoekgeschiedenis, bieden een rijkere ervaring voor de gebruiker.<br /><br /> Bing gebruikt ook deze header om het resultaat classificaties verbeteren door het analyseren van de activiteit die is gegenereerd door een client-ID. Verbeteringen in de volgorde van relevantie helpen met hogere kwaliteit van de resultaten die worden geleverd door Bing-API's en op zijn beurt hoger kunnen via klikken tarieven voor de API-consument.<br /><br />Hier volgen de basisgebruik regels die betrekking hebben op deze header.<br /><ul><li>Elke gebruiker die gebruikmaakt van uw toepassing op het apparaat moet een unieke, Bing gegenereerde client-ID.<br /><br/>Als u deze header in de aanvraag niet opgeeft, wordt Bing een ID gegenereerd en in de header X-MSEdge-ClientID-antwoord geretourneerd. De enige keer dat u moet niet deze header bevatten in een aanvraag is de eerste keer dat de gebruiker maakt gebruik van uw app op het apparaat.<br /><br/></li><li>De client-ID gebruiken voor elke API voor Bing-aanvraag die uw app voor deze gebruiker op het apparaat maakt.<br /><br/></li><li>**Let op:** moet u ervoor zorgen dat deze Client-ID niet gerelateerd aan een authenticatable gebruikersaccountgegevens is.</li><br/><li>Behouden van de client-ID. Om te blijven behouden de ID in een browser-app, gebruikt u een permanente HTTP-cookie om te controleren of dat de ID wordt gebruikt in alle sessies. Gebruik een sessiecookie niet. Voor andere apps, zoals mobiele apps, gebruikt u de permanente opslag van het apparaat om vast te leggen van de ID.<br /><br/>De volgende keer dat de gebruiker maakt gebruik van uw app op het apparaat, krijgen de client-ID die u wilt behouden.</li></ul><br /> **Opmerking:** Bing antwoorden kunnen of kan niet deze header bevatten. Als het antwoord deze header bevat, vastleggen van de client-ID en deze gebruiken voor alle volgende Bing-aanvragen voor de gebruiker op het apparaat.<br /><br /> **Opmerking:** als u de X-MSEdge-ClientID opneemt, moet u cookies niet opnemen in de aanvraag.|  
|<a name="clientip" />X-MSEdge-client-IP|De aanvraagheader is optioneel.<br /><br /> Het IPv4- of IPv6-adres van het clientapparaat. Het IP-adres wordt gebruikt voor het detecteren van de locatie van de gebruiker. De locatie-informatie Bing gebruikt om veilige zoekgedrag te bepalen.<br /><br />  Onleesbaar niet het adres maakt (bijvoorbeeld door het laatste achttal werd wijzigen in 0). Obfuscating de adres-resultaten op de locatie niet wordt overal in de buurt van de werkelijke locatie van het apparaat, die kan leiden tot onjuiste resultaten voor Bing.|  
<br /><br /></li></ul>   

## <a name="query-parameters"></a>Queryparameters  
De aanvraag kan de volgende queryparameters bevatten. Zie de vereiste kolom voor vereiste parameters. Moet u de URL de queryparameters coderen. De query moet een absolute URL met een schema http of https; We bieden geen ondersteuning voor relatieve URL's of andere schema's, zoals ftp: / /
  
  
|Naam|Waarde|Type|Vereist|  
|----------|-----------|----------|--------------|  
|<a name="mkt" />Mkt|De markt waarin de resultaten afkomstig zijn uit. <br /><br />Zie voor een lijst van mogelijke waarden van de markt, [markt Codes](#market-codes).<br /><br /> **Opmerking:** de URL van Preview-API ondersteunt momenteel alleen Amerikaanse Geografie en Engelse taal.<br /><br />|Reeks|Ja|  
|<a name="query" />q|De URL om een voorbeeld van|Reeks|Ja|  
|<a name="responseformat" />responseFormat|Het mediatype dat als u wilt gebruiken voor het antwoord. De volgende zijn waarden mogelijk niet hoofdlettergevoelig.<br /><ul><li>JSON</li><li>JSONLD</li></ul><br /> De standaardwaarde is JSON. Zie voor meer informatie over de JSON-objecten dat het antwoord bevat, [Antwoordobjecten](#response-objects).<br /><br />  Als u JsonLd opgeeft, bevat de antwoordtekst JSON-LD objecten met de lijst met zoekresultaten. Zie voor meer informatie over de JSON-LD [JSON-LD](http://json-ld.org/).|Reeks|Nee|
|<a name="safesearch"/>veilig zoeken|Ongeldige inhoud voor volwassenen, of illegale inhoud, is geblokkeerd met foutcode 400, en de *isFamilyFriendly* vlag wordt niet geretourneerd. <p>Voor juridische inhoud voor volwassenen, ziet hieronder u het gedrag. Statuscode 200 wordt geretourneerd en de *isFamilyFriendly* vlag is ingesteld op false.<ul><li>veilig zoeken = strikte: titel, beschrijving, URL en afbeelding wordt niet geretourneerd.</li><li>veilig zoeken = gemiddeld; Titel, URL en beschrijving, maar niet de beschrijvende afbeelding ophalen</li><li>veilig zoeken = uit; Krijg alle antwoord objecten/elementen: titel, URL, beschrijving en afbeelding.</li></ul> |Reeks|Niet vereist. </br> Standaard ingesteld op safeSearch = strikte.| 

## <a name="response-objects"></a>Antwoordobjecten  
Het antwoordschema is ofwel een [webpagina] of ErrorResponse, zoals in de webzoekopdrachten-API. Als de aanvraag mislukt, wordt het object op het hoogste niveau is het [ErrorResponse](#errorresponse) object.


|Object|Beschrijving|  
|------------|-----------------|  
|[Webpagina](#webpage)|Bovenste niveau JSON-object met de kenmerken van de Preview-versie.|  

  
### <a name="error"></a>Fout  
Hiermee definieert u de fout is opgetreden.  
  
|Element|Beschrijving|Type|  
|-------------|-----------------|----------|  
|<a name="error-code" />Code|De foutcode die aangeeft van de categorie van de fout. Zie voor een lijst van mogelijke codes, [foutcodes](#error-codes).|Reeks|  
|<a name="error-message" />Bericht|Een beschrijving van de fout.|Reeks|  
|<a name="error-moredetails" />moreDetails|Een beschrijving met aanvullende informatie over de fout.|Reeks|  
|<a name="error-parameter" />parameter|De queryparameter in de aanvraag die de fout heeft veroorzaakt.|Reeks|  
|<a name="error-subcode" />de subCode|De foutcode die aangeeft van de fout. Bijvoorbeeld, als `code` InvalidRequest, is `subCode` mogelijk ParameterInvalid of ParameterInvalidValue. |Reeks|  
|<a name="error-value" />waarde|De queryparameter-waarde die niet geldig is.|Reeks|  
  

### <a name="errorresponse"></a>ErrorResponse  
Het object van het type op het hoogste niveau dat het antwoord bevat als de aanvraag is mislukt.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|  
|_Type|Type hint.|Reeks|  
|<a name="errors" />fouten|Een lijst van fouten die worden beschreven van de redenen waarom de aanvraag is mislukt.|[Fout](#error)]|   
  

### <a name="webpage"></a>Webpagina  
Informatie over definieert een de webpagina wordt weergegeven in de Preview-versie.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|
|naam|De titel van de pagina, niet per se de HTML-titel|Reeks|
|url|De URL die daadwerkelijk is verkend (aanvraag kan hebt gevolgd omleidingen)|Reeks|  
|description|Korte beschrijving van de pagina en de inhoud|Reeks|  
|isFamilyFriendly|Meest nauwkeurige voor items in de web-index; realtime haalt doen deze detectie die uitsluitend zijn gebaseerd op de URL en niet op de pagina-inhoud|booleaans|
|primaryImageOfPage/contentUrl|De URL naar een representatieve afbeelding wilt opnemen in de Preview-versie|Reeks| 


### <a name="identifiable"></a>Identificeerbare
|Naam|Waarde|Type|  
|-------------|-----------------|----------|
|id|Een resource-id|Reeks|
 

## <a name="error-codes"></a>Foutcodes

Hier volgen de mogelijke HTTP-statuscodes die een aanvraag retourneert.  
  
|Statuscode|Beschrijving|  
|-----------------|-----------------|  
|200|Geslaagd.|  
|400|Een van de queryparameters is ontbreekt of is ongeldig.| 
|400|ServerError, ResourceError subCode: de aangevraagde URL kan niet worden bereikt.|
|400|ServerError, ResourceError subCode: de aangevraagde URL heeft geen een succescode (inclusief, indien deze geretourneerd HTTP 404) geretourneerd.|
|400|InvalidRequest, geblokkeerd subCode: de aangevraagde URL bevatten inhoud voor volwassen en is geblokkeerd| 
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

|Code|De subCode|Beschrijving
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>Niet geïmplementeerd|HTTP-statuscode is 500.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Geblokkeerd|Bing retourneert InvalidRequest wanneer er een deel van de aanvraag niet geldig is. Bijvoorbeeld, een vereiste parameter ontbreekt of een parameterwaarde is niet geldig.<br/><br/>Als de fout ParameterMissing of ParameterInvalidValue is, is de HTTP-statuscode 400.<br/><br/>Als u het HTTP-protocol in plaats van HTTPS, Bing retourneert HttpNotAllowed en de HTTP-statuscode 410.
|RateLimitExceeded|Er is geen onderliggende codes|Bing retourneert RateLimitExceeded wanneer u uw query's per seconde (QPS) of query's per maand (QPM) quotum overschrijdt.<br/><br/>Als u QPS overschrijdt, Bing retourneert HTTP-statuscode 429 en als u QPM overschrijdt, Bing 403 retourneert.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing retourneert InvalidAuthorization wanneer Bing de oproepende functie kan niet worden geverifieerd. Bijvoorbeeld, de `Ocp-Apim-Subscription-Key` koptekst ontbreekt of de abonnementssleutel is niet geldig.<br/><br/>Redundantie treedt op als u meer dan één verificatiemethode opgeven.<br/><br/>Als de fout InvalidAuthorization is, is de HTTP-statuscode 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing retourneert InsufficientAuthorization wanneer de oproepende functie heeft geen machtigingen voor toegang tot de resource. Dit kan gebeuren als de abonnementssleutel is uitgeschakeld of is verlopen. <br/><br/>Als de fout InsufficientAuthorization is, is de HTTP-statuscode 403.

## <a name="next-steps"></a>Volgende stappen
- [Snelstartgids voor C#](csharp.md)
- [Snelstartgids voor Java](java-quickstart.md)
- [Snelstartgids voor JavaScript](javascript.md)
- [Knooppunt-snelstartgids](node-quickstart.md)
- [Snelstartgids voor Python](python-quickstart.md)

