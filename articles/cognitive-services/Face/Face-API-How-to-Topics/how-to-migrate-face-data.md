---
title: De face-gegevens migreren tussen abonnementen - Face-API
titleSuffix: Azure Cognitive Services
description: Deze handleiding wordt beschreven hoe u uw gegevens opgeslagen gezichten van één Face-API-abonnement migreren naar een andere.
services: cognitive-services
author: lewlu
manager: cgronlun
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: lewlu
ms.openlocfilehash: 702aed12860c090e83b997e6b56d56e06b416568
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65913802"
---
# <a name="migrate-your-face-data-to-a-different-face-subscription"></a>Uw gegevens face migreren naar een ander Face-abonnement

Deze handleiding laat zien hoe u gegevens van de face-, zoals een opgeslagen PersonGroup-object met gezichten, verplaatsen naar een ander abonnement voor Azure Cognitive Services Face-API. Als u wilt de gegevens verplaatst, moet u de momentopname-functie gebruiken. Op deze manier hoeft u niet herhaaldelijk maken en trainen van een object PersonGroup of FaceList wanneer u verplaatst of uw activiteiten uitbreiden. Bijvoorbeeld, wellicht u een PersonGroup-object gemaakt met behulp van een gratis proefabonnement en wilt migreren naar uw betaalde abonnement. Of u moet mogelijk face gegevens synchroniseren tussen regio's voor een grote onderneming-bewerking.

Deze dezelfde migratiestrategie geldt ook voor LargePersonGroup en LargeFaceList-objecten. Als u niet bekend bent met de concepten in deze handleiding, raadpleegt u de definities in de [geconfronteerd erkenning concepten](../concepts/face-recognition.md) handleiding. Deze handleiding gebruikt de Face-API .NET-clientbibliotheek met C#.

## <a name="prerequisites"></a>Vereisten

U hebt de volgende items nodig:

- Twee Face-API abonnementssleutels, één met de bestaande gegevens en één voor het migreren naar. Volg de instructies in om u te abonneren op de Face-API-service en haal uw sleutel, [een Cognitive Services-account maken](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).
- De Face-API-abonnement-id die overeenkomt met het doelabonnement. Selecteer om te zoeken, **overzicht** in Azure portal. 
- Een versie van [Visual Studio 2015 of 2017](https://www.visualstudio.com/downloads/).

## <a name="create-the-visual-studio-project"></a>Het Visual Studio-project maken

Deze handleiding gebruikt een eenvoudige consoletoepassing voor het uitvoeren van de face-gegevensmigratie. Zie voor een volledige implementatie, de [Face-API momentopname voorbeeld](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample) op GitHub.

1. Maak een nieuw project voor het .NET Framework Console-app in Visual Studio. Geef het de naam **FaceApiSnapshotSample**.
1. Download de vereiste NuGet-pakketten. Met de rechtermuisknop op uw project in Solution Explorer en selecteer **NuGet-pakketten beheren**. Selecteer de **Bladeren** tabblad, en selecteer **Include prerelease**. Zoeken en installeren van de volgende pakketten:
    - [Microsoft.Azure.CognitiveServices.Vision.Face 2.3.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview)

## <a name="create-face-clients"></a>Face-clients maken

In de **Main** methode in *Program.cs*, twee [FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet) exemplaren voor uw bron- en -abonnementen. Dit voorbeeld wordt een abonnement gezicht in de regio Oost-Azië als de bron- en een VS-West-abonnement als het doel. In dit voorbeeld ziet u hoe u gegevens migreren van een Azure-regio naar een andere. Als uw abonnementen in verschillende regio's zijn, wijzigt u de `Endpoint` tekenreeksen.

```csharp
var FaceClientEastAsia = new FaceClient(new ApiKeyServiceClientCredentials("<East Asia Subscription Key>"))
    {
        Endpoint = "https://southeastasia.api.cognitive.microsoft.com/>"
    };

var FaceClientWestUS = new FaceClient(new ApiKeyServiceClientCredentials("<West US Subscription Key>"))
    {
        Endpoint = "https://westus.api.cognitive.microsoft.com/"
    };
```

Vul de abonnement-sleutelwaarden en een eindpunt-URL's voor uw bron- en -abonnementen.


## <a name="prepare-a-persongroup-for-migration"></a>Een PersonGroup voorbereiden voor migratie

U moet de ID van de PersonGroup in uw bronabonnement om te migreren naar het doelabonnement. Gebruik de [PersonGroupOperationsExtensions.ListAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongroupoperationsextensions.listasync?view=azure-dotnet) methode voor het ophalen van een lijst met uw PersonGroup-objecten. Haal vervolgens de [PersonGroup.PersonGroupId](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.persongroup.persongroupid?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Vision_Face_Models_PersonGroup_PersonGroupId) eigenschap. Dit proces ziet er anders uit op basis van welke PersonGroup objecten hebt. In deze handleiding worden de bron-ID PersonGroup wordt opgeslagen in `personGroupId`.

> [!NOTE]
> De [voorbeeldcode](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample) maakt en traint u een nieuwe PersonGroup om te migreren. In de meeste gevallen moet u een PersonGroup gebruiken al hebt.

## <a name="take-a-snapshot-of-a-persongroup"></a>Een momentopname van een PersonGroup

Een momentopname is tijdelijke externe opslag voor bepaalde gegevenstypen Face. Het fungeert als een soort Klembord te kopiëren van gegevens van één abonnement naar een andere. Eerst, u een momentopname van de gegevens in de bronabonnement. Vervolgens toepassen u op een nieuw gegevensobject in het doelabonnement.

Het bronabonnement FaceClient exemplaar gebruiken om een momentopname van de PersonGroup. Gebruik [TakeAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.snapshotoperationsextensions.takeasync?view=azure-dotnet) met de PersonGroup-ID en de doel-abonnements-ID. Als u meerdere target-abonnementen hebt, kunt u deze als matrix vermeldingen in de derde parameter toevoegen.

```csharp
var takeSnapshotResult = await FaceClientEastAsia.Snapshot.TakeAsync(
    SnapshotObjectType.PersonGroup,
    personGroupId,
    new[] { "<Azure West US Subscription ID>" /* Put other IDs here, if multiple target subscriptions wanted */ });
```

> [!NOTE]
> Het proces van het maken en toepassen van momentopnamen niet de regelmatige aanroepen voor de bron worden onderbroken of richt u op PersonGroups of FaceLists. Maak niet gelijktijdige aanroepen die de bronobject, zoals wijzigen [FaceList beheeraanroepen](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.facelistoperations?view=azure-dotnet) of de [PersonGroup Train](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongroupoperations?view=azure-dotnet) aanroepen, zo. De momentopnamebewerking kan worden uitgevoerd vóór of na deze bewerkingen of kan er fouten optreden.

## <a name="retrieve-the-snapshot-id"></a>De momentopname-ID ophalen

De methode die wordt gebruikt om momentopnamen te is asynchroon, dus u tot de voltooiing wachten moet. Momentopnamebewerkingen kunnen niet worden geannuleerd. In deze code wordt de `WaitForOperation` methode bewaakt de asynchrone aanroep. Er wordt gecontroleerd met de status van elke 100 ms. Nadat de bewerking is voltooid, een bewerkings-ID ophalen tijdens het parseren van de `OperationLocation` veld. 

```csharp
var takeOperationId = Guid.Parse(takeSnapshotResult.OperationLocation.Split('/')[2]);
var operationStatus = await WaitForOperation(FaceClientEastAsia, takeOperationId);
```

Een typische `OperationLocation` waarde er als volgt uitzien:

```csharp
"/operations/a63a3bdd-a1db-4d05-87b8-dbad6850062a"
```

De `WaitForOperation` Help-methode is hier:

```csharp
/// <summary>
/// Waits for the take/apply operation to complete and returns the final operation status.
/// </summary>
/// <returns>The final operation status.</returns>
private static async Task<OperationStatus> WaitForOperation(IFaceClient client, Guid operationId)
{
    OperationStatus operationStatus = null;
    do
    {
        if (operationStatus != null)
        {
            Thread.Sleep(TimeSpan.FromMilliseconds(100));
        }

        // Get the status of the operation.
        operationStatus = await client.Snapshot.GetOperationStatusAsync(operationId);

        Console.WriteLine($"Operation Status: {operationStatus.Status}");
    }
    while (operationStatus.Status != OperationStatusType.Succeeded
            && operationStatus.Status != OperationStatusType.Failed);

    return operationStatus;
}
```

Nadat de status van de bewerking bevat `Succeeded`, de momentopname-ID ophalen tijdens het parseren van de `ResourceLocation` veld van het geretourneerde OperationStatus-exemplaar.

```csharp
var snapshotId = Guid.Parse(operationStatus.ResourceLocation.Split('/')[2]);
```

Een typische `resourceLocation` waarde er als volgt uitzien:

```csharp
"/snapshots/e58b3f08-1e8b-4165-81df-aa9858f233dc"
```

## <a name="apply-a-snapshot-to-a-target-subscription"></a>Een momentopname van een toepassing naar een doelabonnement

Maak vervolgens de nieuwe PersonGroup in het doelabonnement met behulp van een willekeurig gegenereerde ID. Gebruik vervolgens het doelabonnement FaceClient exemplaar toe te passen van de momentopname op deze PersonGroup. Doorgeven in de momentopname-ID en de nieuwe PersonGroup-ID.

```csharp
var newPersonGroupId = Guid.NewGuid().ToString();
var applySnapshotResult = await FaceClientWestUS.Snapshot.ApplyAsync(snapshotId, newPersonGroupId);
```


> [!NOTE]
> Een Snapshot-object is alleen 48 uur geldig. Alleen een momentopname van een uitvoeren als u van plan bent te gebruiken voor gegevensmigratie snel na.

Een aanvraag voor het toepassen van momentopname retourneert een andere bewerking-ID. Als u deze ID, parseren de `OperationLocation` veld van het geretourneerde applySnapshotResult-exemplaar. 

```csharp
var applyOperationId = Guid.Parse(applySnapshotResult.OperationLocation.Split('/')[2]);
```

Het proces van toepassing is ook asynchrone, gebruikt u dus opnieuw `WaitForOperation` te wachten tot deze is voltooid.

```csharp
operationStatus = await WaitForOperation(FaceClientWestUS, applyOperationId);
```

## <a name="test-the-data-migration"></a>Testen van de gegevensmigratie

Na het toepassen van de momentopname, wordt de nieuwe PersonGroup in het doelabonnement met de oorspronkelijke face-gegevens. Training resultaten worden standaard ook gekopieerd. De nieuwe PersonGroup is gereed voor face id aanroepen zonder opnieuw trainen.

Als u wilt testen van de migratie van gegevens, uitvoeren van de volgende bewerkingen en vergelijk de resultaten die ze naar de console afdrukken:

```csharp
await DisplayPersonGroup(FaceClientEastAsia, personGroupId);
await IdentifyInPersonGroup(FaceClientEastAsia, personGroupId);

await DisplayPersonGroup(FaceClientWestUS, newPersonGroupId);
// No need to retrain the person group before identification,
// training results are copied by snapshot as well.
await IdentifyInPersonGroup(FaceClientWestUS, newPersonGroupId);
```

Gebruik de volgende methoden:

```csharp
private static async Task DisplayPersonGroup(IFaceClient client, string personGroupId)
{
    var personGroup = await client.PersonGroup.GetAsync(personGroupId);
    Console.WriteLine("Person Group:");
    Console.WriteLine(JsonConvert.SerializeObject(personGroup));

    // List persons.
    var persons = await client.PersonGroupPerson.ListAsync(personGroupId);

    foreach (var person in persons)
    {
        Console.WriteLine(JsonConvert.SerializeObject(person));
    }

    Console.WriteLine();
}
```

```csharp
private static async Task IdentifyInPersonGroup(IFaceClient client, string personGroupId)
{
    using (var fileStream = new FileStream("data\\PersonGroup\\Daughter\\Daughter1.jpg", FileMode.Open, FileAccess.Read))
    {
        var detectedFaces = await client.Face.DetectWithStreamAsync(fileStream);

        var result = await client.Face.IdentifyAsync(detectedFaces.Select(face => face.FaceId.Value).ToList(), personGroupId);
        Console.WriteLine("Test identify against PersonGroup");
        Console.WriteLine(JsonConvert.SerializeObject(result));
        Console.WriteLine();
    }
}
```

U kunt nu de nieuwe PersonGroup gebruiken in het doelabonnement. 

Om bij te werken in de toekomst opnieuw het doel PersonGroup, maakt u een nieuwe PersonGroup voor het ontvangen van de momentopname. U doet dit door de stappen in deze handleiding uit te voeren. Een enkel PersonGroup-object kan een momentopname die is toegepast op deze slechts één keer hebben.

## <a name="clean-up-resources"></a>Resources opschonen

Nadat u klaar bent met het migreren van gegevens te maken krijgt, verwijdert u handmatig het snapshot-object.

```csharp
await FaceClientEastAsia.Snapshot.DeleteAsync(snapshotId);
```

## <a name="next-steps"></a>Volgende stappen

Vervolgens ziet u de relevante API-referentiedocumentatie, bekijk een voorbeeld-app die gebruikmaakt van de momentopname-functie of een handleiding aan de slag met de andere API-bewerkingen die worden vermeld hier volgen:

- [Referentiedocumentatie voor momentopname (.NET SDK)](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.snapshotoperations?view=azure-dotnet)
- [Voorbeeld van de momentopname Face-API](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample)
- [Gezichten toevoegen](how-to-add-faces.md)
- [Detecteer gezichten in een afbeelding](HowtoDetectFacesinImage.md)
- [Identificeer gezichten in een afbeelding](HowtoIdentifyFacesinImage.md)
