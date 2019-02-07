---
title: De face-gegevens migreren tussen abonnementen - Face-API
titleSuffix: Azure Cognitive Services
description: Deze handleiding wordt beschreven hoe u uw gegevens opgeslagen gezichten van één Face-API-abonnement migreren naar een andere.
services: cognitive-services
author: lewlu
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: lewlu
ms.openlocfilehash: 5eb198ecf76556e632c5f42bc22362b2f20f8916
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55771542"
---
# <a name="migrate-your-face-data-to-a-different-face-subscription"></a>Uw gegevens face migreren naar een ander Face-abonnement

Deze handleiding wordt beschreven hoe face gegevens verplaatsen (zoals een opgeslagen **PersonGroup** van gezichten wordt uitgevoerd) naar een ander Face-API-abonnement met behulp van de momentopname-functie. Hiermee kunt u om te voorkomen dat herhaaldelijk maken en trainen een **PersonGroup** of **FaceList** bij het verplaatsen of uitbreiden van uw activiteiten. Bijvoorbeeld, u hebt gemaakt een **PersonGroup** met behulp van een gratis proefabonnement en nu wilt migreren naar uw betaalde abonnement, of moet u de face-gegevens synchroniseren tussen regio's voor een grote onderneming-bewerking.

Deze dezelfde migratiestrategie geldt ook voor **LargePersonGroup** en **LargeFaceList** objecten. Als u niet bekend met de concepten in deze handleiding bent, raadpleegt u de definities in de [verklarende woordenlijst](../Glossary.md). Deze handleiding gebruikt de Face-API .NET-clientbibliotheek met C#.

## <a name="prerequisites"></a>Vereisten

