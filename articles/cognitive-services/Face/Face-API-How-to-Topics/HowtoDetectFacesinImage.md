---
title: Vlakken detecteren in afbeeldingen met de Face-API | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: De Face-API in cognitieve Services gebruiken voor het detecteren van vlakken in afbeeldingen.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 57cd0915450428399fd680638aa4fae2cdbe17c9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344523"
---
# <a name="how-to-detect-faces-in-image"></a>Het vaststellen van bespreekt in afbeelding

Deze handleiding wordt gedemonstreerd hoe kunt detecteren vlakken van een installatiekopie, met face-kenmerken zoals geslacht, leeftijd of pose hebt uitgepakt. De voorbeelden zijn geschreven in C# met behulp van de clientbibliotheek Face-API. 

## <a name="concepts"></a> Concepten

Als u niet bekend met een van de volgende concepten in deze handleiding bent, raadpleegt u de definities van onze [verklarende woordenlijst](../Glossary.md) op elk gewenst moment: 

- Gezichtsherkenning
- Face monumenten
- HEAD vormen
- Face-kenmerken

## <a name="preparation"></a> Voorbereiding

In dit voorbeeld wordt gedemonstreerd de volgende functies: 

- Vlakken van een installatiekopie te detecteren en ze te markeren met behulp van rechthoekige framing
- De locaties van pupillen, de neus of mond te analyseren en ze vervolgens te markeren in de afbeelding
- Analyse van de head pose, geslacht en leeftijd van het oppervlak

Om deze functies worden uitgevoerd, moet u een installatiekopie met ten minste één wissen face voorbereiden. 

## <a name="step1"></a> Stap 1: Machtig de API-aanroep

Elke aanroep van de Face-API is een abonnementssleutel vereist. Deze sleutel moet worden doorgegeven via een queryreeksparameter of opgegeven in de aanvraagheader. Als u wilt doorgeven van de sleutel van het abonnement via de queryreeks, raadpleegt u de aanvraag-URL voor de [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) als een voorbeeld:

```
https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes]
&subscription-key=<Subscription Key>
```

Als alternatief kan de abonnementssleutel ook worden opgegeven in de HTTP-aanvraag-header: **ocp-apim-subscription-key: &lt;Abonnementssleutel&gt;**  wanneer u een clientbibliotheek, de abonnementssleutel wordt doorgegeven via de constructor van de klasse FaceServiceClient. Bijvoorbeeld:
```CSharp
faceServiceClient = new FaceServiceClient("<Subscription Key>");
```

## <a name="step2"></a> Stap 2: Een installatiekopie uploaden naar de service en face detectie uitvoeren

De eenvoudigste manier om uit te voeren face detection is door een afbeelding rechtstreeks te uploaden. Dit wordt gedaan door het verzenden van een aanvraag "POST" met het inhoudstype application/octet-stream met de gegevens lezen uit een JPEG-afbeelding. De maximale grootte van de afbeelding is 4 MB.

Met behulp van de clientbibliotheek wordt face detection middels het uploaden gedaan door door te geven in een Stream-object. Zie het voorbeeld hieronder:
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

Houd er rekening mee dat de methode DetectAsync van FaceServiceClient async is. De aangeroepen methode moet worden gemarkeerd als async, ook als u wilt de await-component gebruiken.
Als de installatiekopie al op het web is en een URL, kan face detection worden uitgevoerd door ook de URL. In dit voorbeeld worden de aanvraagtekst een JSON-tekenreeks, die de URL bevat.
Met behulp van de clientbibliotheek kan face detection door middel van een URL worden uitgevoerd met een andere overload van de methode DetectAsync.
```CSharp
string imageUrl = "http://news.microsoft.com/ceo/assets/photos/06_web.jpg";
var faces = await faceServiceClient.DetectAsync(imageUrl, true, true);
 
foreach (var face in faces)
{
    var rect = face.FaceRectangle;
    var landmarks = face.FaceLandmarks;
}
``` 

