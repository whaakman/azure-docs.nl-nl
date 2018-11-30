---
title: Bing lokale bedrijven zoeken-API voor Bing versie 7-verwijzing | Microsoft Docs
description: Beschrijving van de programma-elementen van de Bing API voor zoeken van lokale bedrijven.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-local-business
ms.topic: article
ms.date: 11/01/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: b985521fdcfc5ca67b6410719ad72c0962004df6
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52499599"
---
# <a name="bing-local-business-search-api-v7-reference"></a>Verwijzing van lokale bedrijven zoeken-API voor Bing versie 7

De lokale bedrijven zoeken-API verzendt een zoekopdracht naar Bing restaurants, hotels of andere lokale bedrijven omvatten de resultaten ophalen. Voor locaties, kunt de query de naam van de lokale bedrijven of Selecteer een categorie (bijvoorbeeld restaurants in de buurt) opgeven. Entiteitsresultaten zijn personen, plaatsen of dingen. Plaats in deze context is bedrijfsentiteiten, Staten, landen, enzovoort.  

In deze sectie vindt u technische gegevens over de antwoordobjecten, en de query-parameters en headers die invloed hebben op de lijst met zoekresultaten. Zie voor meer voorbeelden die laten zien hoe aanvragen, [lokale bedrijven zoeken C# snelstartgids](quickstarts/local-quickstart.md) of [lokale bedrijven zoeken Java-quickstart](quickstarts/local-search-java-quickstart.md). 
  
Zie voor meer informatie over kopteksten die aanvragen moeten [Headers](#headers).  
  
Zie voor meer informatie over queryparameters die aanvragen moeten bevatten [queryparameters](#query-parameters).  
  
Zie voor meer informatie over de JSON-objecten dat het antwoord bevat, [antwoordobjecten](#response-objects).

Zie voor meer informatie over het toegestane gebruik en weergave van resultaten [gebruiken en weergavevereisten](use-display-requirements.md).


  
## <a name="endpoint"></a>Eindpunt  
Om aan te vragen lokale bedrijfsresultaten te verbeteren, verzendt u een GET-aanvraag naar: 

``` 
https://api.cognitive.microsoft.com/bing/v7.0/localbusinesses/search

```
  
In de aanvraag moet u het HTTPS-protocol gebruiken.  
  
> [!NOTE]
> De maximale URL-lengte is 2048 tekens. Om ervoor te zorgen dat uw URL-lengte de limiet niet overschrijdt, moet de maximale lengte van de queryparameters maximaal 1500 tekens. Als de URL is langer dan 2048 tekens, retourneert de server 404 niet gevonden.  
  
  
## <a name="headers"></a>Headers  
Hier volgen de headers die bijvoorbeeld een aanvraag en antwoord bevatten.  
  
|Header|Beschrijving|  
|------------|-----------------|  
|Akkoord|Optionele aanvraagheader.<br /><br /> De standaard-mediatype is application/json. Om op te geven dat het antwoord gebruiken [JSON-LD](http://json-ld.org/), de Accept-header ingesteld op application/ld + json.|  
|<a name="acceptlanguage" />Accept-Language|Optionele aanvraagheader.<br /><br /> Een door komma's gescheiden lijst met talen die moet worden gebruikt voor gebruikersinterfacetekenreeksen. De lijst is in aflopende volgorde van voorkeur. Zie [RFC2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) voor meer informatie, waaronder de verwachte indeling.<br /><br /> Deze header en de queryparameter [setLang](#setlang) sluiten elkaar uit&mdash;geef ze niet beide op.<br /><br /> Als u deze header instelt, moet u ook de queryparameter [cc](#cc) opgeven. Om de markt te bepalen waarvoor resultaten moeten worden geretourneerd, gebruikt Bing de eerste ondersteunde taal die wordt gevonden in de lijst en combineert deze met de parameterwaarde `cc`. Als de lijst geen ondersteunde taal bevat, vindt Bing de dichtstbijzijnde taal en markt die de aanvraag ondersteunen, of gebruikt een geaggregeerde of standaardmarkt voor de resultaten. Zie de header BingAPIs-Market om de markt te bepalen die Bing heeft gebruikt.<br /><br /> Gebruik deze header en de queryparameter `cc` alleen als u meerdere talen opgeeft. Gebruik anders de queryparameters [mkt](#mkt) en [setLang](#setlang).<br /><br /> Een gebruikersinterfacetekenreeks is een tekenreeks die wordt gebruikt als label in een gebruikersinterface. Er zijn maar weinig gebruikersinterfacetekenreeksen in de JSON-antwoordobjecten. De opgegeven taal wordt toegepast op koppelingen naar Bing.com-eigenschappen in de antwoordobjecten.|  
|<a name="market" />BingAPIs-Market|Antwoordheader.<br /><br /> De markt die wordt gebruikt door de aanvraag. De notatie is \<languageCode\>-\<countryCode\>. Bijvoorbeeld: nl-NL.|  
|<a name="traceid" />BingAPIs-TraceId|Antwoordheader.<br /><br /> De id van de logboekvermelding die de details van de aanvraag bevat. Registreer deze id wanneer er een fout optreedt. Als u het probleem niet kunt vaststellen en oplossen, neemt u deze id op bij de andere informatie die u aan het ondersteuningsteam verstrekt.|  
|<a name="subscriptionkey" />Ocp-Apim-Subscription-Key|Vereiste aanvraagheader.<br /><br /> De abonnementssleutel die u hebt ontvangen toen u zich hebt geregistreerd voor deze service in [Cognitive Services](https://www.microsoft.com/cognitive-services/).|  
|<a name="pragma" />Pragma|Optionele aanvraagheader<br /><br /> Bing retourneert standaard cache-inhoud, indien beschikbaar. Om te voorkomen dat Bing inhoud uit de cache retourneert, stelt u de Pragma-header in op no-cache (bijvoorbeeld: Pragma: no-cache).
|<a name="useragent" />User-Agent|Optionele aanvraagheader.<br /><br /> De gebruikersagent waarvan de aanvraag afkomstig is. Bing gebruikt de user-agent om mobiele gebruikers een geoptimaliseerde ervaring te bieden. Hoewel dit optioneel is, wordt u aangeraden deze header altijd op te geven.<br /><br /> De user-agent moet dezelfde tekenreeks zijn die elke veelgebruikte browser verzendt. Zie [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) voor meer informatie over gebruikersagenten.<br /><br /> Hier volgen enkele voorbeelden van user-agent-tekenreeksen.<br /><ul><li>Windows Phone&mdash;Mozilla/5.0 (compatibel; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)<br /><br /></li><li>Android&mdash;Mozilla/5.0 (Linux; U; Android 2.3.5; nl-nl; SCH-I500 Build/GINGERBREAD) AppleWebKit/533.1 (KHTML; like Gecko) Version/4.0 Mobile Safari/533.1<br /><br /></li><li>iPhone&mdash;Mozilla/5.0 (iPhone; CPU iPhone OS 6_1 like Mac OS X) AppleWebKit/536.26 (KHTML; zoals Gecko) Mobile/10B142 iPhone4;1 BingWeb/3.03.1428.20120423<br /><br /></li><li>PC&mdash;Mozilla/5.0 (Windows NT 6.3; WOW64; Trident/7.0; Touch; rv:11.0) zoals Gecko<br /><br /></li><li>iPad&mdash;Mozilla/5.0 (iPad; CPU OS 7_0 zoals Mac OS X) AppleWebKit/537.51.1 (KHTML, zoals Gecko) Version/7.0 Mobile/11A465 Safari/9537.53</li></ul>|
|<a name="clientid" />X-MSEdge-ClientID|Optionele aanvraag- en antwoord-header.<br /><br /> Bing gebruikt deze header om gebruikers consistent gedrag te bieden bij Bing API-aanroepen. Bing introduceert nieuwe functies en verbeteringen vaak in flights, en gebruikt de client-ID als sleutel voor het toewijzen van verkeer aan verschillende flights. Als u niet dezelfde client-id gebruikt voor een gebruiker voor meerdere aanvragen, kan Bing de gebruiker toewijzen aan meerdere conflicterende flights. Toewijzing aan meerdere conflicterende flights kan leiden tot een inconsistente gebruikerservaring. Als de tweede aanvraag bijvoorbeeld een andere flighttoewijzing heeft dan de eerste, kan de ervaring onverwacht zijn. Bing kan de client-id ook gebruiken om webresultaten aan te passen aan de zoekgeschiedenis van die client-id, waardoor de gebruiker een rijkere ervaring krijgt.<br /><br /> Bing gebruikt deze header ook om de rangschikking van resultaten te verbeteren door de activiteit te analyseren die wordt gegenereerd door een client-id. De relevantie-verbeteringen helpen de kwaliteit van de resultaten die door Bing-API's worden geleverd te verbeteren, en maken op hun beurt hogere doorklikpercentages mogelijk voor de API-consument.<br /><br /> **BELANGRIJK:** Hoewel deze header optioneel is, dient u deze als verplicht te beschouwen. Door de client-id te behouden voor meerdere aanvragen voor dezelfde combinatie van eindgebruiker en apparaat, zorgt u voor 1) een consistente gebruikerservaring van de API-consument, en 2) hogere doorklikpercentages via een betere kwaliteit van de resultaten van de Bing-API's.<br /><br /> Hieronder volgen de basisgebruiksregels die van toepassing zijn op deze header.<br /><ul><li>Elke gebruiker die uw toepassing op het apparaat gebruikt, moet een unieke door Bing gegenereerde client-id hebben.<br /><br/>Als u deze header niet in de aanvraag opneemt, genereert Bing een id en retourneert deze in de X-MSEdge-ClientID-antwoordheader. De enige keer dat u deze header NIET in een aanvraag moet opnemen, is de eerste keer dat de gebruiker uw app op dat apparaat gebruikt.<br /><br/></li><li>Gebruik de client-id voor elke Bing-API-aanvraag die uw app voor deze gebruiker op het apparaat doet.<br /><br/></li><li>**Let op:** moet u ervoor zorgen dat deze Client-ID niet gerelateerd aan een authenticatable gebruikersaccountgegevens is.</li><br/><li>Maak de client-id persistent. Gebruik in een browser-app een persistent HTTP-cookie om ervoor te zorgen dat de id in alle sessies wordt gebruikt. Gebruik geen sessiecookie. Voor andere apps, zoals mobiele apps, gebruikt u de persistente opslag van het apparaat om de id persistent te maken.<br /><br/>De volgende keer dat de gebruiker uw app op dat apparaat gebruikt, haalt u de client-id op die u persistent hebt gemaakt.</li></ul><br /> **OPMERKING:** Bing-antwoorden kunnen deze header al dan niet bevatten. Als het antwoord deze header bevat, registreert u de client-id en gebruikt u deze voor alle volgende Bing-aanvragen voor de gebruiker op dat apparaat.<br /><br /> **OPMERKING:** als u de X-MSEdge-ClientID opneemt, moet u geen cookies opnemen in de aanvraag.|  
|<a name="clientip" />X-MSEdge-ClientIP|Optionele aanvraagheader.<br /><br /> Het IPv4- of IPv6-adres van het clientapparaat. Het IP-adres wordt gebruikt voor het detecteren van de locatie van de gebruiker. Bing gebruikt de locatie-informatie om het gedrag van Veilig Zoeken te bepalen.<br /><br /> **OPMERKING:** Hoewel dit optioneel is, wordt u aangeraden deze header en de X-Search-Location-header altijd op te geven.<br /><br /> Verdoezel het adres niet (bijvoorbeeld door het laatste octet te wijzigen in 0). Wanneer u het adres verdoezelt, is de locatie totaal niet in de buurt van de werkelijke locatie van het apparaat, wat ertoe kan leiden dat Bing onjuiste resultaten geeft.|  
|<a name="location" />X-Search-Location|Optionele aanvraagheader.<br /><br /> Een met puntkomma's gescheiden lijst met sleutel-waardeparen die de geografische locatie van de client beschrijven. Bing gebruikt de locatie-informatie om het gedrag van Veilig Zoeken te bepalen en relevante lokale inhoud te retourneren. Geef het sleutel-waardepaar op als \<sleutel\>:\<waarde\>. Hier volgen de sleutels die u gebruikt om de locatie van de gebruiker op te geven.<br /><br /><ul><li>LAT&mdash;de breedte van de locatie van de client, in graden. De breedtegraad moet groter dan of gelijk zijn aan -90.0 en kleiner dan of gelijk aan +90.0. Negatieve waarden geven zuidelijke breedtegraden aan, en positieve waarden noordelijke.<br /><br /></li><li>lange&mdash;de lengtegraad van de locatie van de client, in graden. De lengtegraad moet groter dan of gelijk zijn aan -180.0 en kleiner dan of gelijk aan +180.0. Negatieve waarden geven westelijke lengtegraden aan, en positieve waarden oostelijke.<br /><br /></li><li>RE&mdash; de straal in meters, waarmee de horizontale nauwkeurigheid van de coördinaten. Geef de waarde door die wordt geretourneerd door de locatieservice van het apparaat. Typische waarden zijn bijvoorbeeld 22 m voor GPS/Wi-Fi, 380 m voor triangulatie op basis van gsm-masten en 18.000 m voor reverse IP-lookup.<br /><br /></li><li>TS&mdash; de UTC-UNIX-timestamp van wanneer de client op de locatie is. (Het UNIX-tijdstempel is het aantal seconden sinds 1 januari 1970.)<br /><br /></li><li>head&mdash;Optioneel. De relatieve koers of reisrichting van de client. Geef de reisrichting op als graden van 0 t/m 360, gerekend met de klok mee ten opzichte van het ware noorden. Geef deze sleutel alleen op als de `sp`-sleutel niet nul is.<br /><br /></li><li>SP&mdash; de horizontale uit te voeren (snelheid), in meter per seconde, die de client-apparaat is verzonden.<br /><br /></li><li>ALT&mdash; de hoogte van het clientapparaat, in meters.<br /><br /></li><li>are&mdash;Optioneel. De straal, in meters, die de verticale nauwkeurigheid van de coördinaten aangeeft. RADIUS-standaard ingesteld op 50 kilometer zijn verwijderd. Geef deze sleutel alleen op als u de `alt`-sleutel opgeeft.<br /><br /></li></ul> **Opmerking:** Hoewel deze sleutels optioneel zijn, de informatie die u opgeeft, hoe nauwkeuriger de resultaten van de locatie zijn.<br /><br /> **Opmerking:** u wordt aangeraden om op te geven altijd de geografische locatie van de gebruiker. Het opgeven van de locatie is vooral belangrijk als het IP-adres van de client de fysieke locatie van de gebruiker niet nauwkeurig weergeeft (bijvoorbeeld als de client VPN gebruikt). Voor optimale resultaten moet u zowel deze header als de X-MSEdge-ClientIP-header opnemen, maar neem minimaal deze header op.|

> [!NOTE] 
> Vergeet niet dat de gebruiksvoorwaarden naleving van alle toepasselijke wetgeving vereisen, inclusief die over het gebruik van deze headers. In bepaalde rechtsgebieden, zoals Europa, is het bijvoorbeeld vereist om toestemming van de gebruiker te verkrijgen voordat bepaalde traceringsapparaten op gebruikersapparaten wordt geplaatst.
  

## <a name="query-parameters"></a>Queryparameters  
De aanvraag kan de volgende queryparameters bevatten. Zie de vereiste kolom voor vereiste parameters. Moet u de URL de queryparameters coderen.  
  
  
|Naam|Waarde|Type|Vereist|  
|----------|-----------|----------|--------------|
|<a name="count" />Aantal|Het aantal resuts moeten worden geretourneerd vanaf de index opgegeven door de `offset` parameter.|Reeks|Nee|   
|<a name="localCategories" />localCategories|Lijst met opties die zoeken op categorie zakelijke definiëren.  Zie [categorieën van lokale bedrijven zoeken](local-categories.md)|Reeks|Nee|  
|<a name="mkt" />mkt|De markt waaruit de resultaten afkomstig zijn. <br /><br />Zie voor een lijst van mogelijke waarden van de markt, [markt Codes](#market-codes).<br /><br /> **Opmerking:** de lokale bedrijven zoeken-API ondersteunt momenteel alleen en-us markt en taal.<br /><br />|Reeks|Ja|
|<a name="offset"/>offset|De index om te starten van de resultaten die zijn opgegeven door de `count` parameter.|Geheel getal|Nee|  
|<a name="query" />q|Zoekterm van de gebruiker.|Reeks|Nee|  
|<a name="responseformat" />responseFormat|Het mediatype dat als u wilt gebruiken voor het antwoord. De volgende zijn waarden mogelijk niet hoofdlettergevoelig.<br /><ul><li>JSON</li><li>JSONLD</li></ul><br /> De standaardwaarde is JSON. Zie voor meer informatie over de JSON-objecten dat het antwoord bevat, [Antwoordobjecten](#response-objects).<br /><br />  Als u JsonLd opgeeft, bevat de antwoordtekst JSON-LD objecten met de lijst met zoekresultaten. Zie voor meer informatie over de JSON-LD [JSON-LD](http://json-ld.org/).|Reeks|Nee|  
|<a name="safesearch" />safeSearch|Een filter dat wordt gebruikt voor het filteren van inhoud voor volwassenen. Hier volgen de mogelijke niet-hoofdlettergevoelige filterwaarden.<br /><ul><li>Uit&mdash;webpagina's met volwassen tekst, afbeeldingen of video's retourneren.<br /><br/></li><li>Gemiddeld&mdash;webpagina's met volwassen tekst, maar niet volwassen afbeeldingen of video's retourneren.<br /><br/></li><li>Strikte&mdash;webpagina's met volwassen tekst, afbeeldingen of video's niet retourneren.</li></ul><br /> De standaardwaarde is Moderate.<br /><br /> **Opmerking:** als de aanvraag afkomstig van een markt is van die Bing volwassenen beleid vereist dat `safeSearch` is ingesteld op strikt, Bing negeert de `safeSearch` waarde en maakt gebruik van strikt.<br/><br/>**OPMERKING:** Als u de zoekoperator `site:` gebruikt, is het mogelijk dat het antwoord inhoud voor volwassenen bevat, ongeacht de instelling van de queryparameter `safeSearch`. Gebruik `site:` alleen als u zich bewust bent van de inhoud op de site en uw scenario de mogelijkheid van inhoud voor volwassenen ondersteunt. |Reeks|Nee|  
|<a name="setlang" />setLang|De taal die moet worden gebruikt voor gebruikersinterfacetekenreeksen. Geef de taal op met behulp van de tweeletterige ISO 639-1 taalcode. De taalcode voor Nederlands is bijvoorbeeld NL. De standaardwaarde is EN (Engels).<br /><br /> Hoewel dit optioneel is, moet u altijd de taal opgeven. Doorgaans stelt u `setLang` in op dezelfde taal die is opgegeven door `mkt`, tenzij de gebruiker wil dat gebruikersinterfacetekenreeksen in een andere taal worden weergegeven.<br /><br /> Deze parameter en de header [Accept-Language](#acceptlanguage) sluiten elkaar uit&mdash;geef ze niet beide op.<br /><br /> Een gebruikersinterfacetekenreeks is een tekenreeks die wordt gebruikt als label in een gebruikersinterface. Er zijn maar weinig gebruikersinterfacetekenreeksen in de JSON-antwoordobjecten. De opgegeven taal wordt ook toegepast op koppelingen naar Bing.com-eigenschappen in de antwoordobjecten.|Reeks|Nee| 


## <a name="response-objects"></a>Antwoordobjecten  
Hieronder vindt u de JSON-antwoord-objecten die het antwoord kan bevatten. Als de aanvraag is geslaagd, wordt het object op het hoogste niveau in het antwoord is de [SearchResponse](#searchresponse) object. Als de aanvraag mislukt, wordt het object op het hoogste niveau is het [ErrorResponse](#errorresponse) object.


|Object|Beschrijving|  
|------------|-----------------|  
|[Plaats](#place)|Informatie over een lokale bedrijven, zoals een restaurant of in een hotel definieert.|  

  
### <a name="error"></a>Fout  
Hiermee definieert u de fout is opgetreden.  
  
|Element|Beschrijving|Type|  
|-------------|-----------------|----------|  
|<a name="error-code" />Code|De foutcode die aangeeft van de categorie van de fout. Zie voor een lijst van mogelijke codes, [foutcodes](#error-codes).|Reeks|  
|<a name="error-message" />Bericht|Een beschrijving van de fout.|Reeks|  
|<a name="error-moredetails" />moreDetails|Een beschrijving met aanvullende informatie over de fout.|Reeks|  
|<a name="error-parameter" />Parameter|De queryparameter in de aanvraag die de fout heeft veroorzaakt.|Reeks|  
|<a name="error-subcode" />De subCode|De foutcode die aangeeft van de fout. Bijvoorbeeld, als `code` InvalidRequest, is `subCode` mogelijk ParameterInvalid of ParameterInvalidValue. |Reeks|  
|<a name="error-value" />Waarde|De queryparameter-waarde die niet geldig is.|Reeks|  
  

### <a name="errorresponse"></a>ErrorResponse  
Het object van het type op het hoogste niveau dat het antwoord bevat als de aanvraag is mislukt.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|  
|_Type|Type hint.|Reeks|  
|<a name="errors" />Fouten|Een lijst van fouten die worden beschreven van de redenen waarom de aanvraag is mislukt.|[Fout](#error)]|  

  
  
### <a name="license"></a>Licentie  
Hiermee definieert u de licentie waaronder de tekst of foto's kan worden gebruikt.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|  
|naam|De naam van de licentie.|Reeks|  
|url|De URL naar een website waar de gebruiker meer informatie over de licentie krijgt.<br /><br /> De naam en URL gebruiken om een hyperlink te maken.|Reeks|  


### <a name="link"></a>Koppeling  
Hiermee definieert u de onderdelen van een hyperlink.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|  
|_Type|Type hint.|Reeks|  
|tekst|De tekst.|Reeks|  
|url|EEN URL. Gebruik de URL en tekst voor het maken van een hyperlink weer te geven.|Reeks|  
  


  
### <a name="organization"></a>Organisatie  
Hiermee definieert u een uitgever.  
  
Houd er rekening mee dat de uitgever hun naam of de website of beide bieden kan.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|  
|naam|Naam van de uitgever.|Reeks|  
|url|De URL naar de website van de uitgever.<br /><br /> Houd er rekening mee dat de uitgever niet een website bieden mogelijk.|Reeks|  
  
  

### <a name="place"></a>Plaats  
Informatie over een lokale bedrijven, zoals een restaurant of een hotel definieert.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|  
|_Type|Type-hint die kan worden ingesteld op een van de volgende:<br /><br /><ul><li>Hotel</li><li>LocalBusiness<br /></li><li>Restaurant</ul><li>|Reeks|  
|Adres|Het postadres van waar de entiteit zich bevindt.|[PostalAddress](#postaladdress)|  
|entityPresentationInfo|Meer informatie over de entiteit, zoals de hints die u gebruiken kunt om te bepalen van de entiteit-type. Of het is bijvoorbeeld een restaurant of in een hotel. De `entityScenario` ListItem is ingeschakeld.|[entityPresentationInfo](#entitypresentationinfo)|  
|naam|De naam van de entiteit.|Reeks|  
|Telefoonnummer|Het telefoonnummer van de entiteit.|Reeks|  
|url|De URL naar de website van de entiteit.<br /><br /> Gebruik deze URL samen met de naam van de entiteit op een hyperlink maken die terugkoppeling leidt de gebruiker naar de website van de entiteit.|Reeks|  
|webSearchUrl|De URL van de Bing-zoekresultaten voor deze locatie.|Reeks| 
  
  
### <a name="querycontext"></a>QueryContext  
Hiermee definieert u de querycontext die Bing voor de aanvraag gebruikt.  
  
|Element|Beschrijving|Type|  
|-------------|-----------------|----------|  
|adultIntent|Een Booleaanse waarde die aangeeft of de opgegeven query volwassenen heeft. De waarde is **waar** als de query volwassenen heeft; anders **false**.|Booleaans|  
|alterationOverrideQuery|De queryreeks gebruiken om af te dwingen van Bing voor het gebruik van de oorspronkelijke reeks. Bijvoorbeeld, als de query-tekenreeks is *saling downwind*, de query-tekenreeks voor het overschrijven is *+ saling downwind*. Vergeet niet om te coderen van de querytekenreeks die resulteert in *% 2Bsaling + downwind*.<br /><br /> Dit veld wordt alleen opgenomen als de oorspronkelijke querytekenreeks een fout spelling bevat.|Reeks|  
|alteredQuery|De query-tekenreeks die wordt gebruikt door Bing voor het uitvoeren van de query. Bing maakt gebruik van de gewijzigde query-tekenreeks als de oorspronkelijke query-tekenreeks spelfouten opgenomen. Bijvoorbeeld, als de query-tekenreeks is `saling downwind`, worden de gewijzigde query-tekenreeks `sailing downwind`.<br /><br /> Dit veld wordt alleen opgenomen als de oorspronkelijke querytekenreeks een fout spelling bevat.|Reeks|  
|askUserForLocation|Een Booleaanse waarde die aangeeft of de locatie van de gebruiker om nauwkeurig resultaten te bieden voor Bing is vereist. Als u de locatie van de gebruiker met behulp van opgegeven de [X-MSEdge-client-IP](#clientip) en [X zoeklocatie](#location) headers, kunt u dit veld negeren.<br /><br /> Voor de locatie op de hoogte-query's, zoals "weerbericht van vandaag" of "restaurants in de buurt" die de locatie van de gebruiker voor nauwkeurige resultaten, moeten dit veld is ingesteld op **waar**.<br /><br /> Voor de locatie op de hoogte lidmaatschapregelquery's op met de locatie (bijvoorbeeld: "Seattle weer'), in dit veld is ingesteld op **false**. Dit veld ook is ingesteld op **false** voor query's die geen locatie op de hoogte, zoals 'beste verkopers'.|Booleaans|  
|originalQuery|De query-tekenreeks die zijn opgegeven in de aanvraag.|Reeks|  

### <a name="identifiable"></a>Identificeerbare
|Naam|Waarde|Type|  
|-------------|-----------------|----------|
|id|Een resource-id|Reeks|
 
### <a name="rankinggroup"></a>RankingGroup
Definieert een groep resultaten, zoals mainline.
|Naam|Waarde|Type|  
|-------------|-----------------|----------|
|items|Een lijst met resultaten om weer te geven in de groep.|RankingItem|

### <a name="rankingitem"></a>RankingItem
Hiermee definieert u een item voor het resultaat van zoeken om weer te geven.
|Naam|Waarde|Type|  
|-------------|-----------------|----------|
|resultIndex|Een op nul gebaseerde index van het item in het antwoord om weer te geven. Als het item bevat geen dit veld, moet u alle items weergeven in het antwoord. Bijvoorbeeld, alle nieuwsartikelen weergegeven in het antwoord nieuws.|Geheel getal|
|answerType|Het antwoord waarin het item om weer te geven. Bijvoorbeeld, nieuws.<br /><br />Gebruik het type in het antwoord vinden in het object SearchResponse. Het type is de naam van een veld SearchResponse.<br /><br /> Echter, het antwoordtype alleen gebruiken als dit object het waardeveld bevat. anders wordt deze negeren.|Reeks|
|textualIndex|De index van het antwoord in textualAnswers om weer te geven.| Geheel getal zonder teken|
|waarde|De ID die u identificeert een antwoord om weer te geven of een item van het antwoord om weer te geven. Als de ID verwijst naar een antwoord, moet u alle items van het antwoord weergegeven.|Identificeerbare|

### <a name="rankingresponse"></a>RankingResponse  
Definieert wanneer de zoekopdracht in de resultaten pagina-inhoud moet worden geplaatst en in welke volgorde.  
  
|Naam|Waarde|  
|----------|-----------|  
|<a name="ranking-mainline" />mainline|De lijst met zoekresultaten om weer te geven in de hoofdlijnen.|  
|<a name="ranking-pole" />pool|De lijst met zoekresultaten die moeten worden toegepast als de meest zichtbaar behandeling (bijvoorbeeld weergegeven boven de hoofdlijnen en zijbalk).|  
|<a name="ranking-sidebar" />zijbalk|De lijst met zoekresultaten om weer te geven in de zijbalk.| 

### <a name="searchresponse"></a>SearchResponse  
Hiermee definieert u het op het hoogste niveau object dat het antwoord bevat als de aanvraag is geslaagd.  
  
Houd er rekening mee dat als de service een denial of service-aanval vermoedt, de aanvraag worden uitgevoerd (HTTP-statuscode is 200 OK); de hoofdtekst van het antwoord wordt echter niet leeg zijn.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|  
|_Type|Type-hint die is ingesteld op SearchResponse.|Reeks|  
|Plaatsen|Een lijst met entiteiten die relevant voor de zoekquery zijn.|JSON-object|  
|QueryContext|Een object dat de querytekenreeks die Bing voor de aanvraag gebruikt bevat.<br /><br /> Dit object bevat de query-tekenreeks, zoals opgegeven door de gebruiker. Het kan ook een gewijzigde querytekenreeks die Bing voor de query gebruikt als de querytekenreeks een fout spelling bevat bevatten.|[QueryContext](#querycontext)|  


## <a name="error-codes"></a>Foutcodes

Hier volgen de mogelijke HTTP-statuscodes die een aanvraag retourneert.  
  
|Statuscode|Beschrijving|  
|-----------------|-----------------|  
|200|Geslaagd.|  
|400|Een van de queryparameters is ontbreekt of is ongeldig.|  
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
- [Snelstartgids voor lokale bedrijven zoeken](quickstarts/local-quickstart.md)
- [Lokale bedrijven zoeken Java-snelstartgids](quickstarts/local-search-java-quickstart.md)
- [Lokale bedrijven zoeken Node-quickstart](quickstarts/local-search-node-quickstart.md)
- [Lokale bedrijven zoeken Python-snelstartgids](quickstarts/local-search-python-quickstart.md)