---
title: Translator Text-API v2.0
titleSuffix: Azure Cognitive Services
description: Referentiedocumentatie voor de Translator Text-API-versie 2.0.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 05/15/2018
ms.author: swmachan
ms.openlocfilehash: a29e123c44ca198ce19db451ee4c624b6f993538
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705432"
---
# <a name="translator-text-api-v20"></a>Translator Text-API v2.0

> [!IMPORTANT]
> Deze versie van de Translator Text-API is afgeschaft. [Documentatie voor versie 3 van de Translator Text-API bekijken](v3-0-reference.md).

Versie 2 van de Translator Text-API kan naadloos worden geïntegreerd in uw apps, websites, hulpprogramma's of andere oplossingen wil bieden voor meerdere talen gebruikerservaringen. U kunt deze op elk hardwareplatform en met elk besturingssysteem om uit te voeren vertaalservices en andere taken met betrekking tot taal, zoals tekst taaldetectie en tekst naar spraak, op basis van industrienormen. Zie voor meer informatie, [Translator Text-API](../translator-info-overview.md).

## <a name="getting-started"></a>Aan de slag
Voor toegang tot de Translator Text-API, moet u [zich registreren voor Microsoft Azure](../translator-text-how-to-signup.md).

## <a name="authentication"></a>Verificatie 
Aanroepen naar de Translator Text-API moeten een abonnementssleutel voor verificatie. De API ondersteunt drie methoden voor verificatie:

- Een toegangstoken. Gebruik de abonnementssleutel waarnaar wordt verwezen in stap 9 een toegangstoken maken door een POST-aanvraag naar de service voor verificatie. Zie de documentatie van de service voor beveiligingstokens voor meer informatie. Het toegangstoken doorgeven aan de Translator-service met behulp van de `Authorization` header of de `access_token` queryparameter. Het toegangstoken is geldig voor 10 minuten. Een nieuw toegangstoken verkrijgen om de 10 minuten, en met behulp van dezelfde toegang token voor herhaalde aanvragen tijdens de 10 minuten.
- Een abonnementssleutel die rechtstreeks worden gebruikt. Uw abonnementssleutel doorgegeven als een waarde in de `Ocp-Apim-Subscription-Key` header inbegrepen bij uw aanvraag voor de Translator Text-API. Wanneer u de abonnementssleutel rechtstreeks gebruikt, hebt u geen om aan te roepen van het token authentication-service voor het maken van een toegangstoken.
- Een [meerdere services abonnement van Azure Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/). Deze methode kunt u één geheime sleutel gebruiken om aanvragen voor meerdere services te verifiëren.
Wanneer u een geheime sleutel van meerdere services gebruikt, moet u twee verificatieheaders met uw aanvraag opnemen. De eerste header geeft de geheime sleutel. De tweede-header geeft de regio die is gekoppeld aan uw abonnement:
   - `Ocp-Apim-Subscription-Key`
   - `Ocp-Apim-Subscription-Region`

De regio is vereist voor het abonnement met meerdere services Text-API. De regio die u selecteert, is de enige regio die u voor tekstvertaling gebruiken kunt wanneer u de abonnementssleutel voor meerdere services gebruikt. Het moet dezelfde regio die u hebt geselecteerd toen u zich hebt geregistreerd voor uw abonnement op meerdere services in Azure portal.

De beschikbare regio's zijn `australiaeast`, `brazilsouth`, `canadacentral`, `centralindia`, `centraluseuap`, `eastasia`, `eastus`, `eastus2`, `japaneast`, `northeurope`, `southcentralus`, `southeastasia`, `uksouth`, `westcentralus`, `westeurope`, `westus`, en `westus2`.

Uw abonnementssleutel en access token zijn geheimen die moeten worden verborgen.

## <a name="profanity-handling"></a>Grof taalgebruik verwerken
De Translator-service wordt normaal gesproken grof taalgebruik dat aanwezig is in de bron behouden. De mate van grof taalgebruik en de context waarmee woorden grof verschillen op basis van cultuur. De mate van grof taalgebruik in de doel-taal kan zodanig worden vergroot of verkleind.

Als u wilt om te voorkomen dat grof taalgebruik in de vertaling, zelfs wanneer deze zich in de brontekst, kunt u de optie voor de methoden die ondersteuning bieden voor het filteren taalgebruik. De optie kunt u kiezen of u wilt zien van grof taalgebruik verwijderd of gemarkeerd met de juiste tags of of u wilt toestaan dat de grof taalgebruik in het doel. De geaccepteerde waarden van `ProfanityAction` zijn `NoAction` (standaard), `Marked`, en `Deleted`.


|ProfanityAction    |Bewerking |Voorbeeld van de bron (Japans)  |Voorbeeld van de vertaling (Engels)  |
|:--|:--|:--|:--|
|NoAction   |Standaard. Hetzelfde als de instelling van de optie. Grof taalgebruik geeft van bron naar doel.        |彼はジャッカスです。     |Hij is een jackass.   |
|Gemarkeerd     |Grof woorden omringd door de XML-tags \<grof taalgebruik > en \</profanity >.       |彼はジャッカスです。 |Hij is een \<grof taalgebruik > jackass\</profanity >.  |
|Verwijderen    |Grof woorden worden verwijderd uit de uitvoer zonder vervanging.     |彼はジャッカスです。 |Hij is een.   |

    
## <a name="excluding-content-from-translation"></a>Met uitzondering van de inhoud van de vertaling
Wanneer u inhoud met zoals HTML-tags vertalen (`contentType=text/html`), is het soms nuttig voor het uitsluiten van specifieke inhoud van de vertaling. U kunt het kenmerk `class=notranslate` om op te geven van inhoud die moet blijven in de oorspronkelijke taal. In het volgende voorbeeld wordt de inhoud in de eerste `div` element wordt niet worden vertaald, maar de inhoud in de tweede `div` element wordt omgezet.

