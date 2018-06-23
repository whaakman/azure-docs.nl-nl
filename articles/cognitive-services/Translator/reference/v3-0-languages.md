---
title: Microsoft Translator tekst API talen methode | Microsoft Docs
description: Gebruik de talen voor tekst-API van Microsoft Translator-methode.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 93c06218a560faf439f05903438d021b372ce257
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345332"
---
# <a name="text-api-30-languages"></a>Tekst API 3.0: talen

Hiermee wordt het aantal talen momenteel ondersteund door andere bewerkingen van de API van de tekst opgehaald. 

## <a name="request-url"></a>Aanvraag-URL

Verzenden van een `GET` aanvraag voor het:
```HTTP
https://api.cognitive.microsofttranslator.com/languages?api-version=3.0
```

## <a name="request-parameters"></a>Aanvraagparameters

Aanvragen in de queryreeks doorgegeven parameters zijn:

<table width="100%">
  <th width="20%">Queryparameter</th>
  <th>Beschrijving</th>
  <tr>
    <td>API-versie</td>
    <td>*Vereiste parameter*.<br/>De versie van de API die door de client wordt aangevraagd. De waarde moet `3.0`.</td>
  </tr>
  <tr>
    <td>scope</td>
    <td>*Optionele parameter*.<br/>Een door komma's gescheiden lijst met namen van de groep van talen om terug te definiëren. Toegestane namen zijn: `translation`, `transliteration` en `dictionary`. Als er geen bereik is opgegeven, wordt alle groepen worden geretourneerd die gelijk is aan doorgeven `scope=translation,transliteration,dictionary`. Om te bepalen welke set van ondersteunde talen geschikt is voor uw scenario, Zie de beschrijving van de [response-object](#response-body).</td>
  </tr>
</table> 

Aanvraagheaders zijn:

<table width="100%">
  <th width="20%">Headers</th>
  <th>Beschrijving</th>
  <tr>
    <td>Accepteer taal</td>
    <td>*Optionele aanvraagheader*.<br/>De taal die moet worden gebruikt voor tekenreeksen voor interface. Sommige van de velden in het antwoord zijn namen van de regio's of namen van talen. Gebruik deze parameter voor het definiëren van de taal waarin deze namen worden geretourneerd. De taal is opgegeven door een goed ingedeelde BCP 47 taallabel. Gebruik bijvoorbeeld de waarde `fr` aan te vragen van de namen in het Frans of gebruik de waarde `zh-Hant` naar namen van de aanvraag in traditioneel Chinees.<br/>Namen worden opgegeven in de Engelse taal als een doel-taal is niet opgegeven of als de lokalisatie is niet beschikbaar.
    </td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*Optionele aanvraagheader*.<br/>Een client gegenereerde GUID als unieke identificatie van de aanvraag.</td>
  </tr>
</table> 

Verificatie is niet vereist om op te halen taalbestanden.

## <a name="response-body"></a>Antwoordtekst

Een client gebruikt de `scope` queryparameter om te definiëren welke groepen van talen die het geïnteresseerd in.

* `scope=translation` biedt de talen die worden ondersteund voor het vertalen van de tekst van één taal op een andere taal;

* `scope=transliteration` biedt mogelijkheden voor het converteren van tekst in één taal van een script naar een ander script;

* `scope=dictionary` voorziet in paren taal waarvoor `Dictionary` operations gegevens retourneren.

Een client kan meerdere groepen tegelijkertijd ophalen door te geven van een door komma's gescheiden lijst met namen. Bijvoorbeeld: `scope=translation,transliteration,dictionary` ondersteunde talen voor alle groepen wilt retourneren.

Een geslaagde reactie is een JSON-object met een eigenschap voor elke aangevraagde groep:

```json
{
    "translation": {
        //... set of languages supported to translate text (scope=translation)
    },
    "transliteration": {
        //... set of languages supported to convert between scripts (scope=transliteration)
    },
    "dictionary": {
        //... set of languages supported for alternative translations and examples (scope=dictionary)
    }
}
```

De waarde voor elke eigenschap is als volgt.

