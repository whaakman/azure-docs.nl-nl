---
title: Translator Text-API-woordenlijst voorbeelden methode
titlesuffix: Azure Cognitive Services
description: Gebruik de methode Translator Text-API-woordenlijst voorbeelden.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: b12b9d56f42911da606e3bdcfedbe3f789d2c4e8
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55466930"
---
# <a name="translator-text-api-30-dictionary-examples"></a>Translator Text-API 3.0: Voorbeelden in woordenlijst

Biedt voorbeelden die laten zien hoe de voorwaarden in de woordenlijst in de context worden gebruikt. Deze bewerking wordt gebruikt in combinatie met [Dictionary lookup](./v3-0-dictionary-lookup.md).

## <a name="request-url"></a>Aanvraag-URL

Verzendt een `POST` aanvragen:

```HTTP
https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0
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
    <td>uit</td>
    <td>*Vereiste parameter*.<br/>Hiermee geeft u de taal van de invoertekst. De source-taal moet een van de [ondersteunde talen](./v3-0-languages.md) opgenomen in de `dictionary` bereik.</td>
  </tr>
  <tr>
    <td>tot</td>
    <td>*Vereiste parameter*.<br/>Hiermee geeft u de taal van de uitvoertekst. De doeltaal moet een van de [ondersteunde talen](./v3-0-languages.md) opgenomen in de `dictionary` bereik.</td>
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
    <td>*Optioneel*.<br/>Een client gegenereerde GUID voor het aanduiden van de aanvraag. U kunt deze header weglaten als u de trace-ID opnemen in de querytekenreeks met behulp van een queryparameter met de naam `ClientTraceId`.</td>
  </tr>
</table> 

## <a name="request-body"></a>Aanvraagbody

De hoofdtekst van de aanvraag is een JSON-matrix. Elk matrixelement is een JSON-object met de volgende eigenschappen:

  * `Text`: Een tekenreeks op te geven de term te zoeken. Dit moet de waarde van een `normalizedText` veld uit de back-vertalingen van een vorige [Dictionary lookup](./v3-0-dictionary-lookup.md) aanvraag. Het kan ook worden de waarde van de `normalizedSource` veld.

  * `Translation`: Een tekenreeks op te geven de vertaalde tekst die eerder zijn geretourneerd door de [Dictionary lookup](./v3-0-dictionary-lookup.md) bewerking. Dit moet de waarde van de `normalizedTarget` veld in de `translations` lijst van de [Dictionary lookup](./v3-0-dictionary-lookup.md) antwoord. Voorbeelden voor het specifieke bron-doel word-paar retourneert de service.

Er is een voorbeeld:

```json
[
    {"Text":"fly", "Translation":"volar"}
]
```

Er gelden de volgende beperkingen:

* De matrix kan maximaal 10 elementen hebben.
* De tekstwaarde van een element van de matrix niet langer zijn dan 100 tekens inclusief spaties.

## <a name="response-body"></a>De hoofdtekst van antwoord

Een geslaagde reactie is een JSON-matrix met één resultaat voor elke tekenreeks in de invoermatrix. Een resultaatobject bevat de volgende eigenschappen:

  * `normalizedSource`: Een tekenreeks waarin de gestandaardiseerde vorm van de bronterm. Over het algemeen dit moet identiek zijn aan de waarde van de `Text` veld in de overeenkomende lijstindex in de hoofdtekst van de aanvraag.
    
  * `normalizedTarget`: Een tekenreeks waarin de gestandaardiseerde vorm van de doelterm. Over het algemeen dit moet identiek zijn aan de waarde van de `Translation` veld in de overeenkomende lijstindex in de hoofdtekst van de aanvraag.
  
  * `examples`: Een lijst met voorbeelden voor de (bronterm, doelterm) paar. Elk element van de lijst is een object met de volgende eigenschappen:

    * `sourcePrefix`: De tekenreeks om samen te voegen _voordat_ de waarde van `sourceTerm` om te vormen een compleet voorbeeld. Voeg een spatie, omdat er nog wordt weergegeven wanneer deze moet worden. Deze waarde kan een lege tekenreeks zijn.

    * `sourceTerm`: Een tekenreeks gelijk is aan de werkelijke term opgezocht. De tekenreeks wordt toegevoegd met `sourcePrefix` en `sourceSuffix` om te vormen het complete voorbeeld. De waarde wordt gescheiden, zodat deze kan worden gemarkeerd in een gebruikersinterface, bijvoorbeeld door vet deze.

    * `sourceSuffix`: De tekenreeks om samen te voegen _nadat_ de waarde van `sourceTerm` om te vormen een compleet voorbeeld. Voeg een spatie, omdat er nog wordt weergegeven wanneer deze moet worden. Deze waarde kan een lege tekenreeks zijn.

    * `targetPrefix`: Een tekenreeks die vergelijkbaar is met `sourcePrefix` , maar voor het doel.

    * `targetTerm`: Een tekenreeks die vergelijkbaar is met `sourceTerm` , maar voor het doel.

    * `targetSuffix`: Een tekenreeks die vergelijkbaar is met `sourceSuffix` , maar voor het doel.

    > [!NOTE]
    > Als er geen voorbeelden in de woordenlijst, het antwoord is 200 (OK), maar de `examples` lijst is een lege lijst.

## <a name="examples"></a>Voorbeelden

In dit voorbeeld laat zien hoe voor het opzoeken van voorbeelden voor het paar van de Engelse term opgebouwd `fly` en de Spaans vertaling `volar`.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0&from=en&to=es" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly', 'Translation':'volar'}]"
```

---

De hoofdtekst van antwoord (afgekort voor de duidelijkheid) is:

```
[
    {
        "normalizedSource":"fly",
        "normalizedTarget":"volar",
        "examples":[
            {
                "sourcePrefix":"They need machines to ",
                "sourceTerm":"fly",
                "sourceSuffix":".",
                "targetPrefix":"Necesitan máquinas para ",
                "targetTerm":"volar",
                "targetSuffix":"."
            },      
            {
                "sourcePrefix":"That should really ",
                "sourceTerm":"fly",
                "sourceSuffix":".",
                "targetPrefix":"Eso realmente debe ",
                "targetTerm":"volar",
                "targetSuffix":"."
            },
            //
            // ...list abbreviated for documentation clarity
            //
        ]
    }
]
```