```HTML
<div class="notranslate">This will not be translated.</div>
<div>This will be translated. </div>
```

## <a name="get-translate"></a>OPHALEN / vertalen

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Zet een tekenreeks met tekst uit één taal naar een andere.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/Translate`.

**Geretourneerde waarde:** Een tekenreeks met de vertaalde tekst.

Als u eerder hebt gebruikt `AddTranslation` of `AddTranslationArray` in te voeren een vertaling met een classificatie van 5 of hoger voor de zin dezelfde bron `Translate` retourneert alleen de eerste keuze die beschikbaar is voor uw systeem. "Dezelfde bron zin" betekent exact de dezelfde (100%-koppeling), met uitzondering van hoofdletters, witruimte tagwaarden en leestekens aan het einde van een zin. Als er geen beoordeling is opgeslagen met een classificatie van 5 of hoger, is de geretourneerde resultaten voor de automatische vertaling met Microsoft Translator.

### <a name="response-class-status-200"></a>Antwoord-klasse (status 200)

string

Antwoord-inhoudstype: application/xml

### <a name="parameters"></a>Parameters

|Parameter|Value|Description    |Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|appid  |(leeg)    |Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat u de `appid` veld leeg. Anders, omvat een tekenreeks met `"Bearer" + " " + "access_token"`.|query|string|
|text|(leeg)   |Vereist. Een tekenreeks met de tekst te vertalen. De tekst kan niet meer dan 10.000 tekens bevatten.|query|string|
|from|(leeg)   |Optioneel. Een tekenreeks met de taalcode van de tekst wordt vertaald. Bijvoorbeeld, en voor Engels.|query|string|
|tot|(leeg) |Vereist. Een tekenreeks met de code van de taal voor de omzetting van de tekst in.|query|string|
|contentType|(leeg)    |Optioneel. De indeling van de tekst wordt vertaald. Ondersteunde indelingen zijn `text/plain` (standaard) en `text/html`. Een HTML-elementen moeten juist opgemaakt, volledige elementen.|query|string|
|category|(leeg)   |Optioneel. Een tekenreeks zijn met de categorie (domein) van de vertaling. De standaardwaarde is `general`.|query|string|
|Autorisatie|(leeg)  |Vereist als zowel de `appid` veld en de `Ocp-Apim-Subscription-Key` header zijn leeg. Autorisatietoken: `"Bearer" + " " + "access_token"`.|koptekst|string|
|OCP-Apim-Subscription-Key|(leeg)  |Vereist als zowel de `appid` veld en de `Authorization` header zijn leeg.|koptekst|string|


### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reason|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties.|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="post-translatearray"></a>POST /TranslateArray

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Hiermee haalt u vertalingen voor meerdere bron teksten.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/TranslateArray`.

Hier volgt de indeling van de hoofdtekst van de aanvraag:

```
<TranslateArrayRequest>
  <AppId />
  <From>language-code</From>
  <Options>
    <Category xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >string-value</Category>
    <ContentType xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">text/plain</ContentType>
    <ReservedFlags xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" />
    <State xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >int-value</State>
    <Uri xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >string-value</Uri>
    <User xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >string-value</User>
  </Options>
  <Texts>
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">string-value</string>
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">string-value</string>
  </Texts>
  <To>language-code</To>
</TranslateArrayRequest>
```

Deze elementen zijn `TranslateArrayRequest`:


* `AppId`: Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat u de `AppId` veld leeg. Anders, omvat een tekenreeks met `"Bearer" + " " + "access_token"`.
* `From`: Optioneel. Een tekenreeks met de taalcode van de tekst wordt vertaald. Als dit veld leeg wordt gelaten, wordt het antwoord het resultaat van de automatische taaldetectie bevatten.
* `Options`: Optioneel. Een `Options` -object dat de volgende waarden bevat. Ze zijn optioneel en standaard naar de meest voorkomende instellingen. Opgegeven elementen moeten worden vermeld in alfabetische volgorde.
    - `Category`: Een tekenreeks zijn met de categorie (domein) van de vertaling. De standaardwaarde is `general`.
    - `ContentType`: De indeling van de tekst wordt vertaald. De ondersteunde indelingen zijn `text/plain` (standaard), `text/xml`, en `text/html`. Een HTML-elementen moeten juist opgemaakt, volledige elementen.
    - `ProfanityAction`: Hiermee geeft u op hoe profanities worden verwerkt, zoals eerder beschreven. Geaccepteerde waarden zijn `NoAction` (standaard), `Marked`, en `Deleted`.
    - `State`: De status van de gebruiker om u te helpen bij de aanvraag en respons correleren. Dezelfde inhoud in het antwoord geretourneerd.
    - `Uri`: Filter de resultaten op basis van deze URI. Standaard: `all`.
    - `User`: Filter de resultaten door deze gebruiker. Standaard: `all`.
* `Texts`: Vereist. Een matrix met de tekst voor de vertaling. Alle tekenreeksen moeten zich in dezelfde taal. Het totaal van alle tekst die moet worden vertaald, kan niet groter zijn dan 10.000 tekens. Het maximum aantal matrixelementen is 2000.
* `To`: Vereist. Een tekenreeks met de code van de taal voor de omzetting van de tekst in.

U kunt optionele elementen weglaten. Elementen die directe onderliggende items van `TranslateArrayRequest` moet worden vermeld in alfabetische volgorde.

De `TranslateArray` methode accepteert `application/xml` of `text/xml` voor `Content-Type`.

**Geretourneerde waarde:** Een `TranslateArrayResponse` matrix. Elke `TranslateArrayResponse` deze elementen heeft:

