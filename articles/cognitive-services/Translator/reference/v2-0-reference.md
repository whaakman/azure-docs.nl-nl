---
title: Microsoft Translator tekst API V2.0-verwijzing | Microsoft Docs
titleSuffix: Cognitive Services
description: Documentatie voor de API V2.0 Microsoft Translator tekst.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 05/15/2018
ms.author: v-jansko
ms.openlocfilehash: e32e28608d2fecf27b61acff74af7eb6849f0ba1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345663"
---
# <a name="translator-text-api-v20"></a>Conversieprogramma tekst API v2.0

> [!IMPORTANT]
> Deze versie van de tekst conversieprogramma API is afgeschaft. [Documentatie voor v3 van de API van de tekst conversieprogramma](v3-0-reference.md).

Microsoft Translator V2 tekst API kunnen probleemloos worden geïntegreerd in uw toepassingen, websites, hulpprogramma's of andere oplossingen om te bieden meertalige gebruikerservaringen. Mogelijkheden van industrienormen, kan deze worden gebruikt op elk hardwareplatform en met een besturingssysteem vertaling en andere taal gerelateerde bewerkingen zoals tekst taal wordt gedetecteerd of tekst-naar-spraak uit te voeren. Klik hier voor meer informatie over de API van Microsoft Translator.

## <a name="getting-started"></a>Aan de slag
Voor toegang tot de Microsoft Translator tekst-API wordt gebruikt, u moet [aanmelden voor Microsoft Azure](../translator-text-how-to-signup.md).

## <a name="authorization"></a>Autorisatie
Alle aanroepen voor het Microsoft Translator tekst API vereist een abonnementssleutel om te verifiëren. De API ondersteunt twee modi van verificatie:

* Met behulp van een toegangstoken. Gebruik de abonnementssleutel waarnaar wordt verwezen in **stap** 9 voor het genereren van een toegangstoken door het maken van een POST-aanvraag naar de autorisatie-service. Zie de documentatie van de service voor beveiligingstokens voor meer informatie. Het toegangstoken doorgeven aan de NAT-service met behulp van de autorisatie-header of de queryparameter access_token. Het toegangstoken is geldig voor 10 minuten. Vraag een nieuw toegangstoken elke 10 minuten en blijven gebruiken dezelfde toegang token voor herhaalde aanvragen binnen deze 10 minuten.

* Met behulp van een abonnementssleutel rechtstreeks. De abonnementssleutel van uw doorgeven als een waarde in `Ocp-Apim-Subscription-Key` header die deel uitmaakt van uw aanvraag voor de NAT-API. In deze modus hoeft u niet de token authentication-service voor het genereren van een toegangstoken niet aanroepen.

De abonnementssleutel van uw en het toegangstoken beschouwen als geheimen die moeten worden verborgen.

## <a name="profanity-handling"></a>Taalgebruik verwerking
De NAT-service wordt normaal gesproken taalgebruik die aanwezig is in de bron in de vertaling behouden. De mate van taalgebruik en de context waarin woorden schennende verschillen tussen culturen en daardoor de mate van taalgebruik in de taal van het doel kan versterkt of verminderd.

Als u wilt vermijden taalgebruik in de vertaling, ongeacht de aanwezigheid van taalgebruik in de brontekst, kunt u de optie voor de methoden die ondersteuning bieden voor het filteren taalgebruik. De optie kunt u kiezen of u wilt zien taalgebruik verwijderd of is gemarkeerd met de juiste labels of geen actie ondernomen. De toegestane waarden van `ProfanityAction` zijn `NoAction` (standaard), gemarkeerd en `Deleted`.


|ProfanityAction    |Actie |Voorbeeld van de bron (Japans)  |Voorbeeld van de vertaling (Engels)  |
|:--|:--|:--|:--|
|NoAction   |Standaard. Hetzelfde als de optie niet instellen. Taalgebruik geeft van bron naar doel.        |彼はジャッカスです。     |Hij is een jackass.   |
|Gemarkeerd     |Ongepaste woorden omgeven door een XML-labels <profanity> en </profanity>.     |彼はジャッカスです。 |Hij een <profanity>jackass</profanity>.    |
|Verwijderd    |Ongepaste woorden wordt verwijderd uit de uitvoer zonder vervanging.     |彼はジャッカスです。 |Hij een.   |

    
## <a name="excluding-content-from-translation"></a>Met uitzondering van de inhoud van de vertaling
Tijdens het vertalen van inhoud met zoals HTML-tags (`contentType=text/html`), is het vaak nuttig uitsluiten van specifieke inhoudstypen van de vertaling. U kunt het kenmerk `class=notranslate` om op te geven van inhoud die moet blijven in de oorspronkelijke taal. In het volgende voorbeeld wordt de inhoud in de eerste `div` element niet worden vertaald, terwijl de inhoud van de tweede `div` element worden vertaald.

```HTML
<div class="notranslate">This will not be translated.</div>
<div>This will be translated. </div>
```

## <a name="get-translate"></a>OPHALEN / vertalen

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Zet een tekenreeks van één taal naar een andere.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/Translate`.

**Retourwaarde:** een tekenreeks voor de vertaalde tekst.

Als u eerder hebt gebruikt `AddTranslation` of `AddTranslationArray` invoeren van een vertaling met een classificatie van 5 of hoger voor dezelfde bron zin `Translate` retourneert alleen de bovenste keuze die beschikbaar is voor uw systeem. 'Dezelfde bron zin' betekent dat de exacte dezelfde (100%-koppeling), met uitzondering van hoofdlettergebruik, spaties, labelwaarden en leestekens aan het einde van een zin. Als er geen waardering is opgeslagen met een classificatie van 5 of hoger worden het geretourneerde resultaat de automatische vertaling door Microsoft Translator.

### <a name="response-class-status-200"></a>Antwoord klasse (Status 200)

tekenreeks

Type van de inhoud van de reactie: application/xml 

### <a name="parameters"></a>Parameters

|Parameter|Waarde|Beschrijving    |Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|AppID  |(leeg)    |Vereist. Als de autorisatie of Ocp-Apim-Subscription-Key-header wordt gebruikt, laat het veld appid leeg anders opgenomen in een tekenreeks met 'Bearer' + ' ' + 'access_token'.|query|tekenreeks|
|tekst|(leeg)   |Vereist. Een tekenreeks die de tekst te vertalen vertegenwoordigt. De grootte van de tekst moet 10000 tekens niet overschrijden.|query|tekenreeks|
|uit|(leeg)   |Optioneel. Een tekenreeks die de taalcode van de Vertalingtekst vertegenwoordigt. Bijvoorbeeld, en voor Engels.|query|tekenreeks|
|tot|(leeg) |Vereist. Een tekenreeks die de taalcode van de tekst in vertalen vertegenwoordigt.|query|tekenreeks|
|contentType|(leeg)    |Optioneel. De indeling van de tekst wordt vertaald. De ondersteunde indelingen zijn text/plain (standaard) en text/html. HTML-code moet een geldige, volledige-element.|query|tekenreeks|
|category|(leeg)   |Optioneel. Een tekenreeks met de categorie (domein) van de vertaling. De standaardwaarde 'algemene'.|query|tekenreeks|
|Autorisatie|(leeg)  |Vereist als de appid veld of Ocp-Apim-Subscription-Key-header is niet opgegeven. Verificatietoken: 'Bearer' + ' ' + 'access_token'.|koptekst|tekenreeks|
|OCP-Apim-Subscription-Key|(leeg)  |Vereist als de appid veld of de autorisatie-header is niet opgegeven.|koptekst|tekenreeks|


### <a name="response-messages"></a>Antwoordberichten

|HTTP-statuscode|Reden|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties|
|500    |Serverfout. Als de fout zich blijft voordoen, laat ons weten. Geef ons bij de geschatte datum en tijd van de aanvraag en met de aanvraag-ID die is opgenomen in de response-header X-MS-Trans-Info.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat ons weten als de fout zich blijft voordoen.|

## <a name="post-translatearray"></a>Post een bericht /TranslateArray

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Gebruik de `TranslateArray` methode voor het ophalen van vertalingen voor meerdere bron teksten.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/TranslateArray`.

De indeling van de aanvraagtekst moeten als volgt:

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

Elementen in de `TranslateArrayRequest` zijn:


* `appid`: Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat het veld appid leeg anders omvatten een tekenreeks met `"Bearer" + " " + "access_token"`.
* `from`: Dit is optioneel. Een tekenreeks die de taalcode omzetten van de tekst uit. De reactie omvat het resultaat van de taal van automatische detectie als dit veld leeg.
* `options`: Dit is optioneel. Een `Options` object met de onderstaande waarden. Ze zijn optioneel en standaard ingesteld op de meest voorkomende instellingen. Opgegeven elementen moeten worden vermeld in alfabetische volgorde.
    - `Category`: Een tekenreeks met de categorie (domein) van de vertaling. Standaard ingesteld op `general`.
    - `ContentType`: De indeling van de tekst wordt vertaald. De ondersteunde indelingen zijn `text/plain` (standaard), `text/xml` en `text/html`. HTML-code moet een geldige, volledige-element.
    - `ProfanityAction`: Hiermee geeft u op hoe profanities worden verwerkt, zoals hierboven is uitgelegd. Geaccepteerde waarden van `ProfanityAction` zijn `NoAction` (standaard), `Marked` en `Deleted`.
    - `State`: Status gebruiker om te vergelijken aanvraag en -antwoord. Dezelfde inhoud wordt in het antwoord geretourneerd.
    - `Uri`: De resultaten worden op deze URI filteren. Standaardwaarde: `all`.
    - `User`: De resultaten van deze gebruiker filteren. Standaardwaarde: `all`.
* `texts`: Vereist. Een matrix met de tekst voor vertaling. Alle tekenreeksen moeten van dezelfde taal. Het totaal van alle teksten worden vertaald mag niet groter zijn dan 10000 tekens. Het maximum aantal matrixelementen is 2000.
* `to`: Vereist. Een tekenreeks die de taalcode van de tekst in vertalen vertegenwoordigt.

Optionele elementen kunnen worden weggelaten. Elementen die directe onderliggende van TranslateArrayRequest elementen moeten worden vermeld in alfabetische volgorde.

De methode TranslateArray accepteert `application/xml` of `text/xml` voor `Content-Type`.

**Retourwaarde:** A `TranslateArrayResponse` matrix. Elke `TranslateArrayResponse` heeft de volgende elementen:

* `Error`: Hiermee geeft een fout of een is opgetreden. Anders ingesteld op null.
* `OriginalSentenceLengths`: Er is een matrix van gehele getallen die de lengte van elke zin in de oorspronkelijke brontekst aangeeft. De lengte van de matrix geeft het aantal zinnen.
* `TranslatedText`: De vertaalde tekst.
* `TranslatedSentenceLengths`: Er is een matrix van gehele getallen die de lengte van elke zin in de vertaalde tekst aangeeft. De lengte van de matrix geeft het aantal zinnen.
* `State`: Status gebruiker om te vergelijken aanvraag en -antwoord. Retourneert het dezelfde inhoud zoals in de aanvraag.

De indeling van de antwoordtekst is als volgt.

