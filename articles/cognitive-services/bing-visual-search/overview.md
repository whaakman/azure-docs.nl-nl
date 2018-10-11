---
title: Wat is Bing Visual Search?
titleSuffix: Azure Cognitive Services
description: Bing Visual Search biedt details of inzichten over een afbeelding, zoals vergelijkbare afbeeldingen of winkelbronnen.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: overview
ms.date: 04/10/2018
ms.author: scottwhi
ms.openlocfilehash: 85240f1f44278cec0f76c9b4e2e79aa065224840
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48886292"
---
# <a name="what-is-bing-visual-search-api"></a>Wat is Bing Visual Search-API?

Bing Visual Search-API biedt een ervaring die lijkt op de afbeeldingsdetails die worden getoond op Bing.com/images. Met Visual Search kunt u een afbeelding uploaden en inzichten over de afbeelding terugkrijgen, zoals visueel vergelijkbare afbeeldingen, winkelbronnen, webpagina's met de afbeelding en meer. In plaats van een afbeelding te uploaden, kunt u ook een inzichttoken opgeven dat u krijgt van een afbeelding in de zoekresultaten van afbeeldingen (zie [Bing Images API](../bing-image-search/overview.md)).

Visual Search kan beroemdheden, monumenten en oriëntatiepunten, kunstwerken, woninginrichting, mode, producten, tekenherkenning (OCR) en meer identificeren.

U kunt de volgende inzichten ontdekken met Visual Search.

- Visueel vergelijkbare afbeeldingen&mdash;een lijst met afbeeldingen die visueel op de ingevoerde afbeelding lijken
- Visueel vergelijkbare producten&mdash;een lijst met afbeeldingen van producten die visueel vergelijkbaar zijn met het in de ingevoerde afbeelding getoonde product
- Winkelbronnen&mdash;Een lijst met plaatsen waar u het artikel kunt kopen dat wordt weergegeven in de ingevoerde afbeelding
- Verwante zoekopdrachten&mdash;Een lijst met verwante zoekopdrachten die door anderen zijn gedaan of die zijn gebaseerd op de inhoud van de afbeelding
- Webpagina's met de afbeelding&mdash;Een lijst met webpagina's die de ingevoerde afbeelding bevatten
- Recepten&mdash;Een lijst van webpagina's met recepten voor het maken van het gerecht dat wordt weergegeven in de ingevoerde afbeelding

Naast deze inzichten retourneert Visual Search ook een diverse reeks termen (tags) die zijn afgeleid van de ingevoerde afbeelding. Aan de hand van deze tags kunnen gebruikers in de afbeelding gevonden concepten verkennen. Als de ingevoerde afbeelding bijvoorbeeld van een beroemde atleet is, kan een van de tags de naam van de atleet zijn, een andere tag kan Sport zijn. Of als de ingevoerde afbeelding een appeltaart is, kunnen de tags Appeltaart, Taarten, Toetjes zijn, zodat gebruikers gerelateerde concepten kunnen verkennen.

De resultaten van Visual Search bevatten ook begrenzingsvakken voor interessegebieden in de afbeelding. Als de afbeelding bijvoorbeeld meerdere beroemdheden bevat, kunnen de resultaten begrenzingsvakken bevatten voor elk van de herkende beroemdheden in de afbeelding. Of, als Bing een product of kledingstuk herkent in de afbeelding, kan het resultaat een begrenzingsvak bevatten voor het herkende product of kledingstuk.

> [!IMPORTANT]
> Als u het eindpunt /images/details gebruikt om [afbeeldingsinzichten](../bing-image-search/image-insights.md) te krijgen, moet u uw code bijwerken om Visual Search te gebruiken, omdat dit meer uitgebreide inzichten oplevert.


## <a name="the-request"></a>De aanvraag

De volgende zijn de opties voor het verkrijgen van inzichten over een afbeelding. 

- Een inzichttoken versturen dat u hebt ontvangen van een afbeelding in een eerdere aanroep naar een van de [Bing Images API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)-eindpunten
- De URL van een afbeelding verzenden
- Een afbeelding uploaden (binair)


Als u een afbeeldingstoken of URL naar Visual Search verzendt, wordt hieronder het JSON-object weergegeven dat u in de hoofdtekst van de POST moet opnemen. 

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

