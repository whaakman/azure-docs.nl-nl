---
title: Toevoegen van vlakken met de Face-API | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Gebruik de Face-API in cognitieve Services vlakken toevoegen aan installatiekopieën.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 3306c13d6c3d231ddbda38cfcbc5419fcdbd30db
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344471"
---
# <a name="how-to-add-faces"></a>Het toevoegen van vlakken

Deze handleiding laat zien dat de aanbevolen procedure om het grote aantal personen en vlakken toevoegen aan een PersonGroup.
De strategie voor dezelfde geldt ook voor FaceList en LargePersonGroup.
De voorbeelden zijn geschreven in C# met behulp van de clientbibliotheek Face-API.

## <a name="step1"></a> Stap 1: initialisatie

Meerdere variabelen worden gedeclareerd en een Help-functie wordt geïmplementeerd als u de aanvragen plannen.

- `PersonCount` is het totale aantal personen.
- `CallLimitPerSecond` het maximum aantal aanroepen per seconde volgens de abonnement-laag is.
- `_timeStampQueue` is een wachtrij om vast te leggen van de aanvraag tijdstempels.
- `await WaitCallLimitPerSecondAsync()` Wacht totdat de volgende aanvraag verzenden geldig is.

```CSharp
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

## <a name="step2"></a> Stap 2: Machtig de API-aanroep

Wanneer u een clientbibliotheek, wordt de abonnementssleutel doorgegeven de constructor van de klasse FaceServiceClient. Bijvoorbeeld:

```CSharp
FaceServiceClient faceServiceClient = new FaceServiceClient("<Subscription Key>");
```

De abonnementssleutel kan worden verkregen via de Marketplace-pagina van uw Azure-portal. Zie [abonnementen](https://www.microsoft.com/cognitive-services/en-us/sign-up).

## <a name="step3"></a> Stap 3: De PersonGroup maken

Een PersonGroup met de naam 'MyPersonGroup' is gemaakt voor het opslaan van de personen.
De aanvraagtijd is in de wachtrij voor `_timeStampQueue` om te controleren of de algehele validatie.

```CSharp
const string personGroupId = "mypersongroupid";
const string personGroupName = "MyPersonGroup";
_timeStampQueue.Enqueue(DateTime.UtcNow);
await faceServiceClient.CreatePersonGroupAsync(personGroupId, personGroupName);
```

## <a name="step4"></a> Stap 4: De personen op de PersonGroup maken

Personen die gelijktijdig worden gemaakt en `await WaitCallLimitPerSecondAsync()` wordt ook toegepast om te voorkomen dat de aanroep is overschreden.

```CSharp
CreatePersonResult[] persons = new CreatePersonResult[PersonCount];
Parallel.For(0, PersonCount, async i =>
{
    await WaitCallLimitPerSecondAsync();

    string personName = $"PersonName#{i}";
    persons[i] = await faceServiceClient.CreatePersonAsync(personGroupId, personName);
});
```

## <a name="step5"></a> Stap 5: Vlakken toevoegen aan de personen

Toe te voegen vlakken aan andere personen gelijktijdig worden uitgevoerd, terwijl voor een bepaalde persoon sequentiële is.
Opnieuw `await WaitCallLimitPerSecondAsync()` wordt aangeroepen om ervoor te zorgen de frequentie van de aanvraag binnen het bereik van beperking.

```CSharp
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

## <a name="summary"></a> Samenvatting

U kunt het proces van het maken van een PersonGroup met grote aantallen personen en vlakken hebt geleerd in deze handleiding. Verschillende herinneringen:

- Deze strategie geldt ook voor FaceList en LargePersonGroup.
- Toevoegen/verwijderen vlakken naar andere personen in LargePersonGroup of FaceLists kunnen gelijktijdig worden verwerkt.
- Bewerkingen voor een specifieke FaceList of persoon in LargePersonGroup moeten opeenvolgend worden uitgevoerd.
- Als u wilt behouden de eenvoud, wordt de verwerking van mogelijke uitzondering weggelaten in deze handleiding. Als u meer robuustheid verbeteren wilt, moet de juiste opnieuw beleid worden toegepast.

Hieronder vindt u een snelle herinnering van de functies die eerder zijn beschreven en gedemonstreerd:

- Maken van PersonGroups met behulp van de [PersonGroup - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API
- Maken van personen met behulp van de [PersonGroup persoon - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API
- Vlakken toe te voegen aan personen met behulp van de [PersonGroup persoon - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API

## <a name="related"></a> Verwante onderwerpen
- [Het identificeren van bespreekt in afbeelding](HowtoIdentifyFacesinImage.md)
- [Het vaststellen van bespreekt in afbeelding](HowtoDetectFacesinImage.md)
- [Het gebruik van de functie voor grootschalige](how-to-use-large-scale.md)
