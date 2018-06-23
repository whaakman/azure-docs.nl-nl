---
title: Overzicht van Bing Visual zoeken-API | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Laat zien hoe u meer informatie of inzichten over een afbeelding zoals soortgelijke afbeeldingen of winkelwagen bronnen.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 04/10/2018
ms.author: scottwhi
ms.openlocfilehash: 95f10d8ea7ebe1d40d45231a8ea40df81543fe8b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345533"
---
# <a name="what-is-bing-visual-search-api"></a>Wat is Bing Visual Search API?

Bing Visual zoeken-API biedt een vergelijkbaar met de details van de afbeelding die wordt weergegeven op Bing.com/images ervaring. Met Visual Search kunt u een afbeelding uploadt en terughalen van inzicht in de afbeelding zoals visueel soortgelijke afbeeldingen, winkelwagen bronnen, webpagina's met de installatiekopie en andere elementen bevatten. In plaats van een afbeelding te uploaden, kunt u ook een token insights die u via een installatiekopie in de zoekresultaten installatiekopieën opgeven (Zie [Bing-API voor installatiekopieën](../bing-image-search/overview.md)).

Visual zoekopdracht kunt identificeren beroemdheden, monumenten en monumenten, illustraties, thuis inrichten, mode, producten, (tekenherkenning) en meer.

Hier volgen de inzichten Visual zoeken kunt u detecteren.

- Visueel soortgelijke afbeeldingen&mdash;een lijst met afbeeldingen die visueel vergelijkbaar met de installatiekopie van het invoerbestand zijn
- Visueel vergelijkbare producten&mdash;een lijst met afbeeldingen met producten die visueel vergelijkbaar met het product weergegeven in de installatiekopie van het invoerbestand zijn
- Bronnen winkelen&mdash;een lijst van plaatsen waar u het item weergegeven in de invoer installatiekopie kunt kopen
- Zoekopdrachten gerelateerde&mdash;een lijst met verwante zoekopdrachten die door anderen of die zijn gebaseerd op de inhoud van de installatiekopie
- Webpagina's met de installatiekopie van het&mdash;een lijst met webpagina's met de installatiekopie van het invoerbestand
- Recepten&mdash;een lijst met webpagina's met recepten voor het maken van de schaal wordt weergegeven in de installatiekopie van het invoerbestand

Naast deze inzichten retourneert Visual zoeken ook een uitgebreide set voorwaarden (tags) afgeleid van de installatiekopie van het invoerbestand. Deze tags kunnen gebruikers om te verkennen concepten gevonden in de installatiekopie. Als de installatiekopie van de invoer van een beroemdheid atleet, bijvoorbeeld één van de labels kan deze de naam van de atleet, sport door een andere code kan worden. Of als de installatiekopie van de invoer van een apple-cirkelsegment is, worden de labels Apple Pie, pasteien, Desserts, zodat gebruikers gerelateerde concepten kunnen verkennen.

De visuele zoekresultaten ook begrenzingsvak vakken voor de regio's in de installatiekopie van het van belang. Bijvoorbeeld, als de installatiekopie verschillende beroemdheden bevat, omvatten de resultaten mogelijk vakken voor elk van de herkende beroemdheden in de installatiekopie van het begrenzingsvak. Of als Bing een product of kleding in de installatiekopie herkent, het resultaat een selectiekader voor het product dat wordt herkend of kleding item kan bevatten.

> [!IMPORTANT]
> Als u de installatiekopieën/details van het eindpunt naar [installatiekopie inzichten verkrijgen](../bing-image-search/image-insights.md), moet u uw code voor het gebruik van Visual zoeken in plaats daarvan omdat het uitgebreider inzicht biedt bijwerken.


## <a name="the-request"></a>De aanvraag

Hieronder vindt u de opties voor het verkrijgen van inzicht in een afbeelding. 

- Verzenden van een token insights die u via een installatiekopie in een eerdere aanroep voor een van de [Bing-API voor installatiekopieën](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) eindpunten
- De URL van een installatiekopie van een verzenden
- Upload een afbeelding (binair)


Als u Visual Search een token van de afbeelding of URL verzendt, wordt het volgende ziet het JSON-object dat u in de hoofdtekst van het bericht opnemen moet. 

```json
{
    "imageInfo" : {
        "url" : "",
        "imageInsightsToken" : "",
        "cropArea" : {
            "top" : 0.1,
            "left" : 0.5,
            "right" : 0.9,
            "bottom" : 0.9
        }
    },
    "knowledgeRequest" : {
      "filters" : {
        "site" : ""
      }
    }
}
```

