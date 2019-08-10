---
title: Methode voor Translator Text-API talen
titleSuffix: Azure Cognitive Services
description: Gebruik de Translator Text-API talen methode.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 02/01/2019
ms.author: swmachan
ms.openlocfilehash: 3dd875fbf9a8da464abe1b6312cec84c030dc624
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68934022"
---
# <a name="translator-text-api-30-languages"></a>Translator Text-API 3,0: Languages

Hiermee wordt de set talen opgehaald die momenteel door andere bewerkingen van de Translator Text-API worden ondersteund. 

## <a name="request-url"></a>Aanvraag-URL

Een `GET` aanvraag verzenden naar:
```HTTP
https://api.cognitive.microsofttranslator.com/languages?api-version=3.0
```

## <a name="request-parameters"></a>Aanvraagparameters

Aanvraag parameters die zijn door gegeven voor de query reeks zijn:

<table width="100%">
  <th width="20%">Queryparameter</th>
  <th>Description</th>
  <tr>
    <td>api-version</td>
    <td><em>Vereiste para meter</em>.<br/>De versie van de API die door de client is aangevraagd. Waarde moet zijn `3.0`.</td>
  </tr>
  <tr>
    <td>bereik</td>
    <td>*Optionele para meter*.<br/>Een door komma's gescheiden lijst met namen die de groep talen bepalen die moet worden geretourneerd. Toegestane groeps namen zijn `translation`: `transliteration` , `dictionary`en. Als er geen bereik is opgegeven, worden alle groepen geretourneerd, wat gelijk is aan door `scope=translation,transliteration,dictionary`geven. Zie de beschrijving van het [antwoord object](#response-body)om te bepalen welke set ondersteunde talen geschikt is voor uw scenario.</td>
  </tr>
</table> 

Aanvraag headers zijn:

<table width="100%">
  <th width="20%">Headers</th>
  <th>Description</th>
  <tr>
    <td>Accept-Language</td>
    <td>*Optionele aanvraag header*.<br/>De taal die moet worden gebruikt voor gebruikersinterfacetekenreeksen. Sommige velden in het antwoord zijn namen van talen of namen van regio's. Gebruik deze para meter om de taal te definiëren waarin deze namen worden geretourneerd. De taal wordt opgegeven door een juist opgemaakte BCP 47-taal code op te geven. Gebruik bijvoorbeeld de waarde `fr` voor het aanvragen van namen in het Frans of gebruik de waarde `zh-Hant` om namen aan te vragen in traditioneel Chinees.<br/>Namen worden in de Engelse taal weer gegeven wanneer er geen doel taal is opgegeven of wanneer lokalisatie niet beschikbaar is.
    </td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*Optionele aanvraag header*.<br/>Een door de client gegenereerde GUID om de aanvraag uniek te identificeren.</td>
  </tr>
</table> 

Verificatie is niet vereist om taal bronnen op te halen.

## <a name="response-body"></a>Antwoord tekst

Een client gebruikt de `scope` query parameter voor het definiëren van de groepen van de talen waarin het is geïnteresseerd.

* `scope=translation`biedt talen ondersteuning voor het vertalen van tekst van de ene taal naar een andere taal.

* `scope=transliteration`biedt mogelijkheden voor het converteren van tekst in één taal van het ene script naar een ander script;

* `scope=dictionary`biedt taal paren waarvoor `Dictionary` bewerkingen gegevens retour neren.

Een client kan meerdere groepen tegelijkertijd ophalen door een door komma's gescheiden lijst met namen op te geven. Retourneert bijvoorbeeld `scope=translation,transliteration,dictionary` ondersteunde talen voor alle groepen.

Een geslaagde reactie is een JSON-object met één eigenschap voor elke aangevraagde groep:

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

* `translation`eigenschap

  De waarde van de `translation` eigenschap is een woorden lijst van paren (sleutel, waarde). Elke sleutel is een BCP 47-taal code. Een sleutel geeft een taal aan waarvoor tekst kan worden vertaald of vertaald. De waarde die is gekoppeld aan de sleutel is een JSON-object met eigenschappen die de taal beschrijven:

  * `name`: Weergave naam van de taal in de land instelling die `Accept-Language` via de header is aangevraagd.

  * `nativeName`: Weergave naam van de taal in de systeem eigen land instelling voor deze taal.

  * `dir`: Directionality, `rtl` voor talen die van rechts naar links worden geschreven of `ltr` van links naar rechts.

  Een voor beeld is:
          
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

* `transliteration`eigenschap

  De waarde van de `transliteration` eigenschap is een woorden lijst van paren (sleutel, waarde). Elke sleutel is een BCP 47-taal code. Een sleutel geeft een taal aan waarvoor tekst kan worden geconverteerd van een script naar een ander script. De waarde die is gekoppeld aan de sleutel is een JSON-object met eigenschappen die de taal en de ondersteunde scripts beschrijven:

  * `name`: Weergave naam van de taal in de land instelling die `Accept-Language` via de header is aangevraagd.

  * `nativeName`: Weergave naam van de taal in de systeem eigen land instelling voor deze taal.

  * `scripts`: Lijst met scripts waaruit moet worden geconverteerd. Elk element van de `scripts` lijst heeft eigenschappen:

    * `code`: Code voor het identificeren van het script.

    * `name`: Weergave naam van het script in de land instelling die `Accept-Language` via de header is aangevraagd.

    * `nativeName`: Weergave naam van de taal in de land instellingen systeem eigen voor de taal.

    * `dir`: Directionality, `rtl` voor talen die van rechts naar links worden geschreven of `ltr` van links naar rechts.

    * `toScripts`: Lijst met scripts die beschikbaar zijn voor het converteren van tekst naar. Elk element van de `toScripts` lijst heeft eigenschappen `code`, `name`, `nativeName`en `dir` zoals eerder beschreven.

  Een voor beeld is:

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