```
<ArrayOfTranslateArrayResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"
  xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
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

### <a name="response-class-status-200"></a>Antwoord klasse (Status 200)
Een geslaagde reactie bevat een matrix van `TranslateArrayResponse` in de indeling die hierboven worden beschreven.

tekenreeks

Type van de inhoud van de reactie: application/xml

### <a name="parameters"></a>Parameters

|Parameter|Waarde|Beschrijving|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|Autorisatie|(leeg)) |Vereist als de appid veld of Ocp-Apim-Subscription-Key-header is niet opgegeven. Verificatietoken: 'Bearer' + ' ' + 'access_token'.|koptekst|tekenreeks|
|OCP-Apim-Subscription-Key|(leeg)|Vereist als de appid veld of de autorisatie-header is niet opgegeven.|koptekst|tekenreeks|

### <a name="response-messages"></a>Antwoordberichten

|HTTP-statuscode   |Reden|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht. Veelvoorkomende fouten zijn onder andere: <ul><li>Element van de matrix mag niet leeg zijn</li><li>Ongeldige categorienaam</li><li>Van taal is ongeldig</li><li>Taal is ongeldig</li><li>De aanvraag bevat te veel elementen</li><li>De From-taal wordt niet ondersteund.</li><li>De taal aan wordt niet ondersteund.</li><li>Vertalen aanvraag heeft te veel gegevens</li><li>HTML is niet de juiste indeling</li><li>Te veel tekenreeksen zijn doorgegeven in de Vertaal aanvragen</li></ul>|
|401    |Ongeldige referenties|
|500    |Serverfout. Als de fout zich blijft voordoen, laat ons weten. Geef ons bij de geschatte datum en tijd van de aanvraag en met de aanvraag-ID die is opgenomen in de response-header X-MS-Trans-Info.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat ons weten als de fout zich blijft voordoen.|

## <a name="post-getlanguagenames"></a>POST /GetLanguageNames

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Als de parameter doorgegeven haalt beschrijvende namen voor de talen `languageCodes`, en een gelokaliseerde met de taal van de doorgegeven landinstelling.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/GetLanguageNames`.

De aanvraagtekst bevat een string-matrix die de ISO taalcodes 639-1 voor het ophalen van de beschrijvende namen voor vertegenwoordigt. Bijvoorbeeld:

```
<ArrayOfstring xmlns:i="http://www.w3.org/2001/XMLSchema-instance"  xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>zh</string>
    <string>en</string>
</ArrayOfstring>
```

**Retourwaarde:** een string-matrix met namen van talen ondersteund door de Service conversieprogramma, in de gevraagde taal is gelokaliseerd.

### <a name="response-class-status-200"></a>Antwoord klasse (Status 200)
Een string-matrix met namen van talen ondersteund door de Service conversieprogramma, in de gevraagde taal is gelokaliseerd.

tekenreeks

Type van de inhoud van de reactie: application/xml
 
### <a name="parameters"></a>Parameters

|Parameter|Waarde|Beschrijving|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|AppID|(leeg)|Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat het veld appid leeg anders omvatten een tekenreeks met `"Bearer" + " " + "access_token"`.|query|tekenreeks|
|Landinstellingen|(leeg) |Vereist. Een tekenreeks die een combinatie van een ISO 639 twee letters kleine cultuur-code die is gekoppeld aan een taal en een code van de twee letters, hoofdletters subcultuur ISO 3166 voor lokalisatie van de taalnamen van de of een kleine cultuur ISO 639 code zelfstandig vertegenwoordigt.|query|tekenreeks|
|Autorisatie|(leeg)  |Vereist als het veld appid of `Ocp-Apim-Subscription-Key` -header is niet opgegeven. Verificatietoken: `"Bearer" + " " + "access_token"`.|koptekst|tekenreeks|
|OCP-Apim-Subscription-Key|(leeg)  |Vereist als het veld appid of `Authorization` -header is niet opgegeven.|koptekst|tekenreeks|

### <a name="response-messages"></a>Antwoordberichten

|HTTP-statuscode|Reden|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties|
|500    |Serverfout. Als de fout zich blijft voordoen, laat ons weten. Geef ons bij de geschatte datum en tijd van de aanvraag en met de aanvraag-ID die is opgenomen in de response-header X-MS-Trans-Info.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat ons weten als de fout zich blijft voordoen.|

## <a name="get-getlanguagesfortranslate"></a>/GetLanguagesForTranslate ophalen

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Een lijst met taalcodes voor talen die worden ondersteund door de vertaalservice ophalen.  `Translate` en `TranslateArray` tussen twee van deze talen kan vertalen.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForTranslate`.

**Retourwaarde:** een tekenreeksmatrix met de taalcodes die wordt ondersteund door de NAT-Services.

### <a name="response-class-status-200"></a>Antwoord klasse (Status 200)
Een string-matrix met de taalcodes die wordt ondersteund door de NAT-Services.

tekenreeks

Type van de inhoud van de reactie: application/xml
 
### <a name="parameters"></a>Parameters

|Parameter|Waarde|Beschrijving|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|AppID|(leeg)|Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat het veld appid leeg anders omvatten een tekenreeks met `"Bearer" + " " + "access_token"`.|query|tekenreeks|
|Autorisatie|(leeg)  |Vereist als de `appid` veld of `Ocp-Apim-Subscription-Key` -header is niet opgegeven. Verificatietoken: `"Bearer" + " " + "access_token"`.|koptekst|tekenreeks|
|OCP-Apim-Subscription-Key|(leeg)|Vereist als de `appid` veld of `Authorization` -header is niet opgegeven.|koptekst|tekenreeks|

### <a name="response-messages"></a>Antwoordberichten

|HTTP-statuscode|Reden|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties|
|500    |Serverfout. Als de fout zich blijft voordoen, laat ons weten. Geef ons bij de geschatte datum en tijd van de aanvraag en met de aanvraag-ID die is opgenomen in de response-header X-MS-Trans-Info.|
|503|De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat ons weten als de fout zich blijft voordoen.|

## <a name="get-getlanguagesforspeak"></a>/GetLanguagesForSpeak ophalen

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Hiermee worden de talen opgehaald die beschikbaar zijn voor spraaksynthese.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForSpeak`.

**Retourwaarde:** een tekenreeksmatrix met de taalcodes voor spraak-synthese ondersteund door de NAT-Service.

### <a name="response-class-status-200"></a>Antwoord klasse (Status 200)
Een string-matrix met de taalcodes voor spraak-synthese ondersteund door de NAT-Service.

tekenreeks

Type van de inhoud van de reactie: application/xml

### <a name="parameters"></a>Parameters

