---
title: Translator Text-API V2.0
titleSuffix: Azure Cognitive Services
description: Referentiedocumentatie voor de V2.0 Translator Text-API.
services: cognitive-services
author: Jann-Skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 05/15/2018
ms.author: v-jansko
ms.openlocfilehash: f8d57723f6e51fb392e4fdbfb2b2a445d48635e3
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55861711"
---
# <a name="translator-text-api-v20"></a>Translator Text-API v2.0

> [!IMPORTANT]
> Deze versie van de Translator Text-API is afgeschaft. [Documentatie voor v3 van de Translator Text-API bekijken](v3-0-reference.md).

Translator Text-API V2 kunnen naadloos worden geïntegreerd in uw toepassingen, websites, hulpprogramma's of andere oplossingen voor meerdere talen gebruikerservaringen. Gebruik van industriële standaarden, kan deze worden gebruikt op elk hardwareplatform en met elk besturingssysteem om uit te voeren vertaalservices en andere bewerkingen met betrekking tot taal, zoals taaldetectie tekst of tekst naar spraak. Klik hier voor meer informatie over de Microsoft Translator-API.

## <a name="getting-started"></a>Aan de slag
Voor toegang tot de Translator Text-API u moet [zich registreren voor Microsoft Azure](../translator-text-how-to-signup.md).

## <a name="authorization"></a>Autorisatie
Aanroepen naar de Translator Text-API is een abonnementssleutel voor verificatie vereist. De API ondersteunt twee modi van verificatie:

* Met behulp van een toegangstoken. Gebruik de abonnementssleutel waarnaar wordt verwezen in **stap** 9 voor het genereren van een toegangstoken door een POST-aanvraag naar de autorisatieservice. Zie de documentatie van de service voor beveiligingstokens voor meer informatie. Het toegangstoken doorgeven aan de Translator-service met behulp van de autorisatie-header of de access_token-queryparameter. Het toegangstoken is geldig voor 10 minuten. Een nieuw toegangstoken verkrijgen om de 10 minuten, en houd met behulp van dezelfde toegang token voor herhaalde aanvragen binnen deze 10 minuten.

* Met behulp van een abonnementssleutel rechtstreeks. Uw abonnementssleutel doorgegeven als een waarde in `Ocp-Apim-Subscription-Key` header inbegrepen bij uw aanvraag voor de API van Translator. In deze modus hoeft u niet om aan te roepen van het token authentication-service voor het genereren van een toegangstoken.

Houd rekening met uw abonnementssleutel en het toegangstoken als geheimen die moeten worden verborgen.

## <a name="profanity-handling"></a>Grof taalgebruik verwerken
De Translator-service blijft normaal grof taalgebruik dat aanwezig is in de bron in de vertaling. De mate van grof taalgebruik en de context waarmee woorden grof verschillen tussen culturen en als gevolg hiervan de mate van grof taalgebruik in de taal van het doel kan worden versterkt of verminderd.

Als u wilt vermijden grof taalgebruik in de vertaling, ongeacht de aanwezigheid van grof taalgebruik in de brontekst, kunt u de optie voor de methoden die ondersteuning bieden voor het filteren taalgebruik. De optie kunt u kiezen of u wilt zien grof taalgebruik verwijderd of gemarkeerd met de juiste tags of geen actie ondernomen. De geaccepteerde waarden van `ProfanityAction` zijn `NoAction` (standaard), gemarkeerd en `Deleted`.


|ProfanityAction    |Bewerking |Voorbeeld van de bron (Japans)  |Voorbeeld van de vertaling (Engels)  |
|:--|:--|:--|:--|
|NoAction   |Standaard. Hetzelfde als de instelling van de optie. Grof taalgebruik geeft van bron naar doel.        |彼はジャッカスです。     |Hij is een jackass.   |
|Gemarkeerd     |Grof woorden omringd door de XML-tags <profanity> en </profanity>.     |彼はジャッカスです。 |Hij is een <profanity>jackass</profanity>.    |
|Deleted    |Grof woorden worden verwijderd uit de uitvoer zonder vervanging.     |彼はジャッカスです。 |Hij is een.   |

    
## <a name="excluding-content-from-translation"></a>Met uitzondering van de inhoud van de vertaling
Tijdens het vertalen van inhoud met zoals HTML-tags (`contentType=text/html`), is het soms nuttig voor het uitsluiten van specifieke inhoud van de vertaling. U kunt het kenmerk `class=notranslate` om op te geven van inhoud die moet blijven in de oorspronkelijke taal. In het volgende voorbeeld wordt de inhoud van de eerste `div` element wordt niet omgezet, terwijl de inhoud in de tweede `div` element wordt omgezet.

```HTML
<div class="notranslate">This will not be translated.</div>
<div>This will be translated. </div>
```