Het `imageInfo`-object moet het veld `url` of `imageInsightsToken` bevatten, maar niet beide. Stel het veld `url` in op de URL van een via internet toegankelijke afbeelding. De maximale ondersteunde afbeeldingsgrootte is 1 MB.

De `imageInsightsToken` moet zijn ingesteld op een inzichttoken. Roep de Bing Image API aan om een inzichttoken te krijgen. Het antwoord bevat een lijst met `Image`-objecten. Elke `Image`-object bevat een `imageInsightsToken`-veld, dat het token bevat.

Het veld `cropArea` is optioneel. Het bijsnijdgebied geeft de linkerbovenhoek en de rechterbenedenhoek van een interessegebied aan. Geef waarden op in het bereik 0,0 t/m 1,0. De waarden zijn een percentage van de totale breedte of hoogte. In het bovenstaande voorbeeld wordt bijvoorbeeld de rechterhelft van de afbeelding gemarkeerd als het interessegebied. Neem dit op als u de inzichtaanvraag wilt beperken tot het interessegebied.

Het `filters`-object bevat een sitefilter (zie het veld `site`) dat u kunt gebruiken om de resultaten met vergelijkbare afbeeldingen en vergelijkbare producten te beperken tot een specifiek domein. Als de afbeelding bijvoorbeeld van een Surface Book is, kunt u `site` instellen op www.microsoft.com. 

Als u inzichten wilt krijgen over een lokale kopie van een afbeelding, uploadt u de afbeelding als binaire gegevens.

