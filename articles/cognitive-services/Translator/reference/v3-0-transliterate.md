---
title: Translator Text-API transliteratie methode
titlesuffix: Azure Cognitive Services
description: Gebruik de methode transliteratie van Translator Text-API.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: a37b069d533dc51067aba4f9d9bd87608b116ae6
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55466452"
---
# <a name="translator-text-api-30-transliterate"></a>Translator Text-API 3.0: Transliterate

Converteert tekst in één taal van één script naar een ander script.

## <a name="request-url"></a>Aanvraag-URL

Verzendt een `POST` aanvragen:

```HTTP
https://api.cognitive.microsofttranslator.com/transliterate?api-version=3.0
```

## <a name="request-parameters"></a>Aanvraagparameters

Parameters van de aanvraag doorgegeven aan de query-tekenreeks zijn:

<table width="100%">
  <th width="20%">Queryparameter</th>
  <th>Description</th>
  <tr>
    <td>API-versie</td>
    <td>*Vereiste parameter*.<br/>De versie van de API die is aangevraagd door de client. De waarde moet liggen `3.0`.</td>
  </tr>
  <tr>
    <td>language</td>
    <td>*Vereiste parameter*.<br/>Hiermee geeft u de taal van de tekst die moet worden geconverteerd van een script naar een andere. Mogelijke talen worden vermeld in de `transliteration` bereik verkregen door het opvragen van de service voor de [ondersteunde talen](./v3-0-languages.md).</td>
  </tr>
  <tr>
    <td>fromScript</td>
    <td>*Vereiste parameter*.<br/>Hiermee geeft u het script dat wordt gebruikt door de invoertekst. Opzoeken [ondersteunde talen](./v3-0-languages.md) met behulp van de `transliteration` bereik, zoeken invoer scripts beschikbaar voor de geselecteerde taal.</td>
  </tr>
  <tr>
    <td>toScript</td>
    <td>*Vereiste parameter*.<br/>Hiermee geeft u het uitvoerscript. Opzoeken [ondersteunde talen](./v3-0-languages.md) met behulp van de `transliteration` bereik, zoeken naar uitvoer-scripts die beschikbaar zijn voor de geselecteerde combinatie van taal- en invoer-script.</td>
  </tr>
</table> 

Aanvraagheaders zijn onder andere:

<table width="100%">
  <th width="20%">Headers</th>
  <th>Description</th>
  <tr>
    <td>_Een autorisatie_<br/>_header_</td>
    <td>*Vereiste aanvraagheader*.<br/>Zie [beschikbare opties voor verificatie](./v3-0-reference.md#authentication).</td>
  </tr>
  <tr>
    <td>Content-Type</td>
    <td>*Vereiste aanvraagheader*.<br/>Hiermee geeft u het type inhoud van de nettolading. Mogelijke waarden zijn: `application/json`.</td>
  </tr>
  <tr>
    <td>Content-Length</td>
    <td>*Vereiste aanvraagheader*.<br/>De lengte van de aanvraagtekst.</td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*Optioneel*.<br/>Een client gegenereerde GUID voor het aanduiden van de aanvraag. Houd er rekening mee dat u deze header weglaten kunt als u de trace-ID opnemen in de querytekenreeks met behulp van een queryparameter met de naam `ClientTraceId`.</td>
  </tr>
</table> 

## <a name="request-body"></a>Aanvraagbody

De hoofdtekst van de aanvraag is een JSON-matrix. Elk matrixelement is een JSON-object met de tekenreekseigenschap van een met de naam `Text`, die staat voor de tekenreeks moet worden geconverteerd.

```json
[
    {"Text":"こんにちは"},
    {"Text":"さようなら"}
]
```

Er gelden de volgende beperkingen:

* De matrix kan maximaal 10 elementen hebben.
* De tekstwaarde van een element van de matrix niet langer zijn dan 1000 tekens inclusief spaties.
* De volledige tekst is opgenomen in de aanvraag kan niet langer zijn dan 5000 tekens inclusief spaties.

## <a name="response-body"></a>De hoofdtekst van antwoord

Een geslaagde reactie is een JSON-matrix met één resultaat voor elk element in de invoermatrix. Een resultaatobject bevat de volgende eigenschappen:

  * `text`: Een tekenreeks. dit het resultaat is van het converteren van de ingevoerde tekenreeks voor het uitvoerscript.
  
  * `script`: Een tekenreeks die het script dat wordt gebruikt in de uitvoer op te geven.

Een voorbeeld-JSON-antwoord is:

```json
[
    {"text":"konnnichiha","script":"Latn"},
    {"text":"sayounara","script":"Latn"}
]
```

## <a name="response-headers"></a>Antwoordheaders

<table width="100%">
  <th width="20%">Headers</th>
  <th>Description</th>
  <tr>
    <td>X-RequestId</td>
    <td>De waarde die wordt gegenereerd door de service voor het identificeren van de aanvraag. Het wordt gebruikt voor het oplossen van problemen.</td>
  </tr>
</table> 

## <a name="response-status-codes"></a>Antwoord-statuscodes

Hier volgen de mogelijke HTTP-statuscodes die een aanvraag retourneert. 

<table width="100%">
  <th width="20%">Statuscode</th>
  <th>Description</th>
  <tr>
    <td>200</td>
    <td>Geslaagd.</td>
  </tr>
  <tr>
    <td>400</td>
    <td>Een van de queryparameters is ontbreekt of is ongeldig. De parameters van de juiste aanvraag voordat opnieuw wordt geprobeerd.</td>
  </tr>
  <tr>
    <td>401</td>
    <td>De aanvraag kan niet worden geverifieerd. Controleer of de referenties opgegeven en is geldig zijn.</td>
  </tr>
  <tr>
    <td>403</td>
    <td>De aanvraag is niet gemachtigd. Controleer het foutbericht voor meer informatie. Dit betekent meestal dat alle gratis vertalingen voorzien van een proefabonnement zijn verbruikt.</td>
  </tr>
  <tr>
    <td>429</td>
    <td>De aanroeper is te veel aanvragen verzenden.</td>
  </tr>
  <tr>
    <td>500</td>
    <td>Er is een onverwachte fout opgetreden. Als de fout zich blijft voordoen, rapporteren met: datum en tijd van de fout, aanvraag-id van de reactieheader `X-RequestId`, en de client-id van aanvraagheader `X-ClientTraceId`.</td>
  </tr>
  <tr>
    <td>503</td>
    <td>De server is tijdelijk niet beschikbaar. De aanvraag opnieuw. Als de fout zich blijft voordoen, rapporteren met: datum en tijd van de fout, aanvraag-id van de reactieheader `X-RequestId`, en de client-id van aanvraagheader `X-ClientTraceId`.</td>
  </tr>
</table> 

## <a name="examples"></a>Voorbeelden

Het volgende voorbeeld ziet hoe u twee Japanse tekenreeksen omzetten in Japanse Romanized.

# <a name="curltabcurl"></a>[curl](#tab/curl)

De JSON-nettolading voor de aanvraag in dit voorbeeld:

```
[{"text":"こんにちは","script":"jpan"},{"text":"さようなら","script":"jpan"}]
```

Als u van cURL in een opdrachtregelvenster die geen ondersteuning biedt voor Unicode-tekens gebruikmaakt, worden de volgende JSON-nettolading en sla deze op in een bestand met de naam `request.txt`. Zorg ervoor dat u het bestand met de `UTF-8` codering.

```
curl -X POST "https://api.cognitive.microsofttranslator.com/transliterate?api-version=3.0&language=ja&fromScript=Jpan&toScript=Latn" -H "X-ClientTraceId: 875030C7-5380-40B8-8A03-63DACCF69C11" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d @request.txt
```

---