## <a name="get-translate"></a>OPHALEN / vertalen

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Zet een tekenreeks met tekst uit één taal naar een andere.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/Translate`.

**Geretourneerde waarde:** Een tekenreeks met de vertaalde tekst.

Als u eerder hebt gebruikt `AddTranslation` of `AddTranslationArray` in te voeren een vertaling met een classificatie van 5 of hoger voor de zin dezelfde bron `Translate` retourneert alleen de eerste keuze die beschikbaar is voor uw systeem. "Dezelfde bron zin" betekent precies hetzelfde (100%-koppeling), met uitzondering van hoofdletters, witruimte tagwaarden en leestekens aan het einde van een zin. Als er geen beoordeling is opgeslagen met een classificatie van 5 of hoger wordt de automatische vertaling met Microsoft Translator worden op de geretourneerde resultaten.

### <a name="response-class-status-200"></a>Antwoord-klasse (Status 200)

string

Type van de inhoud van de reactie: application/xml 

### <a name="parameters"></a>Parameters

|Parameter|Value|Description    |Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|toepassings-id  |(leeg)    |Vereist. Als de autorisatie of Ocp-Apim-Subscription-Key-header wordt gebruikt, laat de toepassings-id-veld leeg anders voegt u een tekenreeks met 'Bearer' + ' ' + 'access_token'.|query|string|
|tekst|(leeg)   |Vereist. Een tekenreeks die de tekst te vertalen vertegenwoordigt. De grootte van de tekst mag niet groter zijn dan 10000 tekens.|query|string|
|uit|(leeg)   |Optioneel. Een tekenreeks die de taalcode van de vertaalde tekst. Bijvoorbeeld, en voor Engels.|query|string|
|tot|(leeg) |Vereist. Een tekenreeks die de taalcode voor de omzetting van de tekst in.|query|string|
|contentType|(leeg)    |Optioneel. De indeling van de tekst wordt vertaald. De ondersteunde indelingen zijn text/plain (standaard) en text/html. HTML-code moet een juist opgemaakte en volledige-element.|query|string|
|category|(leeg)   |Optioneel. Een tekenreeks met de categorie (domein) van de vertaling. Standaard ingesteld op 'Algemeen'.|query|string|
|Autorisatie|(leeg)  |Vereist als de toepassings-id-veld of de Ocp-Apim-Subscription-Key-header niet is opgegeven. Autorisatietoken:  "Bearer" + " " + "access_token".|koptekst|string|
|OCP-Apim-Subscription-Key|(leeg)  |Vereist als het App-id-veld of de autorisatie-header niet is opgegeven.|koptekst|string|


### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reden|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en met de aanvraag-ID die is opgenomen in de antwoord-header X-MS-Trans-Info.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="post-translatearray"></a>POST /TranslateArray

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Gebruik de `TranslateArray` methode voor het ophalen van vertalingen voor meerdere bron teksten.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/TranslateArray`.

De indeling van de hoofdtekst van de aanvraag moet als volgt:

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


* `appid`: Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat het veld appid leeg anders bevatten een tekenreeks met `"Bearer" + " " + "access_token"`.
* `from`: Optioneel. Een tekenreeks die de taalcode voor de omzetting van de tekst van vertegenwoordigt. Als u niets opgeeft wordt het antwoord het resultaat van de automatische taaldetectie bevatten.
* `options`: Optioneel. Een `Options` object met de onderstaande waarden. Ze zijn optioneel en standaard naar de meest voorkomende instellingen. Opgegeven elementen moeten worden vermeld in alfabetische volgorde.
    - `Category`: Een tekenreeks met de categorie (domein) van de vertaling. Standaard ingesteld op `general`.
    - `ContentType`: De indeling van de tekst wordt vertaald. De ondersteunde indelingen zijn `text/plain` (standaard), `text/xml` en `text/html`. HTML-code moet een juist opgemaakte en volledige-element.
    - `ProfanityAction`: Hiermee geeft u op hoe profanities worden verwerkt, zoals hierboven is uitgelegd. Geaccepteerde waarden van `ProfanityAction` zijn `NoAction` (standaard), `Marked` en `Deleted`.
    - `State`: De status van de gebruiker om te vergelijken aanvraag en antwoord. Dezelfde inhoud in het antwoord geretourneerd.
    - `Uri`: Filter de resultaten op basis van deze URI. Standaard: `all`.
    - `User`: Filter de resultaten door deze gebruiker. Standaard: `all`.
* `texts`: Vereist. Een matrix met de tekst voor de vertaling. Alle tekenreeksen moeten van dezelfde taal. Het totaal van alle teksten worden omgezet mag niet groter zijn dan 10000 tekens. Het maximum aantal matrixelementen is 2000.
* `to`: Vereist. Een tekenreeks die de taalcode voor de omzetting van de tekst in.

Optionele elementen kunnen worden weggelaten. Elementen die directe onderliggende van TranslateArrayRequest elementen moeten worden vermeld in alfabetische volgorde.

Methode TranslateArray accepteert `application/xml` of `text/xml` voor `Content-Type`.

**Geretourneerde waarde:** Een `TranslateArrayResponse` matrix. Elke `TranslateArrayResponse` heeft de volgende elementen:

* `Error`: Geeft een fout als een is opgetreden. Anders is ingesteld op null.
* `OriginalSentenceLengths`: Een matrix van gehele getallen van de lengte van elke zin in de oorspronkelijke brontekst waarmee wordt aangegeven. De lengte van de matrix geeft het aantal zinnen.
* `TranslatedText`: De vertaalde tekst.
* `TranslatedSentenceLengths`: Een matrix van gehele getallen van de lengte van elke zin in de vertaalde tekst waarmee wordt aangegeven. De lengte van de matrix geeft het aantal zinnen.
* `State`: De status van de gebruiker om te vergelijken aanvraag en antwoord. Retourneert de dezelfde inhoud zoals in de aanvraag.

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