Zie [Typen inhoudformulieren](#content-form-types) voor informatie over het opnemen van deze opties in de hoofdtekst van de POST.


### <a name="endpoint"></a>Eindpunt

Het eindpunt van Visual Search is: https:\/\/api.cognitive.microsoft.com/bing/v7.0/images/visualsearch.

Aanvragen mogen alleen worden verzonden als HTTP POST-aanvragen. 


### <a name="query-parameters"></a>Queryparameters

Hier volgen de queryparameters die in uw aanvraag moeten worden opgegeven. U moet minimaal de queryparameter `mkt` opnemen.

|Naam|Waarde|Type|Vereist|  
|----------|-----------|----------|--------------|  
|<a name="cc" />cc|Een landcode van 2 tekens van het land waar de resultaten vandaan komen.<br /><br /> Als u deze parameter instelt, moet u ook de [Accept-Language](#acceptlanguage)-header opgeven. Bing gebruikt de eerste ondersteunde taal die wordt gevonden in de lijst met talen en combineert de taal met de landcode die u opgeeft om de markt te bepalen waaruit de resultaten moeten worden geretourneerd. Als de talenlijst geen ondersteunde taal bevat, vindt Bing de dichtstbijzijnde taal en markt die de aanvraag ondersteunen. Of het kan een geaggregeerde of standaardmarkt voor de resultaten gebruiken in plaats van degene die is opgegeven.<br /><br /> Gebruik deze queryparameter en de parameter `Accept-Language` alleen als u meerdere talen opgeeft; anders moet u de queryparameters `mkt` en `setLang` gebruiken.<br /><br /> Deze parameter en de parameter [mkt](#mkt) sluiten elkaar uit&mdash;geef ze niet beide op.|Tekenreeks|Nee|  
|<a name="mkt" />mkt|De markt waaruit de resultaten afkomstig zijn. <br /><br /> **Opmerking:** u wordt aangeraden de markt altijd op te geven, wanneer deze bekend is. Het specificeren van de markt helpt Bing de aanvraag te routeren en een passend en optimaal antwoord te geven.<br /><br /> Deze parameter en de parameter [cc](#cc) sluiten elkaar uit&mdash;geef ze niet beide op.|Tekenreeks|Ja|  
|<a name="safesearch" />safeSearch|Een filter dat wordt gebruikt voor het filteren van inhoud voor volwassenen. Hier volgen de mogelijke niet-hoofdlettergevoelige filterwaarden.<br /><ul><li>Uit&mdash;Retourneer webpagina's met tekst of afbeeldingen voor volwassenen.<br /><br/></li><li>Gemiddeld&mdash;Retourneer webpagina's met tekst voor volwassenen, maar geen afbeeldingen voor volwassenen.<br /><br/></li><li>Strikt&mdash;Retourneer geen webpagina's met tekst of afbeeldingen voor volwassenen.</li></ul><br /> De standaardwaarde is Moderate.<br /><br /> **OPMERKING:** Als de aanvraag afkomstig is van een markt waar Bing's beleid voor volwassenen vereist dat `safeSearch` op Strikt wordt ingesteld, negeert Bing de waarde van `safeSearch` en gebruikt Strikt.<br/><br/>**OPMERKING:** Als u de zoekoperator `site:` gebruikt, is het mogelijk dat het antwoord inhoud voor volwassenen bevat, ongeacht de instelling van de queryparameter `safeSearch`. Gebruik `site:` alleen als u zich bewust bent van de inhoud op de site en uw scenario de mogelijkheid van inhoud voor volwassenen ondersteunt. |Tekenreeks|Nee|  
|<a name="setlang" />setLang|De taal die moet worden gebruikt voor gebruikersinterfacetekenreeksen. Geef de taal op met behulp van de tweeletterige ISO 639-1 taalcode. De taalcode voor Nederlands is bijvoorbeeld NL. De standaardwaarde is EN (Engels).<br /><br /> Hoewel dit optioneel is, moet u altijd de taal opgeven. Doorgaans stelt u `setLang` in op dezelfde taal die is opgegeven door `mkt`, tenzij de gebruiker wil dat gebruikersinterfacetekenreeksen in een andere taal worden weergegeven.<br /><br /> Deze parameter en de header [Accept-Language](#acceptlanguage) sluiten elkaar uit&mdash;geef ze niet beide op.<br /><br /> Een gebruikersinterfacetekenreeks is een tekenreeks die wordt gebruikt als label in een gebruikersinterface. Er zijn maar weinig gebruikersinterfacetekenreeksen in de JSON-antwoordobjecten. De opgegeven taal wordt ook toegepast op koppelingen naar Bing.com-eigenschappen in de antwoordobjecten.|Tekenreeks|Nee| 

### <a name="headers"></a>Headers

Hier volgen de headers die in uw aanvraag moeten worden opgegeven. Content-Type en Ocp-Apim-Subscription-Key zijn de enige vereiste headers, maar u moet ook User-Agent, X-MSEdge-ClientID, X-MSEdge-ClientIP en X-Search-Location opnemen.


|Header|Beschrijving|  
|------------|-----------------|  
|<a name="acceptlanguage" />Accept-Language|Optionele aanvraagheader.<br /><br /> Een door komma's gescheiden lijst met talen die moet worden gebruikt voor gebruikersinterfacetekenreeksen. De lijst is in aflopende volgorde van voorkeur. Zie [RFC2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) voor meer informatie, waaronder de verwachte indeling.<br /><br /> Deze header en de queryparameter [setLang](#setlang) sluiten elkaar uit&mdash;geef ze niet beide op.<br /><br /> Als u deze header instelt, moet u ook de queryparameter [cc](#cc) opgeven. Om de markt te bepalen waarvoor resultaten moeten worden geretourneerd, gebruikt Bing de eerste ondersteunde taal die wordt gevonden in de lijst en combineert deze met de parameterwaarde `cc`. Als de lijst geen ondersteunde taal bevat, vindt Bing de dichtstbijzijnde taal en markt die de aanvraag ondersteunen, of gebruikt een geaggregeerde of standaardmarkt voor de resultaten. Zie de header BingAPIs-Market om de markt te bepalen die Bing heeft gebruikt.<br /><br /> Gebruik deze header en de queryparameter `cc` alleen als u meerdere talen opgeeft. Gebruik anders de queryparameters [mkt](#mkt) en [setLang](#setlang).<br /><br /> Een gebruikersinterfacetekenreeks is een tekenreeks die wordt gebruikt als label in een gebruikersinterface. Er zijn maar weinig gebruikersinterfacetekenreeksen in de JSON-antwoordobjecten. De opgegeven taal wordt toegepast op koppelingen naar Bing.com-eigenschappen in de antwoordobjecten.|  
|<a name="contenttype" />Content-Type|Vereiste aanvraagheader.<br /><br />Moet worden ingesteld op multipart/form-data en een grensparameter bevatten (bijvoorbeeld multipart/form-data; boundary=\<boundary string\>). Zie [typen inhoudformulieren](#content-form-types) voor meer informatie.
|<a name="market" />BingAPIs-Market|Antwoordheader.<br /><br /> De markt die wordt gebruikt door de aanvraag. De notatie is \<languageCode\>-\<countryCode\>. Bijvoorbeeld: nl-NL.|  
|<a name="traceid" />BingAPIs-TraceId|Antwoordheader.<br /><br /> De id van de logboekvermelding die de details van de aanvraag bevat. Registreer deze id wanneer er een fout optreedt. Als u het probleem niet kunt vaststellen en oplossen, neemt u deze id op bij de andere informatie die u aan het ondersteuningsteam verstrekt.|  
|<a name="subscriptionkey" />Ocp-Apim-Subscription-Key|Vereiste aanvraagheader.<br /><br /> De abonnementssleutel die u hebt ontvangen toen u zich hebt geregistreerd voor deze service in [Cognitive Services](https://www.microsoft.com/cognitive-services/).|  
|<a name="pragma" />Pragma|Optionele aanvraagheader<br /><br /> Bing retourneert standaard cache-inhoud, indien beschikbaar. Om te voorkomen dat Bing inhoud uit de cache retourneert, stelt u de Pragma-header in op no-cache (bijvoorbeeld: Pragma: no-cache).
|<a name="useragent" />User-Agent|Optionele aanvraagheader.<br /><br /> De gebruikersagent waarvan de aanvraag afkomstig is. Bing gebruikt de user-agent om mobiele gebruikers een geoptimaliseerde ervaring te bieden. Hoewel dit optioneel is, wordt u aangeraden deze header altijd op te geven.<br /><br /> De user-agent moet dezelfde tekenreeks zijn die elke veelgebruikte browser verzendt. Zie [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) voor meer informatie over gebruikersagenten.<br /><br /> Hier volgen enkele voorbeelden van user-agent-tekenreeksen.<br /><ul><li>Windows Phone&mdash;Mozilla/5.0 (compatibel; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)<br /><br /></li><li>Android&mdash;Mozilla/5.0 (Linux; U; Android 2.3.5; nl-nl; SCH-I500 Build/GINGERBREAD) AppleWebKit/533.1 (KHTML; like Gecko) Version/4.0 Mobile Safari/533.1<br /><br /></li><li>iPhone&mdash;Mozilla/5.0 (iPhone; CPU iPhone OS 6_1 like Mac OS X) AppleWebKit/536.26 (KHTML; zoals Gecko) Mobile/10B142 iPhone4;1 BingWeb/3.03.1428.20120423<br /><br /></li><li>PC&mdash;Mozilla/5.0 (Windows NT 6.3; WOW64; Trident/7.0; Touch; rv:11.0) zoals Gecko<br /><br /></li><li>iPad&mdash;Mozilla/5.0 (iPad; CPU OS 7_0 zoals Mac OS X) AppleWebKit/537.51.1 (KHTML, zoals Gecko) Version/7.0 Mobile/11A465 Safari/9537.53</li></ul>|
|<a name="clientid" />X-MSEdge-ClientID|Optionele aanvraag- en antwoord-header.<br /><br /> Bing gebruikt deze header om gebruikers consistent gedrag te bieden bij Bing API-aanroepen. Bing introduceert nieuwe functies en verbeteringen vaak in flights, en gebruikt de client-ID als sleutel voor het toewijzen van verkeer aan verschillende flights. Als u niet dezelfde client-id gebruikt voor een gebruiker voor meerdere aanvragen, kan Bing de gebruiker toewijzen aan meerdere conflicterende flights. Toewijzing aan meerdere conflicterende flights kan leiden tot een inconsistente gebruikerservaring. Als de tweede aanvraag bijvoorbeeld een andere flighttoewijzing heeft dan de eerste, kan de ervaring onverwacht zijn. Bing kan de client-id ook gebruiken om webresultaten aan te passen aan de zoekgeschiedenis van die client-id, waardoor de gebruiker een rijkere ervaring krijgt.<br /><br /> Bing gebruikt deze header ook om de rangschikking van resultaten te verbeteren door de activiteit te analyseren die wordt gegenereerd door een client-id. De relevantie-verbeteringen helpen de kwaliteit van de resultaten die door Bing-API's worden geleverd te verbeteren, en maken op hun beurt hogere doorklikpercentages mogelijk voor de API-consument.<br /><br /> **BELANGRIJK:** Hoewel deze header optioneel is, dient u deze als verplicht te beschouwen. Door de client-id te behouden voor meerdere aanvragen voor dezelfde combinatie van eindgebruiker en apparaat, zorgt u voor 1) een consistente gebruikerservaring van de API-consument, en 2) hogere doorklikpercentages via een betere kwaliteit van de resultaten van de Bing-API's.<br /><br /> Hieronder volgen de basisgebruiksregels die van toepassing zijn op deze header.<br /><ul><li>Elke gebruiker die uw toepassing op het apparaat gebruikt, moet een unieke door Bing gegenereerde client-id hebben.<br /><br/>Als u deze header niet in de aanvraag opneemt, genereert Bing een id en retourneert deze in de X-MSEdge-ClientID-antwoordheader. De enige keer dat u deze header NIET in een aanvraag moet opnemen, is de eerste keer dat de gebruiker uw app op dat apparaat gebruikt.<br /><br/></li><li>**LET OP:** U moet ervoor zorgen dat deze client-id niet kan worden gekoppeld aan geverifieerde gebruikersaccountgegevens.</li><li>Gebruik de client-id voor elke Bing-API-aanvraag die uw app voor deze gebruiker op het apparaat doet.<br /><br/></li><li>Maak de client-id persistent. Gebruik in een browser-app een persistent HTTP-cookie om ervoor te zorgen dat de id in alle sessies wordt gebruikt. Gebruik geen sessiecookie. Voor andere apps, zoals mobiele apps, gebruikt u de persistente opslag van het apparaat om de id persistent te maken.<br /><br/>De volgende keer dat de gebruiker uw app op dat apparaat gebruikt, haalt u de client-id op die u persistent hebt gemaakt.</li></ul><br /> **OPMERKING:** Bing-antwoorden kunnen deze header al dan niet bevatten. Als het antwoord deze header bevat, registreert u de client-id en gebruikt u deze voor alle volgende Bing-aanvragen voor de gebruiker op dat apparaat.<br /><br /> **OPMERKING:** als u de X-MSEdge-ClientID opneemt, moet u geen cookies opnemen in de aanvraag.|  
|<a name="clientip" />X-MSEdge-ClientIP|Optionele aanvraagheader.<br /><br /> Het IPv4- of IPv6-adres van het clientapparaat. Het IP-adres wordt gebruikt voor het detecteren van de locatie van de gebruiker. Bing gebruikt de locatie-informatie om het gedrag van Veilig Zoeken te bepalen.<br /><br /> **OPMERKING:** Hoewel dit optioneel is, wordt u aangeraden deze header en de X-Search-Location-header altijd op te geven.<br /><br /> Verdoezel het adres niet (bijvoorbeeld door het laatste octet te wijzigen in 0). Wanneer u het adres verdoezelt, is de locatie totaal niet in de buurt van de werkelijke locatie van het apparaat, wat ertoe kan leiden dat Bing onjuiste resultaten geeft.|  
|<a name="location" />X-Search-Location|Optionele aanvraagheader.<br /><br /> Een met puntkomma's gescheiden lijst met sleutel-waardeparen die de geografische locatie van de client beschrijven. Bing gebruikt de locatie-informatie om het gedrag van Veilig Zoeken te bepalen en relevante lokale inhoud te retourneren. Geef het sleutel-waardepaar op als \<sleutel\>:\<waarde\>. Hier volgen de sleutels die u gebruikt om de locatie van de gebruiker op te geven.<br /><br /><ul><li>lat&mdash;Vereist. De breedtegraad van de locatie van de client, in graden. De breedtegraad moet groter dan of gelijk zijn aan -90.0 en kleiner dan of gelijk aan +90.0. Negatieve waarden geven zuidelijke breedtegraden aan, en positieve waarden noordelijke.<br /><br /></li><li>long&mdash;Vereist. De lengtegraad van de locatie van de client, in graden. De lengtegraad moet groter dan of gelijk zijn aan -180.0 en kleiner dan of gelijk aan +180.0. Negatieve waarden geven westelijke lengtegraden aan, en positieve waarden oostelijke.<br /><br /></li><li>re&mdash;Vereist. De straal, in meters, die de horizontale nauwkeurigheid van de coördinaten aangeeft. Geef de waarde door die wordt geretourneerd door de locatieservice van het apparaat. Typische waarden zijn bijvoorbeeld 22 m voor GPS/Wi-Fi, 380 m voor triangulatie op basis van gsm-masten en 18.000 m voor reverse IP-lookup.<br /><br /></li><li>ts&mdash;Optioneel. Het UTC-UNIX-tijdstempel van wanneer de client op de locatie was. (Het UNIX-tijdstempel is het aantal seconden sinds 1 januari 1970.)<br /><br /></li><li>head&mdash;Optioneel. De relatieve koers of reisrichting van de client. Geef de reisrichting op als graden van 0 t/m 360, gerekend met de klok mee ten opzichte van het ware noorden. Geef deze sleutel alleen op als de `sp`-sleutel niet nul is.<br /><br /></li><li>sp&mdash;Optioneel. De horizontale snelheid, in meters per seconde, waarmee het clientapparaat reist.<br /><br /></li><li>alt&mdash;Optioneel. De hoogte van het clientapparaat, in meters.<br /><br /></li><li>are&mdash;Optioneel. De straal, in meters, die de verticale nauwkeurigheid van de coördinaten aangeeft. Geef deze sleutel alleen op als u de `alt`-sleutel opgeeft.<br /><br /></li></ul> **OPMERKING:** Hoewel veel van de sleutels optioneel zijn, geldt dat hoe meer informatie u verstrekt, hoe nauwkeuriger de locatieresultaten zijn.<br /><br /> **OPMERKING:** Hoewel dit optioneel is, wordt u aangeraden de geografische locatie van de gebruiker altijd op te geven. Het opgeven van de locatie is vooral belangrijk als het IP-adres van de client de fysieke locatie van de gebruiker niet nauwkeurig weergeeft (bijvoorbeeld als de client VPN gebruikt). Voor optimale resultaten moet u zowel deze header als de X-MSEdge-ClientIP-header opnemen, maar neem minimaal deze header op.|

> [!NOTE] 
> Vergeet niet dat de gebruiksvoorwaarden naleving van alle toepasselijke wetgeving vereisen, inclusief die over het gebruik van deze headers. In bepaalde rechtsgebieden, zoals Europa, is het bijvoorbeeld vereist om toestemming van de gebruiker te verkrijgen voordat bepaalde traceringsapparaten op gebruikersapparaten wordt geplaatst.


<a name="content-form-types" />

### <a name="content-form-types"></a>Typen formulierinhoud

Elke aanvraag moet de Content-Type-header bevatten. De header moet worden ingesteld op: multipart/form-data; boundary=\<grenstekenreeks\>, waarbij \<grenstekenreeks\> een unieke, ondoorzichtige tekenreeks is die de grens van de formuliergegevens aangeeft. Bijvoorbeeld: boundary=boundary_1234-abcd.


Als u een afbeeldingstoken of URL naar Visual Search verzendt, worden hieronder de formuliergegevens weergegeven die u in de hoofdtekst van de POST moet opnemen. De formuliergegevens moeten de header Content-Disposition bevatten en de parameter `name` daarvan moet zijn ingesteld op 'knowledgeRequest'. Zie [De aanvraag](#the-request) voor informatie over het `imageInfo`-object.


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

Als u een lokale afbeelding uploadt, toont het volgende de formuliergegevens die u in de POST moet opnemen. De formuliergegevens moeten de header Content-Disposition bevatten. De parameter `name` moet worden ingesteld op "image" en de parameter `filename` kan op een willekeurige tekenreeks worden ingesteld. De Content-Type-header kan worden ingesteld op elk algemeen gebruikt MIME-type voor afbeeldingen. De inhoud van het formulier is het binaire bestand van de afbeelding. De maximale afbeeldingsgrootte die u kunt uploaden is 1 MB. De grootste zijde van de afbeelding (breedte of hoogte) moet 1500 pixels of minder zijn.


```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
Content-Type: image/jpeg

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Hieronder ziet u hoe u het interessegebied van geüploade afbeeldingen kunt opgeven.

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



### <a name="example-request"></a>Voorbeeldaanvraag

Het volgende toont een volledige afbeeldingsinzichtaanvraag die een afbeeldingstoken en interessegebied doorgeeft. U haalt het inzichttoken op uit een eerdere aanroep van /images search.


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

Als er Inzichten beschikbaar zijn voor de afbeelding, bevat het antwoord een of meer `tags` die de Inzichten bevatten. Het veld `image` bevat het inzichttoken voor de invoerafbeelding.

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

Het veld `tags` bevat een weergavenaam en een lijst met acties (inzichten). Een van de tags bevat een `displayName`-veld dat is ingesteld op een lege tekenreeks. Deze tag bevat de standaardinzichten, zoals webpagina's met de afbeelding, visueel vergelijkbare afbeeldingen en winkelbronnen voor in de afbeelding gevonden artikelen. Omdat de volledige afbeelding van belang is, bevat de standaardinzichttag geen begrenzingsvakken voor interessegebieden.


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

Zie [Standaardinzichten](./default-insights-tag.md) voor een lijst met alle standaardinzichten.



De overige tags bevatten andere inzichten die van belang kunnen zijn voor de gebruiker. Als de afbeelding bijvoorbeeld tekst bevat, kan een van de tags het inzicht TextResults bevatten, dat de herkende tekst bevat. En als Bing een entiteit (persoon, plaats of ding) in de afbeelding herkent, kan een van de tags de entiteit identificeren. Visual Search retourneert ook een diverse reeks termen (tags) die zijn afgeleid van de ingevoerde afbeelding. Aan de hand van deze tags kunnen gebruikers in de afbeelding gevonden concepten verkennen. Als de invoerafbeelding bijvoorbeeld een beroemde atleet bevat, kan een van de tags Sport zijn, die koppelingen naar afbeeldingen van sport bevat.

Elke tag bevat een weergavenaam die u kunt gebruiken om het inzicht te categoriseren, heen begrenzingsvak dat het interessegebied identificeert waarop het inzicht van toepassing is, de inzichten zelf, en een miniatuur van de afbeelding. Als de afbeelding bijvoorbeeld van een persoon is die een sporttrui draagt, kan een van de tags een begrenzingsvak bevatten dat de trui begrenst en VisualSearch- en ProductVisualSearch-inzichten bevatten. En een andere tag kan een ImageResults-inzicht bevatten met een URL voor een /images/search-API-aanvraag om afbeeldingen te krijgen die gerelateerd zijn aan het onderwerp of een zoek-URL van Bing.com die de gebruiker naar de Bing.com-zoekresultaten voor afbeeldingen brengt.

Alle andere tags dan de standaardinzichttag bevatten begrenzingsvakken die interessegebieden in de afbeelding identificeren. Als de afbeelding bijvoorbeeld meerdere herkende personen bevat, kunnen tags begrenzingsvakken voor elk van de mensen bevatten of als de afbeelding herkende kledingstukken bevat, kunnen tags begrenzingsvakken bevatten voor elk herkend kledingstuk. U kunt de begrenzingsvakken gebruiken om hotspots te maken op de afbeelding die, wanneer erop wordt geklikt, details bieden over de inhoud in dat deel van de afbeelding. U moet geen hotspots in een afbeelding opnemen voor begrenzingsvakken die de volledige afbeelding identificeren.

### <a name="text-recognition"></a>Tekstherkenning

Als de afbeelding tekst bevat die door de service wordt herkend, bevat een van de tags het inzicht TextResults (actie). De `displayName` van het inzicht bevat de herkende tekst. 

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

Omdat het veld `displayName` van de tag ##TextRecognition bevat, moet u dit niet gebruiken als categorietitel in de UX. Dat geldt voor elke weergavenaam die begint met ##. Gebruik in plaats daarvan de weergavenaam van de actie.


Tekstherkenning kan ook de contactgegevens op visitekaartjes herkennen, zoals telefoonnummers en e-mailadressen. Het selectiekader geeft de locatie van de contactgegevens op het kaartje aan. 

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

Als de afbeelding een herkende entiteit bevat zoals een persoon, plaats of ding, kan een van de tags een entiteitsinzicht bevatten. 

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

Zie de snelstarts om snel aan de slag te gaan met uw eerste aanvraag: [C#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [node.js](quickstarts/nodejs.md) | [Python](quickstarts/python.md).

Probeer de API uit. Ga naar [Visual Search API Testing Console](https://dev.cognitive.microsoft.com/docs/services/878c38e705b84442845e22c7bff8c9ac). 


Maak uzelf vertrouwd met de [Visual Search API Reference](https://aka.ms/bingvisualsearchreferencedoc). De handleiding bevat de lijst met eindpunten, headers en queryparameters die u nodig hebt om zoekresultaten op te vragen. Daarnaast vindt u hier definities van de responsobjecten. 

Lees [Gebruiks- en weergavevereisten voor Bing](./use-and-display-requirements.md) om er zeker van te zijn dat u alle regels voor het gebruik van de zoekresultaten volgt.


