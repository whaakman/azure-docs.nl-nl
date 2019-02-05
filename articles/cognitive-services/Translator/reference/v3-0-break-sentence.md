---
title: Translator Text-API BreakSentence methode
titlesuffix: Azure Cognitive Services
description: Gebruik de Translator Text-API BreakSentence-methode.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 02/01/2019
ms.author: v-jansko
ms.openlocfilehash: a74b3ba4fe9f12ce9d205b3865435edfb99566a8
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55693697"
---
# <a name="translator-text-api-30-breaksentence"></a>Translator Text-API 3.0: BreakSentence

Hiermee geeft u de plaatsing van de zin grenzen in een stuk tekst.

## <a name="request-url"></a>Aanvraag-URL

Verzendt een `POST` aanvragen:

```HTTP
https://api.cognitive.microsofttranslator.com/breaksentence?api-version=3.0
```

## <a name="request-parameters"></a>Aanvraagparameters

Parameters van de aanvraag doorgegeven aan de query-tekenreeks zijn:

<table width="100%">
  <th width="20%">Queryparameter</th>
  <th>Description</th>
  <tr>
    <td>API-versie</td>
    <td>*Vereiste queryparameter*.<br/>De versie van de API die is aangevraagd door de client. De waarde moet liggen `3.0`.</td>
  </tr>
  <tr>
    <td>language</td>
    <td>*Optionele queryparameter*.<br/>De taalcode voor het identificeren van de taal van de invoertekst. Als een code niet opgegeven is, wordt automatische taaldetectie worden toegepast.</td>
  </tr>
  <tr>
    <td>script</td>
    <td>*Optionele queryparameter*.<br/>Script-code voor het identificeren van het script dat wordt gebruikt door de invoertekst. Als een script niet opgegeven is, wordt aangenomen dat het standaardscript van de taal.</td>
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

De hoofdtekst van de aanvraag is een JSON-matrix. Elk matrixelement is een JSON-object met de tekenreekseigenschap van een met de naam `Text`. Zin grenzen worden berekend voor de waarde van de `Text` eigenschap. De hoofdtekst van een voorbeeld-aanvraag met een stuk tekst ziet er zo uit:

```json
[
    { "Text": "How are you? I am fine. What did you do today?" }
]
```

Er gelden de volgende beperkingen:

* De matrix kan maximaal 100 elementen hebben.
* De tekstwaarde van een element van de matrix niet langer zijn dan 10.000 tekens inclusief spaties.
* De volledige tekst is opgenomen in de aanvraag kan niet langer zijn dan 50.000 tekens inclusief spaties.
* Als de `language` query-parameter is opgegeven, wordt alle matrixelementen moeten zich in dezelfde taal. Automatische taaldetectie wordt anders toegepast op elk matrixelement het onafhankelijk van elkaar.

## <a name="response-body"></a>De hoofdtekst van antwoord

Een geslaagde reactie is een JSON-matrix met één resultaat voor elke tekenreeks in de invoermatrix. Een resultaatobject bevat de volgende eigenschappen:

  * `sentLen`: Een matrix met gehele getallen die de lengte van de zinnen in het tekstelement. De lengte van de matrix is het aantal zinnen en de waarden worden de lengte van elke zin. 

  * `detectedLanguage`: Een object met een beschrijving van de gedetecteerde taal via de volgende eigenschappen:

     * `language`: De code van de gedetecteerde taal.

     * `score`: Een float-waarde die aangeeft van het vertrouwen in het resultaat. De score tussen 0 en een en een lage score geeft aan dat een lage vertrouwen.
     
    Houd er rekening mee dat de `detectedLanguage` eigenschap is alleen aanwezig in het resultaatobject wanneer automatische taaldetectie wordt aangevraagd.

Een voorbeeld-JSON-antwoord is:

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

Als er een fout optreedt, wordt de aanvraag ook een JSON-fout antwoord retourneren. De foutcode is een 6-cijferige numerieke combineren het 3-cijferige HTTP-statuscode gevolgd door een getal 3 cijfers en verder categoriseren van de fout. Veelvoorkomende foutcodes kunnen u vinden op de [v3 Translator Text-API-verwijzingspagina](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#errors). 

## <a name="examples"></a>Voorbeelden

Het volgende voorbeeld ziet hoe u verkrijgt zin grenzen voor een enkele zin. De taal van de zin wordt automatisch gedetecteerd door de service.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/breaksentence?api-version=3.0" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'How are you? I am fine. What did you do today?'}]"
```

---