### <a name="response-class-status-200"></a>Antwoord-klasse (Status 200)
Een geslaagde reactie bevat een matrix van `TranslateArrayResponse` in de indeling die hierboven worden beschreven.

string

Type van de inhoud van de reactie: application/xml

### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|Autorisatie|(empty)) |Vereist als de toepassings-id-veld of de Ocp-Apim-Subscription-Key-header niet is opgegeven. Autorisatietoken:  "Bearer" + " " + "access_token".|koptekst|string|
|OCP-Apim-Subscription-Key|(leeg)|Vereist als het App-id-veld of de autorisatie-header niet is opgegeven.|koptekst|string|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode   |Reden|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht. Veelvoorkomende fouten zijn onder andere: <ul><li>Matrixelement mag niet leeg zijn</li><li>Ongeldige categorie</li><li>Van een taal is ongeldig</li><li>Taal is ongeldig</li><li>De aanvraag bevat te veel elementen</li><li>De taal van wordt niet ondersteund</li><li>De taal aan wordt niet ondersteund</li><li>Vertalen aanvraag heeft te veel gegevens</li><li>HTML-code is niet de juiste indeling</li><li>Te veel tekenreeksen zijn doorgegeven in de vertalen aanvragen</li></ul>|
|401    |Ongeldige referenties|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en met de aanvraag-ID die is opgenomen in de antwoord-header X-MS-Trans-Info.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="post-getlanguagenames"></a>POST /GetLanguageNames

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Beschrijvende namen voor de talen die worden opgehaald als de parameter doorgegeven `languageCodes`, en een gelokaliseerde met behulp van de taal van de doorgegeven landinstelling.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/GetLanguageNames`.

De aanvraagtekst bevat een string-matrix die de ISO 639-1-taal-codes om op te halen van de beschrijvende namen voor vertegenwoordigt. Bijvoorbeeld:

```
<ArrayOfstring xmlns:i="http://www.w3.org/2001/XMLSchema-instance"  xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>zh</string>
    <string>en</string>
</ArrayOfstring>
```

**Geretourneerde waarde:** Een string-matrix met de namen van talen ondersteund door de Translator-Service, in de gevraagde taal is gelokaliseerd.

### <a name="response-class-status-200"></a>Antwoord-klasse (Status 200)
Een string-matrix met de namen van talen ondersteund door de Translator-Service, in de gevraagde taal is gelokaliseerd.

string

Type van de inhoud van de reactie: application/xml
 
### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|toepassings-id|(leeg)|Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat het veld appid leeg anders bevatten een tekenreeks met `"Bearer" + " " + "access_token"`.|query|string|
|landinstelling|(leeg) |Vereist. Een tekenreeks die een combinatie van een ISO 639 kleine cultuur van twee letters-code die is gekoppeld aan een taal en een code van de twee letters, hoofdletters subcultuur ISO 3166 voor lokalisatie van de taalnamen van de of een ISO 639-code in kleine letters cultuur op zichzelf.|query|string|
|Autorisatie|(leeg)  |Vereist als het veld appid of `Ocp-Apim-Subscription-Key` -header is niet opgegeven. Autorisatietoken: `"Bearer" + " " + "access_token"`.|koptekst|string|
|OCP-Apim-Subscription-Key|(leeg)  |Vereist als het veld appid of `Authorization` -header is niet opgegeven.|koptekst|string|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reden|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en met de aanvraag-ID die is opgenomen in de antwoord-header X-MS-Trans-Info.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="get-getlanguagesfortranslate"></a>GET /GetLanguagesForTranslate

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Een lijst met taalcodes die talen die worden ondersteund door de vertaling-Service ophalen.  `Translate` en `TranslateArray` kunt vertalen tussen twee van deze talen.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForTranslate`.

**Geretourneerde waarde:** Een string-matrix met de taalcodes worden ondersteund door de Translator-Services.

### <a name="response-class-status-200"></a>Antwoord-klasse (Status 200)
Een string-matrix met de taalcodes worden ondersteund door de Translator-Services.

string

Type van de inhoud van de reactie: application/xml
 
### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|toepassings-id|(leeg)|Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat het veld appid leeg anders bevatten een tekenreeks met `"Bearer" + " " + "access_token"`.|query|string|
|Autorisatie|(leeg)  |Vereist als de `appid` veld of `Ocp-Apim-Subscription-Key` -header is niet opgegeven. Autorisatietoken: `"Bearer" + " " + "access_token"`.|koptekst|string|
|OCP-Apim-Subscription-Key|(leeg)|Vereist als de `appid` veld of `Authorization` -header is niet opgegeven.|koptekst|string|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reden|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en met de aanvraag-ID die is opgenomen in de antwoord-header X-MS-Trans-Info.|
|503|De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="get-getlanguagesforspeak"></a>/GetLanguagesForSpeak ophalen

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Hiermee worden de talen opgehaald die beschikbaar zijn voor spraaksynthese.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForSpeak`.

**Geretourneerde waarde:** Een string-matrix met de taalcodes voor spraaksynthese wordt ondersteund door de Translator-Service.

### <a name="response-class-status-200"></a>Antwoord-klasse (Status 200)
Een string-matrix met de taalcodes voor spraaksynthese wordt ondersteund door de Translator-Service.

string

Type van de inhoud van de reactie: application/xml

### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|toepassings-id|(leeg)|Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat het veld appid leeg anders bevatten een tekenreeks met `"Bearer" + " " + "access_token"`.|query|string|
|Autorisatie|(leeg)|Vereist als de `appid` veld of `Ocp-Apim-Subscription-Key` -header is niet opgegeven. Autorisatietoken: `"Bearer" + " " + "access_token"`.|koptekst|string|
|OCP-Apim-Subscription-Key|(leeg)|Vereist als de `appid` veld of `Authorization` -header is niet opgegeven.|koptekst|string|
 
### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reden|
|:--|:--|
|400|Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401|Ongeldige referenties|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="get-speak"></a>OPHALEN / spreken

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Retourneert een wave of mp3-stroom van de doorgegeven in-tekst wordt gesproken in de gewenste taal.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/Speak`.