* `Error`: Geeft een fout als een zich voordoet. Anders is ingesteld op null.
* `OriginalSentenceLengths`: Een matrix met gehele getallen die de lengte van elke zin in de brontekst geeft. De lengte van de matrix geeft het aantal zinnen.
* `TranslatedText`: De vertaalde tekst.
* `TranslatedSentenceLengths`: Een matrix met gehele getallen die de lengte van elke zin in de vertaalde tekst geeft. De lengte van de matrix geeft het aantal zinnen.
* `State`: De status van de gebruiker om u te helpen bij de aanvraag en respons correleren. Retourneert de dezelfde inhoud als de aanvraag.

Hier volgt de indeling van de antwoordtekst:

```
<ArrayOfTranslateArrayResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"
  xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <TranslateArrayResponse>
    <From>language-code</From>
    <OriginalTextSentenceLengths xmlns:a="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
      <a:int>int-value</a:int>
    </OriginalTextSentenceLengths>
    <State/>
    <TranslatedText>string-value</TranslatedText>
    <TranslatedTextSentenceLengths xmlns:a="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
      <a:int>int-value</a:int>
    </TranslatedTextSentenceLengths>
  </TranslateArrayResponse>
</ArrayOfTranslateArrayResponse>
```

### <a name="response-class-status-200"></a>Antwoord-klasse (status 200)
Een geslaagde reactie bevat een matrix van `TranslateArrayResponse` matrices in de indeling die eerder zijn beschreven.

string

Antwoord-inhoudstype: application/xml

### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|Autorisatie|(leeg)  |Vereist als zowel de `appid` veld en de `Ocp-Apim-Subscription-Key` header zijn leeg. Autorisatietoken: `"Bearer" + " " + "access_token"`.|koptekst|string|
|OCP-Apim-Subscription-Key|(leeg)|Vereist als zowel de `appid` veld en de `Authorization` header zijn leeg.|koptekst|string|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode   |Reason|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht. Veelvoorkomende fouten zijn onder andere: <ul><li>Matrixelement mag niet leeg zijn.</li><li>Ongeldige categorie.</li><li>Van een taal is ongeldig.</li><li>Taal is ongeldig.</li><li>De aanvraag bevat te veel elementen.</li><li>De taal van wordt niet ondersteund.</li><li>De taal aan wordt niet ondersteund.</li><li>Vertalen aanvraag heeft te veel gegevens.</li><li>HTML-code heeft niet de juiste indeling.</li><li>Te veel tekenreeksen zijn doorgegeven in de vertalen aanvragen.</li></ul>|
|401    |Ongeldige referenties.|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="post-getlanguagenames"></a>POST /GetLanguageNames

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Beschrijvende namen voor de talen die worden opgehaald als de parameter doorgegeven `languageCodes`, gelokaliseerde in de doorgegeven `locale` taal.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/GetLanguageNames`.

De aanvraagtekst bevat een string-matrix die de ISO 639-1 taalcodes waarvoor u vertegenwoordigt wilt ophalen van de beschrijvende namen. Hier volgt een voorbeeld:

```
<ArrayOfstring xmlns:i="https://www.w3.org/2001/XMLSchema-instance"  xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>zh</string>
    <string>en</string>
</ArrayOfstring>
```

**Geretourneerde waarde:** Een string-matrix met taalnamen ondersteund door de Translator-service, in de gevraagde taal is gelokaliseerd.

### <a name="response-class-status-200"></a>Antwoord-klasse (status 200)
Een string-matrix die namen van talen ondersteund door de Translator-service, gelocaliseerd in de gevraagde taal bevat.

string

Antwoord-inhoudstype: application/xml
 
### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|appid|(leeg)|Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat u de `appid` veld leeg. Anders, omvat een tekenreeks met `"Bearer" + " " + "access_token"`.|query|string|
|Landinstelling|(leeg) |Vereist. Een tekenreeks met een van de volgende gebruikt voor het lokaliseren van de taalnamen van de: <ul><li>De combinatie van een ISO 639 kleine cultuur van twee letters die zijn gekoppeld aan een taal en een ISO-norm 3166 twee letters, hoofdletters subcultuur code. <li>Een kleine cultuur ISO 639-code op zichzelf.|query|string|
|Autorisatie|(leeg)  |Vereist als zowel de `appid` veld en de `Ocp-Apim-Subscription-Key` header zijn leeg. Autorisatietoken: `"Bearer" + " " + "access_token"`.|koptekst|string|
|OCP-Apim-Subscription-Key|(leeg)  |Vereist als zowel de `appid` veld en de `Authorization` header zijn leeg.|koptekst|string|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reason|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties.|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="get-getlanguagesfortranslate"></a>GET /GetLanguagesForTranslate

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Hiermee haalt u een lijst met taalcodes die staan voor talen die worden ondersteund door de vertaling-service.  `Translate` en `TranslateArray` kunt vertalen tussen twee van deze talen.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForTranslate`.

**Geretourneerde waarde:** Een string-matrix met de taalcodes worden ondersteund door de Translator-service.

### <a name="response-class-status-200"></a>Antwoord-klasse (status 200)
Een string-matrix met de taalcodes worden ondersteund door de Translator-service.

string

Antwoord-inhoudstype: application/xml
 
### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|appid|(leeg)|Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat u de `appid` veld leeg. Anders, omvat een tekenreeks met `"Bearer" + " " + "access_token"`.|query|string|
|Autorisatie|(leeg)  |Vereist als zowel de `appid` veld en de `Ocp-Apim-Subscription-Key` header zijn leeg. Autorisatietoken: `"Bearer" + " " + "access_token"`.|koptekst|string|
|OCP-Apim-Subscription-Key|(leeg)|Vereist als zowel de `appid` veld en de `Authorization` header zijn leeg.|koptekst|string|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reason|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties.|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503|De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="get-getlanguagesforspeak"></a>/GetLanguagesForSpeak ophalen

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Hiermee haalt u de beschikbare talen voor spraaksynthese.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForSpeak`.

**Geretourneerde waarde:** Een string-matrix met de taalcodes voor spraaksynthese wordt ondersteund door de Translator-service.

### <a name="response-class-status-200"></a>Antwoord-klasse (status 200)
Een string-matrix met de taalcodes voor spraaksynthese wordt ondersteund door de Translator-service.

string

Antwoord-inhoudstype: application/xml

### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|appid|(leeg)|Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat u de `appid` veld leeg. Anders, omvat een tekenreeks met `"Bearer" + " " + "access_token"`.|query|string|
|Autorisatie|(leeg)|Vereist als zowel de `appid` veld en de `Ocp-Apim-Subscription-Key` header zijn leeg. Autorisatietoken: `"Bearer" + " " + "access_token"`.|koptekst|string|
|OCP-Apim-Subscription-Key|(leeg)|Vereist als zowel de `appid` veld en de `Authorization` header zijn leeg.|koptekst|string|
 
### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reason|
|:--|:--|
|400|Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401|Ongeldige referenties.|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="get-speak"></a>OPHALEN / spreken

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Retourneert een WAV- of MP3-stroom van de doorgegeven in tekst, die in de gewenste taal gesproken.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/Speak`.

**Geretourneerde waarde:** Een WAV- of MP3-stroom van de doorgegeven in tekst, die in de gewenste taal gesproken.

### <a name="response-class-status-200"></a>Antwoord-klasse (status 200)

binary

Antwoord-inhoudstype: application/xml

### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|appid|(leeg)|Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat u de `appid` veld leeg. Anders, omvat een tekenreeks met `"Bearer" + " " + "access_token"`.|query|string|
|text|(leeg)   |Vereist. Een tekenreeks is die een of meer zinnen worden uitgesproken voor de stream, in de opgegeven taal bevat. De tekst mag niet groter zijn dan 2000 tekens.|query|string|
|language|(leeg)   |Vereist. Een tekenreeks is die de ondersteunde taalcode van de taal waarin u kunt de tekst uitspreken vertegenwoordigt. De code moet een van de codes die wordt geretourneerd door de methode `GetLanguagesForSpeak`.|query|string|
|format|(leeg)|Optioneel. Een tekenreeks waarmee de inhoud van het type-ID. Op dit moment `audio/wav` en `audio/mp3` beschikbaar zijn. De standaardwaarde is `audio/wav`.|query|string|
|opties|(leeg)    |Optioneel. Een tekenreeks waarmee de eigenschappen van de kunstmatige spraak:<ul><li>`MaxQuality` en `MinSize` de kwaliteit van de audiosignaal opgeven. `MaxQuality` biedt de hoogste kwaliteit. `MinSize` biedt de kleinste grootte. De standaardwaarde is `MinSize`.</li><li>`female` en `male` opgeven van de gewenste geslacht van de stem. De standaardwaarde is `female`. Gebruik de verticale balk (<code>\|</code>) naar meerdere opties bevatten. Bijvoorbeeld `MaxQuality|Male`.</li></li></ul>  |query|string|
|Autorisatie|(leeg)|Vereist als zowel de `appid` veld en de `Ocp-Apim-Subscription-Key` header zijn leeg. Autorisatietoken: `"Bearer" + " " + "access_token"`.|koptekst|string|
|OCP-Apim-Subscription-Key|(leeg)  |Vereist als zowel de `appid` veld en de `Authorization` header zijn leeg.|koptekst|string|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reason|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties.|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="get-detect"></a>OPHALEN / detecteren

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Hiermee geeft u de taal van een deel van de tekst.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/Detect`.

**Geretourneerde waarde:** Een tekenreeks is die een taalcode van twee tekens voor de tekst bevat.

### <a name="response-class-status-200"></a>Antwoord-klasse (status 200)

string

Antwoord-inhoudstype: application/xml

### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|appid|(leeg)  |Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat u de `appid` veld leeg. Anders, omvat een tekenreeks met `"Bearer" + " " + "access_token"`.|query|string|
|text|(leeg)|Vereist. Een tekenreeks met tekst waarvan de taal kan worden geïdentificeerd. De tekst mag niet groter zijn dan 10.000 tekens.|query|  string|
|Autorisatie|(leeg)|Vereist als zowel de `appid` veld en de `Ocp-Apim-Subscription-Key` header zijn leeg. Autorisatietoken: `"Bearer" + " " + "access_token"`.|koptekst|string|
|OCP-Apim-Subscription-Key  |(leeg)    |Vereist als zowel de `appid` veld en de `Authorization` header zijn leeg.|koptekst|string|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reason|
|:--|:--|
|400|Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties.|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|


## <a name="post-detectarray"></a>/DetectArray plaatsen

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie

Hiermee geeft u de talen in een matrix met tekenreeksen. Onafhankelijk van elkaar detecteert de taal van elke afzonderlijke matrixelement en retourneert een resultaat voor elke rij van de matrix.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/DetectArray`.

Hier volgt de indeling van de hoofdtekst van de aanvraag:

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>string-value-1</string>
    <string>string-value-2</string>
</ArrayOfstring>
```

De tekst kan niet langer zijn dan 10.000 tekens.

**Geretourneerde waarde:** Een string-matrix met een taalcode van twee tekens voor elke rij in de invoermatrix.

Hier volgt de indeling van de antwoordtekst:

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays" xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <string>language-code-1</string>
  <string>language-code-2</string>
</ArrayOfstring>
```

### <a name="response-class-status-200"></a>Antwoord-klasse (status 200)
`DetectArray` is voltooid. Retourneert een string-matrix met een taalcode van twee tekens voor elke rij van de invoermatrix.

string

Antwoord-inhoudstype: application/xml
 
### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|appid|(leeg)|Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat u de `appid` veld leeg. Anders, omvat een tekenreeks met `"Bearer" + " " + "access_token"`.|query|string|
|Autorisatie|(leeg)|Vereist als zowel de `appid` veld en de `Ocp-Apim-Subscription-Key` header zijn leeg.  Autorisatietoken: `"Bearer" + " " + "access_token"`.|koptekst|string|
|OCP-Apim-Subscription-Key|(leeg)|Vereist als zowel de `appid` veld en de `Authorization` header zijn leeg.|koptekst|string|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reason|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties.|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="get-addtranslation"></a>/AddTranslation ophalen

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie

> [!IMPORTANT]
> **Afschaffing Opmerking:** Deze methode wordt niet na 31 januari 2018 nieuwe zin inzendingen worden geaccepteerd. U ontvangt een foutbericht weergegeven. Zie de aankondiging over wijzigingen aan de Collaborative Translation Framework (CTF).

Een vertaling toevoegen aan de NAT-geheugen.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/AddTranslation`.

### <a name="response-class-status-200"></a>Antwoord-klasse (status 200)

string

Antwoord-inhoudstype: aanvraag: xml
 
### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype   |
|:--|:--|:--|:--|:--|
|appid|(leeg)|Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat u de `appid` veld leeg. Anders, omvat een tekenreeks met `"Bearer" + " " + "access_token"`.|query|string|
|originalText|(leeg)|Vereist. Een tekenreeks met de tekst te vertalen. De maximale lengte van de tekenreeks is 1000 tekens.|query|string|
|translatedText|(leeg) |Vereist. Een tekenreeks met tekst wordt vertaald naar de doel-taal. De maximale lengte van de tekenreeks is 2000 tekens.|query|string|
|from|(leeg)   |Vereist. Een tekenreeks met de taalcode van de oorspronkelijke taal van de tekst. Bijvoorbeeld, en voor Engels en de voor Duits.|query|string|
|tot|(leeg)|Vereist. Een tekenreeks met de taalcode van de taal voor de omzetting van de tekst in.|query|string|
|rating|(leeg) |Optioneel. Een geheel getal dat staat voor de classificatie van de kwaliteit van de tekenreeks. De waarde ligt tussen 10 en 10. De standaardwaarde is 1.|query|integer|
|contentType|(leeg)    |Optioneel. De indeling van de tekst wordt vertaald. De ondersteunde indelingen zijn `text/plain` en `text/html`. Een HTML-elementen moeten juist opgemaakt, volledige elementen.    |query|string|
|category|(leeg)|Optioneel. Een tekenreeks zijn met de categorie (domein) van de vertaling. De standaardwaarde is `general`.|query|string|
|user|(leeg)|Vereist. Een tekenreeks die wordt gebruikt voor het bijhouden van de afzender van de inzending.|query|string|
|URI|(leeg)|Optioneel. Een tekenreeks zijn met de locatie van de inhoud van de vertaling.|query|string|
|Autorisatie|(leeg)|Vereist als zowel de `appid` veld en de `Ocp-Apim-Subscription-Key` header zijn leeg.  Autorisatietoken: `"Bearer" + " " + "access_token"`.  |koptekst|string|
|OCP-Apim-Subscription-Key|(leeg)|Vereist als zowel de `appid` veld en de `Authorization` header zijn leeg.|koptekst|string|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reason|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties.|
|410|`AddTranslation` wordt niet meer ondersteund.|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="post-addtranslationarray"></a>/AddTranslationArray plaatsen

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie

> [!IMPORTANT]
> **Afschaffing Opmerking:** Deze methode wordt niet na 31 januari 2018 nieuwe zin inzendingen worden geaccepteerd. U ontvangt een foutbericht weergegeven. Zie de aankondiging over wijzigingen aan de Collaborative Translation Framework (CTF).

Een matrix van vertalingen toegevoegd in het geheugen van de vertaling. Deze methode is de versie van een matrix van `AddTranslation`.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/AddTranslationArray`.

Hier volgt de indeling van de hoofdtekst van de aanvraag:

```
<AddtranslationsRequest>
  <AppId></AppId>
  <From>A string containing the language code of the source language</From>
  <Options>
    <Category xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Category>
    <ContentType xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">text/plain</ContentType>
    <User xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</User>
    <Uri xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Uri>
  </Options>
  <To>A string containing the language code of the target language</To>
  <Translations>
    <Translation xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">
      <OriginalText>string-value</OriginalText>
      <Rating>int-value</Rating>
      <Sequence>int-value</Sequence>
      <TranslatedText>string-value</TranslatedText>
    </Translation>
  </Translations>
</AddtranslationsRequest>
```

Deze elementen zijn `AddtranslationsRequest`:

* `AppId`: Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat u de `AppId` veld leeg. Anders, omvat een tekenreeks met `"Bearer" + " " + "access_token"`.
* `From`: Vereist. Een tekenreeks zijn met de taalcode van de source-taal. U moet een van de talen die zijn geretourneerd door de `GetLanguagesForTranslate` methode.
* `To`: Vereist. Een tekenreeks zijn met de taalcode van de doel-taal. U moet een van de talen die zijn geretourneerd door de `GetLanguagesForTranslate` methode.
* `Translations`: Vereist. Een matrix van vertalingen om toe te voegen in het geheugen van de vertaling. Elke omzetting moet bevatten `OriginalText`, `TranslatedText`, en `Rating`. De maximale grootte van elk `OriginalText` en `TranslatedText` is 1000 tekens. Het totaal van alle `OriginalText` en `TranslatedText` elementen mag niet meer dan 10.000 tekens. Het maximum aantal matrixelementen is 100.
* `Options`: Vereist. Een set met opties, met inbegrip van `Category`, `ContentType`, `Uri`, en `User`. `User` is vereist. `Category`, `ContentType`, en `Uri` zijn optioneel. Opgegeven elementen moeten worden vermeld in alfabetische volgorde.

### <a name="response-class-status-200"></a>Antwoord-klasse (status 200)
`AddTranslationArray` methode is voltooid. 

