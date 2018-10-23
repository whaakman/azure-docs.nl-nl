---
title: Gegevens wijziging concepten in LUIS - Language Understanding
titleSuffix: Azure Cognitive Services
description: Informatie over hoe gegevens kunnen worden gewijzigd voordat voorspellingen in Language Understanding (LUIS)
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 6db7fd2474688608eb029fce1529ba1d3f00c5d3
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/22/2018
ms.locfileid: "49637167"
---
# <a name="data-alterations"></a>Wijzigingen van gegevens
LUIS biedt methoden voor het bewerken van de utterance vóór of tijdens de voorspelling. Het gaat hierbij spelling, en het verhelpen van problemen met de tijdzone voor prebuild datetimeV2 oplossen. 

## <a name="correct-spelling-errors-in-utterance"></a>Corrigeren van spelfouten in utterance
Maakt gebruik van LUIS [Bing spellingcontrole controleren-API-versie 7](https://azure.microsoft.com/services/cognitive-services/spell-check/) te corrigeren van spelfouten in de utterance. LUIS moet de sleutel die is gekoppeld aan die service. De sleutel maken en vervolgens de sleutel toevoegen als een queryreeksparameter aan de [eindpunt](https://aka.ms/luis-endpoint-apis). 

U kunt ook corrigeren van spelfouten in de **Test** door het deelvenster [invoeren van de sleutel](luis-interactive-test.md#view-bing-spell-check-corrections-in-test-panel). De sleutel wordt opgeslagen als een variabele van sessie in de browser voor het deelvenster Test. De sleutel toevoegen aan het deelvenster Test in elke spelling gecorrigeerd gewenste browsersessie. 

Gebruik van de sleutel in het deelvenster testen en op het eindpunt tellen mee voor de [sleutelgebruik](https://azure.microsoft.com/pricing/details/cognitive-services/spellcheck-api/) quotum. LUIS implementeert Bing Spell Check-beperkingen voor tekstlengte. 

Het eindpunt moet twee parameters voor spellingcorrecties om te werken:

|Param|Waarde|
|--|--|
|`spellCheck`|booleaans|
|`bing-spell-check-subscription-key`|[Bing spellingcontrole controleren-API-versie 7](https://azure.microsoft.com/services/cognitive-services/spell-check/) eindpuntsleutel|

Wanneer [Bing spellingcontrole controleren-API-versie 7](https://azure.microsoft.com/services/cognitive-services/spell-check/) detecteert een fout, de oorspronkelijke utterance en de gecorrigeerde utterance worden geretourneerd, samen met de voorspellingen van het eindpunt.

```JSON
{
  "query": "Book a flite to London?",
  "alteredQuery": "Book a flight to London?",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.780123
  },
  "entities": []
}
```
 
### <a name="whitelist-words"></a>Lijst met toegestane adressen woorden
De Bing spell check-API die wordt gebruikt in LUIS biedt geen ondersteuning voor een wit-lijst met woorden worden genegeerd tijdens de spelling controleren op wijzigingen. Als u witte lijst woorden of afkortingen wilt, verwerkt de utterance in de clienttoepassing met een witte lijst voordat de utterance worden verzonden naar LUIS voor intentie voorspelling.

## <a name="change-time-zone-of-prebuilt-datetimev2-entity"></a>Tijdzone van de vooraf gedefinieerde datetimeV2 entiteit wijzigen
Wanneer een LUIS-app maakt gebruik van de vooraf gedefinieerde datetimeV2 entiteit, kan een datum / tijdwaarde in het antwoord voorspelling worden geretourneerd. De tijdzone van de aanvraag wordt gebruikt om te bepalen van de juiste datum/tijd om terug te keren. Als de aanvraag afkomstig is van een bot of een andere gecentraliseerde toepassing voorafgaand aan LUIS, corrigeer dan de tijdzone die LUIS gebruikt. 

### <a name="endpoint-querystring-parameter"></a>Eindpunt querystring-parameter
De tijdzone wordt verholpen door het toevoegen van de tijdzone van de gebruiker naar de [eindpunt](https://aka.ms/luis-endpoint-apis) met behulp van de `timezoneOffset` param. De waarde van `timezoneOffset` moet de positief of negatief getal in minuten, voor het wijzigen van de tijd.  

|Param|Waarde|
|--|--|
|`timezoneOffset`|positief of negatief getal, in minuten|

### <a name="daylight-savings-example"></a>Voorbeeld van de zomer-en besparingen
Als u de geretourneerde vooraf gedefinieerde datetimeV2 om aan te passen voor zomer-en wintertijd, moet u de `timezoneOffset` querystring-parameter met een +/-waarde in minuten voor de [eindpunt](https://aka.ms/luis-endpoint-apis) query.

60 minuten toevoegen: 

https://{Region}.API.cognitive.Microsoft.com/Luis/v2.0/Apps/{appId}?q=TURN de verlichting op? **timezoneOffset = 60**& uitgebreide = {Booleaanse waarde} & spellingcontrole = {Booleaanse waarde} & staging = {Booleaanse waarde} & bing-spellingcontrole-controle-subscription-key = {tekenreeks} & logboek = {Booleaanse waarde}

60 minuten verwijderen: 

https://{Region}.API.cognitive.Microsoft.com/Luis/v2.0/Apps/{appId}?q=TURN de verlichting op? **timezoneOffset = 60**& uitgebreide = {Booleaanse waarde} & spellingcontrole = {Booleaanse waarde} & staging = {Booleaanse waarde} & bing-spellingcontrole-controle-subscription-key = {tekenreeks} & logboek = {Booleaanse waarde}

## <a name="c-code-determines-correct-value-of-timezoneoffset"></a>C#-code bepaalt de juiste waarde van timezoneOffset
De volgende C#-code gebruikt de [TimeZoneInfo](https://docs.microsoft.com/dotnet/api/system.timezoneinfo?view=netframework-4.7.1) van de klasse [FindSystemTimeZoneById](https://docs.microsoft.com/dotnet/api/system.timezoneinfo.findsystemtimezonebyid?view=netframework-4.7.1#examples) methode om te bepalen van de juiste `timezoneOffset` op basis van de tijd van systeem:

```CSharp
// Get CST zone id
TimeZoneInfo targetZone = TimeZoneInfo.FindSystemTimeZoneById("Central Standard Time");

// Get local machine's value of Now
DateTime utcDatetime = DateTime.UtcNow;

// Get Central Standard Time value of Now
DateTime cstDatetime = TimeZoneInfo.ConvertTimeFromUtc(utcDatetime, targetZone);

// Find timezoneOffset
int timezoneOffset = (int)((cstDatetime - utcDatetime).TotalMinutes);
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Juiste spelfouten in deze zelfstudie](luis-tutorial-bing-spellcheck.md)