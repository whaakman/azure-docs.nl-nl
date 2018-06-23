---
title: De functie voor grootschalige gebruiken in de Face-API | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Gebruik de functie voor grootschalige in de Face-API van cognitieve Services.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: f7b3ac57cf6b24c8a90b4ea59757d3a2cfafd781
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344567"
---
# <a name="how-to-use-the-large-scale-feature"></a>Het gebruik van de functie voor grootschalige

Deze handleiding is een geavanceerde artikel over de migratie van de code naar opschalen van bestaande PersonGroup en FaceList LargePersonGroup en LargeFaceList respectievelijk.
Deze handleiding wordt uitgelegd dat het migratieproces op veronderstelling van basisgebruik van PersonGroup en FaceList weten.
Voor het ophalen van bekend bent met basisbewerkingen, Zie andere zelfstudies zoals [identificeren vlakken in afbeeldingen](HowtoIdentifyFacesinImage.md),

Microsoft Face-API heeft twee functies om grootschalige scenario's mogelijk, LargePersonGroup en LargeFaceList, gezamenlijk worden aangeduid als grootschalige bewerkingen onlangs uitgebracht.
LargePersonGroup mag maximaal 1.000.000 personen met een maximum van 248 vlakken en LargeFaceList tot 1.000.000 vlakken kan bevatten.

De grootschalige bewerkingen zijn vergelijkbaar met de conventionele PersonGroup en FaceList, maar enkele belangrijke verschillen vanwege de nieuwe architectuur hebben.
Deze handleiding wordt uitgelegd dat het migratieproces op veronderstelling van basisgebruik van PersonGroup en FaceList weten.
De voorbeelden zijn geschreven in C# met behulp van de clientbibliotheek Face-API.

Om in te schakelen Face zoekprestaties voor identificatie en FindSimilar in grote schaal, moet u een Train-bewerking voor het vooraf verwerken van de LargeFaceList en LargePersonGroup introduceren.
De trainingstijd varieert van seconden tot ongeveer een half uur, afhankelijk van de werkelijke capaciteit.
Tijdens de training periode is het nog steeds mogelijk om uit te voeren identificatie en FindSimilar als een geslaagde training hoofdzakelijk plaats voordat het.
Het nadeel is echter dat de nieuw toegevoegde personen/vlakken niet wordt weergegeven in het resultaat totdat een nieuwe na migratie naar grootschalige training is voltooid.

## <a name="concepts"></a> Concepten

Als u niet bekend met de volgende concepten in deze handleiding bent, de definities vindt u in de [verklarende woordenlijst](../Glossary.md):

- LargePersonGroup: Een verzameling van personen met een capaciteit tot 1.000.000.
- LargeFaceList: Een verzameling vlakken capaciteit tot 1.000.000.
- Train: Een vooraf proces-id/FindSimilar prestaties te garanderen.
- Identificatie: Een of meer vlakken uit een PersonGroup of LargePersonGroup identificeren.
- FindSimilar: Zoek vergelijkbare vlakken uit een FaceList of LargeFaceList.

## <a name="initialization"></a> Stap 1: Machtig de API-aanroep

Wanneer u de clientbibliotheek Face-API, de abonnementssleutel en het abonnement eindpunt doorgegeven de constructor van de klasse FaceServiceClient. Bijvoorbeeld:

```CSharp
string SubscriptionKey = "<Subscription Key>";
// Use your own subscription endpoint corresponding to the subscription key.
string SubscriptionRegion = "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/";
FaceServiceClient FaceServiceClient = new FaceServiceClient(SubscriptionKey, SubscriptionRegion);
```

