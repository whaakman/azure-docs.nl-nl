---
title: Inzicht in gegevens afwijking concepten in LUIS - Azure | Microsoft Docs
description: Meer informatie over hoe gegevens kunnen worden gewijzigd voordat voorspellingen in Language Understanding (LUIS)
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/26/2018
ms.author: v-geberr
ms.openlocfilehash: ab9b9fe5c34c334fd65971f9040c8e43467aa242
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37112147"
---
# <a name="data-alterations"></a>Wijzigingen voor gegevens
LUIS biedt methoden voor het bewerken van de utterance vóór of tijdens de voorspelling. 

## <a name="correct-spelling-errors-in-utterance"></a>Juiste spelling fouten in utterance
Maakt gebruik van LUIS [Bing spellen controleren API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) spelling fouten in de utterance corrigeren. LUIS moet de sleutel gekoppeld aan die service. Maak de sleutel, en voeg vervolgens de sleutel als een parameter querystring op de [eindpunt](https://aka.ms/luis-endpoint-apis). 

U kunt ook typfouten in de **Test** van het deelvenster door [invoeren van de sleutel](interactive-test.md#view-bing-spell-check-corrections-in-test-panel). De sleutel wordt opgeslagen als een sessievariabele in de browser voor het paneel Test. De sleutel toevoegen aan het deelvenster Test in elke spelling gecorrigeerd gewenste browsersessie. 

Informatie over het gebruik van de sleutel in het deelvenster test en het aantal eindpunt naar de [sleutelgebruik](https://azure.microsoft.com/pricing/details/cognitive-services/spellcheck-api/) quotum. LUIS implementeert spellingcontrole van Bing limieten voor tekstlengte. 

Het eindpunt moet twee parameters voor correcties werken:

|Param|Waarde|
|--|--|
|`spellCheck`|booleaans|
|`bing-spell-check-subscription-key`|[Bing spellen controleren API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) eindpuntsleutel|

Wanneer [Bing spellen controleren API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) detecteert een fout opgetreden, de oorspronkelijke utterance en de gecorrigeerde utterance worden geretourneerd samen met de voorspellingen van het eindpunt.

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
 
## <a name="change-time-zone-of-prebuilt-datetimev2-entity"></a>De tijdzone wijzigen van vooraf gedefinieerde datetimeV2 entiteit
Wanneer een app LUIS gebruikmaakt van de vooraf gedefinieerde datetimeV2 entiteit, kan een datetime-waarde in het antwoord voorspelling worden geretourneerd. De tijdzone van de aanvraag wordt gebruikt om te bepalen van de juiste datum/tijd om terug te keren. Als de aanvraag afkomstig is van een bot of een andere gecentraliseerde toepassing voorafgaand aan LUIS, corrigeer dan de tijdzone die LUIS gebruikt. 

### <a name="endpoint-querystring-parameter"></a>De parameter querystring eindpunt
De tijdzone is opgelost doordat de tijdzone van de gebruiker naar de [eindpunt](https://aka.ms/luis-endpoint-apis) met behulp van de `timezoneOffset` param. De waarde van `timezoneOffset` moet het getal positief of negatief in minuten, voor het wijzigen van de tijd.  

|Param|Waarde|
|--|--|
|`timezoneOffset`|positief of negatief getal, in minuten|

### <a name="daylight-savings-example"></a>Voorbeeld van de zomer-en besparingen
Als u de geretourneerde vooraf gedefinieerde datetimeV2 wintertijd aanpassen moet, moet u de `timezoneOffset` parameter querystring met een +/-waarde in minuten voor de [eindpunt](https://aka.ms/luis-endpoint-apis) query.

60 minuten toevoegen: 

https://{Region}.API.cognitive.Microsoft.com/Luis/v2.0/Apps/{appId}?q=TURN de lichten op? **van timezoneoffset is = 60**& uitgebreide = {Booleaanse waarde} & spellingcontrole = {Booleaanse waarde} & fasering = {Booleaanse waarde} & bing-spellen-controle-subscription-key = {tekenreeks} & Meld = {Booleaanse waarde}

60 minuten verwijderen: 

https://{Region}.API.cognitive.Microsoft.com/Luis/v2.0/Apps/{appId}?q=TURN de lichten op? **van timezoneoffset is = 60**& uitgebreide = {Booleaanse waarde} & spellingcontrole = {Booleaanse waarde} & fasering = {Booleaanse waarde} & bing-spellen-controle-subscription-key = {tekenreeks} & Meld = {Booleaanse waarde}

## <a name="c-code-determines-correct-value-of-timezoneoffset"></a>C#-code bepaalt de juiste waarde van timezoneoffset is
De volgende C#-code gebruikt de [TimeZoneInfo](https://docs.microsoft.com/dotnet/api/system.timezoneinfo?view=netframework-4.7.1) van de klasse [FindSystemTimeZoneById](https://docs.microsoft.com/dotnet/api/system.timezoneinfo.findsystemtimezonebyid?view=netframework-4.7.1#examples) methode om te bepalen van de juiste `timezoneOffset` op basis van de systeemtijd:

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
> [Juiste spellingcontrole uit te voeren in deze zelfstudie](luis-tutorial-bing-spellcheck.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions