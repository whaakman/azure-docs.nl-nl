---
title: 'Voorbeeld: De functie voor grootschalige verwerking gebruiken - Face-API'
titleSuffix: Azure Cognitive Services
description: Gebruik de functie Grootschalig in de Face-API.
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 9289f7178a6e285b447041937f191d283fc2f2f0
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55222895"
---
# <a name="example-how-to-use-the-large-scale-feature"></a>Voorbeeld: De functie voor grootschalige verwerking gebruiken

Deze handleiding is een geavanceerd artikel over de migratie van code om een bestaande PersonGroup en FaceList omhoog te schalen naar LargePersonGroup en LargeFaceList.
Deze handleiding toont u het migratieproces, waarbij ervan wordt uitgegaan dat u basiskennis hebt van het gebruik van PersonGroup en FaceList.
Voor meer informatie over basisbewerkingen raadpleegt u andere zelfstudies, zoals [Gezichten in een afbeelding identificeren](HowtoIdentifyFacesinImage.md),

De Face-API heeft onlangs twee functies vrijgegeven voor grootschalige scenario's: LargePersonGroup en LargeFaceList, gezamenlijk aangeduid als grootschalige bewerkingen.
LargePersonGroup kan maximaal 1.000.000 personen bevatten met een maximum van 248 gezichten en LargeFaceList kan tot 1.000.000 gezichten bevatten.

De grootschalige bewerkingen zijn vergelijkbaar met de conventionele PersonGroup en FaceList, maar er zijn enkele belangrijke verschillen vanwege de nieuwe architectuur.
Deze handleiding toont u het migratieproces, waarbij ervan wordt uitgegaan dat u basiskennis hebt van het gebruik van PersonGroup en FaceList.
De voorbeelden zijn geschreven in C# met de Face-API-clientbibliotheek.

Als u Face-zoekprestaties voor Identification en FindSimilar op grote schaal wilt inschakelen, moet u LargeFaceList en LargePersonGroup voorbereiden door een Train-bewerking uit te voeren.
De trainingstijd varieert van seconden tot ongeveer een half uur, afhankelijk van de werkelijke capaciteit.
Tijdens de trainingsperiode is het nog steeds mogelijk Identification en FindSimilar uit te voeren als er al eerder een geslaagde training is uitgevoerd.
Het nadeel is echter dat de nieuw toegevoegde personen/gezichten niet worden weergegeven in het resultaat totdat er een nieuwe grootschalige training na de migratie is uitgevoerd.

## <a name="concepts"></a>Concepten

Als u niet bekend met de volgende concepten in deze handleiding, bekijk dan de [verklarende woordenlijst](../Glossary.md) voor de definities:

- LargePersonGroup: Een verzameling personen met een maximale capaciteit van 1.000.000.
- LargeFaceList: Een verzameling gezichten met een maximale capaciteit van 1.000.000.
- Trainen: Een vooraf uitgevoerd proces om de prestaties van Identification/FindSimilar te waarborgen.
- Identification: Een of meer gezichten in een PersonGroup of LargePersonGroup identificeren.
- FindSimilar: Soortgelijke gezichten zoeken in een FaceList of LargeFaceList.

## <a name="step-1-authorize-the-api-call"></a>Stap 1: De API-aanroep autoriseren

Als u gebruikmaakt van de Face-API-clientbibliotheek, worden de abonnementssleutel en het abonnementseindpunt doorgegeven via de constructor van de klasse FaceServiceClient. Bijvoorbeeld:

```CSharp
string SubscriptionKey = "<Subscription Key>";
// Use your own subscription endpoint corresponding to the subscription key.
string SubscriptionRegion = "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/";
FaceServiceClient FaceServiceClient = new FaceServiceClient(SubscriptionKey, SubscriptionRegion);
```