De abonnementssleutel met de bijbehorende eindpunt kan worden verkregen via de Marketplace-pagina van uw Azure-portal.
Zie [abonnementen](https://azure.microsoft.com/services/cognitive-services/directory/vision/).

## <a name="migrate"></a> Stap 2: Code migratie in actie

Deze sectie is alleen gericht op migreren PersonGroup/FaceList implementatie LargePersonGroup/LargeFaceList.
Hoewel LargePersonGroup/LargeFaceList van PersonGroup/FaceList in de ontwerp- en interne implementatie verschilt, zijn de API-interfaces identiek voor back-compatibiliteit.

Gegevensmigratie wordt niet ondersteund, hebt u in plaats daarvan de LargePersonGroup/LargeFaceList opnieuw te maken.

## <a name="largepersongroup"></a> Stap 2.1: PersonGroup migreren naar LargePersonGroup

De migratie van PersonGroup naar LargePersonGroup is smooth als ze precies dezelfde groepeerniveau bewerkingen delen.

Voor PersonGroup/persoon gerelateerde implementatie, u hoeft alleen te wijzigen in de API-paden of SDK klasse/module LargePersonGroup en LargePersonGroup persoon.

Zie in termen van de gegevensmigratie [hoe toevoegen bespreekt](how-to-add-faces.md) ter referentie.

## <a name="largefacelist"></a> Step 2.2: FaceList migreren naar LargeFaceList

| FaceList API 's | LargeFaceList API 's |
|:---:|:---:|
| Maken | Maken |
| Verwijderen | Verwijderen |
| Ophalen | Ophalen |
| Lijst | Lijst |
| Update | Update |
| - | Trainen |
| - | Trainingsstatus ophalen |

De voorgaande tabel is een vergelijking van bewerkingen tussen FaceList en LargeFaceList-niveau.
Zoals wordt weergegeven, is LargeFaceList wordt geleverd met nieuwe bewerkingen, Train en Training Status ophalen in vergelijking met FaceList.
Ophalen van de LargeFaceList getraind is een voorwaarde van [FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) bewerking terwijl er geen Train voor FaceList vereist is.
Het volgende fragment bevat een Help-functie moet worden gewacht op de training van een LargeFaceList.

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

Voorheen zou standaardgebruik van FaceList met het toevoegen van vlakken en FindSimilar zijn

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

Wanneer u deze migreert naar LargeFaceList, moeten worden

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

Zoals hierboven is beschreven, zijn de data management en het gedeelte FindSimilar bijna hetzelfde.
De enige uitzondering hierop is dat er een nieuwe vooraf verwerken Train-bewerking in de LargeFaceList voltooien moet voordat FindSimilar werkt.

## <a name="train"></a>Stap 3: Suggesties trainen

Hoewel de trein bewerking sneller de [FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) en [identificatie](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), de trainingstijd leidt dit tot slechtere vooral wanneer afkomstig is van grote schaal.
De geschatte trainingstijd in verschillende schalen wordt vermeld in de volgende tabel:

| Schaal (vlakken of personen) | Geschatte tijd Training |
|:---:|:---:|
| 1000 | 1-2-s |
| 10.000 | 5-10-s |
| 100,000 | 1 - 2 min |
| 1.000.000 | 10 - 30 min |

Om u beter gebruikmaken van de functie voor grootschalige, worden een aantal strategieën aanbevolen rekening te houden.

## <a name="interval"></a>Stap 3.1: Tijdsinterval aanpassen

Zoals wordt weergegeven in de `TrainLargeFaceList()`, er is een `timeIntervalInMilliseconds` vertraging van de trainingsstatus van de oneindige proces controleren.
LargeFaceList met meer vlakken beperkt met behulp van een groter interval de aanroep aantallen en de kosten.
Het tijdsinterval moet volgens de verwachte capaciteit van de LargeFaceList worden aangepast.

Een strategie voor dezelfde geldt ook voor LargePersonGroup.
Bij het trainen van een LargePersonGroup met 1.000.000 personen, bijvoorbeeld de `timeIntervalInMilliseconds` 60.000 (ook kan worden de interval 1 minuut).

## <a name="buffer"></a>Stap 3.2 kleinschalige buffer

Personen/vlakken in LargePersonGroup/LargeFaceList zijn doorzoekbaar alleen geldig na getraind.
In een scenario met dynamische nieuwe personen/vlakken voortdurend worden toegevoegd en moet onmiddellijk doorzoekbaar nog training kan langer duren dan gewenste.
Om dit probleem verhelpen, kunt u een extra kleinschalige LargePersonGroup/LargeFaceList als een buffer alleen voor de zojuist toegevoegde vermeldingen.
Deze buffer korter duurt te trainen vanwege veel kleinere en de onmiddellijke zoeken op deze tijdelijke buffer moet werken.
Gebruik deze buffer in combinatie met de training op de master LargePersonGroup/LargeFaceList door het uitvoeren van de master training op een meer sparse interval, bijvoorbeeld in het midden nacht en dagelijks.

Een van de voorbeeldwerkstroom:
1. Maak een master LargePersonGroup/LargeFaceList (master verzameling) en een buffer LargePersonGroup/LargeFaceList (buffer verzameling). De buffer-verzameling is alleen voor toegevoegde personen/vlakken.
1. Nieuwe personen/vlakken toevoegen aan de verzameling en de buffer-verzameling.
1. Alleen training van de buffer-verzameling met een korte tijdsinterval om te controleren of de zojuist toegevoegde vermeldingen toegepast.
1. Identificatie/FindSimilar aanroepen op basis van zowel de master verzameling en de buffer-verzameling en voegt u de resultaten.
1. Wanneer verzameling buffergrootte naar een drempelwaarde of op een niet-actieve systeemtijd verhoogt, een nieuwe buffer-verzameling maken en activeren van de trein op master verzameling.
1. Verwijder de oude buffer verzameling na de einddatum van training voor de master-verzameling.

## <a name="standalone"></a>Stap 3.3 zelfstandige Training

Als een relatief lange latentie aanvaardbaar is, is het niet nodig voor het activeren van de bewerking Train direct na het toevoegen van nieuwe gegevens.
In plaats daarvan worden de trein-bewerking gesplitst van het logische en regelmatig worden geactiveerd.
Deze strategie is geschikt voor dynamische scenario's met aanvaardbare latentie en kan worden toegepast op statische scenario's voor de frequentie Train verder te beperken.

Stel dat er een `TrainLargePersonGroup` functie die vergelijkbaar is met de `TrainLargeFaceList`.
Een typische implementatie van de zelfstandige Training op LargePersonGroup door aan te roepen de [ `Timer` ](https://msdn.microsoft.com/library/system.timers.timer(v=vs.110).aspx) -klasse in `System.Timers` zou zijn:

```CSharp
private static void Main()
{
    // Create a LargePersonGroup.
    const string LargePersonGroupId = "mylargepersongroupid_001";
    const string LargePersonGroupName = "MyLargePersonGroupDisplayName";
    FaceServiceClient.CreateLargePersonGroupAsync(LargePersonGroupId, LargePersonGroupName).Wait();

    // Setup a standalone training at regular intervals.
    const int TimeIntervalForStatus = 1000 * 60; // 1 minute interval for getting training status.
    const double TimeIntervalForTrain = 1000 * 60 * 60; // 1 hour interval for training.
    var trainTimer = new Timer(TimeIntervalForTrain);
    trainTimer.Elapsed += (sender, args) => TrainTimerOnElapsed(LargePersonGroupId, TimeIntervalForStatus);
    trainTimer.AutoReset = true;
    trainTimer.Enabled = true;

    // Other operations like creating persons, adding faces and Identification except for Train.
    // ...
}

private static void TrainTimerOnElapsed(string largePersonGroupId, int timeIntervalInMilliseconds)
{
    TrainLargePersonGroup(largePersonGroupId, timeIntervalInMilliseconds).Wait();
}
```

Zie voor meer informatie over het beheer van gegevens en identificatie-gerelateerde implementaties [toevoegen bespreekt hoe](how-to-add-faces.md) en [hoe identificeren bespreekt in afbeelding](HowtoIdentifyFacesinImage.md).

## <a name="summary"></a> Samenvatting

In deze handleiding hebt u geleerd hoe de bestaande PersonGroup/FaceList code (geen gegevens) migreren naar de LargePersonGroup/LargeFaceList:

- LargePersonGroup en LargeFaceList werkt vergelijkbaar met de PersonGroup/FaceList, behalve Train-bewerking is vereist voor LargeFaceList.
- Nemen juiste train-strategie voor dynamische gegevens-update voor grootschalige dataset.

## <a name="related"></a> Verwante onderwerpen

- [Het identificeren van bespreekt in afbeelding](HowtoIdentifyFacesinImage.md)
- [Het toevoegen van bespreekt](how-to-add-faces.md)