* `dictionary`eigenschap

  De waarde van de `dictionary` eigenschap is een woorden lijst van paren (sleutel, waarde). Elke sleutel is een BCP 47-taal code. De sleutel geeft een taal aan waarvoor alternatieve vertalingen en back-vertalingen beschikbaar zijn. De waarde is een JSON-object dat de bron taal en de doel talen beschrijft met beschik bare vertalingen:

  * `name`: Weergave naam van de bron taal in de land instelling die `Accept-Language` via de header is aangevraagd.

  * `nativeName`: Weergave naam van de taal in de systeem eigen land instelling voor deze taal.

  * `dir`: Directionality, `rtl` voor talen die van rechts naar links worden geschreven of `ltr` van links naar rechts.

  * `translations`: Lijst met talen met alterative-vertalingen en voor beelden voor de query, uitgedrukt in de bron taal. Elk element van de `translations` lijst heeft eigenschappen:

    * `name`: Weergave naam van de doel taal in de land instelling die `Accept-Language` via de header is aangevraagd.

    * `nativeName`: Weergave naam van de doel taal in de land instellingen systeem eigen voor de doel taal.

    * `dir`: Directionality, `rtl` voor talen die van rechts naar links worden geschreven of `ltr` van links naar rechts.
    
    * `code`: Taal code die de doel taal aangeeft.

  Een voor beeld is:

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

De structuur van het antwoord object wordt niet gewijzigd zonder een wijziging in de versie van de API. Voor dezelfde versie van de API kan de lijst met beschik bare talen na verloop van tijd veranderen omdat micro soft Translator de lijst met talen die door de services worden ondersteund, doorlopend uitbreidt.

De lijst met ondersteunde talen wordt niet regel matig gewijzigd. Om de netwerk bandbreedte te besparen en de reactie tijd te verbeteren, moet een client toepassing overwegen om taal bronnen en de`ETag`bijbehorende entiteits code () in de cache te plaatsen. Vervolgens kan de client toepassing periodiek (bijvoorbeeld elke 24 uur) een query uitvoeren op de service om de meest recente set ondersteunde talen op te halen. Door de huidige `ETag` waarde in een `If-None-Match` header veld door te geven, kan de service de reactie optimaliseren. Als de resource niet is gewijzigd, wordt de status code 304 en een lege antwoord tekst door de service geretourneerd.

## <a name="response-headers"></a>Antwoordheaders

<table width="100%">
  <th width="20%">Headers</th>
  <th>Description</th>
  <tr>
    <td>eTag</td>
    <td>Huidige waarde van de entiteits code voor de aangevraagde groepen met ondersteunde talen. Om volgende aanvragen efficiënter te maken, kan de client de `ETag` waarde in een `If-None-Match` header-veld verzenden.
    </td>
  </tr>
  <tr>
    <td>X-RequestId</td>
    <td>De waarde die door de service is gegenereerd om de aanvraag te identificeren. Dit wordt gebruikt voor het oplossen van problemen.</td>
  </tr>
</table> 

## <a name="response-status-codes"></a>Antwoord status codes

Hier volgen de mogelijke HTTP-status codes die een aanvraag retourneert. 

<table width="100%">
  <th width="20%">Statuscode</th>
  <th>Description</th>
  <tr>
    <td>200</td>
    <td>Voltooid.</td>
  </tr>
  <tr>
    <td>304</td>
    <td>De resource is niet gewijzigd sinds de versie die is opgegeven door de `If-None-Match`aanvraag headers.</td>
  </tr>
  <tr>
    <td>400</td>
    <td>Een van de query parameters ontbreekt of is ongeldig. Corrigeer de aanvraag parameters voordat u het opnieuw probeert.</td>
  </tr>
  <tr>
    <td>429</td>
    <td>De server heeft de aanvraag geweigerd omdat de aanvraag limieten voor de client is overschreden.</td>
  </tr>
  <tr>
    <td>500</td>
    <td>Er is een onverwachte fout opgetreden. Als de fout zich blijft voordoen, meldt u deze met: datum en tijd van de fout, aanvraag `X-RequestId`-id van antwoord header en client `X-ClientTraceId`-id uit aanvraag header.</td>
  </tr>
  <tr>
    <td>503</td>
    <td>De server is tijdelijk niet beschikbaar. Voer de aanvraag opnieuw uit. Als de fout zich blijft voordoen, meldt u deze met: datum en tijd van de fout, aanvraag `X-RequestId`-id van antwoord header en client `X-ClientTraceId`-id uit aanvraag header.</td>
  </tr>
</table> 

Als er een fout optreedt, wordt door de aanvraag ook een JSON-fout bericht geretourneerd. De fout code is een getal van 6 cijfers, waarbij de HTTP-status code van 3 cijfers wordt gevolgd door een getal van drie cijfers om de fout verder te categoriseren. Algemene fout codes vindt u op de [pagina v3-Translator text-API-referentie](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#errors). 

## <a name="examples"></a>Voorbeelden

In het volgende voor beeld ziet u hoe u talen kunt ophalen die worden ondersteund voor tekst vertaling.

```curl
curl "https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation"
```
