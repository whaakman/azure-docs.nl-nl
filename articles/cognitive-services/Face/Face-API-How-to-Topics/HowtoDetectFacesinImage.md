---
title: 'Voorbeeld: Gezichten in afbeeldingen detecteren - Face-API'
titleSuffix: Azure Cognitive Services
description: De Face-API gebruiken voor het detecteren van gezichten in afbeeldingen.
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: sample
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: a4c74ff70a4426abf97562bf997479a91afbf17a
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46124045"
---
# <a name="example-how-to-detect-faces-in-image"></a>Voorbeeld: Gezichten in een afbeelding detecteren

Deze handleiding demonstreert hoe u gezichten herkent van een afbeelding, met de gezichtskenmerken als geslacht, leeftijd of de geëxtraheerde houding. De voorbeelden zijn geschreven in C# met de Face-API-clientbibliotheek. 

## <a name="concepts"></a>Concepten

Als u niet bekend bent met een van de volgende concepten in deze handleiding, raadpleegt u de definities van onze [Woordenlijst](../Glossary.md) op elk gewenst moment: 

- Gezichtsdetectie
- Gezichtsoriëntatiepunten
- Hoofdhouding
- Gezichtskenmerken

## <a name="preparation"></a>Voorbereiding

In dit voorbeeld worden de volgende functies toegelicht: 

- Gezichten detecteren in een afbeelding en ze markeren met behulp van rechthoekige framing
- Analyseren van de locaties van pupillen, neus of mond en ze vervolgens te markeren in de afbeelding
- Analyseren van de hoofdhouding, geslacht en leeftijd van het gezicht

U moet een afbeelding voorbereiden met ten minste één helder gezicht om deze functies uit te voeren. 

## <a name="step-1-authorize-the-api-call"></a>Stap 1: De API-aanroep autoriseren

Voor elke aanroep naar de Face-API is een abonnementssleutel vereist. Deze sleutel moet worden doorgegeven via een tekenreeksparameter, of zijn opgegeven in de aanvraagheader. Als u de abonnementssleutel via query-tekenreeks wilt doorgeven, raadpleegt u de aanvraag-URL voor de [Face - Detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) als een voorbeeld:

```
https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes]
&subscription-key=<Subscription Key>
```

Als alternatief, kan de abonnementssleutel ook worden opgegeven in de HTTP-aanvraagheader: **ocp-apim-subscription-key: &lt;Abonnementssleutel&gt;** Wanneer u een clientbibliotheek gebruikt, wordt de abonnementssleutel doorgegeven via de constructor van de klasse FaceServiceClient. Bijvoorbeeld:
```CSharp
faceServiceClient = new FaceServiceClient("<Subscription Key>");
```

## <a name="step-2-upload-an-image-to-the-service-and-execute-face-detection"></a>Stap 2: Een afbeelding naar de service uploaden en gezichtsdetectie uitvoeren

De eenvoudigste manier voor het uitvoeren van gezichtsdetectie is door het rechtstreeks uploaden van een afbeelding. Dit wordt gedaan door een aanvraag "POST" te sturen met het inhoudstype toepassing/octet-stream, met de gegevens gelezen uit een JPEG-afbeelding. De maximale grootte van de parameter is 4 MB.

Met behulp van de clientbibliotheek, wordt gezichtsdetectie met behulp van uploaden uitgevoerd door het doorgeven in een Stream-object. Zie het voorbeeld hieronder:

```CSharp
using (Stream s = File.OpenRead(@"D:\MyPictures\image1.jpg"))
{
    var faces = await faceServiceClient.DetectAsync(s, true, true);
 
    foreach (var face in faces)
    {
        var rect = face.FaceRectangle;
        var landmarks = face.FaceLandmarks;
    }
}
```

Houd er rekening mee dat de methode DetectAsync van FaceServiceClient asynchroon is. De aanroepende methode moet worden gemarkeerd ook als asynchroon om de await-component te gebruiken.
Als de afbeelding al op het web is en een URL bevat, kan gezichtsdetectie worden uitgevoerd door ook een URL te verstrekken. In dit voorbeeld is de aanvraagbody een JSON-tekenreeks die de URL bevat.
Met behulp van de clientbibliotheek, kan gezichtsdetectie met behulp van een URL eenvoudig met behulp van een andere overbelasting van de methode DetectAsync worden uitgevoerd.

```CSharp
string imageUrl = "http://news.microsoft.com/ceo/assets/photos/06_web.jpg";
var faces = await faceServiceClient.DetectAsync(imageUrl, true, true);
 
foreach (var face in faces)
{
    var rect = face.FaceRectangle;
    var landmarks = face.FaceLandmarks;
}
``` 

De eigenschap FaceRectangle die wordt geretourneerd met gedetecteerde gezichten bestaat in feite uit locaties op het gezicht in pixels. Normaal gesproken bevat deze rechthoek de ogen, wenkbrauwen, de neus en de mond – uitgezonderd de bovenkant van de hoofd, oren en de kin. Als u een volledige hoofd- of halve staande opname (een foto-ID-type installatiekopie) bijsnijdt, kunt u het gebied van het rechthoekige gezichtsframe uitbreiden omdat het gebied van het gezicht mogelijk te klein is voor sommige toepassingen. Om een gezicht nog preciezer te zoeken zal het gebruik van gezichtsoriëntatiepunten (gezichtsfuncties of gezichtsrichtingmechanismen) zoals beschreven in de volgende sectie zijn nut bewijzen.