* `translation` Eigenschap

  De waarde van de `translation` eigenschap is een dictionary van (sleutel, waarde) paren. Elke sleutel is een code van de taal BCP 47. Een sleutel identificeert een taal waarvoor tekst kan worden omgezet in of uit vertaald. De waarde die is gekoppeld aan de sleutel is een JSON-object met eigenschappen die de taal te beschrijven:

  * `name`: De weergavenaam van de taal in de landinstellingen die is aangevraagd `Accept-Language` header.

  * `nativeName`: Weergavenaam van de taal in de oorspronkelijke landinstellingen voor deze taal.

  * `dir`: Richtinggevoeligheid, namelijk `rtl` voor talen van rechts naar links of `ltr` voor talen van links naar rechts.

  Er is een voorbeeld:
          
  ```json
  {
    "translation": {
      ...
      "fr": {
        "name": "French",
        "nativeName": "Français",
        "dir": "ltr"
      },
      ...
    }
  }
  ```

* `transliteration` Eigenschap

  De waarde van de `transliteration` eigenschap is een dictionary van (sleutel, waarde) paren. Elke sleutel is een code van de taal BCP 47. Een sleutel identificeert een taal waarvoor tekst kan worden geconverteerd van een script naar een ander script. De waarde die is gekoppeld aan de sleutel is een JSON-object met eigenschappen die de taal en de bijbehorende ondersteunde scripts wordt beschreven:

  * `name`: De weergavenaam van de taal in de landinstellingen die is aangevraagd `Accept-Language` header.

  * `nativeName`: Weergavenaam van de taal in de oorspronkelijke landinstellingen voor deze taal.

  * `scripts`: Een lijst met scripts te converteren. Elk element van de `scripts` lijst heeft eigenschappen:

    * `code`: Code ter identificatie van het script.

    * `name`: De weergavenaam van het script in de landinstellingen die is aangevraagd `Accept-Language` header.

    * `nativeName`: Weergave de naam van de taal in de oorspronkelijke landinstellingen voor de taal.

    * `dir`: Richtinggevoeligheid, namelijk `rtl` voor talen van rechts naar links of `ltr` voor talen van links naar rechts.

    * `toScripts`: Een lijst met scripts die beschikbaar zijn voor de tekst te converteren. Elk element van de `toScripts` lijst heeft eigenschappen `code`, `name`, `nativeName`, en `dir` zoals eerder beschreven.

  Er is een voorbeeld:

  ```json
  {
    "transliteration": {
      ...
      "ja": {
        "name": "Japanese",
        "nativeName": "日本語",
        "scripts": [
          {
            "code": "Jpan",
            "name": "Japanese",
            "nativeName": "日本語",
            "dir": "ltr",
            "toScripts": [
              {
                "code": "Latn",
                "name": "Latin",
                "nativeName": "ラテン語",
                "dir": "ltr"
              }
            ]
          },
          {
            "code": "Latn",
            "name": "Latin",
            "nativeName": "ラテン語",
            "dir": "ltr",
            "toScripts": [
              {
                "code": "Jpan",
                "name": "Japanese",
                "nativeName": "日本語",
                "dir": "ltr"
              }
            ]
          }
        ]
      },
      ...
    }
  }
  ```

* `dictionary` Eigenschap

  De waarde van de `dictionary` eigenschap is een dictionary van (sleutel, waarde) paren. Elke sleutel is een code van de taal BCP 47. De sleutel geeft een taal waarvoor alternatieve vertalingen en back-vertalingen beschikbaar zijn. De waarde is een JSON-object dat de taal van de bron en de doel-talen met de beschikbare vertalingen beschrijft:

  * `name`: De weergavenaam van de bron-taal in de landinstellingen die is aangevraagd `Accept-Language` header.

  * `nativeName`: Weergavenaam van de taal in de oorspronkelijke landinstellingen voor deze taal.

  * `dir`: Richtinggevoeligheid, namelijk `rtl` voor talen van rechts naar links of `ltr` voor talen van links naar rechts.

  * `translations`: De lijst met talen en alternatief vertalingen en voorbeelden voor de query die wordt uitgedrukt in de taal van de bron. Elk element van de `translations` lijst heeft eigenschappen:

    * `name`: De weergavenaam van de doel-taal in de landinstellingen die is aangevraagd `Accept-Language` header.

    * `nativeName`: Naam van de doel-taal weergegeven in de landinstellingen systeemeigen voor de doel-taal.

    * `dir`: Richtinggevoeligheid, namelijk `rtl` voor talen van rechts naar links of `ltr` voor talen van links naar rechts.
    
    * `code`: Taalcode identificeren van de doel-taal.

  Er is een voorbeeld:

  ```json
  "es": {
    "name": "Spanish",
    "nativeName": "Español",
    "dir": "ltr",
    "translations": [
      {
        "name": "English",
        "nativeName": "English",
        "dir": "ltr",
        "code": "en"
      }
    ]
  },
  ```