De FaceRectangle-eigenschap die wordt geretourneerd met gedetecteerde vlakken is in wezen locaties op het vlak in pixels. Normaal gesproken deze rechthoek bevat de ogen, eyebrows, de neus en de mond – de bovenkant van het hoofd, oren en de kin zijn niet opgenomen. Als u een volledige head of halverwege geschoten Staand (een foto-ID type afbeelding) bijsnijdt, wilt u mogelijk Vouw het gebied van de rechthoekige face-frame, omdat het gebied van het oppervlak mogelijk te klein voor sommige toepassingen. Een gezicht preciezer, vinden met behulp van face monumenten (zoek face-functies of richting mechanismen geconfronteerd) die worden beschreven in de volgende sectie wordt bewijzen nuttig.

## <a name="step3"></a> Stap 3: Begrijpen en gebruiken van face monumenten

Face monumenten zijn een aantal gedetailleerde punten op een gezicht; doorgaans punten face-onderdelen, zoals de pupillen, canthus of neus. Face monumenten zijn optionele kenmerken die tijdens de detectie van face kunnen worden geanalyseerd. U kunt beide pass 'true' als een Booleaanse waarde in de queryparameter returnFaceLandmarks bij het aanroepen van de [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), of de optionele parameter returnFaceLandmarks voor de klasse FaceServiceClient DetectAsync methode in volgorde van gebruik de monumenten face opnemen in de resultaten.

Er zijn standaard 27 vooraf gedefinieerde kenmerkende punten. De volgende afbeelding toont hoe alle 27 punten worden gedefinieerd:

![HowToDetectFace](../Images/landmarks.1.jpg)

De geretourneerde punten zijn in pixels, net als de rechthoekige face-frame-eenheden. Waardoor u ze eenvoudiger markeren specifieke punten in de installatiekopie van het van belang. De volgende code toont de locaties van de neus en pupillen ophalen:
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

Naast de markering face-functies in een installatiekopie, worden face monumenten ook gebruikt voor het berekenen van de richting van het oppervlak nauwkeurig. Bijvoorbeeld, definiëren we de richting van het oppervlak als vector vanaf het midden van de mond naar het midden van de ogen. De volgende code wordt dit in detail uitgelegd:

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

De richting van de zijde is in weet, kunt u vervolgens het frame rechthoekige face wilt uitlijnen op het vlak draaien. Het is duidelijk dat met face monumenten meer detail en -hulpprogramma bieden kunt.

## <a name="step4"></a> Stap 4: Met behulp van andere face-kenmerken

API detecteren naast face monumenten, Face – kunt ook verschillende kenmerken van een gezicht analyseren. Deze kenmerken zijn onder andere:

- Leeftijd
- Geslacht
- Intensiteit glimlach
- Analyseert haar
- Een 3D-head pose

Deze kenmerken worden voorspeld met behulp van statistische algoritmen en mag niet altijd 100% nauwkeurig zijn. Ze zijn echter nog steeds handig als u wilt classificeren vlakken door deze kenmerken. Raadpleeg voor meer informatie over elk van de kenmerken van de [verklarende woordenlijst](../Glossary.md).

Hieronder vindt u een eenvoudig voorbeeld van het extraheren van face kenmerken tijdens de detectie van face:
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
## <a name="summary"></a> Samenvatting

In deze handleiding hebt u geleerd, de functionaliteit van [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API, hoe deze vlakken kunt detecteren voor lokale geüpload installatiekopieën of afbeelding-URL's op het web; hoe vlakken kunnen worden gedetecteerd door te retourneren van de rechthoekige face frames; en hoe deze ook kunt analyseren Face monumenten, 3D head vormt en andere face-kenmerken.

Raadpleeg voor meer informatie over API de API-Naslaggids voor [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="related"></a> Verwante onderwerpen

[Het identificeren van bespreekt in afbeelding](HowtoIdentifyFacesinImage.md)
