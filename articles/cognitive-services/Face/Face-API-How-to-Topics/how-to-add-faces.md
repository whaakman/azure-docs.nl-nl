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
ms.openlocfilehash: 83aef90702e4a4cc4fd9bdfda486841f9b2a63a4
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66124503"
---
# <a name="add-faces-to-a-persongroup"></a>Gezichten toevoegen aan een PersonGroup

Deze handleiding laat zien hoe u een groot aantal personen en gezichten toevoegen aan een PersonGroup-object. De strategie voor dezelfde geldt ook voor LargePersonGroup, FaceList en LargeFaceList-objecten. In dit voorbeeld is geschreven in C# met behulp van de Azure Cognitive Services Face-API .NET-clientbibliotheek.

## <a name="step-1-initialization"></a>Stap 1: De initialisatie

De volgende code declareert verschillende variabelen en implementeert toevoegen van een Help-functie voor het plannen van de face-aanvragen:

- `PersonCount` is het totale aantal personen.
- `CallLimitPerSecond` is het maximum aantal aanroepen per seconde op basis van de abonnementslaag.
- `_timeStampQueue` is een wachtrij om aanvraag tijdstempels vast te leggen.
- `await WaitCallLimitPerSecondAsync()` Wacht totdat de parameter geldig is voor het verzenden van de volgende aanvraag.

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

Als u de abonnementssleutel, gaat u naar de Azure Marketplace via Azure portal. Zie voor meer informatie, [abonnementen](https://www.microsoft.com/cognitive-services/sign-up).

## <a name="step-3-create-the-persongroup"></a>Stap 3: De PersonGroup maken

Een PersonGroup met de naam 'MyPersonGroup' wordt gemaakt om de personen op te slaan.
De aanvraagtijd wordt in de wachtrij `_timeStampQueue` geplaatst om de algehele validatie te controleren.

```csharp
const string personGroupId = "mypersongroupid";
const string personGroupName = "MyPersonGroup";
_timeStampQueue.Enqueue(DateTime.UtcNow);
await faceServiceClient.CreatePersonGroupAsync(personGroupId, personGroupName);
```

## <a name="step-4-create-the-persons-for-the-persongroup"></a>Stap 4: De personen die voor de PersonGroup maken

Personen die gelijktijdig, worden gemaakt en `await WaitCallLimitPerSecondAsync()` wordt ook toegepast om te voorkomen dat de aanroep is overschreden.

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

Gezichten toegevoegd aan verschillende personen worden gelijktijdig kunnen verwerkt. Gezichten toegevoegd voor een specifieke persoon worden opeenvolgend verwerkt;.
Nogmaals, `await WaitCallLimitPerSecondAsync()` wordt aangeroepen om ervoor te zorgen dat de frequentie van de aanvraag binnen het bereik van beperking.

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

In deze handleiding, hebt u het proces voor het maken van een PersonGroup met een groot aantal personen en gezichten geleerd. Verschillende herinneringen:

- Deze strategie is ook van toepassing op FaceLists en LargePersonGroups.
- Toevoegen of verwijderen van gezichten naar andere personen in LargePersonGroups of FaceLists worden gelijktijdig kunnen verwerkt.
- Toevoegen of verwijderen van gezichten op een specifieke FaceList of persoon in een LargePersonGroup worden opeenvolgend uitgevoerd.
- Verwerken van een mogelijke uitzondering is voor het gemak weggelaten in deze handleiding. Als u meer robuustheid te verbeteren wilt, het juiste beleid van toepassing.

De volgende onderdelen zijn uitgelegd en gedemonstreerd:

- PersonGroups maken met behulp van de [PersonGroup - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API.
- Personen maken met behulp van de [PersonGroup persoon - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API.
- Gezichten voor de personen toevoegen met behulp van de [PersonGroup persoon - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API.

## <a name="related-topics"></a>Verwante onderwerpen

- [Identificeer gezichten in een afbeelding](HowtoIdentifyFacesinImage.md)
- [Detecteer gezichten in een afbeelding](HowtoDetectFacesinImage.md)
- [Gebruik de functie voor grootschalige](how-to-use-large-scale.md)
