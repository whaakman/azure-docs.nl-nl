---
title: URL-Preview referentie - cognitieve Services van Microsoft Project | Microsoft Docs
description: Verwijzing voor Project URL Preview-eindpunt.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/29/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: adc2f83f703e740e40d9ba4fd3ed08ba429e5d97
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345340"
---
# <a name="project-url-preview-v7-reference"></a>Projectverwijzing URL Preview v7

URL-Preview biedt ondersteuning voor korte beschrijvingen van bronnen op het Web voor blogberichten, forum discussies, preview pagina's, enzovoort.  De URL mag type Internet resource: webpagina, nieuws, image of video. De query moet een absolute URL met een schema http of https; relatieve URL's of andere schema's, zoals ftp: / / worden niet ondersteund.

Toepassingen die gebruikmaken van URL Preview verzenden webaanvragen naar het eindpunt met een URL naar het voorbeeld in een queryparameter.  De aanvraag moet bevatten de *Ocp-Apim-Subscription-Key* header.   

De JSON-antwoord voor de preview-informatie kan worden geparseerd: naam, beschrijving van de resource *isFamilyFriendly*, en koppelingen die toegang tot een representatieve installatiekopie en de volledige resource online bieden.

Alleen de gegevens van de URL-voorbeeld moet u preview codefragmenten en miniatuurafbeeldingen hyperlink naar hun bronsites in een eindgebruiker geïnitieerde URL delen op sociale media, chat bot of vergelijkbare aanbiedingen weergeven. U moet niet kopiëren, opslaan of alle gegevens die u van Project URL Preview ontvangt in de cache. U moet voldoen aan de verzoeken om uit te schakelen voorbeelden die kunnen worden weergegeven van de website of eigenaren van inhoud.

## <a name="endpoint"></a>Eindpunt
Als URL voorbeeldresultaten, een aanvraag te verzenden naar het volgende eindpunt. De kop- en URL-parameters gebruiken voor het definiëren van verdere specificaties.

GET-eindpunt: 
````
https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search?q=queryURL

````

De aanvraag moet de HTTPS-protocol gebruiken en omvatten queryparameter te volgen:

 q - de query waarmee de URL voor de preview 

De volgende secties vindt u technische gegevens over de antwoord-objecten, queryparameters en headers die invloed hebben op de zoekresultaten. 
  
