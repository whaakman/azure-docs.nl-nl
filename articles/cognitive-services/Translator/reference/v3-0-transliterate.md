---
title: Microsoft Translator tekst API Transliterate methode | Microsoft Docs
description: Gebruik de methode Microsoft Translator tekst API Transliterate.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: fdd6fa9236f0c02685198b6de3228c444993dad6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345603"
---
# <a name="text-api-30-transliterate"></a>Tekst API 3.0: Transliterate

Zet tekst in één taal van een script om een ander script.

## <a name="request-url"></a>Aanvraag-URL

Verzenden van een `POST` aanvraag voor het:

```HTTP
https://api.cognitive.microsofttranslator.com/transliterate?api-version=3.0
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
    <td>taal</td>
    <td>*Vereiste parameter*.<br/>Hiermee geeft u de taal van de tekst van een script converteren naar een andere. Mogelijke talen worden vermeld in de `transliteration` bereik verkregen door het opvragen van de service voor de [ondersteunde talen](.\v3-0-languages.md).</td>
  </tr>
  <tr>
    <td>fromScript</td>
    <td>*Vereiste parameter*.<br/>Hiermee geeft u het script dat wordt gebruikt door de ingevoerde tekst. Lookup [ondersteunde talen](.\v3-0-languages.md) met behulp van de `transliteration` bereik invoer scripts beschikbaar vinden voor de geselecteerde taal.</td>
  </tr>
  <tr>
    <td>toScript</td>
    <td>*Vereiste parameter*.<br/>Hiermee geeft u het uitvoerscript. Lookup [ondersteunde talen](.\v3-0-languages.md) met behulp van de `transliteration` bereik, zoeken naar uitvoer-scripts die beschikbaar zijn voor de geselecteerde combinatie van taal en invoer script.</td>
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

De hoofdtekst van de aanvraag is een JSON-matrix. Elk matrixelement is een JSON-object met een string-eigenschap met de naam `Text`, die staat voor de tekenreeks om te converteren.

```json
[
    {"Text":"こんにちは"},
    {"Text":"さようなら"}
]
```

Er gelden de volgende beperkingen:

* De matrix kan maximaal 10 elementen hebben.
* De tekstwaarde van een matrixelement kan niet groter zijn dan 1000 tekens inclusief spaties.
* De volledige tekst is opgenomen in de aanvraag kan niet groter zijn dan 5000 tekens inclusief spaties.

## <a name="response-body"></a>Antwoordtekst

Een geslaagde reactie is een JSON-matrix met één resultaat voor elk element in de invoermatrix. Een resultaatobject bevat de volgende eigenschappen:

  * `text`: Een tekenreeks die het resultaat is van de conversie van invoertekenreeks aan het uitvoerscript.
  
  * `script`: Een tekenreeks opgeven van het script dat wordt gebruikt in de uitvoer.

Er is een voorbeeld van JSON-antwoord:

```json
[
    {"text":"konnnichiha","script":"Latn"},
    {"text":"sayounara","script":"Latn"}
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

Het volgende voorbeeld laat zien hoe twee Japanse tekenreeksen omzetten in Japanse Romanized.

# <a name="curltabcurl"></a>[cURL](#tab/curl)

De JSON-nettolading voor de aanvraag in dit voorbeeld:

```
[{"text":"こんにちは","script":"jpan"},{"text":"さようなら","script":"jpan"}]
```

Als u cUrl gebruikt in een opdrachtregelvenster die geen ondersteuning biedt voor Unicode-tekens, nemen de nettolading van de volgende JSON en sla het bestand in een bestand met de naam `request.txt`. Sla het bestand met de `UTF-8` codering.

```
curl -X POST "https://api.cognitive.microsofttranslator.com/transliterate?api-version=3.0&language=ja&fromScript=Jpan&toScript=Latn" -H "X-ClientTraceId: 875030C7-5380-40B8-8A03-63DACCF69C11" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d @request.txt
```

---
