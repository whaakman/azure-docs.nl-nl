---
title: Project Answer Search-verwijzing
titlesuffix: Azure Cognitive Services
description: Referentie voor het Project antwoord zoeken-eindpunten.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: project-answer-search
ms.topic: reference
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 1149f4d5ec0a3ef55c435d0555f944329cf5b890
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48869587"
---
# <a name="project-answer-search-v7-reference"></a>Antwoord zoeken v7 verwijzing project

Bing antwoord SearchAPI neemt een queryparameter en retourneert een `searchResponse` met `answerType`: `facts` of `entities`. 

Toepassingen die gebruikmaken van de Search-API van het antwoord verzendt aanvragen naar het eindpunt met een URL naar het voorbeeld in een queryparameter.  De aanvraag moet bevatten de `q=searchTerm` parameter en *Ocp-Apim-Subscription-Key* header.   

De JSON-antwoord kan worden geparseerd voor feiten- en entiteiten die meer informatie over het object van het zoeken bevatten.

## <a name="endpoint"></a>Eindpunt
Om aan te vragen beantwoorden zoekresultaten, moet u een aanvraag verzenden naar het volgende eindpunt. De kop- en URL-parameters gebruiken om te definiëren verder specificaties.

GET-eindpunt: 
````
https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search?q=<searchTerm>&subscription-key=0123456789ABCDEF&mkt=en-us

````

De aanvraag moet de HTTPS-protocol gebruiken en omvatten queryparameter te volgen:
-  q =<URL> -de query die het object van search identificeert

