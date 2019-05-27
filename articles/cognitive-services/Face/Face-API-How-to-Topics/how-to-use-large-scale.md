---
title: 'Voorbeeld: De grootschalige voorziening - Face-API gebruiken'
titleSuffix: Azure Cognitive Services
description: Gebruik de functie Grootschalig in de Face-API.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 05/01/2019
ms.author: sbowles
ms.openlocfilehash: 5a4085f713d66859a464ab59b00d856921db8ec3
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66124486"
---
# <a name="example-use-the-large-scale-feature"></a>Voorbeeld: De functie Grootschalig gebruiken

Deze handleiding is een geavanceerde artikel over het omhoog schalen van bestaande PersonGroup en FaceList objecten aan LargePersonGroup en LargeFaceList objecten, respectievelijk. Deze handleiding leest u het migratieproces. Wordt ervan uitgegaan dat een enigszins bekend bent met PersonGroup en FaceList objecten, de [Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4) bewerking en de face-opname-functies. Zie voor meer informatie over deze onderwerpen, de [Gezichtsherkenning-](../concepts/face-recognition.md) conceptuele handleiding.

LargePersonGroup en LargeFaceList gezamenlijk worden aangeduid als grootschalige bewerkingen. LargePersonGroup kan tot wel 1 miljoen personen, elk met een maximum van 248 gezichten bevatten. LargeFaceList kan tot wel 1 miljoen gezichten bevatten. De grootschalige bewerkingen zijn vergelijkbaar met de conventionele PersonGroup en FaceList maar enkele verschillen vanwege de nieuwe architectuur. 

De voorbeelden zijn geschreven in C# met behulp van de clientbibliotheek van Azure Cognitive Services Face-API.

> [!NOTE]
> Om in te schakelen gezichten zoekprestaties voor identificatie en FindSimilar in grote schaal, voeren een bewerking van de trein als u wilt de LargeFaceList en LargePersonGroup voorverwerken. De trainingstijd varieert van seconden tot ongeveer een half uur op basis van de werkelijke capaciteit. Tijdens de periode training is het mogelijk uit te voeren identificatie en FindSimilar als voordat het besturingssysteem van een geslaagde training is uitgevoerd. Het nadeel is dat de nieuw toegevoegde personen en gezichten worden niet in het resultaat weergegeven tot een nieuwe nadat de migratie voor grootschalige training is voltooid.

## <a name="step-1-initialize-the-client-object"></a>Stap 1: Het clientobject initialiseren

Wanneer u de Face-API-clientbibliotheek, worden de abonnementssleutel en het abonnement eindpunt doorgegeven via de constructor van de klasse FaceServiceClient. Bijvoorbeeld:

```CSharp
string SubscriptionKey = "<Subscription Key>";
// Use your own subscription endpoint corresponding to the subscription key.
string SubscriptionRegion = "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/";
FaceServiceClient FaceServiceClient = new FaceServiceClient(SubscriptionKey, SubscriptionRegion);
```