**Geretourneerde waarde:** Een wave of mp3-stroom van de doorgegeven in-tekst wordt gesproken in de gewenste taal.

### <a name="response-class-status-200"></a>Antwoord-klasse (Status 200)

binaire bestanden

Type van de inhoud van de reactie: application/xml 

### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|toepassings-id|(leeg)|Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat het veld appid leeg anders bevatten een tekenreeks met `"Bearer" + " " + "access_token"`.|query|string|
|tekst|(leeg)   |Vereist. Een tekenreeks met een zin of zinnen van de opgegeven taal worden uitgesproken voor de wave-stream. De grootte van de tekst te spreken mag niet groter zijn dan 2000 tekens.|query|string|
|language|(leeg)   |Vereist. Een tekenreeks voor de ondersteunde taalcode die moet worden de tekst in te spreken. De code moet aanwezig zijn in de lijst van codes die zijn geretourneerd door de methode `GetLanguagesForSpeak`.|query|string|
|Indeling|(leeg)|Optioneel. Een tekenreeks die de inhoud van het type id op te geven Op dit moment `audio/wav` en `audio/mp3` beschikbaar zijn. De standaardwaarde is `audio/wav`.|query|string|
|opties|(leeg)    |<ul><li>Optioneel. Een tekenreeks op te geven eigenschappen van de kunstmatige spraak:<li>`MaxQuality` en `MinSize` zijn beschikbaar om op te geven van de kwaliteit van de audio signalen. Met `MaxQuality`, krijgt u stemmen met de hoogste kwaliteit en met `MinSize`, krijgt u de stemmen met de kleinste grootte. De standaardwaarde is `MinSize`.</li><li>`female` en `male` zijn beschikbaar om op te geven van de gewenste geslacht van de stem. De standaardwaarde is `female`. Gebruik de verticale balk `|` naar meerdere opties bevatten. Bijvoorbeeld `MaxQuality|Male`.</li></li></ul> |query|string|
|Autorisatie|(leeg)|Vereist als de `appid` veld of `Ocp-Apim-Subscription-Key` -header is niet opgegeven. Autorisatietoken: `"Bearer" + " " + "access_token"`.|koptekst|string|
|OCP-Apim-Subscription-Key|(leeg)  |Vereist als de `appid` veld of `Authorization` -header is niet opgegeven.|koptekst|string|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reden|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="get-detect"></a>OPHALEN / detecteren

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Gebruik de `Detect` methode voor het identificeren van de taal van een geselecteerde stuk tekst.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/Detect`.

**Geretourneerde waarde:** Een tekenreeks met een taalcode van twee tekens voor de opgegeven tekst. .

### <a name="response-class-status-200"></a>Antwoord-klasse (Status 200)

string

Type van de inhoud van de reactie: application/xml

### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|toepassings-id|(leeg)  |Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat het veld appid leeg anders bevatten een tekenreeks met `"Bearer" + " " + "access_token"`.|query|string|
|tekst|(leeg)|Vereist. Een tekenreeks met tekst waarvan de taal is kan worden geïdentificeerd. De grootte van de tekst mag niet groter zijn dan 10000 tekens.|query| string|
|Autorisatie|(leeg)|Vereist als de `appid` veld of `Ocp-Apim-Subscription-Key` -header is niet opgegeven. Autorisatietoken: `"Bearer" + " " + "access_token"`.|koptekst|string|
|OCP-Apim-Subscription-Key  |(leeg)    |Vereist als de `appid` veld of `Authorization` -header is niet opgegeven.|koptekst|string|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reden|
|:--|:--|
|400|Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|


## <a name="post-detectarray"></a>/DetectArray plaatsen

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Gebruik de `DetectArray` methode voor het identificeren van de taal van een matrix van tekenreeks in één keer. Onafhankelijke detectie van elke afzonderlijke matrixelement voert en retourneert een resultaat voor elke rij van de matrix.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/DetectArray`.

De indeling van de hoofdtekst van de aanvraag moet als volgt.

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>string-value-1</string>
    <string>string-value-2</string>
</ArrayOfstring>
```

De grootte van de tekst mag niet groter zijn dan 10000 tekens.

**Geretourneerde waarde:** Een string-matrix met een taalcodes twee tekens voor elke rij van de invoermatrix.

De indeling van de antwoordtekst is als volgt.

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
  <string>language-code-1</string>
  <string>language-code-2</string>
</ArrayOfstring>
```

### <a name="response-class-status-200"></a>Antwoord-klasse (Status 200)
DetectArray is voltooid. Retourneert een string-matrix met een taalcodes twee tekens voor elke rij van de invoermatrix.