Na 31 januari 2018 wordt niet zin inzendingen worden geaccepteerd. De service reageert met foutcode 410.

string

Antwoord-inhoudstype: application/xml
 
### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|Autorisatie|(leeg)|Vereist als zowel de `appid` veld en de `Ocp-Apim-Subscription-Key` header zijn leeg.  Autorisatietoken: `"Bearer" + " " + "access_token"`.|koptekst|string|
|OCP-Apim-Subscription-Key|(leeg)|Vereist als zowel de `appid` veld en de `Authorization` header zijn leeg.|koptekst|string|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reason|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties.|
|410    |`AddTranslation` wordt niet meer ondersteund.|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503|De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="get-breaksentences"></a>/BreakSentences ophalen

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Neemt een deel van de tekst in zinnen en retourneert een matrix met de lengte van elke zin.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/BreakSentences`.

**Geretourneerde waarde:** Een matrix met gehele getallen die de lengte van de zinnen aangeeft. De lengte van de matrix geeft het aantal zinnen. De waarden staan de lengte van elke zin.

### <a name="response-class-status-200"></a>Antwoord-klasse (status 200)
Een matrix met gehele getallen die de lengte van de zinnen aangeeft. De lengte van de matrix geeft het aantal zinnen. De waarden staan de lengte van elke zin.

integer

Antwoord-inhoudstype: application/xml

### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|appid|(leeg)  |Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat u de `appid` veld leeg. Anders, omvat een tekenreeks met `"Bearer" + " " + "access_token"`.|query| string|
|text|(leeg)   |Vereist. Een tekenreeks die de tekst die moet worden gesplitst in zinnen aangeeft. De maximale grootte van de tekst is 10.000 tekens.|query|string|
|language   |(leeg)    |Vereist. Een tekenreeks met de taalcode van de invoertekst.|query|string|
|Autorisatie|(leeg)|Vereist als zowel de `appid` veld en de `Ocp-Apim-Subscription-Key` header zijn leeg. Autorisatietoken: `"Bearer" + " " + "access_token"`.   |koptekst|string|
|OCP-Apim-Subscription-Key|(leeg)|Vereist als zowel de `appid` veld en de `Authorization` header zijn leeg.|koptekst|string|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reason|
|:--|:--|
|400|Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401|Ongeldige referenties.|
|500|Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503|De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="post-gettranslations"></a>POST /GetTranslations

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Een matrix van vertalingen voor een bepaalde taalpaar opgehaald uit de store en de MT-engine. `GetTranslations` wijkt af van `Translate` in dat alle beschikbare vertalingen worden geretourneerd.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/GetTranslations`.

De hoofdtekst van de aanvraag bevat de optionele `TranslationOptions` object, dat deze indeling heeft:

```
<TranslateOptions xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">
  <Category>string-value</Category>
  <ContentType>text/plain</ContentType>
  <ReservedFlags></ReservedFlags>
  <State>int-value</State>
  <Uri>string-value</Uri>
  <User>string-value</User>
</TranslateOptions>
```

De `TranslateOptions` -object bevat de waarden in de volgende lijst. Ze zijn optioneel en standaard naar de meest voorkomende instellingen. Opgegeven elementen moeten worden vermeld in alfabetische volgorde.

* `Category`: Een tekenreeks zijn met de categorie (domein) van de vertaling. De standaardwaarde is `general`.
* `ContentType`: De enige ondersteunde optie, en de standaardinstelling is `text/plain`.
* `IncludeMultipleMTAlternatives`: Een Booleaanse vlag om op te geven of meer dan één in plaats van de MT-engine moet worden geretourneerd. Geldige waarden zijn `true` en `false` (hoofdlettergevoelig). De standaardwaarde is `false`, die slechts één alternatieve retourneert. De vlag instellen op `true` wordt het maken van kunstmatige alternatieven, volledig geïntegreerd met de Collaborative Translation Framework (CTF). De functie kunt terugkerende alternatieven voor zinnen die geen vertalingen in CTF hebben door toe te voegen kunstmatige alternatieven van de *n*-aanbevolen lijst van de decoder.
    - Classificaties. De classificaties worden toegepast als volgt: 
         - De beste automatische vertaling heeft een classificatie van 5.
       - De alternatieven van CTF weerspiegelen de instantie van de revisor. Ze variëren van-10 op + 10.
       - De automatisch gegenereerde (*n*-beste) vertaling alternatieven hebben een classificatie van 0 en de mate van een overeenkomst van 100.
    - Het aantal alternatieven. Het aantal geretourneerde alternatieven mag zo hoog als de waarde die is opgegeven `maxTranslations`, maar het kan zijn lager.
    - Taal-paren. Deze functionaliteit is niet beschikbaar voor vertalingen tussen Chinees vereenvoudigd en traditioneel Chinees, in beide richtingen. Het is beschikbaar voor alle andere taal paren die door Microsoft Translator worden ondersteund.
* `State`: De status van de gebruiker om u te helpen bij de aanvraag en respons correleren. Dezelfde inhoud in het antwoord geretourneerd.
* `Uri`: Filter de resultaten op basis van deze URI. Als er geen waarde is ingesteld, is de standaardinstelling `all`.
* `User`: Filter de resultaten door deze gebruiker. Als er geen waarde is ingesteld, is de standaardinstelling `all`.

Aanvraag `Content-Type` moet `text/xml`.

**Geretourneerde waarde:** Hier volgt de indeling van het antwoord:

```
<GetTranslationsResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"
  xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <From>Two character language code</From>
  <State/>
  <Translations>
    <TranslationMatch>
      <Count>int-value</Count>
      <MatchDegree>int-value</MatchDegree>
      <MatchedOriginalText/>
      <Rating>int value</Rating>
      <TranslatedText>string-value</TranslatedText>
    </TranslationMatch>
  </Translations>
</GetTranslationsResponse>
```

