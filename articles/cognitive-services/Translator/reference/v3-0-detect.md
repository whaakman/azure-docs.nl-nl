---
title: Microsoft Translator tekst API detecteren methode | Microsoft Docs
description: Gebruik de methode Microsoft Translator tekst API detecteren.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 7e81e91230e1ada4423d77d22134b1b64df65d9d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345604"
---
# <a name="text-api-30-detect"></a>Tekst API 3.0: detecteren

Geeft de taal van tekst.

## <a name="request-url"></a>Aanvraag-URL

Verzenden van een `POST` aanvraag voor het:

```HTTP
https://api.cognitive.microsofttranslator.com/detect?api-version=3.0
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
</table> 

Aanvraagheaders omvatten:

<table width="100%">
  <th width="20%">Headers</th>
  <th>Beschrijving</th>
  <tr>
    <td>_Een autorisatie_<br/>_Koptekst_</td>
    <td>*Vereiste aanvraagheader*.<br/>Zie [beschikbare opties voor verificatie](./v3-0-reference.md#authentication).</td>
  </tr>
  <tr>
    <td>Inhoudstype</td>
    <td>*Vereiste aanvraagheader*.<br/>Hiermee geeft u het type inhoud van de nettolading. Mogelijke waarden zijn: `application/json`.</td>
  </tr>
  <tr>
    <td>Content-Length</td>
    <td>*Vereiste aanvraagheader*.<br/>De lengte van de aanvraagtekst.</td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*Optioneel*.<br/>Een client gegenereerde GUID als unieke identificatie van de aanvraag. Opmerking u kunt deze header weglaten als u de trace-ID aanwezig in de query-tekenreeks met een queryparameter met de naam `ClientTraceId`.</td>
  </tr>
</table> 

## <a name="request-body"></a>Aanvraagtekst

De hoofdtekst van de aanvraag is een JSON-matrix. Elk matrixelement is een JSON-object met een string-eigenschap met de naam `Text`. Taaldetectie wordt toegepast op de waarde van de `Text` eigenschap. De aanvraagtekst voor een voorbeeld ziet er zo uit:

```json
[
    { "Text": "Ich würde wirklich gern Ihr Auto um den Block fahren ein paar Mal." }
]
```

Er gelden de volgende beperkingen:

* De matrix kan maximaal 100 elementen hebben.
* De tekstwaarde van een matrixelement kan niet groter zijn dan 10.000 tekens inclusief spaties.
* De volledige tekst is opgenomen in de aanvraag kan niet groter zijn dan 50.000 tekens inclusief spaties.

## <a name="response-body"></a>Antwoordtekst

Een geslaagde reactie is een JSON-matrix met één resultaat voor elke tekenreeks in de invoermatrix. Een resultaatobject bevat de volgende eigenschappen:

  * `language`: De code van de gedetecteerde taal.

  * `score`: Een float-waarde die aangeeft het vertrouwen in het resultaat. De score tussen nul en één is en een lage score geeft aan dat een lage vertrouwen.

  * `isTranslationSupported`: Een Booleaanse waarde die ingesteld op true is als de gedetecteerde taal een van de talen die worden ondersteund voor de tekstvertaling van is.

  * `isTransliterationSupported`: Een Booleaanse waarde die ingesteld op true is als de gedetecteerde taal een van de talen die worden ondersteund voor transliteration is.
  
  * `alternatives`: Er is een matrix van andere mogelijke talen. Elk element van de matrix is een ander object met dezelfde eigenschappen die hierboven worden genoemd: `language`, `score`, `isTranslationSupported` en `isTransliterationSupported`.

Er is een voorbeeld van JSON-antwoord:

```json
[
  {
    "language": "de",
    "score": 0.92,
    "isTranslationSupported": true,
    "isTransliterationSupported": false,
    "alternatives": [
      {
        "language": "pt",
        "score": 0.23,
        "isTranslationSupported": true,
        "isTransliterationSupported": false
      },
      {
        "language": "sk",
        "score": 0.23,
        "isTranslationSupported": true,
        "isTransliterationSupported": false
      }
    ]
  }
]
```

## <a name="response-headers"></a>Antwoordheaders

<table width="100%">
  <th width="20%">Headers</th>
  <th>Beschrijving</th>
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
    <td>400</td>
    <td>Een van de queryparameters is ontbreekt of ongeldig. Aanvraagparameters voordat u probeert te corrigeren.</td>
  </tr>
  <tr>
    <td>401</td>
    <td>De aanvraag kan niet worden geverifieerd. Controleer of de referenties opgegeven en geldig zijn.</td>
  </tr>
  <tr>
    <td>403</td>
    <td>De aanvraag is niet gemachtigd. Controleer het foutbericht voor meer informatie. Dit betekent meestal dat alle gratis vertalingen opgegeven met een proefabonnement van zijn gebruikt.</td>
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
curl -X POST "https://api.cognitive.microsofttranslator.com/detect?api-version=3.0" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'What language is this text written in?'}]"
```

---