De `imageInfo` object moet een bevatten de `url` en `imageInsightsToken` veld maar niet beide. Stel de `url` veld met de URL van een installatiekopie van het Internet toegankelijk is. De maximale ondersteunde afbeeldingsgrootte is 1 MB.

De `imageInsightsToken` moet worden ingesteld op een insights-token. Als u een token inzichten, roept u de installatiekopie-API van Bing. Het antwoord bevat een lijst met `Image` objecten. Elke `Image` object bevat een `imageInsightsToken` veld waarin het token.

De `cropArea` veld is optioneel. Gebied bijsnijden geeft de top, linkerhoek en onder, rechterhoek van een gebied van belang. Geef de waarden in het bereik 0,0 en 1,0. De waarden zijn een percentage van de totale breedte of hoogte. Bijvoorbeeld, markeert het bovenstaande voorbeeld het recht de helft van de afbeelding als de regio van belang. Opnemen als u wilt beperken van de aanvraag inzichten voor de regio van belang.

De `filters` object bevat een site-filter (Zie de `site` veld) waarmee u kunt de soortgelijke afbeeldingen en vergelijkbare producten resultaten beperken tot een specifiek domein. Bijvoorbeeld, als de installatiekopie van een Surface Book is, kunt u instellen `site` naar www.microsoft.com. 

Als u verkrijgen van inzicht in een lokale kopie van een installatiekopie wilt, moet u de installatiekopie uploadt als binaire gegevens.

