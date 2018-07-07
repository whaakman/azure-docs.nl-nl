---
title: Inzicht in gegevens wijziging concepten in LUIS - Azure | Microsoft Docs
description: Informatie over hoe gegevens kunnen worden gewijzigd voordat voorspellingen in Language Understanding (LUIS)
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/26/2018
ms.author: v-geberr
ms.openlocfilehash: ab70839b4fa8f2064a3ff929034a6ffcb8fc7788
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37887739"
---
# <a name="data-alterations"></a>Wijzigingen van gegevens
LUIS biedt methoden voor het bewerken van de utterance vóór of tijdens de voorspelling. 

## <a name="correct-spelling-errors-in-utterance"></a>Corrigeren van spelfouten in utterance
Maakt gebruik van LUIS [Bing spellingcontrole controleren-API-versie 7](https://azure.microsoft.com/services/cognitive-services/spell-check/) te corrigeren van spelfouten in de utterance. LUIS moet de sleutel die is gekoppeld aan die service. De sleutel maken en vervolgens de sleutel toevoegen als een queryreeksparameter aan de [eindpunt](https://aka.ms/luis-endpoint-apis). 

U kunt ook corrigeren van spelfouten in de **Test** door het deelvenster [invoeren van de sleutel](interactive-test.md#view-bing-spell-check-corrections-in-test-panel). De sleutel wordt opgeslagen als een variabele van sessie in de browser voor het deelvenster Test. De sleutel toevoegen aan het deelvenster Test in elke spelling gecorrigeerd gewenste browsersessie. 

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