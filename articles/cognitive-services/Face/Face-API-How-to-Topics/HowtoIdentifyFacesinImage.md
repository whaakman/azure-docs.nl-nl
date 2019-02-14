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
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 013467cb64220b525d429c901c48028bf65b3852
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55864924"
---
# <a name="example-how-to-identify-faces-in-images"></a>Voorbeeld: Gezichten in een afbeelding identificeren

Deze handleiding laat zien hoe u onbekende gezichten identificeert met PersonGroups (persoonsgroepen), die vooraf worden gemaakt op basis van bekende personen. De voorbeelden zijn geschreven in C# met de Face-API-clientbibliotheek.

## <a name="concepts"></a>Concepten

Als u niet bekend bent met de volgende concepten in deze handleiding, kunt u de definities op elk gewenst moment opzoeken in de [woordenlijst](../Glossary.md):

- Face - Detecteren
- Face - Identificeren
- PersonGroup

## <a name="preparation"></a>Voorbereiding

In dit voorbeeld wordt het volgende toegelicht:

- Het maken van een PersonGroup - deze PersonGroup bevat een lijst met bekende personen.
- Het toewijzen van gezichten aan elke persoon - deze gezichten worden gebruikt als basislijn voor het identificeren van personen. Het wordt aanbevolen om van voren genomen foto's van gezichten te gebruiken, net zoals een pasfoto. Een goede verzameling foto's bevat gezichten van dezelfde persoon in verschillende poses, in verschillende kleuren kleding of met verschillende haarstijlen.

Om deze demonstratie uit te voeren, moet u een aantal afbeeldingen voorbereiden:

- Enkele foto's van het gezicht van de persoon. [Klik hier om voorbeeldfoto's te downloaden](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data) voor Anna, Bill en Clare.
- De identificatiefunctie wordt getest met een reeks foto's waarop de gezichten van Anna, Bill of Clare al dan niet te zien zijn. U kunt ook enkele voorbeeldafbeeldingen selecteren via de vorige koppeling.

## <a name="step-1-authorize-the-api-call"></a>Stap 1: De API-aanroep autoriseren

Voor elke aanroep naar de Face-API is een abonnementssleutel vereist. Deze sleutel kan worden doorgegeven via een tekenreeksparameter of worden opgegeven in de aanvraagheader. Als u de abonnementssleutel via query-tekenreeks wilt doorgeven, raadpleegt u de aanvraag-URL voor de [Face - Detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) als een voorbeeld:
```
https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes]
&subscription-key=<Subscription key>
```

Als alternatief kan de abonnementssleutel ook worden opgegeven in de HTTP-aanvraagheader: **ocp-apim-subscription-key: &lt;Abonnementssleutel&gt;** Als u gebruikmaakt van een clientbibliotheek wordt de abonnementssleutel doorgegeven via de constructor van de klasse FaceServiceClient. Bijvoorbeeld:
 
```CSharp 
faceServiceClient = new FaceServiceClient("<Subscription Key>");
```
 