string

Type van de inhoud van de reactie: application/xml
 
### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|toepassings-id|(leeg)|Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat het veld appid leeg anders bevatten een tekenreeks met `"Bearer" + " " + "access_token"`.|query|string|
|Autorisatie|(leeg)|Vereist als de `appid` veld of `Ocp-Apim-Subscription-Key` -header is niet opgegeven. Autorisatietoken: `"Bearer" + " " + "access_token"`.|koptekst|string|
|OCP-Apim-Subscription-Key|(leeg)|Vereist als de `appid` veld of de autorisatie-header is niet opgegeven.|koptekst|string|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reden|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en met de aanvraag-ID die is opgenomen in de antwoord-header X-MS-Trans-Info.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="get-addtranslation"></a>/AddTranslation ophalen

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie

> [!IMPORTANT]
> **AFSCHAFFING OPMERKING:** Deze methode accepteert geen nieuwe zin inzendingen na 31 januari 2018 en ontvangt u een foutbericht weergegeven. Raadpleeg deze aankondiging over wijzigingen in de Collaborative Translation-functies.

Een vertaling toevoegen aan de NAT-geheugen.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/AddTranslation`.

### <a name="response-class-status-200"></a>Antwoord-klasse (Status 200)

string

Type van de inhoud van de reactie: aanvraag: xml
 
### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype   |
|:--|:--|:--|:--|:--|
|toepassings-id|(leeg)|Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat het veld appid leeg anders bevatten een tekenreeks met `"Bearer" + " " + "access_token"`.|query|string|
|originalText|(leeg)|Vereist. Een tekenreeks met de tekst voor de omzetting van. De tekenreeks heeft een maximale lengte van 1000 tekens.|query|string|
|translatedText|(leeg) |Vereist. Een tekenreeks met vertaald tekst in de doel-taal. De tekenreeks heeft een maximale lengte van 2000 tekens.|query|string|
|uit|(leeg)   |Vereist. Een tekenreeks die de taalcode van de vertaalde tekst. NL = Engels, de Duitse enzovoort =...|query|string|
|tot|(leeg)|Vereist. Een tekenreeks die de taalcode voor de omzetting van de tekst in.|query|string|
|rating|(leeg) |Optioneel. Een geheel getal dat aangeeft van de beoordeling van kwaliteit voor deze tekenreeks. De waarde tussen 10 en 10. Standaard ingesteld op 1.|query|geheel getal|
|contentType|(leeg)    |Optioneel. De indeling van de tekst wordt vertaald. De ondersteunde indelingen zijn ' text/plain' en ' text/html'. HTML-code moet een juist opgemaakte en volledige-element.   |query|string|
|category|(leeg)|Optioneel. Een tekenreeks met de categorie (domein) van de vertaling. Standaard ingesteld op 'Algemeen'.|query|string|
|user|(leeg)|Vereist. Een tekenreeks die wordt gebruikt voor het bijhouden van de afzender van de inzending.|query|string|
|uri|(leeg)|Optioneel. Een tekenreeks met de locatie van de inhoud van deze omzetting.|query|string|
|Autorisatie|(leeg)|Vereist als het veld appid of `Ocp-Apim-Subscription-Key` -header is niet opgegeven. Autorisatietoken: `"Bearer" + " " + "access_token"`.    |koptekst|string|
|OCP-Apim-Subscription-Key|(leeg)|Vereist als de `appid` veld of `Authorization` -header is niet opgegeven.|koptekst|string|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reden|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties|
|410|AddTranslation wordt niet meer ondersteund.|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en met de aanvraag-ID die is opgenomen in de antwoord-header X-MS-Trans-Info.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="post-addtranslationarray"></a>/AddTranslationArray plaatsen

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie

> [!IMPORTANT]
> **AFSCHAFFING OPMERKING:** Deze methode accepteert geen nieuwe zin inzendingen na 31 januari 2018 en ontvangt u een foutbericht weergegeven. Raadpleeg deze aankondiging over wijzigingen in de Collaborative Translation-functies.

Voegt een matrix van vertalingen maken in het geheugen voor vertaling toevoegen. Dit is de versie van een matrix van `AddTranslation`.

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

* `AppId`: Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat het veld appid leeg anders bevatten een tekenreeks met `"Bearer" + " " + "access_token"`.
* `From`: Vereist. Een tekenreeks met de taalcode van de source-taal. U moet een van de talen die zijn geretourneerd door de `GetLanguagesForTranslate` methode.
* `To`: Vereist. Een tekenreeks met de taalcode van de doel-taal. U moet een van de talen die zijn geretourneerd door de `GetLanguagesForTranslate` methode.
* `Translations`: Vereist. Een matrix van vertalingen om toe te voegen in het geheugen van de vertaling. Elke omzetting moet bevatten: originalText, translatedText en classificatie. De grootte van elke originalText en translatedText is beperkt tot 1000 tekens. Het totaal van alle originalText(s) en translatedText(s) mag niet groter zijn dan 10000 tekens. Het maximum aantal matrixelementen is 100.
* `Options`: Vereist. Een set met opties, met inbegrip van categorie, ContentType, Uri en gebruiker. Gebruiker is vereist. Categorie, ContentType en Uri zijn optioneel. Opgegeven elementen moeten worden vermeld in alfabetische volgorde.

### <a name="response-class-status-200"></a>Antwoord-klasse (Status 200)
AddTranslationArray methode is voltooid. Na 31 januari 2018 worden geen zin inzendingen geaccepteerd. De service reageert met foutcode 410.

string

Type van de inhoud van de reactie: application/xml
 
### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|Autorisatie|(leeg)|Vereist als de toepassings-id-veld of de Ocp-Apim-Subscription-Key-header niet is opgegeven. Autorisatietoken:  "Bearer" + " " + "access_token".|koptekst|string|
|OCP-Apim-Subscription-Key|(leeg)|Vereist als het App-id-veld of de autorisatie-header niet is opgegeven.|koptekst|string|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reden|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties|
|410    |AddTranslation wordt niet meer ondersteund.|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503|De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="get-breaksentences"></a>/BreakSentences ophalen

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Neemt een stuk tekst in zinnen en retourneert een matrix met de lengte in elke zin.

De aanvraag-URI is `https://api.microsofttranslator.com/V2/Http.svc/BreakSentences`.