Dit antwoord bevat een `GetTranslationsResponse` element met de volgende waarden:

* `Translations`: Een matrix van de overeenkomsten gevonden, die zijn opgeslagen in `TranslationMatch` objecten (beschreven in de volgende sectie). De vertalingen advies inwinnen lichte varianten van de oorspronkelijke tekst (fuzzy-koppeling). De vertalingen worden gesorteerd: 100% komt overeen met eerst fuzzy komt overeen met volgende.
* `From`: Als de methode niet opgeven een `From` taal, deze waarde is afkomstig van automatische taaldetectie. Anders wordt zijn de opgegeven `From` taal.
* `State`: De status van de gebruiker om u te helpen bij de aanvraag en respons correleren. Bevat de waarde die is opgegeven in de `TranslateOptions` parameter.

De `TranslationMatch` bestaat uit van de volgende waarden:

* `Error`: De foutcode, als er een fout optreedt voor een specifieke invoer tekenreeks. Anders wordt is dit veld leeg.
* `MatchDegree`: Geeft aan hoe de invoertekst overeenkomt met de oorspronkelijke tekst gevonden in het archief. Het systeem komt overeen met invoer zinnen op basis van de store, zoals onnauwkeurig resultaten. De waarde geretourneerd bereiken tussen 0 en 100, waarbij 0 is er geen overeenkomsten en 100 is een overeenkomst exacte en hoofdlettergevoelig.
* `MatchedOriginalText`: Oorspronkelijke tekst die voor dit resultaat overeen. Deze waarde wordt geretourneerd, alleen als de overeenkomende oorspronkelijke tekst anders dan de invoertekst is. Het wordt gebruikt om de brontekst van een benadering te retourneren. Deze waarde is niet voor de Microsoft Translator-resultaten geretourneerd.
* `Rating`: Geeft aan dat de instantie van de persoon die het besluit kwaliteit. Automatische vertaling resultaten hebben een classificatie van 5. Anoniem opgegeven vertalingen hebben doorgaans een classificatie van 1 tot en met 4. Bindende opgegeven vertalingen hebben doorgaans een classificatie van 6 t/m 10.
* `Count`: Het aantal keren dat die deze tekst met deze beoordeling is geselecteerd. De waarde is 0 voor het automatisch vertaalde antwoord.
* `TranslatedText`: De vertaalde tekst.

### <a name="response-class-status-200"></a>Antwoord-klasse (status 200)
Een `GetTranslationsResponse` -object in de indeling die eerder zijn beschreven.

string

Antwoord-inhoudstype: application/xml
 
### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|appid|(leeg)|Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat u de `appid` veld leeg. Anders, omvat een tekenreeks met `"Bearer" + " " + "access_token"`.|query|string|
|text|(leeg)|Vereist. Een tekenreeks met de tekst te vertalen. De maximale grootte van de tekst is 10.000 tekens.|query|string|
|from|(leeg)|Vereist. Een tekenreeks met de taalcode van de tekst wordt vertaald.|query|string|
|tot |(leeg)    |Vereist. Een tekenreeks met de taalcode van de taal voor de omzetting van de tekst in.|query|string|
|maxTranslations|(leeg)|Vereist. Een geheel getal dat staat voor het maximum aantal vertalingen om terug te keren.|query|integer|
|Autorisatie| (leeg)|Vereist als zowel de `appid` veld en de `Ocp-Apim-Subscription-Key` header zijn leeg. Autorisatietoken: `"Bearer" + " " + "access_token"`.|string|  koptekst|
|OCP-Apim-Subscription-Key|(leeg)  |Vereist als zowel de `appid` veld en de `Authorization` header zijn leeg.|koptekst|string|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reason|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties.|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503|De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="post-gettranslationsarray"></a>POST /GetTranslationsArray

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Hiermee haalt u meerdere vertaling kandidaten voor meerdere bron teksten.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/GetTranslationsArray`.

Hier volgt de indeling van de hoofdtekst van de aanvraag:

```
<GetTranslationsArrayRequest>
  <AppId></AppId>
  <From>language-code</From>
  <MaxTranslations>int-value</MaxTranslations>
  <Options>
    <Category xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Category>
    <ContentType xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">text/plain</ContentType>
    <ReservedFlags xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"/>
    <State xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">int-value</State>
    <Uri xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Uri>
    <User xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</User>
  </Options>
  <Texts>
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">string-value</string>
  </Texts>
  <To>language-code</To>
