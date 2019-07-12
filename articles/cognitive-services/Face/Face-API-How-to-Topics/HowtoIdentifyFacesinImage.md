---
title: 'Voorbeeld: Gezichten in afbeeldingen identificeren - Face-API'
titleSuffix: Azure Cognitive Services
description: De Face-API gebruiken voor het identificeren van gezichten in afbeeldingen.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 04/10/2019
ms.author: sbowles
ms.openlocfilehash: 5806c17b0532f4d18b7ac57fbf70c92ed9d47daa
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827503"
---
# <a name="example-identify-faces-in-images"></a>Voorbeeld: Gezichten in foto's identificeren

Deze handleiding laat zien hoe u onbekende gezichten identificeren met behulp van PersonGroup-objecten die zijn gemaakt op basis van bekende personen vooraf. De voorbeelden zijn geschreven in C# met behulp van de clientbibliotheek van Azure Cognitive Services Face-API.

## <a name="preparation"></a>Voorbereiding

Dit voorbeeld laat zien:

- Het maken van een PersonGroup. Deze PersonGroup bevat een lijst met bekende personen.
- Klik hier voor meer informatie over het gezichten toewijzen aan elke persoon. Deze gezichten worden gebruikt als een basislijn voor identificatie van personen. U wordt aangeraden dat u duidelijk voorzijde weergaven van gezichten wordt uitgevoerd. Een voorbeeld is een foto-ID. Een goede set van foto's bevat gezichten van dezelfde persoon in verschillende poses, kleding kleuren of kapsels uit.

Als u wilt de demonstratie van dit voorbeeld is uitgevoerd, voorbereiden:

- Enkele foto's van het gezicht van de persoon. [Voorbeeld van foto's downloaden](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data) voor Anna, factureren en Clare.
- Een reeks test foto's. De foto's kunnen of kunnen niet de gezichten van Anna, factuur of Clare bevatten. Deze worden gebruikt voor test-ID. Aantal voorbeeldafbeeldingen ook selecteren in de vorige koppeling.

## <a name="step-1-authorize-the-api-call"></a>Stap 1: De API-aanroep autoriseren

Voor elke aanroep naar de Face-API is een abonnementssleutel vereist. Deze sleutel kan worden doorgegeven via een tekenreeksparameter of zijn opgegeven in de aanvraagheader. Als u wilt de abonnementssleutel via een queryreeks kunt toevoegen, Zie de aanvraag-URL voor de [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) als een voorbeeld:
```
https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes]
&subscription-key=<Subscription key>
```

Als alternatief kunt u de abonnementssleutel opgeven in de HTTP-aanvraagheader **ocp-apim-subscription-key: &lt;Abonnementssleutel&gt;** .
Wanneer u een clientbibliotheek gebruikt, wordt de abonnementssleutel doorgegeven de constructor van de klasse FaceClient. Bijvoorbeeld:
 
```csharp 
private readonly IFaceClient faceClient = new FaceClient(
            new ApiKeyServiceClientCredentials("<subscription key>"),
            new System.Net.Http.DelegatingHandler[] { });
```
 
