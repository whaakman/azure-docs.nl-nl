---
title: HeadPose gebruiken om aan te passen de gezichtsrechthoek
titleSuffix: Azure Cognitive Services
description: Informatie over het gebruik van het kenmerk HeadPose automatisch draaien het gezichtsrechthoek.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: pafarley
ms.openlocfilehash: ddc5bc522c0d3ac258581f2a48a5c3b755302f01
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/26/2019
ms.locfileid: "64576500"
---
# <a name="use-the-headpose-attribute-to-adjust-the-face-rectangle"></a>Het kenmerk HeadPose gebruiken om aan te passen de gezichtsrechthoek

In deze handleiding gebruikt u een kenmerk van de gedetecteerde face, HeadPose, draait de rechthoek van een gezicht-object. Het voorbeeld van code in deze handleiding worden uit de [Cognitive Services Face WPF](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) voorbeeld-app maakt gebruik van de .NET SDK.

Het gezichtsrechthoek geretourneerd met elke gedetecteerde gezicht, markeert de locatie en grootte van het gezicht in de afbeelding. De rechthoek is standaard altijd uitgelijnd met de installatiekopie (zijn kanten zijn perfect verticaal en horizontaal); Dit kan zijn voor framing schuine gezichten inefficiënt. In situaties waar u programmatisch bijsnijden gezichten in een afbeelding, wordt u aangeraden om het draaien van de rechthoek bijsnijden te kunnen.

## <a name="explore-the-sample-code"></a>De voorbeeldcode verkennen

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

## <a name="display-the-updated-rectangle"></a>De bijgewerkte rechthoek weergeven

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

## <a name="next-steps"></a>Volgende stappen

Zie de [Cognitive Services Face WPF](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) -app op GitHub voor een werkvoorbeeld van gezichtsrechthoeken gedraaid. Of Raadpleeg de [Face-API-HeadPose voorbeeld](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples) app, die het kenmerk HeadPose in realtime voor het detecteren van verschillende head verplaatsingen van het type (nodding, heen en weer schudden) worden bijgehouden.