---
title: Het kenmerk HeadPose gebruiken
titleSuffix: Azure Cognitive Services
description: Informatie over het gebruik van het kenmerk HeadPose automatisch draaien het gezichtsrechthoek of head gebaren detecteren in een video-feed.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 05/29/2019
ms.author: pafarley
ms.openlocfilehash: 168b4fce873206e39a32a83da3dc5509b431d6a1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67058571"
---
# <a name="use-the-headpose-attribute"></a>Het kenmerk HeadPose gebruiken

In deze handleiding ziet u hoe u het kenmerk HeadPose van een gedetecteerde gezicht kunt gebruiken om in te schakelen van enkele belangrijke scenario's.

## <a name="rotate-the-face-rectangle"></a>Het gezichtsrechthoek draaien

Het gezichtsrechthoek geretourneerd met elke gedetecteerde gezicht, markeert de locatie en grootte van het gezicht in de afbeelding. De rechthoek is standaard altijd uitgelijnd met de installatiekopie (zijn kanten zijn verticaal en horizontaal); Dit kan zijn voor framing schuine gezichten inefficiënt. In situaties waar u programmatisch bijsnijden gezichten in een afbeelding, is het beter om te kunnen draaien van de rechthoek bijsnijden.

De [Cognitive Services Face WPF](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) voorbeeld-app gebruikt de HeadPose-kenmerk voor de gedetecteerde gezichtsrechthoeken draaien.

### <a name="explore-the-sample-code"></a>De voorbeeldcode verkennen

U kunt het gezichtsrechthoek programmatisch met behulp van het kenmerk HeadPose draaien. Als u dit kenmerk opgeeft bij het detecteren van gezichten (Zie [hoe u gezichtsherkenning kunt](HowtoDetectFacesinImage.md)), kunt u zich later query kunnen uitvoeren. De volgende methode van de [Cognitive Services Face WPF](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) app neemt een lijst **DetectedFace** objecten en retourneert een lijst van **[Face](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/app-samples/Cognitive-Services-Face-WPF/Sample-WPF/Controls/Face.cs)** objecten. **Face** Hier volgt een aangepaste klasse dat opgeslagen gegevens, inclusief de bijgewerkte rechthoekcoördinaten geconfronteerd. Nieuwe waarden worden berekend voor **boven**, **links**, **breedte**, en **hoogte**, en een nieuw veld **FaceAngle**geeft de draaiing.

