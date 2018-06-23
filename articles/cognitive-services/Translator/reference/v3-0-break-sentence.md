---
title: Microsoft Translator tekst API BreakSentence methode | Microsoft Docs
description: Gebruik de methode Microsoft Translator tekst API BreakSentence.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 8ce6644d21b397ea0e7f2e71e3c3a5a96638eec5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345607"
---
# <a name="text-api-30-breaksentence"></a>Tekst API 3.0: BreakSentence

Geeft de plaatsing van zin grenzen in een stuk tekst.

## <a name="request-url"></a>Aanvraag-URL

Verzenden van een `POST` aanvraag voor het:

```HTTP
https://api.cognitive.microsofttranslator.com/breaksentence?api-version=3.0
```

## <a name="request-parameters"></a>Aanvraagparameters

Aanvragen in de queryreeks doorgegeven parameters zijn:

<table width="100%">
  <th width="20%">Queryparameter</th>
  <th>Beschrijving</th>
  <tr>
    <td>API-versie</td>
    <td>*Vereiste queryparameter*.<br/>De versie van de API die door de client wordt aangevraagd. De waarde moet `3.0`.</td>
  </tr>
  <tr>
    <td>taal</td>
    <td>*Optionele queryparameter*.<br/>Taallabel is de taal van de ingevoerde tekst te identificeren. Als een code niet is opgegeven, is de van de automatische taaldetectie wordt toegepast.</td>
  </tr>
  <tr>
    <td>script</td>
    <td>*Optionele queryparameter*.<br/>Scriptcode identificeren van het script dat wordt gebruikt door de ingevoerde tekst. Als u een script niet is opgegeven, wordt aangenomen dat het standaardscript van de taal.</td>
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

De hoofdtekst van de aanvraag is een JSON-matrix. Elk matrixelement is een JSON-object met een string-eigenschap met de naam `Text`. Zin grenzen worden berekend voor de waarde van de `Text` eigenschap. Een aanvraagtekst voorbeeld met een gedeelte van de tekst ziet er zo uit:

```json
[
    { "Text": "How are you? I am fine. What did you do today?" }
]
```

Er gelden de volgende beperkingen:

* De matrix kan maximaal 100 elementen hebben.
* De tekstwaarde van een matrixelement kan niet groter zijn dan 10.000 tekens inclusief spaties.
* De volledige tekst is opgenomen in de aanvraag kan niet groter zijn dan 50.000 tekens inclusief spaties.
* Als de `language` queryparameter wordt opgegeven, wordt alle matrixelementen moeten in dezelfde taal. Anders wordt taal automatische detectie aan elk matrixelement onafhankelijk toegepast.

## <a name="response-body"></a>Antwoordtekst

Een geslaagde reactie is een JSON-matrix met één resultaat voor elke tekenreeks in de invoermatrix. Een resultaatobject bevat de volgende eigenschappen:

  * `sentLen`: Er is een matrix van gehele getallen die de lengte van de zinnen in het tekstelement. De lengte van de matrix is het aantal zinnen en de waarden worden de lengte van elke zin. 

  * `detectedLanguage`: Er is een object met een beschrijving van de gedetecteerde taal via de volgende eigenschappen:

     * `language`: De code van de gedetecteerde taal.

     * `score`: Een float-waarde die aangeeft het vertrouwen in het resultaat. De score tussen nul en één is en een lage score geeft aan dat een lage vertrouwen.
     
    Houd er rekening mee dat de `detectedLanguage` eigenschap is alleen aanwezig in het resultaatobject wanneer automatische detectie van taal wordt aangevraagd.

Er is een voorbeeld van JSON-antwoord:

```json
[
  {
    "sentenceLengths": [ 13, 11, 22 ]
    "detectedLanguage": {
      "language": "en",
      "score": 401
    },
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
    <td>Geslaagd.</td>
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

Het volgende voorbeeld laat zien hoe zin grenzen voor een enkele zin verkrijgen. De taal van de zin wordt automatisch gedetecteerd door de service.

# <a name="curltabcurl"></a>[cURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/breaksentence?api-version=3.0" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'How are you? I am fine. What did you do today?'}]"
```

---

