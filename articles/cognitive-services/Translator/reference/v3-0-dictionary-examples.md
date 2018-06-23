---
title: Microsoft Translator tekst API woordenlijst voorbeelden methode | Microsoft Docs
description: Gebruik de methode Microsoft Translator tekst API woordenlijst voorbeelden.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 9960f3be42090edaec1df935d70e4c1a0d25b691
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345606"
---
# <a name="text-api-30-dictionary-examples"></a>Tekst API 3.0: Voorbeelden woordenlijst

Biedt voorbeelden die laten zien hoe de voorwaarden in de woordenlijst in de context worden gebruikt. Deze bewerking wordt gebruikt in combinatie met [woordenboek](.\v3-0-dictionary-lookup.md).

## <a name="request-url"></a>Aanvraag-URL

Verzenden van een `POST` aanvraag voor het:

```HTTP
https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0
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
    <td>uit</td>
    <td>*Vereiste parameter*.<br/>Hiermee geeft u de taal van de ingevoerde tekst. De taal van de bron moet een van de [ondersteunde talen](.\v3-0-languages.md) opgenomen in de `dictionary` bereik.</td>
  </tr>
  <tr>
    <td>tot</td>
    <td>*Vereiste parameter*.<br/>Hiermee geeft u de taal van de uitvoertekst. De taal van het doel moet een van de [ondersteunde talen](.\v3-0-languages.md) opgenomen in de `dictionary` bereik.</td>
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
    <td>*Optioneel*.<br/>Een client gegenereerde GUID als unieke identificatie van de aanvraag. U kunt deze header weglaten als u de trace-ID aanwezig in de query-tekenreeks met een queryparameter met de naam `ClientTraceId`.</td>
  </tr>
</table> 

## <a name="request-body"></a>Aanvraagtekst

De hoofdtekst van de aanvraag is een JSON-matrix. Elk matrixelement is een JSON-object met de volgende eigenschappen:

  * `Text`: Een tekenreeks opgeven van de termijn opzoeken. Dit is de waarde van een `normalizedText` veld van de back-vertalingen van een eerdere [woordenboek](.\v3-0-dictionary-lookup.md) aanvraag. Het kan ook worden veroorzaakt de waarde van de `normalizedSource` veld.

  * `Translation`: Een tekenreeks waarmee de vertaalde tekst die eerder zijn geretourneerd door de [woordenboek](.\v3-0-dictionary-lookup.md) bewerking. Dit is de waarde van de `normalizedTarget` veld in de `translations` lijst met de [woordenboek](.\v3-0-dictionary-lookup.md) antwoord. De service wordt voorbeelden voor het specifieke bron doel word-paar geretourneerd.

Er is een voorbeeld:

```json
[
    {"Text":"fly", "Translation":"volar"}
]
```

Er gelden de volgende beperkingen:

* De matrix kan maximaal 10 elementen hebben.
* De tekstwaarde van een matrixelement kan niet groter zijn dan 100 tekens inclusief spaties.

## <a name="response-body"></a>Antwoordtekst

Een geslaagde reactie is een JSON-matrix met één resultaat voor elke tekenreeks in de invoermatrix. Een resultaatobject bevat de volgende eigenschappen:

  * `normalizedSource`: Een tekenreeks waarin de genormaliseerde vorm van de bronterm. In het algemeen dit moet identiek zijn aan de waarde van de `Text` veld bij de overeenkomende lijstindex in de hoofdtekst van de aanvraag.
    
  * `normalizedTarget`: Een tekenreeks waarin de genormaliseerde vorm van de doelterm. In het algemeen dit moet identiek zijn aan de waarde van de `Translation` veld bij de overeenkomende lijstindex in de hoofdtekst van de aanvraag.
  
  * `examples`: Een lijst met voorbeelden voor het (bron termijn, doelterm) paar. Elk element van de lijst is een object met de volgende eigenschappen:

    * `sourcePrefix`: De tekenreeks voor het samenvoegen van _voordat_ de waarde van `sourceTerm` vormt een compleet voorbeeld. Voeg een spatie, geen omdat deze nog wordt weergegeven wanneer deze moet worden. Deze waarde kan een lege tekenreeks zijn.

    * `sourceTerm`: Een tekenreeks die gelijk is aan de werkelijke term wordt opgezocht. De tekenreeks wordt toegevoegd met `sourcePrefix` en `sourceSuffix` vormen het complete voorbeeld. De waarde ervan wordt gescheiden, zodat deze kan worden gemarkeerd in de gebruikersinterface, bijvoorbeeld door vet deze.

    * `sourceSuffix`: De tekenreeks voor het samenvoegen van _nadat_ de waarde van `sourceTerm` vormt een compleet voorbeeld. Voeg een spatie, geen omdat deze nog wordt weergegeven wanneer deze moet worden. Deze waarde kan een lege tekenreeks zijn.

    * `targetPrefix`: Een tekenreeks die vergelijkbaar is met `sourcePrefix` maar voor het doel.

    * `targetTerm`: Een tekenreeks die vergelijkbaar is met `sourceTerm` maar voor het doel.

    * `targetSuffix`: Een tekenreeks die vergelijkbaar is met `sourceSuffix` maar voor het doel.

    > [!NOTE]
    > Als er geen voorbeelden in de woordenlijst, het antwoord is 200 (OK), maar de `examples` lijst is een lege lijst.

## <a name="examples"></a>Voorbeelden

In dit voorbeeld laat zien hoe voor het opzoeken van voorbeelden voor het paar bestaat uit de Engelse term `fly` en de Spaanse vertaling `volar`.

# <a name="curltabcurl"></a>[cURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0&from=en&to=es" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly', 'Translation':'volar'}]"
```

---

De hoofdtekst van de reactie (afgekort voor de duidelijkheid) is:

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
