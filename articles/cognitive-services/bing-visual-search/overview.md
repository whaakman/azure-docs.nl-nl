---
title: Bing visuele zoekopdrachten-API-overzicht | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Laat zien hoe u meer informatie of inzichten over een afbeelding, zoals vergelijkbare afbeeldingen of winkelwagen bronnen.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 04/10/2018
ms.author: scottwhi
ms.openlocfilehash: aa563d89b1834f5be952f13c31a2451d809709b1
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39006526"
---
# <a name="what-is-bing-visual-search-api"></a>Wat is Bing Visual Search-API?

Bing visuele zoekopdrachten-API biedt een ervaring die vergelijkbaar is met de details van de afbeelding op Bing.com/images weergegeven. Met visuele zoekopdrachten, kunt u een afbeelding uploaden en weer toegang krijgen van inzicht in de afbeelding, zoals visueel vergelijkbare afbeeldingen, bronnen voor winkelen, webpagina's, zoals de installatiekopie en meer. In plaats van het uploaden van een afbeelding, kunt u ook een token inzichten die u via een installatiekopie in de zoekresultaten voor installatiekopieën opgeven (Zie [afbeeldingen-API voor Bing](../bing-image-search/overview.md)).

Visuele zoekopdrachten kunt identificeren beroemdheden, monumenten en monumenten, kunstwerken, home meubels, wijze, producten, tekenherkenning (OCR) en meer.

Hier volgen de inzichten die Visual Search kunt u detecteren.

- Visueel vergelijkbare afbeeldingen&mdash;een lijst met installatiekopieën die visueel op de afbeelding lijken
- Visueel vergelijkbare producten&mdash;een lijst met installatiekopieën met producten die visueel vergelijkbaar met het product wordt weergegeven in de afbeelding zijn
- Bronnen winkelen&mdash;een lijst met locaties waar u het item wordt weergegeven in de afbeelding kunt kopen
- Gerelateerde zoekopdrachten&mdash;een lijst met verwante zoekopdrachten die zijn gemaakt door anderen of die zijn gebaseerd op de inhoud van de installatiekopie
- Webpagina's met de installatiekopie van het&mdash;een lijst van webpagina's met de invoer van installatiekopieën
- Recepten&mdash;een lijst van webpagina's met recepten voor het maken van het gerecht wordt weergegeven in de afbeelding

Naast deze inzichten kunt retourneert Visual Search ook een groot aantal voorwaarden (tags) worden afgeleid van de afbeelding. Deze tags kunnen gebruikers verkennen concepten gevonden in de afbeelding. Bijvoorbeeld, als de afbeelding van een beroemde atleet, één van de labels kan overeenkomen met de naam van de atleet, een andere code kan worden Sport. Of, als de afbeelding van een cirkel apple, de labels Apple cirkel, Pies, Desserts, kunnen worden, zodat gebruikers verwante concepten onderzoeken kunnen.

De visuele zoekresultaten bevatten ook omsluitende kaders voor regio's in de installatiekopie van het van belang. Bijvoorbeeld, als de afbeelding verschillende beroemdheden bevat, bevatten de resultaten vakken begrenzingsvak voor elk van de herkende beroemdheden in de afbeelding. Of, als Bing een product of de kledingfabrikanten in de afbeelding herkent, het resultaat een begrenzingsvak voor het product dat wordt herkend of kleding item bevatten.

> [!IMPORTANT]
> Als u de/afbeeldingen/details-eindpunt op [verkrijgen van inzichten in afbeeldingen](../bing-image-search/image-insights.md), moet u uw code voor het gebruik van visuele zoekopdrachten in plaats daarvan omdat het uitgebreidere inzichten biedt bijwerken.


## <a name="the-request"></a>De aanvraag

De volgende zijn de opties voor het verkrijgen van inzichten over een afbeelding. 

- Verzenden van een token inzichten die u via een installatiekopie in een vorige aanroep aan een van de [afbeeldingen-API voor Bing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) eindpunten
- De URL van een afbeelding verzenden
- Upload een afbeelding (binaire)


Als u Visual Search een token van de installatiekopie of een URL verzendt, ziet hieronder u de JSON-object dat u in de hoofdtekst van het bericht moet bevatten. 

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

De `imageInfo` object moet een bevatten de `url` of `imageInsightsToken` veld, maar niet beide. Stel de `url` veld naar de URL van een installatiekopie van een Internet toegankelijk is. Grootte van de maximale ondersteunde installatiekopie is 1 MB.