|Parameter|Waarde|Beschrijving|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|AppID|(leeg)|Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat het veld appid leeg anders omvatten een tekenreeks met `"Bearer" + " " + "access_token"`.|query|tekenreeks|
|Autorisatie|(leeg)|Vereist als de `appid` veld of `Ocp-Apim-Subscription-Key` -header is niet opgegeven. Verificatietoken: `"Bearer" + " " + "access_token"`.|koptekst|tekenreeks|
|OCP-Apim-Subscription-Key|(leeg)|Vereist als de `appid` veld of `Authorization` -header is niet opgegeven.|koptekst|tekenreeks|
 
### <a name="response-messages"></a>Antwoordberichten

|HTTP-statuscode|Reden|
|:--|:--|
|400|Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401|Ongeldige referenties|
|500    |Serverfout. Als de fout zich blijft voordoen, laat ons weten. Geef ons met de geschatte datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat ons weten als de fout zich blijft voordoen.|

## <a name="get-speak"></a>OPHALEN / uitspreken

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Retourneert een wave of mp3 stroom van de doorgegeven-tekst wordt gesproken in de gewenste taal.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/Speak`.

**Retourwaarde:** een wave of mp3 stroom van de doorgegeven-tekst wordt gesproken in de gewenste taal.

### <a name="response-class-status-200"></a>Antwoord klasse (Status 200)

Binaire

Type van de inhoud van de reactie: application/xml 

### <a name="parameters"></a>Parameters

|Parameter|Waarde|Beschrijving|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|AppID|(leeg)|Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat het veld appid leeg anders omvatten een tekenreeks met `"Bearer" + " " + "access_token"`.|query|tekenreeks|
|tekst|(leeg)   |Vereist. Een tekenreeks met een zin of zinnen van de opgegeven taal worden uitgesproken voor de wave-stroom. De grootte van de tekst te spreken mag maximaal 2000 tekens.|query|tekenreeks|
|taal|(leeg)   |Vereist. Een tekenreeks die de code van de ondersteunde taal voor de tekst in te spreken. De code moet aanwezig zijn in de lijst met codes die worden geretourneerd van de methode `GetLanguagesForSpeak`.|query|tekenreeks|
|Indeling|(leeg)|Optioneel. Een tekenreeks waarmee de inhoud-ID. Op dit moment `audio/wav` en `audio/mp3` beschikbaar zijn. De standaardwaarde is `audio/wav`.|query|tekenreeks|
|opties|(leeg)    |<ul><li>Optioneel. Een tekenreeks opgeven van de eigenschappen van de kunstmatige spraak:<li>`MaxQuality` en `MinSize` beschikbaar zijn voor de kwaliteit van de audio signalen opgeven. Met `MaxQuality`, krijgt u stemmen met de hoogste kwaliteit en met `MinSize`, krijgt u de stemmen met de kleinste grootte. Standaard is `MinSize`.</li><li>`female` en `male` beschikbaar zijn voor het opgeven van de gewenste geslacht van de stem. De standaardwaarde is `female`. Gebruik de verticale balk '|` to include multiple options. For example  `MaxQuality|Man '.</li></li></ul> |query|tekenreeks|
|Autorisatie|(leeg)|Vereist als de `appid` veld of `Ocp-Apim-Subscription-Key` -header is niet opgegeven. Verificatietoken: `"Bearer" + " " + "access_token"`.|koptekst|tekenreeks|
|OCP-Apim-Subscription-Key|(leeg)  |Vereist als de `appid` veld of `Authorization` -header is niet opgegeven.|koptekst|tekenreeks|

### <a name="response-messages"></a>Antwoordberichten

|HTTP-statuscode|Reden|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties|
|500    |Serverfout. Als de fout zich blijft voordoen, laat ons weten. Geef ons met de geschatte datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat ons weten als de fout zich blijft voordoen.|

## <a name="get-detect"></a>OPHALEN / detecteren

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Gebruik de `Detect` methode voor het identificeren van de taal van een geselecteerde stuk tekst.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/Detect`.

**Retourwaarde:** een tekenreeks met een taalcode van twee tekens voor de opgegeven tekst. .

### <a name="response-class-status-200"></a>Antwoord klasse (Status 200)

tekenreeks

Type van de inhoud van de reactie: application/xml

### <a name="parameters"></a>Parameters

|Parameter|Waarde|Beschrijving|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|AppID|(leeg)  |Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat het veld appid leeg anders omvatten een tekenreeks met `"Bearer" + " " + "access_token"`.|query|tekenreeks|
|tekst|(leeg)|Vereist. Een tekenreeks met tekst waarvan de taal is om te worden geïdentificeerd. De grootte van de tekst moet 10000 tekens niet overschrijden.|query| tekenreeks|
|Autorisatie|(leeg)|Vereist als de `appid` veld of `Ocp-Apim-Subscription-Key` -header is niet opgegeven. Verificatietoken: `"Bearer" + " " + "access_token"`.|koptekst|tekenreeks|
|OCP-Apim-Subscription-Key  |(leeg)    |Vereist als de `appid` veld of `Authorization` -header is niet opgegeven.|koptekst|tekenreeks|

### <a name="response-messages"></a>Antwoordberichten

|HTTP-statuscode|Reden|
|:--|:--|
|400|Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties|
|500    |Serverfout. Als de fout zich blijft voordoen, laat ons weten. Geef ons met de geschatte datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat ons weten als de fout zich blijft voordoen.|


## <a name="post-detectarray"></a>Post een bericht /DetectArray

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Gebruik de `DetectArray` methode voor het identificeren van de taal van een matrix van tekenreeks tegelijk. Onafhankelijke detectie van elk matrixelement afzonderlijke voert en retourneert een resultaat voor elke rij van de matrix.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/DetectArray`.

De indeling van de aanvraagtekst moet als volgt.

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>string-value-1</string>
    <string>string-value-2</string>
</ArrayOfstring>
```

De grootte van de tekst moet 10000 tekens niet overschrijden.

**Retourwaarde:** een tekenreeksmatrix met een taal twee tekens codes voor elke rij van de invoermatrix.