Als u de abonnementssleutel met het bijbehorende eindpunt, gaat u naar de Azure Marketplace via Azure portal.
Zie voor meer informatie, [abonnementen](https://azure.microsoft.com/services/cognitive-services/directory/vision/).

## <a name="step-2-code-migration"></a>Stap 2: Code migreren

In deze sectie richt zich op het migreren van PersonGroup of FaceList-implementatie naar LargePersonGroup of LargeFaceList. Hoewel LargePersonGroup of LargeFaceList PersonGroup of FaceList in ontwerp en de interne implementatie verschilt, zijn de API-interfaces vergelijkbaar voor achterwaartse compatibiliteit.

De gegevensmigratie wordt niet ondersteund. U maken opnieuw de LargePersonGroup of LargeFaceList in plaats daarvan.

### <a name="migrate-a-persongroup-to-a-largepersongroup"></a>Een PersonGroup migreren naar een LargePersonGroup

Migratie van een PersonGroup naar een LargePersonGroup is eenvoudig. Ze delen precies dezelfde op groepsniveau bewerkingen.

Voor de implementatie PersonGroup of persoon die zijn gerelateerd is het nodig om te wijzigen van alleen het API-paden of de SDK-klasse/module LargePersonGroup en LargePersonGroup persoon.

Toevoegen alle gezichten en personen van de PersonGroup aan de nieuwe LargePersonGroup. Zie voor meer informatie, [toevoegen gezichten](how-to-add-faces.md).

### <a name="migrate-a-facelist-to-a-largefacelist"></a>Een FaceList migreren naar een LargeFaceList

| FaceList-API's | LargeFaceList-API's |
|:---:|:---:|
| Maken | Maken |
| Verwijderen | Verwijderen |
| Ophalen | Ophalen |
| Lijst | Lijst |
| Bijwerken | Bijwerken |
| - | Trainen |
| - | Trainingsstatus ophalen |

De bovenstaande tabel bevat een vergelijking van bewerkingen tussen FaceList en LargeFaceList op lijstniveau. Zoals wordt weergegeven, is LargeFaceList wordt geleverd met nieuwe bewerkingen, trainen en Training-Status ophalen in vergelijking met FaceList. De LargeFaceList training is een voorwaarde van de [FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) bewerking. Training is niet vereist voor FaceList. Het volgende codefragment wordt een Help-functie moet worden gewacht voor de training van een LargeFaceList:

```CSharp
/// <summary>
/// Helper function to train LargeFaceList and wait for finish.
/// </summary>
/// <remarks>
/// The time interval can be adjusted considering the following factors:
/// - The training time which depends on the capacity of the LargeFaceList.
/// - The acceptable latency for getting the training status.
/// - The call frequency and cost.
///
/// Estimated training time for LargeFaceList in different scale:
/// -     1,000 faces cost about  1 to  2 seconds.
/// -    10,000 faces cost about  5 to 10 seconds.
/// -   100,000 faces cost about  1 to  2 minutes.
/// - 1,000,000 faces cost about 10 to 30 minutes.
/// </remarks>
/// <param name="largeFaceListId">The Id of the LargeFaceList for training.</param>
/// <param name="timeIntervalInMilliseconds">The time interval for getting training status in milliseconds.</param>
/// <returns>A task of waiting for LargeFaceList training finish.</returns>
private static async Task TrainLargeFaceList(
    string largeFaceListId,
    int timeIntervalInMilliseconds = 1000)
{
    // Trigger a train call.
    await FaceServiceClient.TrainLargeFaceListAsync(largeFaceListId);

    // Wait for training finish.
    while (true)
    {
        Task.Delay(timeIntervalInMilliseconds).Wait();
        var status = await FaceServiceClient.GetLargeFaceListTrainingStatusAsync(largeFaceListId);

        if (status.Status == Status.Running)
        {
            continue;
        }
        else if (status.Status == Status.Succeeded)
        {
            break;
        }
        else
        {
            throw new Exception("The train operation is failed!");
        }
    }
}
```

Eerder bekeken standaardgebruik van FaceList met toegevoegde gezichten en FindSimilar als volgt uit:

```CSharp
// Create a FaceList.
const string FaceListId = "myfacelistid_001";
const string FaceListName = "MyFaceListDisplayName";
const string ImageDir = @"/path/to/FaceList/images";
FaceServiceClient.CreateFaceListAsync(FaceListId, FaceListName).Wait();

// Add Faces to the FaceList.
Parallel.ForEach(
    Directory.GetFiles(ImageDir, "*.jpg"),
    async imagePath =>
        {
            using (Stream stream = File.OpenRead(imagePath))
            {
                await FaceServiceClient.AddFaceToFaceListAsync(FaceListId, stream);
            }
        });

// Perform FindSimilar.
const string QueryImagePath = @"/path/to/query/image";
var results = new List<SimilarPersistedFace[]>();
using (Stream stream = File.OpenRead(QueryImagePath))
{
    var faces = FaceServiceClient.DetectAsync(stream).Result;
    foreach (var face in faces)
    {
        results.Add(await FaceServiceClient.FindSimilarAsync(face.FaceId, FaceListId, 20));
    }
}
```

Wanneer deze naar LargeFaceList migreert, wordt het volgende:

```CSharp
// Create a LargeFaceList.
const string LargeFaceListId = "mylargefacelistid_001";
const string LargeFaceListName = "MyLargeFaceListDisplayName";
const string ImageDir = @"/path/to/FaceList/images";
FaceServiceClient.CreateLargeFaceListAsync(LargeFaceListId, LargeFaceListName).Wait();

// Add Faces to the LargeFaceList.
Parallel.ForEach(
    Directory.GetFiles(ImageDir, "*.jpg"),
    async imagePath =>
        {
            using (Stream stream = File.OpenRead(imagePath))
            {
                await FaceServiceClient.AddFaceToLargeFaceListAsync(LargeFaceListId, stream);
            }
        });

// Train() is newly added operation for LargeFaceList.
// Must call it before FindSimilarAsync() to ensure the newly added faces searchable.
await TrainLargeFaceList(LargeFaceListId);

// Perform FindSimilar.
const string QueryImagePath = @"/path/to/query/image";
var results = new List<SimilarPersistedFace[]>();
using (Stream stream = File.OpenRead(QueryImagePath))
{
    var faces = FaceServiceClient.DetectAsync(stream).Result;
    foreach (var face in faces)
    {
        results.Add(await FaceServiceClient.FindSimilarAsync(face.FaceId, largeFaceListId: LargeFaceListId));
    }
}
```

Zoals eerder weergegeven zijn het gegevensbeheer en het onderdeel FindSimilar bijna hetzelfde. De enige uitzondering hierop is dat een nieuwe voorverwerking Train-bewerking moet worden voltooid in de LargeFaceList voordat FindSimilar werkt.

## <a name="step-3-train-suggestions"></a>Stap 3: Suggesties van de trein

Hoewel de bewerking van de trein versnelt [FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) en [identificatie](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), de trainingstijd leidt dit tot slechtere, met name wanneer ze op grote schaal. De trainingstijd geschatte in verschillende schalen wordt in de volgende tabel vermeld.

| Schaal voor personen of gezichten | Geschatte trainingstijd |
|:---:|:---:|
| 1000 | 1-2 sec |
| 10.000 | 5-10 sec. |
| 100.000 | 1-2 min |
| 1.000.000 | 10-30 min |

Beter gebruik van de functie voor grootschalige, wordt aangeraden de volgende strategieën.

### <a name="step-31-customize-time-interval"></a>Stap 3.1: Tijdsinterval aanpassen

Zoals wordt weergegeven in `TrainLargeFaceList()`, er is een bepaalde periode in milliseconden voor het uitstellen van de trainingsstatus van de oneindige proces controleren. Als u een langer interval gebruikt voor LargeFaceList met meer gezichten, vermindert u het aantal oproepen en de kosten. Het tijdsinterval op basis van de verwachte capaciteit van de LargeFaceList aanpassen.

De strategie voor dezelfde geldt ook voor LargePersonGroup. Bijvoorbeeld, wanneer u een LargePersonGroup trainen met 1 miljoen personen, `timeIntervalInMilliseconds` mogelijk 60.000, dit is een interval van 1 minuut.

### <a name="step-32-small-scale-buffer"></a>Stap 3.2: Kleine buffer

Personen of gezichten in een LargePersonGroup of een LargeFaceList zijn doorzoekbaar pas na wordt getraind. In een scenario voor dynamische, nieuwe personen of gezichten voortdurend worden toegevoegd en moeten onmiddellijk doorzoekbare nog training duurt mogelijk langer dan de gewenste. 

Als u wilt dit probleem verhelpen, gebruikt u een extra kleinschalige LargePersonGroup of LargeFaceList als een buffer alleen voor de toegevoegde vermeldingen. Deze buffer wordt een kortere tijd met het trainen van vanwege de kleinere grootte. De mogelijkheid van Direct zoeken op deze tijdelijke buffer kunnen worden gebruikt. Gebruik deze buffer in combinatie met de training op de master LargePersonGroup of LargeFaceList door het uitvoeren van de master training in een interval van sparser. Voorbeelden zijn in het midden van de nacht en de dagelijkse.

Voorbeeld van een werkstroom:

1. Maak een master LargePersonGroup of LargeFaceList, dit is de master-verzameling. Maak een buffer LargePersonGroup of LargeFaceList, dit de buffer-verzameling is. De buffer-verzameling is alleen voor toegevoegde personen of gezichten.
1. Nieuwe personen of gezichten toevoegen aan zowel de master-collectie en de buffer-verzameling.
1. Alleen train de buffer-verzameling met een interval van korte tijd om ervoor te zorgen dat de zojuist toegevoegde vermeldingen van kracht.
1. Aanroep-id of FindSimilar tegen zowel de master-collectie en de buffer-verzameling. De resultaten samenvoegen.
1. Wanneer de grootte van de verzameling op een latentiedrempelwaarde van of op een niet-actieve tijd van systeem, moet u een nieuwe buffer-verzameling maken. Activeren van de trein-bewerking op de master-verzameling.
1. Verwijder de oude buffer-collectie nadat de bewerking van de trein is voltooid op de master-verzameling.

### <a name="step-33-standalone-training"></a>Stap 3.3: Zelfstandige training

Als een relatief lange latentie aanvaardbaar is, is het niet die nodig zijn voor de trigger de juiste Train-bewerking na het toevoegen van nieuwe gegevens. De Train-bewerking kan ook worden gescheiden van de hoofdlogica en op regelmatige tijden worden geactiveerd. Deze strategie is geschikt voor dynamische scenario's met aanvaardbare latentie. Het kan worden toegepast op statische scenario's om de frequentie van de trein verder te beperken.

Stel dat er een `TrainLargePersonGroup` die vergelijkbaar is met de functie `TrainLargeFaceList`. Een typische implementatie van de zelfstandige training op een LargePersonGroup door het aanroepen van de [ `Timer` ](https://msdn.microsoft.com/library/system.timers.timer(v=vs.110).aspx) in klasse `System.Timers` is:

```CSharp
private static void Main()
{
    // Create a LargePersonGroup.
    const string LargePersonGroupId = "mylargepersongroupid_001";
    const string LargePersonGroupName = "MyLargePersonGroupDisplayName";
    FaceServiceClient.CreateLargePersonGroupAsync(LargePersonGroupId, LargePersonGroupName).Wait();

    // Set up standalone training at regular intervals.
    const int TimeIntervalForStatus = 1000 * 60; // 1-minute interval for getting training status.
    const double TimeIntervalForTrain = 1000 * 60 * 60; // 1-hour interval for training.
    var trainTimer = new Timer(TimeIntervalForTrain);
    trainTimer.Elapsed += (sender, args) => TrainTimerOnElapsed(LargePersonGroupId, TimeIntervalForStatus);
    trainTimer.AutoReset = true;
    trainTimer.Enabled = true;

    // Other operations like creating persons, adding faces, and identification, except for Train.
    // ...
}

private static void TrainTimerOnElapsed(string largePersonGroupId, int timeIntervalInMilliseconds)
{
    TrainLargePersonGroup(largePersonGroupId, timeIntervalInMilliseconds).Wait();
}
```

Zie voor meer informatie over het gegevensbeheer en implementaties met betrekking tot identificatie [toevoegen gezichten](how-to-add-faces.md) en [Identificeer gezichten in een afbeelding](HowtoIdentifyFacesinImage.md).

## <a name="summary"></a>Samenvatting

In deze handleiding, hebt u geleerd over het migreren van de bestaande PersonGroup of FaceList code, geen gegevens naar de LargePersonGroup of LargeFaceList:

- LargePersonGroup en LargeFaceList werken die vergelijkbaar is met PersonGroup of FaceList, behalve dat de bewerking van de trein is vereist voor LargeFaceList.
- Breng de juiste strategie van de trein naar dynamische gegevens bijwerken voor grootschalige gegevenssets.

## <a name="next-steps"></a>Volgende stappen

Ga als volgt een handleiding voor meer informatie over gezichten toevoegen aan een PersonGroup of de bewerking identificeren op een PersonGroup niet uitvoeren.

- [Gezichten toevoegen](how-to-add-faces.md)
- [Identificeer gezichten in een afbeelding](HowtoIdentifyFacesinImage.md)