Zie voor meer informatie over deze opties in de hoofdtekst van het bericht, inclusief [formulier inhoudstypen](#content-form-types).


### <a name="endpoint"></a>Eindpunt

Het eindpunt Visual zoeken: https:\/\/api.cognitive.microsoft.com/bing/v7.0/images/visualsearch.

Aanvragen moeten worden verzonden als een HTTP POST-aanvragen. 


### <a name="query-parameters"></a>Queryparameters

Hier volgen de queryparameters die uw aanvraag moet opgeven. U dient ten minste nemen de `mkt` queryparameter.

|Naam|Waarde|Type|Vereist|  
|----------|-----------|----------|--------------|  
|<a name="cc" />CC|Een 2-teken landcode van het land waar de resultaten afkomstig zijn uit.<br /><br /> Als u deze parameter instelt, moet u ook opgeven de [Accept-Language](#acceptlanguage) header. Bing maakt gebruik van de eerste ondersteunde taal gezocht in de lijst met talen en de taal combineert met de landcode die u opgeeft om te bepalen van de markt retourneren van resultaten uit. Als de lijst met talen niet onder een ondersteunde taal, Bing zoeken naar de dichtstbijzijnde taal en markt die ondersteuning biedt voor de aanvraag. Of mogelijk gebruik van een samengevoegde of standaard markt voor de resultaten in plaats van de opgegeven.<br /><br /> Moet u deze queryparameter en de `Accept-Language` queryparameter alleen als u meerdere talen opgeven; anders moet u de `mkt` en `setLang` queryparameters.<br /><br /> Deze parameter en de [mkt](#mkt) queryparameter sluiten elkaar wederzijds uit&mdash;niet beide opgeeft.|Reeks|Nee|  
|<a name="mkt" />Mkt|De markt waar de resultaten afkomstig zijn uit. <br /><br /> **Opmerking:** u wordt geadviseerd om op te geven altijd de markt, indien bekend. Het opgeven van dat de markt helpt Bing routeren van de aanvraag en een geschikte en optimale antwoord retourneren.<br /><br /> Deze parameter en de [cc](#cc) queryparameter sluiten elkaar wederzijds uit&mdash;niet beide opgeeft.|Reeks|Ja|  
|<a name="safesearch" />veilig zoeken|Een filter dat wordt gebruikt voor het filteren van inhoud voor volwassenen. Hier volgen de mogelijke hoofdlettergevoelige filterwaarden.<br /><ul><li>Uit&mdash;webpagina's met volwassenen tekst of afbeeldingen retourneren.<br /><br/></li><li>Gemiddeld&mdash;webpagina's met volwassenen tekst, maar niet volwassenen installatiekopieën retourneren.<br /><br/></li><li>Strikte&mdash;webpagina's met volwassenen tekst of afbeeldingen niet retourneren.</li></ul><br /> De standaardwaarde is matig.<br /><br /> **Opmerking:** wanneer de aanvraag afkomstig van een markt is die Bing volwassenen beleid vereist dat `safeSearch` worden ingesteld op Strict, Bing, negeert de `safeSearch` waarde en Strict gebruikt.<br/><br/>**Opmerking:** als u de `site:` queryoperator, bestaat de kans dat het antwoord inhoud voor volwassenen ongeacht wat bevat de `safeSearch` queryparameter is ingesteld op. Gebruik `site:` alleen als u zich bewust bent van de inhoud op de site en de mogelijkheid van inhoud voor volwassenen ondersteuning biedt voor uw scenario. |Reeks|Nee|  
|<a name="setlang" />taal instellen|De taal die moet worden gebruikt voor tekenreeksen voor interface. Geef de taal die de ISO 639-1-2-letterige taalcode gebruikt. De taalcode voor Engels is bijvoorbeeld nl. De standaardwaarde is NL (Engels).<br /><br /> Hoewel dit optioneel is, moet u altijd de taal opgeven. Gewoonlijk stelt u `setLang` op de dezelfde taal die is opgegeven door `mkt` tenzij de gebruiker wil dat de gebruikersinterface tekenreeksen in een andere taal weergegeven.<br /><br /> Deze parameter en de [Accept-Language](#acceptlanguage) header sluiten elkaar wederzijds uit&mdash;niet beide opgeeft.<br /><br /> Een tekenreeks van de interface is een tekenreeks die wordt gebruikt als een label in een gebruikersinterface. Er zijn enkele interface tekenreeksen in de JSON-antwoord-objecten. Ook koppelingen naar Bing.com eigenschappen in de antwoord-objecten gelden voor de opgegeven taal.|Reeks|Nee| 

### <a name="headers"></a>Headers

Hier volgen de headers die uw aanvraag moet opgeven. De Content-Type en Ocp-Apim-Subscription-Key headers zijn de enige vereiste headers maar moet u ook opnemen gebruikersagent, X-MSEdge-ClientID, X-MSEdge-client-IP en X-Search-locatie.


|Koptekst|Beschrijving|  
|------------|-----------------|  
|<a name="acceptlanguage" />Accepteer taal|De aanvraagheader is optioneel.<br /><br /> Een door komma's gescheiden lijst met talen voor tekenreeksen voor interface. De lijst is in aflopende volgorde van voorkeur. Zie voor meer informatie, waaronder de verwachte indeling [RFC2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Deze header en het [taal instellen](#setlang) queryparameter sluiten elkaar wederzijds uit&mdash;niet beide opgeeft.<br /><br /> Als u deze header instellen, moet u ook opgeven de [cc](#cc) queryparameter. Om te bepalen op de markt retourneren van resultaten voor, gebruikt de Bing ondersteunde taal deze zoekt in de lijst en combineert met de `cc` parameterwaarde. Als de lijst geen ondersteunde taal bevat, Bing zoeken naar de dichtstbijzijnde taal en markt die ondersteuning biedt voor de aanvraag of het maakt gebruik van een samengevoegde of standaard markt voor de resultaten. Om te bepalen op de markt die Bing gebruikt, Zie de header BingAPIs markt.<br /><br /> Gebruiken deze header en het `cc` queryparameter alleen als u meerdere talen opgeven. Gebruik anders de [mkt](#mkt) en [taal instellen](#setlang) queryparameters.<br /><br /> Een tekenreeks van de interface is een tekenreeks die wordt gebruikt als een label in een gebruikersinterface. Er zijn enkele interface tekenreeksen in de JSON-antwoord-objecten. Alle koppelingen naar Bing.com eigenschappen in de antwoord-objecten van toepassing de opgegeven taal.|  
|<a name="contenttype" />Content-Type|De aanvraagheader is vereist.<br /><br />Moet worden ingesteld op een multipart-formuliergegevens en bevatten een grensparameter (bijvoorbeeld multipart-formuliergegevens; boundary =\<grens tekenreeks\>). Zie voor meer informatie [formulier inhoudstypen](#content-form-types).
|<a name="market" />BingAPIs markt|Antwoordheader.<br /><br /> De markt die wordt gebruikt door de aanvraag. Het formulier is \<languageCode\>-\<countryCode\>. En-US.|  
|<a name="traceid" />BingAPIs TraceId|Antwoordheader.<br /><br /> De ID van de vermelding die de details van de aanvraag bevat. Wanneer een fout optreedt, vastleggen deze ID. Als u niet kunt bepalen en los het probleem, zijn deze ID samen met andere gegevens dat u opgeeft dat het ondersteuningsteam.|  
|<a name="subscriptionkey" />OCP-Apim-Subscription-Key|De aanvraagheader is vereist.<br /><br /> De sleutel van het abonnement dat u hebt ontvangen toen u zich registreerde voor deze service in [cognitieve Services](https://www.microsoft.com/cognitive-services/).|  
|<a name="pragma" />Pragma|Optionele aanvraag-header<br /><br /> Bing retourneert standaard de inhoud in cache, indien beschikbaar. Om te voorkomen Bing retourneert de inhoud in cache, stelt u de header Pragma op no-cache (bijvoorbeeld Pragma: Nee-cache).
|<a name="useragent" />Gebruikersagent|De aanvraagheader is optioneel.<br /><br /> De gebruikersagent die afkomstig zijn van de aanvraag. Bing maakt gebruik van de gebruikersagent mobiele gebruikers een geoptimaliseerd om ervaring te bieden. Hoewel dit optioneel is, wordt u aangeraden altijd opgeven om deze header.<br /><br /> De gebruikersagent moet gelijk aan de tekenreeks die een veelgebruikte browser verzendt. Zie voor meer informatie over gebruikersagenten [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Hier volgen enkele voorbeelden van de gebruikersagent tekenreeksen.<br /><ul><li>Windows Phone&mdash;Mozilla/5.0 (compatibel; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)<br /><br /></li><li>Android&mdash;Mozilla/5.0 (Linux; U; Android 2.3.5; en-us; SCH-I500 Build/ONTBIJTKOEK) AppleWebKit/533.1 (KHTML; Gecko, zoals) versie 4.0/Mobile Safari/533.1<br /><br /></li><li>iPhone&mdash;Mozilla/5.0 (iPhone; CPU iPhone OS 6_1 zoals Mac OS X) AppleWebKit/536.26 (KHTML; Gecko, zoals) Mobile/10B142 iPhone4; 1 BingWeb/3.03.1428.20120423<br /><br /></li><li>PC&mdash;Mozilla/5.0 (Windows NT 6.3; WOW64; Trident/7.0; Touch; RV:11.0) zoals Gecko<br /><br /></li><li>iPad&mdash;Mozilla/5.0 (iPad; CPU-OS 7_0 zoals Mac OS X) AppleWebKit/537.51.1 (zoals Gecko KHTML) versie 7.0/Mobile/11A465 Safari/9537.53</li></ul>|
|<a name="clientid" />X-MSEdge-ClientID|Optionele aanvraag en antwoord-header.<br /><br /> Deze header Bing gebruikt om gebruikers met consistent gedrag via Bing-API-aanroepen. Bing lijnen vaak nieuwe functies en verbeteringen en gebruikt de client-ID als een sleutel voor het toewijzen van verkeer op verschillende vlucht. Als u niet dezelfde client-ID voor een gebruiker over meerdere aanvragen gebruikt, kunnen Bing kan de gebruiker toewijzen aan meerdere conflicterende vlucht. Wordt toegewezen aan meerdere conflicterende vlucht kan leiden tot een inconsistente gebruikerservaring. Bijvoorbeeld, als de tweede aanvraag de toewijzing van een andere vlucht dan de eerste heeft, de ervaring mogelijk onverwacht. Bing kunt ook de client-ID gebruiken om aan te passen webresultaten voor deze client-id's in zoekgeschiedenis, bieden een rijkere ervaring voor de gebruiker.<br /><br /> Deze header Bing ook gebruikt voor het resultaat classificaties verbeteren door het analyseren van de activiteit die is gegenereerd door een client-ID. De verbeteringen relevantie helpen met een betere resultaten geleverd door Bing-API's en op zijn beurt hoger schakelt doorklikken tarieven voor de consument API.<br /><br /> **Belangrijk:** Hoewel optioneel, kunt u overwegen deze header vereist. Het behouden blijven van de client-ID voor meerdere aanvragen voor de dezelfde gebruiker en apparaat combinatie kunt 1) de consument API voor het ontvangen van een consistente gebruikerservaring en 2) een hogere doorklikken tarieven via betere kwaliteit van de resultaten van de Bing-API's.<br /><br /> Hier volgen de basisgebruik regels die betrekking hebben op deze header.<br /><ul><li>Elke gebruiker die de toepassing wordt gebruikt op het apparaat moet een unieke, Bing gegenereerd client-ID.<br /><br/>Als u deze header niet in de aanvraag opnemen, wordt Bing een ID gegenereerd en wordt in de header X-MSEdge-ClientID-antwoord geretourneerd. De enige keer dat u deze header niet in een aanvraag opnemen moet is de eerste keer dat de gebruiker maakt gebruik van uw app op het apparaat.<br /><br/></li><li>**Let op:** moet u ervoor zorgen dat deze Client-ID niet gerelateerd aan de accountgegevens van een geverifieerde gebruiker is.</li><li>De client-ID gebruiken voor elke Bing-API-aanvraag die uw app voor deze gebruiker op het apparaat maakt.<br /><br/></li><li>Behouden van de client-ID. Om te blijven behouden de ID in een browser-app, gebruikt u een permanente HTTP-cookie om te controleren of dat de ID wordt gebruikt in alle sessies. Gebruik een sessiecookie niet. Gebruik voor andere apps, zoals mobiele apps, de permanente opslag van het apparaat om vast te leggen van de-ID.<br /><br/>De volgende keer dat de gebruiker maakt gebruik van uw app op het apparaat ophalen van de client-ID die u persistent hebt gemaakt.</li></ul><br /> **Opmerking:** Bing antwoorden mogelijk of mag deze header bevatten. Als het antwoord deze header bevat, vastleggen van de client-ID en deze gebruiken voor alle volgende Bing-aanvragen voor de gebruiker op het apparaat.<br /><br /> **Opmerking:** als u de X-MSEdge-ClientID opneemt, moet u cookies niet opnemen in de aanvraag.|  
|<a name="clientip" />X-MSEdge-client-IP|De aanvraagheader is optioneel.<br /><br /> Het IPv4- of IPv6-adres van het clientapparaat. Het IP-adres wordt gebruikt voor het detecteren van de locatie van de gebruiker. Bing gebruikt de locatie-informatie veilig zoekgedrag bepalen.<br /><br /> **Opmerking:** Hoewel optioneel, wordt u aangeraden altijd opgeven om deze en de header X-Search-locatie.<br /><br /> Niet verbergen, weergeven het adres (bijvoorbeeld door het laatste octet wijzigen in 0). De resultaten van het adres in de locatie niet overal in de buurt van de werkelijke locatie van het apparaat wordt obfuscating, waardoor de Bing voor de onjuiste resultaten.|  
|<a name="location" />X-Search-locatie|De aanvraagheader is optioneel.<br /><br /> Een met puntkomma's gescheiden lijst met sleutel-waardeparen die de geografische locatie van de client beschrijven. Bing gebruikt de locatie-informatie om te bepalen van veilige zoekgedrag en terug te keren relevante lokale inhoud. Geef de sleutel-waardepaar als \<sleutel\>:\<waarde\>. Hieronder vindt u de sleutels die u gebruikt om op te geven locatie van de gebruiker.<br /><br /><ul><li>LAT&mdash;vereist. De breedte van de locatie van de client, in graden. De breedte moet groter zijn dan of gelijk zijn aan-90.0 en kleiner dan of gelijk zijn aan +90.0. Negatieve waarden geven zuidelijke Latitude en positieve waarden geven Noord Latitude-systemen.<br /><br /></li><li>lange&mdash;vereist. De lengtegraad van locatie van de client, in graden. De lengte moet groter zijn dan of gelijk zijn aan-180.0 en kleiner dan of gelijk zijn aan +180.0. Negatieve waarden geven western lengten en positieve waarden geven eastern lengten.<br /><br /></li><li>RE&mdash;vereist. De straal, in de meter, waarmee de horizontale nauwkeurigheid van de coördinaten. De waarde die is geretourneerd door het apparaat Locatieservice doorgeven. Normale waarden mogelijk 22m GPS/Wi-Fi, 380m voor cel tower triangulatie en 18.000 m voor reverse lookup van het IP.<br /><br /></li><li>TS&mdash;optioneel. De tijdstempel van de UTC-UNIX wanneer de client zich op de locatie. (De UNIX-tijdstempel is het aantal seconden sinds 1 januari 1970.)<br /><br /></li><li>HEAD&mdash;optioneel. Relatieve kop of richting van de client. Geef de richting van reizen als graden tussen 0 en 360 rechtsom ten opzichte van de waarde true Noord tellen. Geef deze sleutel alleen als de `sp` sleutel gelijk is aan nul.<br /><br /></li><li>SP&mdash;optioneel. De horizontale snelheid (snelheid) in meter per seconde die het clientapparaat is verzonden.<br /><br /></li><li>ALT&mdash;optioneel. De hoogte van het clientapparaat in meters.<br /><br /></li><li>zijn&mdash;optioneel. De straal, in de meter, waarmee de verticale nauwkeurigheid van de coördinaten. Deze sleutel alleen opgeven als u de `alt` sleutel.<br /><br /></li></ul> **Opmerking:** Hoewel veel van de sleutels optioneel zijn, de informatie die u opgeeft, hoe nauwkeuriger de resultaten van de locatie zijn.<br /><br /> **Opmerking:** Hoewel optioneel, wordt u aangeraden altijd opgeven om de geografische locatie van de gebruiker. Voorzien in de locatie is vooral belangrijk als het IP-adres van de client de fysieke locatie van de gebruiker niet nauwkeurig overeen (bijvoorbeeld als de client VPN gebruikt). Voor optimale resultaten, moet u deze en de X-MSEdge-client-IP-header opnemen, maar minimaal, moet u deze header opnemen.|

> [!NOTE] 
> Houd er rekening mee dat de gebruiksvoorwaarden voldoen aan alle toepasselijke wetten moet, waaronder met betrekking tot gebruik van deze headers. Bijvoorbeeld, in bepaalde rechtsgebieden, zoals Europa, zijn er vereisten voor het verkrijgen van de gebruiker toestemming gevraagd voor het plaatsen van bepaalde apparaten bijhouden op apparaten van gebruikers.


<a name="content-form-types" />

### <a name="content-form-types"></a>De typen inhoud formulier

Elke aanvraag vergezeld gaan van de header Content-Type. De header moet worden ingesteld op: formulier-multipart-gegevens; grens =\<grens tekenreeks\>, waarbij \<grens tekenreeks\> is een unieke, ondoorzichtige tekenreeks waarin de grens van de gegevens. Bijvoorbeeld: grens = boundary_1234 abcd.


Als u Visual Search een token van de afbeelding of URL verzendt, ziet hieronder u de gegevens dat moet u in de hoofdtekst van het bericht opnemen. De formuliergegevens vergezeld gaan van de header Content-Disposition en de bijbehorende `name` parameter moet worden ingesteld op 'knowledgeRequest'. Voor meer informatie over de `imageInfo` object, Zie [de aanvraag](#the-request).


```
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "url" : "https://contoso.com/2018/05/fashion/red.jpg"
    }
}

--boundary_1234-abcd--
```

Als u een lokale installatiekopie uploadt, ziet hieronder u de gegevens dat moet u in de hoofdtekst van het bericht opnemen. De formuliergegevens moet de header Content-Disposition bevatten. De `name` parameter moet worden ingesteld op 'installatiekopie' en de `filename` parameter kan worden ingesteld op een willekeurige tekenreeks. De header Content-Type kan worden ingesteld voor alle veelgebruikte MIME-afbeeldingstype. De inhoud van het formulier is het binaire bestand van de afbeelding. De grootte van de maximale installatiekopie die u kan uploaden is 1 MB. 


```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
Content-Type: image/jpeg

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Hieronder ziet u hoe u de regio van belang zijn van een installatiekopie van een geüploade opgeven.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "cropArea" : {
            "top" : 0.2,
            "left" : 0.3,
            "bottom" : 0.7,
            "right" : 0.6
        }
    }
}

--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="image"
Content-Type: image/jpeg


ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```



### <a name="example-request"></a>Voorbeeld van een aanvraag

Hieronder ziet u een volledige afbeelding insights-aanvraag die een token van de installatiekopie en de regio van belang zijn doorgegeven. U krijgt de insights-token van een eerdere aanroep voor /images/search.


```  
POST https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch?mkt=en-us HTTP/1.1  
Content-Type: multipart/form-data; boundary=boundary_1234-abcd
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com 

--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "imageInsightsToken" : "mid_D6426898706EC7..."
        "cropArea" : {
            "top" : 0.1,
            "left" : 0.2,
            "bottom" : 0.7,
            "right" : 0.5
        }
    }
}

--boundary_1234-abcd--
```


## <a name="the-response"></a>Het antwoord

Als er insights beschikbaar voor de installatiekopie, het antwoord bevat een of meer `tags` die de inzichten bevatten. De `image` veld bevat de insights-token voor de installatiekopie van het invoerbestand.

```json
{
  "_type" : "ImageKnowledge",
  "tags" : [
    {...},
    {...},
    {...},
    {...},
    {...}
  ],
  "image" : {
    "imageInsightsToken" : "bcid_AF8C9CA409421B..."
  }
}
```

De `tags` veld bevat een weergavenaam en een lijst van acties (insights). Een van de labels bevat een `displayName` veld dat is ingesteld op een lege tekenreeks. Deze code bevat de standaard inzichten zoals webpagina's met de installatiekopie, visueel soortgelijke afbeeldingen en winkelwagen bronnen voor items gevonden in de installatiekopie. Omdat de volledige afbeelding van belang is, bevat het standaardlabel insights geen begrenzingsvak vakken voor de regio's van belang.


```json
{
  "_type" : "ImageKnowledge",
  "tags" : [
    {
      "displayName" : "",
      "actions" : [
        {...},
        {...},
        {...},
        {...}
      ]
    },
    {...},
    {...},
    {...},
    {...}
  ],
  "image" : {
    "imageInsightsToken" : "bcid_AF8C9CA409421B..."
  }
}
```

Zie voor een lijst van de standaard inzicht [insights standaard](./default-insights-tag.md).



De resterende labels bevatten andere inzichten die mogelijk van belang zijn voor de gebruiker. Als de installatiekopie van het tekst bevat, kan één van de labels bijvoorbeeld een inzicht TextResults, waarin de herkende tekst bevatten. Of als Bing een entiteit (persoon, plaats of ding) in de afbeelding herkent, de entiteit een van de labels kan worden geïdentificeerd. Visual Search retourneert ook een uitgebreide set voorwaarden (tags) afgeleid van de installatiekopie van het invoerbestand. Deze tags kunnen gebruikers om te verkennen concepten gevonden in de installatiekopie. Bijvoorbeeld, als de installatiekopie van de invoer van een beroemdheid atleet, één van de labels mogelijk sport, die bevat koppelingen naar afbeeldingen van sport.

Elke tag bevat een weergavenaam die u gebruiken kunt om de inzichten te categoriseren begrenzingsvak waarmee de regio van belang dat het inzicht toepast op de inzichten zelf en een miniatuur van de afbeelding. Als de installatiekopie van een persoon, draagt een sport-jersey is, kan een van de labels bijvoorbeeld een selectiekader dat de jersey bounds en omvat VisualSearch en ProductVisualSearch insights bevatten. En bevat een andere code mogelijk inzicht ImageResults dat een URL voor een /images/search API-aanvraag voor het ophalen van de installatiekopieën die gerelateerd zijn topisch of een URL voor het zoeken van Bing.com die de gebruiker naar de zoekresultaten van Bing.com installatiekopie doorstuurt bevat.

Alle codes dan de standaard insights tag bevatten begrenzingsvak vakken waarmee gebieden van interesse in de afbeelding geïdentificeerd. Bijvoorbeeld, als de installatiekopie meerdere herkende mensen bevat, tags kunnen zijn vakken begrenzingsvak voor elk van de personen of als de installatiekopie van het herkende kleding items bevat, tags uit voor elk item herkende kleding begrenzingsvak kunnen zijn. U kunt de omsluitende vakken maken hotspots op de afbeelding die als geklikt, vindt u informatie over de inhoud in deze regio van de afbeelding. U mag geen hotspots bevatten in een installatiekopie van een selectiekader vakken identificatie van de volledige afbeelding.

### <a name="text-recognition"></a>Tekstherkenning

Als de installatiekopie tekst dat de service wordt herkend bevat, bevat een van de labels een inzicht TextResults (actie). Het inzicht `displayName` de herkende tekst bevat. 

```json
    {
        "image" : {
            "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23Text..."
        },
        "displayName" : "##TextRecognition",
        "boundingBox" : {
            "queryRectangle" : {
                "topLeft" : {"x" : 0, "y" : 0},
                "topRight" : {"x" : 1, "y" : 0},
                "bottomRight" : {"x" : 1, "y" : 1},
                "bottomLeft" : {"x" : 0, "y" : 1}
            },
            "displayRectangle" : {
                "topLeft" : {"x" : 0, "y" : 0},
                "topRight" : {"x" : 1, "y" : 0},
                "bottomRight" : {"x" : 1, "y" : 1},
                "bottomLeft" : {"x" : 0, "y" : 1}
            }
        },
        "actions" : [{
            "displayName" : "WALK BIKE ACROSS BRIDGE",
            "actionType" : "TextResults"
        }],
        "sources" : ["OCR"]
    }
```

Omdat de tag `displayName` veld bevat ##TextRecognition gebruik dan niet als een categorie in de UX title Toegevoegd voor elke weergave naam die begint met ##. Gebruik in plaats daarvan de weergavenaam van de actie.


Tekstherkenning herkent de contactgegevens op visitekaartjes, zoals telefoonnummers en e-mailadressen. Het begrenzingsvak geeft de locatie van de contactgegevens op de kaart. 

```json
    {
      "image" : {
        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23TextRecognition..."
      },
      "displayName" : "##TextRecognition",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0.635, "y" : 0},
          "topRight" : {"x" : 0.77, "y" : 0},
          "bottomRight" : {"x" : 0.77, "y" : 0.4873333},
          "bottomLeft" : {"x" : 0.635, "y" : 0.4873333}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0.635, "y" : 0},
          "topRight" : {"x" : 0.77, "y" : 0},
          "bottomRight" : {"x" : 0.77, "y" : 0.4873333},
          "bottomLeft" : {"x" : 0.635, "y" : 0.4873333}
        }
      },
      "actions" : [
        {
          "url" : "tel:888%20555%201212",
          "actionType" : "Uri"
        }
      ],
      "sources" : ["OCR"]
    },
    {
      "image" : {
        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23TextRecognition..."
      },
      "displayName" : "##TextRecognition",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0.63, "y" : 0},
          "topRight" : {"x" : 0.866, "y" : 0},
          "bottomRight" : {"x" : 0.866, "y" : 0.5553334},
          "bottomLeft" : {"x" : 0.63, "y" : 0.5553334}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0.63, "y" : 0},
          "topRight" : {"x" : 0.866, "y" : 0},
          "bottomRight" : {"x" : 0.866, "y" : 0.5553334},
          "bottomLeft" : {"x" : 0.63, "y" : 0.5553334}
        }
      },
      "actions" : [
        {
          "url" : "mailto:someone@outlook.com",
          "actionType" : "Uri"
        }
      ],
      "sources" : ["OCR"]
    },
    {
      "image" : {
        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23TextRecognition..."
      },
      "displayName" : "##TextRecognition",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0, "y" : 0},
          "topRight" : {"x" : 1, "y" : 0},
          "bottomRight" : {"x" : 1, "y" : 1},
          "bottomLeft" : {"x" : 0, "y" : 1}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0, "y" : 0},
          "topRight" : {"x" : 1, "y" : 0},
          "bottomRight" : {"x" : 1, "y" : 1},
          "bottomLeft" : {"x" : 0, "y" : 1}
        }
      },
      "actions" : [
        {
          "displayName" : "CHARLENE WHITNEY Graphic Designer 888 555 1212 someone@outlook.com www.contoso.com",
          "actionType" : "TextResults"
        }
      ],
      "sources" : ["OCR"]
    }
```

Als de afbeelding een herkende entiteit zoals een persoon, plaats of ding bevat, kan een van de labels inzicht entiteit bevatten. Entiteiten kunnen ook algemene vragen bevatten, zoals wordt weergegeven in het volgende voorbeeld:

```json
    {
      "image" : {
        "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?q=Statue+of+Liberty..."
      },
      "displayName" : "Statue of Liberty",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0.40625, "y" : 0.1757813},
          "topRight" : {"x" : 0.6171875, "y" : 0.1757813},
          "bottomRight" : {"x" : 0.6171875, "y" : 0.3867188},
          "bottomLeft" : {"x" : 0.40625, "y" : 0.3867188}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0.40625, "y" : 0.1757813},
          "topRight" : {"x" : 0.6171875, "y" : 0.1757813},
          "bottomRight" : {"x" : 0.6171875, "y" : 0.3867188},
          "bottomLeft" : {"x" : 0.40625, "y" : 0.3867188}
        }
      },
      "actions" : [
        {
          "_type" : "ImageEntityAction",
          "webSearchUrl" : "https:\/\/www.bing.com\/search?q=Statue+of+Liberty",
          "displayName" : "Statue of Liberty",
          "actionType" : "Entity",
        },
        {
          "_type" : "ImageModuleAction",
          "actionType" : "Trivia",
          "data" : {
            "value" : [
              {
                "name" : "Where was the cornerstone of the statue of liberty laid",
                "text" : "<the answer>",
                "hostPageUrl" : "http:\/\/contoso.com\/history\/...",
              },
              {
                "name" : "Why Is the Statue of Liberty Green",
                "text" : "<the answer>",
                "hostPageUrl" : "https:\/\/www.contoso.com\/why-statue-of-liberty-is-green",
              },
              {
                "name" : "What is the Statue of Liberty made of",
                "text" : "<the answer>",
                "hostPageUrl" : "https:\/\/www.contoso.com\/art-literature\/statue-liberty-made",
              }
            ]
          }
        }
      ]
    }
```



## <a name="next-steps"></a>Volgende stappen

Om snel aan de slag met de eerste aanvraag, Zie de snelstartgidsen: [C#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [node.js](quickstarts/nodejs.md)  |  [Python](quickstarts/python.md).

De API uitproberen. Ga naar [Visual zoeken-API voor testdoeleinden Console](https://dev.cognitive.microsoft.com/docs/services/878c38e705b84442845e22c7bff8c9ac). 


Vertrouwd raken met de [Visual zoeken-API-referentiemateriaal](https://aka.ms/bingvisualsearchreferencedoc). De verwijzing bevat de lijst met eindpunten, kopteksten en queryparameters die u gebruiken wilt om aan te vragen van de zoekresultaten. Dit omvat ook de definities van de antwoord-objecten. 

Lees [Bing gebruiken en de vereisten van de weergave](./use-and-display-requirements.md) zodat u de regels over het gebruik van de zoekresultaten niet verbreken.