De indeling van de antwoordtekst is als volgt.

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
  <string>language-code-1</string>
  <string>language-code-2</string>
</ArrayOfstring>
```

### <a name="response-class-status-200"></a>Antwoord klasse (Status 200)
DetectArray is geslaagd. Retourneert een string-matrix met een twee tekens taalcodes voor elke rij van de invoermatrix.

tekenreeks

Type van de inhoud van de reactie: application/xml
 
### <a name="parameters"></a>Parameters

|Parameter|Waarde|Beschrijving|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|AppID|(leeg)|Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat het veld appid leeg anders omvatten een tekenreeks met `"Bearer" + " " + "access_token"`.|query|tekenreeks|
|Autorisatie|(leeg)|Vereist als de `appid` veld of `Ocp-Apim-Subscription-Key` -header is niet opgegeven. Verificatietoken: `"Bearer" + " " + "access_token"`.|koptekst|tekenreeks|
|OCP-Apim-Subscription-Key|(leeg)|Vereist als de `appid` veld of de autorisatie-header is niet opgegeven.|koptekst|tekenreeks|

### <a name="response-messages"></a>Antwoordberichten

|HTTP-statuscode|Reden|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties|
|500    |Serverfout. Als de fout zich blijft voordoen, laat ons weten. Geef ons bij de geschatte datum en tijd van de aanvraag en met de aanvraag-ID die is opgenomen in de response-header X-MS-Trans-Info.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat ons weten als de fout zich blijft voordoen.|

## <a name="get-addtranslation"></a>/AddTranslation ophalen

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie

> [!IMPORTANT]
> **AFSCHAFFING Opmerking:** na 31 januari 2018, deze methode accepteert geen nieuwe zin verzendingen en u ontvangt een foutbericht weergegeven. Raadpleeg deze aankondiging over wijzigingen aan de samenwerking vertaling-functies.

Voegt een vertaling toe aan het geheugen van de vertaling.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/AddTranslation`.

### <a name="response-class-status-200"></a>Antwoord klasse (Status 200)

tekenreeks

Type van de inhoud van de reactie: toepassing: xml
 
### <a name="parameters"></a>Parameters

|Parameter|Waarde|Beschrijving|Parametertype|Gegevenstype   |
|:--|:--|:--|:--|:--|
|AppID|(leeg)|Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat het veld appid leeg anders omvatten een tekenreeks met `"Bearer" + " " + "access_token"`.|query|tekenreeks|
|OriginalText|(leeg)|Vereist. Een tekenreeks met de tekst die moet worden vertaald uit. De tekenreeks mag uit maximaal 1000 tekens.|query|tekenreeks|
|TranslatedText|(leeg) |Vereist. Een tekenreeks met vertaald tekst in de doel-taal. De tekenreeks mag uit maximaal 2000 tekens.|query|tekenreeks|
|uit|(leeg)   |Vereist. Een tekenreeks die de taalcode van de Vertalingtekst vertegenwoordigt. NL = Engels, de Duitse enzovoort =...|query|tekenreeks|
|tot|(leeg)|Vereist. Een tekenreeks die de taalcode van de tekst in vertalen vertegenwoordigt.|query|tekenreeks|
|rating|(leeg) |Optioneel. Een geheel getal dat de beoordeling van de kwaliteit voor deze tekenreeks. De waarde tussen-10 en 10. De standaardwaarde is 1.|query|geheel getal|
|contentType|(leeg)    |Optioneel. De indeling van de tekst wordt vertaald. De ondersteunde indelingen zijn ' text/plain' en ' text/html'. HTML-code moet een geldige, volledige-element.   |query|tekenreeks|
|category|(leeg)|Optioneel. Een tekenreeks met de categorie (domein) van de vertaling. De standaardwaarde 'algemene'.|query|tekenreeks|
|Gebruiker|(leeg)|Vereist. Een tekenreeks die wordt gebruikt voor het bijhouden van de afzender van het verzenden.|query|tekenreeks|
|uri|(leeg)|Optioneel. Een tekenreeks met de locatie van de inhoud van deze vertaling.|query|tekenreeks|
|Autorisatie|(leeg)|Vereist als het veld appid of `Ocp-Apim-Subscription-Key` -header is niet opgegeven. Verificatietoken: `"Bearer" + " " + "access_token"`.    |koptekst|tekenreeks|
|OCP-Apim-Subscription-Key|(leeg)|Vereist als de `appid` veld of `Authorization` -header is niet opgegeven.|koptekst|tekenreeks|

### <a name="response-messages"></a>Antwoordberichten

|HTTP-statuscode|Reden|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties|
|410|AddTranslation wordt niet langer ondersteund.|
|500    |Serverfout. Als de fout zich blijft voordoen, laat ons weten. Geef ons bij de geschatte datum en tijd van de aanvraag en met de aanvraag-ID die is opgenomen in de response-header X-MS-Trans-Info.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat ons weten als de fout zich blijft voordoen.|

## <a name="post-addtranslationarray"></a>Post een bericht /AddTranslationArray

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie

> [!IMPORTANT]
> **AFSCHAFFING Opmerking:** na 31 januari 2018, deze methode accepteert geen nieuwe zin verzendingen en u ontvangt een foutbericht weergegeven. Raadpleeg deze aankondiging over wijzigingen aan de samenwerking vertaling-functies.