```csharp
/// <summary>
/// Calculate the rendering face rectangle
/// </summary>
/// <param name="faces">Detected face from service</param>
/// <param name="maxSize">Image rendering size</param>
/// <param name="imageInfo">Image width and height</param>
/// <returns>Face structure for rendering</returns>
public static IEnumerable<Face> CalculateFaceRectangleForRendering(IList<DetectedFace> faces, int maxSize, Tuple<int, int> imageInfo)
{
    var imageWidth = imageInfo.Item1;
    var imageHeight = imageInfo.Item2;
    var ratio = (float)imageWidth / imageHeight;
    int uiWidth = 0;
    int uiHeight = 0;
    if (ratio > 1.0)
    {
        uiWidth = maxSize;
        uiHeight = (int)(maxSize / ratio);
    }
    else
    {
        uiHeight = maxSize;
        uiWidth = (int)(ratio * uiHeight);
    }

    var uiXOffset = (maxSize - uiWidth) / 2;
    var uiYOffset = (maxSize - uiHeight) / 2;
    var scale = (float)uiWidth / imageWidth;

    foreach (var face in faces)
    {
        var left = (int)(face.FaceRectangle.Left * scale + uiXOffset);
        var top = (int)(face.FaceRectangle.Top * scale + uiYOffset);

        // Angle of face rectangles, default value is 0 (not rotated).
        double faceAngle = 0;

        // If head pose attributes have been obtained, re-calculate the left & top (X & Y) positions.
        if (face.FaceAttributes?.HeadPose != null)
        {
            // Head pose's roll value acts directly as the face angle.
            faceAngle = face.FaceAttributes.HeadPose.Roll;
            var angleToPi = Math.Abs((faceAngle / 180) * Math.PI);

            // _____       | / \ |
            // |____|  =>  |/   /|
            //             | \ / |
            // Re-calculate the face rectangle's left & top (X & Y) positions.
            var newLeft = face.FaceRectangle.Left +
                face.FaceRectangle.Width / 2 -
                (face.FaceRectangle.Width * Math.Sin(angleToPi) + face.FaceRectangle.Height * Math.Cos(angleToPi)) / 2;

            var newTop = face.FaceRectangle.Top +
                face.FaceRectangle.Height / 2 -
                (face.FaceRectangle.Height * Math.Sin(angleToPi) + face.FaceRectangle.Width * Math.Cos(angleToPi)) / 2;

            left = (int)(newLeft * scale + uiXOffset);
            top = (int)(newTop * scale + uiYOffset);
        }

        yield return new Face()
        {
            FaceId = face.FaceId?.ToString(),
            Left = left,
            Top = top,
            OriginalLeft = (int)(face.FaceRectangle.Left * scale + uiXOffset),
            OriginalTop = (int)(face.FaceRectangle.Top * scale + uiYOffset),
            Height = (int)(face.FaceRectangle.Height * scale),
            Width = (int)(face.FaceRectangle.Width * scale),
            FaceAngle = faceAngle,
        };
    }
}
```

### <a name="display-the-updated-rectangle"></a>De bijgewerkte rechthoek weergeven

Hier kunt u de geretourneerde **Face** objecten in uw weer te geven. De volgende regels van [FaceDetectionPage.xaml](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/app-samples/Cognitive-Services-Face-WPF/Sample-WPF/Controls/FaceDetectionPage.xaml) laten zien hoe de nieuwe rechthoek is opgebouwd op basis van deze gegevens:

```xaml
 <DataTemplate>
    <Rectangle Width="{Binding Width}" Height="{Binding Height}" Stroke="#FF26B8F4" StrokeThickness="1">
        <Rectangle.LayoutTransform>
            <RotateTransform Angle="{Binding FaceAngle}"/>
        </Rectangle.LayoutTransform>
    </Rectangle>
</DataTemplate>
```

## <a name="detect-head-gestures"></a>Ga gebaren detecteren

U kunt head gebaren zoals nodding en head schudden door het bijhouden van wijzigingen in realtime HeadPose detecteren. U kunt deze functie gebruiken als een aangepaste liveness detector.

Liveness detectie is de taak te bepalen of een onderwerp een echte persoon en niet een afbeelding of video-weergave is. Een gebaar van de hoofd-detector kan fungeren als een manier om u te helpen controleren liveness, met name in plaats van de weergave van een installatiekopie van een persoon.

> [!CAUTION]
> Als u wilt detecteren head gebaren in realtime, moet u de Face-API aanroepen met een hoge frequentie (meer dan één keer per seconde). Als u een gratis-laag (f0)-abonnement hebt, is dit niet mogelijk. Als u een betaalde laag-abonnement hebt, zorg ervoor dat u hebt berekend dat de kosten van het maken van snelle API aanroepen voor de kop gebaar detectie.

Zie de [Face-API-HeadPose voorbeeld](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceAPIHeadPoseSample) op GitHub voor een voorbeeld van een werkende van hoofd gebaar van detectie.

## <a name="next-steps"></a>Volgende stappen

Zie de [Cognitive Services Face WPF](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) -app op GitHub voor een werkvoorbeeld van gezichtsrechthoeken gedraaid. Of Raadpleeg de [Face-API-HeadPose voorbeeld](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples) app, die het kenmerk HeadPose in realtime voor het detecteren van hoofd verplaatsingen van het type worden bijgehouden.