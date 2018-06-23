---
title: Antwoord zoeken referentie - cognitieve Services van Microsoft Project | Microsoft Docs
description: Verwijzing voor het eindpunt Project antwoord vinden.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.technology: project-answer-search
ms.topic: article
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 8c95fac0c031ec62a9d98d6c3278bd3b3f345140
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345335"
---
# <a name="project-answer-search-v7-reference"></a>Projectverwijzing antwoord zoeken v7

Bing antwoord SearchAPI heeft een queryparameter en retourneert een `searchResponse` met `answerType`: `facts` of `entities`. 

Toepassingen die gebruikmaken van de antwoord-API van zoekservice verzenden aanvragen naar het eindpunt met een URL naar het voorbeeld in een queryparameter.  De aanvraag moet bevatten de `q=searchTerm` parameter en *Ocp-Apim-Subscription-Key* header.   

De JSON-antwoord kan worden geparseerd voor facts en de entiteiten die informatie over het object van het zoeken bevatten.

## <a name="endpoint"></a>Eindpunt
Als antwoord zoekresultaten, een aanvraag te verzenden naar het volgende eindpunt. De kop- en URL-parameters gebruiken voor het definiëren van verdere specificaties.

GET-eindpunt: 
````
https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search?q=<searchTerm>&subscription-key=0123456789ABCDEF&mkt=en-us

````

De aanvraag moet de HTTPS-protocol gebruiken en omvatten queryparameter te volgen:
-  q =<URL> -de query waarmee het object van het zoeken

Zie voor voorbeelden die laten hoe zien aanvragen [C# Quick Start](c-sharp-quickstart.md) of [Java Quick Start](java-quickstart.md). 

De volgende secties vindt u technische gegevens over de antwoord-objecten, queryparameters en headers die invloed hebben op de zoekresultaten. 
  
Zie voor meer informatie over kopteksten die moeten worden opgenomen in aanvragen [Headers](#headers).  
  
Zie voor meer informatie over queryparameters die moeten worden opgenomen in aanvragen [queryparameters](#query-parameters).  
  
Zie voor meer informatie over de JSON-objecten dat het antwoord bevat [antwoord objecten](#response-objects).

De maximale URL-lengte is 2048 tekens. Om ervoor te zorgen dat uw URL-lengte de limiet niet overschrijdt, moet de maximale lengte van de queryparameters maximaal 1500 tekens. Als de URL langer is dan 2048 tekens, retourneert de server 404 niet gevonden.  

Zie voor meer informatie over toegestaan gebruik en weergeven van resultaten [gebruiken en weergeven van vereisten](use-display-requirements.md). 

> [!NOTE]
> Sommige aanvraagheaders die relevant zijn voor andere zoeken-API's zijn niet van invloed op URL-Preview
> - Pragma – de aanroeper heeft geen controle over het feit of de URL Preview cache gebruikt
> - Cache-Control: de aanroeper heeft geen controle over het feit of de URL Preview cache gebruikt
> - Gebruikersagent

> Bovendien sommige parameters zijn niet op dit moment relevant zijn voor de Preview-API-URL, maar kunnen worden gebruikt in de toekomst bevindt voor verbeterde globalisatie. 
 
## <a name="headers"></a>Headers  
Hier volgen de headers die een aanvraag en -antwoord kunnen bevatten.  
  
|Koptekst|Beschrijving|  
|------------|-----------------|  
|Accepteren|De aanvraagheader is optioneel.<br /><br /> Het mediatype standaard is application/json. Instellen dat het antwoord [JSON-LD](http://json-ld.org/), de Accept-header ingesteld op toepassing/ld + json.|  
|<a name="acceptlanguage" />Accepteer taal|De aanvraagheader is optioneel.<br /><br /> Een door komma's gescheiden lijst met talen voor tekenreeksen voor interface. De lijst is in aflopende volgorde van voorkeur. Zie voor meer informatie, waaronder de verwachte indeling [RFC2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Deze header en het [taal instellen](#setlang) queryparameter sluiten elkaar wederzijds uit&mdash;niet beide opgeeft.<br /><br /> Als u deze header instellen, moet u ook opgeven de [cc](#cc) queryparameter. Om te bepalen op de markt retourneren van resultaten voor, gebruikt de Bing ondersteunde taal deze zoekt in de lijst en combineert met de `cc` parameterwaarde. Als de lijst geen ondersteunde taal bevat, Bing zoeken naar de dichtstbijzijnde taal en markt die ondersteuning biedt voor de aanvraag of het maakt gebruik van een samengevoegde of standaard markt voor de resultaten. Om te bepalen op de markt die Bing gebruikt, Zie de header BingAPIs markt.<br /><br /> Gebruiken deze header en het `cc` queryparameter alleen als u meerdere talen opgeven. Gebruik anders de [mkt](#mkt) en [taal instellen](#setlang) queryparameters.<br /><br /> Een tekenreeks van de interface is een tekenreeks die wordt gebruikt als een label in een gebruikersinterface. Er zijn enkele interface tekenreeksen in de JSON-antwoord-objecten. Alle koppelingen naar Bing.com eigenschappen in de antwoord-objecten van toepassing de opgegeven taal.|  
|<a name="market" />BingAPIs markt|Antwoordheader.<br /><br /> De markt die wordt gebruikt door de aanvraag. Het formulier is \<languageCode\>-\<countryCode\>. En-US.|  
|<a name="traceid" />BingAPIs TraceId|Antwoordheader.<br /><br /> De ID van de vermelding die de details van de aanvraag bevat. Wanneer een fout optreedt, vastleggen deze ID. Als u niet kunt bepalen en los het probleem, zijn deze ID samen met andere gegevens dat u opgeeft dat het ondersteuningsteam.|  
|<a name="subscriptionkey" />OCP-Apim-Subscription-Key|De aanvraagheader is vereist.<br /><br /> De sleutel van het abonnement dat u hebt ontvangen toen u zich registreerde voor deze service in [cognitieve Services](https://www.microsoft.com/cognitive-services/).|  
|<a name="pragma" />Pragma|Optionele aanvraag-header<br /><br /> Bing retourneert standaard de inhoud in cache, indien beschikbaar. Om te voorkomen Bing retourneert de inhoud in cache, stelt u de header Pragma op no-cache (bijvoorbeeld Pragma: Nee-cache).
|<a name="useragent" />Gebruikersagent|De aanvraagheader is optioneel.<br /><br /> De gebruikersagent die afkomstig zijn van de aanvraag. Bing maakt gebruik van de gebruikersagent mobiele gebruikers een geoptimaliseerd om ervaring te bieden. Hoewel dit optioneel is, wordt u aangeraden altijd opgeven om deze header.<br /><br /> De gebruikersagent moet gelijk aan de tekenreeks die een veelgebruikte browser verzendt. Zie voor meer informatie over gebruikersagenten [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Hier volgen enkele voorbeelden van de gebruikersagent tekenreeksen.<br /><ul><li>Windows Phone&mdash;Mozilla/5.0 (compatibel; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)<br /><br /></li><li>Android&mdash;Mozilla/5.0 (Linux; U; Android 2.3.5; en-us; SCH-I500 Build/ONTBIJTKOEK) AppleWebKit/533.1 (KHTML; Gecko, zoals) versie 4.0/Mobile Safari/533.1<br /><br /></li><li>iPhone&mdash;Mozilla/5.0 (iPhone; CPU iPhone OS 6_1 zoals Mac OS X) AppleWebKit/536.26 (KHTML; Gecko, zoals) Mobile/10B142 iPhone4; 1 BingWeb/3.03.1428.20120423<br /><br /></li><li>PC&mdash;Mozilla/5.0 (Windows NT 6.3; WOW64; Trident/7.0; Touch; RV:11.0) zoals Gecko<br /><br /></li><li>iPad&mdash;Mozilla/5.0 (iPad; CPU-OS 7_0 zoals Mac OS X) AppleWebKit/537.51.1 (zoals Gecko KHTML) versie 7.0/Mobile/11A465 Safari/9537.53</li></ul>|
|<a name="clientid" />X-MSEdge-ClientID|Optionele aanvraag en antwoord-header.<br /><br /> Deze header Bing gebruikt om gebruikers met consistent gedrag via Bing-API-aanroepen. Bing lijnen vaak nieuwe functies en verbeteringen en gebruikt de client-ID als een sleutel voor het toewijzen van verkeer op verschillende vlucht. Als u niet dezelfde client-ID voor een gebruiker over meerdere aanvragen gebruikt, kunnen Bing kan de gebruiker toewijzen aan meerdere conflicterende vlucht. Wordt toegewezen aan meerdere conflicterende vlucht kan leiden tot een inconsistente gebruikerservaring. Bijvoorbeeld, als de tweede aanvraag de toewijzing van een andere vlucht dan de eerste heeft, de ervaring mogelijk onverwacht. Bing kunt ook de client-ID gebruiken om aan te passen webresultaten voor deze client-id's in zoekgeschiedenis, bieden een rijkere ervaring voor de gebruiker.<br /><br /> Deze header Bing ook gebruikt voor het resultaat classificaties verbeteren door het analyseren van de activiteit die is gegenereerd door een client-ID. De verbeteringen relevantie helpen met een betere resultaten geleverd door Bing-API's en op zijn beurt hoger schakelt doorklikken tarieven voor de consument API.<br /><br /> **Belangrijk:** Hoewel optioneel, kunt u overwegen deze header vereist. Het behouden blijven van de client-ID voor meerdere aanvragen voor de dezelfde gebruiker en apparaat combinatie kunt 1) de consument API voor het ontvangen van een consistente gebruikerservaring en 2) een hogere doorklikken tarieven via betere kwaliteit van de resultaten van de Bing-API's.<br /><br /> Hier volgen de basisgebruik regels die betrekking hebben op deze header.<br /><ul><li>Elke gebruiker die de toepassing wordt gebruikt op het apparaat moet een unieke, Bing gegenereerd client-ID.<br /><br/>Als u deze header niet in de aanvraag opnemen, wordt Bing een ID gegenereerd en wordt in de header X-MSEdge-ClientID-antwoord geretourneerd. De enige keer dat u deze header niet in een aanvraag opnemen moet is de eerste keer dat de gebruiker maakt gebruik van uw app op het apparaat.<br /><br/></li><li>De client-ID gebruiken voor elke Bing-API-aanvraag die uw app voor deze gebruiker op het apparaat maakt.<br /><br/></li><li>**Let op:** moet u ervoor zorgen dat deze Client-ID niet gerelateerd aan authenticatable gebruikersgegevens account is.</li><br/><li>Behouden van de client-ID. Om te blijven behouden de ID in een browser-app, gebruikt u een permanente HTTP-cookie om te controleren of dat de ID wordt gebruikt in alle sessies. Gebruik een sessiecookie niet. Gebruik voor andere apps, zoals mobiele apps, de permanente opslag van het apparaat om vast te leggen van de-ID.<br /><br/>De volgende keer dat de gebruiker maakt gebruik van uw app op het apparaat ophalen van de client-ID die u persistent hebt gemaakt.</li></ul><br /> **Opmerking:** Bing antwoorden mogelijk of mag deze header bevatten. Als het antwoord deze header bevat, vastleggen van de client-ID en deze gebruiken voor alle volgende Bing-aanvragen voor de gebruiker op het apparaat.<br /><br /> **Opmerking:** als u de X-MSEdge-ClientID opneemt, moet u cookies niet opnemen in de aanvraag.|  
|<a name="clientip" />X-MSEdge-client-IP|De aanvraagheader is optioneel.<br /><br /> Het IPv4- of IPv6-adres van het clientapparaat. Het IP-adres wordt gebruikt voor het detecteren van de locatie van de gebruiker. Bing gebruikt de locatie-informatie veilig zoekgedrag bepalen.<br /><br /> **Opmerking:** Hoewel optioneel, wordt u aangeraden altijd opgeven om deze en de header X-Search-locatie.<br /><br /> Niet verbergen, weergeven het adres (bijvoorbeeld door het laatste octet wijzigen in 0). De resultaten van het adres in de locatie niet overal in de buurt van de werkelijke locatie van het apparaat wordt obfuscating, waardoor de Bing voor de onjuiste resultaten.|  
|<a name="location" />X-Search-locatie|De aanvraagheader is optioneel.<br /><br /> Een met puntkomma's gescheiden lijst met sleutel-waardeparen die de geografische locatie van de client beschrijven. Bing gebruikt de locatie-informatie om te bepalen van veilige zoekgedrag en terug te keren relevante lokale inhoud. Geef de sleutel-waardepaar als \<sleutel\>:\<waarde\>. Hieronder vindt u de sleutels die u gebruikt om op te geven locatie van de gebruiker.<br /><br /><ul><li>LAT&mdash;de breedte van de locatie van de client, in graden. De breedte moet groter zijn dan of gelijk zijn aan-90.0 en kleiner dan of gelijk zijn aan +90.0. Negatieve waarden geven zuidelijke Latitude en positieve waarden geven Noord Latitude-systemen.<br /><br /></li><li>lange&mdash;de lengtegraad van locatie van de client, in graden. De lengte moet groter zijn dan of gelijk zijn aan-180.0 en kleiner dan of gelijk zijn aan +180.0. Negatieve waarden geven western lengten en positieve waarden geven eastern lengten.<br /><br /></li><li>RE&mdash; de straal in meters, waarmee de horizontale nauwkeurigheid van de coördinaten. De waarde die is geretourneerd door het apparaat Locatieservice doorgeven. Normale waarden mogelijk 22m GPS/Wi-Fi, 380m voor cel tower triangulatie en 18.000 m voor reverse lookup van het IP.<br /><br /></li><li>TS&mdash; UNIX met de UTC-timestamp wanneer de client zich op de locatie. (De UNIX-tijdstempel is het aantal seconden sinds 1 januari 1970.)<br /><br /></li><li>HEAD&mdash;optioneel. Relatieve kop of richting van de client. Geef de richting van reizen als graden tussen 0 en 360 rechtsom ten opzichte van de waarde true Noord tellen. Geef deze sleutel alleen als de `sp` sleutel gelijk is aan nul.<br /><br /></li><li>SP&mdash; de horizontale snelheid (snelheid) in meter per seconde die het clientapparaat is verzonden.<br /><br /></li><li>ALT&mdash; de hoogte van het clientapparaat in meters.<br /><br /></li><li>zijn&mdash;optioneel. De straal, in de meter, waarmee de verticale nauwkeurigheid van de coördinaten. RADIUS-standaardwaarde is 50 kilometer. Deze sleutel alleen opgeven als u de `alt` sleutel.<br /><br /></li></ul> **Opmerking:** Hoewel deze sleutels optioneel zijn, de informatie die u opgeeft, hoe nauwkeuriger de resultaten van de locatie zijn.<br /><br /> **Opmerking:** , wordt u aangeraden altijd opgeven om de geografische locatie van de gebruiker. Voorzien in de locatie is vooral belangrijk als het IP-adres van de client de fysieke locatie van de gebruiker niet nauwkeurig overeen (bijvoorbeeld als de client VPN gebruikt). Voor optimale resultaten, moet u deze en de X-MSEdge-client-IP-header opnemen, maar minimaal, moet u deze header opnemen.|

> [!NOTE] 
> Houd er rekening mee dat de gebruiksvoorwaarden voldoen aan alle toepasselijke wetten moet, waaronder met betrekking tot gebruik van deze headers. Bijvoorbeeld, in bepaalde rechtsgebieden, zoals Europa, zijn er vereisten voor het verkrijgen van de gebruiker toestemming gevraagd voor het plaatsen van bepaalde apparaten bijhouden op apparaten van gebruikers.
  

## <a name="query-parameters"></a>Queryparameters  
De aanvraag kan de volgende queryparameters bevatten. Zie de vereiste kolom voor vereiste parameters. Moet u de URL de queryparameters coderen.  
  
  
|Naam|Waarde|Type|Vereist|  
|----------|-----------|----------|--------------|  
|<a name="mkt" />Mkt|De markt waar de resultaten afkomstig zijn uit. <br /><br />Zie voor een lijst van mogelijke waarden voor markt [markt Codes](#market-codes).<br /><br /> **Opmerking:** de URL Preview-API ondersteunt momenteel alleen en-us markt- en taalinstellingen.<br /><br />|Reeks|Ja|  
|<a name="query" />q|De URL voor de preview|Reeks|Ja|  
|<a name="responseformat" />responseFormat|Het mediatype als u wilt gebruiken voor het antwoord. De volgende zijn waarden mogelijk niet hoofdlettergevoelig.<br /><ul><li>JSON</li><li>JSONLD</li></ul><br /> De standaardwaarde is JSON. Zie voor meer informatie over de JSON-objecten dat het antwoord bevat [antwoord objecten](#response-objects).<br /><br />  Als u JsonLd opgeeft, bevat de antwoordtekst JSON-LD-objecten die de zoekresultaten bevatten. Zie voor meer informatie over de JSON-LD [JSON-LD](http://json-ld.org/).|Reeks|Nee|  
|<a name="safesearch" />veilig zoeken|Een filter dat wordt gebruikt voor het filteren van inhoud voor volwassenen. Hier volgen de mogelijke hoofdlettergevoelige filterwaarden.<br /><ul><li>Uit&mdash;webpagina's met volwassenen tekst, afbeeldingen of video's retourneren.<br /><br/></li><li>Gemiddeld&mdash;webpagina's met volwassenen tekst, maar niet volwassenen afbeeldingen of video's retourneren.<br /><br/></li><li>Strikte&mdash;webpagina's met volwassenen tekst, afbeeldingen of video's niet retourneren.</li></ul><br /> De standaardwaarde is matig.<br /><br /> **Opmerking:** wanneer de aanvraag afkomstig van een markt is die Bing volwassenen beleid vereist dat `safeSearch` is ingesteld voor Strict, Bing, negeert de `safeSearch` waarde en Strict gebruikt.<br/><br/>**Opmerking:** als u de `site:` queryoperator, bestaat de kans dat het antwoord inhoud voor volwassenen ongeacht wat bevat de `safeSearch` queryparameter is ingesteld op. Gebruik `site:` alleen als u zich bewust bent van de inhoud op de site en de mogelijkheid van inhoud voor volwassenen ondersteuning biedt voor uw scenario. |Reeks|Nee|  
|<a name="setlang" />taal instellen|De taal die moet worden gebruikt voor tekenreeksen voor interface. Geef de taal die de ISO 639-1-2-letterige taalcode gebruikt. De taalcode voor Engels is bijvoorbeeld nl. De standaardwaarde is NL (Engels).<br /><br /> Hoewel dit optioneel is, moet u altijd de taal opgeven. Gewoonlijk stelt u `setLang` op de dezelfde taal die is opgegeven door `mkt` tenzij de gebruiker wil dat de gebruikersinterface tekenreeksen in een andere taal weergegeven.<br /><br /> Deze parameter en de [Accept-Language](#acceptlanguage) header sluiten elkaar wederzijds uit&mdash;niet beide opgeeft.<br /><br /> Een tekenreeks van de interface is een tekenreeks die wordt gebruikt als een label in een gebruikersinterface. Er zijn enkele interface tekenreeksen in de JSON-antwoord-objecten. Ook koppelingen naar Bing.com eigenschappen in de antwoord-objecten gelden voor de opgegeven taal.|Reeks|Nee| 


## <a name="response-objects"></a>Antwoord-objecten  
Het antwoordschema van het is ofwel een [webpagina] of ErrorResponse, zoals in de Web-API voor zoeken. Als de aanvraag is mislukt, het object dat op het hoogste niveau is de [ErrorResponse](#errorresponse) object.


|Object|Beschrijving|  
|------------|-----------------|  
|[Webpagina]|Bovenste niveau JSON-object met de kenmerken van preview.|  
|[Feit]|Bovenste niveau JSON-object dat feiten bevat.| 
|[Entiteiten|Bovenste niveau JSON-object met de entiteitsdetails.| 

  
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

  
  
### <a name="license"></a>Licentie  
Hiermee definieert u de licentie waaronder de tekst of foto's kan worden gebruikt.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|  
|naam|De naam van de licentie.|Reeks|  
|url|De URL naar een website waar de gebruiker meer informatie over de licentie krijgt.<br /><br /> Gebruik de naam en de URL voor het maken van een hyperlink.|Reeks|  
  

### <a name="licenseattribution"></a>LicenseAttribution  
Definieert een contractueel regel voor toekenning van licentie.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|  
|_Type|Een hint type is ingesteld op LicenseAttribution.|Reeks|  
|licentie|De licentie waaronder de inhoud kan worden gebruikt.|[Licentie](#license)|  
|licenseNotice|De licentie om naast het betreffende veld weer te geven. Bijvoorbeeld 'Tekst onder licentie CC door SA'.<br /><br /> Gebruik de licentie-naam en URL in de `license` veld voor het maken van een hyperlink naar de website die de details van de licentie worden beschreven. Vervang vervolgens de naam van de licentie in de `licenseNotice` tekenreeks (bijvoorbeeld, CC-door-SA) met de hyperlink die u zojuist hebt gemaakt.|Reeks|  
|mustBeCloseToContent|Een Booleaanse waarde die bepaalt of de inhoud van de regel moeten worden geplaatst in de nabijheid van het veld dat de regel geldt voor. Als **true**, de inhoud dicht moeten worden geplaatst. Als **false**, of dit veld niet bestaat, wordt de inhoud van de aanroeper goeddunken mag worden gebracht.|Boole-waarde|  
|targetPropertyName|De naam van het veld dat de regel van toepassing op.|Reeks|  
  

### <a name="link"></a>Koppeling  
Hiermee definieert u de onderdelen van een hyperlink.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|  
|_Type|Type hint.|Reeks|  
|tekst|De weergavetekst.|Reeks|  
|url|EEN URL. De URL gebruiken en weergeven van tekst voor het maken van een hyperlink.|Reeks|  
  

### <a name="linkattribution"></a>LinkAttribution  
Definieert een contractueel regel voor toekenning van de koppeling.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|  
|_Type|Een hint type is ingesteld op LinkAttribution.|Reeks|  
|mustBeCloseToContent|Een Booleaanse waarde die bepaalt of de inhoud van de regel moeten worden geplaatst in de nabijheid van het veld dat de regel geldt voor. Als **true**, de inhoud dicht moeten worden geplaatst. Als **false**, of dit veld niet bestaat, wordt de inhoud van de aanroeper goeddunken mag worden gebracht.|Boole-waarde|  
|targetPropertyName|De naam van het veld dat de regel van toepassing op.<br /><br /> Als een doel niet is opgegeven, wordt het kenmerk is van toepassing op de entiteit in zijn geheel en onmiddellijk na de presentatie entiteit moet worden weergegeven. Als er meerdere tekst en koppeling toekenning-regels met een doel niet opgeeft, moet u ze samen en deze weer te geven met behulp van een ' gegevens in: "label. Bijvoorbeeld, "gegevens uit < provider name1\> &#124; < provider naam2\>'.|Reeks|  
|tekst|De tekst toekenning.|Reeks|  
|url|De URL moet de website van de provider. Gebruik `text` en de URL voor het maken van de hyperlink.|Reeks|  
  
  
### <a name="mediaattribution"></a>MediaAttribution  
Definieert een contractueel regel voor toekenning van media.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|  
|_Type|Een hint type is ingesteld op MediaAttribution.|Reeks|  
|mustBeCloseToContent|Een Booleaanse waarde die bepaalt of de inhoud van de regel moeten worden geplaatst in de nabijheid van het veld dat de regel geldt voor. Als **true**, de inhoud dicht moeten worden geplaatst. Als **false**, of dit veld niet bestaat, wordt de inhoud van de aanroeper goeddunken mag worden gebracht.|Boole-waarde|  
|targetPropertyName|De naam van het veld dat de regel van toepassing op.|Reeks|  
|url|De URL die u gebruikt voor het maken van de hyperlink van de media-inhoud. Als het doel een installatiekopie is, zou u de URL gebruiken voor het maken van de afbeelding waarop kan worden geklikt.|Reeks|  
  
  
  
### <a name="organization"></a>Organisatie  
Hiermee definieert u een uitgever.  
  
Houd er rekening mee dat een uitgever hun naam of de website of beide bieden kan.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|  
|naam|De naam van de uitgever.|Reeks|  
|url|De URL moet de website van de uitgever.<br /><br /> Houd er rekening mee dat de uitgever kan een website niet bieden.|Reeks|  
  
  

### <a name="webpage"></a>Webpagina  
Informatie over definieert een de webpagina in preview.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|
|naam|De titel van de pagina, niet per se de HTML-titel|Reeks|
|url|De URL die daadwerkelijk is verkend (aanvraag kan hebt gevolgd omleidingen)|Reeks|  
|description|Korte beschrijving van de de pagina en de inhoud|Reeks|  
|isFamilyFriendly|Meest nauwkeurige voor items in de index web; real-time op te halen komen deze detectie op basis van uitsluitend op de URL en niet op de pagina-inhoud|booleaans|
|primaryImageOfPage/contentUrl|De URL van een representatieve afbeelding wilt opnemen in de Preview-versie|Reeks| 
  
  
### <a name="querycontext"></a>QueryContext  
Hiermee definieert u de querycontext die Bing voor de aanvraag gebruikt.  
  
|Element|Beschrijving|Type|  
|-------------|-----------------|----------|  
|adultIntent|Een Boolean die aangeeft of de opgegeven query volwassenen doel heeft. De waarde is **true** als de query volwassenen opzet heeft; anders **false**.|Boole-waarde|  
|alterationOverrideQuery|De queryreeks te gebruiken om af te dwingen Bing naar de oorspronkelijke reeks wordt gebruikt. Bijvoorbeeld, als de queryreeks *saling downwind*, worden de queryreeks onderdrukking *+ saling downwind*. Houd er rekening mee voor het coderen van de queryreeks, wat tot leidt *% 2Bsaling + downwind*.<br /><br /> Dit veld wordt alleen opgenomen als de oorspronkelijke queryreeks een fout spelling bevat.|Reeks|  
|alteredQuery|De query-tekenreeks die wordt gebruikt door Bing voor het uitvoeren van de query. Bing wordt de gewijzigde queryreeks gebruikt als de oorspronkelijke queryreeks fouten bevat. Bijvoorbeeld, als de queryreeks `saling downwind`, worden de gewijzigde queryreeks `sailing downwind`.<br /><br /> Dit veld wordt alleen opgenomen als de oorspronkelijke queryreeks een fout spelling bevat.|Reeks|  
|askUserForLocation|Een Boolean die aangeeft of de locatie van de gebruiker voor nauwkeurige resultaten voor Bing is vereist. Als u de locatie van de gebruiker met behulp van opgegeven de [X-MSEdge-client-IP](#clientip) en [X zoeken locatie](#location) headers, kunt u dit veld negeren.<br /><br /> Voor de locatie op de hoogte query's, zoals 'vandaag weer' of 'restaurant dichtbij' die de locatie van de gebruiker voor nauwkeurige resultaten moeten dit veld is ingesteld op **true**.<br /><br /> Voor de locatie op de hoogte query's die de locatie (bijvoorbeeld "Seattle weer') bevatten, dit veld is ingesteld op **false**. Dit veld ook is ingesteld op **false** voor query's die niet op de hoogte, zoals 'best verkopers' locatie.|Boole-waarde|  
|originalQuery|De queryreeks weergegeven zoals opgegeven in de aanvraag.|Reeks|  

### <a name="identifiable"></a>Persoonsgegevens
|Naam|Waarde|Type|  
|-------------|-----------------|----------|
|id|Een bron-id|Reeks|
 
### <a name="rankinggroup"></a>RankingGroup
Definieert een zoekresultaten groep, zoals mainline.
|Naam|Waarde|Type|  
|-------------|-----------------|----------|
|items|Een lijst met zoekresultaten wilt weergeven in de groep.|RankingItem|

### <a name="rankingitem"></a>RankingItem
Hiermee definieert u een item van het resultaat zoeken om weer te geven.
|Naam|Waarde|Type|  
|-------------|-----------------|----------|
|resultIndex|Een op nul gebaseerde index van het item in het antwoord om weer te geven. Als het item niet in dit veld bevat, moet u alle items weergeven in het antwoord. Bijvoorbeeld, alle nieuwsartikelen weergegeven in het antwoord nieuws.|Geheel getal|
|answerType|Het antwoord met de items om weer te geven. Bijvoorbeeld, nieuws.<br /><br />Het type het antwoord vinden in het object SearchResponse gebruiken. Het type is de naam van een veld SearchResponse.<br /><br /> Echter, het antwoordtype alleen gebruiken als dit object het waardeveld bevat. anders wordt deze negeren.|Reeks|
|textualIndex|De index van het antwoord op textualAnswers om weer te geven.| Niet-ondertekend geheel getal|
|waarde|De ID die u identificeert een antwoord om weer te geven of een item van het antwoord om weer te geven. Als de ID verwijst naar een antwoord, moet u alle items van het antwoord weergegeven.|Persoonsgegevens|

### <a name="rankingresponse"></a>RankingResponse  
Bepaalt waar op de zoekopdracht resultaten pagina-inhoud moet worden geplaatst en in welke volgorde.  
  
|Naam|Waarde|  
|----------|-----------|  
|<a name="ranking-mainline" />mainline|De zoekresultaten wilt weergeven in de hoofdlijnen.|  
|<a name="ranking-pole" />pool|De zoekresultaten die moeten worden toegepast als de meest zichtbaar behandeling (bijvoorbeeld weergegeven boven de hoofdlijnen en zijbalk).|  
|<a name="ranking-sidebar" />zijbalk|De zoekresultaten wilt weergeven in de zijbalk.| 


### <a name="searchresponse"></a>SearchResponse  
Hiermee definieert u de site op het hoogste object dat het antwoord bevat als de aanvraag is geslaagd.  
  
Houd er rekening mee dat als de service een denial of service-aanval vermoedt, de aanvraag slaagt (HTTP-statuscode is 200 OK); de hoofdtekst van het antwoord is leeg.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|  
|_Type|Type-hint die is ingesteld op SearchResponse.|Reeks|  
|Webpagina|Een JSON-object dat de Preview-versie definieert|tekenreeks|  
  
  
### <a name="textattribution"></a>TextAttribution  
Hiermee definieert u een regel voor tekst zonder opmaak daarmee het contractueel.  
  
|Naam|Waarde|Type|  
|----------|-----------|----------|  
|_Type|Een hint type is ingesteld op TextAttribution.|Reeks|  
|tekst|De tekst toekenning.<br /><br /> Tekst toekenning geldt voor de entiteit in zijn geheel en onmiddellijk na de presentatie entiteit moet worden weergegeven. Als er meerdere tekst of een koppeling toekenning-regels met een doel niet opgeeft, moet u ze samen en deze weer te geven met behulp van een ' gegevens in: "label.|Reeks| 


## <a name="error-codes"></a>Foutcodes

Hier volgen de mogelijke HTTP-statuscodes die een aanvraag als resultaat geeft.  
  
|Statuscode|Beschrijving|  
|-----------------|-----------------|  
|200|Geslaagd.|  
|400|Een van de queryparameters is ontbreekt of ongeldig.|  
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
- [Quick Start C#](c-sharp-quickstart.md)
- [Java-Quick Start](java-quickstart.md)
- [Knooppunt Quick Start](node-quickstart.md)
- [Snelstartgids voor Python](python-quickstart.md)