Voegt een matrix van vertalingen om toe te voegen van de vertaling van geheugen. Dit is de versie van een matrix van `AddTranslation`.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/AddTranslationArray`.

De indeling van de aanvraagtekst is als volgt.

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

Elementen in het element AddtranslationsRequest zijn:

* `AppId`: Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat het veld appid leeg anders omvatten een tekenreeks met `"Bearer" + " " + "access_token"`.
* `From`: Vereist. Een tekenreeks met de taalcode van de bron-taal. U moet een van de talen die zijn geretourneerd door de `GetLanguagesForTranslate` methode.
* `To`: Vereist. Een tekenreeks met de taalcode van de doel-taal. U moet een van de talen die zijn geretourneerd door de `GetLanguagesForTranslate` methode.
* `Translations`: Vereist. Een matrix met vertalingen toevoegen aan de vertaling van geheugen. Elke vertaling moet bevatten: originalText, translatedText en classificatie. De grootte van elke originalText en translatedText is beperkt tot 1000 tekens. Het totaal van alle originalText(s) en translatedText(s) mag niet groter zijn dan 10000 tekens. Het maximum aantal matrixelementen is 100.
* `Options`: Vereist. Een set opties, waaronder categorie, ContentType Uri en gebruiker. Gebruiker is vereist. Categorie, ContentType en Uri zijn optioneel. Opgegeven elementen moeten worden vermeld in alfabetische volgorde.

### <a name="response-class-status-200"></a>Antwoord klasse (Status 200)
De methode AddTranslationArray is geslaagd. Na 31 januari 2018, zin inzendingen niet geaccepteerd. De service wordt gereageerd met foutcode 410.

tekenreeks

Type van de inhoud van de reactie: application/xml
 
### <a name="parameters"></a>Parameters

|Parameter|Waarde|Beschrijving|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|Autorisatie|(leeg)|Vereist als de appid veld of Ocp-Apim-Subscription-Key-header is niet opgegeven. Verificatietoken: 'Bearer' + ' ' + 'access_token'.|koptekst|tekenreeks|
|OCP-Apim-Subscription-Key|(leeg)|Vereist als de appid veld of de autorisatie-header is niet opgegeven.|koptekst|tekenreeks|

### <a name="response-messages"></a>Antwoordberichten

|HTTP-statuscode|Reden|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties|
|410    |AddTranslation wordt niet langer ondersteund.|
|500    |Serverfout. Als de fout zich blijft voordoen, laat ons weten. Geef ons met de geschatte datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503|De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat ons weten als de fout zich blijft voordoen.|

## <a name="get-breaksentences"></a>/BreakSentences ophalen

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Een gedeelte van de tekst breekt in zinnen en retourneert een matrix met de lengte in elke zin.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/BreakSentences`.

**Retourwaarde:** een matrix van gehele getallen die de lengte van de zinnen. De lengte van de matrix is het aantal zinnen en de waarden worden de lengte van elke zin.

### <a name="response-class-status-200"></a>Antwoord klasse (Status 200)
Een matrix met gehele getallen die de lengte van de zinnen. De lengte van de matrix is het aantal zinnen en de waarden worden de lengte van elke zin.

geheel getal

Type van de inhoud van de reactie: application/xml 

### <a name="parameters"></a>Parameters

|Parameter|Waarde|Beschrijving|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|AppID|(leeg)  |Vereist. Als de autorisatie of Ocp-Apim-Subscription-Key-header wordt gebruikt, laat het veld appid leeg anders opgenomen in een tekenreeks met 'Bearer' + ' ' + 'access_token'.|query| tekenreeks|
|tekst|(leeg)   |Vereist. Een tekenreeks die de tekst die moet worden gesplitst in zinnen vertegenwoordigt. De grootte van de tekst moet 10000 tekens niet overschrijden.|query|tekenreeks|
|taal   |(leeg)    |Vereist. Een tekenreeks die de taalcode van de ingevoerde tekst vertegenwoordigt.|query|tekenreeks|
|Autorisatie|(leeg)|Vereist als de appid veld of Ocp-Apim-Subscription-Key-header is niet opgegeven. Verificatietoken: 'Bearer' + ' ' + 'access_token'.    |koptekst|tekenreeks|
|OCP-Apim-Subscription-Key|(leeg)|Vereist als de appid veld of de autorisatie-header is niet opgegeven.|koptekst|tekenreeks|

### <a name="response-messages"></a>Antwoordberichten

|HTTP-statuscode|Reden|
|:--|:--|
|400|Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401|Ongeldige referenties|
|500|Serverfout. Als de fout zich blijft voordoen, laat ons weten. Geef ons bij de geschatte datum en tijd van de aanvraag en met de aanvraag-ID die is opgenomen in de response-header X-MS-Trans-Info.|
|503|De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat ons weten als de fout zich blijft voordoen.|

## <a name="post-gettranslations"></a>POST /GetTranslations

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Een matrix van vertalingen voor een bepaalde taalpaar opgehaald uit de store en de MT-engine. GetTranslations verschilt van vertalen als alle beschikbare vertalingen geretourneerd.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/GetTranslations`.

De hoofdtekst van de aanvraag bevat de optionele TranslationOptions-object, dat de volgende indeling heeft.

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

De `TranslateOptions` object bevat de onderstaande waarden. Ze zijn optioneel en standaard ingesteld op de meest voorkomende instellingen. Opgegeven elementen moeten worden vermeld in alfabetische volgorde.

* `Category`: Een tekenreeks met de categorie (domein) van de vertaling. De standaardwaarde 'algemene'.
* `ContentType`: De enige ondersteunde, en de standaard optie ' text/plain'.
* `IncludeMultipleMTAlternatives`: Booleaanse vlag om te bepalen of er meerdere alternatieven moeten worden geretourneerd door de engine MT. Geldige waarden zijn true en false (hoofdlettergevoelig). Standaard is ingesteld op false en slechts 1 alternatieve bevat. Kan de vlag ingesteld op true voor het genereren van kunstmatige alternatieven vertaling volledig geïntegreerd met het framework gezamenlijke vertalingen (CTF). Kan het onderdeel voor het retourneren van alternatieven voor zinnen waarvoor geen alternatieven in CTF, door kunstmatige alternatieven toe te voegen in de lijst n beste van de decoder.
    - De classificaties worden als volgt toegepast classificaties: 1) de beste automatische vertaling is een beoordeling van 5. 2) de alternatieven van CTF weerspiegelen de instantie van de revisor van-10 naar + 10. 3) de automatisch gegenereerde (n beste) vertaling alternatieven een classificatie van 0 hebben en een overeenkomst mate van 100 hebben.
    - Het aantal alternatieven het aantal geretourneerde alternatieven is maximaal maxTranslations, maar kan kleiner zijn.
    - Taal paren deze functionaliteit is niet beschikbaar voor vertalingen tussen vereenvoudigd en traditioneel Chinees, beide richtingen. Het is beschikbaar voor alle andere paren van Microsoft Translator ondersteunde taal.
* `State`: Status gebruiker om te vergelijken aanvraag en -antwoord. Dezelfde inhoud wordt in het antwoord geretourneerd.
* `Uri`: De resultaten worden op deze URI filteren. Als geen waarde is ingesteld, is de standaardinstelling is alles.
* `User`: De resultaten van deze gebruiker filteren. Als geen waarde is ingesteld, is de standaardinstelling is alles.

Aanvraag `Content-Type` moet `text/xml`.

**Retourwaarde:** de indeling van het antwoord is als volgt.

```
<GetTranslationsResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"
  xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
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

