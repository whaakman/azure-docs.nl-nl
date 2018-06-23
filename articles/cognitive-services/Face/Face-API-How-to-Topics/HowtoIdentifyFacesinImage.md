---
title: Identificeren van vlakken in afbeeldingen met de Face-API | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: De Face-API in cognitieve Services gebruiken om te identificeren vlakken in afbeeldingen.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 3f75db176055d9f784ec978497d7cae077ff629f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345743"
---
# <a name="how-to-identify-faces-in-images"></a>Vlakken in installatiekopieën identificeren

Deze handleiding wordt uitgelegd hoe onbekende vlakken PersonGroups die vooraf worden gemaakt van bekende personen met identificeren. De voorbeelden zijn geschreven in C# met behulp van de clientbibliotheek Face-API.

## <a name="concepts"></a> Concepten

Als u niet bekend met de volgende concepten in deze handleiding bent, zoek de definities van onze [verklarende woordenlijst](../Glossary.md) op elk gewenst moment:

- Geconfronteerd - detecteren
- Geconfronteerd: bepalen
- PersonGroup

## <a name="preparation"></a> Voorbereiding

In dit voorbeeld ziet u het volgende:

- Het maken van een PersonGroup - deze PersonGroup bevat een lijst met bekende personen.
- Vlakken toewijzen aan elke persoon - deze vlakken worden gebruikt als basis voor het identificeren van mensen. Het verdient aanbeveling gebruik wissen front vlakken, net als uw foto-ID. Een aantal foto's moet vlakken van dezelfde persoon in verschillende vormt de kleding kleuren of haar stijlen bevatten.

Als u wilt de demonstratie van dit voorbeeld is uitgevoerd, moet u een aantal afbeeldingen voorbereiden:

- Een paar foto's met de persoon face. [Klik hier om te downloaden van voorbeeld foto's](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data) voor Anna-, factuur- en Clare.
- Een reeks testen foto's, die kunnen of mogen niet de vlakken van Anna, factuur of Clare gebruikt voor de test-ID. U kunt ook enkele voorbeeldafbeeldingen selecteren via de bovenstaande koppeling.

## <a name="step1"></a> Stap 1: Machtig de API-aanroep

Elke aanroep van de Face-API is een abonnementssleutel vereist. Deze sleutel kunt worden doorgegeven via een queryreeksparameter of opgegeven in de aanvraagheader. Als u wilt doorgeven van de sleutel van het abonnement via de queryreeks, raadpleegt u de aanvraag-URL voor de [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) als een voorbeeld:
```
https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes]
&subscription-key=<Subscription key>
```

Als alternatief kan de abonnementssleutel ook worden opgegeven in de HTTP-aanvraag-header: **ocp-apim-subscription-key: &lt;Abonnementssleutel&gt;**  wanneer u een clientbibliotheek, de abonnementssleutel wordt doorgegeven via de constructor van de klasse FaceServiceClient. Bijvoorbeeld:
 
```CSharp 
faceServiceClient = new FaceServiceClient("<Subscription Key>");
```
 
