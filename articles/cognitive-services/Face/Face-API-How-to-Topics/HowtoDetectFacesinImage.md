---
title: Detecteer gezichten in een afbeelding - Face-API
titleSuffix: Azure Cognitive Services
description: Informatie over het gebruik van de verschillende gegevens die zijn geretourneerd door de face detection-functie.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: sbowles
ms.openlocfilehash: 46bd1bdd55725878bc7b1bd55d5e24b78d82aada
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66124555"
---
# <a name="get-face-detection-data"></a>Face detection-gegevens ophalen

Deze handleiding laat zien hoe u gezichtsherkenning kunt kenmerken, zoals geslacht, leeftijd of houding onttrekken aan een bepaalde installatiekopie. De codefragmenten in deze handleiding zijn geschreven in C# met behulp van de clientbibliotheek van Azure Cognitive Services Face-API. Dezelfde functionaliteit is beschikbaar via de [REST-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

Deze handleiding ziet u hoe u naar:

- Verkrijg de locaties en dimensies van gezichten wordt uitgevoerd in een afbeelding.
- De locaties van verschillende gezichtsoriëntatiepunten, zoals leerlingen nose en mond vandaan te houden, in een afbeelding ophalen.
- Het geslacht, leeftijd, emotie en andere kenmerken van een gedetecteerde gezicht raden.

## <a name="setup"></a>Instellen

