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
ms.date: 02/22/2019
ms.author: sbowles
ms.openlocfilehash: bf3af8f5d1d2f063199a8275c2f49c70140e8732
ms.sourcegitcommit: 89b5e63945d0c325c1bf9e70ba3d9be6888da681
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2019
ms.locfileid: "57588768"
---
# <a name="get-face-detection-data"></a>Face detection-gegevens ophalen

Deze handleiding wordt gedemonstreerd hoe u gezichtsherkenning kunt kenmerken, zoals geslacht, leeftijd of houding onttrekken aan een bepaalde installatiekopie. De codefragmenten in deze handleiding zijn geschreven in C# met behulp van de Face-API-clientbibliotheek, maar dezelfde functionaliteit is beschikbaar via de [REST-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

Deze handleiding leert u hoe aan:

- Verkrijg de locaties en dimensies van gezichten wordt uitgevoerd in een afbeelding.
- De locaties van verschillende gezichtsoriëntatiepunten (leerlingen, nose, mond vandaan te houden, enzovoort) in een afbeelding ophalen.
- Het geslacht, leeftijd, en emoties en andere kenmerken van een gedetecteerde gezicht raden.

## <a name="setup"></a>Instellen

Deze handleiding wordt ervan uitgegaan dat u al hebt gemaakt een **[FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet)** -object, met de naam `faceClient`, met een sleutel en het eindpunt-URL voor de abonnementen voor Face. Hier kunt u de face detection-functie kunt gebruiken door het aanroepen van een **[DetectWithUrlAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithurlasync?view=azure-dotnet)** (gebruikt in deze handleiding) of **[DetectWithStreamAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithstreamasync?view=azure-dotnet)**. Zie de [Snelstartgids voor het detecteren van gezichten C# ](../quickstarts/csharp-detect-sdk.md) voor instructies over hoe u dit instellen.

Deze handleiding wordt de nadruk gelegd op de details van de analyse-aanroep&mdash;welke argumenten die u kunt doorgeven en wat u kunt doen met de geretourneerde gegevens. Het is raadzaam om alleen query's uitvoeren voor de functies die u nodig hebt, omdat elke bewerking meer tijd neemt in beslag.

## <a name="get-basic-face-data"></a>Basic face gegevens ophalen

Als u wilt zoeken gezichten en hun locaties in een afbeelding ophalen, roept u de methode met de _returnFaceId_ parameter ingesteld op **waar** (standaard).

```csharp
IList<DetectedFace> faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, true, false, null);
```

De geretourneerde **[DetectedFace](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.detectedface?view=azure-dotnet)** objecten kunnen worden opgevraagd voor de unieke id's en een rechthoek waarmee de pixelcoördinaten van het gezicht.

```csharp
foreach (var face in faces)
{
    string id = face.FaceId.ToString();
    FaceRectangle rect = face.FaceRectangle;
}
```

Zie **[FaceRectangle](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.facerectangle?view=azure-dotnet)** voor meer informatie over het parseren van de locatie en de afmetingen van het gezicht. Normaal gesproken bevat deze rechthoek de ogen, eyebrows, nose en mond; de bovenkant van de hoofd-, klinken en chin zijn niet noodzakelijkerwijs opgenomen. Als u van plan bent met het gezichtsrechthoek bijsnijden een volledige hoofd- of halverwege shot Staand (een foto-ID-type afbeelding), kunt u om uit te breiden, de rechthoek met een bepaalde marge in elke richting.

## <a name="get-face-landmarks"></a>Gezichtsoriëntatiepunten ophalen

Gezichtsoriëntatiepunten zijn een verzameling van punten op een gezicht, zoals de leerlingen gemakkelijk te vinden of de punt van nose. U kunt face oriëntatiepunt gegevens ophalen door in te stellen de _returnFaceLandmarks_ parameter **waar**.

```csharp
IList<DetectedFace> faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, true, true, null);
```

Standaard zijn er 27 oriëntatiepunten vooraf gedefinieerde punten. De volgende afbeelding ziet u alle 27 punten:

![Een diagram face met alle 27 oriëntatiepunten gelabeld](../Images/landmarks.1.jpg)

De punten die wordt geretourneerd, zijn in eenheden van pixels, net als bij het kader van de rechthoek face. De volgende code ziet u hoe u de locaties van de nose en leerlingen mogelijk ophalen:

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

Face oriëntatiepunten gegevens kunnen ook worden gebruikt voor een nauwkeurige berekening van de richting van het gezicht. We kunnen bijvoorbeeld de rotatie van de face definiëren als een vector uit het midden van de bek in het midden van de ogen. De onderstaande code berekent deze vector:

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

De richting van het gezicht weet, kunt u vervolgens draaien het frame rechthoekige face meer uitlijnen. Als u gezichten in een afbeelding bijsnijden wilt, kunt u de installatiekopie via een programma draaien zodat de gezichten altijd wordt weergegeven.

## <a name="get-face-attributes"></a>Face kenmerken ophalen

De face detection-API kunt verschillende algemene kenmerken van een gezicht analyseren naast gezichtsrechthoeken en monumenten. Deze omvatten:

- Leeftijd
- Geslacht
- Glimlach-intensiteit
- Gezichtshaar
- Bril
- 3D-head houding
- Emotion

> [!IMPORTANT]
> Deze kenmerken worden voorspeld door het gebruik van statistische algoritmen en altijd mogelijk niet in nauwkeurig. Wees voorzichtig bij het nemen van besluiten op basis van kenmerkgegevens.
>

Voor het analyseren van de face-kenmerken instellen de _returnFaceAttributes_ parameter voor een lijst met **[FaceAttributeType Enum](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.faceattributetype?view=azure-dotnet)** waarden.

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

Vervolgens ophalen van verwijzingen naar de geretourneerde gegevens en verder doen operations op basis van uw behoeften.

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

Raadpleeg voor meer informatie over elk van de kenmerken van de [verklarende woordenlijst](../Glossary.md).

## <a name="next-steps"></a>Volgende stappen

In deze handleiding hebt u geleerd hoe u de verschillende functies van gezichtsdetectie. Hieronder staan de [verklarende woordenlijst](../Glossary.md) voor een meer gedetailleerde Kijk op de face-gegevens die u hebt opgehaald.

## <a name="related-topics"></a>Verwante onderwerpen

- [Referentiedocumentatie voor (REST)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
- [Referentiedocumentatie voor (.NET SDK)](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/face?view=azure-dotnet)