**Geretourneerde waarde:** Een matrix met gehele getallen die de lengte van de zinnen. De lengte van de matrix is het aantal zinnen en de waarden worden de lengte van elke zin.

### <a name="response-class-status-200"></a>Antwoord-klasse (Status 200)
Een matrix met gehele getallen die de lengte van de zinnen. De lengte van de matrix is het aantal zinnen en de waarden worden de lengte van elke zin.

geheel getal

Type van de inhoud van de reactie: application/xml 

### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|toepassings-id|(leeg)  |Vereist. Als de autorisatie of Ocp-Apim-Subscription-Key-header wordt gebruikt, laat de toepassings-id-veld leeg anders voegt u een tekenreeks met 'Bearer' + ' ' + 'access_token'.|query| string|
|tekst|(leeg)   |Vereist. Een tekenreeks die de tekst om te splitsen in zinnen aangeeft. De grootte van de tekst mag niet groter zijn dan 10000 tekens.|query|string|
|language   |(leeg)    |Vereist. Een tekenreeks die de taalcode van invoertekst vertegenwoordigt.|query|string|
|Autorisatie|(leeg)|Vereist als de toepassings-id-veld of de Ocp-Apim-Subscription-Key-header niet is opgegeven. Autorisatietoken:  "Bearer" + " " + "access_token".    |koptekst|string|
|OCP-Apim-Subscription-Key|(leeg)|Vereist als het App-id-veld of de autorisatie-header niet is opgegeven.|koptekst|string|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reden|
|:--|:--|
|400|Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401|Ongeldige referenties|
|500|Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en met de aanvraag-ID die is opgenomen in de antwoord-header X-MS-Trans-Info.|
|503|De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="post-gettranslations"></a>POST /GetTranslations

### <a name="implementation-notes"></a>Opmerkingen bij de implementatie
Een matrix van vertalingen voor een bepaalde taalpaar opgehaald uit de store en de MT-engine. GetTranslations wijkt af van vertalen als het resultaat van alle beschikbare vertalingen.

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

De `TranslateOptions` -object bevat de onderstaande waarden. Ze zijn optioneel en standaard naar de meest voorkomende instellingen. Opgegeven elementen moeten worden vermeld in alfabetische volgorde.

* `Category`: Een tekenreeks met de categorie (domein) van de vertaling. Standaard ingesteld op 'Algemeen'.
* `ContentType`: De enige ondersteunde, en de standaard, optie ' text/plain'.
* `IncludeMultipleMTAlternatives`: Booleaanse vlag om te bepalen of meer dan één alternatieven moeten worden geretourneerd van de MT-engine. Geldige waarden zijn true en false (hoofdlettergevoelig). Standaard is ingesteld op false en slechts 1 alternatief bevat. Kunt u de vlag instellen op ' True ' voor het genereren van kunstmatige alternatieven vertaling volledig geïntegreerd met het gezamenlijke vertalingen framework (CTF). De functie kunt voor het retourneren van alternatieven voor zinnen waarvoor geen alternatieven in CTF, door kunstmatige alternatieven uit de n-beste lijst van de decoder toe te voegen.
    - Classificaties de classificaties zijn als volgt toegepast: (1) het beste voor automatische vertaling heeft een classificatie van 5. 2) de alternatieven van CTF weerspiegelen de instantie van de revisor van-10 op + 10. 3) de automatisch gegenereerde (n-beste) vertaling alternatieven hebben een classificatie van 0 en de mate van een overeenkomst van 100.
    - Het aantal alternatieven voor het aantal geretourneerde alternatieven is maximaal maxTranslations, maar kan kleiner zijn.
    - Taal paren deze functionaliteit is niet beschikbaar voor vertalingen tussen vereenvoudigd en traditioneel Chinees, beide richtingen. Het is beschikbaar voor alle andere paren van Microsoft Translator-ondersteunde taal.
* `State`: De status van de gebruiker om te vergelijken aanvraag en antwoord. Dezelfde inhoud in het antwoord geretourneerd.
* `Uri`: Filter de resultaten op basis van deze URI. Als er geen waarde is ingesteld, is de standaardinstelling is alles.
* `User`: Filter de resultaten door deze gebruiker. Als er geen waarde is ingesteld, is de standaardinstelling is alles.

Aanvraag `Content-Type` moet `text/xml`.