De `imageInsightsToken` moet worden ingesteld op een token inzichten. Als u een token inzichten, roept u de Bing afbeeldingen-API. Het antwoord bevat een lijst met `Image` objecten. Elke `Image` -object bevat een `imageInsightsToken` veld, die de token bevat.

De `cropArea` veld is optioneel. Het gebied bijsnijden Hiermee geeft u de top, linkerhoek en onder, de rechterhoek van een gebied van belang zijn. Geef de waarden in het bereik 0,0 en 1,0. De waarden zijn een percentage van de totale breedte of hoogte. Bijvoorbeeld markeert het bovenstaande voorbeeld het recht de helft van de installatiekopie als de regio van belang zijn. Opnemen als u wilt beperken de insights-aanvraag naar de regio van belang zijn.

De `filters` -object bevat een site-filter (Zie de `site` veld) waarmee u kunt de vergelijkbare afbeeldingen en vergelijkbare producten resultaten beperken tot een specifiek domein. Bijvoorbeeld, als de installatiekopie van een Surface Book is, kunt u instellen `site` naar www.microsoft.com. 

Als u inzicht verkijgen in een lokale kopie van een afbeelding wilt, moet u de installatiekopie uploaden als binaire gegevens.

Zie voor meer informatie over het toevoegen van deze opties in de hoofdtekst van het bericht [inhoudstypen uw formulier](#content-form-types).


### <a name="endpoint"></a>Eindpunt

Het visuele zoekopdrachten-eindpunt: https:\/\/api.cognitive.microsoft.com/bing/v7.0/images/visualsearch.

Aanvragen moeten worden verzonden als een HTTP POST-aanvragen. 


### <a name="query-parameters"></a>Queryparameters

Hier volgen de queryparameters die uw aanvraag moet opgeven. U moet ten minste opnemen de `mkt` queryparameter.

|Naam|Waarde|Type|Vereist|  
|----------|-----------|----------|--------------|  
|<a name="cc" />CC|Een 2-teken landcode van het land waar de resultaten afkomstig zijn uit.<br /><br /> Als u deze parameter instelt, moet u ook opgeven de [Accept-Language](#acceptlanguage) header. Bing maakt gebruik van de eerste ondersteunde taal gezocht in de lijst met talen en de taal combineert met de landcode die u opgeeft om te bepalen van de markt te retourneren van resultaten uit. Als de lijst met talen niet onder een ondersteunde taal, Bing zoeken naar de dichtstbijzijnde taal en markt die ondersteuning biedt voor de aanvraag. Of het is mogelijk gebruik van een samengevoegde of standaard markt voor de resultaten in plaats van de opgegeven ETag.<br /><br /> Moet u deze queryparameter en de `Accept-Language` queryparameter alleen als u meerdere talen opgeven; anders moet u de `mkt` en `setLang` queryparameters.<br /><br /> Deze parameter en de [mkt](#mkt) queryparameter zijn sluiten elkaar wederzijds uit&mdash;beide niet opgeven.|Reeks|Nee|  
|<a name="mkt" />Mkt|De markt waarin de resultaten afkomstig zijn uit. <br /><br /> **Opmerking:** u wordt aangeraden altijd opgeven om de markt, indien bekend. De markt op te geven, kunt u Bing routeren van de aanvraag en een antwoord nodig en optimale retourneren.<br /><br /> Deze parameter en de [cc](#cc) queryparameter zijn sluiten elkaar wederzijds uit&mdash;beide niet opgeven.|Reeks|Ja|  
|<a name="safesearch" />veilig zoeken|Een filter dat wordt gebruikt voor het filteren van inhoud voor volwassenen. Hier volgen de mogelijke niet-hoofdlettergevoelige filterwaarden.<br /><ul><li>Uit&mdash;webpagina's met volwassen tekst of afbeeldingen retourneren.<br /><br/></li><li>Gemiddeld&mdash;webpagina's met volwassen tekst, maar niet volwassen installatiekopieën te retourneren.<br /><br/></li><li>Strikte&mdash;webpagina's met volwassen tekst of afbeeldingen niet retourneren.</li></ul><br /> De standaardwaarde is normaal.<br /><br /> **Opmerking:** als de aanvraag afkomstig van een markt is van die Bing volwassenen beleid vereist dat `safeSearch` worden ingesteld op strikt, Bing negeert de `safeSearch` waarde en Strict gebruikt.<br/><br/>**Opmerking:** als u de `site:` query-operator, bestaat de kans dat het antwoord inhoud voor volwassen, ongeacht wat bevatten de `safeSearch` queryparameter is ingesteld op. Gebruik `site:` alleen als u zich bewust bent van de inhoud op de site en de mogelijkheid om inhoud voor volwassenen biedt ondersteuning voor uw scenario. |Reeks|Nee|  
|<a name="setlang" />taal instellen|De taal die moet worden gebruikt voor tekenreeksen voor interface. Geef de taal met behulp van de ISO 639-1-2-letter-taalcode. Bijvoorbeeld, is de taalcode voor Engels EN. De standaardwaarde is EN (Engels).<br /><br /> Hoewel dit optioneel is, moet u altijd de taal. Meestal stelt `setLang` op dezelfde taal opgegeven door `mkt` , tenzij de gebruiker wil dat de gebruikersinterface tekenreeksen weergegeven in een andere taal.<br /><br /> Deze parameter en de [Accept-Language](#acceptlanguage) header zijn sluiten elkaar wederzijds uit&mdash;beide niet opgeven.<br /><br /> Een tekenreeks van de interface is een tekenreeks die wordt gebruikt als een label in een gebruikersinterface. Er zijn enkele interface tekenreeksen in de JSON-antwoord-objecten. Ook een koppeling maakt naar Bing.com eigenschappen in de antwoordobjecten gelden voor de opgegeven taal.|Reeks|Nee| 

### <a name="headers"></a>Headers

Hier volgen de headers die uw aanvraag moet opgeven. De Content-Type en de Ocp-Apim-Subscription-Key-headers zijn de enige vereiste kopteksten, maar u moet ook gebruikersagent, X-MSEdge-ClientID, X-MSEdge-client-IP en X zoeklocatie opnemen.


|Koptekst|Beschrijving|  
|------------|-----------------|  
|<a name="acceptlanguage" />Accepteer taal|De aanvraagheader is optioneel.<br /><br /> Een door komma's gescheiden lijst met talen voor tekenreeksen voor interface. De lijst is in aflopende volgorde van voorkeur. Zie voor meer informatie, waaronder de verwachte indeling [RFC2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Deze header en het [taal instellen](#setlang) queryparameter zijn sluiten elkaar wederzijds uit&mdash;beide niet opgeven.<br /><br /> Als u deze header instellen, moet u ook opgeven de [cc](#cc) queryparameter. Om te bepalen de markt te retourneren van resultaten voor, Bing maakt gebruik van de eerste ondersteunde taal die wordt gevonden in de lijst, en combineert met de `cc` parameterwaarde. Als de lijst bevat geen een ondersteunde taal, Bing zoeken naar de dichtstbijzijnde taal en markt die ondersteuning biedt voor de aanvraag of het maakt gebruik van een samengevoegde of standaard markt voor de resultaten. Om te bepalen de markt die Bing gebruikt, Zie de BingAPIs-Market-header.<br /><br /> Gebruik deze header en het `cc` queryparameter alleen als u meerdere talen opgeven. Gebruik anders de [mkt](#mkt) en [taal instellen](#setlang) queryparameters.<br /><br /> Een tekenreeks van de interface is een tekenreeks die wordt gebruikt als een label in een gebruikersinterface. Er zijn enkele interface tekenreeksen in de JSON-antwoord-objecten. Een koppeling maakt naar Bing.com eigenschappen in de reactie-objecten zijn van toepassing de opgegeven taal.|  
|<a name="contenttype" />Content-Type|De aanvraagheader is vereist.<br /><br />Moet worden ingesteld op multipart/formuliergegevens en voegt u een grens parameter toe (bijvoorbeeld multipart/formuliergegevens; grens =\<grens tekenreeks\>). Zie voor meer informatie, [inhoudstypen uw formulier](#content-form-types).
|<a name="market" />BingAPIs-market te versnellen|Antwoordheader.<br /><br /> De markt die worden gebruikt door de aanvraag. Het formulier is \<languageCode\>-\<countryCode\>. Bijvoorbeeld: nl-nl.|  
|<a name="traceid" />BingAPIs TraceId|Antwoordheader.<br /><br /> De ID van de vermelding die de details van de aanvraag bevat. Wanneer er een fout optreedt, vastleggen van deze ID. Als u niet kunt bepalen en los het probleem, zijn deze ID samen met de andere informatie die het ondersteuningsteam op te geven.|  
|<a name="subscriptionkey" />OCP-Apim-Subscription-Key|De aanvraagheader is vereist.<br /><br /> De abonnementssleutel die u hebt ontvangen toen u zich hebt geregistreerd voor deze service in [Cognitive Services](https://www.microsoft.com/cognitive-services/).|  
|<a name="pragma" />Pragma|Optionele aanvraagheader<br /><br /> Bing retourneert standaard in de cache inhoud, indien beschikbaar. Om te voorkomen dat Bing retourneren in de cache inhoud, stelt u de header Pragma op no-cache (bijvoorbeeld Pragma: niet-cache).
|<a name="useragent" />Gebruikersagent|De aanvraagheader is optioneel.<br /><br /> De gebruikersagent die afkomstig zijn van de aanvraag. Bing maakt gebruik van de gebruikersagent zodat mobiele gebruikers beschikken over een optimale ervaring. Hoewel dit optioneel is, wordt u aangeraden altijd opgeven om deze header.<br /><br /> De gebruikersagent moet dezelfde tekenreeks waarmee elke gebruikte browser wordt verzonden. Zie voor meer informatie over gebruikersagenten [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Hier volgen enkele voorbeelden van gebruiker agent tekenreeksen.<br /><ul><li>Windows Phone&mdash;Mozilla 5.0 (compatibel; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)<br /><br /></li><li>Android&mdash;Mozilla 5.0 (Linux; U; Android 2.3.5; en-us; SCH-I500 Build/ONTBIJTKOEK) AppleWebKit/533.1 (KHTML; zoals Gecko) versie 4.0/Mobile Safari/533.1<br /><br /></li><li>iPhone&mdash;Mozilla 5.0 (iPhone; CPU iPhone OS 6_1, zoals Mac OS X) AppleWebKit/536.26 (KHTML; zoals Gecko) mobiele/10B142 iPhone4; 1 BingWeb/3.03.1428.20120423<br /><br /></li><li>PC&mdash;Mozilla 5.0 (Windows NT 6.3; WOW64-MODUS; Trident/7.0; Touch; RV:11.0), zoals Gecko<br /><br /></li><li>iPad&mdash;Mozilla 5.0 (iPad; CPU-OS 7_0, zoals Mac OS X) AppleWebKit/537.51.1 (zoals Gecko KHTML) versie/7.0 Mobile/11A465 Safari/9537.53</li></ul>|
|<a name="clientid" />X-MSEdge-ClientID|Optionele aanvraag en antwoord-header.<br /><br /> Bing maakt gebruik van deze header voor gebruikers met een consistent gedrag voor Bing-API-aanroepen. Bing vluchten vaak nieuwe functies en verbeteringen en gebruikt de client-ID als een sleutel voor het toewijzen van verkeer op verschillende vluchten. Als u gebruik niet dezelfde client-ID voor een gebruiker met meerdere aanvragen, kunnen Bing kan de gebruiker toewijzen aan meerdere conflicterende vluchten. Wordt toegewezen aan meerdere conflicterende vluchten kan leiden tot een inconsistente gebruikerservaring. Bijvoorbeeld, als de tweede aanvraag de toewijzing van een andere vlucht dan de eerste heeft, de ervaring mogelijk onverwachte. Bing kunt ook de client-ID gebruiken om aan te passen webresultaten voor deze client id zoekgeschiedenis, bieden een rijkere ervaring voor de gebruiker.<br /><br /> Bing gebruikt ook deze header om het resultaat classificaties verbeteren door het analyseren van de activiteit die is gegenereerd door een client-ID. Verbeteringen in de volgorde van relevantie helpen met hogere kwaliteit van de resultaten die worden geleverd door Bing-API's en op zijn beurt hoger kunnen via klikken tarieven voor de API-consument.<br /><br /> **Belangrijk:** Hoewel dit optioneel is, kunt u overwegen deze header vereist. Opslaan van de client-ID voor meerdere aanvragen voor de dezelfde gebruiker en apparaat combinatie kan (1) de consument API voor het ontvangen van een consistente gebruikerservaring, en 2) een hogere doorklikken tarieven via betere kwaliteit van de resultaten van de Bing-API's.<br /><br /> Hier volgen de basisgebruik regels die betrekking hebben op deze header.<br /><ul><li>Elke gebruiker die gebruikmaakt van uw toepassing op het apparaat moet een unieke, Bing gegenereerde client-ID.<br /><br/>Als u deze header in de aanvraag niet opgeeft, wordt Bing een ID gegenereerd en in de header X-MSEdge-ClientID-antwoord geretourneerd. De enige keer dat u moet niet deze header bevatten in een aanvraag is de eerste keer dat de gebruiker maakt gebruik van uw app op het apparaat.<br /><br/></li><li>**Let op:** moet u ervoor zorgen dat deze Client-ID niet gerelateerd aan de accountgegevens van elke geverifieerde gebruiker is.</li><li>De client-ID gebruiken voor elke API voor Bing-aanvraag die uw app voor deze gebruiker op het apparaat maakt.<br /><br/></li><li>Behouden van de client-ID. Om te blijven behouden de ID in een browser-app, gebruikt u een permanente HTTP-cookie om te controleren of dat de ID wordt gebruikt in alle sessies. Gebruik een sessiecookie niet. Voor andere apps, zoals mobiele apps, gebruikt u de permanente opslag van het apparaat om vast te leggen van de ID.<br /><br/>De volgende keer dat de gebruiker maakt gebruik van uw app op het apparaat, krijgen de client-ID die u wilt behouden.</li></ul><br /> **Opmerking:** Bing antwoorden kunnen of kan niet deze header bevatten. Als het antwoord deze header bevat, vastleggen van de client-ID en deze gebruiken voor alle volgende Bing-aanvragen voor de gebruiker op het apparaat.<br /><br /> **Opmerking:** als u de X-MSEdge-ClientID opneemt, moet u cookies niet opnemen in de aanvraag.|  
|<a name="clientip" />X-MSEdge-client-IP|De aanvraagheader is optioneel.<br /><br /> Het IPv4- of IPv6-adres van het clientapparaat. Het IP-adres wordt gebruikt voor het detecteren van de locatie van de gebruiker. De locatie-informatie Bing gebruikt om veilige zoekgedrag te bepalen.<br /><br /> **Opmerking:** Hoewel dit optioneel is, u wordt aangeraden deze header en de X-Search-Location-header altijd opgeven.<br /><br /> Onleesbaar niet het adres maakt (bijvoorbeeld door het laatste achttal werd wijzigen in 0). Obfuscating de adres-resultaten op de locatie niet wordt overal in de buurt van de werkelijke locatie van het apparaat, die kan leiden tot onjuiste resultaten voor Bing.|  
|<a name="location" />X-Search-locatie|De aanvraagheader is optioneel.<br /><br /> Een met puntkomma's gescheiden lijst met sleutel/waarde-paren die de geografische locatie van de client beschrijven. Bing maakt gebruik van de locatie-informatie om te bepalen het gedrag van veilig zoeken en om terug te keren relevante lokale inhoud. Geef de sleutel/waarde-paar als \<sleutel\>:\<waarde\>. Hieronder vindt u de sleutels die u gebruikt om op te geven van de locatie van de gebruiker.<br /><br /><ul><li>LAT&mdash;vereist. De breedte van de locatie van de client, in graden. De breedtegraad moet groter zijn dan of gelijk zijn aan-90.0 en kleiner dan of gelijk zijn aan +90.0. Negatieve waarden geven zuidelijk Latitude en positieve waarden geven Noord Latitude.<br /><br /></li><li>lange&mdash;vereist. De lengtegraad van de locatie van de client, in graden. De lengte moet groter zijn dan of gelijk zijn aan-180.0 en kleiner dan of gelijk zijn aan +180.0. Negatieve waarden geven West lengten en positieve waarden geven eastern lengten.<br /><br /></li><li>RE&mdash;vereist. De radius, in meters, waarmee de horizontale nauwkeurigheid van de coördinaten. Geeft de waarde die wordt geretourneerd door de locatie-service van het apparaat. Typische waarden mogelijk 22m voor GPS/Wi-Fi, 380m voor cel tower triangulatie en 18.000 m voor reverse lookup van IP.<br /><br /></li><li>TS&mdash;optioneel. De UTC-UNIX-timestamp van wanneer de client op de locatie is. (De UNIX-timestamp is het aantal seconden sinds 1 januari 1970.)<br /><br /></li><li>HEAD&mdash;optioneel. Relatieve kop of richting van de client. Geef de richting van reizen als graden tussen 0 en 360, tellen rechtsom ten opzichte van de waarde true Noord. Geef deze sleutel alleen als de `sp` sleutel gelijk is aan nul.<br /><br /></li><li>SP&mdash;optioneel. De horizontale snelheid (snelheid), in meter per seconde, die de client-apparaat is verzonden.<br /><br /></li><li>ALT&mdash;optioneel. De hoogte van het clientapparaat, in meters.<br /><br /></li><li>zijn&mdash;optioneel. De radius, in meters, waarmee de verticale nauwkeurigheid van de coördinaten. Deze sleutel alleen opgeven als u de `alt` sleutel.<br /><br /></li></ul> **Opmerking:** Hoewel veel van de sleutels optioneel zijn, de informatie die u opgeeft, hoe nauwkeuriger de resultaten van de locatie zijn.<br /><br /> **Opmerking:** Hoewel dit optioneel is, u wordt aangeraden om op te geven altijd de geografische locatie van de gebruiker. De locatie die is vooral belangrijk als de fysieke locatie van de gebruiker (bijvoorbeeld, als de client maakt gebruik van VPN) in de IP-adres van de client niet nauwkeurig worden weergegeven. Voor optimale resultaten, moet u deze en de X-MSEdge-client-IP-header opnemen, maar ten minste, moet u deze header bevatten.|

> [!NOTE] 
> Houd er rekening mee dat de gebruiksvoorwaarden zijn vereist voor compatibiliteit met alle van toepassing zijnde wetten, waaronder met betrekking tot gebruik van deze headers. Bijvoorbeeld, in bepaalde rechtsgebieden, zoals Europa, zijn er vereisten voor het verkrijgen van gebruikers toestemming geven voordat het plaatsen van bepaalde apparaten bijhouden op apparaten van gebruikers.


<a name="content-form-types" />

### <a name="content-form-types"></a>Typen inhoud

Elke aanvraag moet de Content-Type-header bevatten. De header moet worden ingesteld op: multipart/formuliergegevens; grens =\<grens tekenreeks\>, waarbij \<grens tekenreeks\> is een unieke, ondoorzichtige tekenreeks die de grens van de gegevens identificeert. Bijvoorbeeld, grens = boundary_1234 abcd.


Als u Visual Search een token van de installatiekopie of een URL verzendt, ziet hieronder u de gegevens dat moet u in de hoofdtekst van het bericht opnemen. De gegevens van het formulier de header Content-Disposition moet bevatten en de bijbehorende `name` parameter moet worden ingesteld op "knowledgeRequest." Voor meer informatie over de `imageInfo` object, Zie [de aanvraag](#the-request).


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

Als u een lokale installatiekopie uploadt, ziet hieronder u de gegevens dat moet u in de hoofdtekst van het bericht opnemen. De gegevens moet de header Content-Disposition bevatten. De `name` parameter moet worden ingesteld op 'afbeelding' en de `filename` parameter kan worden ingesteld op een willekeurige tekenreeks. De header Content-Type kan worden ingesteld op een installatiekopie van de meest gebruikte mime-type. De inhoud van het formulier is het binaire bestand van de installatiekopie. De grootte van de maximale installatiekopie die u kunt uploaden is 1 MB. De grootste van de breedte of hoogte moet zijn 1500 pixels of minder.


```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
Content-Type: image/jpeg

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Hieronder ziet u hoe u de regio van belang van geüploade afbeeldingen op te geven.

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



### <a name="example-request"></a>Van de voorbeeldaanvraag

Hieronder ziet u een aanvraag van de volledige installatiekopie inzichten die een token van de installatiekopie en de regio van belang zijn doorgegeven. U ophalen de insights-token van een vorige aanroep aan /images/search.


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

Als er inzicht geboden beschikbaar voor de installatiekopie wordt, het antwoord bevat een of meer `tags` die de inzichten bevatten. De `image` veld de insights-token voor de installatiekopie van de invoer bevat.

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

De `tags` veld bevat een weergavenaam en lijst met acties (inzichten). Een van de labels bevat een `displayName` veld dat is ingesteld op een lege tekenreeks. Dit label bevat de standaard-inzichten zoals webpagina's met de installatiekopie, visueel vergelijkbare afbeeldingen en winkelwagen bronnen voor items gevonden in de afbeelding. Omdat de volledige afbeelding van belang zijn is, de standaard insights-code bevat geen begrenzingsvak vakken voor de regio's van belang zijn.


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

Zie voor een lijst van de standaard-inzichten, [insights standaard](./default-insights-tag.md).



De overige tags bevatten verkrijgen van informatie die van belang zijn voor de gebruiker kunnen zijn. Als de afbeelding tekst bevat, kan één van de labels bijvoorbeeld een TextResults inzicht, en waarin de herkende tekst bevatten. Of, als Bing een entiteit (persoon, plaats of ding) in de afbeelding herkent, een van de labels kan worden geïdentificeerd door de entiteit. Visuele zoekopdrachten retourneert ook een groot aantal voorwaarden (tags) worden afgeleid van de afbeelding. Deze tags kunnen gebruikers verkennen concepten gevonden in de afbeelding. Bijvoorbeeld, als de afbeelding van een beroemde atleet, één van de labels mogelijk sport, die bevat koppelingen naar afbeeldingen van de Sport.

Elke tag omvat een weergavenaam die u gebruiken kunt voor het categoriseren van het inzicht vak dat de regio van belang dat het inzicht van toepassing op de inzichten zelf en een miniatuur van de installatiekopie. Als de installatiekopie van een persoon met een jersey sport, kan één van de labels voor bijvoorbeeld een selectiekader dat de jersey bounds en bevat inzichten met VisualSearch en ProductVisualSearch bevatten. En een andere code advies inwinnen bij een ImageResults inzicht met een URL voor een /images/search API-aanvraag voor het ophalen van installatiekopieën die gerelateerd zijn plaatselijk aangebracht of een URL voor het zoeken van Bing.com waarmee de gebruiker de zoekresultaten voor installatiekopieën van Bing.com.

Alle tags dan de standaard insights tag bevatten omsluitende kaders die regio's in de installatiekopie van het van belang te identificeren. Bijvoorbeeld, als de afbeelding meerdere herkende mensen bevat, codes kunnen bevatten vakken begrenzingsvak voor elk van de personen of als de installatiekopie van het herkende kleding items bevat, tags uit voor elk item herkende kleding begrenzingsvak kunnen bevatten. Het omsluitende kaders kunt u maken hotspots op de afbeelding die als hebt geklikt, vindt u informatie over de inhoud in deze regio van de installatiekopie. U moet hotspots niet opnemen in een afbeelding voor omsluitende vakken waarmee de volledige afbeelding worden geïdentificeerd.

### <a name="text-recognition"></a>Tekstherkenning

Als de afbeelding bevat de tekst die door de service worden herkend, wordt één van de labels een TextResults inzicht (actie) bevatten. Van het inzicht `displayName` de herkende tekst bevat. 

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

Omdat van de tag `displayName` ##TextRecognition bevat, niet gebruiken als de titel van een categorie in de UX Dat geldt voor elke weergave een naam die begint met ##. In plaats daarvan gebruikt u de weergavenaam van de actie.


Tekstherkenning herkent ook de contactgegevens op visitekaartjes, zoals telefoonnummers en e-mailadressen. De locatie van de contactgegevens op de kaart wordt aangegeven door het selectiekader. 

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

Als de afbeelding een herkende entiteit, zoals een persoon, een plaats of een ding bevat, bevatten één van de labels inzicht van de entiteit. 

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
        }
      ]
    }
```



## <a name="next-steps"></a>Volgende stappen

Als u wilt snel aan de slag met uw eerste aanvraag, Zie de Quick starts: [C#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [node.js](quickstarts/nodejs.md)  |  [Python](quickstarts/python.md).

Probeer nu de API. Ga naar [visuele zoekopdrachten-API voor testdoeleinden Console](https://dev.cognitive.microsoft.com/docs/services/878c38e705b84442845e22c7bff8c9ac). 


Maak uzelf vertrouwd met de [Visual Search API-verwijzing](https://aka.ms/bingvisualsearchreferencedoc). De handleiding bevat de lijst met eindpunten, headers en queryparameters die u nodig hebt om zoekresultaten op te vragen. Daarnaast vindt u hier definities van de responsobjecten. 

Lees [Gebruiks- en weergavevereisten voor Bing](./use-and-display-requirements.md) om er zeker van te zijn dat u alle regels voor het gebruik van de zoekresultaten volgt.


