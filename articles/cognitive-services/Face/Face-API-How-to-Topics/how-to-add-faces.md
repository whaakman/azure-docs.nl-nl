---
title: 'Voorbeeld: Gezichten toevoegen aan een PersonGroup - Face-API'
titleSuffix: Azure Cognitive Services
description: De Face-API gebruiken voor het toevoegen van gezichten in afbeeldingen.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 04/10/2019
ms.author: sbowles
ms.openlocfilehash: 04fe9251ba124ed5d218daf915339c7f84efdeb6
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64704185"
---
# <a name="how-to-add-faces-to-a-persongroup"></a>Gezichten toevoegen aan een PersonGroup

Deze handleiding leest u de aanbevolen procedures voor het toevoegen van een groot aantal personen en gezichten op een PersonGroup-object. De strategie voor dezelfde geldt ook voor LargePersonGroup FaceList en LargeFaceList. In dit voorbeeld is geschreven in C# met de Face-API .NET-clientbibliotheek.

## <a name="step-1-initialization"></a>Stap 1: Initialisatie

De volgende code declareert verschillende variabelen en implementeert toevoegen van een Help-functie voor het plannen van de face-aanvragen.

- `PersonCount` is het totale aantal personen.
- `CallLimitPerSecond` is het maximum aantal aanroepen per seconde op basis van de abonnementslaag.
- `_timeStampQueue` is een wachtrij om aanvraag tijdstempels vast te leggen.
- `await WaitCallLimitPerSecondAsync()` wacht totdat het geldig is voor het verzenden van de volgende aanvraag.

```csharp
const int PersonCount = 10000;
const int CallLimitPerSecond = 10;
static Queue<DateTime> _timeStampQueue = new Queue<DateTime>(CallLimitPerSecond);

static async Task WaitCallLimitPerSecondAsync()
{
    Monitor.Enter(_timeStampQueue);
    try
    {
        if (_timeStampQueue.Count >= CallLimitPerSecond)
        {
            TimeSpan timeInterval = DateTime.UtcNow - _timeStampQueue.Peek();
            if (timeInterval < TimeSpan.FromSeconds(1))
            {
                await Task.Delay(TimeSpan.FromSeconds(1) - timeInterval);
            }
            _timeStampQueue.Dequeue();
        }
        _timeStampQueue.Enqueue(DateTime.UtcNow);
    }
    finally
    {
        Monitor.Exit(_timeStampQueue);
    }
}
```

## <a name="step-2-authorize-the-api-call"></a>Stap 2: De API-aanroep autoriseren

Wanneer u een clientbibliotheek gebruikt, moet u uw abonnementssleutel doorgeven aan de constructor van de klasse FaceServiceClient. Bijvoorbeeld:

```csharp
FaceServiceClient faceServiceClient = new FaceServiceClient("<Subscription Key>");
```

De abonnementssleutel kan worden verkregen vanuit de Marketplace-pagina van uw Azure Portal. Zie [Abonnementen](https://www.microsoft.com/cognitive-services/sign-up).

## <a name="step-3-create-the-persongroup"></a>Stap 3: De PersonGroup maken

Een PersonGroup met de naam 'MyPersonGroup' wordt gemaakt om de personen op te slaan.
De aanvraagtijd wordt in de wachtrij `_timeStampQueue` geplaatst om de algehele validatie te controleren.

```csharp
const string personGroupId = "mypersongroupid";
const string personGroupName = "MyPersonGroup";
_timeStampQueue.Enqueue(DateTime.UtcNow);
await faceServiceClient.CreatePersonGroupAsync(personGroupId, personGroupName);
```

## <a name="step-4-create-the-persons-to-the-persongroup"></a>Stap 4: De personen in de PersonGroup maken

Personen worden gelijktijdig gemaakt en `await WaitCallLimitPerSecondAsync()` wordt ook toegepast om te voorkomen dat de aanroeplimiet wordt overschreden.

```csharp
CreatePersonResult[] persons = new CreatePersonResult[PersonCount];
Parallel.For(0, PersonCount, async i =>
{
    await WaitCallLimitPerSecondAsync();

    string personName = $"PersonName#{i}";
    persons[i] = await faceServiceClient.CreatePersonAsync(personGroupId, personName);
});
```

## <a name="step-5-add-faces-to-the-persons"></a>Stap 5: Gezichten voor de personen toevoegen

Toevoegen van gezichten voor de verschillende personen wordt gelijktijdig verwerkt, hoewel het voor een specifieke persoon opeenvolgend gebeurt.
Nogmaals, `await WaitCallLimitPerSecondAsync()` wordt aangeroepen om ervoor te zorgen de frequentie van de aanvraag binnen het bereik van de beperking is.

```csharp
Parallel.For(0, PersonCount, async i =>
{
    Guid personId = persons[i].PersonId;
    string personImageDir = @"/path/to/person/i/images";

    foreach (string imagePath in Directory.GetFiles(personImageDir, "*.jpg"))
    {
        await WaitCallLimitPerSecondAsync();

        using (Stream stream = File.OpenRead(imagePath))
        {
            await faceServiceClient.AddPersonFaceAsync(personGroupId, personId, stream);
        }
    }
});
```

## <a name="summary"></a>Samenvatting

U hebt het proces voor het maken van een PersonGroup met een groot aantal personen en gezichten in deze handleiding geleerd. Verschillende herinneringen:

- Deze strategie geldt ook voor FaceList en LargePersonGroup.
- Gezichten toevoegen/verwijderen naar verschillende FaceLists of personen in LargePersonGroup kan gelijktijdig worden verwerkt.
- Dezelfde bewerkingen op een specifieke FaceList of persoon in LargePersonGroup moeten opeenvolgend worden uitgevoerd.
- Om het eenvoudig te houden, wordt de verwerking van mogelijke uitzonderingen weggelaten in deze handleiding. Als de robuustheid wilt verbeteren, moet het juiste beleid voor opnieuw proberen worden toegepast.

Hier volgt een snelle herinnering van de functies die eerder werden uitgelegd en gedemonstreerd:

- Het maken van PersonGroups met behulp van de API [PersonGroup - Maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)
- Het maken van personen met behulp van de API [PersonGroup Person - Maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)
- Gezichten aan personen toevoegen met behulp van de API [PersonGroup Person - Gezicht toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b)

## <a name="related-topics"></a>Verwante onderwerpen

- [Gezichten in een afbeelding identificeren](HowtoIdentifyFacesinImage.md)
- [Gezichten in een afbeelding detecteren](HowtoDetectFacesinImage.md)
- [De functie Grootschalig gebruiken](how-to-use-large-scale.md)