De abonnementssleutel kan worden verkregen via de Marketplace-pagina van uw Azure-portal. Zie [abonnementen](https://azure.microsoft.com/try/cognitive-services/).

## <a name="step2"></a> Stap 2: De PersonGroup maken

In deze stap, hebben we een PersonGroup met de naam 'MyFriends' met drie personen gemaakt: Anna-, factuur- en Clare. Elke persoon heeft verschillende vlakken geregistreerd. De vlakken moeten worden gedetecteerd, van de installatiekopieën. Nadat alle deze stappen hebt u een PersonGroup zoals de volgende afbeelding:

![HowToIdentify1](../Images/group.image.1.jpg)

### <a name="step2-1"></a> 2.1 personen voor de PersonGroup definiëren
Een persoon is een basic-eenheid identificeren. Een gebruiker kan een of meer bekende vlakken geregistreerd hebben. Echter een PersonGroup is een verzameling van mensen en iedereen is gedefinieerd binnen een bepaalde PersonGroup. De identificatie wordt uitgevoerd voor een PersonGroup. De taak is dus een PersonGroup maken en vervolgens mensen te maken in, zoals Anna factuur en Clare.

U moet eerst een nieuwe PersonGroup maken. Dit wordt uitgevoerd met behulp van de [PersonGroup - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API. De bijbehorende clientbibliotheek API is de methode CreatePersonGroupAsync voor de klasse FaceServiceClient. De groeps-ID opgegeven voor het maken van de groep is uniek voor elk abonnement – u kunt ook verkrijgen, bijwerken of verwijderen met behulp van andere PersonGroup APIs PersonGroups. Als er een groep is gedefinieerd, mensen kan vervolgens worden gedefinieerd binnen dit met behulp van de [PersonGroup persoon - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API. De client-bibliotheek-methode is CreatePersonAsync. U kunt face toevoegen aan elke persoon nadat ze zijn gemaakt.

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
### <a name="step2-2"></a> 2.2 vlakken detecteren en registreren op de juiste persoon
Detectie wordt uitgevoerd door het verzenden van een webaanvraag "POST" naar de [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API met het installatiekopiebestand in de HTTP-aanvraag. Wanneer u de clientbibliotheek gebruikt, wordt met de methode DetectAsync voor de klasse FaceServiceClient face detection uitgevoerd.

Voor elk vlak ontdekt u kunt aanroepen [PersonGroup persoon – Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) toe te voegen aan de juiste persoon.

De volgende code toont het proces van het detecteren van een gezicht van een installatiekopie en voeg deze toe aan een persoon:
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
U ziet dat als de installatiekopie meer dan één face bevat, alleen de grootste face is toegevoegd. U kunt andere vlakken toevoegen aan de persoon die door een reeks wordt doorgegeven in de indeling van ' targetFace links, top, width en height = ' naar [PersonGroup persoon - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API van de queryparameter targetFace of met behulp van de optionele parameter targetFace voor de AddPersonFaceAsync methode voor het toevoegen van andere vlakken. Elk vlak toegevoegd aan de persoon krijgt een unieke persistente face-ID, die kan worden gebruikt in [PersonGroup persoon – Face verwijderen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523e) en [geconfronteerd – identificeren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).
## <a name="step3"></a> Stap 3: De PersonGroup trainen

De PersonGroup moet worden getraind voordat een identificatie kan worden uitgevoerd met behulp van deze. Bovendien heeft om te worden retrained na het toevoegen of verwijderen van een persoon of als een persoon hun geregistreerde face bewerkt heeft. De training wordt uitgevoerd door de [PersonGroup – Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) API. Bij gebruik van de clientbibliotheek is gewoon een aanroep van de methode TrainPersonGroupAsync:
 
```CSharp 
await faceServiceClient.TrainPersonGroupAsync(personGroupId);
```
 
De training is een asynchrone proces. Deze kan niet worden voltooid zelfs nadat de methode TrainPersonGroupAsync heeft geretourneerd. Mogelijk moet u de trainingsstatus door opvragen [PersonGroup - Training Status ophalen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395247) API of GetPersonGroupTrainingStatusAsync-methode van de clientbibliotheek. De volgende code toont een eenvoudige logische wachten PersonGroup training om te kunnen voltooien:
 
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

## <a name="step4"></a> Stap 4: Een gezicht tegen een gedefinieerde PersonGroup identificeren
Bij het uitvoeren van id's, de Face-API de overeenkomsten van een gezicht test tussen alle vlakken binnen een groep kunt berekenen en de meest vergelijkbare die voor die test face retourneert. Dit wordt gedaan door de [geconfronteerd: bepalen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) API of de methode IdentifyAsync van de clientbibliotheek.

De test face moet worden gedetecteerd met behulp van de vorige stappen en vervolgens de face-ID wordt doorgegeven aan de gebruikersidentiteit API als een tweede argument. Meerdere face id's tegelijk kan worden geïdentificeerd en het resultaat bevat alle identificeren resultaten. Standaard retourneert de gebruikersidentiteit slechts één persoon die het beste komt overeen met de test-zijde. Als u liever, kunt u de optionele parameter maxNumOfCandidatesReturned zodat de gebruikersidentiteit meer kandidaten retourneren.

De volgende code toont het proces voor het identificeren:
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

Wanneer u de stappen hebt voltooid, kunt u proberen om te bepalen van de verschillende vlakken en zien als de vlakken van Anna, factuur of Clare kunnen worden geïdentificeerd, volgens de installatiekopieën voor face detection geüpload. Zie de volgende voorbeelden:

![HowToIdentify2](../Images/identificationResult.1.jpg )

## <a name="step5"></a> Stap 5: Aanvraag voor grootschalige

Als u bekend is, kan een PersonGroup maximaal 10.000 personen vanwege de beperking van de vorige ontwerp bevatten.
Voor meer informatie over tot miljoen scale scenario's, Zie [het gebruik van de functie voor grootschalige](how-to-use-large-scale.md).

## <a name="summary"></a> Samenvatting

In deze handleiding hebt u geleerd het proces van het maken van een PersonGroup en een persoon te identificeren. Hieronder vindt u een snelle herinnering van de functies die eerder zijn beschreven en gedemonstreerd:

- Detecteren bespreekt met behulp van de [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d) API
- Maken van PersonGroups met behulp van de [PersonGroup - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API
- Maken van personen met behulp van de [PersonGroup persoon - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API
- Trainen een PersonGroup met de [PersonGroup – Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) API
- Onbekende vlakken tegen het gebruik van de PersonGroup identificeren de [geconfronteerd: bepalen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) API

## <a name="related"></a> Verwante onderwerpen

- [Het vaststellen van bespreekt in afbeelding](HowtoDetectFacesinImage.md)
- [Het toevoegen van bespreekt](how-to-add-faces.md)
- [Het gebruik van de functie voor grootschalige](how-to-use-large-scale.md)