Zie voor meer informatie over kopteksten die moeten worden opgenomen in aanvragen [Headers](#headers).  
  
Zie voor meer informatie over queryparameters die moeten worden opgenomen in aanvragen [queryparameters](#query-parameters).  
  
Zie voor meer informatie over de JSON-objecten dat het antwoord bevat [antwoord objecten](#response-objects).

De maximale URL-lengte is 2048 tekens. Om ervoor te zorgen dat uw URL-lengte de limiet niet overschrijdt, moet de maximale lengte van de queryparameters maximaal 1500 tekens. Als de URL langer is dan 2048 tekens, retourneert de server 404 niet gevonden.  

Zie voor meer informatie over toegestaan gebruik en weergeven van resultaten [gebruiken en weergeven van vereisten](use-display-requirements.md). 

> [!NOTE]
> Sommige aanvraagheaders die relevant zijn voor andere zoeken-API's zijn niet van invloed op URL-Preview
> - Pragma – de aanroeper heeft geen controle over het feit of de URL Preview cache gebruikt
> - Gebruikersagent – nu Url Preview-API biedt niet verschillende antwoorden voor aanroepen afkomstig van een PC of Laptop Mobile.

> Bovendien sommige parameters zijn niet op dit moment relevant zijn voor de Preview-API-URL, maar kunnen worden gebruikt in de toekomst bevindt voor verbeterde globalisatie. 
 
## <a name="headers"></a>Headers  
Hier volgen de headers die een aanvraag en -antwoord kunnen bevatten.  
  
|Koptekst|Beschrijving|  
|------------|-----------------|   
|<a name="market" />BingAPIs markt|Antwoordheader.<br /><br /> De markt die wordt gebruikt door de aanvraag. Het formulier is \<languageCode\>-\<countryCode\>. En-US.|  
|<a name="traceid" />BingAPIs TraceId|Antwoordheader.<br /><br /> De ID van de vermelding die de details van de aanvraag bevat. Wanneer een fout optreedt, vastleggen deze ID. Als u niet kunt bepalen en los het probleem, zijn deze ID samen met andere gegevens dat u opgeeft dat het ondersteuningsteam.|  
|<a name="subscriptionkey" />OCP-Apim-Subscription-Key|De aanvraagheader is vereist.<br /><br /> De sleutel van het abonnement dat u hebt ontvangen toen u zich registreerde voor deze service in [cognitieve Services](https://www.microsoft.com/cognitive-services/).|  
|<a name="clientid" />X-MSEdge-ClientID|Optionele aanvraag en antwoord-header.<br /><br /> Deze header Bing gebruikt om gebruikers met consistent gedrag via Bing-API-aanroepen. Bing lijnen vaak nieuwe functies en verbeteringen en gebruikt de client-ID als een sleutel voor het toewijzen van verkeer op verschillende vlucht. Als u niet dezelfde client-ID voor een gebruiker over meerdere aanvragen gebruikt, kunnen Bing kan de gebruiker toewijzen aan meerdere conflicterende vlucht. Wordt toegewezen aan meerdere conflicterende vlucht kan leiden tot een inconsistente gebruikerservaring. Bijvoorbeeld, als de tweede aanvraag de toewijzing van een andere vlucht dan de eerste heeft, de ervaring mogelijk onverwacht. Bing kunt ook de client-ID gebruiken om aan te passen webresultaten voor deze client-id's in zoekgeschiedenis, bieden een rijkere ervaring voor de gebruiker.<br /><br /> Deze header Bing ook gebruikt voor het resultaat classificaties verbeteren door het analyseren van de activiteit die is gegenereerd door een client-ID. De verbeteringen relevantie helpen met een betere resultaten geleverd door Bing-API's en op zijn beurt hoger schakelt doorklikken tarieven voor de consument API.<br /><br />Hier volgen de basisgebruik regels die betrekking hebben op deze header.<br /><ul><li>Elke gebruiker die de toepassing wordt gebruikt op het apparaat moet een unieke, Bing gegenereerd client-ID.<br /><br/>Als u deze header niet in de aanvraag opnemen, wordt Bing een ID gegenereerd en wordt in de header X-MSEdge-ClientID-antwoord geretourneerd. De enige keer dat u deze header niet in een aanvraag opnemen moet is de eerste keer dat de gebruiker maakt gebruik van uw app op het apparaat.<br /><br/></li><li>De client-ID gebruiken voor elke Bing-API-aanvraag die uw app voor deze gebruiker op het apparaat maakt.<br /><br/></li><li>**Let op:** moet u ervoor zorgen dat deze Client-ID niet gerelateerd aan authenticatable gebruikersgegevens account is.</li><br/><li>Behouden van de client-ID. Om te blijven behouden de ID in een browser-app, gebruikt u een permanente HTTP-cookie om te controleren of dat de ID wordt gebruikt in alle sessies. Gebruik een sessiecookie niet. Gebruik voor andere apps, zoals mobiele apps, de permanente opslag van het apparaat om vast te leggen van de-ID.<br /><br/>De volgende keer dat de gebruiker maakt gebruik van uw app op het apparaat ophalen van de client-ID die u persistent hebt gemaakt.</li></ul><br /> **Opmerking:** Bing antwoorden mogelijk of mag deze header bevatten. Als het antwoord deze header bevat, vastleggen van de client-ID en deze gebruiken voor alle volgende Bing-aanvragen voor de gebruiker op het apparaat.<br /><br /> **Opmerking:** als u de X-MSEdge-ClientID opneemt, moet u cookies niet opnemen in de aanvraag.|  
|<a name="clientip" />X-MSEdge-client-IP|De aanvraagheader is optioneel.<br /><br /> Het IPv4- of IPv6-adres van het clientapparaat. Het IP-adres wordt gebruikt voor het detecteren van de locatie van de gebruiker. Bing gebruikt de locatie-informatie veilig zoekgedrag bepalen.<br /><br />  Niet verbergen, weergeven het adres (bijvoorbeeld door het laatste octet wijzigen in 0). De resultaten van het adres in de locatie niet overal in de buurt van de werkelijke locatie van het apparaat wordt obfuscating, waardoor de Bing voor de onjuiste resultaten.|  
<br /><br /></li></ul>   

## <a name="query-parameters"></a>Queryparameters  
De aanvraag kan de volgende queryparameters bevatten. Zie de vereiste kolom voor vereiste parameters. Moet u de URL de queryparameters coderen. De query moet een absolute URL met een schema http of https; niet ondersteund relatieve URL's of andere schema's, zoals ftp: / /
  
  
|Naam|Waarde|Type|Vereist|  
|----------|-----------|----------|--------------|  
|<a name="mkt" />Mkt|De markt waar de resultaten afkomstig zijn uit. <br /><br />Zie voor een lijst van mogelijke waarden voor markt [markt Codes](#market-codes).<br /><br /> **Opmerking:** de URL Preview-API ondersteunt momenteel alleen VS Geografie en Engelse taal.<br /><br />|Reeks|Ja|  
|<a name="query" />q|De URL voor de preview|Reeks|Ja|  
|<a name="responseformat" />responseFormat|Het mediatype als u wilt gebruiken voor het antwoord. De volgende zijn waarden mogelijk niet hoofdlettergevoelig.<br /><ul><li>JSON</li><li>JSONLD</li></ul><br /> De standaardwaarde is JSON. Zie voor meer informatie over de JSON-objecten dat het antwoord bevat [antwoord objecten](#response-objects).<br /><br />  Als u JsonLd opgeeft, bevat de antwoordtekst JSON-LD-objecten die de zoekresultaten bevatten. Zie voor meer informatie over de JSON-LD [JSON-LD](http://json-ld.org/).|Reeks|Nee|
|<a name="safesearch"/>veilig zoeken|Ongeldige inhoud voor volwassenen of illegale inhoud is geblokkeerd met foutcode 400, en de *isFamilyFriendly* vlag wordt niet geretourneerd. <p>Voor juridische inhoud voor volwassenen ziet hieronder u het gedrag. Statuscode 200, retourneert en de *isFamilyFriendly* vlag is ingesteld op false.<ul><li>veilig zoeken = strict: titel, beschrijving, URL en de installatiekopie, worden niet geretourneerd.</li><li>veilig zoeken = gemiddeld; Titel, URL en beschrijving, maar niet de beschrijvende afbeelding ophalen.</li><li>veilig zoeken = off; Haal alle antwoord objecten/elementen – titel, de URL, de beschrijving en de installatiekopie.</li></ul> |Reeks|Niet vereist. </br> Standaard veilig zoeken = strict.| 

## <a name="response-objects"></a>Antwoord-objecten  
Het antwoordschema van het is ofwel een [webpagina] of ErrorResponse, zoals in de Web-API voor zoeken. Als de aanvraag is mislukt, het object dat op het hoogste niveau is de [ErrorResponse](#errorresponse) object.


|Object|Beschrijving|  
|------------|-----------------|  
|[Webpagina](#webpage)|Bovenste niveau JSON-object met de kenmerken van preview.|  

  
### <a name="error"></a>Fout  
Hiermee definieert u de volgende fout is opgetreden.  
  
|Element|Beschrijving|Type|  
|-------------|-----------------|----------|  
|<a name="error-code" />Code|De foutcode die de categorie van de fout identificeert. Zie voor een lijst van mogelijke codes [foutcodes](#error-codes).|Reeks|  
|<a name="error-message" />Bericht|Een beschrijving van de fout.|Reeks|  
|<a name="error-moredetails" />moreDetails|Een beschrijving op die aanvullende informatie over de fout biedt.|Reeks|  
|<a name="error-parameter" />Parameter|De queryparameter in de aanvraag die de fout heeft veroorzaakt.|Reeks|  
|<a name="error-subcode" />SubCode|De foutcode die de fout identificeert. Bijvoorbeeld, als `code` is InvalidRequest, `subCode` mogelijk ParameterInvalid of ParameterInvalidValue. |Reeks|  
|<a name="error-value" />Waarde|De queryparameter-waarde die niet geldig is.|Reeks|  
  

### <a name="errorresponse"></a>ErrorResponse  
Het object van het hoogste niveau dat het antwoord bevat als de aanvraag is mislukt.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|  
|_Type|Type hint.|Reeks|  
|<a name="errors" />Fouten|Een lijst van fouten die worden beschreven de redenen waarom de aanvraag is mislukt.|[Fout](#error)]|   
  

### <a name="webpage"></a>Webpagina  
Informatie over definieert een de webpagina in preview.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|
|naam|De titel van de pagina, niet per se de HTML-titel|Reeks|
|url|De URL die daadwerkelijk is verkend (aanvraag kan hebt gevolgd omleidingen)|Reeks|  
|beschrijving|Korte beschrijving van de de pagina en de inhoud|Reeks|  
|isFamilyFriendly|Meest nauwkeurige voor items in de index web; real-time op te halen komen deze detectie op basis van uitsluitend op de URL en niet op de pagina-inhoud|booleaans|
|primaryImageOfPage/contentUrl|De URL van een representatieve afbeelding wilt opnemen in de Preview-versie|Reeks| 


### <a name="identifiable"></a>Persoonsgegevens
|Naam|Waarde|Type|  
|-------------|-----------------|----------|
|id|Een bron-id|Reeks|
 

## <a name="error-codes"></a>Foutcodes

Hier volgen de mogelijke HTTP-statuscodes die een aanvraag als resultaat geeft.  
  
|Statuscode|Beschrijving|  
|-----------------|-----------------|  
|200|Gelukt.|  
|400|Een van de queryparameters is ontbreekt of ongeldig.| 
|400|ServerError, ResourceError subCode: de aangevraagde URL kan niet worden bereikt.|
|400|ServerError, ResourceError subCode: de aangevraagde URL heeft geen een code (inclusief als er een HTTP 404 geretourneerd) geretourneerd.|
|400|InvalidRequest, geblokkeerd subCode: de aangevraagde URL inhoud voor volwassenen kunnen bevatten en is geblokkeerd| 
|401|De abonnementssleutel ontbreekt of is niet geldig.|  
|403|De gebruiker is geverifieerd (bijvoorbeeld, ze gebruikt een geldig abonnement-sleutel), maar ze geen machtiging voor de aangevraagde bron.<br /><br /> Bing mogelijk deze status ook geretourneerd als de aanroeper hun query's per maand quotum overschreden.|  
|410|De aanvraag gebruikt HTTP in plaats van het HTTPS-protocol. HTTPS is de enige ondersteunde protocollen.|  
|429|De aanroeper hun query's per tweede quotum is overschreden.|  
|500|Onverwachte serverfout.|

Als de aanvraag is mislukt, wordt het antwoord bevat een [ErrorResponse](#errorresponse) object, dat een lijst met bevat [fout](#error) objecten die wordt beschreven wat de oorzaak van de fout. Als de fout is gerelateerd aan een parameter, de `parameter` veld identificeert de parameter die het probleem is. En als de fout is gerelateerd aan een parameterwaarde de `value` veld geeft de waarde die is niet geldig.

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

Hier volgen de waarden voor de mogelijke fout code en de onderliggende fout.

|Code|SubCode|Beschrijving
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>Niet geïmplementeerd|HTTP-statuscode is 500.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Geblokkeerd|Bing retourneert InvalidRequest wanneer een deel van de aanvraag niet geldig is. Bijvoorbeeld, een vereiste parameter ontbreekt of een parameterwaarde is niet geldig.<br/><br/>Als de fout ParameterMissing of ParameterInvalidValue is, is de HTTP-statuscode 400.<br/><br/>Als u het HTTP-protocol in plaats van HTTPS gebruikt, Bing retourneert HttpNotAllowed en de HTTP-statuscode 410 is.
|RateLimitExceeded|Er is geen onderliggende codes|Bing retourneert RateLimitExceeded wanneer u uw query's per seconde (QPS) of de query's per maand (QPM)-quota overschrijdt.<br/><br/>Als u QPS overschrijdt, Bing HTTP-statuscode 429 retourneert en als u QPM overschrijdt, Bing 403 retourneert.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing retourneert InvalidAuthorization wanneer Bing de aanroeper kan niet worden geverifieerd. Bijvoorbeeld, de `Ocp-Apim-Subscription-Key` header ontbreekt of de abonnementssleutel is niet geldig.<br/><br/>Redundantie treedt op als u meer dan één verificatiemethode opgeven.<br/><br/>Als de fout InvalidAuthorization is, is de HTTP-statuscode 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing retourneert InsufficientAuthorization wanneer de aanroeper is niet gemachtigd voor toegang tot de bron. Dit kan gebeuren als de abonnementssleutel is uitgeschakeld of is verlopen. <br/><br/>Als de fout InsufficientAuthorization is, is de HTTP-statuscode 403.

## <a name="next-steps"></a>Volgende stappen
- [Quick Start C#](csharp.md)
- [Java-Quick Start](java-quickstart.md)
- [JavaScript Quick Start](javascript.md)
- [Knooppunt Quick Start](node-quickstart.md)
- [Snelstartgids voor Python](python-quickstart.md)