Dit omvat een `GetTranslationsResponse` element met de volgende waarden:

* `Translations`: Er is een matrix van komt overeen met gevonden, opgeslagen in TranslationMatch (Zie hieronder)-objecten. De vertalingen kunnen lichte varianten van de oorspronkelijke tekst (fuzzy overeenkomstig) bevatten. De vertalingen worden gesorteerd: 100% overeenkomt met eerst Gedeeltelijke matches hieronder.
* `From`: Als de methode geen een From-taal hebt opgegeven, is dit is het resultaat van de taal wordt automatisch gedetecteerd. Anders worden de opgegeven taal.
* `State`: Status gebruiker om te vergelijken aanvraag en -antwoord. Bevat de dezelfde waarde die is opgegeven in de parameter TranslateOptions.

TranslationMatch object bestaat uit het volgende:

* `Error`: Als er een fout opgetreden voor een specifieke invoerreeks, is de foutcode opgeslagen. Anders is het veld leeg.
* `MatchDegree`: Het systeem komt overeen met invoer zinnen tegen de store, zoals onnauwkeurig overeenkomsten.  MatchDegree geeft aan hoe de invoertekst overeenkomt met de oorspronkelijke tekst in het archief gevonden. De waarde geretourneerd bereiken tussen 0 en 100, waarbij 0 is geen overeenkomsten en 100 een exacte overeenkomst hoofdlettergevoelig is.
MatchedOriginalText: Oorspronkelijke tekst die is afgestemd voor dit resultaat. Alleen geretourneerd als de oorspronkelijke tekst komt niet overeen met de ingevoerde tekst. Gebruikt om de brontekst van een algoritmecode te retourneren. Geen Microsoft Translator resultaten geretourneerd.
* `Rating`: Hiermee geeft u de instantie van de persoon die de beslissing kwaliteit. Resultaten van automatische vertalingen hebben een beoordeling van 5. Anoniem opgegeven vertalingen hebben doorgaans een classificatie van 1 tot en met 4 terwijl bindend opgegeven vertalingen doorgaans een classificatie van 6 tot en met 10 hebben.
* `Count`: Het aantal keren dat deze vertaling met deze classificatie is geselecteerd. De waarde gelijk aan 0 voor het automatisch vertaalde antwoord.
* `TranslatedText`: De vertaalde tekst.

### <a name="response-class-status-200"></a>Antwoord klasse (Status 200)
Een `GetTranslationsResponse` -object in de indeling die hierboven worden beschreven.

tekenreeks

Type van de inhoud van de reactie: application/xml
 
### <a name="parameters"></a>Parameters

|Parameter|Waarde|Beschrijving|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|AppID|(leeg)|Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat het veld appid leeg anders omvatten een tekenreeks met `"Bearer" + " " + "access_token"`.|query|tekenreeks|
|tekst|(leeg)|Vereist. Een tekenreeks die de tekst te vertalen vertegenwoordigt. De grootte van de tekst moet 10000 tekens niet overschrijden.|query|tekenreeks|
|uit|(leeg)|Vereist. Een tekenreeks die de taalcode van de Vertalingtekst vertegenwoordigt.|query|tekenreeks|
|tot |(leeg)    |Vereist. Een tekenreeks die de taalcode van de tekst in vertalen vertegenwoordigt.|query|tekenreeks|
|maxTranslations|(leeg)|Vereist. Een geheel getal dat het maximum aantal vertalingen om terug te keren.|query|geheel getal|
|Autorisatie| (leeg)|Vereist als de `appid` veld of `Ocp-Apim-Subscription-Key` -header is niet opgegeven. Verificatietoken: `"Bearer" + " " + "access_token"`.|tekenreeks| koptekst|
|OCP-Apim-Subscription-Key|(leeg)  |Vereist als de `appid` veld of `Authorization` -header is niet opgegeven.|koptekst|tekenreeks|

### <a name="response-messages"></a>Antwoordberichten

|HTTP-statuscode|Reden|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties|
|500    |Serverfout. Als de fout zich blijft voordoen, laat ons weten. Geef ons met de geschatte datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503|De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat ons weten als de fout zich blijft voordoen.|