</GetTranslationsArrayRequest>
```

`GetTranslationsArrayRequest` deze elementen bevat:

* `AppId`: Vereist. Als de `Authorization` header wordt gebruikt, laat u de `AppId` veld leeg. Anders, omvat een tekenreeks met `"Bearer" + " " + "access_token"`.
* `From`: Vereist. Een tekenreeks met de taalcode van de tekst wordt vertaald.
* `MaxTranslations`: Vereist. Een geheel getal dat staat voor het maximum aantal vertalingen om terug te keren.
* `Options`: Optioneel. Een `Options` -object dat de volgende waarden bevat. Ze zijn optioneel en standaard naar de meest voorkomende instellingen. Opgegeven elementen moeten worden vermeld in alfabetische volgorde.
    - `Category`: Een tekenreeks zijn met de categorie (domein) van de vertaling. De standaardwaarde is `general`.
    - `ContentType`: De enige ondersteunde optie, en de standaardinstelling is `text/plain`.
    - `IncludeMultipleMTAlternatives`: Een Booleaanse vlag om op te geven of meer dan één in plaats van de MT-engine moet worden geretourneerd. Geldige waarden zijn `true` en `false` (hoofdlettergevoelig). De standaardwaarde is `false`, die slechts één alternatieve retourneert. De vlag instellen op `true` kunt genereren van kunstmatige alternatieven vertaling volledig geïntegreerd met de gezamenlijke vertalingen Framework (CTF). De functie kunt terugkerende alternatieven voor zinnen waarvoor geen alternatieven in CTF door toe te voegen kunstmatige alternatieven van de *n*-aanbevolen lijst van de decoder.
        - Classificaties de classificaties worden toegepast als volgt:
          - De beste automatische vertaling heeft een classificatie van 5.
          - De alternatieven van CTF weerspiegelen de instantie van de revisor. Ze variëren van-10 op + 10.
          - De automatisch gegenereerde (*n*-beste) vertaling alternatieven hebben een classificatie van 0 en de mate van een overeenkomst van 100.
        - Het aantal alternatieven. Het aantal geretourneerde alternatieven mag zo hoog als de waarde die is opgegeven `maxTranslations`, maar het kan zijn lager.
        - Taal-paren. Deze functionaliteit is niet beschikbaar voor vertalingen tussen Chinees vereenvoudigd en traditioneel Chinees, in beide richtingen. Het is beschikbaar voor alle andere taal paren die door Microsoft Translator worden ondersteund.
* `State`: De status van de gebruiker om u te helpen bij de aanvraag en respons correleren. Dezelfde inhoud in het antwoord geretourneerd.
* `Uri`: Filter de resultaten op basis van deze URI. Als er geen waarde is ingesteld, is de standaardinstelling `all`.
* `User`: Filter de resultaten door deze gebruiker. Als er geen waarde is ingesteld, is de standaardinstelling `all`.
* `Texts`: Vereist. Een matrix met de tekst voor de vertaling. Alle tekenreeksen moeten zich in dezelfde taal. Het totaal van alle tekst die moet worden vertaald, kan niet groter zijn dan 10.000 tekens. Het maximum aantal matrixelementen is 10.
* `To`: Vereist. Een tekenreeks met de taalcode van de taal voor de omzetting van de tekst in.

U kunt optionele elementen weglaten. Elementen die directe onderliggende items van `GetTranslationsArrayRequest` moet worden vermeld in alfabetische volgorde.

Aanvraag `Content-Type` moet `text/xml`.

**Geretourneerde waarde:** Hier volgt de indeling van het antwoord:

```
<ArrayOfGetTranslationsResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <GetTranslationsResponse>
    <From>language-code</From>
    <State/>
    <Translations>
      <TranslationMatch>
        <Count>int-value</Count>
        <MatchDegree>int-value</MatchDegree>
        <MatchedOriginalText>string-value</MatchedOriginalText>
        <Rating>int-value</Rating>
        <TranslatedText>string-value</TranslatedText>
      </TranslationMatch>
      <TranslationMatch>
        <Count>int-value</Count>
        <MatchDegree>int-value</MatchDegree>
        <MatchedOriginalText/>
        <Rating>int-value</Rating>
        <TranslatedText>string-value</TranslatedText>
      </TranslationMatch>
    </Translations>
  </GetTranslationsResponse>
</ArrayOfGetTranslationsResponse>
```

Elke `GetTranslationsResponse` element bevat deze waarden:

* `Translations`: Een matrix van de overeenkomsten gevonden, die zijn opgeslagen in `TranslationMatch` objecten (beschreven in de volgende sectie). De vertalingen advies inwinnen lichte varianten van de oorspronkelijke tekst (fuzzy-koppeling). De vertalingen worden gesorteerd: 100% komt overeen met eerst fuzzy komt overeen met volgende.
* `From`: Als de methode niet opgeven een `From` taal, deze waarde is afkomstig van automatische taaldetectie. Anders wordt zijn de opgegeven `From` taal.
* `State`: De status van de gebruiker om u te helpen bij de aanvraag en respons correleren. Bevat de waarde die is opgegeven in de `TranslateOptions` parameter.

De `TranslationMatch` -object bevat de volgende waarden:
* `Error`: De foutcode, als er een fout optreedt voor een specifieke invoer tekenreeks. Anders wordt is dit veld leeg.
* `MatchDegree`: Geeft aan hoe de invoertekst overeenkomt met de oorspronkelijke tekst gevonden in het archief. Het systeem komt overeen met invoer zinnen op basis van de store, zoals onnauwkeurig resultaten. De waarde geretourneerd bereiken tussen 0 en 100, waarbij 0 is er geen overeenkomsten en 100 is een overeenkomst exacte en hoofdlettergevoelig.
* `MatchedOriginalText`: Oorspronkelijke tekst die voor dit resultaat overeen. Deze waarde wordt geretourneerd, alleen als de overeenkomende oorspronkelijke tekst anders dan de invoertekst is. Het wordt gebruikt om de brontekst van een benadering te retourneren. Deze waarde is niet voor de Microsoft Translator-resultaten geretourneerd.
* `Rating`: Geeft aan dat de instantie van de persoon die het besluit kwaliteit. Automatische vertaling resultaten hebben een classificatie van 5. Anoniem opgegeven vertalingen hebben doorgaans een classificatie van 1 tot en met 4. Bindende opgegeven vertalingen hebben doorgaans een classificatie van 6 t/m 10.
* `Count`: Het aantal keren dat die deze tekst met deze beoordeling is geselecteerd. De waarde is 0 voor het automatisch vertaalde antwoord.
* `TranslatedText`: De vertaalde tekst.


### <a name="response-class-status-200"></a>Antwoord-klasse (status 200)

string

Antwoord-inhoudstype: application/xml
 
### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|Autorisatie  |(leeg)    |Vereist als zowel de `appid` veld en de `Ocp-Apim-Subscription-Key` header zijn leeg.  Autorisatietoken: `"Bearer" + " " + "access_token"`.|koptekst|string|
|OCP-Apim-Subscription-Key|(leeg)  |Vereist als zowel de `appid` veld en de `Authorization` header zijn leeg.|koptekst|string|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reason|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties.|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Migreren naar Translator Text-API v3 ](../migrate-to-v3.md)