De abonnementssleutel met het bijbehorende eindpunt kan worden verkregen vanuit de Marketplace-pagina van uw Azure Portal.
Zie [Abonnementen](https://azure.microsoft.com/services/cognitive-services/directory/vision/).

## <a name="step-2-code-migration-in-action"></a>Stap 2: Codemigratie in actie

In deze sectie richten we ons alleen op de migratie van de PersonGroup/FaceList-implementatie naar LargePersonGroup/LargeFaceList.
Hoewel het ontwerp en de interne implementatie van LargePersonGroup/LargeFaceList en PersonGroup/FaceList verschillen, zijn de API-interfaces vergelijkbaar voor back-compatibiliteit.

Gegevensmigratie wordt niet ondersteund, u moet de LargePersonGroup/LargeFaceList opnieuw maken.

## <a name="step-21-migrate-persongroup-to-largepersongroup"></a>Stap 2.1: PersonGroup migreren naar LargePersonGroup

De migratie van PersonGroup naar LargePersonGroup is gemakkelijk omdat ze precies dezelfde bewerkingen op groepsniveau delen.

Voor de implementatie van PersonGroup/Person hoeft u alleen de API-paden of de SDK-klasse/module te wijzigen van LargePersonGroup en LargePersonGroup Person.

Zie [Gezichten toevoegen](how-to-add-faces.md) als referentie voor de gegevensmigratie.

## <a name="step-22-migrate-facelist-to-largefacelist"></a>Stap 2.2: FaceList migreren naar LargeFaceList

| FaceList-API's | LargeFaceList-API's |
|:---:|:---:|
| Maken | Maken |
| Verwijderen | Verwijderen |
| Ophalen | Ophalen |
| Lijst | Lijst |
| Update | Update |
| - | Trainen |
| - | Trainingsstatus ophalen |

De bovenstaande tabel bevat een vergelijking van bewerkingen tussen FaceList en LargeFaceList op lijstniveau.
Zoals weergegeven, wordt LargeFaceList geleverd met nieuwe bewerkingen (Trainen en Trainingsstatus ophalen) als uitbreiding op FaceList.
Het trainen van LargeFaceList is een voorwaarde voor de [FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237)-bewerking, terwijl Trainen daarentegen niet is vereist voor FaceList.
Het volgende codefragment is een Help-functie tijdens het wachten op de training van een LargeFaceList.

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

De standaardprocedure voor gezichten toevoegen en FindSimilar in FaceList was vroeger als volgt

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

Na de migratie naar LargeFaceList is het als volgt

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

Zoals hierboven beschreven, worden het gegevensbeheer en het onderdeel FindSimilar bijna op dezelfde manier uitgevoerd.
De enige uitzondering hierop is dat eerst een nieuwe Train-bewerking moet worden voltooid in LargeFaceList voordat FindSimilar werkt.

## <a name="step-3-train-suggestions"></a>Stap 3: Suggesties trainen

Hoewel door de Train-bewerking [FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) en [Identification](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) sneller worden uitgevoerd, is de trainingstijd aanzienlijk langer bij een grote schaal.
De geschatte trainingstijd in verschillende schalen wordt in de volgende tabel vermeld:

| Schaal (gezichten of personen) | Geschatte trainingstijd |
|:---:|:---:|
| 1000 | 1-2 s |
| 10.000 | 5-10 s |
| 100.000 | 1 - 2 min |
| 1.000.000 | 10 - 30 min |

Wij bevelen enkele strategieën aan om beter gebruik te kunnen maken van de functie Grootschalig.

## <a name="step-31-customize-time-interval"></a>Stap 3.1: Tijdsinterval aanpassen

Zoals wordt weergegeven in `TrainLargeFaceList()`, is er `timeIntervalInMilliseconds` om het oneindige proces voor controle van de trainingsstatus uit te stellen.
Als u een langer interval gebruikt voor LargeFaceList met meer gezichten, vermindert u het aantal oproepen en de kosten.
Het tijdsinterval moet worden aangepast op basis van de verwachte capaciteit van LargeFaceList.

Dezelfde strategie geldt ook voor LargePersonGroup.
Bij het trainen van een LargePersonGroup met 1.000.000 personen, kan `timeIntervalInMilliseconds` bijvoorbeeld 60.000 zijn (d.w.z. een interval van 1 minuut).

## <a name="step-32-small-scale-buffer"></a>Stap 3.2 Kleinschalige buffer

Personen/gezichten in LargePersonGroup/LargeFaceList zijn alleen doorzoekbaar wanneer ze zijn getraind.
In een dynamisch scenario worden voortdurend nieuwe personen/gezichten toegevoegd, die onmiddellijk doorzoekbaar moeten zijn, maar de training kan langer duren dan gewenst.
Als u dit probleem wilt verhelpen, kunt u een extra kleinschalige LargePersonGroup/LargeFaceList voor alleen de toegevoegde vermeldingen gebruiken als buffer.
Deze tijdelijke buffer kan sneller worden getraind vanwege de veel kleinere omvang en is daarom vrijwel meteen doorzoekbaar.
Gebruik deze buffer in combinatie met de training van de master LargePersonGroup/LargeFaceList door de mastertraining met een langere interval uit te voeren, bijvoorbeeld een maal daags om middernacht.

Voorbeeld van een werkstroom:
1. Maak een master LargePersonGroup/LargeFaceList (masterverzameling) en een buffer LargePersonGroup/LargeFaceList (bufferverzameling). De bufferverzameling is alleen voor nieuw toegevoegde personen/gezichten.
1. Voeg nieuwe personen/gezichten toe aan zowel de masterverzameling als de bufferverzameling.
1. Train alleen de bufferverzameling met een kort interval om te waarborgen dat de zojuist toegevoegde vermeldingen bruikbaar zijn.
1. Roep Identification/FindSimilar aan voor zowel de masterverzameling als de bufferverzameling en voeg de resultaten samen.
1. Wanneer de omvang van de bufferverzameling een drempelwaarde bereikt, of op een tijd dat het systeem niet actief is, maakt u een nieuwe bufferverzameling en triggert u de training van de masterverzameling.
1. Verwijder de oude bufferverzameling wanneer de training van de masterverzameling is voltooid.

## <a name="step-33-standalone-training"></a>Stap 3.3 Zelfstandige training

Als een relatief lange latentie aanvaardbaar is, is het niet nodig om de Train-bewerking te triggeren direct na het toevoegen van nieuwe gegevens.
De Train-bewerking kan ook worden gescheiden van de hoofdlogica en op regelmatige tijden worden geactiveerd.
Deze strategie is geschikt voor dynamische scenario's met aanvaardbare latentie en kan worden toegepast op statische scenario's om de frequentie van trainingen nog verder te beperken.

Stel dat er een `TrainLargePersonGroup`-functie is vergelijkbaar met `TrainLargeFaceList`.
Een typische implementatie van de zelfstandige Training voor LargePersonGroup door het aanroepen van de [`Timer`](https://msdn.microsoft.com/library/system.timers.timer(v=vs.110).aspx)-
klasse `System.Timers` is:

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

Voor meer informatie over gegevensbeheer en aan identificatie gerelateerde implementaties raadpleegt u [Gezichten toevoegen](how-to-add-faces.md) en [Gezichten in een afbeeldingen identificeren](HowtoIdentifyFacesinImage.md).

## <a name="summary"></a>Samenvatting

In deze handleiding hebt u geleerd hoe u de bestaande PersonGroup/FaceList-code (geen gegevens) migreert naar LargePersonGroup/LargeFaceList:

- LargePersonGroup en LargeFaceList werken op een manier die vergelijkbaar is met PersonGroup/FaceList, behalve dat de Train-bewerking nodig is voor LargeFaceList.
- Pas een juiste trainingsstrategie toe voor dynamische gegevensupdates van grootschalige gegevenssets.

## <a name="related-topics"></a>Verwante onderwerpen

- [Gezichten in een afbeelding identificeren](HowtoIdentifyFacesinImage.md)
- [Gezichten toevoegen](how-to-add-faces.md)