## <a name="post-gettranslationsarray"></a>Post een bericht /GetTranslationsArray

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Gebruik de `GetTranslationsArray` methode voor het ophalen van meerdere vertaling kandidaten voor meerdere bron teksten.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/GetTranslationsArray`.

De indeling van de aanvraagtekst is als volgt.

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

`GetTranslationsArrayRequest` bevat de volgende elementen:

* `AppId`: Vereist. Als autorisatie-header wordt gebruikt, laat het veld appid leeg anders opgenomen in een tekenreeks met `"Bearer" + " " + "access_token"`.
* `From`: Vereist. Een tekenreeks die de taalcode van de Vertalingtekst vertegenwoordigt.
* `MaxTranslations`: Vereist. Een geheel getal dat het maximum aantal vertalingen om terug te keren.
* `Options`: Dit is optioneel. Een object voor opties die de onderstaande waarden bevat. Ze zijn optioneel en standaard ingesteld op de meest voorkomende instellingen. Opgegeven elementen moeten worden vermeld in alfabetische volgorde.
    - Categorie ': een tekenreeks met de categorie (domein) van de vertaling. De standaardwaarde is algemeen.
    - `ContentType`: De enige ondersteunde, en de standaard optie text/plain.
    - `IncludeMultipleMTAlternatives`: Booleaanse vlag om te bepalen of er meerdere alternatieven moeten worden geretourneerd door de engine MT. Geldige waarden zijn true en false (hoofdlettergevoelig). Standaard is ingesteld op false en slechts 1 alternatieve bevat. Kan de vlag ingesteld op true voor het genereren van kunstmatige alternatieven vertaling volledig geïntegreerd met het framework gezamenlijke vertalingen (CTF). Kan het onderdeel voor het retourneren van alternatieven voor zinnen waarvoor geen alternatieven in CTF, door kunstmatige alternatieven toe te voegen in de lijst n beste van de decoder.
        - De classificaties worden als volgt toegepast classificaties: 1) de beste automatische vertaling is een beoordeling van 5. 2) de alternatieven van CTF weerspiegelen de instantie van de revisor van-10 naar + 10. 3) de automatisch gegenereerde (n beste) vertaling alternatieven een classificatie van 0 hebben en een overeenkomst mate van 100 hebben.
        - Het aantal alternatieven het aantal geretourneerde alternatieven is maximaal maxTranslations, maar kan kleiner zijn.
        - Taal paren deze functionaliteit is niet beschikbaar voor vertalingen tussen vereenvoudigd en traditioneel Chinees, beide richtingen. Het is beschikbaar voor alle andere paren van Microsoft Translator ondersteunde taal.
* `State`: Status gebruiker om te vergelijken aanvraag en -antwoord. Dezelfde inhoud wordt in het antwoord geretourneerd.
* `Uri`: De resultaten worden op deze URI filteren. Als geen waarde is ingesteld, is de standaardinstelling is alles.
* `User`: De resultaten van deze gebruiker filteren. Als geen waarde is ingesteld, is de standaardinstelling is alles.
* `Texts`: Vereist. Een matrix met de tekst voor vertaling. Alle tekenreeksen moeten van dezelfde taal. Het totaal van alle teksten worden vertaald mag niet groter zijn dan 10000 tekens. Het maximum aantal matrixelementen is 10.
* `To`: Vereist. Een tekenreeks die de taalcode van de tekst in vertalen vertegenwoordigt.

Optionele elementen kunnen worden weggelaten. Elementen die directe onderliggende elementen van `GetTranslationsArrayRequest` moet worden vermeld in alfabetische volgorde.

Aanvraag `Content-Type` moet `text/xml`.

**Retourwaarde:** de indeling van het antwoord is als volgt.

```
<ArrayOfGetTranslationsResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
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

Elke `GetTranslationsResponse` element bevat de volgende waarden:

* `Translations`: Er is een matrix van overeenkomsten gevonden, opgeslagen in `TranslationMatch` objecten (Zie hieronder). De vertalingen kunnen lichte varianten van de oorspronkelijke tekst (fuzzy overeenkomstig) bevatten. De vertalingen worden gesorteerd: 100% overeenkomt met eerst Gedeeltelijke matches hieronder.
* `From`: Als de methode niet opgegeven is. een `From` taal, dit is het resultaat van de taal wordt automatisch gedetecteerd. Anders worden de opgegeven taal.
* `State`: Status gebruiker om te vergelijken aanvraag en -antwoord. Bevat de dezelfde waarde die is opgegeven in de `TranslateOptions` parameter.

`TranslationMatch` object bestaat uit het volgende:
* `Error`: Als er een fout opgetreden voor een specifieke invoerreeks, is de foutcode opgeslagen. Anders is het veld leeg.
* `MatchDegree`: Het systeem komt overeen met invoer zinnen tegen de store, zoals onnauwkeurig overeenkomsten.  `MatchDegree` Hiermee wordt aangegeven hoe de invoertekst overeenkomt met de oorspronkelijke tekst in het archief gevonden. De waarde geretourneerd bereiken tussen 0 en 100, waarbij 0 is geen overeenkomsten en 100 een exacte overeenkomst hoofdlettergevoelig is.
* `MatchedOriginalText`: De oorspronkelijke tekst die voor dit resultaat, is gekoppeld. Alleen geretourneerd als de oorspronkelijke tekst komt niet overeen met de ingevoerde tekst. Gebruikt om de brontekst van een algoritmecode te retourneren. Geen Microsoft Translator resultaten geretourneerd.
* `Rating`: Hiermee geeft u de instantie van de persoon die de beslissing kwaliteit. Resultaten van automatische vertalingen hebben een beoordeling van 5. Anoniem opgegeven vertalingen hebben doorgaans een classificatie van 1 tot en met 4 terwijl bindend opgegeven vertalingen doorgaans een classificatie van 6 tot en met 10 hebben.
* `Count`: Het aantal keren dat deze vertaling met deze classificatie is geselecteerd. De waarde gelijk aan 0 voor het automatisch vertaalde antwoord.
* `TranslatedText`: De vertaalde tekst.


### <a name="response-class-status-200"></a>Antwoord klasse (Status 200)

tekenreeks

Type van de inhoud van de reactie: application/xml
 
### <a name="parameters"></a>Parameters

|Parameter|Waarde|Beschrijving|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|Autorisatie  |(leeg)    |Vereist als de `appid` veld of `Ocp-Apim-Subscription-Key` -header is niet opgegeven. Verificatietoken: `"Bearer" + " " + "access_token"`.|koptekst|tekenreeks|
|OCP-Apim-Subscription-Key|(leeg)  |Vereist als de `appid` veld of `Authorization` -header is niet opgegeven.|koptekst|tekenreeks|

### <a name="response-messages"></a>Antwoordberichten

|HTTP-statuscode|Reden|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties|
|500    |Serverfout. Als de fout zich blijft voordoen, laat ons weten. Geef ons met de geschatte datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat ons weten als de fout zich blijft voordoen.|

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Migreren naar v3 conversieprogramma tekst API](../migrate-to-v3.md)