## <a name="step-3-understanding-and-using-face-landmarks"></a>Stap 3: Begrip en gebruik van gezichtsoriëntatiepunten

Gezichtsoriëntatiepunten bestaan uit een reeks van gedetailleerde punten op een gezicht; normaal gesproken punten van gezichtsonderdelen zoals de pupillen, de ooghoek of de neus. Gezichtsoriëntatiepunten zijn optionele kenmerken die tijdens de gezichtsdetectie kunnen worden geanalyseerd. U kunt beide 'waar' als een Booleaanse waarde voor de queryparameter returnFaceLandmarks doorgeven bij het aanroepen van de [Gezicht - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), of de optionele parameter returnFaceLandmarks gebruiken voor de klasse FaceServiceClient DetectAsync-methode om de gezichtsoriëntatiepunten in de richtingsresultaten op te nemen.

Standaard zijn er 27 oriëntatiepunten vooraf gedefinieerde punten. De volgende afbeelding toont hoe alle 27 punten zijn gedefinieerd:

![HowToDetectFace](../Images/landmarks.1.jpg)

De punten worden geretourneerd in eenheden van pixels, net als bij het rechthoekige frame van het gezicht. Daarom wordt het gemakkelijker om specifieke nuttige plaats in de afbeelding te markeren. De volgende code verschijnt bij het ophalen van de locaties van de neus en de pupillen:

```CSharp
var faces = await faceServiceClient.DetectAsync(imageUrl, returnFaceLandmarks:true);
 
foreach (var face in faces)
{
    var rect = face.FaceRectangle;
    var landmarks = face.FaceLandmarks;
 
    double noseX = landmarks.NoseTip.X;
    double noseY = landmarks.NoseTip.Y;
 
    double leftPupilX = landmarks.PupilLeft.X;
    double leftPupilY = landmarks.PupilLeft.Y;
 
    double rightPupilX = landmarks.PupilRight.X;
    double rightPupilY = landmarks.PupilRight.Y;
}
``` 

Naast de markering van gezichtsfuncties in een afbeelding, worden gezichtsoriëntatiepunten ook gebruikt voor een nauwkeurige berekening van de richting van het gezicht. We kunnen bijvoorbeeld de richting van het gezicht definiëren als een vector uit het midden van de mond naar het midden van de ogen. De code hieronder legt dit in detail uit:

```CSharp
var landmarks = face.FaceLandmarks;
 
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

Met de kennis van de richting waarin het gezicht zich bevindt, kunt u vervolgens het rechthoekige gezichtsframe draaien om dit met het gezicht uit te lijnen. Het is duidelijk dat met het gebruik van gezichtsoriëntatiepunten meer details en een hulpprogramma bieden.

## <a name="step-4-using-other-face-attributes"></a>Stap 4: Gebruik van andere gezichtskenmerken

Naast de gezichtsoriëntatiepunten, kan Gezicht - Detecteren API ook diverse andere kenmerken van een gezicht analyseren. Deze kenmerken zijn onder andere:

- Leeftijd
- Geslacht
- Glimlach-intensiteit
- Gezichtshaar
- Een 3D-hoofdhouding

Deze kenmerken worden voorspeld met behulp van statistische algoritmen en zijn mogelijk niet altijd 100% nauwkeurig. Ze zijn echter nog steeds nuttig als u gezichten wilt classificeren met deze kenmerken. Raadpleeg voor meer informatie over elk van de kenmerken de [Woordenlijst](../Glossary.md).

Hieronder volgt een eenvoudig voorbeeld van het extraheren van gezichtskenmerken tijdens gezichtsdetectie:

```CSharp
var requiredFaceAttributes = new FaceAttributeType[] {
                FaceAttributeType.Age,
                FaceAttributeType.Gender,
                FaceAttributeType.Smile,
                FaceAttributeType.FacialHair,
                FaceAttributeType.HeadPose,
                FaceAttributeType.Glasses
            };
var faces = await faceServiceClient.DetectAsync(imageUrl,
    returnFaceLandmarks: true,
    returnFaceAttributes: requiredFaceAttributes);

foreach (var face in faces)
{
    var id = face.FaceId;
    var attributes = face.FaceAttributes;
    var age = attributes.Age;
    var gender = attributes.Gender;
    var smile = attributes.Smile;
    var facialHair = attributes.FacialHair;
    var headPose = attributes.HeadPose;
    var glasses = attributes.Glasses;
}
``` 

## <a name="summary"></a>Samenvatting

In deze handleiding hebt u de functionaliteiten van [Gezicht - Detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API geleerd, hoe dit gezichten kan detecteren voor lokale geüploade afbeeldingen of afbeelding-URL's op het web ; hoe dit gezichten kan detecteren door rechthoekige gezichtsframes te retourneren; en hoe dit ook gezichtsoriëntatiepunten, 3D-gezichtshoudingen en andere gezichtskenmerken kan analyseren.

Raadpleeg voor meer informatie over API-details, de API-naslaggids voor [Gezicht - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="related-topics"></a>Verwante onderwerpen

[Gezichten in een afbeelding identificeren](HowtoIdentifyFacesinImage.md)