Zie voor meer voorbeelden die laten zien hoe aanvragen, [C#-snelstartgids](c-sharp-quickstart.md) of [Java-quickstart](java-quickstart.md). 

De volgende secties vindt u technische gegevens over de antwoordobjecten, query-parameters en headers die invloed hebben op de lijst met zoekresultaten. 
  
Zie voor meer informatie over kopteksten die aanvragen moeten [Headers](#headers).  
  
Zie voor meer informatie over queryparameters die aanvragen moeten bevatten [queryparameters](#query-parameters).  
  
Zie voor meer informatie over de JSON-objecten dat het antwoord bevat, [antwoordobjecten](#response-objects).

De maximale URL-lengte is 2048 tekens. Om ervoor te zorgen dat uw URL-lengte de limiet niet overschrijdt, moet de maximale lengte van de queryparameters maximaal 1500 tekens. Als de URL is langer dan 2048 tekens, retourneert de server 404 niet gevonden.  

Zie voor meer informatie over het toegestane gebruik en weergave van resultaten [gebruiken en weergavevereisten](use-display-requirements.md). 

> [!NOTE]
> Sommige aanvraagheaders die relevant zijn voor andere search API's zijn niet van invloed op URL-voorbeeld
> - Pragma – de oproepende functie heeft geen controle over of URL-voorbeeld maakt gebruik van cache
> - Cache-Control: de oproepende functie heeft geen controle over of URL-voorbeeld maakt gebruik van cache
> - Gebruikersagent

> Bovendien sommige parameters zijn niet op dit moment zinvol is voor de Preview-API-URL, maar kunnen worden gebruikt in de toekomst bevindt voor verbeterde globalisatie. 
 
## <a name="headers"></a>Headers  
Hier volgen de headers die bijvoorbeeld een aanvraag en antwoord bevatten.  
  
|Koptekst|Beschrijving|  
|------------|-----------------|  
|Accepteren|De aanvraagheader is optioneel.<br /><br /> De standaard-mediatype is application/json. Om op te geven dat het antwoord gebruiken [JSON-LD](http://json-ld.org/), de Accept-header ingesteld op application/ld + json.|  
|<a name="acceptlanguage" />Accepteer taal|De aanvraagheader is optioneel.<br /><br /> Een door komma's gescheiden lijst met talen voor tekenreeksen voor interface. De lijst is in aflopende volgorde van voorkeur. Zie voor meer informatie, waaronder de verwachte indeling [RFC2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Deze header en het [taal instellen](#setlang) queryparameter zijn sluiten elkaar wederzijds uit&mdash;beide niet opgeven.<br /><br /> Als u deze header instellen, moet u ook opgeven de [cc](#cc) queryparameter. Om te bepalen de markt te retourneren van resultaten voor, Bing maakt gebruik van de eerste ondersteunde taal die wordt gevonden in de lijst, en combineert met de `cc` parameterwaarde. Als de lijst bevat geen een ondersteunde taal, Bing zoeken naar de dichtstbijzijnde taal en markt die ondersteuning biedt voor de aanvraag of het maakt gebruik van een samengevoegde of standaard markt voor de resultaten. Om te bepalen de markt die Bing gebruikt, Zie de BingAPIs-Market-header.<br /><br /> Gebruik deze header en het `cc` queryparameter alleen als u meerdere talen opgeven. Gebruik anders de [mkt](#mkt) en [taal instellen](#setlang) queryparameters.<br /><br /> Een tekenreeks van de interface is een tekenreeks die wordt gebruikt als een label in een gebruikersinterface. Er zijn enkele interface tekenreeksen in de JSON-antwoord-objecten. Een koppeling maakt naar Bing.com eigenschappen in de reactie-objecten zijn van toepassing de opgegeven taal.|  
|<a name="market" />BingAPIs-market te versnellen|Antwoordheader.<br /><br /> De markt die worden gebruikt door de aanvraag. Het formulier is \<languageCode\>-\<countryCode\>. Bijvoorbeeld: nl-nl.|  
|<a name="traceid" />BingAPIs TraceId|Antwoordheader.<br /><br /> De ID van de vermelding die de details van de aanvraag bevat. Wanneer er een fout optreedt, vastleggen van deze ID. Als u niet kunt bepalen en los het probleem, zijn deze ID samen met de andere informatie die het ondersteuningsteam op te geven.|  
|<a name="subscriptionkey" />OCP-Apim-Subscription-Key|De aanvraagheader is vereist.<br /><br /> De abonnementssleutel die u hebt ontvangen toen u zich hebt geregistreerd voor deze service in [Cognitive Services](https://www.microsoft.com/cognitive-services/).|  
|<a name="pragma" />Pragma|Optionele aanvraagheader<br /><br /> Bing retourneert standaard in de cache inhoud, indien beschikbaar. Om te voorkomen dat Bing retourneren in de cache inhoud, stelt u de header Pragma op no-cache (bijvoorbeeld Pragma: niet-cache).
|<a name="useragent" />Gebruikersagent|De aanvraagheader is optioneel.<br /><br /> De gebruikersagent die afkomstig zijn van de aanvraag. Bing maakt gebruik van de gebruikersagent zodat mobiele gebruikers beschikken over een optimale ervaring. Hoewel dit optioneel is, wordt u aangeraden altijd opgeven om deze header.<br /><br /> De gebruikersagent moet dezelfde tekenreeks waarmee elke gebruikte browser wordt verzonden. Zie voor meer informatie over gebruikersagenten [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Hier volgen enkele voorbeelden van gebruiker agent tekenreeksen.<br /><ul><li>Windows Phone&mdash;Mozilla 5.0 (compatibel; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)<br /><br /></li><li>Android&mdash;Mozilla 5.0 (Linux; U; Android 2.3.5; en-us; SCH-I500 Build/ONTBIJTKOEK) AppleWebKit/533.1 (KHTML; zoals Gecko) versie 4.0/Mobile Safari/533.1<br /><br /></li><li>iPhone&mdash;Mozilla 5.0 (iPhone; CPU iPhone OS 6_1, zoals Mac OS X) AppleWebKit/536.26 (KHTML; zoals Gecko) mobiele/10B142 iPhone4; 1 BingWeb/3.03.1428.20120423<br /><br /></li><li>PC&mdash;Mozilla 5.0 (Windows NT 6.3; WOW64-MODUS; Trident/7.0; Touch; RV:11.0), zoals Gecko<br /><br /></li><li>iPad&mdash;Mozilla 5.0 (iPad; CPU-OS 7_0, zoals Mac OS X) AppleWebKit/537.51.1 (zoals Gecko KHTML) versie/7.0 Mobile/11A465 Safari/9537.53</li></ul>|
|<a name="clientid" />X-MSEdge-ClientID|Optionele aanvraag en antwoord-header.<br /><br /> Bing maakt gebruik van deze header voor gebruikers met een consistent gedrag voor Bing-API-aanroepen. Bing vluchten vaak nieuwe functies en verbeteringen en gebruikt de client-ID als een sleutel voor het toewijzen van verkeer op verschillende vluchten. Als u gebruik niet dezelfde client-ID voor een gebruiker met meerdere aanvragen, kunnen Bing kan de gebruiker toewijzen aan meerdere conflicterende vluchten. Wordt toegewezen aan meerdere conflicterende vluchten kan leiden tot een inconsistente gebruikerservaring. Bijvoorbeeld, als de tweede aanvraag de toewijzing van een andere vlucht dan de eerste heeft, de ervaring mogelijk onverwachte. Bing kunt ook de client-ID gebruiken om aan te passen webresultaten voor deze client id zoekgeschiedenis, bieden een rijkere ervaring voor de gebruiker.<br /><br /> Bing gebruikt ook deze header om het resultaat classificaties verbeteren door het analyseren van de activiteit die is gegenereerd door een client-ID. Verbeteringen in de volgorde van relevantie helpen met hogere kwaliteit van de resultaten die worden geleverd door Bing-API's en op zijn beurt hoger kunnen via klikken tarieven voor de API-consument.<br /><br /> **Belangrijk:** Hoewel dit optioneel is, kunt u overwegen deze header vereist. Opslaan van de client-ID voor meerdere aanvragen voor de dezelfde gebruiker en apparaat combinatie kan (1) de consument API voor het ontvangen van een consistente gebruikerservaring, en 2) een hogere doorklikken tarieven via betere kwaliteit van de resultaten van de Bing-API's.<br /><br /> Hier volgen de basisgebruik regels die betrekking hebben op deze header.<br /><ul><li>Elke gebruiker die gebruikmaakt van uw toepassing op het apparaat moet een unieke, Bing gegenereerde client-ID.<br /><br/>Als u deze header in de aanvraag niet opgeeft, wordt Bing een ID gegenereerd en in de header X-MSEdge-ClientID-antwoord geretourneerd. De enige keer dat u moet niet deze header bevatten in een aanvraag is de eerste keer dat de gebruiker maakt gebruik van uw app op het apparaat.<br /><br/></li><li>De client-ID gebruiken voor elke API voor Bing-aanvraag die uw app voor deze gebruiker op het apparaat maakt.<br /><br/></li><li>**Let op:** moet u ervoor zorgen dat deze Client-ID niet gerelateerd aan een authenticatable gebruikersaccountgegevens is.</li><br/><li>Behouden van de client-ID. Om te blijven behouden de ID in een browser-app, gebruikt u een permanente HTTP-cookie om te controleren of dat de ID wordt gebruikt in alle sessies. Gebruik een sessiecookie niet. Voor andere apps, zoals mobiele apps, gebruikt u de permanente opslag van het apparaat om vast te leggen van de ID.<br /><br/>De volgende keer dat de gebruiker maakt gebruik van uw app op het apparaat, krijgen de client-ID die u wilt behouden.</li></ul><br /> **Opmerking:** Bing antwoorden kunnen of kan niet deze header bevatten. Als het antwoord deze header bevat, vastleggen van de client-ID en deze gebruiken voor alle volgende Bing-aanvragen voor de gebruiker op het apparaat.<br /><br /> **Opmerking:** als u de X-MSEdge-ClientID opneemt, moet u cookies niet opnemen in de aanvraag.|  
|<a name="clientip" />X-MSEdge-client-IP|De aanvraagheader is optioneel.<br /><br /> Het IPv4- of IPv6-adres van het clientapparaat. Het IP-adres wordt gebruikt voor het detecteren van de locatie van de gebruiker. De locatie-informatie Bing gebruikt om veilige zoekgedrag te bepalen.<br /><br /> **Opmerking:** Hoewel dit optioneel is, u wordt aangeraden deze header en de X-Search-Location-header altijd opgeven.<br /><br /> Onleesbaar niet het adres maakt (bijvoorbeeld door het laatste achttal werd wijzigen in 0). Obfuscating de adres-resultaten op de locatie niet wordt overal in de buurt van de werkelijke locatie van het apparaat, die kan leiden tot onjuiste resultaten voor Bing.|  
|<a name="location" />X-Search-locatie|De aanvraagheader is optioneel.<br /><br /> Een met puntkomma's gescheiden lijst met sleutel/waarde-paren die de geografische locatie van de client beschrijven. Bing maakt gebruik van de locatie-informatie om te bepalen het gedrag van veilig zoeken en om terug te keren relevante lokale inhoud. Geef de sleutel/waarde-paar als \<sleutel\>:\<waarde\>. Hieronder vindt u de sleutels die u gebruikt om op te geven van de locatie van de gebruiker.<br /><br /><ul><li>LAT&mdash;de breedte van de locatie van de client, in graden. De breedtegraad moet groter zijn dan of gelijk zijn aan-90.0 en kleiner dan of gelijk zijn aan +90.0. Negatieve waarden geven zuidelijk Latitude en positieve waarden geven Noord Latitude.<br /><br /></li><li>lange&mdash;de lengtegraad van de locatie van de client, in graden. De lengte moet groter zijn dan of gelijk zijn aan-180.0 en kleiner dan of gelijk zijn aan +180.0. Negatieve waarden geven West lengten en positieve waarden geven eastern lengten.<br /><br /></li><li>RE&mdash; de straal in meters, waarmee de horizontale nauwkeurigheid van de coördinaten. Geeft de waarde die wordt geretourneerd door de locatie-service van het apparaat. Typische waarden mogelijk 22m voor GPS/Wi-Fi, 380m voor cel tower triangulatie en 18.000 m voor reverse lookup van IP.<br /><br /></li><li>TS&mdash; de UTC-UNIX-timestamp van wanneer de client op de locatie is. (De UNIX-timestamp is het aantal seconden sinds 1 januari 1970.)<br /><br /></li><li>HEAD&mdash;optioneel. Relatieve kop of richting van de client. Geef de richting van reizen als graden tussen 0 en 360, tellen rechtsom ten opzichte van de waarde true Noord. Geef deze sleutel alleen als de `sp` sleutel gelijk is aan nul.<br /><br /></li><li>SP&mdash; de horizontale uit te voeren (snelheid), in meter per seconde, die de client-apparaat is verzonden.<br /><br /></li><li>ALT&mdash; de hoogte van het clientapparaat, in meters.<br /><br /></li><li>zijn&mdash;optioneel. De radius, in meters, waarmee de verticale nauwkeurigheid van de coördinaten. RADIUS-standaard ingesteld op 50 kilometer zijn verwijderd. Deze sleutel alleen opgeven als u de `alt` sleutel.<br /><br /></li></ul> **Opmerking:** Hoewel deze sleutels optioneel zijn, de informatie die u opgeeft, hoe nauwkeuriger de resultaten van de locatie zijn.<br /><br /> **Opmerking:** u wordt aangeraden om op te geven altijd de geografische locatie van de gebruiker. De locatie die is vooral belangrijk als de fysieke locatie van de gebruiker (bijvoorbeeld, als de client maakt gebruik van VPN) in de IP-adres van de client niet nauwkeurig worden weergegeven. Voor optimale resultaten, moet u deze en de X-MSEdge-client-IP-header opnemen, maar ten minste, moet u deze header bevatten.|

> [!NOTE] 
> Houd er rekening mee dat de gebruiksvoorwaarden zijn vereist voor compatibiliteit met alle van toepassing zijnde wetten, waaronder met betrekking tot gebruik van deze headers. Bijvoorbeeld, in bepaalde rechtsgebieden, zoals Europa, zijn er vereisten voor het verkrijgen van gebruikers toestemming geven voordat het plaatsen van bepaalde apparaten bijhouden op apparaten van gebruikers.
  

## <a name="query-parameters"></a>Queryparameters  
De aanvraag kan de volgende queryparameters bevatten. Zie de vereiste kolom voor vereiste parameters. Moet u de URL de queryparameters coderen.  
  
  
|Naam|Waarde|Type|Vereist|  
|----------|-----------|----------|--------------|  
|<a name="mkt" />Mkt|De markt waarin de resultaten afkomstig zijn uit. <br /><br />Zie voor een lijst van mogelijke waarden van de markt, [markt Codes](#market-codes).<br /><br /> **Opmerking:** de URL van Preview-API ondersteunt momenteel alleen en-us markt en taal.<br /><br />|Reeks|Ja|  
|<a name="query" />q|De URL om een voorbeeld van|Reeks|Ja|  
|<a name="responseformat" />responseFormat|Het mediatype dat als u wilt gebruiken voor het antwoord. De volgende zijn waarden mogelijk niet hoofdlettergevoelig.<br /><ul><li>JSON</li><li>JSONLD</li></ul><br /> De standaardwaarde is JSON. Zie voor meer informatie over de JSON-objecten dat het antwoord bevat, [Antwoordobjecten](#response-objects).<br /><br />  Als u JsonLd opgeeft, bevat de antwoordtekst JSON-LD objecten met de lijst met zoekresultaten. Zie voor meer informatie over de JSON-LD [JSON-LD](http://json-ld.org/).|Reeks|Nee|  
|<a name="safesearch" />veilig zoeken|Een filter dat wordt gebruikt voor het filteren van inhoud voor volwassenen. Hier volgen de mogelijke niet-hoofdlettergevoelige filterwaarden.<br /><ul><li>Uit&mdash;webpagina's met volwassen tekst, afbeeldingen of video's retourneren.<br /><br/></li><li>Gemiddeld&mdash;webpagina's met volwassen tekst, maar niet volwassen afbeeldingen of video's retourneren.<br /><br/></li><li>Strikte&mdash;webpagina's met volwassen tekst, afbeeldingen of video's niet retourneren.</li></ul><br /> De standaardwaarde is normaal.<br /><br /> **Opmerking:** als de aanvraag afkomstig van een markt is van die Bing volwassenen beleid vereist dat `safeSearch` is ingesteld op strikt, Bing negeert de `safeSearch` waarde en maakt gebruik van strikt.<br/><br/>**Opmerking:** als u de `site:` query-operator, bestaat de kans dat het antwoord inhoud voor volwassen, ongeacht wat bevatten de `safeSearch` queryparameter is ingesteld op. Gebruik `site:` alleen als u zich bewust bent van de inhoud op de site en de mogelijkheid om inhoud voor volwassenen biedt ondersteuning voor uw scenario. |Reeks|Nee|  
|<a name="setlang" />taal instellen|De taal die moet worden gebruikt voor tekenreeksen voor interface. Geef de taal met behulp van de ISO 639-1-2-letter-taalcode. Bijvoorbeeld, is de taalcode voor Engels EN. De standaardwaarde is EN (Engels).<br /><br /> Hoewel dit optioneel is, moet u altijd de taal. Meestal stelt `setLang` op dezelfde taal opgegeven door `mkt` , tenzij de gebruiker wil dat de gebruikersinterface tekenreeksen weergegeven in een andere taal.<br /><br /> Deze parameter en de [Accept-Language](#acceptlanguage) header zijn sluiten elkaar wederzijds uit&mdash;beide niet opgeven.<br /><br /> Een tekenreeks van de interface is een tekenreeks die wordt gebruikt als een label in een gebruikersinterface. Er zijn enkele interface tekenreeksen in de JSON-antwoord-objecten. Ook een koppeling maakt naar Bing.com eigenschappen in de antwoordobjecten gelden voor de opgegeven taal.|Reeks|Nee| 


## <a name="response-objects"></a>Antwoordobjecten  
Het antwoordschema is ofwel een [webpagina] of ErrorResponse, zoals in de webzoekopdrachten-API. Als de aanvraag mislukt, wordt het object op het hoogste niveau is het [ErrorResponse](#errorresponse) object.


|Object|Beschrijving|  
|------------|-----------------|  
|[Webpagina]|Bovenste niveau JSON-object met de kenmerken van de Preview-versie.|  
|[Feit]|Bovenste niveau JSON-object dat gegevens bevat.| 
|[Entiteiten|Bovenste niveau JSON-object met de entiteitsdetails.| 

  
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
|<a name="errors" />Fouten|Een lijst van fouten die worden beschreven van de redenen waarom de aanvraag is mislukt.|[Fout](#error)|  

  
  
### <a name="license"></a>Licentie  
Hiermee definieert u de licentie waaronder de tekst of foto's kan worden gebruikt.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|  
|naam|De naam van de licentie.|Reeks|  
|url|De URL naar een website waar de gebruiker meer informatie over de licentie krijgt.<br /><br /> De naam en URL gebruiken om een hyperlink te maken.|Reeks|  
  

### <a name="licenseattribution"></a>LicenseAttribution  
Definieert een contractuele regel voor attribution license.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|  
|_Type|Een hint type is ingesteld op LicenseAttribution.|Reeks|  
|licentie|De licentie waaronder de inhoud kan worden gebruikt.|[Licentie](#license)|  
|licenseNotice|De licentie om naast het betreffende veld weer te geven. Bijvoorbeeld: 'Tekst onder licentie CC door SA'.<br /><br /> Gebruik de naam van de licentie en de URL in de `license` veld om te maken van een hyperlink naar de website waaronder de details van de licentie worden beschreven. Vervang vervolgens de licentienaam van de in de `licenseNotice` tekenreeks (bijvoorbeeld, CC-van-SA) met de hyperlink die u zojuist hebt gemaakt.|Reeks|  
|mustBeCloseToContent|Een Booleaanse waarde waarmee wordt bepaald of de inhoud van de regel moeten worden geplaatst in de nabijheid van het veld dat de regel van toepassing op. Als **waar**, de inhoud moeten worden geplaatst in de nabijheid. Als **false**, of dit veld niet bestaat, wordt de inhoud van de oproepende functie goeddunken mag worden geplaatst.|Booleaans|  
|targetPropertyName|De naam van het veld dat de regel van toepassing op.|Reeks|  
  

### <a name="link"></a>Koppeling  
Hiermee definieert u de onderdelen van een hyperlink.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|  
|_Type|Type hint.|Reeks|  
|tekst|De tekst.|Reeks|  
|url|EEN URL. Gebruik de URL en tekst voor het maken van een hyperlink weer te geven.|Reeks|  
  

### <a name="linkattribution"></a>LinkAttribution  
Definieert een contractuele regel voor koppeling attribution.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|  
|_Type|Een hint type is ingesteld op LinkAttribution.|Reeks|  
|mustBeCloseToContent|Een Booleaanse waarde waarmee wordt bepaald of de inhoud van de regel moeten worden geplaatst in de nabijheid van het veld dat de regel van toepassing op. Als **waar**, de inhoud moeten worden geplaatst in de nabijheid. Als **false**, of dit veld niet bestaat, wordt de inhoud van de oproepende functie goeddunken mag worden geplaatst.|Booleaans|  
|targetPropertyName|De naam van het veld dat de regel van toepassing op.<br /><br /> Als een doel niet opgegeven is, wordt de toekenning is van toepassing op de entiteit als geheel en direct na de presentatie van de entiteit moet worden weergegeven. Als er meerdere tekst en koppeling attribution-regels met een doel niet opgeeft, moet u ze samenvoegen en deze weer te geven met behulp van een ' gegevens in: "label. Bijvoorbeeld, "gegevens uit < provider name1\> &#124; < provider name2\>'.|Reeks|  
|tekst|De tekst attribution.|Reeks|  
|url|De URL naar de website van de provider. Gebruik `text` en de URL voor het maken van een hyperlink.|Reeks|  
  
  
### <a name="mediaattribution"></a>MediaAttribution  
Definieert een contractuele regel voor media attribution.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|  
|_Type|Een hint type is ingesteld op MediaAttribution.|Reeks|  
|mustBeCloseToContent|Een Booleaanse waarde waarmee wordt bepaald of de inhoud van de regel moeten worden geplaatst in de nabijheid van het veld dat de regel van toepassing op. Als **waar**, de inhoud moeten worden geplaatst in de nabijheid. Als **false**, of dit veld niet bestaat, wordt de inhoud van de oproepende functie goeddunken mag worden geplaatst.|Booleaans|  
|targetPropertyName|De naam van het veld dat de regel van toepassing op.|Reeks|  
|url|De URL die u gebruiken voor het maken van een hyperlink van de media-inhoud. Als het doel een afbeelding is, zou u de URL gebruiken om de installatiekopie van het geklikt.|Reeks|  
  
  
  
### <a name="organization"></a>Organisatie  
Hiermee definieert u een uitgever.  
  
Houd er rekening mee dat de uitgever hun naam of de website of beide bieden kan.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|  
|naam|Naam van de uitgever.|Reeks|  
|url|De URL naar de website van de uitgever.<br /><br /> Houd er rekening mee dat de uitgever niet een website bieden mogelijk.|Reeks|  
  
  

### <a name="webpage"></a>Webpagina  
Informatie over definieert een de webpagina wordt weergegeven in de Preview-versie.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|
|naam|De titel van de pagina, niet per se de HTML-titel|Reeks|
|url|De URL die daadwerkelijk is verkend (aanvraag kan hebt gevolgd omleidingen)|Reeks|  
|description|Korte beschrijving van de pagina en de inhoud|Reeks|  
|isFamilyFriendly|Meest nauwkeurige voor items in de web-index; realtime haalt doen deze detectie die uitsluitend zijn gebaseerd op de URL en niet op de pagina-inhoud|booleaans|
|primaryImageOfPage/contentUrl|De URL naar een representatieve afbeelding wilt opnemen in de Preview-versie|Reeks| 
  
  
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
|Webpagina|Een JSON-object dat de Preview-versie|tekenreeks|  
  
  
### <a name="textattribution"></a>TextAttribution  
Definieert een contractuele regel voor attribution tekst zonder opmaak.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|  
|_Type|Een hint type is ingesteld op TextAttribution.|Reeks|  
|tekst|De tekst attribution.<br /><br /> Tekst attribution geldt voor de entiteit als geheel en direct na de presentatie van de entiteit moet worden weergegeven. Als er meerdere tekst of een koppeling attribution-regels met een doel niet opgeeft, moet u ze samenvoegen en deze weer te geven met behulp van een ' gegevens in: "label.|Reeks| 


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
- [Snelstartgids voor C#](c-sharp-quickstart.md)
- [Snelstartgids voor Java](java-quickstart.md)
- [Knooppunt-snelstartgids](node-quickstart.md)
- [Snelstartgids voor Python](python-quickstart.md)