**Geretourneerde waarde:** De indeling van het antwoord is als volgt.

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

* `Translations`: Een matrix van overeenkomsten gevonden, opgeslagen in objecten TranslationMatch (Zie hieronder). De vertalingen, omvat mogelijk enigszins varianten van de oorspronkelijke tekst (fuzzy-koppeling). De vertalingen worden gesorteerd: 100% komt overeen met eerst fuzzy overeenkomsten hieronder.
* `From`: Als de methode een From-taal niet opgeeft heeft, is dit het resultaat van de automatische taaldetectie. Anders worden de opgegeven taal.
* `State`: De status van de gebruiker om te vergelijken aanvraag en antwoord. Bevat de dezelfde waarde als opgegeven in de parameter TranslateOptions.

TranslationMatch object bestaat uit het volgende:

* `Error`: Als er een fout voor een specifieke invoerreeks opgetreden is, wordt de foutcode die zijn opgeslagen. Anders wordt is het veld leeg.
* `MatchDegree`: Het systeem komt overeen met invoer zinnen op basis van de store, zoals onnauwkeurig resultaten.  MatchDegree geeft aan hoe de invoertekst overeenkomt met de oorspronkelijke tekst gevonden in het archief. De waarde geretourneerd bereiken tussen 0 en 100, waarbij 0 is geen overeenkomsten en 100 een exacte overeenkomst hoofdlettergevoelig is.
MatchedOriginalText: Oorspronkelijke tekst die voor dit resultaat overeen. Alleen geretourneerd als de oorspronkelijke overeenkomende tekst anders dan de invoertekst is. Gebruikt om de brontekst van een benadering te retourneren. Geen Microsoft Translator-resultaten geretourneerd.
* `Rating`: Geeft aan dat de instantie van de persoon die het besluit kwaliteit. Automatische vertaling resultaten heeft een classificatie van 5. Anoniem opgegeven vertalingen hebben doorgaans een classificatie van 1 tot en met 4, terwijl bindend opgegeven vertalingen over het algemeen een classificatie van 6 tot en met 10 hebben wordt.
* `Count`: Het aantal keren dat die deze tekst met deze beoordeling is geselecteerd. De waarde is 0 voor het automatisch vertaalde antwoord.
* `TranslatedText`: De vertaalde tekst.

### <a name="response-class-status-200"></a>Antwoord-klasse (Status 200)
Een `GetTranslationsResponse` -object in de indeling die hierboven worden beschreven.

string

Type van de inhoud van de reactie: application/xml
 
### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|toepassings-id|(leeg)|Vereist. Als de `Authorization` of `Ocp-Apim-Subscription-Key` header wordt gebruikt, laat het veld appid leeg anders bevatten een tekenreeks met `"Bearer" + " " + "access_token"`.|query|string|
|tekst|(leeg)|Vereist. Een tekenreeks die de tekst te vertalen vertegenwoordigt. De grootte van de tekst mag niet groter zijn dan 10000 tekens.|query|string|
|uit|(leeg)|Vereist. Een tekenreeks die de taalcode van de vertaalde tekst.|query|string|
|tot |(leeg)    |Vereist. Een tekenreeks die de taalcode voor de omzetting van de tekst in.|query|string|
|maxTranslations|(leeg)|Vereist. Een integer voor het maximum aantal vertalingen om terug te keren.|query|geheel getal|
|Autorisatie| (leeg)|Vereist als de `appid` veld of `Ocp-Apim-Subscription-Key` -header is niet opgegeven. Autorisatietoken: `"Bearer" + " " + "access_token"`.|string| koptekst|
|OCP-Apim-Subscription-Key|(leeg)  |Vereist als de `appid` veld of `Authorization` -header is niet opgegeven.|koptekst|string|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reden|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503|De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="post-gettranslationsarray"></a>POST /GetTranslationsArray

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