De structuur van het antwoordobject wijzigen niet zonder een wijziging in de versie van de API. Voor de dezelfde versie van de API kan de lijst met beschikbare talen na verloop van tijd wijzigen, omdat Microsoft Translator voortdurend de lijst met talen die worden ondersteund door de services breidt.

De lijst met ondersteunde talen wordt niet regelmatig wordt gewijzigd. Voor het opslaan van de netwerkbandbreedte en de reactiesnelheid verbeteren, een clienttoepassing moet rekening houden met caching taal resources en de bijbehorende entiteitscode (`ETag`). Vervolgens de clienttoepassing kunt regelmatig (bijvoorbeeld, elke 24 uur) query uitvoeren op de service voor het ophalen van de meest recente set van ondersteunde talen. Het doorgeven van de huidige `ETag` waarde in een `If-None-Match` header-veld kan de service om te optimaliseren van het antwoord. Als de bron is niet gewijzigd, wordt de service 304-statuscode en de hoofdtekst van een leeg antwoord geretourneerd.

## <a name="response-headers"></a>Antwoordheaders

<table width="100%">
  <th width="20%">Headers</th>
  <th>Beschrijving</th>
  <tr>
    <td>ETag</td>
    <td>Huidige waarde van de tag entiteit voor de aangevraagde groepen van ondersteunde talen. Als u de volgende aanvragen efficiënter, verzendt de client kan de `ETag` waarde in een `If-None-Match` header-veld.
    </td>
  </tr>
  <tr>
    <td>X-RequestId</td>
    <td>De waarde is gegenereerd door de service voor het identificeren van de aanvraag. Het wordt gebruikt voor het oplossen van problemen.</td>
  </tr>
</table> 

## <a name="response-status-codes"></a>Statuscodes van antwoorden

Hier volgen de mogelijke HTTP-statuscodes die een aanvraag als resultaat geeft. 

<table width="100%">
  <th width="20%">Statuscode</th>
  <th>Beschrijving</th>
  <tr>
    <td>200</td>
    <td>Gelukt.</td>
  </tr>
  <tr>
    <td>304</td>
    <td>De resource is niet gewijzigd sinds de versie die is opgegeven door de aanvraagheaders `If-None-Match`.</td>
  </tr>
  <tr>
    <td>400</td>
    <td>Een van de queryparameters is ontbreekt of ongeldig. Aanvraagparameters voordat u probeert te corrigeren.</td>
  </tr>
  <tr>
    <td>429</td>
    <td>De aanroeper is te veel aanvragen verzenden.</td>
  </tr>
  <tr>
    <td>500</td>
    <td>Er is een onverwachte fout opgetreden. Als de fout zich blijft voordoen, met rapporteren: datum en tijd van de fout, aanvraag-id van antwoordheader `X-RequestId`, en de client-id van de aanvraag-header `X-ClientTraceId`.</td>
  </tr>
  <tr>
    <td>503</td>
    <td>De server is tijdelijk niet beschikbaar. De aanvraag opnieuw proberen. Als de fout zich blijft voordoen, met rapporteren: datum en tijd van de fout, aanvraag-id van antwoordheader `X-RequestId`, en de client-id van de aanvraag-header `X-ClientTraceId`.</td>
  </tr>
</table> 

## <a name="examples"></a>Voorbeelden

Het volgende voorbeeld laat zien hoe talen die worden ondersteund voor tekstvertaling ophalen.

# <a name="curltabcurl"></a>[cURL](#tab/curl)

```
curl "https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation"
```

---