- Twee Face-API abonnementssleutels (met de bestaande gegevens en één voor het migreren naar). Volg de instructies in [een Cognitive Services-account maken](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) abonneren op de Face-API-service en haal uw sleutel.
- De Face-API-abonnement-id die overeenkomt met het doelabonnement (gevonden in de **overzicht** blade in de Azure portal). 
- Een versie van [Visual Studio 2015 of 2017](https://www.visualstudio.com/downloads/).


## <a name="create-the-visual-studio-project"></a>Het Visual Studio-project maken

Deze handleiding wordt een eenvoudige consoletoepassing voor het uitvoeren van de gegevensmigratie face gebruiken. Zie voor een volledige implementatie, de [Face-API-momentopname voorbeeld](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample) op GitHub.

1. In Visual Studio, maak een nieuwe **Console-app (.NET Framework)** project en geef deze de naam **FaceApiSnapshotSample**. 
1. Download de vereiste NuGet-pakketten. Klik met de rechtermuisknop op het project in Solution Explorer en selecteer **NuGet-pakketten beheren**. Klik op het tabblad **Bladeren** en selecteer **voorlopige release opnemen**. Zoek en installeer vervolgens het volgende pakket:
    - [Microsoft.Azure.CognitiveServices.Vision.Face 2.3.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview)


## <a name="create-face-clients"></a>Face-clients maken

In de **Main** methode in *Program.cs*, twee **[FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet)** exemplaren voor uw bron- en -abonnementen. In dit voorbeeld gebruiken we een abonnement gezicht in de regio Oost-Azië als de bron en een VS-West-abonnement als het doel. Dit wordt gedemonstreerd hoe u gegevens migreren van een Azure-regio naar een andere. Als uw abonnementen in verschillende regio's, moet u wijzigt de `Endpoint` tekenreeksen.

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

U moet de abonnement-sleutelwaarden en een eindpunt-URL's voor uw bron- en -abonnementen in te vullen.


## <a name="prepare-a-persongroup-for-migration"></a>Een PersonGroup voorbereiden voor migratie

Hebt u de ID van de **PersonGroup** in uw bronabonnement om te migreren naar het doelabonnement. Gebruik de **[PersonGroupOperationsExtensions.ListAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongroupoperationsextensions.listasync?view=azure-dotnet)** methode voor het ophalen van een lijst met uw **PersonGroup** objecten, waarna de **[ PersonGroup.PersonGroupId](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.persongroup.persongroupid?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Vision_Face_Models_PersonGroup_PersonGroupId)** eigenschap. Dit proces verloopt ongeveer verschillend, afhankelijk van wat **PersonGroup** objecten hebt. In deze handleiding, de bron **PersonGroup** ID wordt opgeslagen in `personGroupId`.

> [!NOTE]
> De [voorbeeldcode](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample) maakt en een nieuwe traint **PersonGroup** om te migreren, maar in de meeste gevallen u hebt al een **PersonGroup** te gebruiken.

## <a name="take-snapshot-of-persongroup"></a>Momentopname van PersonGroup

Een momentopname is een tijdelijke, externe opslag voor bepaalde gegevenstypen Face. Het fungeert als een soort Klembord te kopiëren van gegevens van één abonnement naar een andere. Eerst de gebruiker "" een momentopname van de gegevens in het bronabonnement en vervolgens deze 'toepassen' op een nieuw gegevensobject in het doelabonnement.

Gebruik van het bronabonnement **FaceClient** exemplaar om een momentopname van de **PersonGroup**, met **[TakeAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.snapshotoperationsextensions.takeasync?view=azure-dotnet)** met de  **PersonGroup** -ID en de doel-abonnements-ID. Als u meerdere target-abonnementen hebt, kunt u hen toevoegen als matrix vermeldingen in de derde parameter.

```csharp
var takeSnapshotResult = await FaceClientEastAsia.Snapshot.TakeAsync(
    SnapshotObjectType.PersonGroup,
    personGroupId,
    new[] { "<Azure West US Subscription ID>" /* Put other IDs here, if multiple target subscriptions wanted */ });
```

> [!NOTE]
> Het proces van het maken en toepassen van momentopnamen worden niet verstoort de regelmatige aanroepen voor de bron- of doelentiteit **PersonGroup**s (of **FaceList**s). Echter niet aangeraden gelijktijdige aanroepen die het bronobject wijzigen ([Face lijst beheeraanroepen](https://docs.microsoft.com/rest/api/cognitiveservices/face/facelist) of de [Persoonsgroep - Train](https://docs.microsoft.com/rest/api/cognitiveservices/face/persongroup/train) aanroepen, bijvoorbeeld), omdat de momentopnamebewerking mogelijk vóór of na deze bewerkingen uit te voeren of kunnen fouten optreden. 

## <a name="retrieve-the-snapshot-id"></a>De momentopname-ID ophalen

De methode waarbij momentopname is asynchroon, dus u moet wachten tot de voltooiing (momentopname bewerkingen kunnen niet worden geannuleerd). In deze code wordt de `WaitForOperation` methode bewaakt de asynchrone aanroep, controleren van de status van elke 100 MS. Wanneer de bewerking is voltooid, kunt u zich om op te halen van een bewerking-ID. U ontvangt deze tijdens het parseren van de `OperationLocation` veld. 

```csharp
var takeOperationId = Guid.Parse(takeSnapshotResult.OperationLocation.Split('/')[2]);
var operationStatus = await WaitForOperation(FaceClientEastAsia, takeOperationId);
```

Een typische `OperationLocation` waarde er als volgt:

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

Wanneer de status van de bewerking is gemarkeerd als `Succeeded`, u kunt vervolgens de momentopname-ID ophalen tijdens het parseren van de `ResourceLocation` veld van de geretourneerde **OperationStatus** exemplaar.

```csharp
var snapshotId = Guid.Parse(operationStatus.ResourceLocation.Split('/')[2]);
```

Een typische `resourceLocation` waarde er als volgt:

```csharp
"/snapshots/e58b3f08-1e8b-4165-81df-aa9858f233dc"
```

## <a name="apply-snapshot-to-target-subscription"></a>Momentopname naar het doelabonnement toepassen

Maak vervolgens de nieuwe **PersonGroup** in het doelabonnement met behulp van een willekeurig gegenereerde ID. Gebruik vervolgens het doelabonnement **FaceClient** exemplaar naar de momentopname wordt toegepast op deze PersonGroup, waarbij in de momentopname-ID en nieuwe **PersonGroup** -id. 

```csharp
var newPersonGroupId = Guid.NewGuid().ToString();
var applySnapshotResult = await FaceClientWestUS.Snapshot.ApplyAsync(snapshotId, newPersonGroupId);
```


> [!NOTE]
> Een Snapshot-object is alleen geldig voor 48 uur. U moet alleen een momentopname van een uitvoeren als u van plan bent te gebruiken voor gegevensmigratie snel na.

Een aanvraag voor het toepassen van momentopname resulteert in een andere bewerking-ID. U kunt deze ID ophalen tijdens het parseren van de `OperationLocation` veld van de geretourneerde **applySnapshotResult** exemplaar. 

```csharp
var applyOperationId = Guid.Parse(applySnapshotResult.OperationLocation.Split('/')[2]);
```

Het proces van toepassing is ook asynchrone, gebruikt u dus opnieuw `WaitForOperation` na afloop van deze is voltooid.

```csharp
operationStatus = await WaitForOperation(FaceClientWestUS, applyOperationId);
```

## <a name="test-the-data-migration"></a>Testen van de gegevensmigratie

Nadat u hebt toegepast en de momentopname, de nieuwe **PersonGroup** in de doel-abonnement moet worden ingevuld met de oorspronkelijke face-gegevens. Training resultaten worden standaard ook gekopieerd, zodat de nieuwe **PersonGroup** is gereed voor face id aanroepen zonder opnieuw trainen.

Als u wilt testen van de migratie van gegevens, kunt u de volgende bewerkingen uitvoeren en vergelijk de resultaten die ze naar de console afgedrukt.

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

Nu kunt u beginnen met de nieuwe **PersonGroup** in het doelabonnement. 

Als u wilt bijwerken van het doel **PersonGroup** opnieuw in de toekomst, moet u een nieuw **PersonGroup** (volgens de stappen van deze handleiding) voor het ontvangen van de momentopname. Een enkel **PersonGroup** object kan alleen een momentopname die is toegepast op het één keer hebben.

## <a name="clean-up-resources"></a>Resources opschonen

Zodra u klaar bent met face gegevens migreert, wordt u aangeraden dat u het snapshot-object voor het handmatig verwijderen.

```csharp
await FaceClientEastAsia.Snapshot.DeleteAsync(snapshotId);
```

## <a name="related-topics"></a>Verwante onderwerpen

- [Referentiedocumentatie voor momentopname (.NET SDK)](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.snapshotoperations?view=azure-dotnet)
- [Face-API-momentopname voorbeeld](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample)
- [Het toevoegen van gezichten](how-to-add-faces.md)
- [Gezichten in een afbeelding detecteren](HowtoDetectFacesinImage.md)
- [Het identificeren van gezichten in afbeelding](HowtoIdentifyFacesinImage.md)