De abonnementssleutel kan worden verkregen vanuit de Marketplace-pagina van uw Azure Portal. Zie [Abonnementen](https://azure.microsoft.com/try/cognitive-services/).

## <a name="step-2-create-the-persongroup"></a>Stap 2: De PersonGroup maken

In deze stap hebben we de PersonGroup met de naam 'MyFriends' gemaakt. Deze bevat drie personen: Anna, Bill en Clare. Voor elke persoon zijn verschillende gezichten geregistreerd. De gezichten moeten worden gedetecteerd in de afbeeldingen. Nadat al deze stappen zijn uitgevoerd, hebt u een PersonGroup die eruitziet als deze afbeelding:

![HowToIdentify1](../Images/group.image.1.jpg)

### <a name="21-define-people-for-the-persongroup"></a>2.1 Personen voor de PersonGroup definiëren
Een persoon is een basisidentificatie-eenheid. Een gebruiker kan een of meer bekende gezichten geregistreerd hebben. Een PersonGroup is echter een verzameling mensen, en elke persoon wordt gedefinieerd binnen een bepaalde PersonGroup. De identificatie wordt uitgevoerd op basis van een PersonGroup. De taak bestaat dus uit het maken van een PersonGroup en het maken van personen in die PersonGroup, zoals Anna, Bill en Clare.

Eerst moet u een nieuwe PersonGroup maken. Dit doet u met behulp van de API [PersonGroup - Maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244). De bijbehorende clientbibliotheek-API is de methode CreatePersonGroupAsync voor de klasse FaceServiceClient. De groeps-id die is opgegeven bij het maken van de groep is uniek voor elk abonnement. U kunt ook PersonGroups ophalen, bijwerken of verwijderen met behulp van andere PersonGroup-API's. Zodra een groep is gedefinieerd, kunt u personen in de groep definiëren met behulp van de API [PersonGroup Person - Maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c). De clientbibliotheekmethode is CreatePersonAsync. Nadat de personen zijn gemaakt, kunt u aan elke persoon een of meer gezichten toevoegen.

```CSharp 
// Create an empty PersonGroup
string personGroupId = "myfriends";
await faceServiceClient.CreatePersonGroupAsync(personGroupId, "My Friends");
 
// Define Anna
CreatePersonResult friend1 = await faceServiceClient.CreatePersonAsync(
    // Id of the PersonGroup that the person belonged to
    personGroupId,    
    // Name of the person
    "Anna"            
);
 
// Define Bill and Clare in the same way
```
### <a name="step2-2"></a> 2.2 Gezichten detecteren en deze registreren bij de juiste persoon
De detectie wordt uitgevoerd door een POST-webaanvraag naar de API [Face - Detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) te verzenden met het afbeeldingsbestand in de HTTP-aanvraagbody. Wanneer u de clientbibliotheek gebruikt, wordt de gezichtsdetectie uitgevoerd via de methode DetectAsync voor de klasse FaceServiceClient.

Voor elk gedetecteerd gezicht kunt u [PersonGroup Person – Gezicht toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) aanroepen om het gezicht toe te voegen aan de juiste persoon.

In de volgende code ziet u het proces voor het detecteren van een gezicht in een afbeelding en het toevoegen van het gezicht aan een persoon:
```CSharp 
// Directory contains image files of Anna
const string friend1ImageDir = @"D:\Pictures\MyFriends\Anna\";
 
foreach (string imagePath in Directory.GetFiles(friend1ImageDir, "*.jpg"))
{
    using (Stream s = File.OpenRead(imagePath))
    {
        // Detect faces in the image and add to Anna
        await faceServiceClient.AddPersonFaceAsync(
            personGroupId, friend1.PersonId, s);
    }
}
// Do the same for Bill and Clare
``` 
Als de afbeelding meer dan één gezicht bevat, wordt alleen het grootste gezicht toegevoegd. U kunt andere gezichten aan de persoon toevoegen door een tekenreeks in de indeling "targetFace = left, top, width, height" door te geven aan de targetFace-queryparameter van de API [PersonGroup Person - Gezicht toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) of door de optionele targetFace-parameter voor de methode AddPersonFaceAsync te gebruiken voor het toevoegen van andere gezichten. Elk gezicht dat aan de persoon wordt toegevoegd, krijgt een unieke persistente gezichts-id, die kan worden gebruikt in [PersonGroup Person – Gezicht verwijderen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523e) en [Face – Identificeren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

## <a name="step-3-train-the-persongroup"></a>Stap 3: De PersonGroup trainen

De PersonGroup moet worden getraind voordat u er gezichten van personen mee kunt identificeren. De PersonGroup moet bovendien telkens opnieuw worden getraind als een persoon is toegevoegd of verwijderd, of als een persoon zijn of haar geregistreerde gezicht heeft bewerkt. De training wordt uitgevoerd met behulp van de API [PersonGroup – Trainen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249). Wanneer u de clientbibliotheek gebruikt, hoeft u alleen de methode TrainPersonGroupAsync aan te roepen:
 
```CSharp 
await faceServiceClient.TrainPersonGroupAsync(personGroupId);
```
 
Het trainen is een asynchroon proces. Dit proces kan nog actief zijn nadat de methode TrainPersonGroupAsync is geretourneerd. Mogelijk moet u trainingsstatus opvragen met behulp van de API [PersonGroup - Trainingsstatus ophalen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395247) of de methode GetPersonGroupTrainingStatusAsync van de clientbibliotheek. In de volgende code ziet u eenvoudige logica van een PersonGroup-training in afwachting van voltooiing:
 
```CSharp 
TrainingStatus trainingStatus = null;
while(true)
{
    trainingStatus = await faceServiceClient.GetPersonGroupTrainingStatusAsync(personGroupId);
 
    if (trainingStatus.Status != Status.Running)
    {
        break;
    }
 
    await Task.Delay(1000);
} 
``` 

## <a name="step-4-identify-a-face-against-a-defined-persongroup"></a>Stap 4: Een gezicht identificeren op basis van een gedefinieerde PersonGroup

Bij het uitvoeren van identificaties kan de Face-API de overeenkomst tussen een testgezicht en alle gezichten in een groep berekenen en de meest overeenkomende persoon of personen retourneren. Dit gebeurt met behulp van de API [Face - Identificeren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) of de methode IdentifyAsync van de clientbibliotheek.

Het testgezicht moet worden gedetecteerd met behulp van de vorige stappen, waarna de id van het gezicht wordt doorgegeven aan de identificatie-API als een tweede argument. Er kunnen meerdere gezichts-id's tegelijk worden geïdentificeerd en het resultaat bevat alle identificatieresultaten. Standaard wordt alleen de persoon weergegeven die het meeste overeenkomt met het testgezicht. Desgewenst kunt u de optionele parameter maxNumOfCandidatesReturned gebruiken om meer kandidaten te retourneren.

In de volgende code ziet u hoe het identificatieproces in zijn werk gaat:

```CSharp 
string testImageFile = @"D:\Pictures\test_img1.jpg";

using (Stream s = File.OpenRead(testImageFile))
{
    var faces = await faceServiceClient.DetectAsync(s);
    var faceIds = faces.Select(face => face.FaceId).ToArray();
 
    var results = await faceServiceClient.IdentifyAsync(personGroupId, faceIds);
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
            var person = await faceServiceClient.GetPersonAsync(personGroupId, candidateId);
            Console.WriteLine("Identified as {0}", person.Name);
        }
    }
}
``` 

Wanneer u de stappen hebt voltooid, kunt u proberen om andere gezichten te identificeren en zien of de gezichten van Anna, Bill of Clare correct kunnen worden geïdentificeerd aan de hand van de afbeelding(en) die u hebt geüpload voor gezichtsdetectie. Zie de volgende voorbeelden:

![HowToIdentify2](../Images/identificationResult.1.jpg )

## <a name="step-5-request-for-large-scale"></a>Stap 5: Grootschalige capaciteit aanvragen

Zoals bekend, kan een PersonGroup maximaal 10.000 personen bevatten vanwege de eerdere beperking van het ontwerp.
Zie voor meer informatie over scenario's met grootschalige capaciteit voor maximaal een miljoen personen [De functie Grootschalig gebruiken](how-to-use-large-scale.md).

## <a name="summary"></a>Samenvatting

In deze handleiding hebt u het proces voor het maken van een PersonGroup en het identificeren van gezichten geleerd. Hier volgt een snelle herinnering van de functies die eerder werden uitgelegd en gedemonstreerd:

- Gezichten detecteren met behulp van de API [Face - Detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d)
- Het maken van PersonGroups met behulp van de API [PersonGroup - Maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)
- Het maken van personen met behulp van de API [PersonGroup Person - Maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)
- Een PersonGroup trainen met behulp van de API [PersonGroup – Trainen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249)
- Onbekende gezichten identificeren op basis van de PersonGroup met behulp van de API [Face - Identificeren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)

## <a name="related-topics"></a>Verwante onderwerpen

- [Gezichten in een afbeelding detecteren](HowtoDetectFacesinImage.md)
- [Gezichten toevoegen](how-to-add-faces.md)
- [De functie Grootschalig gebruiken](how-to-use-large-scale.md)