Deze handleiding wordt ervan uitgegaan dat u al gemaakt een [FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet) -object, met de naam `faceClient`, met een sleutel en het eindpunt-URL voor de abonnementen voor Face. Hier kunt u de face detection-functie kunt gebruiken door het aanroepen van een [DetectWithUrlAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithurlasync?view=azure-dotnet), die wordt gebruikt in deze handleiding, of [DetectWithStreamAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithstreamasync?view=azure-dotnet). Zie voor instructies over het instellen van deze functie, de [analyse gezichten quickstart voor C# ](../quickstarts/csharp-detect-sdk.md).

Deze handleiding is gericht op de details van de aanroep van analyse, zoals welke argumenten die u kunt doorgeven en wat u kunt doen met de geretourneerde gegevens. Het is raadzaam dat u een query uitvoeren voor alleen de functies die u nodig hebt. Elke bewerking neemt meer tijd in beslag.

## <a name="get-basic-face-data"></a>Basic face gegevens ophalen

Als u wilt zoeken gezichten en hun locaties in een afbeelding ophalen, roept u de methode met de _returnFaceId_ parameter ingesteld op **waar**. Dit is de standaardinstelling.

```csharp
IList<DetectedFace> faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, true, false, null);
```

U kunt een query de geretourneerde [DetectedFace](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.detectedface?view=azure-dotnet) objecten voor hun unieke id's en een rechthoek waarmee de pixelcoördinaten van het gezicht.

```csharp
foreach (var face in faces)
{
    string id = face.FaceId.ToString();
    FaceRectangle rect = face.FaceRectangle;
}
```

Zie voor meer informatie over het parseren van de locatie en de afmetingen van het gezicht [FaceRectangle](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.facerectangle?view=azure-dotnet). Deze rechthoek bevat normaal gesproken de ogen, eyebrows, nose en mond vandaan te houden. De bovenkant van de hoofd-, klinken en chin zijn niet noodzakelijkerwijs opgenomen. Als u het gezichtsrechthoek wilt bijsnijden een volledige head of een halverwege shot staand, bijvoorbeeld voor een afbeelding van het type-ID ophalen, kunt u de rechthoek in elke richting uitbreiden.

## <a name="get-face-landmarks"></a>Gezichtsoriëntatiepunten ophalen

[Face-oriëntatiepunten](../concepts/face-detection.md#face-landmarks) zijn een set van punten op een gezicht, zoals de leerlingen of de tip van de voorzijde gemakkelijk te vinden. Face oriëntatiepunt gegevens krijgen, stelt u de _returnFaceLandmarks_ parameter **waar**.

```csharp
IList<DetectedFace> faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, true, true, null);
```

De volgende code ziet u hoe u de locaties van de nose en leerlingen mogelijk ophalen:

```csharp
foreach (var face in faces)
{
    var landmarks = face.FaceLandmarks;

    double noseX = landmarks.NoseTip.X;
    double noseY = landmarks.NoseTip.Y;

    double leftPupilX = landmarks.PupilLeft.X;
    double leftPupilY = landmarks.PupilLeft.Y;

    double rightPupilX = landmarks.PupilRight.X;
    double rightPupilY = landmarks.PupilRight.Y;
}
```

U kunt ook face oriëntatiepunten gegevens gebruiken voor een nauwkeurige berekening van de richting van het gezicht. U kunt bijvoorbeeld de draaihoek van het gezicht definiëren als een vector uit het midden van de bek in het midden van de ogen. De volgende code wordt deze vector berekend:

```csharp
var upperLipBottom = landmarks.UpperLipBottom;
var underLipTop = landmarks.UnderLipTop;

var centerOfMouth = new Point(
    (upperLipBottom.X + underLipTop.X) / 2,
    (upperLipBottom.Y + underLipTop.Y) / 2);

var eyeLeftInner = landmarks.EyeLeftInner;
var eyeRightInner = landmarks.EyeRightInner;

var centerOfTwoEyes = new Point(
    (eyeLeftInner.X + eyeRightInner.X) / 2,
    (eyeLeftInner.Y + eyeRightInner.Y) / 2);

Vector faceDirection = new Vector(
    centerOfTwoEyes.X - centerOfMouth.X,
    centerOfTwoEyes.Y - centerOfMouth.Y);
```

Als u de richting van het gezicht weet, kunt u het frame rechthoekige face meer uitlijnen draaien. U kunt via een programma de afbeelding draaien wilt gezichten in een afbeelding bijsnijden, zodat de gezichten altijd wordt weergegeven.

## <a name="get-face-attributes"></a>Face kenmerken ophalen

De face detection-API kunt verschillende algemene kenmerken van een gezicht analyseren naast gezichtsrechthoeken en monumenten. Zie voor een volledige lijst, de [kenmerken](../concepts/face-detection.md#attributes) conceptuele sectie.

Voor het analyseren van de face-kenmerken instellen de _returnFaceAttributes_ parameter voor een lijst met [FaceAttributeType Enum](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.faceattributetype?view=azure-dotnet) waarden.

```csharp
var requiredFaceAttributes = new FaceAttributeType[] {
    FaceAttributeType.Age,
    FaceAttributeType.Gender,
    FaceAttributeType.Smile,
    FaceAttributeType.FacialHair,
    FaceAttributeType.HeadPose,
    FaceAttributeType.Glasses,
    FaceAttributeType.Emotion
};
var faces = await faceClient.DetectWithUrlAsync(imageUrl, true, false, requiredFaceAttributes);
```

Vervolgens ophalen van verwijzingen naar de geretourneerde gegevens en meer bewerkingen op basis van uw behoeften.

```csharp
foreach (var face in faces)
{
    var attributes = face.FaceAttributes;
    var age = attributes.Age;
    var gender = attributes.Gender;
    var smile = attributes.Smile;
    var facialHair = attributes.FacialHair;
    var headPose = attributes.HeadPose;
    var glasses = attributes.Glasses;
    var emotion = attributes.Emotion;
}
```

Zie voor meer informatie over elk van de kenmerken van de [Gezichtsdetectie en kenmerken](../concepts/face-detection.md) conceptuele handleiding.

## <a name="next-steps"></a>Volgende stappen

In deze handleiding, hebt u geleerd hoe u de verschillende functies van gezichtsdetectie. Integreer vervolgens deze functies in uw app door een uitgebreide zelfstudie te volgen.

- [Zelfstudie: Een WPF-app voor de face-gegevens weergeven in een installatiekopie maken](../Tutorials/FaceAPIinCSharpTutorial.md)
- [Zelfstudie: Een Android-app om te detecteren en frame gezichten in een installatiekopie maken](../Tutorials/FaceAPIinJavaForAndroidTutorial.md)

## <a name="related-topics"></a>Verwante onderwerpen

- [Referentiedocumentatie voor (REST)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
- [Referentiedocumentatie voor (.NET SDK)](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/face?view=azure-dotnet)