Als u de abonnementssleutel, gaat u naar de Azure Marketplace via Azure portal. Zie voor meer informatie, [abonnementen](https://azure.microsoft.com/try/cognitive-services/).

## <a name="step-2-create-the-persongroup"></a>Stap 2: De PersonGroup maken

In deze stap bevat een PersonGroup met de naam 'MyFriends' Anna factuur en Clare. Voor elke persoon zijn verschillende gezichten geregistreerd. De gezichten moeten worden gedetecteerd via de installatiekopieën. Nadat al deze stappen zijn uitgevoerd, hebt u een PersonGroup die eruitziet als deze afbeelding:

![MyFriends](../Images/group.image.1.jpg)

### <a name="step-21-define-people-for-the-persongroup"></a>Stap 2.1: Personen voor de PersonGroup definiëren
Een persoon is een basisidentificatie-eenheid. Een gebruiker kan een of meer bekende gezichten geregistreerd hebben. Een PersonGroup is een verzameling van mensen. Elke persoon die is gedefinieerd binnen een bepaalde PersonGroup. Identificatie wordt uitgevoerd op basis van een PersonGroup. De taak is het maken van een PersonGroup en maak vervolgens de mensen in, zoals Anna, factureren en Clare.

Maak eerst een nieuwe PersonGroup met behulp van de [PersonGroup - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API. De bijbehorende-clientbibliotheek API is de methode CreatePersonGroupAsync voor de klasse FaceClient. De groeps-ID die opgegeven voor het maken van de groep is uniek voor elk abonnement. U kunt ook ophalen, bijwerken of verwijderen van PersonGroups met behulp van andere APIs PersonGroup. 

Nadat een groep wordt gedefinieerd, kunt u personen binnen deze definiëren met behulp van de [PersonGroup persoon - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API. De clientbibliotheekmethode is CreatePersonAsync. U kunt een gezicht toevoegen aan elke persoon nadat ze zijn gemaakt.

```csharp 
// Create an empty PersonGroup
string personGroupId = "myfriends";
await faceClient.PersonGroup.CreateAsync(personGroupId, "My Friends");
 
// Define Anna
CreatePersonResult friend1 = await faceClient.PersonGroupPerson.CreateAsync(
    // Id of the PersonGroup that the person belonged to
    personGroupId,    
    // Name of the person
    "Anna"            
);
 
// Define Bill and Clare in the same way
```
### <a name="step2-2"></a> Stap 2.2: Detecteer gezichten en ze kan registreren bij de juiste persoon
De detectie wordt uitgevoerd door een POST-webaanvraag naar de API [Face - Detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) te verzenden met het afbeeldingsbestand in de HTTP-aanvraagbody. Wanneer u de clientbibliotheek, wordt met de methode DetectAsync voor de klasse FaceClient gezichtsdetectie gedaan.

Aanroepen voor elk gezicht die wordt gedetecteerd, [PersonGroup persoon – Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) toe te voegen aan de juiste persoon.

In de volgende code ziet u het proces voor het detecteren van een gezicht in een afbeelding en het toevoegen van het gezicht aan een persoon:

```csharp 
// Directory contains image files of Anna
const string friend1ImageDir = @"D:\Pictures\MyFriends\Anna\";
 
foreach (string imagePath in Directory.GetFiles(friend1ImageDir, "*.jpg"))
{
    using (Stream s = File.OpenRead(imagePath))
    {
        // Detect faces in the image and add to Anna
        await faceClient.PersonGroupPerson.AddFaceFromStreamAsync(
            personGroupId, friend1.PersonId, s);
    }
}
// Do the same for Bill and Clare
``` 
Als de afbeelding meer dan één face bevat, wordt alleen het grootste gezicht toegevoegd. U kunt andere gezichten toevoegen aan de persoon. Een tekenreeks worden doorgegeven in de indeling van ' targetFace links, boven, width en height = "aan [PersonGroup persoon - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API's targetFace queryparameter. U kunt ook de targetFace optionele parameter voor de methode AddPersonFaceAsync gebruiken om toe te voegen andere gezichten. Elk gezicht toegevoegd aan de persoon krijgt een unieke persistente face ID. U kunt deze ID in [PersonGroup persoon – gezichten verwijderen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523e) en [geconfronteerd – identificeren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

## <a name="step-3-train-the-persongroup"></a>Stap 3: De PersonGroup trainen

De PersonGroup moet worden getraind voordat een id kan worden uitgevoerd door het te gebruiken. De PersonGroup moet opnieuw worden getraind na het toevoegen of verwijderen van een persoon of als u van een persoon geregistreerde face bewerken. De training wordt uitgevoerd met behulp van de API [PersonGroup – Trainen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249). Wanneer u de clientbibliotheek gebruikt, is een aanroep van de methode TrainPersonGroupAsync:
 
```csharp 
await faceClient.PersonGroup.TrainAsync(personGroupId);
```
 
Training is een asynchroon proces. Deze kan niet worden voltooid, zelfs nadat de TrainPersonGroupAsync-methode retourneert. U moet mogelijk de trainingsstatus opvragen. Gebruik de [PersonGroup - Training-Status ophalen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395247) API of GetPersonGroupTrainingStatusAsync methode van de clientbibliotheek. De volgende code toont een eenvoudige logische wachten PersonGroup training om te voltooien:
 
```csharp 
TrainingStatus trainingStatus = null;
while(true)
{
    trainingStatus = await faceClient.PersonGroup.GetTrainingStatusAsync(personGroupId);
 
    if (trainingStatus.Status != TrainingStatusType.Running)
    {
        break;
    }
 
    await Task.Delay(1000);
} 
``` 

## <a name="step-4-identify-a-face-against-a-defined-persongroup"></a>Stap 4: Een gezicht identificeren op basis van een gedefinieerde PersonGroup

Wanneer de Face-API id's uitvoert, berekent de gelijkenis van een gezicht test tussen de gezichten in een groep. Retourneert de meest vergelijkbare personen voor het testen gezicht. Dit proces vindt plaats via de [geconfronteerd - identificeren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) API of de IdentifyAsync-methode van de clientbibliotheek.

Het testen gezicht moet worden gedetecteerd met behulp van de vorige stappen. De face ID wordt vervolgens doorgegeven aan de API-id als een tweede argument. Meerdere face id's kan in één keer worden geïdentificeerd. Het resultaat bevat de geïdentificeerde resultaten. Standaard retourneert de identificatieproces slechts één persoon die het beste komt overeen met het gezicht test. Als u liever, geeft u de optionele parameter-maxNumOfCandidatesReturned zodat het id-proces meer kandidaten retourneren.

De volgende code ziet u de id-proces:

```csharp 
string testImageFile = @"D:\Pictures\test_img1.jpg";

using (Stream s = File.OpenRead(testImageFile))
{
    var faces = await faceClient.Face.DetectAsync(s);
    var faceIds = faces.Select(face => face.FaceId).ToArray();
 
    var results = await faceClient.Face.IdentifyAsync(faceIds, personGroupId);
    foreach (var identifyResult in results)
    {
        Console.WriteLine("Result of face: {0}", identifyResult.FaceId);
        if (identifyResult.Candidates.Length == 0)
        {
            Console.WriteLine("No one identified");
        }
        else
        {
            // Get top 1 among all candidates returned
            var candidateId = identifyResult.Candidates[0].PersonId;
            var person = await faceClient.PersonGroupPerson.GetAsync(personGroupId, candidateId);
            Console.WriteLine("Identified as {0}", person.Name);
        }
    }
}
``` 

Nadat u de stappen hebt voltooid, probeert u te identificeren van verschillende vlakken. Zie als de gezichten van Anna, factuur of Clare correct kunnen worden geïdentificeerd op basis van de installatiekopieën geüpload voor gezichtsdetectie. Zie de volgende voorbeelden:

![Identificeren van verschillende vlakken](../Images/identificationResult.1.jpg )

## <a name="step-5-request-for-large-scale"></a>Stap 5: Aanvraag voor grote schaal

Een PersonGroup kan maximaal 10.000 personen op basis van de vorige ontwerp-beperking bevatten.
Zie voor meer informatie over scenario's met grootschalige capaciteit voor maximaal een miljoen personen [De functie Grootschalig gebruiken](how-to-use-large-scale.md).

## <a name="summary"></a>Samenvatting

In deze handleiding, hebt u geleerd het proces voor het maken van een PersonGroup en het identificeren van een persoon. De volgende onderdelen zijn uitgelegd en gedemonstreerd:

- Gezichten detecteren met behulp van de [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d) API.
- PersonGroups maken met behulp van de [PersonGroup - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API.
- Personen maken met behulp van de [PersonGroup persoon - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API.
- Een PersonGroup met behulp van de trein de [PersonGroup – Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) API.
- Onbekende gezichten op basis van de PersonGroup identificeren met behulp van de [geconfronteerd - identificeren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) API.

## <a name="related-topics"></a>Verwante onderwerpen

- [Face-opname-concepten](../concepts/face-recognition.md)
- [Detecteer gezichten in een afbeelding](HowtoDetectFacesinImage.md)
- [Gezichten toevoegen](how-to-add-faces.md)
- [Gebruik de functie voor grootschalige](how-to-use-large-scale.md)