* `AppId`: Vereist. Als autorisatie-header wordt gebruikt, laat het veld appid leeg anders voegt u een tekenreeks met `"Bearer" + " " + "access_token"`.
* `From`: Vereist. Een tekenreeks die de taalcode van de vertaalde tekst.
* `MaxTranslations`: Vereist. Een integer voor het maximum aantal vertalingen om terug te keren.
* `Options`: Optioneel. Een object voor opties die de waarden in de lijst hieronder bevat. Ze zijn optioneel en standaard naar de meest voorkomende instellingen. Opgegeven elementen moeten worden vermeld in alfabetische volgorde.
    - Category`: Een tekenreeks met de categorie (domein) van de vertaling. De standaardwaarde is general.
    - `ContentType`: De enige ondersteunde, en de standaard optie text/plain.
    - `IncludeMultipleMTAlternatives`: Booleaanse vlag om te bepalen of meer dan één alternatieven moeten worden geretourneerd van de MT-engine. Geldige waarden zijn true en false (hoofdlettergevoelig). Standaard is ingesteld op false en slechts 1 alternatief bevat. Kunt u de vlag instellen op ' True ' voor het genereren van kunstmatige alternatieven vertaling volledig geïntegreerd met het gezamenlijke vertalingen framework (CTF). De functie kunt voor het retourneren van alternatieven voor zinnen waarvoor geen alternatieven in CTF, door kunstmatige alternatieven uit de n-beste lijst van de decoder toe te voegen.
        - Classificaties de classificaties zijn als volgt toegepast: (1) het beste voor automatische vertaling heeft een classificatie van 5. 2) de alternatieven van CTF weerspiegelen de instantie van de revisor van-10 op + 10. 3) de automatisch gegenereerde (n-beste) vertaling alternatieven hebben een classificatie van 0 en de mate van een overeenkomst van 100.
        - Het aantal alternatieven voor het aantal geretourneerde alternatieven is maximaal maxTranslations, maar kan kleiner zijn.
        - Taal paren deze functionaliteit is niet beschikbaar voor vertalingen tussen vereenvoudigd en traditioneel Chinees, beide richtingen. Het is beschikbaar voor alle andere paren van Microsoft Translator-ondersteunde taal.
* `State`: De status van de gebruiker om te vergelijken aanvraag en antwoord. Dezelfde inhoud in het antwoord geretourneerd.
* `Uri`: Filter de resultaten op basis van deze URI. Als er geen waarde is ingesteld, is de standaardinstelling is alles.
* `User`: Filter de resultaten door deze gebruiker. Als er geen waarde is ingesteld, is de standaardinstelling is alles.
* `Texts`: Vereist. Een matrix met de tekst voor de vertaling. Alle tekenreeksen moeten van dezelfde taal. Het totaal van alle teksten worden omgezet mag niet groter zijn dan 10000 tekens. Het maximum aantal matrixelementen is 10.
* `To`: Vereist. Een tekenreeks die de taalcode voor de omzetting van de tekst in.

Optionele elementen kunnen worden weggelaten. Elementen die zijn direct onderliggende objecten van `GetTranslationsArrayRequest` moet worden vermeld in alfabetische volgorde.

Aanvraag `Content-Type` moet `text/xml`.

**Geretourneerde waarde:** De indeling van het antwoord is als volgt.

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

* `Translations`: Een matrix van overeenkomsten gevonden, die zijn opgeslagen in `TranslationMatch` objecten (Zie hieronder). De vertalingen, omvat mogelijk enigszins varianten van de oorspronkelijke tekst (fuzzy-koppeling). De vertalingen worden gesorteerd: 100% komt overeen met eerst fuzzy overeenkomsten hieronder.
* `From`: Als de methode niet hebt opgegeven een `From` taal, dit is het resultaat van de automatische taaldetectie. Anders worden de opgegeven taal.
* `State`: De status van de gebruiker om te vergelijken aanvraag en antwoord. Bevat de dezelfde waarde als opgegeven in de `TranslateOptions` parameter.

`TranslationMatch` object bestaat uit het volgende:
* `Error`: Als er een fout voor een specifieke invoerreeks opgetreden is, wordt de foutcode die zijn opgeslagen. Anders wordt is het veld leeg.
* `MatchDegree`: Het systeem komt overeen met invoer zinnen op basis van de store, zoals onnauwkeurig resultaten.  `MatchDegree` Geeft aan hoe de invoertekst overeenkomt met de oorspronkelijke tekst gevonden in het archief. De waarde geretourneerd bereiken tussen 0 en 100, waarbij 0 is geen overeenkomsten en 100 een exacte overeenkomst hoofdlettergevoelig is.
* `MatchedOriginalText`: Oorspronkelijke tekst die voor dit resultaat overeen. Alleen geretourneerd als de oorspronkelijke overeenkomende tekst anders dan de invoertekst is. Gebruikt om de brontekst van een benadering te retourneren. Geen Microsoft Translator-resultaten geretourneerd.
* `Rating`: Geeft aan dat de instantie van de persoon die het besluit kwaliteit. Automatische vertaling resultaten heeft een classificatie van 5. Anoniem opgegeven vertalingen hebben doorgaans een classificatie van 1 tot en met 4, terwijl bindend opgegeven vertalingen over het algemeen een classificatie van 6 tot en met 10 hebben wordt.
* `Count`: Het aantal keren dat die deze tekst met deze beoordeling is geselecteerd. De waarde is 0 voor het automatisch vertaalde antwoord.
* `TranslatedText`: De vertaalde tekst.


### <a name="response-class-status-200"></a>Antwoord-klasse (Status 200)

string

Type van de inhoud van de reactie: application/xml
 
### <a name="parameters"></a>Parameters

|Parameter|Value|Description|Parametertype|Gegevenstype|
|:--|:--|:--|:--|:--|
|Autorisatie  |(leeg)    |Vereist als de `appid` veld of `Ocp-Apim-Subscription-Key` -header is niet opgegeven. Autorisatietoken: `"Bearer" + " " + "access_token"`.|koptekst|string|
|OCP-Apim-Subscription-Key|(leeg)  |Vereist als de `appid` veld of `Authorization` -header is niet opgegeven.|koptekst|string|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reden|
|:--|:--|
|400    |Ongeldige aanvraag. Controleer de invoerparameters en het gedetailleerde foutbericht.|
|401    |Ongeldige referenties|
|500    |Server-fout. Als de fout zich blijft voordoen, laat het ons weten. Geef ons met de bij benadering de datum en tijd van de aanvraag en de aanvraag-ID die is opgenomen in de antwoordheader `X-MS-Trans-Info`.|
|503    |De service is tijdelijk niet beschikbaar. Probeer het opnieuw en laat het ons weten als de fout zich blijft voordoen.|

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Migreren naar v3 Translator Text-API](../migrate-to-v3.md)










